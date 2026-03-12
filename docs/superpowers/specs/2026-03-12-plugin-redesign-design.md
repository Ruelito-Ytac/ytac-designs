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
- `router.md` — lightweight intent-to-skill dispatcher (used ONLY by base `/design` command for interactive menu and free-text intent parsing; sub-commands bypass it entirely)
- `governance.md` — design system governance rules

Each `/design:*` sub-command routes directly to its handler instead of going through the full orchestrator.

### Technical Validation: Sub-Command Naming

Claude Code's plugin system uses the `plugin-name:skill-name` format for skill references (e.g., `superpowers:brainstorming`, `ruel-system-design-ui-ux-figma:design`). Sub-commands will be implemented as separate **command files** in `commands/` with the colon separator in the `name` frontmatter field.

- **Primary format:** `design:ux-audit` (matching the user's requested `/design:ux-audit` invocation)
- **Fallback format:** If Claude Code's command parser does not support colons in command names, use hyphens: `design-ux-audit` (invoked as `/design-ux-audit`)
- **Validation step:** During implementation, create one test command file with a colon name and verify it appears in Claude Code's skill list before creating all 6 sub-commands

### Router.md Role Definition

`router.md` is ONLY loaded by the base `/design` command. Its responsibilities:

1. **Interactive menu** — when `/design` is invoked with no arguments, present the menu of available workflows
2. **Free-text intent classification** — when `/design <some prompt>` is invoked, classify intent using keyword matching:
   - "audit flow/UX/journey" → load skill 02
   - "audit UI/visual/design" → load skill 03
   - "audit auto layout/autolayout" → load skill 05
   - "design/create/build screen" → load skill 01
   - "build in Figma/create in Figma" → load skill 04
   - "full audit/review everything" → skill 02 then skill 03
3. **Sub-commands bypass router.md entirely** — `/design:ux-audit` loads skill 02 directly without touching router.md

### Governance.md Contents

Extracted from `agents.md`, `governance.md` contains:

- **Design system initialization checklist** (current Step 3): text styles, color systems, spacing scales setup in Figma
- **Gate check**: no designing until text styles + color styles exist in Figma
- **Component lifecycle stages**: Proposed → Draft → Beta → Stable → Deprecated
- **Contribution process**: Identify → Propose → Review → Integrate
- **Version management**: semantic versioning rules (MAJOR/MINOR/PATCH)
- **Deprecation process**: 5-step with 30-day minimum notice
- **Audit schedule**: weekly/monthly/quarterly/annually cadence
- **Ownership model**: System Owner, Contributors, Consumers, Reviewers

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

6 questions asked sequentially, each waiting for the user's response before proceeding:

1. **Project name & description** — What's the project called and what does it do?
2. **Platform** — Mobile / Web / Cross-platform? (if mobile: iOS, Android, or both?)
3. **Target audience** — Who are the primary users?
4. **Brand colors & visual tone** — Primary/secondary colors + tone (Clean, Bold, Dark, Warm, Corporate, Playful)
5. **Navigation pattern** — Bottom tabs / Top tabs / Drawer / Sidebar? How many sections?
6. **Priority flows/screens** — What are the first 2-3 screens or flows to design?

### Deep Questions (On-Demand)

Asked one at a time, only when the current task needs that information:

| Trigger Context | Questions Asked |
|----------------|----------------|
| Building in Figma with existing icon assets | Figma icon page name (e.g., "UI Icons", "Assets/Icons") |
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
| `/design` | discovery.md → router.md → interactive menu | Yes (6 core Q's if no APP_PLAN) |
| `/design:screen` | discovery.md (if needed) → skill 01 | Yes if no APP_PLAN |
| `/design:ux-audit` | skill 02 directly | Optional (works without) |
| `/design:ui-audit` | skill 03 directly | Optional (works without) |
| `/design:full-design-audit` | skill 02 then skill 03 (sequential, see context transfer below) | Optional (works without) |
| `/design:figma-autolayout-audit` | skill 05 (NEW) | Optional |
| `/design:figma-build` | skill 04 (build mode) | Yes if no APP_PLAN |

### Command File Structure

Each command file is a markdown skill definition with:
- `name` — the command name (e.g., `design:ux-audit`)
- `description` — what it does (shown in Claude Code's skill list)
- Body — instructions to load APP_PLAN.md if it exists, then route to the target skill

Audit commands work without discovery — you can point them at any Figma URL and get immediate results. Design/build commands need project context first.

### Full Design Audit: Context Transfer Between Skills

When `/design:full-design-audit` runs skill 02 (UX) then skill 03 (UI) sequentially:

1. Skill 02 runs all 7 phases and produces its findings report
2. Findings are written to the `Outstanding Issues` section of APP_PLAN.md (if it exists) or kept in conversation context
3. Skill 03 then runs all 11 layers, with access to skill 02's findings for cross-referencing
4. The final output is **two separate reports presented sequentially** (UX findings first, then UI findings), with a combined severity summary at the end
5. Both skills always run regardless of severity — Critical UX issues do not block the UI audit, since the user requested a full review

### All commands accept `$ARGUMENTS`

Each sub-command accepts an optional prompt via `$ARGUMENTS` for direct context:
- `/design:ux-audit` → prompts for Figma URL
- `/design:ux-audit https://figma.com/design/...` → starts auditing immediately
- `/design:screen Login screen` → starts designing the login screen

## Feature 3: Figma Icon Page Integration

### New Field in APP_PLAN.md

```yaml
branding:
  icon_set: "Phosphor"          # RETAINED — icon library name (Phosphor, Lucide, etc.)
  icon_page_name: "UI Icons"    # NEW — name of Figma page containing project icons
```

Note: `icon_set` (icon library) is retained alongside `icon_page_name` (Figma page). They serve different purposes — `icon_set` identifies the library for consistency checks, `icon_page_name` tells Figma MCP where to find the actual components.

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

### Stricter Audit Checks (Added to Skills 02, 03) — Placement Map

New checks are inserted into specific phases/layers of the existing audit frameworks:

**Skill 02 (UX audit) — new checks by phase:**

| Phase | New Check |
|-------|-----------|
| Phase 2: Screen-by-Screen | Bottom sheets with 3+ content sections → Major severity |
| Phase 2: Screen-by-Screen | Screens showing all information at once without hierarchy → Major severity |
| Phase 7: Cognitive Load | No progressive disclosure pattern used → Minor severity |
| Phase 7: Cognitive Load | Flows that require user to process 4+ distinct data categories simultaneously → Major severity |

**Skill 03 (UI audit) — new checks by layer:**

| Layer | New Check |
|-------|-----------|
| Layer 1: Spacing & Alignment | Spacing less than 16px between distinct content sections → Critical severity |
| Layer 1: Spacing & Alignment | Content touching device edges (0px screen padding) → Critical severity |
| Layer 5: Icons & Imagery | Map marker/icon clustering without grouping within 40px → Major severity |
| Layer 5: Icons & Imagery | Cards with 4+ data points in compact layout → Major severity |
| Layer 6: Surfaces & Elevation | Hard borders (1px solid) used instead of subtle shadows → Minor severity |
| Layer 8: Modern Design Patterns | Corner radius under 8px on cards/buttons → Minor severity |
| Layer 8: Modern Design Patterns | No progressive disclosure pattern on data-dense screens → Major severity |

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

## Migration Notes

- The base `/design` command remains fully functional — existing users are not disrupted
- Existing APP_PLAN.md files are forward-compatible (they just won't have `icon_page_name` until the user provides it; the fallback prompt handles this)
- `agents.md` deletion happens as the LAST step after all three replacement files (`discovery.md`, `router.md`, `governance.md`) are verified working
- Skill 04 (Figma build) should recommend running `/design:figma-autolayout-audit` after completing a build session, since the two workflows are complementary

## Testing Plan

### Happy Path Tests

1. Run `/design` with no existing APP_PLAN.md — verify 6 questions asked one at a time
2. Run `/design` with existing APP_PLAN.md — verify discovery is skipped, routes to menu
3. Run each sub-command (`/design:ux-audit`, `/design:ui-audit`, etc.) — verify direct routing
4. Run `/design:screen` — verify it asks for project context if no APP_PLAN exists
5. Run `/design:ux-audit` on a Figma URL — verify it works without discovery
6. Create a user flow — verify icons are fetched from the configured icon page
7. Run `/design:ui-audit` on the Trikogo screenshot — verify anti-crowding rules trigger
8. Run `/design:figma-autolayout-audit` on a Figma frame — verify 6-phase inspection runs

### Edge Case & Error Tests

9. Run `/design:screen` with no Figma MCP connected — verify graceful error message
10. Run a flow that references `icon_page_name` but the page doesn't exist in the Figma file — verify it asks the user to correct it
11. Run `/design:full-design-audit` and interrupt mid-way through skill 02 — verify skill 03 does not run with partial context
12. Verify existing APP_PLAN.md files without `icon_page_name` trigger the fallback prompt when icons are needed
13. Verify sub-command naming works in Claude Code (colon format validation) — if not, switch to hyphen fallback
