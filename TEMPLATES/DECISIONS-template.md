# Architectural Decisions Log

**Project:** [Project Name]  
**Last Updated:** [Date]

---

## Purpose of This Document

This log captures:
- **Why** we made key technical and product decisions
- **What alternatives** we considered
- **What tradeoffs** we accepted
- **What we explicitly rejected** and why

**This prevents:**
- Re-debating settled decisions
- Forgetting context for past choices
- AI suggesting already-rejected approaches
- Future you wondering "why did I do it this way?"

---

## Decision Template

Use this structure for each decision:
```markdown
## Decision [NUMBER]: [Short Title]

**Date:** [YYYY-MM-DD]  
**Status:** [Proposed / Accepted / Deprecated / Superseded]  
**Context:** [Strategy / Architecture / Quality]

### Problem
[What decision needed to be made and why]

### Considered Options
1. **Option A:** [Description]
   - Pros: [Benefits]
   - Cons: [Drawbacks]
   
2. **Option B:** [Description]
   - Pros: [Benefits]
   - Cons: [Drawbacks]

3. **Option C:** [Description]
   - Pros: [Benefits]
   - Cons: [Drawbacks]

### Decision
**Chosen:** [Which option]

**Rationale:**
[Why this option over the others. Be specific about what factors tipped the decision.]

### Consequences
**Positive:**
- [What this enables]
- [What this makes easier]

**Negative:**
- [What this constrains]
- [What this makes harder]

**Enterprise Impact:**
- [How this affects scale]
- [What we'll need to change later if we grow]

### Implementation Notes
- [Specific technical details]
- [Patterns to follow]
- [Things to watch out for]

### Related Decisions
- [Link to Decision #X that depends on this]
- [Link to Decision #Y that this supersedes]

### Review Date
[When to reconsider this decision, if applicable]
```

---

## Active Decisions

### Core Architecture

---

## Decision 001: [Tech Stack Selection]

**Date:** [YYYY-MM-DD]  
**Status:** Accepted  
**Context:** Architecture

### Problem
Need to choose foundational tech stack for entire application. Must support rapid MVP development while enabling future enterprise scale.

### Considered Options

1. **Next.js + Supabase**
   - Pros: Fast development, built-in auth/storage, PostgreSQL, can self-host later
   - Cons: Vendor dependency, pricing at scale unknown

2. **Create React App + Custom Node.js Backend**
   - Pros: Full control, no vendor lock-in
   - Cons: Slower to ship, more infrastructure to manage solo

3. **No-code Platform (e.g., Bubble)**
   - Pros: Fastest to MVP
   - Cons: Limited customization, hard to escape later, not real coding experience

### Decision
**Chosen:** Next.js + Supabase

**Rationale:**
- Speed to MVP critical for pilot validation
- Supabase gives enterprise-grade Postgres underneath
- Can self-host if pricing becomes issue (open source)
- Real code = portfolio for CODA application
- TypeScript ecosystem = transferable skills

### Consequences

**Positive:**
- Ship features in days, not weeks
- Built-in auth/storage reduces boilerplate
- Row-level security in database supports multi-tenancy
- Vercel deployment is seamless

**Negative:**
- Monthly costs increase with usage
- Some vendor dependency on Supabase
- Need to learn Supabase-specific patterns

**Enterprise Impact:**
- PostgreSQL = proven at scale
- RLS policies = secure multi-tenancy from day one
- Can migrate to self-hosted Supabase if costs become issue

### Implementation Notes
- Use TypeScript everywhere (type safety)
- Implement RLS policies on every table
- Keep business logic in app, not database triggers (easier to test)
- Use Supabase's realtime features sparingly (can be expensive)

### Related Decisions
- Decision 002: Database schema design
- Decision 003: Authentication approach

### Review Date
After second pilot customer (re-evaluate pricing/scale)

---

## Decision 002: [Database Schema - Multi-Tenant From Day One]

**Date:** [YYYY-MM-DD]  
**Status:** Accepted  
**Context:** Architecture

### Problem
Need to design database schema that works for single pilot school today but supports multiple schools tomorrow without major refactor.

### Considered Options

1. **Single-Tenant (Refactor Later)**
   - Pros: Simpler to start, faster initial development
   - Cons: Painful migration when adding customer #2, high rewrite risk

2. **Multi-Tenant with org_id on Every Table**
   - Pros: Supports scale from day one, add customers easily
   - Cons: Slightly more complex queries, need RLS policies

3. **Separate Database Per Customer**
   - Pros: Maximum isolation, easy to shard
   - Cons: Complex to manage solo, expensive at scale

### Decision
**Chosen:** Multi-Tenant with org_id on Every Table

**Rationale:**
- Adding customer #2 should take hours, not months
- RLS policies provide security and isolation
- Queries slightly more complex but manageable
- Industry standard approach (proven pattern)

### Consequences

**Positive:**
- Can onboard new schools without code changes
- Data properly isolated by tenant
- Shared infrastructure = lower costs
- Cross-tenant analytics possible later

**Negative:**
- Must remember org_id filter on every query
- RLS policies add complexity
- Can't accidentally query across tenants (good, but needs care)

**Enterprise Impact:**
- Ready for district-level deals (multiple schools)
- Can support 100+ tenants on single database
- Clear path to enterprise features (district dashboards)

### Implementation Notes
```sql
-- Every table gets org_id
CREATE TABLE students (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  org_id UUID NOT NULL REFERENCES organizations(id),
  name TEXT NOT NULL,
  -- other fields
  CONSTRAINT fk_org FOREIGN KEY (org_id) REFERENCES organizations(id)
);

-- Enable RLS on every table
ALTER TABLE students ENABLE ROW LEVEL SECURITY;

-- Policy: users only see their org's data
CREATE POLICY "Users can only access their org's students"
  ON students
  FOR ALL
  USING (org_id = auth.jwt() ->> 'org_id');
```

### Related Decisions
- Decision 001: Tech stack (Supabase supports RLS natively)
- Decision 003: Authentication (need org_id in JWT)

### Review Date
After 10+ customers (evaluate if single DB still sufficient)

---

## Decision 003: [Authentication - Supabase Auth with Org Context]

**Date:** [YYYY-MM-DD]  
**Status:** Accepted  
**Context:** Architecture

### Problem
Need authentication system that works for pilot school today but can support SSO for enterprise later.

### Considered Options

1. **Custom Auth (JWT + bcrypt)**
   - Pros: Full control, no vendor dependency
   - Cons: Security risk building from scratch, no SSO path

2. **Auth0**
   - Pros: Enterprise-grade, SSO ready, best-in-class
   - Cons: Expensive, overkill for MVP, another vendor

3. **Supabase Auth**
   - Pros: Integrated with database, can add SSO later, free tier
   - Cons: Less mature than Auth0, tied to Supabase

### Decision
**Chosen:** Supabase Auth

**Rationale:**
- Integrated with database (org_id in JWT automatically)
- Free tier supports pilot phase
- Can add Google/Microsoft SSO via configuration later
- Magic links = easy pilot user onboarding
- Good enough for enterprise (used by companies at scale)

### Consequences

**Positive:**
- Don't build auth from scratch (security win)
- org_id propagates to RLS policies automatically
- Magic links = no password management for pilot
- SSO is configuration, not code change

**Negative:**
- Tied to Supabase ecosystem
- Can't customize auth flow extensively
- Need to understand JWT claims for RLS

**Enterprise Impact:**
- SSO support ready when needed
- Role-based access control (RBAC) supported
- Audit logs available
- Compliant with enterprise security requirements

### Implementation Notes
```typescript
// Store org_id in user metadata on signup
const { data, error } = await supabase.auth.signUp({
  email: email,
  password: password,
  options: {
    data: {
      org_id: organizationId, // This goes in JWT
      role: 'coach', // Also in JWT for RBAC
    }
  }
});

// org_id is now available in RLS policies
// auth.jwt() ->> 'org_id'
```

### Related Decisions
- Decision 001: Tech stack (Supabase)
- Decision 002: Database schema (org_id required)

### Review Date
Before first enterprise customer (validate SSO requirements)

---

### [Add More Decisions Here Following Template Above]

---

## Rejected Approaches (Don't Suggest These Again)

### Rejected Option: [Approach Name]

**Date Rejected:** [YYYY-MM-DD]  
**Why We Considered It:** [Context]  
**Why We Rejected It:** [Specific reasons]  
**Would Only Reconsider If:** [Conditions that would change decision]

---

### Rejected Option: Firebase

**Date Rejected:** [YYYY-MM-DD]  
**Why We Considered It:** Popular, fast to start, lots of tutorials  
**Why We Rejected It:** 
- NoSQL doesn't fit relational data model well
- Vendor lock-in harder to escape than Supabase
- Pricing at scale unclear
- Want SQL for complex queries

**Would Only Reconsider If:** Building a completely different product with document-based data

---

### Rejected Option: [Another Approach]

**Date Rejected:** [YYYY-MM-DD]  
**Why We Considered It:** [Context]  
**Why We Rejected It:** [Reasons]  
**Would Only Reconsider If:** [Conditions]

---

## Deprecated Decisions (Used to Do This, Now Don't)

### Deprecated: [Old Approach]

**Original Decision Date:** [YYYY-MM-DD]  
**Deprecation Date:** [YYYY-MM-DD]  
**What We Used to Do:** [Description]  
**Why We Changed:** [Reason for deprecation]  
**What We Do Now:** [Link to new decision that supersedes this]  
**Migration Path:** [How to update existing code]

---

## Patterns & Conventions

### Code Organization

**Pattern:** [Description of how we organize files/folders]  
**Rationale:** [Why this pattern]  
**Example:**
```
/app
  /[org_id]
    /dashboard
    /students
    /reports
/components
  /ui (shadcn components)
  /shared (custom reusable)
/lib
  /db (database utilities)
  /auth (auth helpers)
```

---

### Naming Conventions

**Database Tables:** snake_case (students, eligibility_records)  
**TypeScript:** PascalCase for types/interfaces, camelCase for variables  
**React Components:** PascalCase for components, kebab-case for files  
**API Routes:** kebab-case (/api/students, /api/eligibility-check)

**Rationale:** Industry standards, consistent with frameworks we use

---

### Error Handling

**Pattern:** [How we handle errors consistently]  
**Rationale:** [Why this approach]  
**Example:**
```typescript
try {
  const { data, error } = await supabase
    .from('students')
    .select('*')
    .eq('org_id', orgId);
    
  if (error) throw error;
  return { success: true, data };
} catch (error) {
  console.error('Error fetching students:', error);
  return { success: false, error: error.message };
}
```

---

## Learning from Mistakes

### Mistake: [What Went Wrong]

**Date:** [YYYY-MM-DD]  
**What Happened:** [Description of issue]  
**Root Cause:** [Why it happened]  
**How We Fixed It:** [Solution]  
**Prevention:** [How we prevent this in future]  
**Related Decision:** [Link to decision that addresses this]

---

## Open Questions / Future Decisions

### Question: [Topic Needing Decision]

**Context:** [Why this matters]  
**Options Being Considered:** [Preliminary thoughts]  
**Information Needed:** [What we need to decide]  
**Decision Deadline:** [When this needs resolution]  
**Owner:** [Who's responsible for deciding]

---

## Decision Review Schedule

**Monthly Review:**
- Review "Proposed" decisions → move to "Accepted" or table
- Check "Review Date" on accepted decisions
- Update any deprecated approaches

**Quarterly Review:**
- Evaluate if any decisions need deprecation
- Check if rejected approaches should be reconsidered
- Assess if patterns are working

---

## How to Use This Document

### When Making a New Decision
1. Copy decision template
2. Fill out all sections honestly
3. Get AI to help evaluate options
4. Document the choice and rationale
5. Date stamp and commit

### When AI Suggests Something
1. Search this document first
2. Check if we already decided against it
3. Reference the decision in your response
4. If not covered, maybe it's worth considering

### When Reviewing Code
1. Check if patterns match documented conventions
2. Verify decisions are being followed
3. Update this doc if you find inconsistencies

---

## Change Log

| Date | Decision # | Change | Reason |
|------|-----------|--------|--------|
| [Date] | [#] | [What changed] | [Why] |
| [Date] | [#] | [What changed] | [Why] |

---

**Last Updated:** [Date]  
**Next Review:** [Date]
