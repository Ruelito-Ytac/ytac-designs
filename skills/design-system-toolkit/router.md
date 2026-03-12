# Router — Intent Classification & Menu

> This module is ONLY loaded by the base `/design` command.
> Sub-commands (`/design:ux-audit`, etc.) bypass this file entirely.
>
> Purpose:
> 1. Present an interactive menu when `/design` is invoked with no arguments
> 2. Classify free-text intent when `/design <some prompt>` is used

---

## When No Arguments Are Provided

Present this menu and ask the user to pick one:

> **What would you like to do?**
>
> 1. **Start a new design project** — Set up project context, colors, typography, and design system
> 2. **Design a screen or feature** — Create new screens using the design guide
> 3. **Audit a UX flow** — Validate a user journey (7-phase inspection)
> 4. **Audit UI visuals** — Check visual quality and consistency (11-layer inspection)
> 5. **Full design audit (UX + UI)** — Run both audits sequentially
> 6. **Audit Figma auto layout** — Check auto layout structure and sizing modes
> 7. **Build or fix in Figma** — Create or modify designs directly in Figma
> 8. **Create components** — Build reusable components with variants and properties
> 9. **Export to code** — Convert Figma designs to React/HTML/CSS/Tailwind

Wait for the user's choice, then route accordingly.

---

## When Arguments Are Provided

Classify the user's intent from `$ARGUMENTS` using keyword matching. Route to the first match:

| Keywords in User's Request | Route To |
|---------------------------|----------|
| "audit flow", "audit UX", "check flow", "review journey", "UX audit", "flow audit" | Load `skills/02-ux-flow-audit.md` |
| "audit UI", "audit visual", "check design", "visual audit", "look modern", "check screens" | Load `skills/03-ui-visual-audit.md` |
| "full audit", "review everything", "audit all", "complete review", "full design audit" | Load `skills/02-ux-flow-audit.md` THEN `skills/03-ui-visual-audit.md` |
| "audit auto layout", "audit autolayout", "check layout", "layout audit" | Load `skills/05-figma-autolayout-audit.md` |
| "design screen", "new screen", "design the", "design a", "plan screen", "screen design" | Load `skills/01-design-guide.md` (+ `skills/01-ref-design-patterns.md` if review/reference needed) |
| "build in Figma", "create in Figma", "build screen", "create screen", "add auto layout", "construct" | Load `skills/04a-figma-build-core.md` |
| "build component", "create component", "make reusable", "add variants", "component set" | Load `skills/04b-component-architect.md` |
| "fix Figma", "repair", "clean up", "audit file", "find issues" | Load `skills/04d-figma-repair.md` |
| "export", "convert to code", "to React", "to HTML", "generate code", "to CSS", "to Tailwind" | Load `skills/04c-code-export.md` |
| "set up styles", "create variables", "initialize design system", "add text styles" | Load `governance.md` → Run design system initialization |
| "start new project", "new project", "start from scratch" | Load `discovery.md` → Run full discovery |
| "update context", "change project", "switch project" | Load `discovery.md` → Re-run specific questions |

**If no keywords match:** Default to presenting the interactive menu above.

---

## Routing Rules

1. **Always load `project/APP_PLAN.md` first** (if it exists) before routing to any sub-skill
2. **For design/build routes**: Check if text styles + color styles exist in Figma. If not, run design system initialization from `governance.md` first.
3. **For audit routes**: No prerequisites — audits work standalone
4. **After routing**: Pass the user's original `$ARGUMENTS` to the loaded sub-skill as context
