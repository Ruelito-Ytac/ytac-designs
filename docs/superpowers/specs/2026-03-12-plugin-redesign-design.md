# Plugin Redesign: Sub-Commands, Progressive Discovery, Modern Design Rules

**Date:** 2026-03-12
**Status:** Approved

## Problem Statement

The ytac-designs plugin has four issues:

1. **Bulk questions** — Starting a new project asks 23 questions grouped into 7 categories all at once, overwhelming the user
2. **No sub-commands** — `/design` is the only command; users must describe intent in prose rather than invoking specific workflows directly
3. **Figma icons not used in flows** — User flow creation doesn't reference the project's actual icon components from their Figma file
4. **Crowded/dated design output** — The design guide and audit skills don't enforce modern spacing, density limits, or contemporary mobile patterns (as demonstrated by the Trikogo app home screen)

## Design Decisions

### Architecture: Modular Refactor (Approach B)

Split the monolithic `agents.md` (991 lines) into focused modules:

- `discovery.md` — progressive question flow
- `router.md` — lightweight intent-to-skill dispatcher
- `governance.md` — design system governance rules

Each `/design:*` sub-command routes directly to its handler instead of going through the full orchestrator.

### File Structure (After)

```
commands/
  design.md                          — base /design (menu + new project)
  design:ux-audit.md                 — routes to skill 02
  design:ui-audit.md                 — routes to skill 03
  design:full-design-audit.md        — routes to skill 02 then skill 03
  design:figma-autolayout-audit.md   — routes to skill 05 (NEW)
  design:screen.md                   — routes to skill 01
  design:figma-build.md              — routes to skill 04 (build mode)

skills/design-system-toolkit/
  SKILL.md                           — entry point (updated routing)
  discovery.md                       — progressive discovery (NEW, extracted from agents.md)
  router.md                          — lightweight dispatcher (NEW, extracted from agents.md)
  governance.md                      — design system governance (NEW, extracted from agents.md)

  project/
    APP_PLAN.md                      — updated: adds icon_page_name field

  skills/
    01-mobile-web-ui-ux-design-guide.md  — updated: modern spacing + icon page ref
    02-ux-flow-audit.md                  — updated: stricter modern checks
    03-ui-visual-audit.md                — updated: stricter modern checks
    04-figma-to-code.md                  — unchanged
    05-figma-autolayout-audit.md         — NEW: dedicated auto layout audit
```

`agents.md` is deleted after its content is distributed to `discovery.md`, `router.md`, and `governance.md`.

## Feature 1: Progressive Discovery

### Core Questions (Always Asked, One at a Time)

7 questions asked sequentially, each waiting for the user's response before proceeding:

1. **Project name & description** — What's the project called and what does it do?
2. **Platform** — Mobile / Web / Cross-platform? (if mobile: iOS, Android, or both?)
3. **Target audience** — Who are the primary users?
4. **Brand colors & visual tone** — Primary/secondary colors + tone (Clean, Bold, Dark, Warm, Corporate, Playful)
5. **Navigation pattern** — Bottom tabs / Top tabs / Drawer / Sidebar? How many sections?
6. **Priority flows/screens** — What are the first 2-3 screens or flows to design?
7. **Figma icon page name** — Which Figma page contains your icons? (e.g., "UI Icons", "Assets/Icons")

### Deep Questions (On-Demand)

Asked one at a time, only when the current task needs that information:

| Trigger Context | Questions Asked |
|----------------|----------------|
| Designing text-heavy screens | Brand fonts (primary & secondary), base text size (14/16/18px) |
| Setting up design system | Corner radius style, spacing density, existing design system status |
| Building responsive layouts | Target screen sizes, Figma file structure (single/multi) |
| Scoping special needs | Accessibility requirements, dark mode, i18n, offline, user personas |

### Rules

- Already-known answers from APP_PLAN.md are always skipped
- Each question is its own message — never combine questions
- Multiple choice preferred when possible
- After core questions, generate APP_PLAN.md and route to the appropriate sub-skill

## Feature 2: Sub-Command Routing

### Command Map

| Command | Routes To | Requires Discovery? |
|---------|-----------|---------------------|
| `/design` | discovery.md → router.md → interactive menu | Yes (7 core Q's if no APP_PLAN) |
| `/design:screen` | discovery.md (if needed) → skill 01 | Yes if no APP_PLAN |
| `/design:ux-audit` | skill 02 directly | Optional (works without) |
| `/design:ui-audit` | skill 03 directly | Optional (works without) |
| `/design:full-design-audit` | skill 02 then skill 03 (sequential) | Optional (works without) |
| `/design:figma-autolayout-audit` | skill 05 (NEW) | Optional |
| `/design:figma-build` | skill 04 (build mode) | Yes if no APP_PLAN |

### Command File Structure

Each command file is a markdown skill definition with:
- `name` — the command name (e.g., `design:ux-audit`)
- `description` — what it does (shown in Claude Code's skill list)
- Body — instructions to load APP_PLAN.md if it exists, then route to the target skill

Audit commands work without discovery — you can point them at any Figma URL and get immediate results. Design/build commands need project context first.

### All commands accept `$ARGUMENTS`

Each sub-command accepts an optional prompt via `$ARGUMENTS` for direct context:
- `/design:ux-audit` → prompts for Figma URL
- `/design:ux-audit https://figma.com/design/...` → starts auditing immediately
- `/design:screen Login screen` → starts designing the login screen

## Feature 3: Figma Icon Page Integration

### New Field in APP_PLAN.md

```yaml
branding:
  icon_page_name: "UI Icons"    # NEW — name of Figma page containing project icons
```

### Icon Lookup Process (5 Steps)

When placing any icon in a flow diagram or screen design:

1. Read `icon_page_name` from APP_PLAN.md
2. Use Figma MCP `get_design_context` to fetch available icons from that page
3. Match needed icons to available icons by name/purpose
4. Use the project's actual icon components — never substitute generic/emoji icons
5. If an icon doesn't exist in the page, flag it as missing and suggest adding it

### Fallback

If no `icon_page_name` is set in APP_PLAN.md, ask the user which page contains their icons before proceeding. Store the answer in APP_PLAN.md for future use.

### Updated Skills

- **Skill 01** (design guide) — Icon placement in screen designs must use the lookup process
- **Skill 04** (Figma build) — Component instances must reference the icon page when building

## Feature 4: Modern Design & Anti-Crowding Rules

### Anti-Crowding Rules (Added to Skills 01, 02, 03)

These rules are added as Critical/Major severity checks:

| Rule | Severity | Description |
|------|----------|-------------|
| Bottom sheet density | Critical | Max 2 content sections in a bottom sheet. If more needed, use scrollable sheet or separate views. |
| Card information density | Major | Max 3 data points per compact card. Prioritize essential info, hide details until drill-down. |
| Map marker clustering | Major | When markers overlap or cluster within 40px, show cluster indicator with count instead of individual markers. |
| Section spacing | Critical | Minimum 24px between distinct content sections. Minimum 16px between items within a section. No content touching device edges — 16px minimum screen padding. |

### Modern Mobile Patterns 2024-2026 (Added to Skill 01)

| Pattern | Guidance |
|---------|----------|
| Visual breathing room | Generous whitespace between sections. Content cards with 16px internal padding minimum. Headers with clear separation from content below. |
| Progressive disclosure | Show essential info upfront, details on tap. Expandable cards, drill-down views, peek-and-reveal. Don't show everything at once. |
| Subtle elevation & depth | Soft shadows (`0 2px 8px rgba(0,0,0,0.08)`) instead of hard borders. Layered surfaces with slight elevation differences. Glass/blur effects for overlays. |
| Typography hierarchy | Clear size jumps between heading/body/caption (e.g., 24/16/12). Use font weight contrast (Bold title, Regular body, Medium label) instead of size alone. |
| Rounded, soft UI | Corner radius 12-16px for cards, 8-12px for buttons, 20-24px for chips/tags. Sharp corners (0-4px) look dated. |
| Color restraint | 1 primary action color, 1 secondary. Neutral backgrounds. Color for meaning (status, actions), not decoration. |

### Stricter Audit Checks (Added to Skills 02, 03)

Skills 02 and 03 get new checks that flag:

- **Skill 02 (UX audit):** Bottom sheets with 3+ content sections, screens with no progressive disclosure, flows that show all information at once without hierarchy
- **Skill 03 (UI audit):** Cards with 4+ data points, spacing less than 16px between sections, marker/icon clustering without grouping, corners under 8px radius, hard borders instead of shadows

## Feature 5: Figma Auto Layout Audit (New Skill 05)

### 6-Phase Inspection Framework

| Phase | What It Checks |
|-------|---------------|
| **1. Layout Direction & Wrapping** | Is auto layout H or V? Does it match content flow? Are wrapping rules correct? Flag: horizontal layout for vertical stacks, missing wrap on chip groups. |
| **2. Sizing Modes** | HUG vs FILL vs FIXED correctness. Critical: content area HUG instead of FILL (footer floats), fixed width on stretchable elements. Major: FILL parent with all FIXED children. |
| **3. Padding & Spacing** | 8px grid compliance (8, 12, 16, 20, 24, 32, 40, 48). Flag: zero padding on interactive elements, inconsistent gaps, mixed spacing in same component type. |
| **4. Alignment & Distribution** | Primary and counter alignment correctness. Flag: centered in left-aligned containers, space-between with 1 child, mismatched alignment across siblings. |
| **5. Nesting Depth** | Flag: nesting deeper than 5 levels, unnecessary single-child wrappers, frames without auto layout that should have it. |
| **6. Absolute Positioning** | Flag: absolute-positioned elements that could be in flow, overlapping elements that break on resize, absolute elements without constraints. |

### Severity Ratings

- **Critical** — Layout breaks on different screen sizes
- **Major** — Wasted space, wrong sizing mode
- **Minor** — Off-grid spacing, unnecessary nesting
- **Suggestion** — Could simplify structure

### Output Format

Same structured report as skills 02 and 03: findings grouped by phase, severity-tagged, with specific fix recommendations and Figma property values to change.

## Testing Plan

1. Run `/design` with no existing APP_PLAN.md — verify 7 questions asked one at a time
2. Run `/design` with existing APP_PLAN.md — verify discovery is skipped, routes to menu
3. Run each sub-command (`/design:ux-audit`, `/design:ui-audit`, etc.) — verify direct routing
4. Run `/design:screen` — verify it asks for project context if no APP_PLAN exists
5. Run `/design:ux-audit` on a Figma URL — verify it works without discovery
6. Create a user flow — verify icons are fetched from the configured icon page
7. Run `/design:ui-audit` on the Trikogo screenshot — verify anti-crowding rules trigger
8. Run `/design:figma-autolayout-audit` on a Figma frame — verify 6-phase inspection runs
