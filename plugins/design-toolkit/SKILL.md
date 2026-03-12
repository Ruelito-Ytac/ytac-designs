---
name: design-system-toolkit
description: >
  Master entry point for all UI/UX design work. Import ONLY this file — it automatically loads
  the correct sub-skills based on your project context. Covers design principles, UX flow auditing,
  and UI visual auditing for mobile, web, or cross-platform projects.

  Trigger when the user says anything related to: designing screens, creating UI, reviewing UX,
  auditing a design, checking if a design is modern, building a user flow, starting a new design
  project, setting up a design system, "use the design toolkit", "start design", "design this",
  "audit this", "review my design", "check this flow", "is this UI good", or any Figma design task.

  IMPORTANT: This is the ONLY file you need to read first. It will tell you which files to load next.
---

# Design System Toolkit — Entry Point

> **One import. Three skills. Any project.**

## First Steps

1. **Read `agents.md`** — The orchestrator. It runs project discovery, routes to the right sub-skill, and contains all governance/process knowledge.
2. **Read or create `project/APP_PLAN.md`** — The session state. If it exists, load it for project context and history. If not, `agents.md` will guide you through creating one.
3. **Load sub-skills on demand** from `skills/` — Only when the task requires them. Never load all three upfront.

## File Manifest

```
design-system-toolkit/
│
├── SKILL.md                 ← You are here (entry point — read this first)
├── agents.md                ← Orchestrator (discovery, routing, governance)
├── README.md                ← How to install and use this toolkit
│
├── project/                 ← YOUR project state (Claude updates, you can edit)
│   └── APP_PLAN.md          ← Session state (context, decisions, progress)
│
└── skills/                  ← Reference library (do not edit)
    ├── 01-mobile-web-ui-ux-design-guide.md   ← Design principles & patterns
    ├── 02-ux-flow-audit.md                   ← UX flow validation
    ├── 03-ui-visual-audit.md                 ← UI visual inspection
    └── 04-figma-to-code.md                   ← Figma production & code export
```

## Quick Reference

| What the user says | What to do |
|---|---|
| "Start a new design project" | `agents.md` → Run discovery → Create `project/APP_PLAN.md` → Route to sub-skill |
| "Design [screen/feature]" | Load `project/APP_PLAN.md` for context → Load `skills/01` |
| "Audit this flow" / "Check this UX" | Load `project/APP_PLAN.md` for context → Load `skills/02` |
| "Audit this UI" / "Check these screens" | Load `project/APP_PLAN.md` for context → Load `skills/03` |
| "Build this in Figma" | Load `project/APP_PLAN.md` for context → Load `skills/04` |
| "Convert to code" / "Export to React" | Load `project/APP_PLAN.md` for context → Load `skills/04` |
| "Fix my Figma file" / "Add auto layout" | Load `project/APP_PLAN.md` for context → Load `skills/04` |
| "Set up styles" / "Create variables" | `agents.md` Step 3 → Load `skills/04` for execution |
| "Full design review" | Load `project/APP_PLAN.md` for context → Load `skills/02` then `skills/03` |
| "Show project context" | Read `project/APP_PLAN.md` and display the current state |
| "Update project context" | `agents.md` → Re-run specific discovery questions → Update `project/APP_PLAN.md` |
| "Switch to [project name]" | Load that project's APP_PLAN from `project/` |
