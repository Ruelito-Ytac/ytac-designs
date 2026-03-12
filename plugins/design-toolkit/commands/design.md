---
name: design
description: >
  Launch the Design System Toolkit. Use this command to start a new design project,
  design screens, audit UX flows, audit visual quality, build in Figma, or export to code.
  Accepts an optional prompt describing what you want to do.

  Examples:
    /design Start a new mobile app project
    /design Audit this checkout flow
    /design Check if these screens look modern
    /design Build the login screen in Figma
    /design Convert this component to React
---

# Design System Toolkit — Slash Command

You have been invoked via the `/design` command.

## Instructions

1. **Read the skill entry point**: Load `skills/design-system-toolkit/SKILL.md` from this plugin directory.
2. **Follow SKILL.md instructions exactly** — it will route you to `agents.md` for orchestration, `project/APP_PLAN.md` for session state, and the appropriate sub-skill.
3. **If the user provided a prompt after `/design`**, use `$ARGUMENTS` as their request and route accordingly.
4. **If no prompt was provided**, greet the user and ask what they'd like to work on — offer these options:
   - Start a new design project
   - Design a screen or feature
   - Audit a UX flow
   - Audit visual quality
   - Build or fix something in Figma
   - Export a design to code

## User's Request

$ARGUMENTS
