# Product Roadmap

**Project:** [Project Name]  
**Last Updated:** [Date]  
**Planning Horizon:** [e.g., 6 months, 1 year]

---

## Roadmap Philosophy

**Our Approach:**
- Ship small, learn fast
- One feature at a time (no parallel work)
- Pilot customer feedback drives priorities
- Enterprise foundations built from day one

**Sequencing Principles:**
1. **Customer value first** - Features that solve immediate pain
2. **Foundation second** - Architecture that supports scale
3. **Delight third** - Polish and nice-to-haves

---

## Current Focus

**Active Sprint:** [Week of Date]

**This Week's ONE Feature:**
- **Feature:** [Name]
- **Why Now:** [Reason this is priority]
- **Success Metric:** [How we'll know it worked]
- **Target Ship Date:** [Date]

**Status:** [Not Started / In Progress / In Review / Shipped]

---

## Phase 1: MVP / Pilot Launch

**Goal:** Ship minimum viable product to first pilot customer  
**Timeline:** [Start Date] → [End Date]  
**Status:** [Not Started / In Progress / Complete]

### Core Features (Must Have)

#### ✅ Feature 1: [Feature Name]
- **Description:** [What it does]
- **Why Essential:** [Why it's in MVP]
- **Success Metric:** [How we measure success]
- **Dependencies:** [What needs to exist first]
- **Status:** [Shipped / In Progress / Not Started]
- **Shipped Date:** [Date if complete]

#### ⏳ Feature 2: [Feature Name]
- **Description:** [What it does]
- **Why Essential:** [Why it's in MVP]
- **Success Metric:** [How we measure success]
- **Dependencies:** [What needs to exist first]
- **Status:** [Shipped / In Progress / Not Started]
- **Target Date:** [When we aim to ship]

#### 📋 Feature 3: [Feature Name]
- **Description:** [What it does]
- **Why Essential:** [Why it's in MVP]
- **Success Metric:** [How we measure success]
- **Dependencies:** [What needs to exist first]
- **Status:** [Shipped / In Progress / Not Started]
- **Target Date:** [When we aim to ship]

### Phase 1 Milestones

- [ ] **Milestone 1:** [Description] - [Target Date]
- [ ] **Milestone 2:** [Description] - [Target Date]
- [ ] **Milestone 3:** MVP deployed to pilot customer - [Target Date]

---

## Phase 2: Pilot Validation & Iteration

**Goal:** Validate product-market fit with pilot customer, iterate based on feedback  
**Timeline:** [Start Date] → [End Date]  
**Status:** [Not Started / In Progress / Complete]

### Iteration Features (Based on Pilot Feedback)

#### Feature X: [Feature Name]
- **Description:** [What it does]
- **Pilot Feedback:** [What customer said that drove this]
- **Success Metric:** [How we measure success]
- **Dependencies:** [What needs to exist first]
- **Status:** [Not Started]
- **Target Date:** [When we aim to ship]

#### Feature Y: [Feature Name]
- **Description:** [What it does]
- **Pilot Feedback:** [What customer said that drove this]
- **Success Metric:** [How we measure success]
- **Dependencies:** [What needs to exist first]
- **Status:** [Not Started]
- **Target Date:** [When we aim to ship]

### Phase 2 Milestones

- [ ] **Milestone 1:** Pilot customer actively using product - [Target Date]
- [ ] **Milestone 2:** Core success metrics validated - [Target Date]
- [ ] **Milestone 3:** Product roadmap refined based on learnings - [Target Date]

---

## Phase 3: Multi-Customer Preparation

**Goal:** Prepare for second customer (multi-tenancy, scale infrastructure)  
**Timeline:** [Start Date] → [End Date]  
**Status:** [Not Started / In Progress / Complete]

### Enterprise Foundation Features

#### Feature: Multi-Tenant Data Model
- **Description:** Ensure all data properly isolated by org_id/school_id
- **Why Essential:** Can't add customer #2 without this
- **Success Metric:** Can onboard second customer in < 1 day
- **Dependencies:** Database refactor if needed
- **Status:** [Not Started]
- **Target Date:** [When we aim to ship]

#### Feature: Tenant Management Dashboard
- **Description:** Admin UI for managing organizations/schools
- **Why Essential:** Can't manually SQL every new customer
- **Success Metric:** Non-technical person can add new tenant
- **Dependencies:** Multi-tenant data model complete
- **Status:** [Not Started]
- **Target Date:** [When we aim to ship]

#### Feature: Self-Serve Onboarding
- **Description:** New customers can sign up and configure without you
- **Why Essential:** Can't scale with manual onboarding
- **Success Metric:** Customer goes from signup to using product in < 15 min
- **Dependencies:** Tenant management, billing integration
- **Status:** [Not Started]
- **Target Date:** [When we aim to ship]

### Phase 3 Milestones

- [ ] **Milestone 1:** Second pilot customer onboarded - [Target Date]
- [ ] **Milestone 2:** Both customers operating independently - [Target Date]
- [ ] **Milestone 3:** Self-serve onboarding live - [Target Date]

---

## Phase 4: Market Expansion

**Goal:** Scale to 10+ customers, establish revenue model  
**Timeline:** [Start Date] → [End Date]  
**Status:** [Not Started / In Progress / Complete]

### Scale Features

#### Feature: SSO Integration
- **Description:** Google Workspace and Microsoft SSO support
- **Why Essential:** Enterprise requirement
- **Success Metric:** 80%+ of users login via SSO
- **Dependencies:** Auth system refactor
- **Status:** [Not Started]
- **Target Date:** [When we aim to ship]

#### Feature: Advanced Reporting
- **Description:** District-level analytics and insights
- **Why Essential:** Upsell opportunity for enterprise
- **Success Metric:** 50%+ of enterprise customers use this
- **Dependencies:** Multi-tenant data model
- **Status:** [Not Started]
- **Target Date:** [When we aim to ship]

#### Feature: API Access
- **Description:** REST API for third-party integrations
- **Why Essential:** Integration partners drive acquisition
- **Success Metric:** 3+ integration partners live
- **Dependencies:** API documentation, auth tokens
- **Status:** [Not Started]
- **Target Date:** [When we aim to ship]

### Phase 4 Milestones

- [ ] **Milestone 1:** 10 paying customers - [Target Date]
- [ ] **Milestone 2:** $10K MRR - [Target Date]
- [ ] **Milestone 3:** Sales process documented and repeatable - [Target Date]

---

## Enterprise Foundations Track

**Parallel Track:** Build scale infrastructure alongside customer features

### Infrastructure & Operations

#### Monitoring & Alerting
- **What:** Error tracking, uptime monitoring, performance dashboards
- **Why:** Can't scale without visibility
- **Status:** [Not Started / In Progress / Complete]
- **Target:** [Date]

#### Automated Testing
- **What:** Unit tests for critical paths, integration tests
- **Why:** Can't ship fast without confidence
- **Status:** [Not Started / In Progress / Complete]
- **Target:** [Date]

#### CI/CD Pipeline
- **What:** Automated deployments, staging environment
- **Why:** Manual deploys don't scale
- **Status:** [Not Started / In Progress / Complete]
- **Target:** [Date]

#### Database Optimization
- **What:** Indexes, query optimization, connection pooling
- **Why:** Performance degrades with scale
- **Status:** [Not Started / In Progress / Complete]
- **Target:** [Date]

#### Security Hardening
- **What:** Penetration testing, security audit, SOC 2 prep
- **Why:** Enterprise requirement
- **Status:** [Not Started / In Progress / Complete]
- **Target:** [Date]

---

## Dependencies Map

### Feature Dependencies
```
MVP Features
├── Feature 1 (no dependencies)
├── Feature 2 (depends on Feature 1)
└── Feature 3 (depends on Feature 1, Feature 2)

Multi-Tenant Prep
├── Data model refactor (no dependencies)
├── Tenant management (depends on data model)
└── Self-serve onboarding (depends on tenant management)

Scale Features
├── SSO (depends on auth refactor)
├── Advanced reporting (depends on multi-tenant data model)
└── API access (depends on auth tokens, documentation)
```

### Blocker Tracking

**Current Blockers:**
- [Feature blocked] → [What it's waiting for] → [ETA]

**Upcoming Blockers:**
- [Feature that will block future work] → [Mitigation plan]

---

## Parking Lot (Future Considerations)

### Deferred Features

**Not Prioritized Yet, But Worth Tracking:**

1. **[Feature Name]**
   - Why interesting: [Value proposition]
   - Why deferred: [Reason not doing now]
   - Reconsider when: [Condition for activation]

2. **[Feature Name]**
   - Why interesting: [Value proposition]
   - Why deferred: [Reason not doing now]
   - Reconsider when: [Condition for activation]

### Customer Requests (Not Yet Prioritized)

**From Pilot Feedback:**
- [Request] - [Requested by] - [Date] - [Priority: Low/Med/High]
- [Request] - [Requested by] - [Date] - [Priority: Low/Med/High]

**Review Cadence:** Monthly

---

## Timeline Visualization
```
Q1 2026
├── Feb: MVP development (Features 1-3)
├── Mar: Pilot launch and iteration
└── Apr: Second customer prep

Q2 2026
├── May: Multi-tenant infrastructure
├── Jun: Self-serve onboarding
└── Jul: Market expansion begins

Q3 2026
├── Aug: SSO and enterprise features
├── Sep: API and integrations
└── Oct: 10+ customers milestone

Q4 2026
├── Nov: Advanced features
├── Dec: Year-end review
└── Jan 2027: Planning for next phase
```

---

## Success Criteria by Phase

### Phase 1 Success = MVP Shipped
- [ ] All core features deployed
- [ ] Pilot customer using product
- [ ] No critical bugs blocking usage

### Phase 2 Success = Product-Market Fit
- [ ] Pilot customer success metrics hit targets
- [ ] Pilot customer willing to pay
- [ ] Clear understanding of who else would buy this

### Phase 3 Success = Multi-Customer Ready
- [ ] Second customer onboarded successfully
- [ ] Both customers operating independently
- [ ] Infrastructure can support 10+ customers

### Phase 4 Success = Scalable Business
- [ ] 10+ paying customers
- [ ] Revenue covers costs + your salary
- [ ] Repeatable sales process

---

## Roadmap Review Process

### Weekly Review (Every Friday)
- Update current sprint status
- Check if this week's feature shipped
- Plan next week's ONE feature
- Adjust near-term priorities based on feedback

### Monthly Review (First Friday of Month)
- Review phase progress
- Evaluate parking lot items
- Adjust timeline based on velocity
- Update dependencies and blockers

### Quarterly Review (End of Quarter)
- Major milestone assessment
- Phase transition evaluation
- Strategic pivots if needed
- Long-term roadmap adjustments

---

## Change Log

| Date | Change | Reason | Updated By |
|------|--------|--------|------------|
| [Date] | [What changed in roadmap] | [Why] | [Name] |
| [Date] | [What changed in roadmap] | [Why] | [Name] |

---

## Template Usage Notes

**How to Use This Roadmap:**
1. Start by filling out Phase 1 in detail
2. Keep Phase 2-4 high-level (will change based on learnings)
3. Update weekly with current sprint progress
4. Move features between phases as priorities shift
5. Always maintain ONE active feature at a time

**This Roadmap Is:**
- A living document (expect changes)
- A sequencing tool (what order to build)
- A communication tool (show pilot customers what's coming)

**This Roadmap Is NOT:**
- A commitment (dates will shift)
- Comprehensive (not every feature will be here)
- Fixed (market feedback > original plan)

---

**Last Updated:** [Date]  
**Next Review:** [Date]
