# Design System Toolkit

A Claude Code plugin that turns Claude into a senior UI/UX design consultant. Design screens, audit flows, build directly in Figma, and export to production code — all from the command line.

## How It Works

The toolkit follows three stages:

1. **Discover** — First run asks 6 quick questions (one at a time) about your project: name, platform, audience, brand colors, navigation, and priority screens. It remembers everything across sessions.
2. **Design** — You describe what you want in plain language. The toolkit loads the right skills, applies your project context, and produces design decisions grounded in real UX principles.
3. **Build** — Designs get built directly in Figma via MCP, or exported to React, Vue, HTML, or Tailwind.

## Sample Prompts

The best way to understand this tool is to see it in action. Here's what real usage looks like:

### Starting a New Project

```
You:  /design
Tool: "What would you like to do?"
      1. Start a new design project
      2. Design a screen or feature
      3. Audit a UX flow
      ...
You:  1
Tool: "What is the project name, and what does it do?"
You:  FitTrack — a fitness tracking app for gym beginners
Tool: "What platform?" A) Mobile  B) Web  C) Both
You:  A
      ... (4 more questions, one at a time)
Tool: → Generates your project context (APP_PLAN.md)
      → Sets up text styles, color variables, and spacing tokens in Figma
      → Ready to design
```

### Designing Screens

```
You:  /design:screen Onboarding flow — 3 screens: welcome, goal selection, account creation

What happens:
  1. Loads your project context (colors, typography, brand)
  2. Fetches your icon inventory from the Figma icon page
  3. Presents icon mapping: "I'll use these icons: Chevron Right ✓, Check ✓, User ✓..."
  4. Asks you to confirm before building
  5. Designs each screen following your design system
  6. Places screens 80px apart, left to right
  7. Uses your existing text styles and color variables — never creates duplicates
```

### Auditing an Existing Design

```
You:  /design:ux-audit https://figma.com/design/abc123/...

What happens:
  1. Fetches the design from Figma
  2. Runs a 7-phase UX inspection:
     Flow structure → Screen clarity → Mobile usability →
     Interaction feedback → Edge cases → Accessibility → Cognitive load
  3. Tags each finding with severity (Critical / Major / Minor)
  4. Presents a structured report with fix recommendations
```

```
You:  /design:ui-audit Check if the home screen looks modern

What happens:
  1. Inspects 11 visual layers:
     Spacing → Typography → Color/contrast → Components → Icons →
     Elevation → Responsive → Modern patterns → Dark mode → Motion →
     Design system compliance
  2. Catches issues like: inconsistent padding, hardcoded colors,
     off-grid spacing, missing text styles, duplicate components
  3. Severity-tagged report with exact fixes
```

### Building in Figma

```
You:  /design:figma-build Create the settings page in Figma

What happens:
  1. Queries your existing styles, variables, and components (reuse first)
  2. Fetches icon inventory — asks which icons to use before building
  3. Builds the screen with 100% auto layout, zero raw values
  4. Every text layer → existing Text Style
  5. Every color → existing Color Variable
  6. Every icon → instantiated from your icon page
  7. Positions the screen 80px to the right of your last screen
```

### Exporting to Code

```
You:  /design:figma-export Convert the home screen to React with Tailwind

What happens:
  1. Reads the Figma design structure and tokens
  2. Maps Figma Variables → CSS variables or Tailwind config
  3. Maps Text Styles → typography utility classes
  4. Translates components to React with proper props
  5. Outputs production-ready code
```

### You Can Also Skip Commands

The plugin understands natural language. Just describe what you need:

```
"audit this flow"                    → runs UX flow audit
"is this UI good?"                   → runs UI visual audit
"build the login screen in Figma"    → builds in Figma
"export this to React"               → exports to code
"check the auto layout"              → runs auto layout audit
"review my design"                   → runs full UX + UI audit
```

## Commands

| Command | What it does |
|---|---|
| `/design` | Main menu — pick a workflow or describe what you need |
| `/design:screen` | Design a screen or feature using your project's design system |
| `/design:ux-audit` | Audit a user flow for usability issues (7-phase inspection) |
| `/design:ui-audit` | Audit visual quality and consistency (11-layer inspection) |
| `/design:full-design-audit` | Run both UX + UI audits together |
| `/design:figma-autolayout-audit` | Check auto layout structure and sizing modes (6-phase inspection) |
| `/design:figma-build` | Build or modify designs directly in Figma |
| `/design:figma-export` | Convert Figma designs to React, Vue, HTML, or Tailwind |

Every command accepts an optional prompt:

```
/design:screen Login screen with social auth buttons
/design:ux-audit Check the checkout flow for drop-off risks
/design:figma-build Create the profile page with avatar upload
/design:figma-export Export all card components to Tailwind
```

## What Makes This Different

This isn't a generic "make it pretty" tool. It enforces real design system discipline:

- **Reuse first** — Always queries existing text styles, color variables, and components before building. Never creates duplicates.
- **Icon hard gate** — Before building any screen, fetches your icon inventory, presents mappings, and waits for your confirmation. No emoji substitutes, no fake icons.
- **Zero raw values** — Every text layer references a Text Style. Every color references a Variable. Every spacing value comes from the token scale.
- **Proximity clustering** — Related elements get tighter gaps; unrelated elements get the next step up. Visual grouping through spacing, not borders.
- **80px flow spacing** — Screen frames in user flows are always 80px apart, never stacked on top of each other.
- **Consistent builds** — 100% auto layout, semantic layer naming, proper sizing modes (HUG/FILL/FIXED), and the 8px grid.
- **Project memory** — Remembers your project context, brand colors, typography, platform, and navigation pattern across sessions.

## Install

**Step 1** — Verify your GitHub SSH access (run in terminal):

```bash
ssh -T git@github.com
```

You should see "Hi [username]! You've successfully authenticated..." — if not, [set up SSH keys](https://docs.github.com/en/authentication/connecting-to-github-with-ssh) first.

**Step 2** — Add the marketplace source (run in Claude Code):

```
/plugin marketplace add Ruelito-Ytac/ytac-designs
```

**Step 3** — Install the plugin:

```
/plugin install ruel-system-design-ui-ux-figma@ytac-designs
```

## Requirements

- **Claude Code** with plugin support
- **Figma MCP** (required for Figma build, export, and auto layout audit commands)

## Author

Ruelito Ytac
