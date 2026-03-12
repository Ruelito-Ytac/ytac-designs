# Design System Governance & Initialization

> This module contains:
> 1. Design system initialization procedures (text styles, color system, spacing)
> 2. Gate check: no designing until styles exist in Figma
> 3. Component lifecycle management (Proposed → Draft → Beta → Stable → Deprecated)
> 4. Contribution process, deprecation process, versioning rules
>
> This file is loaded by:
> - The base `/design` command and `/design:screen` when design system needs setup
> - `/design:figma-build` when checking prerequisites
> - Any workflow that needs to verify or create Figma styles

---

## Step 3: Initialize Design System in Figma

**⚠️ MANDATORY: Do this BEFORE designing any screens.** Every text layer and color fill must reference a style — never raw values. This step creates the foundational text styles and color styles in Figma so that all subsequent design work is clean from the start.

### When to Run This Step

| Situation | Action |
|-----------|--------|
| New project, empty Figma file | Run full initialization (text styles + color styles) |
| Existing project, no text/color styles | Run full initialization |
| Existing project, partial styles | Audit what exists, fill gaps |
| Existing project, complete styles | Skip — proceed to Step 4 |

### Pre-Check: What's Already in Figma?

Before creating anything, check the current state of the Figma file:

1. **Check for existing text styles**: Open the Figma file → look at local styles (Text Styles panel). Are there named text styles?
2. **Check for existing color styles**: Look at local styles (Color Styles panel) or Variable collections. Are there named color styles/variables?
3. **Check for raw values**: Select text layers — do they reference a text style or are they raw font/size settings? Select colored elements — do fills reference a style/variable or are they hardcoded hex?

If the file has raw values everywhere, run the full initialization below.

### 3A: Generate the Type Scale

Using the project context from `project/APP_PLAN.md` (brand fonts, base text size, platform, density), generate a complete type scale and create it as **Figma Text Styles**.

**Type Scale Generation Rules:**

1. Start from the project's `base_text_size` (default: 16px)
2. Use the project's `brand_fonts.primary` for all styles (or secondary where specified)
3. Apply platform conventions:
   - **Mobile**: Body min 16px, heading sizes slightly smaller than desktop
   - **Web**: Can go larger on headings, same body minimum
4. Compute line heights: body = 1.5× font size, headings = 1.2–1.3× font size
5. Round all values to whole pixels

**Required Text Styles (minimum set):**

Create ALL of these as named Figma Text Styles:

```
Typography / Display          → [Font], 48px, Bold, line-height: 56px, letter-spacing: -0.5px
Typography / H1               → [Font], 32px, Bold, line-height: 40px, letter-spacing: -0.25px
Typography / H2               → [Font], 24px, Semibold, line-height: 32px
Typography / H3               → [Font], 20px, Semibold, line-height: 28px
Typography / H4               → [Font], 18px, Medium, line-height: 24px
Typography / Body Large        → [Font], 18px, Regular, line-height: 28px
Typography / Body              → [Font], 16px, Regular, line-height: 24px
Typography / Body Bold         → [Font], 16px, Semibold, line-height: 24px
Typography / Body Small        → [Font], 14px, Regular, line-height: 20px
Typography / Body Small Bold   → [Font], 14px, Semibold, line-height: 20px
Typography / Caption           → [Font], 12px, Regular, line-height: 16px
Typography / Caption Bold      → [Font], 12px, Medium, line-height: 16px
Typography / Overline          → [Font], 11px, Medium, line-height: 16px, letter-spacing: 0.5px, UPPERCASE
Typography / Button Large      → [Font], 16px, Semibold, line-height: 24px
Typography / Button            → [Font], 14px, Semibold, line-height: 20px
Typography / Button Small      → [Font], 12px, Medium, line-height: 16px
Typography / Label             → [Font], 14px, Medium, line-height: 20px
Typography / Helper            → [Font], 12px, Regular, line-height: 16px
```

**Adjustments based on project context:**

| Context | Adjustment |
|---------|-----------|
| `base_text_size: 14px` | Shift entire scale down by 2px (Body = 14px, Body Small = 12px, etc.) |
| `base_text_size: 18px` | Shift entire scale up by 2px |
| `density: Compact` | Tighten line heights by ~2px per level |
| `density: Spacious` | Loosen line heights by ~2px per level |
| `brand_fonts.secondary` exists | Apply secondary font to Body/Caption/Helper; keep primary for headings |
| `platform: Mobile` only | Skip Display size or reduce to 36px |

**How to create in Figma (using Figma MCP tools):**

1. Use `figma_execute` with `figma.createTextStyle()` to create each text style programmatically. See 04-figma-to-code.md → "Creating Text Styles via figma_execute" for the complete ready-to-run script that creates all 18 styles.
2. Replace `"Inter"` in the script with the project's actual brand font from APP_PLAN.md.
3. After creation, verify all 18 styles appear in Figma's local styles panel.

### 3B: Generate the Color System

Using the project context from `project/APP_PLAN.md` (primary color, secondary color, visual tone, dark mode setting), generate a complete color system and create it as **Figma Color Styles** or **Figma Color Variables**.

**Color System Generation Rules:**

1. Start from the project's `branding.primary_color` and `branding.secondary_color`
2. Generate tints and shades: 50, 100, 200, 300, 400, 500 (base), 600, 700, 800, 900, 950
3. Generate neutral grays from the project's visual tone (warm → warm grays, cool → cool grays)
4. Generate semantic colors: success (green), warning (amber), error (red), info (blue)
5. Map all colors to semantic tokens for Light mode
6. If `dark_mode: Yes`, generate the Dark mode mapping

**Required Color Styles/Variables (minimum set):**

Create ALL of these as named Figma Color Styles or Color Variables:

```
PRIMITIVE PALETTE (raw colors — designers reference these rarely, tokens reference them):

  Primary / 50              → Lightest tint of primary
  Primary / 100
  Primary / 200
  Primary / 300
  Primary / 400
  Primary / 500             → Base primary color (from project context)
  Primary / 600
  Primary / 700
  Primary / 800
  Primary / 900
  Primary / 950             → Darkest shade of primary

  Secondary / 50–950        → Same scale for secondary color

  Neutral / 0               → #FFFFFF (pure white)
  Neutral / 50              → Lightest gray
  Neutral / 100
  Neutral / 200
  Neutral / 300
  Neutral / 400
  Neutral / 500
  Neutral / 600
  Neutral / 700
  Neutral / 800
  Neutral / 900
  Neutral / 950             → Near black
  Neutral / 1000            → #000000 (pure black)

  Success / 50–900          → Green scale
  Warning / 50–900          → Amber scale
  Error / 50–900            → Red scale
  Info / 50–900             → Blue scale (skip if primary is already blue)

SEMANTIC TOKENS (what you actually apply to designs):

  Light Mode:
    Background / Primary     → Neutral/0 (#FFFFFF)
    Background / Secondary   → Neutral/50
    Background / Tertiary    → Neutral/100
    Background / Inverse     → Neutral/900

    Text / Primary           → Neutral/900
    Text / Secondary         → Neutral/600
    Text / Tertiary          → Neutral/400
    Text / Inverse           → Neutral/0
    Text / Link              → Primary/600
    Text / Error             → Error/600
    Text / Success           → Success/600

    Border / Default         → Neutral/200
    Border / Strong          → Neutral/300
    Border / Focus           → Primary/500

    Surface / Card           → Neutral/0
    Surface / Elevated       → Neutral/0 (with shadow)
    Surface / Overlay        → Neutral/900 at 50% opacity
    Surface / Disabled       → Neutral/100

    Action / Primary         → Primary/500
    Action / Primary Hover   → Primary/600
    Action / Primary Active  → Primary/700
    Action / Secondary       → Neutral/100
    Action / Secondary Hover → Neutral/200
    Action / Destructive     → Error/500
    Action / Destructive Hover → Error/600
    Action / Disabled        → Neutral/300

    Feedback / Success BG    → Success/50
    Feedback / Success Text  → Success/700
    Feedback / Warning BG    → Warning/50
    Feedback / Warning Text  → Warning/700
    Feedback / Error BG      → Error/50
    Feedback / Error Text    → Error/700
    Feedback / Info BG       → Info/50
    Feedback / Info Text     → Info/700

  Dark Mode (if applicable):
    Background / Primary     → Neutral/950
    Background / Secondary   → Neutral/900
    Background / Tertiary    → Neutral/800
    Background / Inverse     → Neutral/50

    Text / Primary           → Neutral/50
    Text / Secondary         → Neutral/400
    Text / Tertiary          → Neutral/500
    Text / Inverse           → Neutral/900
    Text / Link              → Primary/300
    Text / Error             → Error/400
    Text / Success           → Success/400

    Border / Default         → Neutral/700
    Border / Strong          → Neutral/600
    Border / Focus           → Primary/400

    Surface / Card           → Neutral/900
    Surface / Elevated       → Neutral/800
    Surface / Overlay        → Neutral/950 at 60% opacity
    Surface / Disabled       → Neutral/800

    Action / Primary         → Primary/400
    Action / Primary Hover   → Primary/300
    Action / Primary Active  → Primary/200
    (... same pattern, lighter values for dark mode)

    Feedback / Success BG    → Success/950
    Feedback / Success Text  → Success/300
    (... same pattern)
```

**How to implement in Figma:**

Option A — **Figma Variables** (preferred, supports mode switching):
1. Create "Primitives" collection: Use `figma_setup_design_tokens` with all palette colors from the table above. See 04-figma-to-code.md → "Creating Color Variables via MCP → Step 1" for the complete ready-to-run call.
2. Create "Semantic" collection with Light and Dark modes: Use `figma_setup_design_tokens` with all semantic tokens mapped to primitive values. See 04-figma-to-code.md → "Step 2" for the complete ready-to-run call with ~35 tokens.
3. This allows instant Light ↔ Dark theme switching in Figma by changing the variable mode on any frame.

Option B — **Figma Color Styles** (simpler, no mode switching):
1. Use `figma_execute` with `figma.createPaintStyle()` to create each color as a named Paint Style.
2. See 04-figma-to-code.md → "Step 3 (Alternative): Create Color Styles via figma_execute" for the complete ready-to-run script that creates ~70 styles (primitives + semantic tokens).
3. Name using the `Category / Token Name` convention from the table above.

**⚠️ Always replace the default hex values in the scripts with the actual colors from `project/APP_PLAN.md`.** The defaults are Tailwind-based starting points.

**After creation**, run the verification script from 04-figma-to-code.md → "Verifying Color Styles After Creation" to confirm all expected styles exist.

### 3C: Verify & Record

After creating styles in Figma:

1. **Verify text styles**: Confirm all 18 text styles appear in the local styles panel
2. **Verify color styles**: Confirm semantic tokens are available and correctly mapped
3. **Verify border radius tokens**: Confirm FLOAT variables exist: radius/none (0), radius/xs (2), radius/sm (4), radius/md (8), radius/lg (12), radius/xl (16), radius/2xl (24), radius/full (9999). See 01-guide §21.
4. **Verify icon set**: Confirm icon components are imported (minimum: search, close, back, check, error, info, warning, user, star, home, settings, notification, location). All icons must be 24×24 square frames with vector children. See 01-guide §20.
5. **Update APP_PLAN.md**: Record the design system initialization in the Design System Inventory section:

```yaml
# Add to project/APP_PLAN.md → Design System Inventory
Token Collections:
  - Typography: 18 text styles (Typography / [Name])
  - Primitives: [N] color variables (Primary, Secondary, Neutral, Success, Warning, Error, Info)
  - Semantic: [N] color variables with Light + Dark modes
  - Border Radius: 8 FLOAT variables (radius/none through radius/full)
  - Icon Set: [Library Name] — [N] icons imported as components (Icon / [Name])
  - Icon Sizes: 12/16/20/24/32/40/48 (24px default)
```

6. **Add to Decision Log**: Record the type scale, color, radius, and icon library decisions

### 3D: Gate Check — No Designing Without Styles

**🚫 DO NOT proceed to Step 4 (designing screens) until:**

- [ ] All text styles exist in Figma
- [ ] All semantic color tokens exist in Figma
- [ ] Border radius FLOAT variables exist in Figma
- [ ] Icon set imported as components (no text-character icons)
- [ ] APP_PLAN.md inventory is updated

**When designing screens after this step:**
- Every text layer MUST reference a Text Style — never set font/size manually
- Every color fill MUST reference a Color Style or Variable — never hardcode hex values
- Every border radius MUST reference a radius variable — never use raw numbers
- Every icon MUST be a component instance from the icon library — never use text characters (◉, ★, →)
- Icon frames MUST be square (24×24 default) — never non-square dimensions
- Every interactive element (button, input, card, chip, list item) MUST have non-zero padding — zero-padding is a 🔴 Critical bug
- All padding and gap values MUST be on the spacing token scale (2/4/8/12/16/20/24/32/48/64) — off-grid values are bugs
- Buttons, cards, text containers MUST use HUG height — fixed height on content elements is a 🔴 Critical bug
- Content areas (between header and footer) MUST use FILL height — HUG on content area causes footer to float
- Screen content frames MUST have ≥16px left and right padding — zero-margin screens are a 🔴 Critical bug
- If you need a style that doesn't exist, CREATE the style first, then apply it

---

## Design System Governance

As a design system grows, it needs rules for how it evolves — who decides what, how changes are made, and how to keep the system healthy over time.

### Ownership Model

| Role | Responsibility | Who |
|------|---------------|-----|
| **System Owner** | Final decisions on additions/removals, maintains quality bar | Lead designer or design system team |
| **Contributors** | Propose new components, report bugs, submit variants | Any designer or developer on the team |
| **Consumers** | Use the system as-is, request changes through proper channels | Product designers, developers |
| **Reviewers** | Review proposals for consistency, accessibility, feasibility | System owner + senior designer + lead dev |

### Contribution Model

External teams (product designers, developers) contribute to the design system through this process:

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  IDENTIFY    │ →  │  PROPOSE     │ →  │  REVIEW      │ →  │  INTEGRATE   │
│              │    │              │    │              │    │              │
│ Contributor  │    │ Submit       │    │ Reviewers    │    │ System Owner │
│ finds a gap  │    │ proposal     │    │ evaluate     │    │ merges into  │
│ or pattern   │    │ (template    │    │ (2-week      │    │ library      │
│ used 2+      │    │ below)       │    │ review       │    │ + publishes  │
│ times        │    │              │    │ cycle)       │    │              │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
```

**Review Cadence:**
- New proposals reviewed bi-weekly (or weekly during active development)
- Reviewers: System Owner + 1 senior designer + 1 lead developer (minimum)
- Decision within 2 review cycles (max 4 weeks)
- Contributor notified of decision with actionable feedback

**What Qualifies for Contribution:**
- Pattern used in 2+ products or screens
- Addresses a gap not covered by existing components
- Includes all required states (default, hover, active, focus, disabled, loading, error)
- Follows existing token architecture
- Meets accessibility requirements

### Component Lifecycle

Every component goes through these stages:

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│ PROPOSED  │ →  │ DRAFT    │ →  │ BETA     │ →  │ STABLE   │ →  │ DEPRECATED│
│           │    │          │    │          │    │          │    │           │
│ Idea or   │    │ Designed │    │ In use   │    │ Fully    │    │ Scheduled │
│ request   │    │ not yet  │    │ by 1-2   │    │ adopted, │    │ for       │
│ submitted │    │ in prod  │    │ teams    │    │ tested   │    │ removal   │
└──────────┘    └──────────┘    └──────────┘    └──────────┘    └──────────┘
```

**Stage Transitions:**
- Proposed → Draft: Approved by reviewers, designer assigned
- Draft → Beta: Design complete, all states defined, tokens used, accessibility verified
- Beta → Stable: Used by 2+ teams for 30+ days with no critical issues
- Stable → Deprecated: Replacement available, deprecation notice published

### Proposing a New Component

Before creating a new component, answer these:

| Question | Required Answer |
|----------|----------------|
| Does a similar component already exist? | No (or existing one doesn't cover the use case) |
| Will 2+ teams/screens use this? | Yes — don't systemize one-off patterns |
| Is the pattern stable? | Yes — it's been used successfully in at least one product |
| Are all states designed? | Default, hover, active, focus, disabled, loading, error |
| Is it accessible? | Meets WCAG AA, keyboard navigable, screen reader tested |
| Is it responsive? | Works across target breakpoints |
| Are tokens used? | All values reference design tokens (no hardcoded hex/px) |

### Proposal Template

```
## Component Proposal: [Name]

**Proposer**: [Name, Role]
**Date**: [Date]
**Status**: Proposed

### Problem
What user or team need does this solve?

### Use Cases
Where will this be used? (List 2+ screens/products)

### Existing Alternatives
Why can't an existing component cover this?

### Design Spec
[Link to Figma frame with all states]

### Accessibility
- Keyboard behavior: [describe]
- Screen reader: [describe ARIA approach]
- Contrast: [verified AA/AAA]

### Token Usage
List all tokens this component references.

### Decision
- [ ] Approved → Move to Draft
- [ ] Needs revision → [feedback]
- [ ] Rejected → [reason]
```

### Deprecating a Component

| Step | Action |
|------|--------|
| 1 | Mark as deprecated in Figma (add ⚠️ prefix, move to "Deprecated" page) |
| 2 | Announce to team with migration path ("Use [New Component] instead") |
| 3 | Add deprecation notice to component description in Figma |
| 4 | Set removal date (minimum 30 days from announcement) |
| 5 | After removal date: delete from library, clean up instances |

### Audit Schedule

| Frequency | What to Review |
|-----------|---------------|
| **Weekly** | New component requests, bug reports |
| **Monthly** | Token consistency, unused components, detached instances |
| **Quarterly** | Full visual audit (03-ui-visual-audit.md), accessibility audit, performance review |
| **Annually** | Design trend review, competitive benchmark, major version consideration |

---

## Design System Versioning & Changelog

When the design system changes, everyone needs to know what changed, why, and what to do about it.

### Versioning Scheme

Use **Semantic Versioning** adapted for design:

```
MAJOR.MINOR.PATCH

MAJOR (1.0 → 2.0): Breaking changes
  - Component removed or renamed
  - Token values changed significantly
  - Layout structure changed (affects all consumers)

MINOR (1.0 → 1.1): New features, backward-compatible
  - New component added
  - New variant added to existing component
  - New token added

PATCH (1.0.0 → 1.0.1): Fixes, no behavior change
  - Color value adjusted slightly
  - Spacing fix
  - Accessibility fix
  - Bug fix in component
```

### Changelog Template

Maintain a changelog page in your Figma library or documentation site:

```
# Design System Changelog

## v1.3.0 — 2026-03-01

### ✨ Added
- Button: New "ghost" variant for secondary actions
- Card: Added "compact" size variant
- New component: Tag/Chip (with removable option)

### 🔄 Changed
- color/primary: Updated from #2563EB to #2557E8 (slightly warmer)
- spacing/lg: Changed from 20px to 24px (aligns to 8px grid)

### 🐛 Fixed
- Input: Focus ring was 1px, now 2px (accessibility fix)
- Avatar: Image didn't cover full circle on Safari

### ⚠️ Deprecated
- OldButton: Use Button instead. Removal in v2.0.
- color/accent is now color/secondary. Old token will be removed in v2.0.

### 💥 Breaking (major versions only)
- Removed: LegacyCard component (replaced by Card)
- Renamed: all `bg-*` tokens to `surface-*`
```

### Communicating Changes

| Change Type | Communication Method |
|-------------|---------------------|
| Patch (fix) | Figma library update notification (automatic) |
| Minor (new feature) | Slack/Teams message + changelog entry |
| Major (breaking) | Team meeting or email + migration guide + 30-day notice |
| Deprecation | In-component warning + announcement + deadline |

### Migration Guide Template (for breaking changes)

```
## Migration Guide: v1.x → v2.0

### What Changed
[Brief description of breaking changes]

### Step-by-Step Migration

1. **Update Token References**
   - Old: `color/accent` → New: `color/secondary`
   - Old: `bg-card` → New: `surface/card`

2. **Replace Deprecated Components**
   - Old: `<LegacyCard>` → New: `<Card variant="default">`
   - Old: `<OldButton>` → New: `<Button variant="primary">`

3. **Test**
   - Run visual regression tests
   - Check all breakpoints
   - Verify dark mode
   - Run accessibility audit
```
