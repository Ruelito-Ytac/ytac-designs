---
name: design:figma-export
description: >
  Convert Figma designs to production frontend code (React, Vue, HTML/CSS, Tailwind).

  Examples:
    /design:figma-export
    /design:figma-export Convert the home screen to React
    /design:figma-export Export all components to Tailwind
---

# Figma Export to Code

You have been invoked via the `/design:figma-export` command.

## Instructions

1. **Load project context** (required): Read `skills/design-system-toolkit/project/APP_PLAN.md` from this plugin directory.
   - If APP_PLAN.md does NOT exist, run discovery first: read `skills/design-system-toolkit/discovery.md` and ask the 6 core questions one at a time. Generate APP_PLAN.md before proceeding.
2. **Load the export skill**: Read `skills/design-system-toolkit/skills/04c-code-export.md` from this plugin directory.
3. **If the user specified what to export** via `$ARGUMENTS`, start exporting immediately.
4. **If no arguments were provided**, ask: "What would you like to export? Please share the Figma URL or describe the screen/component."

## User's Request

$ARGUMENTS
