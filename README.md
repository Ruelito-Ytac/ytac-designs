# Design System Toolkit

A Claude Code plugin that turns Claude into your UI/UX design partner. Design screens, audit flows, build in Figma, and export to code — all from the command line.

## Install

```
/install-plugin Ruelito-Ytac/ytac-designs
```

## Usage

Run `/design` to open the main menu, or jump straight to a workflow:

| Command | What it does |
|---|---|
| `/design` | Start here — pick what you want to do |
| `/design:screen` | Design a screen or feature |
| `/design:ux-audit` | Audit a user flow for usability issues |
| `/design:ui-audit` | Audit visual quality and consistency |
| `/design:full-design-audit` | Run both UX + UI audits together |
| `/design:figma-autolayout-audit` | Check auto layout structure in Figma |
| `/design:figma-build` | Build or modify designs directly in Figma |
| `/design:figma-export` | Convert Figma designs to React, Vue, HTML, or Tailwind |

Every command accepts an optional prompt:

```
/design:screen Login screen
/design:ux-audit Check the checkout flow
/design:figma-build Create the settings page in Figma
```

You can also skip commands entirely — just describe what you need and the plugin will route you automatically:

```
"audit this flow"
"build the home screen in Figma"
"export this to React"
```

## First time?

When you run `/design` for the first time, the plugin asks 6 quick questions (one at a time) to understand your project — name, platform, audience, brand colors, navigation, and priority screens. After that, it remembers your context across sessions.

## Requirements

- **Claude Code** with plugin support
- **Figma MCP** (for Figma build, export, and auto layout audit commands)

## Author

Ruelito Ytac
