# Gate Checklists: Stage Transition Validation

> Before moving between stages, complete the appropriate checklist. If you can't check every box, stay in the current stage.

---

## Gate 1: STRATEGY → ARCHITECTURE

**Purpose:** Ensure the PRD is complete enough to design a technical solution.

### PRD Completeness

- [ ] **Problem Statement** is clear and specific
  - Answers "who has this problem?"
  - Answers "why does it matter?"
  - Quantifies impact if possible (hours saved, money lost, etc.)

- [ ] **User Personas** are specific, not generic
  - Real titles/roles (not "busy professionals")
  - Specific pain points documented
  - Day-in-the-life context provided

- [ ] **Success Metrics** are measurable
  - Not vanity metrics (downloads, signups)
  - Actual value metrics (hours saved, accuracy improved, etc.)
  - Can be measured with current or planned features

- [ ] **Features** map directly to pain points
  - Each feature solves a specific problem from Problem Statement
  - No "nice to have" features in MVP scope
  - User stories follow "As a [persona], I need [feature] so that [outcome]"

- [ ] **Out of Scope** is explicit
  - Lists features you're NOT building (prevents scope creep)
  - Explains why each is out of scope
  - Documents when they might be added (future phase)

- [ ] **Enterprise Considerations** documented
  - Multi-tenant architecture requirements identified
  - Scalability targets defined (X schools, Y users)
  - Compliance needs listed (FERPA, GDPR, etc.)
  - Integration requirements noted (SSO, APIs, etc.)

### Validation Check

- [ ] **Real user validation**
  - Shown PRD to at least one person in target persona
  - They confirmed the problem resonates
  - They confirmed proposed features would help
  
- [ ] **You can answer these questions:**
  - Why this product? (vs. existing solutions)
  - Why now? (timing/urgency)
  - Why me? (your unique advantage to build this)

### Context Documentation

- [ ] **CONTEXT.md updated** with:
  - Current state (what stage you're in)
  - Why you're building this now
  - Any constraints (time, budget, skills)
  - Your personal context (availability, energy)

- [ ] **ROADMAP.md shows sequencing**
  - Features prioritized
  - Dependencies identified
  - Rough timeline estimated

### Gate Keeper Decision

**Can you proceed to Architecture?**

- [ ] YES - All boxes checked, ready to design technical solution
- [ ] NO - Need more validation, return to user research
- [ ] NEEDS WORK - Specific sections need refinement: _______________

**If NO or NEEDS WORK:** Document what's missing in CONTEXT.md and address before proceeding.

---

## Gate 2: ARCHITECTURE → QUALITY

**Purpose:** Ensure technical design is sound before implementing.

### Technical Specification Completeness

- [ ] **Database schema designed**
  - All tables identified
  - Relationships mapped
  - Supports multi-tenancy (org_id/school_id on every table)
  - Indexes planned for performance
  - RLS policies defined (if using Supabase)

- [ ] **API design documented**
  - Endpoints listed with methods (GET, POST, etc.)
  - Request/response formats defined
  - Authentication/authorization approach clear
  - Error handling strategy documented

- [ ] **Component structure planned**
  - File organization follows standards
  - Component hierarchy makes sense
  - Reusable components identified
  - State management approach clear

- [ ] **Third-party integrations identified**
  - All external services listed
  - API contracts understood
  - Authentication methods documented
  - Rate limits and costs considered

### Enterprise Readiness Check

- [ ] **Multi-tenant architecture**
  - No hardcoded tenant references
  - Data isolation strategy clear
  - Tenant context propagates through all layers
  
- [ ] **SSO compatibility**
  - Auth system can support SSO later
  - User model supports federated identity
  - No blockers to adding Google/Microsoft SSO

- [ ] **Scalability validated**
  - "Add second customer" scenario reviewed
  - No single-tenant assumptions in design
  - Database queries will perform at scale
  - File storage organized by tenant

### Standards Alignment

- [ ] **Follows code standards**
  - File naming conventions match STANDARDS/code-standards.md
  - Code organization follows agreed patterns
  - Testing approach defined

- [ ] **Follows architecture principles**
  - Separation of concerns maintained
  - DRY (Don't Repeat Yourself) principles applied
  - SOLID principles considered
  - Matches patterns in STANDARDS/architecture-principles.md

### Decision Documentation

- [ ] **DECISIONS.md updated** with:
  - Why this tech stack?
  - Why this database schema?
  - Why this component structure?
  - What alternatives were considered?
  - What are the tradeoffs?

### Review Check

- [ ] **"What Would Matt Do?" test passed**
  - Would your mentor approve this approach?
  - Does it follow Clean Code principles?
  - Is it maintainable by you in 6 months?

- [ ] **Second customer scenario tested**
  - Walked through: "What if we add School #2 tomorrow?"
  - Identified any hardcoded assumptions
  - Confirmed tenant isolation works

### Gate Keeper Decision

**Can you proceed to Implementation?**

- [ ] YES - Architecture is sound, ready to build
- [ ] NO - Design has flaws, needs rework
- [ ] NEEDS WORK - Specific areas need refinement: _______________

**If NO or NEEDS WORK:** Document issues in DECISIONS.md and revise architecture.

---

## Gate 3: QUALITY → DONE

**Purpose:** Ensure feature is truly complete before calling it shipped.

### Feature Functionality

- [ ] **Works as specified in PRD**
  - All acceptance criteria met
  - User stories can be completed
  - Edge cases handled

- [ ] **No critical bugs**
  - No console errors or warnings
  - No broken user flows
  - Data saves and loads correctly

- [ ] **Success metrics measurable**
  - Analytics/logging in place to track metrics from PRD
  - Can demonstrate value to pilot customer
  - Dashboard or report shows the data

### Quality Standards

- [ ] **Code quality acceptable**
  - Follows STANDARDS/code-standards.md
  - No obvious code smells
  - Comments explain "why" not "what"
  - Functions are reasonably sized

- [ ] **Responsive design**
  - Works on mobile (if applicable)
  - Works on tablet (if applicable)
  - Works on desktop
  - No layout breaks at different sizes

- [ ] **Performance acceptable**
  - Page loads under 3 seconds
  - No slow queries (check database)
  - Images optimized
  - No memory leaks

### Testing & Review

- [ ] **Manual QA completed**
  - Tested happy path
  - Tested error cases
  - Tested with realistic data volume
  - Tested on multiple browsers (if web app)

- [ ] **AI code review done**
  - Used Devin/Claude Code to review
  - Addressed critical feedback
  - Documented decisions to ignore non-critical items

- [ ] **Security check**
  - No credentials in code
  - User input validated/sanitized
  - Authentication/authorization working
  - RLS policies active (if using Supabase)

### Deployment & Documentation

- [ ] **Deployed to production**
  - Feature live and accessible
  - Database migrations run successfully
  - Environment variables configured
  - Monitoring/logging active

- [ ] **Documentation updated**
  - User-facing docs updated (if applicable)
  - README updated (if applicable)
  - CHANGELOG.md updated with what shipped
  - CONTEXT.md updated with learnings

### Pilot Customer Validation

- [ ] **Pilot customer has access**
  - They can use the feature
  - They know it exists (you told them)
  - You've asked for feedback

- [ ] **Definition of Done met**
  - All criteria from STANDARDS/definition-of-done.md checked
  - No blockers remaining
  - Technical debt documented (if any shortcuts taken)

### Gate Keeper Decision

**Is this feature truly DONE?**

- [ ] YES - Feature is shipped and validated
- [ ] NO - Needs more work before calling it done
- [ ] BLOCKED - External dependency or issue: _______________

**If NO or BLOCKED:** Document what's needed in CONTEXT.md and finish before moving to next feature.

---

## Emergency Gates: When to Stop

### Red Flags That Mean "Stop and Reassess"

**Stop implementing if:**

- [ ] You've been coding 3+ hours without shipping anything
  - **Action:** Update CONTEXT.md, return to Architecture stage

- [ ] You're adding features not in PRD
  - **Action:** Update PRD first, then resume implementation

- [ ] Pilot customer feedback contradicts roadmap
  - **Action:** Return to Strategy, revise PRD based on feedback

- [ ] Architecture won't scale (you realize mid-build)
  - **Action:** Stop coding, return to Architecture, redesign

- [ ] You're building multiple approaches in parallel
  - **Action:** Pick ONE approach, document decision in DECISIONS.md

- [ ] Technical debt is piling up faster than features ship
  - **Action:** Schedule refactor sprint, update ROADMAP.md

### When in Doubt

**Ask yourself:**
1. Can I explain this feature's value to a pilot customer right now?
2. Will this work when we add customer #2?
3. Would Matt approve this code?
4. Am I building what's in the PRD, or something else?

**If you answer NO to any:** Stop and reassess which stage you actually need to be in.

---

## Gate Checklist Usage

### How to Use These Checklists

1. **Copy checklist** into CONTEXT.md before attempting to move stages
2. **Check boxes honestly** - don't rush through
3. **Document gaps** - if you can't check a box, write why
4. **Fix gaps** before proceeding
5. **Sign off** - date stamp when gate passed

### Example Gate Sign-Off in CONTEXT.md
```markdown
## Gate 1: Strategy → Architecture (Completed 2026-02-08)

✅ All PRD sections complete
✅ Validated with athletic trainer at pilot school
✅ Enterprise considerations documented
✅ ROADMAP.md sequenced

Ready to proceed to Architecture stage.

Signed: Phil
```

### When to Skip a Gate Check

**NEVER.** 

These gates exist to prevent:
- Building the wrong thing (Strategy gate)
- Building it the wrong way (Architecture gate)
- Shipping broken features (Quality gate)

**5 minutes checking boxes saves hours of rework.**

---

**Next:** See templates in `/TEMPLATES` directory for document structures.
