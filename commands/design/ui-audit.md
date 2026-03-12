---
name: design:ui-audit
description: >
  Run a UI visual audit on a Figma design. Inspects 11 layers: spacing, typography,
  color/contrast, components, icons, elevation, responsive, modern patterns, dark mode,
  motion, and design system compliance.

  Examples:
    /design:ui-audit
    /design:ui-audit https://figma.com/design/abc123/...
    /design:ui-audit Check if the home screen looks modern
---

# UI Visual Audit

You have been invoked via the `/design:ui-audit` command.

## Instructions

1. **Load project context** (optional): If `skills/design-system-toolkit/project/APP_PLAN.md` exists in this plugin directory, read it for project context. This is NOT required — audits work without it.
2. **Load the audit skill**: Read `skills/design-system-toolkit/skills/03-ui-visual-audit.md` from this plugin directory.
3. **If the user provided a Figma URL or description** via `$ARGUMENTS`, start the audit immediately.
4. **If no arguments were provided**, ask the user to share a Figma URL or describe the screens they want audited.
5. **Apply project context** if APP_PLAN.md was loaded.
6. **Do NOT run discovery** — this command skips project setup entirely.

## User's Request

$ARGUMENTS
