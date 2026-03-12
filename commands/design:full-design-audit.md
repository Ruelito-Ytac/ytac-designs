---
name: design:full-design-audit
description: >
  Run a complete UI/UX design audit. Executes the UX flow audit (7 phases) followed by
  the UI visual audit (11 layers) sequentially, then presents a combined severity summary.

  Examples:
    /design:full-design-audit
    /design:full-design-audit https://figma.com/design/abc123/...
    /design:full-design-audit Review the entire booking flow
---

# Full Design Audit (UX + UI)

You have been invoked via the `/design:full-design-audit` command.

## Instructions

1. **Load project context** (optional): If `skills/design-system-toolkit/project/APP_PLAN.md` exists in this plugin directory, read it for project context.
2. **Run UX audit first**: Read `skills/design-system-toolkit/skills/02-ux-flow-audit.md` and execute all 7 phases. Present the UX findings report.
3. **Write UX findings**: If APP_PLAN.md exists, write findings to its `Outstanding Issues` section. Otherwise keep findings in conversation context.
4. **Run UI audit second**: Read `skills/design-system-toolkit/skills/03-ui-visual-audit.md` and execute all 11 layers. The UI audit has access to the UX findings for cross-referencing.
5. **Present combined summary**: After both audits complete, present a combined severity summary counting all Critical, Major, Minor, and Suggestion findings across both audits.
6. **Both audits always run** — Critical UX issues do NOT block the UI audit.
7. **Do NOT run discovery** — this command skips project setup entirely.

## User's Request

$ARGUMENTS
