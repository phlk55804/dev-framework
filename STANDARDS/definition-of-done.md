# Definition of Done

**Project:** [Your Framework]  
**Last Updated:** [Date]  
**Philosophy:** A feature isn't "done" until it's shipped, validated, and documented.

---

## Purpose

This checklist prevents:
- Shipping half-finished features
- Forgetting critical steps
- Technical debt from accumulating
- "Works on my machine" problems

**The Rule:** If you can't check every box, the feature isn't done.

---

## Core Definition of Done

A feature is DONE when all of these are true:

### 1. Functionality

- [ ] **Feature works as specified in PRD**
  - All acceptance criteria met
  - User stories can be completed
  - Tested with realistic data

- [ ] **No known critical bugs**
  - No blocking issues preventing use
  - No data corruption risks
  - No security vulnerabilities

- [ ] **Edge cases handled**
  - Empty states (no data)
  - Error states (network failures, invalid input)
  - Loading states (async operations)
  - Maximum capacity (large datasets)

### 2. Code Quality

- [ ] **Follows code standards**
  - Matches patterns in `code-standards.md`
  - No linting errors
  - TypeScript types are explicit (no `any`)

- [ ] **Readable and maintainable**
  - Function names reveal intent
  - Complex logic has explanatory comments
  - No "clever" code that's hard to understand

- [ ] **No obvious code smells**
  - Functions under 50 lines
  - Files under 300 lines
  - No deeply nested conditionals (>3 levels)
  - No repeated code (DRY principle)

### 3. Multi-Tenant Requirements

- [ ] **org_id included in all data operations**
  - Database queries filter by org_id
  - File storage organized by org_id
  - No cross-tenant data leakage possible

- [ ] **Tested with multiple orgs**
  - Created test data for 2+ orgs
  - Verified data isolation
  - Confirmed user A can't see user B's data

- [ ] **Works for "customer #2" scenario**
  - No hardcoded tenant references
  - Feature flags use org_id, not conditionals
  - Configuration stored in database, not code

### 4. Security

- [ ] **Input validation**
  - All user input validated
  - Schema validation (e.g., Zod) for API inputs
  - SQL injection prevented (parameterized queries)

- [ ] **Authorization checks**
  - User has permission to perform action
  - org_id matches user's organization
  - Role-based access working correctly

- [ ] **No sensitive data exposed**
  - No credentials in code
  - No PII in logs
  - Error messages don't leak system details

### 5. Performance

- [ ] **Acceptable load times**
  - Page loads in < 3 seconds
  - API responses in < 500ms (p95)
  - No blocking operations on UI thread

- [ ] **Database optimized**
  - Indexes exist for queried columns
  - No N+1 query patterns
  - Pagination for lists over 50 items

- [ ] **No memory leaks**
  - Event listeners cleaned up
  - Intervals/timeouts cleared
  - React effects have cleanup functions

### 6. User Experience

- [ ] **Responsive design**
  - Works on mobile (if applicable)
  - Works on tablet (if applicable)
  - Works on desktop
  - No horizontal scrolling

- [ ] **Error messages are helpful**
  - Users understand what went wrong
  - Users know how to fix it
  - Technical details logged but not shown

- [ ] **Loading states shown**
  - Spinners for async operations
  - Skeleton screens for slow loads
  - Progress indicators for long operations

- [ ] **Success feedback provided**
  - Users know when action succeeded
  - Toast notifications or success messages
  - UI updates to reflect changes

### 7. Testing

- [ ] **Manual testing complete**
  - Happy path tested
  - Error cases tested
  - Edge cases tested
  - Tested on multiple browsers (if web)

- [ ] **AI code review done**
  - Used Devin/Claude Code for review
  - Critical feedback addressed
  - Remaining issues documented

- [ ] **Key business logic tested**
  - Unit tests for calculations
  - Integration tests for critical flows
  - (Automated tests nice-to-have, not required for solo dev)

### 8. Documentation

- [ ] **CHANGELOG.md updated**
  - Feature added to changelog
  - Date and description included
  - Any breaking changes noted

- [ ] **CONTEXT.md updated**
  - Current state reflects this feature
  - Learnings captured
  - Metrics updated if applicable

- [ ] **DECISIONS.md updated (if applicable)**
  - Architectural decisions documented
  - Rationale explained
  - Alternatives considered noted

- [ ] **User-facing docs updated (if needed)**
  - Help docs reflect new feature
  - Screenshots updated
  - Video tutorials updated (if applicable)

### 9. Deployment

- [ ] **Deployed to production**
  - Code merged to main branch
  - Deployed successfully
  - No deployment errors

- [ ] **Database migrations run**
  - Schema changes applied
  - Data migrations completed
  - Rollback plan tested

- [ ] **Environment variables configured**
  - All required env vars set
  - Secrets properly stored
  - No hard-coded configuration

- [ ] **Monitoring active**
  - Error tracking enabled (Sentry, etc.)
  - Logging captures key events
  - Can measure success metrics

### 10. Validation

- [ ] **Pilot customer has access**
  - Feature is available to them
  - They've been notified it exists
  - You've asked for initial feedback

- [ ] **Success metrics measurable**
  - Analytics tracking the metric from PRD
  - Can demonstrate value to customer
  - Dashboard or report shows data

- [ ] **No immediate rollback needed**
  - Feature stable for 24+ hours
  - No critical bugs reported
  - No performance degradation

---

## Feature-Specific Additions

### For Database Schema Changes

Additional requirements:

- [ ] **Migration tested in staging**
  - Migration runs successfully
  - No data loss
  - Performance acceptable

- [ ] **Rollback plan documented**
  - How to revert schema change
  - How to restore data if needed
  - Tested rollback process

- [ ] **Backwards compatible (if possible)**
  - Old code works with new schema
  - Allows zero-downtime deployment

### For API Endpoints

Additional requirements:

- [ ] **API documentation updated**
  - Endpoint documented
  - Request/response examples
  - Error codes explained

- [ ] **Rate limiting configured**
  - Prevents abuse
  - Appropriate limits set
  - Error message when limit hit

- [ ] **Versioned appropriately**
  - Breaking changes use new version
  - Deprecation warnings for old endpoints

### For UI Components

Additional requirements:

- [ ] **Accessibility considered**
  - Keyboard navigation works
  - Screen reader friendly (if critical)
  - Color contrast meets WCAG standards

- [ ] **Loading states polished**
  - No flash of wrong content
  - Skeleton screens for slow loads
  - Smooth transitions

- [ ] **Error states designed**
  - Not just red text
  - Clear recovery actions
  - Friendly tone

### For Integrations (Third-Party APIs)

Additional requirements:

- [ ] **Error handling comprehensive**
  - API failures don't crash app
  - Retry logic for transient errors
  - Graceful degradation if service down

- [ ] **Credentials secured**
  - API keys in environment variables
  - Encrypted at rest
  - Not logged or exposed

- [ ] **Rate limits respected**
  - Stays within provider's limits
  - Implements backoff if needed
  - Monitors usage

---

## Quality Gates

### Before Calling Feature "Done"

Ask yourself:

1. **Would I be embarrassed to show this to Matt?**
   - If yes → Not done, needs cleanup

2. **Can the pilot customer actually use this?**
   - If no → Not done, needs usability work

3. **Will this work when we add customer #2?**
   - If no → Not done, needs multi-tenant fixes

4. **Will I understand this code in 6 months?**
   - If no → Not done, needs better names/comments

5. **Does this match what we specified in the PRD?**
   - If no → Either update PRD or change implementation

---

## Common "Not Done" Scenarios

### ❌ "It works on my machine"
**Missing:** Testing in production-like environment, deployment verification

### ❌ "I'll add docs later"
**Missing:** Documentation updates, changelog entry

### ❌ "It's mostly working, just a few edge cases"
**Missing:** Edge case handling, error states

### ❌ "I'll test it more after shipping"
**Missing:** Manual testing, validation with pilot customer

### ❌ "The code is messy but functional"
**Missing:** Code quality standards, readability

---

## Technical Debt Exceptions

Sometimes you need to ship with known limitations.

**If you must ship with technical debt:**

1. **Document it explicitly**
   - Add TODO comment in code
   - Create ticket/issue for cleanup
   - Note in CONTEXT.md

2. **Set a deadline**
   - "Will refactor by [date]"
   - Link to specific sprint/milestone

3. **Limit the scope**
   - Debt stays localized
   - Doesn't spread to other features
   - Clear boundary around it

4. **Get explicit approval**
   - From yourself (in writing, in CONTEXT.md)
   - From mentor if available (Matt)
   - From pilot customer if affects them

**Example:**
```markdown
## Known Technical Debt (Created 2026-02-15)

### Student Import - CSV Parsing
- **Issue:** Currently loads entire file into memory
- **Impact:** Will fail with >10,000 student CSV
- **Deadline:** Before second pilot school (estimated 2026-03-01)
- **Mitigation:** Current pilot has <500 students, so safe for now
- **Ticket:** #47
```

---

## Definition of "Shipped"

A feature is SHIPPED (beyond just "done") when:

- [ ] **Feature is in production** and accessible
- [ ] **Pilot customer is using it** (not just can use it)
- [ ] **Success metrics are being tracked**
- [ ] **No critical bugs for 7 days**
- [ ] **Feedback collected** from at least one user
- [ ] **Documentation complete** (user-facing and internal)

---

## Emergency Hotfix Exception

For critical production bugs, you can skip some items:

**Required (Non-Negotiable):**
- [ ] Fixes the critical bug
- [ ] Doesn't break anything else
- [ ] Deployed to production
- [ ] CHANGELOG.md updated

**Can Defer:**
- Comprehensive testing (just test the fix)
- Full documentation (add note, complete later)
- Code quality polish (note refactor needed)

**But must circle back within 48 hours** to complete full Definition of Done.

---

## Weekly Review

Every Friday, review features shipped this week:

- [ ] Do they all meet Definition of Done?
- [ ] Any technical debt created?
- [ ] Any patterns emerging (good or bad)?
- [ ] Any updates needed to this document?

---

## Monthly Audit

First Friday of each month:

- [ ] Review all "shipped" features from last month
- [ ] Verify they're still working
- [ ] Check if metrics show expected impact
- [ ] Identify any that need follow-up work

---

## Continuous Improvement

This document should evolve:

**Update When:**
- You find yourself repeatedly missing something
- A "done" feature causes problems later
- Pilot customer feedback reveals gaps
- Matt's code review identifies patterns

**Don't Update When:**
- You're trying to lower the bar (resist!)
- One-off edge case (not a pattern)
- External pressure to ship faster (hold the line)

---

## Checklist Template

Copy this for each feature:
```markdown
## Feature: [Name]

### Core Requirements
- [ ] Works as specified in PRD
- [ ] No critical bugs
- [ ] Edge cases handled

### Code Quality
- [ ] Follows code standards
- [ ] Readable and maintainable
- [ ] No code smells

### Multi-Tenant
- [ ] org_id in all operations
- [ ] Tested with multiple orgs
- [ ] Works for customer #2

### Security
- [ ] Input validation
- [ ] Authorization checks
- [ ] No sensitive data exposed

### Performance
- [ ] Acceptable load times
- [ ] Database optimized
- [ ] No memory leaks

### UX
- [ ] Responsive design
- [ ] Helpful error messages
- [ ] Loading/success states

### Testing
- [ ] Manual testing complete
- [ ] AI code review done
- [ ] Business logic tested

### Documentation
- [ ] CHANGELOG.md updated
- [ ] CONTEXT.md updated
- [ ] DECISIONS.md updated (if needed)

### Deployment
- [ ] Deployed to production
- [ ] Database migrations run
- [ ] Monitoring active

### Validation
- [ ] Pilot customer access
- [ ] Metrics measurable
- [ ] Stable for 24+ hours

**Done Date:** [YYYY-MM-DD]
**Shipped By:** [Your Name]
```

---

**Last Updated:** [Date]  
**Review Schedule:** Monthly, and after any production incident
