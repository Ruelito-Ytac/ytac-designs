---
name: design:figma-autolayout-audit
description: >
  Audit Figma auto layout structure and best practices. Inspects 6 phases: layout direction,
  sizing modes (HUG/FILL/FIXED), padding/spacing, alignment, nesting depth, and absolute positioning.

  Examples:
    /design:figma-autolayout-audit
    /design:figma-autolayout-audit https://figma.com/design/abc123/...
    /design:figma-autolayout-audit Check the card component layout
---

# Figma Auto Layout Audit

You have been invoked via the `/design:figma-autolayout-audit` command.

## Instructions

1. **Load project context** (optional): If `skills/design-system-toolkit/project/APP_PLAN.md` exists in this plugin directory, read it for project context.
2. **Load the audit skill**: Read `skills/design-system-toolkit/skills/05-figma-autolayout-audit.md` from this plugin directory.
3. **If the user provided a Figma URL or description** via `$ARGUMENTS`, start the audit immediately.
4. **If no arguments were provided**, ask the user to share a Figma URL or describe the frames they want audited.
5. **Do NOT run discovery** — this command skips project setup entirely.

## User's Request

$ARGUMENTS
