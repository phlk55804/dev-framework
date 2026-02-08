# Code Standards

**Project:** [Your Framework]  
**Last Updated:** [Date]  
**Based On:** Clean Code principles, industry best practices, Matt's guidance

---

## Purpose

These standards ensure:
- Code is readable by you in 6 months
- AI collaborators produce consistent code
- New features integrate smoothly
- Technical debt stays manageable

**Philosophy:** Write code for humans first, computers second.

---

## General Principles

### 1. Readability Over Cleverness

**DO:**
```typescript
// Clear and obvious
function calculateEligibilityStatus(student: Student): EligibilityStatus {
  const hasRequiredGPA = student.gpa >= 2.0;
  const hasRequiredAttendance = student.attendanceRate >= 0.90;
  
  if (hasRequiredGPA && hasRequiredAttendance) {
    return EligibilityStatus.ELIGIBLE;
  }
  
  return EligibilityStatus.INELIGIBLE;
}
```

**DON'T:**
```typescript
// Too clever
function calcElig(s: any) {
  return s.gpa >= 2.0 && s.att >= .9 ? 'E' : 'I';
}
```

---

### 2. Meaningful Names

**Variables & Functions:**
- Use descriptive names that reveal intent
- Avoid abbreviations unless universally understood
- Boolean variables should sound like yes/no questions

**DO:**
```typescript
const isEligible = checkStudentEligibility(student);
const totalStudentCount = students.length;
const hasRequiredDocuments = documents.length >= 3;
```

**DON'T:**
```typescript
const x = check(s);
const cnt = arr.length;
const flag = docs.length >= 3;
```

**Functions:**
- Verb + noun (e.g., `getStudent`, `updateEligibility`)
- One action per function
- Name should describe what it does, not how

---

### 3. Small Functions

**Rule:** Functions should do ONE thing and do it well.

**Target:** 10-20 lines max (guidelines, not hard rules)

**DO:**
```typescript
async function getEligibleStudents(orgId: string) {
  const students = await fetchStudents(orgId);
  return students.filter(isEligible);
}

function isEligible(student: Student): boolean {
  return student.gpa >= 2.0 && student.attendanceRate >= 0.90;
}
```

**DON'T:**
```typescript
async function getEligibleStudents(orgId: string) {
  const { data, error } = await supabase
    .from('students')
    .select('*')
    .eq('org_id', orgId);
    
  if (error) throw error;
  
  const eligible = [];
  for (const student of data) {
    if (student.gpa >= 2.0) {
      if (student.attendance_rate >= 0.90) {
        if (student.active) {
          eligible.push(student);
        }
      }
    }
  }
  
  return eligible;
}
```

---

### 4. Comments Explain WHY, Not WHAT

**DO:**
```typescript
// FERPA requires we retain records for 5 years after graduation
const RETENTION_PERIOD_YEARS = 5;

// Using Set for O(1) lookup instead of array.includes() O(n)
const eligibleStudentIds = new Set(eligibleStudents.map(s => s.id));
```

**DON'T:**
```typescript
// Set the retention period to 5
const RETENTION_PERIOD_YEARS = 5;

// Loop through students
for (const student of students) {
  // Check if eligible
  if (student.gpa >= 2.0) {
    // Add to array
    eligible.push(student);
  }
}
```

**When to comment:**
- WHY a decision was made (business rule, performance optimization)
- Non-obvious edge cases
- Workarounds for bugs in dependencies
- References to external docs (FERPA regs, API docs)

**When NOT to comment:**
- What the code obviously does
- To explain bad variable names (rename the variable instead)
- Commented-out code (delete it, Git remembers)

---

## TypeScript Specific

### Type Safety

**Always use explicit types for:**
- Function parameters
- Function return values
- Complex objects

**DO:**
```typescript
interface Student {
  id: string;
  org_id: string;
  name: string;
  gpa: number;
  attendanceRate: number;
}

async function getStudent(studentId: string): Promise<Student> {
  const { data, error } = await supabase
    .from('students')
    .select('*')
    .eq('id', studentId)
    .single();
    
  if (error) throw new Error(`Failed to fetch student: ${error.message}`);
  return data;
}
```

**DON'T:**
```typescript
async function getStudent(id) {
  const { data, error } = await supabase
    .from('students')
    .select('*')
    .eq('id', id)
    .single();
    
  if (error) throw error;
  return data;
}
```

---

### Avoid `any`

**DO:**
```typescript
interface ApiResponse<T> {
  data: T;
  error?: string;
}

async function fetchData<T>(url: string): Promise<ApiResponse<T>> {
  // implementation
}
```

**DON'T:**
```typescript
async function fetchData(url: string): Promise<any> {
  // implementation
}
```

---

### Use Enums for Fixed Sets

**DO:**
```typescript
enum EligibilityStatus {
  ELIGIBLE = 'eligible',
  INELIGIBLE = 'ineligible',
  PENDING = 'pending',
  UNKNOWN = 'unknown'
}

function checkEligibility(student: Student): EligibilityStatus {
  if (!student.gpa) return EligibilityStatus.UNKNOWN;
  return student.gpa >= 2.0 
    ? EligibilityStatus.ELIGIBLE 
    : EligibilityStatus.INELIGIBLE;
}
```

**DON'T:**
```typescript
function checkEligibility(student: any): string {
  if (!student.gpa) return 'unknown';
  return student.gpa >= 2.0 ? 'eligible' : 'ineligible';
}
```

---

## React / Next.js Specific

### Component Structure

**File organization:**
```
/components
  /ui           # shadcn components (Button, Input, etc.)
  /shared       # Custom reusable components
  /features     # Feature-specific components
    /students
      StudentList.tsx
      StudentCard.tsx
      StudentForm.tsx
```

---

### Component Naming

**Files:** PascalCase (`StudentList.tsx`)  
**Components:** PascalCase (`function StudentList()`)  
**Props interfaces:** ComponentNameProps

**DO:**
```typescript
// StudentList.tsx
interface StudentListProps {
  students: Student[];
  onStudentClick: (student: Student) => void;
  orgId: string;
}

export function StudentList({ students, onStudentClick, orgId }: StudentListProps) {
  // implementation
}
```

---

### Hooks

**Custom hooks start with `use`:**

**DO:**
```typescript
// hooks/useStudents.ts
export function useStudents(orgId: string) {
  const [students, setStudents] = useState<Student[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);
  
  useEffect(() => {
    fetchStudents(orgId)
      .then(setStudents)
      .catch(err => setError(err.message))
      .finally(() => setLoading(false));
  }, [orgId]);
  
  return { students, loading, error };
}
```

**Usage:**
```typescript
function StudentDashboard({ orgId }: { orgId: string }) {
  const { students, loading, error } = useStudents(orgId);
  
  if (loading) return <Spinner />;
  if (error) return <ErrorMessage error={error} />;
  
  return <StudentList students={students} />;
}
```

---

### State Management

**Keep state as local as possible:**

**DO:**
```typescript
// State only in component that needs it
function StudentForm() {
  const [formData, setFormData] = useState<StudentFormData>(initialData);
  // Form-specific state stays here
}
```

**DON'T:**
```typescript
// Don't lift state up prematurely
function App() {
  const [studentFormData, setStudentFormData] = useState(...);
  // This doesn't need to be at app level
}
```

---

### Server Components vs Client Components (Next.js App Router)

**Default to Server Components:**
```typescript
// app/students/page.tsx (Server Component by default)
async function StudentsPage() {
  const students = await getStudents(); // Direct DB query, no API call
  return <StudentList students={students} />;
}
```

**Use Client Components when you need:**
- useState, useEffect, or other hooks
- Event handlers (onClick, onChange)
- Browser-only APIs
```typescript
// components/StudentList.tsx
'use client'; // Mark as client component

import { useState } from 'react';

export function StudentList({ students }: { students: Student[] }) {
  const [filter, setFilter] = useState('');
  // Client-side filtering
}
```

---

## Database & Supabase

### Always Include org_id

**DO:**
```typescript
const { data, error } = await supabase
  .from('students')
  .select('*')
  .eq('org_id', orgId)  // ALWAYS filter by org
  .eq('id', studentId);
```

**DON'T:**
```typescript
const { data, error } = await supabase
  .from('students')
  .select('*')
  .eq('id', studentId);  // Missing org_id = security risk
```

---

### Error Handling

**DO:**
```typescript
async function getStudent(studentId: string, orgId: string): Promise<Student> {
  const { data, error } = await supabase
    .from('students')
    .select('*')
    .eq('id', studentId)
    .eq('org_id', orgId)
    .single();
  
  if (error) {
    console.error('Database error fetching student:', error);
    throw new Error(`Failed to fetch student: ${error.message}`);
  }
  
  if (!data) {
    throw new Error(`Student ${studentId} not found`);
  }
  
  return data;
}
```

**DON'T:**
```typescript
async function getStudent(studentId: string) {
  const { data } = await supabase
    .from('students')
    .select('*')
    .eq('id', studentId)
    .single();
  
  return data; // What if error? What if not found?
}
```

---

### Use Transactions for Multi-Step Operations

**DO:**
```typescript
// If creating student and eligibility record together, use transaction
const { data, error } = await supabase.rpc('create_student_with_eligibility', {
  student_data: studentData,
  eligibility_data: eligibilityData
});
```

---

## File Naming

### Consistency Matters

**TypeScript/React files:** PascalCase  
- `StudentList.tsx`
- `EligibilityCheck.tsx`

**Utility files:** camelCase  
- `dateHelpers.ts`
- `formatters.ts`

**API routes:** kebab-case  
- `app/api/students/route.ts`
- `app/api/eligibility-check/route.ts`

**Database tables:** snake_case  
- `students`
- `eligibility_records`
- `academic_years`

---

## Error Handling Patterns

### API Routes

**DO:**
```typescript
export async function GET(req: Request) {
  try {
    const orgId = extractOrgId(req);
    const students = await getStudents(orgId);
    
    return Response.json({ 
      success: true, 
      data: students 
    });
  } catch (error) {
    console.error('API error:', error);
    return Response.json(
      { 
        success: false, 
        error: error instanceof Error ? error.message : 'Unknown error' 
      },
      { status: 500 }
    );
  }
}
```

---

### Frontend Components

**DO:**
```typescript
function StudentList() {
  const [students, setStudents] = useState<Student[]>([]);
  const [error, setError] = useState<string | null>(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    fetchStudents()
      .then(setStudents)
      .catch(err => {
        console.error('Failed to load students:', err);
        setError('Unable to load students. Please try again.');
      })
      .finally(() => setLoading(false));
  }, []);
  
  if (loading) return <LoadingSpinner />;
  if (error) return <ErrorAlert message={error} />;
  if (students.length === 0) return <EmptyState />;
  
  return <div>{/* render students */}</div>;
}
```

---

## Performance Considerations

### Pagination for Large Lists

**DO:**
```typescript
const PAGE_SIZE = 50;

async function getStudents(orgId: string, page: number = 0) {
  const { data, error, count } = await supabase
    .from('students')
    .select('*', { count: 'exact' })
    .eq('org_id', orgId)
    .range(page * PAGE_SIZE, (page + 1) * PAGE_SIZE - 1);
    
  return { students: data, totalCount: count };
}
```

---

### Avoid N+1 Queries

**DO:**
```typescript
// Single query with join
const { data } = await supabase
  .from('students')
  .select(`
    *,
    eligibility_records(*)
  `)
  .eq('org_id', orgId);
```

**DON'T:**
```typescript
// N+1 query pattern
const students = await getStudents(orgId);
for (const student of students) {
  const records = await getEligibilityRecords(student.id); // N queries
}
```

---

### Memoization for Expensive Computations

**DO:**
```typescript
import { useMemo } from 'react';

function StudentDashboard({ students }: { students: Student[] }) {
  const eligibilityStats = useMemo(() => {
    return calculateEligibilityStatistics(students);
  }, [students]); // Only recalculate when students change
  
  return <StatsDisplay stats={eligibilityStats} />;
}
```

---

## Testing Approach

### What to Test

**Priority 1 (Must Test):**
- Business logic (eligibility calculations)
- Data transformations
- Multi-tenant data isolation

**Priority 2 (Should Test):**
- API endpoints
- Database queries
- Authentication flows

**Priority 3 (Nice to Have):**
- UI components (if time allows)
- Edge cases

---

### Test Structure

**DO:**
```typescript
describe('calculateEligibility', () => {
  it('should return ELIGIBLE when GPA >= 2.0 and attendance >= 90%', () => {
    const student = createTestStudent({ gpa: 3.0, attendanceRate: 0.95 });
    expect(calculateEligibility(student)).toBe(EligibilityStatus.ELIGIBLE);
  });
  
  it('should return INELIGIBLE when GPA < 2.0', () => {
    const student = createTestStudent({ gpa: 1.8, attendanceRate: 0.95 });
    expect(calculateEligibility(student)).toBe(EligibilityStatus.INELIGIBLE);
  });
});
```

---

## Git Commit Messages

### Format
```
<type>: <subject>

<body (optional)>
```

**Types:**
- `feat:` New feature
- `fix:` Bug fix
- `refactor:` Code restructuring
- `docs:` Documentation
- `style:` Formatting
- `test:` Adding tests
- `chore:` Maintenance

**Examples:**
```
feat: Add bulk student import functionality

fix: Correct eligibility calculation for edge case where GPA is null

refactor: Extract eligibility logic into separate service

docs: Update PRD with new feature requirements
```

---

## Code Review Checklist

Before considering code "done," verify:

- [ ] **Functionality:** Does it work as specified in PRD?
- [ ] **Type Safety:** No `any` types, proper interfaces
- [ ] **Multi-Tenant:** org_id included in all queries
- [ ] **Error Handling:** Try/catch blocks, user-friendly errors
- [ ] **Performance:** No N+1 queries, pagination for lists
- [ ] **Readability:** Clear variable/function names, appropriate comments
- [ ] **Security:** Input validation, no SQL injection risk
- [ ] **Testing:** Key business logic has tests
- [ ] **Documentation:** Complex logic explained in comments

---

## When to Break These Rules

**These are guidelines, not laws.**

**Break them when:**
- Performance truly requires it (measure first!)
- Third-party library has different conventions (be consistent with the library)
- The rule makes code less readable in a specific case

**But:**
- Document WHY you're breaking the rule
- Keep it localized (don't let it spread)
- Consider refactoring later if technical debt accumulates

---

## Resources

**Recommended Reading:**
- Clean Code by Robert Martin
- The Pragmatic Programmer
- Next.js documentation
- TypeScript Handbook

**Tools:**
- ESLint (linting)
- Prettier (formatting)
- TypeScript strict mode
- VS Code extensions

---

**Last Updated:** [Date]  
**Review Schedule:** After every code review with Matt, update based on feedback
