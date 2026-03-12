# Design System Toolkit — Claude Code Plugin

Turn Claude into a senior UI/UX design consultant with structured audits, Figma automation, and design-to-code export.

## Install

```
/plugin marketplace add Ruelito-Ytac/ytac-designs
/plugin install ruel-system-design-ui-ux-figma@ytac-designs
```

## Commands

| Command | Description |
|---|---|
| `/design` | Main menu — routes to the right workflow |
| `/design:screen` | Design a specific screen |
| `/design:ux-audit` | 7-phase UX flow audit |
| `/design:ui-audit` | 11-layer visual audit |
| `/design:full-design-audit` | Combined UX + UI audit |
| `/design:figma-autolayout-audit` | 6-phase auto layout audit |
| `/design:figma-build` | Build or fix designs in Figma |
| `/design:figma-export` | Export Figma designs to code |

All commands accept an optional argument: `/design:screen Login screen`

## What's Inside

**Core skills:**

| Skill | Purpose |
|---|---|
| `01-mobile-web-ui-ux-design-guide` | Design reference — typography, color, layout, accessibility, platform conventions |
| `02-ux-flow-audit` | 7-phase UX validation with Nielsen's heuristics and severity ratings |
| `03-ui-visual-audit` | 11-layer visual inspection — spacing, color, components, responsive, dark mode |
| `05-figma-autolayout-audit` | 6-phase auto layout inspection — direction, sizing, spacing, nesting |

**Modular Figma skills** (load only what you need):

| Module | Purpose |
|---|---|
| `04a-figma-build-core` | Lean build skill — screens, auto layout, batch-first patterns |
| `04b-component-architect` | Smart component creation — variants, properties, collaborative personality |
| `04c-code-export` | Design-to-code — React, Vue, HTML/CSS, Tailwind |
| `04d-figma-repair` | Detection scripts, bulk fixes, file audit |
| `04e-figma-tools-ref` | MCP tool reference (loaded on demand) |

**Supporting modules:** discovery (6-question project onboarding), router (intent classification), governance (design system lifecycle & versioning).

## Quick Start

1. **Install** the plugin (see above)
2. **Run `/design`** — Claude asks what you want to work on and routes you
3. **Pick a workflow** — design a screen, audit a flow, build in Figma, or export to code

The plugin also auto-triggers on design-related prompts. Just say "audit this flow" or "build the login screen in Figma" and Claude loads the right skill.

## Requirements

- **Claude Code** with plugin support
- **Figma MCP** — required for `/design:figma-build`, `/design:figma-export`, and `/design:figma-autolayout-audit`

## Author

**Ruelito Ytac**
