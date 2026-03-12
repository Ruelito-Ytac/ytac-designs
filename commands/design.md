---
name: design
description: >
  Launch the Design System Toolkit. Use this command to start a new design project,
  design screens, audit UX flows, audit visual quality, build in Figma, or export to code.
  Accepts an optional prompt describing what you want to do.

  For direct access to specific workflows, use sub-commands:
    /design:ux-audit — Audit a UX flow (7 phases)
    /design:ui-audit — Audit UI visuals (11 layers)
    /design:full-design-audit — Combined UX + UI audit
    /design:figma-autolayout-audit — Audit Figma auto layout
    /design:screen — Design a specific screen
    /design:figma-build — Build or fix in Figma

  Examples:
    /design Start a new mobile app project
    /design Audit this checkout flow
    /design Check if these screens look modern
---

# Design System Toolkit — Slash Command

You have been invoked via the `/design` command.

## Instructions

1. **Read the skill entry point**: Load `skills/design-system-toolkit/SKILL.md` from this plugin directory.
2. **Check for existing project context**: If `skills/design-system-toolkit/project/APP_PLAN.md` exists, read it.
   - If APP_PLAN.md exists → skip discovery, load `skills/design-system-toolkit/router.md` to classify the user's intent and route to the correct sub-skill.
   - If APP_PLAN.md does NOT exist → load `skills/design-system-toolkit/discovery.md` and run the 6 core questions one at a time. After discovery, generate APP_PLAN.md, then route.
3. **If the user provided a prompt after `/design`**, use `$ARGUMENTS` as their request and route accordingly via `router.md`.
4. **If no prompt was provided**, present the interactive menu:
   - Start a new design project → discovery.md
   - Design a screen or feature → `/design:screen`
   - Audit a UX flow → `/design:ux-audit`
   - Audit visual quality → `/design:ui-audit`
   - Full design audit (UX + UI) → `/design:full-design-audit`
   - Audit Figma auto layout → `/design:figma-autolayout-audit`
   - Build or fix in Figma → `/design:figma-build`
   - Export a design to code → skill 04 (Export Mode)

## User's Request

$ARGUMENTS
