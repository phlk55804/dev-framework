# Phil's Solo SaaS Development Framework

> A systematic SDLC for solopreneurs who want to ship enterprise-ready products fast without descending into chaos.

## Philosophy

**The Problem:** As a solo founder, you can generate code faster than ever with AI. But speed without structure creates technical debt, scope creep, and half-finished projects.

**The Solution:** A three-stage workflow with clear handoffs, living documentation, and enterprise thinking baked in from day one.

**The Goal:** Ship profitable SaaS products that can scale from pilot customers to enterprise deals without rewrites.

---

## How This Framework Works

### Three Stages, Three Mindsets:

1. **STRATEGY** → What & Why (Claude Pro / Strategic AI)
2. **ARCHITECTURE** → How (Deepagent / Claude Code / Implementation AI)  
3. **QUALITY** → Correct & Deploy (Devin / Review AI)

### The Documents Are Your Interface:

- **PRD.md** - Single source of truth for what gets built
- **CONTEXT.md** - Living exposure document (both human and AI state)
- **ROADMAP.md** - Sequencing and dependencies
- **DECISIONS.md** - Architectural choices and rejections
- **CHANGELOG.md** - What actually shipped

### Core Principles:

✅ **One active project at a time** - Everything else goes in parking lot  
✅ **Enterprise-aware from day one** - Multi-tenant, scalable defaults  
✅ **Sequential intentionality** - No parallel prototyping chaos  
✅ **Radical transparency** - Expose human constraints and AI context  
✅ **Ship to learn** - Pilot → Iterate → Scale  

---

## Quick Start

### For a New Project:

1. **Copy templates** from `/TEMPLATES` into your project repo
2. **Fill out PRD.md** - Spend real time here (garbage in, garbage out)
3. **Follow WORKFLOW-SOP.md** - Three stages with gate checks
4. **Reference STAGE-PROMPTS.md** - Copy/paste to your AI collaborators

### For an Existing Project:

1. **Create PRD.md** retroactively - Document what you're actually building
2. **Add CONTEXT.md** - Capture current state and decisions made
3. **Adopt the workflow** going forward - Start clean from next feature

---

## Repository Structure
```
dev-framework/
├── README.md (this file)
├── WORKFLOW-SOP.md
├── STAGE-PROMPTS.md
├── GATE-CHECKLISTS.md
│
├── TEMPLATES/
│   ├── PRD-template.md
│   ├── CONTEXT-template.md
│   ├── ROADMAP-template.md
│   ├── DECISIONS-template.md
│   ├── CHANGELOG-template.md
│   └── ENTERPRISE-CONSIDERATIONS.md
│
└── STANDARDS/
    ├── code-standards.md
    ├── architecture-principles.md
    └── definition-of-done.md
```

---

## Who This Is For

- **Solo founders** building SaaS products
- **Technical entrepreneurs** using AI for development velocity
- **Career transitioners** (like bootcamp grads) who need to show mature SDLC thinking
- **Anyone** who's tired of half-finished projects and wants systematic execution

---

## License

MIT - Use this however helps you ship.

---

## About

Created by a solo founder building enterprise SaaS products on nights and weekends. This framework is how I stay focused and ship scalable products as a team of one (plus AI).
