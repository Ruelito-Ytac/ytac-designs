# Design System Toolkit — Claude Code Plugin

A Claude Code plugin that turns Claude into a senior UI/UX design consultant. Covers design systems, UX flow auditing, visual QA, Figma automation, and design-to-code export.

---

## What This Does

Once installed, Claude gains the knowledge of a senior UX/UI consultant. Instead of giving vague design advice, Claude will:

- **Ask the right questions** before designing anything (23-question discovery)
- **Remember your project context** across sessions via `APP_PLAN.md`
- **Apply industry standards** — Nielsen's heuristics, WCAG accessibility, competitive benchmarks, design tokens
- **Audit existing designs** with structured frameworks (7-phase UX audit, 11-layer visual audit)
- **Build and fix Figma files** — auto layout, components, styles, variables, file cleanup
- **Export designs to code** — React, Vue, HTML/CSS, Tailwind
- **Generate actionable fix recommendations** with severity ratings, not just "this could be better"

---

## Installation

### From GitHub (Marketplace)

**Step 1** — Add the marketplace to Claude Code:

```
/plugin marketplace add Ruelito-Ytac/ytac-ui-ux-plugins
```

**Step 2** — Install the plugin:

```
/plugin install ruel-system-design-ui-ux-figma
```

That's it. The `/design` command and auto-triggering skill are now available.

### Direct Install (No Marketplace)

If you just want to install from the repo directly:

```
/plugin install Ruelito-Ytac/ytac-ui-ux-plugins
```

### Local Development / Testing

Clone the repo and load it locally without installing:

```bash
git clone https://github.com/Ruelito-Ytac/ytac-ui-ux-plugins.git
claude --plugin-dir ./ytac-ui-ux-plugins
```

This is useful for testing changes before pushing to GitHub.

---

## How to Use It

### Option 1: The `/design` Command

The plugin adds a `/design` slash command. Use it with or without a prompt:

```
/design
```

This opens an interactive menu where Claude asks what you'd like to work on.

Or pass your request directly:

```
/design Start a new mobile app for trading game cards
/design Audit this checkout flow
/design Check if these screens look modern
/design Build the login screen in Figma
/design Convert this component to React
/design Fix my Figma file
```

### Option 2: Natural Language (Auto-Trigger)

The skill also auto-triggers when you mention anything design-related. Just talk naturally:

> "I'm starting a new design project"

> "Audit this flow"

> "Is this UI good?"

> "Set up text styles in Figma"

Claude recognizes these phrases and loads the toolkit automatically — no slash command needed.

### Quick Reference

| What you say | What happens |
|---|---|
| `/design` (no prompt) | Interactive menu — pick what you want to do |
| `/design [prompt]` | Routes directly to the right sub-skill |
| "Start a new design project" | Full 23-question discovery → creates `APP_PLAN.md` → routes to sub-skill |
| "Design [screen/feature]" | Design guidance using your project context |
| "Audit this flow" | 7-phase UX flow audit with severity ratings |
| "Audit this UI" / "Check these screens" | 11-layer visual inspection |
| "Full design review" | UX audit + visual audit combined |
| "Build this in Figma" | Constructs screens/components in Figma via MCP |
| "Convert to code" / "Export to React" | Extracts Figma design → generates production code |
| "Fix my Figma file" | Auto layout repair, style compliance, cleanup |
| "Set up text styles" / "Create color variables" | Initializes design system tokens in Figma |
| "Show project context" | Displays current `APP_PLAN.md` |
| "Update project context" | Re-runs specific discovery questions |

---

## What Happens When You Start a New Project

When you say "start a new design project" (or use `/design` to start one), Claude runs a structured discovery:

1. **23-question discovery** covering project identity, platform, branding, design system setup, typography, navigation, and special requirements
2. **Generates `APP_PLAN.md`** — a living document with your project context in YAML
3. **Derives 3–5 design principles** from your answers (e.g. "Clarity over cleverness", "Mobile-first always")
4. **Routes to the right sub-skill** based on what you want to do first

You don't need to answer all 23 questions — Claude skips anything it already knows from your conversation.

---

## The APP_PLAN.md — Your Project's Memory

This is the file that makes the toolkit useful across sessions. Claude creates it during discovery and updates it as you work. It tracks:

- **Project Context** — YAML block with platform, colors, fonts, spacing density, etc.
- **Design Principles** — 3–5 values that guide trade-off decisions
- **Current Focus** — What's actively being worked on, blockers
- **Decision Log** — What was decided, why, and when (prevents re-debating)
- **Completed Work** — Screens designed, flows audited, components created
- **Outstanding Issues** — Open problems from audits, prioritized by severity
- **Design System Inventory** — Token collections and component library status
- **Changelog** — Design system changes over time (semantic versioning)

You can also edit `APP_PLAN.md` directly — it's just Markdown with a YAML block.

### Multiple Projects

Each project gets its own APP_PLAN file:

```
APP_PLAN.md              ← Default / primary project
APP_PLAN_ADMIN.md        ← Admin dashboard project
APP_PLAN_MARKETING.md    ← Marketing site project
```

Say "switch to [project name]" to load a different context.

---

## Plugin Structure

```
ytac-ui-ux-plugins/
│
├── .claude-plugin/
│   ├── plugin.json              ← Plugin metadata
│   └── marketplace.json         ← Marketplace catalog
│
├── commands/
│   └── design.md                ← /design slash command
│
├── skills/
│   └── design-system-toolkit/
│       ├── SKILL.md             ← Entry point (Claude reads this first)
│       ├── agents.md            ← Orchestrator (discovery, routing, governance)
│       ├── project/
│       │   └── APP_PLAN.md      ← Session state template
│       └── skills/
│           ├── 01-mobile-web-ui-ux-design-guide.md
│           ├── 02-ux-flow-audit.md
│           ├── 03-ui-visual-audit.md
│           └── 04-figma-to-code.md
│
├── .gitignore
└── README.md                    ← You are here
```

### What Each File Does

| File | Role |
|------|------|
| **`.claude-plugin/plugin.json`** | Plugin identity — name, version, author |
| **`.claude-plugin/marketplace.json`** | Marketplace catalog for named installs |
| **`commands/design.md`** | The `/design` slash command definition |
| **`skills/.../SKILL.md`** | Entry point — routes to the correct sub-skill |
| **`skills/.../agents.md`** | The brain — discovery, routing, governance rules |
| **`skills/.../project/APP_PLAN.md`** | Template for project session state |
| **`skills/.../skills/01-...`** | Design reference — typography, color, layout, navigation, forms, accessibility, handoff, performance budgets (18 sections) |
| **`skills/.../skills/02-...`** | 7-phase UX flow audit — structure, screen quality, mobile usability, interactions, edge cases, accessibility, cognitive load |
| **`skills/.../skills/03-...`** | 11-layer visual audit — spacing, typography, color, components, icons, surfaces, responsive, modern patterns, dark mode, motion, system compliance |
| **`skills/.../skills/04-...`** | Figma production & code export — auto layout, components, styles/variables, file repair, and design-to-code for React, Vue, HTML/CSS, Tailwind |

---

## What's Inside Each Sub-Skill

### 01 — Design Guide

18 sections covering production-quality interface design: UX thinking, user flows, mobile-first, layout, navigation, touch & interaction, typography, color & theming, forms, feedback & states, edge states, accessibility, platform conventions (iOS HIG / Material 3), design review, Figma workflow, design-to-dev handoff, content & UX writing, and performance budgets.

### 02 — UX Flow Audit

7-phase validation framework: flow structure, screen-by-screen review, mobile usability, interaction & feedback, edge cases, accessibility, and cognitive load. Includes Nielsen's heuristics mapping (H1–H10), severity system, fix framework with effort estimates, competitive benchmarks, and user research integration.

### 03 — Visual Audit

11-layer inspection: spacing & alignment, typography, color & contrast, component consistency, icons & imagery, elevation/borders/surfaces, responsive behavior, modern design patterns, dark mode integrity, motion & transitions, and design system compliance. Includes defect catalogs with before/after examples.

### 04 — Figma Production & Code Export

Figma mastery: auto layout, component construction, variant & property architecture, style & variable setup, file repair & cleanup. Plus design-to-code translation for React, React + Tailwind, Vue 3, and HTML/CSS. Includes Figma MCP tool reference (35+ tools documented).

---

## Governance (Built In)

The toolkit includes design system governance in `agents.md`:

- **Ownership model** — System Owner, Contributors, Consumers, Reviewers
- **Contribution workflow** — Identify → Propose → Review → Integrate
- **Component lifecycle** — Proposed → Draft → Beta → Stable → Deprecated
- **Deprecation process** — 5-step process with 30-day minimum notice
- **Audit schedule** — Weekly, monthly, quarterly, annual review cadence
- **Semantic versioning** — MAJOR.MINOR.PATCH adapted for design systems
- **Changelog & migration templates** — For tracking changes and breaking updates

---

## Requirements

- **Claude Code** v1.0.33 or later (plugin support)
- **Figma MCP** (optional) — needed for direct Figma building/repair via `04-figma-to-code`

---

## Uninstalling

To disable the plugin without removing it:

```
/plugin disable ruel-system-design-ui-ux-figma
```

To fully remove:

```
/plugin uninstall ruel-system-design-ui-ux-figma
```

---

## Stats

| Metric | Value |
|---|---|
| Total lines | ~5,950 |
| Discovery questions | 23 |
| Design guide sections | 18 |
| UX audit phases | 7 |
| Visual audit layers | 11 |
| Figma-to-code sections | 14 |
| Nielsen's heuristics mapped | All 10 |
| Industry benchmarks | 6 categories |
| Supported frameworks | React, Vue, HTML/CSS, Tailwind |
| Figma MCP tools documented | 35+ |

---

## Author

**Ruelito Ytac**

---

## License

See [LICENSE](LICENSE) for details.
