---
name: design:screen
description: >
  Design a specific screen or feature. Uses the design guide with Figma workflow for
  creating new screens, components, and layouts. Requires project context.

  Examples:
    /design:screen
    /design:screen Login screen
    /design:screen Build the onboarding flow
---

# Design Screen

You have been invoked via the `/design:screen` command.

## Instructions

1. **Load project context** (required): Read `skills/design-system-toolkit/project/APP_PLAN.md` from this plugin directory.
   - If APP_PLAN.md does NOT exist, run discovery first: read `skills/design-system-toolkit/discovery.md` and ask the 6 core questions one at a time. Generate APP_PLAN.md before proceeding.
2. **Check design system**: Verify text styles + color styles exist in Figma. If not, read `skills/design-system-toolkit/governance.md` and run the design system initialization (Step 3) first.
3. **Load the design guide**: Read `skills/design-system-toolkit/skills/01-design-guide.md` from this plugin directory. If the task also needs review checklists, Figma workflow tips, iconography, spacing rules, or modern patterns, also load `skills/design-system-toolkit/skills/01-ref-design-patterns.md`.
4. **If the user specified a screen** via `$ARGUMENTS`, start designing it immediately.
5. **If no arguments were provided**, ask the user which screen or feature they want to design.

## User's Request

$ARGUMENTS
