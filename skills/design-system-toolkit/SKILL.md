---
name: design-system-toolkit
description: >
  Master entry point for all UI/UX design work. Import ONLY this file — it automatically loads
  the correct sub-skills based on your project context. Covers design principles, UX flow auditing,
  UI visual auditing, Figma auto layout auditing, and design-to-code export for mobile, web, or cross-platform projects.

  Trigger when the user says anything related to: designing screens, creating UI, reviewing UX,
  auditing a design, checking if a design is modern, building a user flow, starting a new design
  project, setting up a design system, "use the design toolkit", "start design", "design this",
  "audit this", "review my design", "check this flow", "is this UI good", or any Figma design task.

  IMPORTANT: This is the ONLY file you need to read first. It will tell you which files to load next.
---

# Design System Toolkit — Entry Point

> **One import. Five skills. Any project.**

## First Steps

1. **Read `project/APP_PLAN.md`** (if it exists) — The session state with project context and history.
2. **Starting from scratch?** → Read `discovery.md` — Runs progressive discovery (6 core questions, one at a time). Generates APP_PLAN.md.
3. **Need to route?** → Read `router.md` — Classifies user intent and routes to the correct sub-skill. Only needed for base `/design` command.
4. **Design system setup?** → Read `governance.md` — Handles design system initialization in Figma, component lifecycle, versioning.
5. **Load sub-skills on demand** from `skills/` — Only when the task requires them. Never load all upfront.

## File Manifest

```
design-system-toolkit/
│
├── SKILL.md                 ← You are here (entry point — read this first)
├── discovery.md             ← Progressive project discovery (6 core Q's, one at a time)
├── router.md                ← Intent classification & menu (base /design only)
├── governance.md            ← Design system init, lifecycle, versioning
│
├── project/                 ← YOUR project state (Claude updates, you can edit)
│   └── APP_PLAN.md          ← Session state (context, decisions, progress)
│
└── skills/                  ← Reference library (do not edit)
    ├── 01-design-guide.md                    ← Core design principles (§1-13)
    ├── 01-ref-design-patterns.md             ← Reference: checklists, Figma workflow, icons, spacing, patterns (§14-26)
    ├── 02-ux-flow-audit.md                   ← UX flow validation (7 phases)
    ├── 03-ui-visual-audit.md                 ← UI visual inspection (11 layers)
    ├── 04a-figma-build-core.md               ← Figma build: screens & auto layout
    ├── 04b-component-architect.md            ← Smart component creation & variants
    ├── 04c-code-export.md                    ← Design-to-code translation
    ├── 04d-figma-repair.md                   ← File repair & cleanup scripts
    ├── 04e-figma-tools-ref.md                ← MCP tool reference (on demand)
    └── 05-figma-autolayout-audit.md          ← Figma auto layout audit (6 phases)
```

## Quick Reference

| What the user says | What to do |
|---|---|
| "Start a new design project" | `discovery.md` → Create `project/APP_PLAN.md` → `governance.md` Step 3 → Route to sub-skill |
| "Design [screen/feature]" | Load `project/APP_PLAN.md` → Load `skills/01-design-guide` (+ `01-ref-design-patterns` if review/reference needed) |
| "Audit this flow" / "Check this UX" | Load `project/APP_PLAN.md` (if exists) → Load `skills/02` |
| "Audit this UI" / "Check these screens" | Load `project/APP_PLAN.md` (if exists) → Load `skills/03` |
| "Full design review" | Load `project/APP_PLAN.md` (if exists) → Load `skills/02` then `skills/03` |
| "Audit auto layout" / "Check layout" | Load `project/APP_PLAN.md` (if exists) → Load `skills/05` |
| "Build this in Figma" | Load `project/APP_PLAN.md` → Load `skills/04a` |
| "Create a component" / "Add variants" | Load `project/APP_PLAN.md` → Load `skills/04b` |
| "Convert to code" / "Export to React" | Load `project/APP_PLAN.md` → Load `skills/04c` |
| "Fix my Figma file" / "Clean up" | Load `project/APP_PLAN.md` → Load `skills/04d` |
| "Convert to code" / "Export to React" | Load `project/APP_PLAN.md` → Load `skills/04c` |
| "Set up styles" / "Create variables" | `governance.md` → Load `skills/04a` for execution |
| "Show project context" | Read `project/APP_PLAN.md` and display |
| "Update project context" | `discovery.md` → Re-run specific questions → Update `project/APP_PLAN.md` |
| "Switch to [project name]" | Load that project's APP_PLAN from `project/` |
