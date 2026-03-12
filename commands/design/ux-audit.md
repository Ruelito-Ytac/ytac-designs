---
name: design:ux-audit
description: >
  Run a UX flow audit on a Figma design. Validates user journeys using a 7-phase
  inspection framework covering flow structure, screen clarity, mobile usability,
  interaction feedback, edge cases, accessibility, and cognitive load.

  Examples:
    /design:ux-audit
    /design:ux-audit https://figma.com/design/abc123/...
    /design:ux-audit Check the checkout flow
---

# UX Flow Audit

You have been invoked via the `/design:ux-audit` command.

## Instructions

1. **Load project context** (optional): If `skills/design-system-toolkit/project/APP_PLAN.md` exists in this plugin directory, read it for project context. This is NOT required — audits work without it.
2. **Load the audit skill**: Read `skills/design-system-toolkit/skills/02-ux-flow-audit.md` from this plugin directory.
3. **If the user provided a Figma URL or description** via `$ARGUMENTS`, start the audit immediately using that context.
4. **If no arguments were provided**, ask the user to share a Figma URL or describe the flow they want audited.
5. **Apply project context** if APP_PLAN.md was loaded — use it to customize severity ratings and platform-specific checks per the context application rules in the audit skill.
6. **Do NOT run discovery** — this command skips project setup entirely.

## User's Request

$ARGUMENTS
