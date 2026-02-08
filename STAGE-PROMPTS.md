# Stage Prompts: Copy/Paste Templates for AI Collaboration

> Exact prompts to use at each stage of development. Copy, customize with your project details, and paste to your AI collaborator.

---

## Stage 1: STRATEGY (What & Why)

### Prompt 1A: Starting a New Project PRD
```
I'm starting a new SaaS project and need help creating a comprehensive PRD.

PROJECT CONCEPT:
[Describe the problem you're solving and who experiences it]

MY CONSTRAINTS:
- Time available: [X hours/week]
- Technical skills: [Your current level]
- Timeline: [When you want to launch pilot]
- Budget: [Any financial constraints]

ENTERPRISE VISION:
- Starting with: [Pilot customer type]
- Scaling to: [Enterprise vision - districts, states, national]

Please help me create a PRD using this structure:
1. Problem Statement
2. User Personas (be specific, not generic)
3. Success Metrics (measurable outcomes)
4. Feature Specifications (MVP scope)
5. Out of Scope (what we're NOT building)
6. Enterprise Considerations (multi-tenant from day one)
7. Technical Constraints

Push back if my scope is too broad or if I'm making single-tenant assumptions.
```

### Prompt 1B: Adding a Feature to Existing PRD
```
I need to add a new feature to my existing product PRD.

CURRENT CONTEXT:
[Paste relevant section from CONTEXT.md]

EXISTING PRD:
[Paste current PRD.md or relevant section]

NEW FEATURE REQUEST:
[Describe the feature and why it's needed]

USER FEEDBACK:
[Any pilot customer feedback that drove this request]

Questions:
1. Does this feature align with our core problem statement?
2. Does it conflict with anything already in the PRD?
3. What's the simplest version that validates the value?
4. What enterprise considerations does this introduce?

Help me write the PRD section for this feature.
```

### Prompt 1C: Enterprise Impact Check
```
I've drafted a feature specification and need to validate enterprise readiness.

FEATURE SPEC:
[Paste your feature description]

CURRENT ARCHITECTURE:
[Paste relevant section from DECISIONS.md]

ENTERPRISE VISION:
[Describe your scale goal - e.g., "1000+ schools using this"]

Questions:
1. Does this feature design support multi-tenant architecture?
2. What technical decisions NOW will make or break enterprise scale?
3. Are there compliance considerations (FERPA, GDPR, etc.) I should bake in?
4. What's the simplest version that doesn't paint me into a corner?

Think like: "Build for today, architect for tomorrow."
```

---

## Stage 2: ARCHITECTURE (How)

### Prompt 2A: Initial Technical Specification
```
I'm ready to design the technical architecture for a feature.

PRD SECTION:
[Paste the specific feature from PRD.md]

CURRENT CONTEXT:
[Paste relevant sections from CONTEXT.md]

EXISTING DECISIONS:
[Paste relevant architectural decisions from DECISIONS.md]

TECH STACK:
- Frontend: [e.g., Next.js, TypeScript, Tailwind]
- Backend: [e.g., Supabase, PostgreSQL]
- Auth: [e.g., Supabase Auth]
- Storage: [e.g., Supabase Storage]

ENTERPRISE REQUIREMENTS:
- Must support multi-tenancy (org_id/school_id on all tables)
- Must support SSO integration later
- Must scale to [X] concurrent users

Please design:
1. Database schema (with multi-tenant support)
2. API endpoints (RESTful conventions)
3. Component structure (React/Next.js best practices)
4. File organization
5. Third-party integrations needed

Flag any single-tenant assumptions or scalability concerns.
```

### Prompt 2B: Architecture Review Against Standards
```
I've drafted a technical approach and need it reviewed against our standards.

TECHNICAL SPEC:
[Paste your architecture design]

CODE STANDARDS:
[Paste from STANDARDS/code-standards.md]

ARCHITECTURE PRINCIPLES:
[Paste from STANDARDS/architecture-principles.md]

Review this design for:
1. Compliance with our standards
2. Multi-tenant data isolation
3. Security considerations (auth, data access)
4. Performance at scale
5. Maintainability (will I understand this in 6 months?)

Be critical. Tell me what will break when I add customer #2.
```

### Prompt 2C: Database Schema Design
```
I need help designing a database schema for this feature.

FEATURE DESCRIPTION:
[Describe what data needs to be stored]

RELATIONSHIPS:
[Describe how this data relates to existing tables]

MULTI-TENANT REQUIREMENT:
- Every table must support org_id/school_id for tenant isolation
- Row-level security (RLS) policies needed

SCALABILITY CONSIDERATIONS:
- Expected record volume: [e.g., "1000 schools × 500 students each"]
- Query patterns: [e.g., "Coaches query students by eligibility status"]

Please design:
1. Table schemas with proper types and constraints
2. Indexes for performance
3. Foreign key relationships
4. RLS policies for Supabase
5. Migration strategy (if modifying existing tables)

Show me the SQL DDL statements.
```

---

## Stage 3: QUALITY (Correct & Deploy)

### Prompt 3A: Implementation Kickoff
```
I'm ready to implement this feature. Here's everything you need:

TECHNICAL SPEC:
[Paste architecture design from Stage 2]

CODE STANDARDS:
[Paste from STANDARDS/code-standards.md]

DEFINITION OF DONE:
[Paste from STANDARDS/definition-of-done.md]

CURRENT CODEBASE CONTEXT:
- File structure: [Describe where this code will live]
- Existing patterns: [Any established patterns to follow]
- Dependencies available: [List key libraries]

Please implement this feature following our standards. 

If you see opportunities to refactor existing code for consistency, flag them.
```

### Prompt 3B: Code Review Request
```
I've implemented a feature and need code review before deploying.

FEATURE IMPLEMENTED:
[Describe what you built]

CODE:
[Paste relevant code files or link to GitHub PR]

PRD SUCCESS METRICS:
[Paste success metrics from PRD - does code enable measuring these?]

CONCERNS:
[Any areas where you're uncertain or made tradeoffs]

Please review for:
1. Code quality (readability, maintainability)
2. Security vulnerabilities
3. Performance issues
4. Test coverage gaps
5. Alignment with PRD requirements

Be brutally honest. What will I regret in 6 months?
```

### Prompt 3C: Bug Diagnosis and Fix
```
I'm encountering an issue in production and need help diagnosing.

EXPECTED BEHAVIOR:
[What should happen according to PRD]

ACTUAL BEHAVIOR:
[What's actually happening]

ERROR MESSAGES:
[Paste any console errors, logs, or error messages]

RELEVANT CODE:
[Paste the code you suspect is causing the issue]

CONTEXT:
- User action that triggered this: [Describe steps to reproduce]
- Environment: [Production, staging, local?]
- Recent changes: [What was deployed recently?]

Please help me:
1. Identify root cause
2. Propose a fix
3. Suggest how to prevent similar issues
4. Recommend additional logging/monitoring
```

---

## Cross-Stage Prompts

### Prompt X1: Context Handoff Between Stages
```
I'm moving from [STAGE] to [NEXT STAGE] and need to ensure clean handoff.

COMPLETED ARTIFACTS:
[Paste relevant docs from previous stage]

OPEN QUESTIONS:
[List any unresolved items]

ASSUMPTIONS MADE:
[Document any assumptions that need validation]

NEXT STAGE GOALS:
[What needs to be accomplished]

Please review for:
1. Completeness (do I have everything needed to proceed?)
2. Conflicts (any contradictions in the documents?)
3. Risks (what could go wrong if we proceed as-is?)

Gate check: Should I proceed or return to previous stage?
```

### Prompt X2: Technical Debt Audit
```
I just shipped a feature and need to audit technical debt created.

FEATURE SHIPPED:
[Describe what was deployed]

SHORTCUTS TAKEN:
[Be honest about corners cut for speed]

CODE AREAS OF CONCERN:
[Specific files/functions that feel fragile]

CURRENT PAIN POINTS:
- Testing gaps: [What's not covered?]
- Performance: [Any slow queries/renders?]
- Maintainability: [What's hard to understand?]

Help me:
1. Categorize debt (critical vs. can wait)
2. Estimate refactor effort for each item
3. Recommend which to tackle next sprint
4. Identify what's fine to leave as-is

Be practical. I'm solo, so "perfect" isn't the goal.
```

### Prompt X3: Pivot Decision
```
I'm considering a significant change to the product direction.

CURRENT DIRECTION:
[Paste current PRD problem statement and core features]

PROPOSED PIVOT:
[Describe the new direction and why]

FEEDBACK DRIVING THIS:
[User feedback, market research, or other data]

SUNK COST:
[How much is already built toward current direction?]

Questions:
1. Does this pivot invalidate existing architecture?
2. What can be salvaged vs. rebuilt?
3. Is this a true pivot or scope creep?
4. Should I finish current MVP first?

Help me think through this rationally, not emotionally.
```

---

## Prompt Best Practices

### DO:
✅ Paste full context (PRD, CONTEXT.md, DECISIONS.md excerpts)
✅ Be explicit about constraints (time, skills, budget)
✅ State your enterprise vision upfront
✅ Ask AI to push back and be critical
✅ Reference existing standards and decisions

### DON'T:
❌ Assume AI remembers previous conversations (paste context every time)
❌ Ask open-ended "what should I build?" (have an opinion first)
❌ Hide your constraints (AI needs honest context)
❌ Skip the "why" (always explain business rationale)
❌ Accept first suggestion without questioning fit

---

## Customizing These Prompts

**For each prompt:**
1. Copy the template
2. Fill in [bracketed sections] with your project specifics
3. Paste complete context (don't make AI guess)
4. Paste to your AI collaborator (Claude, Deepagent, Devin, etc.)

**The [bracketed] sections are placeholders.** Replace with:
- Your actual project details
- Relevant document excerpts
- Specific technical constraints
- Current state and goals

---

## Which AI for Which Stage?

| Stage | Recommended AI | Why |
|-------|---------------|-----|
| Strategy | Claude Pro, GPT-4 | Deep reasoning, pushback, strategic thinking |
| Architecture | Claude Code, Deepagent | Code structure, scaffolding, technical design |
| Quality | Devin, Claude Code | Implementation, review, deployment |

**But you can use ANY AI** - these prompts work across models. The framework is tool-agnostic.

---

**Next:** See `GATE-CHECKLISTS.md` for validation criteria between stages.
