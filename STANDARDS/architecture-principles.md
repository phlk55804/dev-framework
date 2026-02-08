# Architecture Principles

**Project:** [Your Framework]  
**Last Updated:** [Date]  
**Philosophy:** Build for scale from day one, implement for today's needs.

---

## Core Principles

### 1. Multi-Tenant by Default

**Every architectural decision must support multiple customers.**

**Rule:** If you can't explain how it works with 100 customers, redesign it.

**Example:**
```typescript
// ALWAYS include tenant context
interface BaseEntity {
  id: string;
  org_id: string;  // Every table, every query
  created_at: Date;
  updated_at: Date;
}

// NEVER hardcode tenant references
// ❌ if (org_id === 'steward-school-123')
// ✅ const settings = await getOrgSettings(org_id);
```

**Tenant Isolation:**
- Database: Row-level security on every table
- Storage: Files organized by org_id
- Cache: Keys include org_id prefix
- Sessions: org_id in JWT claims

---

### 2. Separation of Concerns

**Keep business logic, data access, and presentation separate.**

**Layers:**
```
Presentation Layer (UI)
    ↓
Business Logic Layer (Services)
    ↓
Data Access Layer (Repository/ORM)
    ↓
Database
```

**Example:**
```typescript
// ❌ BAD: Business logic in UI component
function StudentList() {
  const [students, setStudents] = useState([]);
  
  useEffect(() => {
    supabase.from('students')
      .select('*')
      .eq('org_id', orgId)
      .then(({ data }) => {
        // Eligibility calculation in UI??
        const eligible = data.filter(s => s.gpa >= 2.0 && s.attendance >= 0.9);
        setStudents(eligible);
      });
  }, []);
}

// ✅ GOOD: Separation of concerns
// Data layer
async function getStudents(orgId: string): Promise<Student[]> {
  const { data, error } = await supabase
    .from('students')
    .select('*')
    .eq('org_id', orgId);
  if (error) throw error;
  return data;
}

// Business logic layer
function filterEligibleStudents(students: Student[]): Student[] {
  return students.filter(isEligible);
}

function isEligible(student: Student): boolean {
  return student.gpa >= 2.0 && student.attendanceRate >= 0.9;
}

// Presentation layer
function StudentList() {
  const { students } = useStudents(orgId);
  const eligibleStudents = filterEligibleStudents(students);
  return <List items={eligibleStudents} />;
}
```

**Benefits:**
- Business logic is testable without UI
- Can change database without touching UI
- Can reuse logic across different interfaces

---

### 3. Database Schema Design

**Principles:**

**Normalization (to a point):**
- Avoid data duplication
- Use foreign keys for relationships
- But denormalize for performance when justified

**Multi-Tenant Schema:**
```sql
-- Organizations (Tenants)
CREATE TABLE organizations (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name TEXT NOT NULL,
  slug TEXT UNIQUE NOT NULL,
  settings JSONB DEFAULT '{}',
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Every table references organization
CREATE TABLE students (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  org_id UUID NOT NULL REFERENCES organizations(id) ON DELETE CASCADE,
  name TEXT NOT NULL,
  email TEXT,
  gpa DECIMAL(3,2),
  attendance_rate DECIMAL(3,2),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Indexes for performance
CREATE INDEX idx_students_org_id ON students(org_id);
CREATE INDEX idx_students_org_gpa ON students(org_id, gpa);

-- Row-level security
ALTER TABLE students ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users access own org's students"
  ON students
  FOR ALL
  USING (org_id = (auth.jwt() ->> 'org_id')::uuid);
```

**Relationships:**
- One-to-many: Use foreign keys
- Many-to-many: Use join tables
- Always cascade deletes appropriately

---

### 4. API Design

**RESTful Conventions:**
```
GET    /api/students           # List students
GET    /api/students/:id       # Get single student
POST   /api/students           # Create student
PUT    /api/students/:id       # Update student (full)
PATCH  /api/students/:id       # Update student (partial)
DELETE /api/students/:id       # Delete student
```

**Response Format (Consistent):**
```typescript
// Success
{
  "success": true,
  "data": { /* resource */ }
}

// Error
{
  "success": false,
  "error": "Error message for user",
  "code": "ELIGIBILITY_CHECK_FAILED"
}

// List with pagination
{
  "success": true,
  "data": [ /* resources */ ],
  "pagination": {
    "page": 1,
    "pageSize": 50,
    "total": 243,
    "hasMore": true
  }
}
```

**Status Codes:**
- 200: Success
- 201: Created
- 400: Bad request (validation error)
- 401: Unauthorized (not logged in)
- 403: Forbidden (logged in but no access)
- 404: Not found
- 500: Server error

---

### 5. Error Handling Strategy

**Fail Fast, Fail Loud, Fail Gracefully:**
```typescript
// Fail fast (input validation)
function createStudent(data: CreateStudentInput) {
  if (!data.name) throw new ValidationError('Name required');
  if (!data.org_id) throw new ValidationError('Organization required');
  // Continue only if valid
}

// Fail loud (unexpected errors)
async function getStudent(id: string) {
  try {
    const { data, error } = await supabase
      .from('students')
      .select('*')
      .eq('id', id)
      .single();
    
    if (error) {
      console.error('Database error:', error);
      throw new DatabaseError(`Failed to fetch student: ${error.message}`);
    }
    
    return data;
  } catch (error) {
    // Log for debugging
    console.error('Unexpected error in getStudent:', error);
    throw error; // Re-throw to caller
  }
}

// Fail gracefully (UI layer)
function StudentDetail({ studentId }: { studentId: string }) {
  const [student, setStudent] = useState<Student | null>(null);
  const [error, setError] = useState<string | null>(null);
  
  useEffect(() => {
    getStudent(studentId)
      .then(setStudent)
      .catch(err => {
        setError('Unable to load student details. Please try again.');
        // User sees friendly message, console has details
      });
  }, [studentId]);
  
  if (error) return <ErrorAlert message={error} />;
  if (!student) return <LoadingSkeleton />;
  return <StudentCard student={student} />;
}
```

**Error Types:**
- **ValidationError:** User input problems (400)
- **AuthenticationError:** Not logged in (401)
- **AuthorizationError:** No permission (403)
- **NotFoundError:** Resource doesn't exist (404)
- **DatabaseError:** DB operation failed (500)

---

### 6. Authentication & Authorization

**Authentication (Who are you?):**
```typescript
// JWT contains user identity
interface JWTPayload {
  sub: string;        // user_id
  email: string;
  org_id: string;     // CRITICAL for multi-tenancy
  role: string;       // For RBAC
  iat: number;
  exp: number;
}

// Middleware extracts and validates
async function requireAuth(req: Request) {
  const token = extractToken(req);
  const payload = await verifyToken(token);
  return payload;
}
```

**Authorization (What can you do?):**
```typescript
enum Role {
  ORG_ADMIN = 'org_admin',
  COACH = 'coach',
  VIEWER = 'viewer'
}

const permissions = {
  [Role.ORG_ADMIN]: ['students:*', 'settings:*', 'users:*'],
  [Role.COACH]: ['students:read', 'students:write', 'eligibility:*'],
  [Role.VIEWER]: ['students:read', 'reports:read']
};

function hasPermission(role: Role, permission: string): boolean {
  const userPermissions = permissions[role];
  return userPermissions.some(p => 
    p === permission || p.endsWith(':*') && permission.startsWith(p.split(':')[0])
  );
}
```

**Data Access Control:**
```typescript
// ALWAYS validate org_id matches user's org_id
async function getStudent(userId: string, studentId: string) {
  const user = await getUser(userId);
  const student = await db.students.findUnique({
    where: { 
      id: studentId,
      org_id: user.org_id  // Ensures user can only access their org's data
    }
  });
  
  if (!student) throw new NotFoundError('Student not found');
  return student;
}
```

---

### 7. State Management

**Keep State Local When Possible:**
```typescript
// ✅ GOOD: Component-local state
function StudentForm() {
  const [formData, setFormData] = useState(initialData);
  // Form state doesn't need to be global
}

// ❌ BAD: Premature global state
function App() {
  const [studentFormData, setStudentFormData] = useState(...);
  // Why is this at app level?
}
```

**When to Use Global State:**
- User session (authenticated user info)
- Organization context (current org)
- Theme preferences
- Data shared across many components

**Recommended Approach:**
- **React Context** for org/user context
- **URL state** for filters, pagination
- **Local state** for everything else
- **Server state** (React Query/SWR) for API data
```typescript
// Context for org/user
export function OrgProvider({ children }: { children: React.ReactNode }) {
  const [org, setOrg] = useState<Organization | null>(null);
  return (
    <OrgContext.Provider value={{ org, setOrg }}>
      {children}
    </OrgContext.Provider>
  );
}

// URL state for filters
function StudentList() {
  const searchParams = useSearchParams();
  const filter = searchParams.get('filter') || 'all';
  // Filter persists in URL, shareable, works with back button
}
```

---

### 8. Performance Architecture

**Caching Strategy:**

**Levels:**
1. **Browser cache:** Static assets (images, CSS, JS)
2. **CDN cache:** Next.js automatic edge caching
3. **Application cache:** Redis for hot data
4. **Database cache:** Query result caching

**When to Cache:**
- ✅ Organization settings (rarely change)
- ✅ User permissions (change infrequently)
- ✅ Lookup tables (states, countries, etc.)
- ❌ Student eligibility (changes frequently)
- ❌ Real-time data

**Cache Invalidation:**
```typescript
// When student is updated, invalidate relevant caches
async function updateStudent(studentId: string, updates: Partial<Student>) {
  const student = await db.students.update({
    where: { id: studentId },
    data: updates
  });
  
  // Invalidate caches
  await cache.delete(`student:${studentId}`);
  await cache.delete(`students:org:${student.org_id}`);
  
  return student;
}
```

**Database Indexing:**
```sql
-- Every foreign key should be indexed
CREATE INDEX idx_students_org_id ON students(org_id);

-- Composite indexes for common queries
CREATE INDEX idx_students_org_gpa 
  ON students(org_id, gpa DESC);

-- Partial indexes for frequent filters
CREATE INDEX idx_eligible_students 
  ON students(org_id) 
  WHERE gpa >= 2.0 AND attendance_rate >= 0.9;
```

---

### 9. File Organization

**Project Structure:**
```
/app                    # Next.js App Router
  /api                  # API routes
    /students
      route.ts          # GET, POST /api/students
    /students/[id]
      route.ts          # GET, PUT, DELETE /api/students/:id
  /(dashboard)          # Route group
    /[org_id]           # Dynamic org routing
      /students         # /dashboard/org-123/students
        page.tsx
      /reports
        page.tsx

/components
  /ui                   # shadcn/ui components
  /shared               # Reusable custom components
  /features             # Feature-specific components
    /students
    /eligibility

/lib
  /db                   # Database utilities
    client.ts           # Supabase client
    queries.ts          # Reusable queries
  /auth                 # Auth helpers
  /utils                # General utilities

/types                  # TypeScript type definitions
  student.ts
  eligibility.ts

/hooks                  # Custom React hooks
  useStudents.ts
  useAuth.ts

/services               # Business logic layer
  studentService.ts
  eligibilityService.ts
```

---

### 10. Security Principles

**Defense in Depth:**

**Layer 1: Input Validation**
```typescript
import { z } from 'zod';

const CreateStudentSchema = z.object({
  name: z.string().min(1).max(100),
  email: z.string().email().optional(),
  gpa: z.number().min(0).max(4.0),
  org_id: z.string().uuid()
});

function validateInput(data: unknown) {
  return CreateStudentSchema.parse(data); // Throws if invalid
}
```

**Layer 2: Authentication**
```typescript
// Every API route requires valid token
export async function GET(req: Request) {
  const user = await requireAuth(req); // Throws if not authenticated
  // Continue only if authenticated
}
```

**Layer 3: Authorization**
```typescript
// Check permissions for specific action
export async function DELETE(req: Request, { params }: { params: { id: string } }) {
  const user = await requireAuth(req);
  
  if (!hasPermission(user.role, 'students:delete')) {
    throw new AuthorizationError('Insufficient permissions');
  }
  
  // Continue only if authorized
}
```

**Layer 4: Data Access Control**
```typescript
// Ensure user can only access their org's data
const student = await db.students.findUnique({
  where: { 
    id: studentId,
    org_id: user.org_id  // Critical check
  }
});
```

**Layer 5: Row-Level Security**
```sql
-- Database enforces even if app has bug
CREATE POLICY "Org isolation"
  ON students
  FOR ALL
  USING (org_id = (auth.jwt() ->> 'org_id')::uuid);
```

**Security Checklist:**
- [ ] Input validation on all user data
- [ ] Parameterized queries (no SQL injection)
- [ ] Output encoding (no XSS)
- [ ] HTTPS only
- [ ] Secure headers (CSP, HSTS, etc.)
- [ ] Rate limiting on auth endpoints
- [ ] Password complexity requirements
- [ ] No secrets in code (use env vars)

---

## Decision Framework

**When designing a new feature, ask:**

1. **Multi-Tenancy:** How does this work with 100 customers?
2. **Separation:** Is business logic separate from UI and data access?
3. **Security:** What could go wrong? How do we prevent it?
4. **Performance:** Will this scale? Do we need caching/indexing?
5. **Testability:** Can we test this without the full app?
6. **Maintainability:** Will I understand this in 6 months?

**If you can't answer all six, revisit the design.**

---

## Anti-Patterns to Avoid

### ❌ God Objects
```typescript
// DON'T: One class/function does everything
class StudentManager {
  createStudent() { }
  updateStudent() { }
  deleteStudent() { }
  calculateEligibility() { }
  sendNotification() { }
  generateReport() { }
  exportToCSV() { }
  // ... 50 more methods
}
```

### ❌ Tight Coupling
```typescript
// DON'T: UI directly coupled to database
function StudentList() {
  const { data } = supabase.from('students').select('*');
  // Now you can't change DB without changing UI
}
```

### ❌ Premature Optimization
```typescript
// DON'T: Complex caching before measuring performance
const megaCacheSystem = new DistributedHyperCache({
  // ... 100 lines of config
});
// Did you measure that you need this?
```

### ❌ Magic Numbers
```typescript
// DON'T:
if (student.gpa >= 2.0) { }  // What does 2.0 mean?

// DO:
const MINIMUM_ELIGIBLE_GPA = 2.0;  // FERPA requirement
if (student.gpa >= MINIMUM_ELIGIBLE_GPA) { }
```

---

## When to Refactor

**Refactor when:**
- Adding same feature to third place (DRY violation)
- Function exceeds 50 lines
- File exceeds 300 lines
- Tests become too complex
- You catch yourself saying "I should really clean this up"

**Don't refactor when:**
- Code works and is clear
- Refactor is purely cosmetic
- You're in the middle of feature work (finish feature, then refactor)

---

## Architecture Review Checklist

Before finalizing architecture for a feature:

- [ ] Multi-tenant compatible (works with N customers)
- [ ] Layers properly separated (UI, logic, data)
- [ ] Security controls in place (auth, authz, validation)
- [ ] Performance considered (indexes, caching if needed)
- [ ] Error handling comprehensive
- [ ] Testable without full app
- [ ] Documented (decisions in DECISIONS.md)

---

**Last Updated:** [Date]  
**Next Review:** After every major architectural decision
