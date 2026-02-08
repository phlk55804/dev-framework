# Workflow SOP: Three-Stage Development Process

> Your battle plan for shipping features without chaos.

---

## Overview

Every feature follows the same path:
```
STRATEGY → ARCHITECTURE → QUALITY
(What/Why)  (How)          (Correct/Deploy)
```

**Each stage has:**
- Clear inputs (what you need to start)
- Clear outputs (what you produce)
- Gate checklist (validation before moving forward)
- AI prompt templates (in STAGE-PROMPTS.md)

---

## Stage 1: STRATEGY (What & Why)

### Purpose
Define WHAT you're building and WHY it matters before writing any code.

### Inputs
- User research / customer interviews
- Pain points identified
- Competitive landscape
- Your constraints (time, skills, resources)

### Process
1. Open `PRD.md` (or copy from template if new project)
2. Reference `STAGE-PROMPTS.md` → "Stage 1: Strategy Session"
3. Work with AI to complete PRD sections:
   - Problem Statement
   - User Personas  
   - Success Metrics
   - Feature Specifications
   - Out of Scope
   - Enterprise Considerations
4. Update `CONTEXT.md` with current state
5. Update `ROADMAP.md` with sequencing

### Outputs
- ✅ Complete PRD.md (or updated section for new feature)
- ✅ CONTEXT.md updated with "why now"
- ✅ ROADMAP.md shows this feature's place in sequence

### Gate Checklist
Before moving to Stage 2, verify:
- [ ] Problem statement answers "why this matters"
- [ ] Success metrics are measurable (not vanity metrics)
- [ ] User personas are specific (not "busy coaches")
- [ ] Features map directly to pain points
- [ ] Out-of-scope is explicit (prevents scope creep)
- [ ] Enterprise considerations documented
- [ ] You've validated this with a real user (or pilot customer)

**Gate Keeper:** You. If you can't check every box, stay in Strategy.

### Time Investment
- New project PRD: 4-8 hours (do NOT rush this)
- New feature addition: 30-60 minutes

### AI Collaborator
Claude Pro (or any strategic/reasoning AI)

---

## Stage 2: ARCHITECTURE (How)

### Purpose
Design the technical approach that delivers on the PRD without painting yourself into a corner.

### Inputs
- ✅ Completed PRD.md
- CONTEXT.md (current state)
- DECISIONS.md (past choices to respect)
- STANDARDS/architecture-principles.md

### Process
1. Reference `STAGE-PROMPTS.md` → "Stage 2: Architecture Session"
2. Work with AI to create technical specification:
   - Database schema (with multi-tenant support)
   - API design
   - Component structure
   - Third-party integrations
   - File organization
3. Document key decisions in `DECISIONS.md`
4. Scaffold code structure (but don't implement yet)
5. Update `CONTEXT.md` with technical approach

### Outputs
- ✅ Technical specification document (can live in PRD or separate file)
- ✅ DECISIONS.md updated with "why we chose X over Y"
- ✅ Code scaffolding (folders, empty files with comments)
- ✅ CONTEXT.md updated with architecture decisions

### Gate Checklist
Before moving to Stage 3, verify:
- [ ] Tech spec maps to every feature in PRD
- [ ] Database schema supports multi-tenancy (school_id/org_id)
- [ ] Authentication approach supports SSO later
- [ ] No single-tenant assumptions in design
- [ ] Integrations identified and API contracts defined
- [ ] File structure follows agreed standards
- [ ] Matt would approve this approach (WWMD - What Would Matt Do?)

**Gate Keeper:** You + AI review. Walk through the "what if we add a second customer" scenario.

### Time Investment
- Major feature: 2-4 hours
- Minor feature: 30-60 minutes

### AI Collaborator
Deepagent, Claude Code, or any implementation-focused AI

---

## Stage 3: QUALITY (Correct & Deploy)

### Purpose
Implement, test, review, and ship the feature with confidence.

### Inputs
- ✅ Completed technical specification
- ✅ Code scaffolding from Stage 2
- STANDARDS/code-standards.md
- STANDARDS/definition-of-done.md

### Process
1. Reference `STAGE-PROMPTS.md` → "Stage 3: Implementation Session"
2. Implement feature following technical spec
3. Test against success metrics from PRD
4. AI code review (Devin or Claude Code)
5. Manual QA in staging environment
6. Deploy to production
7. Update `CHANGELOG.md` with what shipped
8. Update `CONTEXT.md` with learnings
9. Collect user feedback (pilot customer)

### Outputs
- ✅ Working feature in production
- ✅ Tests passing (manual or automated)
- ✅ CHANGELOG.md updated
- ✅ CONTEXT.md updated with deployment notes
- ✅ User feedback collected and documented

### Gate Checklist
Before marking "done", verify:
- [ ] Feature works as described in PRD
- [ ] Success metrics can be measured
- [ ] No console errors or warnings
- [ ] Mobile responsive (if applicable)
- [ ] Pilot customer has access
- [ ] Documentation updated (if user-facing)
- [ ] Meets definition-of-done criteria

**Gate Keeper:** Your pilot customer. If they can't use it, it's not done.

### Time Investment
- Varies by feature complexity
- Build time + 50% for review/testing/deployment

### AI Collaborator
Devin, Claude Code, or any review/deployment AI

---

## Between Stages: Context Handoffs

### STRATEGY → ARCHITECTURE
```
You (to Architecture AI): 
"Here's the completed PRD [paste]. Design the technical approach 
that delivers this without limiting future scale."
```

### ARCHITECTURE → QUALITY  
```
You (to Implementation AI):
"Here's the technical spec [paste]. Implement this feature 
following our code standards [paste standards]."
```

---

## One Active Project Rule

### The System
- Only ONE project has `[ACTIVE]` status at a time
- Active project gets AI time and daily attention
- All other projects live in `/parking-lot`

### Switching Projects
1. Archive current ACTIVE project's context
2. Document why you're switching (in CONTEXT.md)
3. Move to parking lot
4. Promote new project to ACTIVE
5. Review its PRD/CONTEXT before resuming

### Parking Lot Projects
- Can have complete PRDs (planning is allowed)
- Can collect user feedback
- Can update roadmaps
- **Cannot** consume AI implementation time

---

## Emergency Brake: When to Stop and Reassess

**STOP implementing if:**
- You've been coding for 3+ hours without shipping anything
- You're adding features not in the PRD (update PRD first!)
- The pilot customer's feedback contradicts your roadmap
- You realize the architecture won't scale (back to Stage 2)
- You're building in parallel with no integration plan

**Instead:**
1. Update CONTEXT.md with what you learned
2. Return to appropriate stage (usually Strategy)
3. Revise PRD/architecture based on new information
4. Resume with corrected direction

---

## Weekly Cadence

### Monday (Strategy Time)
- Review last week's CHANGELOG
- Check pilot customer feedback
- Update ROADMAP for current week
- Pick ONE feature to build this week

### Tue-Thu (Build Time)
- Stage 2 + Stage 3 execution
- Ship incremental progress
- Collect feedback continuously

### Friday (Review & Plan)
- Deploy week's work to pilot customer
- Update all docs (CHANGELOG, CONTEXT)
- Review parking lot (anything need to move to ACTIVE?)
- Plan next week's ONE feature

### Shabbat (Rest)
- No work from Friday sunset to Saturday sunset
- Let ideas marinate
- Come back Monday refreshed

---

## Metrics That Matter

Track these in CONTEXT.md:

### Velocity
- Features shipped per week
- Lines of code generated (feed your addiction!)
- Pilot customer engagement

### Quality
- Bugs reported by pilot
- Time to fix critical issues
- Technical debt created this week

### Business
- Pilot customer satisfaction
- Feature requests from users
- Revenue (when applicable)

### Learning
- New concepts mastered
- Code review feedback incorporated
- Architectural patterns learned

---

## Tools & Where They Fit

| Tool | Stage | Purpose |
|------|-------|---------|
| Claude Pro | 1 | Strategic planning, PRD refinement |
| Deepagent | 2 | Code scaffolding, structure |
| Claude Code | 2,3 | Implementation, refactoring |
| Devin | 3 | Code review, deployment |
| GitHub | All | Version control, documentation |
| Supabase | 3 | Database, auth, storage |

**You can swap tools** (that's the point of this framework), but the stages remain the same.

---

## This Framework Is For

✅ Shipping profitable SaaS products  
✅ Maintaining velocity without creating chaos  
✅ Building enterprise-ready architecture from day one  
✅ Demonstrating mature SDLC for CODA application  
✅ Taking over the world with AI as your development team  

**Not for:**
❌ Exploratory prototyping (that's pre-Stage 1)
❌ Open-source contributions (different workflow)  
❌ Client services work (scope is externally controlled)

---

**Next:** See `STAGE-PROMPTS.md` for exact prompts to use at each stage.
