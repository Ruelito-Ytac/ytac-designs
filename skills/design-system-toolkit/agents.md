# Design System Toolkit — Orchestrator

> This file is the brain of the toolkit. It handles project discovery, routing decisions,
> context application, governance, and process. It does NOT contain design guidance — that
> lives in the sub-skills (`01`, `02`, `03`).

---

## How This Toolkit Works

```
┌───────────────────────────────────────────────────────┐
│              agents.md (You are here)                  │
│          Orchestrator & Decision Engine                 │
│                                                        │
│  Step 1: Run Project Discovery (below)                 │
│  Step 2: Generate project/APP_PLAN.md (session state)  │
│  Step 3: Initialize Design System in Figma             │
│          (text styles + color styles BEFORE screens)   │
│  Step 4: Route to the correct sub-skill:               │
│                                                        │
│  ┌─────────────────────────────────────────────────┐   │
│  │  📐 01-mobile-web-ui-ux-design-guide.md         │   │
│  │  Design principles, patterns, Figma workflow     │   │
│  │  USE WHEN: Designing new screens or components   │   │
│  └─────────────────────────────────────────────────┘   │
│                                                        │
│  ┌─────────────────────────────────────────────────┐   │
│  │  🔍 02-ux-flow-audit.md                         │   │
│  │  User journey validation & fix recommendations   │   │
│  │  USE WHEN: Reviewing if a flow makes sense       │   │
│  └─────────────────────────────────────────────────┘   │
│                                                        │
│  ┌─────────────────────────────────────────────────┐   │
│  │  🎨 03-ui-visual-audit.md                       │   │
│  │  Visual QA, consistency, modern design check     │   │
│  │  USE WHEN: Checking if screens look right        │   │
│  └─────────────────────────────────────────────────┘   │
│                                                        │
│  ┌─────────────────────────────────────────────────┐   │
│  │  🔧 04-figma-to-code.md                         │   │
│  │  Figma production & design-to-code export        │   │
│  │  USE WHEN: Building in Figma or exporting code   │   │
│  └─────────────────────────────────────────────────┘   │
│                                                        │
│  All sub-skills read project/APP_PLAN.md for context   │
└───────────────────────────────────────────────────────┘
```

### Sub-Skill Files (in the same folder)

| File | Purpose | When to Load |
|------|---------|-------------|
| `skills/01-mobile-web-ui-ux-design-guide.md` | Design principles, layout, typography, color, navigation, forms, accessibility, Figma workflow | Designing new screens, components, or layouts |
| `skills/02-ux-flow-audit.md` | 7-phase UX flow validation with checklists and fix templates | Reviewing a user journey or multi-screen flow |
| `skills/03-ui-visual-audit.md` | 11-layer visual inspection with defect catalogs and fix templates | Checking if UI is consistent, modern, and polished |
| `skills/04-figma-to-code.md` | Figma production (auto layout, components, styles, variables) and design-to-code export (React, HTML/CSS, Tailwind, Vue) | Building in Figma, fixing Figma files, converting designs to code |

### Loading Rules

- **Always read `project/APP_PLAN.md` first** (if it exists) — it has the project context you need
- **Starting from scratch?** → Discovery → APP_PLAN → **Initialize Design System (Step 3)** → then design screens
- **Design system already set up?** → Check that Figma has text styles + color styles before designing. If not, run Step 3 first.
- **Load sub-skills on demand** — only when the task requires them
- **Designing something new?** → Load `skills/01-mobile-web-ui-ux-design-guide.md`
- **Reviewing a flow?** → Load `skills/02-ux-flow-audit.md`
- **Checking visual quality?** → Load `skills/03-ui-visual-audit.md`
- **Building or fixing in Figma?** → Load `skills/04-figma-to-code.md` (Build Mode)
- **Converting designs to code?** → Load `skills/04-figma-to-code.md` (Export Mode)
- **Full design review?** → Load `02` then `03` in sequence

---

## Step 1: Project Discovery

Before any design work, establish the project context. Ask these questions if any are unknown.
Once answered, generate `project/APP_PLAN.md` as the living session state document.

### Discovery Questions

Ask the user these questions. Group related ones together. Skip any that are already known from memory or conversation.

---

### 1. Project Identity

```
Q1: What is the project name?
    → [Free text]

Q2: Brief description — what does this app/site do?
    → [Free text — 1-2 sentences]

Q3: Who is the target audience?
    → [Free text — e.g., "young professionals 25-35", "small business owners", "gamers"]

Q4: What is the project stage?
    → [ ] Brand new — starting from scratch
    → [ ] Existing project — redesigning or adding features
    → [ ] Design system exists — extending or auditing it
```

---

### 2. Platform & Device

```
Q5: What platform(s) are you designing for?
    → [ ] Mobile only (iOS / Android / Both)
    → [ ] Web only (Desktop / Responsive)
    → [ ] Cross-platform (Mobile + Web)
    → [ ] Other (Tablet app, Watch, TV, etc.)

Q6: If mobile — which platform takes priority?
    → [ ] iOS first (Apple HIG conventions)
    → [ ] Android first (Material Design conventions)
    → [ ] Both equally (neutral pattern, no platform-specific conventions)
    → [ ] N/A — web only

Q7: What are the target screen sizes?
    → Mobile:  [ ] 375px (iPhone SE)  [ ] 393px (iPhone 14/15)  [ ] 430px (iPhone Plus/Max)
    → Tablet:  [ ] 768px (iPad Mini)  [ ] 820px (iPad Air)  [ ] 1024px (iPad Pro)
    → Desktop: [ ] 1280px (Laptop)    [ ] 1440px (Standard)     [ ] 1920px (Large)
    → Or specify: ___
```

---

### 3. Branding & Visual Identity

```
Q8: What is the primary brand color?
    → Hex value: #______
    → Or describe: [e.g., "deep blue", "coral orange", "forest green"]

Q9: What is the secondary/accent color? (if any)
    → Hex value: #______
    → Or: [ ] Derive from primary (auto-generate complementary)
    → Or: [ ] Not decided yet

Q10: What is the overall visual tone?
    → [ ] Clean & Minimal (lots of white space, subtle colors)
    → [ ] Bold & Vibrant (strong colors, high contrast, energetic)
    → [ ] Dark & Sleek (dark surfaces, glowing accents, premium feel)
    → [ ] Warm & Friendly (rounded shapes, soft colors, approachable)
    → [ ] Corporate & Professional (structured, neutral tones, formal)
    → [ ] Playful & Fun (bright colors, illustrations, casual)
    → [ ] Other: ___

Q11: Do you have existing brand assets?
    → [ ] Logo (provide or describe)
    → [ ] Brand fonts (specify: ___)
    → [ ] Brand guidelines document
    → [ ] Icon set preference (e.g., Phosphor, Lucide, Material, SF Symbols, custom)
    → [ ] None — define from scratch

Q12: Is dark mode required?
    → [ ] Yes — design both light and dark
    → [ ] No — light mode only
    → [ ] Later — plan for it but don't design now
```

---

### 4. Design System & Figma Setup

```
Q13: Is there an existing design system or component library?
    → [ ] Yes — Figma library exists (provide link or name)
    → [ ] Partial — some components exist but incomplete
    → [ ] No — building from scratch
    → [ ] Using a public system (e.g., Material, shadcn, Ant Design, etc.)

Q14: What is the Figma file structure?
    → [ ] Single file (small project)
    → [ ] Multi-file (design system library + product files)
    → [ ] Not set up yet — help me organize it

Q15: What is the corner radius style?
    → [ ] Sharp (0–4px) — technical, data-heavy apps
    → [ ] Moderate (8–12px) — most modern apps
    → [ ] Rounded (16–24px) — friendly, consumer apps
    → [ ] Fully rounded (pill/capsule buttons) — playful, bold
    → [ ] Mixed/undecided — recommend for me

Q16: What is the spacing density?
    → [ ] Compact — data-dense dashboards, tables, enterprise tools
    → [ ] Comfortable — standard consumer apps (recommended default)
    → [ ] Spacious — marketing sites, editorial, luxury brands
```

---

### 5. Typography

```
Q17: What font(s) are you using?
    → Primary font: ___ (e.g., Inter, SF Pro, Roboto, Plus Jakarta Sans)
    → Secondary font: ___ (optional — for headings or display)
    → [ ] Not decided — recommend for me based on tone

Q18: What is the base body text size?
    → [ ] 14px (compact, desktop-focused — NOT for mobile body text)
    → [ ] 16px (standard, recommended for mobile and general use)
    → [ ] 18px (spacious, editorial, reading-heavy apps)
```

---

### 6. Navigation Pattern

```
Q19: How many primary sections/tabs does the app have?
    → [ ] 2–3 sections
    → [ ] 4–5 sections
    → [ ] 6+ sections
    → [ ] Single flow (no persistent navigation needed)

Q20: Preferred navigation pattern? (or let platform convention decide)
    → Mobile:
      [ ] Bottom tab bar (recommended for 3–5 sections)
      [ ] Top tabs (for peer categories within a section)
      [ ] Drawer / hamburger (for 6+ sections or secondary nav)
      [ ] Let the toolkit recommend based on section count
    → Desktop:
      [ ] Top navigation bar
      [ ] Sidebar navigation (collapsible)
      [ ] Both (sidebar + top bar)
      [ ] Let the toolkit recommend
```

---

### 7. Special Requirements

```
Q21: Are there any specific requirements?
    → [ ] Accessibility (WCAG AA compliance required)
    → [ ] Internationalization (RTL support, translation-ready)
    → [ ] Offline support (needs offline states and handling)
    → [ ] Multi-theme (beyond light/dark — e.g., high contrast, custom themes)
    → [ ] Animation-heavy (microinteractions, transitions are a priority)
    → [ ] Data-heavy (tables, charts, dashboards, analytics)
    → [ ] E-commerce (cart, checkout, payment flows)
    → [ ] Social/community (feeds, profiles, messaging)
    → [ ] Admin/back-office (CRUD operations, settings, management)
    → [ ] None — standard app

Q22: What user personas should be considered?
    → [Free text — list personas with names and roles if applicable]
    → Example: "Buyer = Darrell, Seller = Fernando, Admin = Jane"

Q23: Any specific flows or screens that are the priority?
    → [Free text — e.g., "onboarding flow", "checkout", "dispute management"]
```

---

## Step 2: Generate APP_PLAN.md

After collecting discovery answers, **create or update `project/APP_PLAN.md`** in the same directory.
APP_PLAN.md is the load-bearing session state document. See the APP_PLAN.md template for the full structure.

Key sections to populate:

1. **Project Context** — YAML block compiled from discovery answers
2. **Design Principles** — 3-5 core values guiding design decisions (derived from tone + audience + requirements)
3. **Current Focus** — What the user is working on right now
4. **Decision Log** — Design decisions made during the session
5. **Completed Work** — What's been designed, audited, or shipped
6. **Outstanding Issues** — Open questions, known problems, next steps

### When to Update APP_PLAN.md

| Trigger | What to Update |
|---------|---------------|
| Discovery answers change | `project_context` YAML block |
| User makes a design decision | Add to `decision_log` |
| A screen/flow is completed | Add to `completed_work` |
| Audit finds issues | Add to `outstanding_issues` |
| User starts a new task | Update `current_focus` |
| Design system version bumps | Update `system_version` and `changelog` |

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

## Step 4: Route to the Right Sub-Skill

Based on what the user needs, load the appropriate sub-skill file. Always load `project/APP_PLAN.md` first for context.

### Routing Decision Tree

```
User wants to...

├─ DESIGN something new (screens, components, layout)
│  └─ READ: project/APP_PLAN.md (for context)
│     CHECK: Do text styles + color styles exist in Figma?
│       → NO: Run Step 3 (Initialize Design System) first
│       → YES: Proceed
│     READ: skills/01-mobile-web-ui-ux-design-guide.md
│     Apply Project Context to customize guidance.
│     ENFORCE: Every text layer uses a Text Style. Every fill uses a Color Style/Variable.
│
├─ REVIEW a user flow (journey, multi-screen sequence)
│  └─ READ: project/APP_PLAN.md (for context)
│     READ: skills/02-ux-flow-audit.md
│     Apply Project Context to customize the audit.
│
├─ CHECK visual quality (UI review, consistency, polish)
│  └─ READ: project/APP_PLAN.md (for context)
│     READ: skills/03-ui-visual-audit.md
│     Apply Project Context to customize the audit.
│
├─ DO A FULL REVIEW (flow + visual + data flow)
│  └─ READ: project/APP_PLAN.md (for context)
│     READ: skills/02-ux-flow-audit.md FIRST (includes Phase 8: Data Flow)
│     Then READ: skills/03-ui-visual-audit.md
│     Then READ: skills/04-figma-to-code.md (Phase 9 Data Flow Detection Scripts) for Figma-specific checks.
│     Compile findings from all into a combined report.
│     UPDATE: project/APP_PLAN.md with findings.
│
├─ START A NEW PROJECT from scratch
│  └─ Run Project Discovery (Step 1)
│     CREATE: project/APP_PLAN.md (Step 2)
│     INITIALIZE: Design system in Figma — text styles + color styles (Step 3)
│     Then READ: skills/01-mobile-web-ui-ux-design-guide.md
│     Design the screens (all using styles, never raw values).
│     Then READ: skills/02-ux-flow-audit.md + skills/03-ui-visual-audit.md to validate.
│     Run Phase 8 (Data Flow Audit) to verify all screens have data states designed.
│
├─ SET UP DESIGN SYSTEM / "create styles" / "add text styles" / "set up colors"
│  └─ READ: project/APP_PLAN.md (for context)
│     Run Step 3 (Initialize Design System in Figma)
│     READ: skills/04-figma-to-code.md for Figma execution procedures.
│     Skip to screen design when complete.
│
├─ BUILD IN FIGMA / "create this screen in Figma" / "build components"
│  └─ READ: project/APP_PLAN.md (for context)
│     CHECK: Do text styles + color styles exist?
│       → NO: Run Step 3 first
│       → YES: Proceed
│     READ: skills/04-figma-to-code.md (Build Mode)
│     Use Figma MCP tools to construct directly in Figma.
│
├─ DOCUMENT USER FLOWS / "create flow diagram" / "map the flow" / "add arrows"
│  └─ READ: project/APP_PLAN.md (for context)
│     READ: skills/01-mobile-web-ui-ux-design-guide.md §2 (User Flow Design)
│     READ: skills/04-figma-to-code.md §7 (Flow Connectors & Annotations)
│     Create flow diagrams with proper connectors, color coding, and labeling.
│     Apply screen numbering convention (FLOW-##-State).
│     Add status badges to every screen and flow section.
│
├─ ADD ANNOTATIONS / "annotate for dev" / "add comments" / "mark ready for dev"
│  └─ READ: project/APP_PLAN.md (for context)
│     READ: skills/01-mobile-web-ui-ux-design-guide.md §2B (Annotations & Comments)
│     READ: skills/04-figma-to-code.md §7 (Flow Connectors & Annotations)
│     Add native annotations (Shift+T), set component descriptions, post comments.
│     Run annotation checklist before marking 🟢 Ready for Dev.
│
├─ AUDIT DATA FLOW / "check data flow" / "verify data states" / "review API states" / "check loading states"
│  └─ READ: project/APP_PLAN.md (for context)
│     READ: skills/01-mobile-web-ui-ux-design-guide.md §22 (Data Flow Design)
│     READ: skills/02-ux-flow-audit.md §15 (Phase 8: Data Flow Audit)
│     READ: skills/04-figma-to-code.md (Phase 9 in File Audit + Data Flow Detection Scripts)
│     Run data state coverage detection script in Figma to find missing states.
│     Run data annotation detection script to find undocumented screens.
│     Check screen-to-screen data passing patterns.
│     Verify multi-step forms preserve data on back navigation.
│     Grade data flow coverage using the scoring rubric from 02-ux-flow-audit §15.
│
├─ CONVERT TO CODE / "export to React" / "generate HTML" / "Figma to code"
│  └─ READ: project/APP_PLAN.md (for context)
│     READ: skills/04-figma-to-code.md (Export Mode)
│     Extract design data from Figma → generate code.
│
├─ FIX FIGMA FILE / "add auto layout" / "fix my file" / "clean up layers"
│  └─ READ: project/APP_PLAN.md (for context)
│     READ: skills/04-figma-to-code.md (Repair & Cleanup)
│     Audit and fix the Figma file structure.
│     Run Phase 3B (Spacing & Sizing Audit) to catch zero-padding and wrong sizing modes.
│
├─ AUDIT SPACING / "check spacing" / "fix padding" / "spacing audit" / "why is my UI broken"
│  └─ READ: project/APP_PLAN.md (for context)
│     READ: skills/01-mobile-web-ui-ux-design-guide.md §23 (Spacing & Sizing Enforcement)
│     READ: skills/03-ui-visual-audit.md Layer 1 (Spacing & Alignment)
│     READ: skills/04-figma-to-code.md (Phase 3B + spacing detection/fix scripts)
│     Run spacing detection script to find: zero-padding, off-grid values, wrong sizing modes.
│     Fix using bulk fix scripts or manual correction.
│     Verify: no 🔴 Critical issues remain (zero padding, wrong sizing, zero screen margins).
│
└─ UNKNOWN / GENERAL design question
   └─ READ: project/APP_PLAN.md (if exists, for context)
      READ: skills/01-mobile-web-ui-ux-design-guide.md
      Use relevant section to answer the question.
```

---

## Step 5: Apply Project Context to Sub-Skills

When reading any sub-skill, use the Project Context from `project/APP_PLAN.md` to customize the guidance.

**Pre-check before any design work:** Verify that text styles and color styles exist in Figma. If they don't, run Step 3 first. Never design with raw values.

### Context Application Rules

#### For `skills/01-mobile-web-ui-ux-design-guide.md`:

| Project Context | How It Affects Design Guidance |
|----------------|-------------------------------|
| **Platform = Mobile** | Prioritize: thumb zone, bottom nav, 44px touch targets, 16px body text, single column, safe areas |
| **Platform = Web** | Prioritize: top/sidebar nav, hover states, keyboard shortcuts, 12-column grid, max-width containers |
| **Platform = Cross** | Design mobile first, then adapt for desktop. Shared component library with responsive variants |
| **Tone = Clean & Minimal** | More whitespace, subtle shadows, muted accent usage, thin borders, lots of breathing room |
| **Tone = Bold & Vibrant** | Larger headings, stronger accent presence, bolder shadows, less white space, more color blocks |
| **Tone = Dark & Sleek** | Dark mode as primary, glowing accents, subtle borders, no shadows (use surface layers), premium spacing |
| **Tone = Warm & Friendly** | Larger radii (16-24px), warmer neutral tones, softer shadows, illustration-friendly |
| **Tone = Corporate** | Smaller radii (4-8px), neutral grays, structured grids, data-dense layouts allowed |
| **Tone = Playful** | Pill-shaped buttons, bright accent pops, bouncy microinteractions, illustration-heavy |
| **Density = Compact** | 4-8px gaps, smaller components, denser grids, more info per screen |
| **Density = Comfortable** | 8-16px gaps, standard 48px components, balanced content (default) |
| **Density = Spacious** | 16-32px gaps, larger components, fewer items per screen, editorial feel |
| **Radius = Sharp** | 0-4px on everything, technical/professional feel |
| **Radius = Moderate** | 8-12px on cards/buttons, 4-8px on inputs (modern default) |
| **Radius = Rounded** | 16-24px on cards, 12-16px on buttons, warm/friendly feel |
| **Radius = Pill** | Full-round buttons, rounded everything, playful/bold |
| **Accessibility = Yes** | Enforce ALL WCAG AA checks as mandatory, not suggestions. 4.5:1 contrast, 44px targets, screen reader annotations required |
| **Data-heavy** | Allow compact density, prioritize tables/charts, scan-friendly layouts |
| **E-commerce** | Emphasize product grids, cart flows, checkout optimization, trust signals |
| **Social/community** | Feed patterns, avatar systems, messaging UI, notification patterns |
| **i18n planned** | Apply localization-ready writing rules (Section 17), leave 30-40% extra space |
| **Dev handoff needed** | Generate component API specs + token mapping + annotation standards (Section 16) |
| **Performance-sensitive** | Apply design performance budget constraints (Section 18) |

#### For `skills/02-ux-flow-audit.md`:

| Project Context | How It Affects the Audit |
|----------------|--------------------------|
| **Platform = Mobile** | Enforce thumb zone checks, one-handed completion, bottom nav validation |
| **Platform = Web** | Check keyboard flow, breadcrumbs for deep pages, hover state coverage |
| **Personas defined** | Run the flow through EACH persona's perspective — different goals/paths |
| **Priority flows listed** | Audit those flows first and most thoroughly |
| **Accessibility = Yes** | Phase 6 (Accessibility) findings become 🔴 Critical instead of 🟡 Minor |
| **E-commerce** | Apply stricter step-count benchmarks (checkout ≤ 3 steps) + competitive benchmarks |
| **Offline support** | Phase 5 (Edge Cases) — offline state is mandatory, not optional |
| **All audits** | Map every finding to Nielsen's Heuristic (H1–H10) for industry-standard reporting |
| **Post-launch** | Use User Research Integration (Section 14) to validate findings with real data |

#### For `skills/03-ui-visual-audit.md`:

| Project Context | How It Affects the Audit |
|----------------|--------------------------|
| **Primary/secondary colors** | Check every screen against these — flag any rogue colors |
| **Chosen radius** | Verify ALL components match the chosen radius style |
| **Chosen density** | Judge spacing against the chosen density, not a generic standard |
| **Brand fonts** | Flag any text NOT using the specified fonts |
| **Icon set** | Flag any icons NOT from the specified set |
| **Dark mode = Yes** | Layer 9 (Dark Mode) is mandatory, findings are 🟠+ severity |
| **Dark mode = No** | Skip Layer 9 entirely |
| **Design system exists** | Layer 11 (System Compliance) is strict — all elements must be library instances. Run Token Architecture Audit |
| **Design system = None** | Layer 11 becomes advisory — note what SHOULD be componentized. Recommend 3-tier token structure |
| **Tone = Dark & Sleek** | Dark mode IS the primary mode — audit light mode as secondary |

#### For Data Flow Audits (`skills/02-ux-flow-audit.md §15` + `skills/04-figma-to-code.md Phase 9`):

| Project Context | How It Affects the Data Flow Audit |
|----------------|-------------------------------------|
| **Platform = Mobile** | Offline states are mandatory (🔴 Critical if missing). Cache strategy must be documented. |
| **Platform = Web** | Offline states are advisory (🟡 Minor). Focus on loading/error states. |
| **Real-time features** (chat, tracking, live data) | WebSocket/SSE connection status indicator is mandatory. Optimistic vs pessimistic update pattern must be annotated for every interactive element. |
| **E-commerce / Transactional** | All payment-related actions MUST use pessimistic updates (never optimistic). Confirmation screens mandatory before irreversible actions. |
| **Multi-step flows** (onboarding, checkout, booking) | Draft auto-save is mandatory. Back-navigation data preservation is mandatory. Cross-step dependencies must be annotated. |
| **Data-heavy / Dashboard** | Pagination strategy is mandatory for all lists. Partial loading (section-by-section) should be documented. Refresh/stale indicators needed. |
| **Offline support = Yes** | Every screen needs an offline variant. Cache strategy documented per screen. Sync-on-reconnect behavior annotated. |
| **Offline support = No** | Simple "No connection" error screen is sufficient. Focus audit on API error and loading states. |
| **Personas defined** | Run data flow through each persona — a new user (empty states) vs. power user (overflow/pagination) vs. returning user (stale/cached data). |

---

## Multiple Projects

This toolkit supports multiple projects. Each project gets its own `project/APP_PLAN.md` file (or a clearly separated section within it).

When switching projects, load the correct project's context before loading any sub-skill.

```
Project A: "NZ Game Trading App" → project/APP_PLAN.md (or project/APP_PLAN_NZ.md)
Project B: "Admin Dashboard"     → project/APP_PLAN_ADMIN.md
Project C: "Marketing Site"      → project/APP_PLAN_MARKETING.md
```

Each project's APP_PLAN file customizes how the three sub-skills behave.

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
