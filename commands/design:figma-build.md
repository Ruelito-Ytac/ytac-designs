---
name: design:figma-build
description: >
  Build or modify designs directly in Figma using MCP tools. Creates frames, components,
  auto layout structures, text styles, color variables, and organizes layers.

  Examples:
    /design:figma-build
    /design:figma-build Create the login screen in Figma
    /design:figma-build Fix the auto layout on the card component
---

# Figma Build

You have been invoked via the `/design:figma-build` command.

## Instructions

1. **Load project context** (required): Read `skills/design-system-toolkit/project/APP_PLAN.md` from this plugin directory.
   - If APP_PLAN.md does NOT exist, run discovery first: read `skills/design-system-toolkit/discovery.md` and ask the 6 core questions one at a time. Generate APP_PLAN.md before proceeding.
2. **Check design system**: Verify text styles + color styles exist in Figma. If not, read `skills/design-system-toolkit/governance.md` and run the design system initialization first.
3. **Load the build skill**: Read `skills/design-system-toolkit/skills/04-figma-to-code.md` (Build Mode) from this plugin directory.
4. **If the user specified what to build** via `$ARGUMENTS`, start building immediately.
5. **If no arguments were provided**, ask the user what they want to build or fix in Figma.
6. **After completing a build session**, recommend: "Run `/design:figma-autolayout-audit` to validate the auto layout structure."

## User's Request

$ARGUMENTS
