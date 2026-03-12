# Plugin Redesign Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Refactor the ytac-designs plugin from a monolithic single-command architecture to modular sub-commands with progressive discovery, Figma icon integration, modern design rules, and a new auto layout audit skill.

**Architecture:** Split `agents.md` (991 lines) into `discovery.md`, `router.md`, and `governance.md`. Create 6 new command files for direct sub-command routing. Add new checks to existing audit skills and create a new skill 05.

**Tech Stack:** Markdown skill definitions, Claude Code plugin system (commands/ + skills/ directories), Figma MCP integration

**Spec:** `docs/superpowers/specs/2026-03-12-plugin-redesign-design.md`

---

## Chunk 1: Command Files & Naming Validation

### Task 1: Validate colon sub-command naming

**Files:**
- Create: `commands/design:ux-audit.md` (test file)

- [ ] **Step 1: Create a test command file with colon in name**

```markdown
---
name: design:ux-audit
description: >
  Run a UX flow audit on a Figma design. Validates user journeys using a 7-phase
  inspection framework. Accepts an optional Figma URL or description of the flow to audit.
---

# UX Flow Audit — Test Command

This is a test file to validate colon naming works in Claude Code.

$ARGUMENTS
```

Write this to `commands/design:ux-audit.md`.

- [ ] **Step 2: Verify it appears in Claude Code's skill list**

Reinstall or reload the plugin, then check if `/design:ux-audit` is recognized as a valid command. If the colon works, proceed to Task 2. If not, rename to `design-ux-audit.md` with `name: design-ux-audit` and verify that works instead.

- [ ] **Step 3: Record the naming decision**

If colon works → use `design:*` format for all sub-commands.
If colon doesn't work → use `design-*` format for all sub-commands.
Update the spec file's Technical Validation section with the result.

- [ ] **Step 4: Commit**

```bash
git add commands/design:ux-audit.md
git commit -m "feat: validate sub-command naming convention (colon format)"
```

---

### Task 2: Create all 6 sub-command files

**Files:**
- Create: `commands/design:ux-audit.md` (update from test)
- Create: `commands/design:ui-audit.md`
- Create: `commands/design:full-design-audit.md`
- Create: `commands/design:figma-autolayout-audit.md`
- Create: `commands/design:screen.md`
- Create: `commands/design:figma-build.md`

> **Note:** If Task 1 determined colons don't work, use hyphens instead (e.g., `design-ux-audit.md`). Adjust all filenames and `name:` fields accordingly.

- [ ] **Step 1: Write `commands/design:ux-audit.md`** (replace test file content)

```markdown
---
name: design:ux-audit
description: >
  Run a UX flow audit on a Figma design. Validates user journeys using a 7-phase
  inspection framework covering flow structure, screen clarity, mobile usability,
  interaction feedback, edge cases, accessibility, and cognitive load.

  Examples:
    /design:ux-audit
    /design:ux-audit https://figma.com/design/abc123/...
    /design:ux-audit Check the checkout flow
---

# UX Flow Audit

You have been invoked via the `/design:ux-audit` command.

## Instructions

1. **Load project context** (optional): If `skills/design-system-toolkit/project/APP_PLAN.md` exists in this plugin directory, read it for project context. This is NOT required — audits work without it.
2. **Load the audit skill**: Read `skills/design-system-toolkit/skills/02-ux-flow-audit.md` from this plugin directory.
3. **If the user provided a Figma URL or description** via `$ARGUMENTS`, start the audit immediately using that context.
4. **If no arguments were provided**, ask the user to share a Figma URL or describe the flow they want audited.
5. **Apply project context** if APP_PLAN.md was loaded — use it to customize severity ratings and platform-specific checks per the context application rules in the audit skill.
6. **Do NOT run discovery** — this command skips project setup entirely.

## User's Request

$ARGUMENTS
```

- [ ] **Step 2: Write `commands/design:ui-audit.md`**

```markdown
---
name: design:ui-audit
description: >
  Run a UI visual audit on a Figma design. Inspects 11 layers: spacing, typography,
  color/contrast, components, icons, elevation, responsive, modern patterns, dark mode,
  motion, and design system compliance.

  Examples:
    /design:ui-audit
    /design:ui-audit https://figma.com/design/abc123/...
    /design:ui-audit Check if the home screen looks modern
---

# UI Visual Audit

You have been invoked via the `/design:ui-audit` command.

## Instructions

1. **Load project context** (optional): If `skills/design-system-toolkit/project/APP_PLAN.md` exists in this plugin directory, read it for project context. This is NOT required — audits work without it.
2. **Load the audit skill**: Read `skills/design-system-toolkit/skills/03-ui-visual-audit.md` from this plugin directory.
3. **If the user provided a Figma URL or description** via `$ARGUMENTS`, start the audit immediately.
4. **If no arguments were provided**, ask the user to share a Figma URL or describe the screens they want audited.
5. **Apply project context** if APP_PLAN.md was loaded.
6. **Do NOT run discovery** — this command skips project setup entirely.

## User's Request

$ARGUMENTS
```

- [ ] **Step 3: Write `commands/design:full-design-audit.md`**

```markdown
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
```

- [ ] **Step 4: Write `commands/design:figma-autolayout-audit.md`**

```markdown
---
name: design:figma-autolayout-audit
description: >
  Audit Figma auto layout structure and best practices. Inspects 6 phases: layout direction,
  sizing modes (HUG/FILL/FIXED), padding/spacing, alignment, nesting depth, and absolute positioning.

  Examples:
    /design:figma-autolayout-audit
    /design:figma-autolayout-audit https://figma.com/design/abc123/...
    /design:figma-autolayout-audit Check the card component layout
---

# Figma Auto Layout Audit

You have been invoked via the `/design:figma-autolayout-audit` command.

## Instructions

1. **Load project context** (optional): If `skills/design-system-toolkit/project/APP_PLAN.md` exists in this plugin directory, read it for project context.
2. **Load the audit skill**: Read `skills/design-system-toolkit/skills/05-figma-autolayout-audit.md` from this plugin directory.
3. **If the user provided a Figma URL or description** via `$ARGUMENTS`, start the audit immediately.
4. **If no arguments were provided**, ask the user to share a Figma URL or describe the frames they want audited.
5. **Do NOT run discovery** — this command skips project setup entirely.

## User's Request

$ARGUMENTS
```

- [ ] **Step 5: Write `commands/design:screen.md`**

```markdown
---
name: design:screen
description: >
  Design a specific screen or feature. Uses the design guide with Figma workflow for
  creating new screens, components, and layouts. Requires project context.

  Examples:
    /design:screen
    /design:screen Login screen
    /design:screen Build the onboarding flow
---

# Design Screen

You have been invoked via the `/design:screen` command.

## Instructions

1. **Load project context** (required): Read `skills/design-system-toolkit/project/APP_PLAN.md` from this plugin directory.
   - If APP_PLAN.md does NOT exist, run discovery first: read `skills/design-system-toolkit/discovery.md` and ask the 6 core questions one at a time. Generate APP_PLAN.md before proceeding.
2. **Check design system**: Verify text styles + color styles exist in Figma. If not, read `skills/design-system-toolkit/governance.md` and run the design system initialization (Step 3) first.
3. **Load the design guide**: Read `skills/design-system-toolkit/skills/01-mobile-web-ui-ux-design-guide.md` from this plugin directory.
4. **If the user specified a screen** via `$ARGUMENTS`, start designing it immediately.
5. **If no arguments were provided**, ask the user which screen or feature they want to design.

## User's Request

$ARGUMENTS
```

- [ ] **Step 6: Write `commands/design:figma-build.md`**

```markdown
---
name: design:figma-build
description: >
  Build or modify designs directly in Figma using MCP tools. Creates frames, components,
  auto layout structures, text styles, color variables, and organizes layers.

  Examples:
    /design:figma-build
    /design:figma-build Create the login screen in Figma
    /design:figma-build Fix the auto layout on the card component
---

# Figma Build

You have been invoked via the `/design:figma-build` command.

## Instructions

1. **Load project context** (required): Read `skills/design-system-toolkit/project/APP_PLAN.md` from this plugin directory.
   - If APP_PLAN.md does NOT exist, run discovery first: read `skills/design-system-toolkit/discovery.md` and ask the 6 core questions one at a time. Generate APP_PLAN.md before proceeding.
2. **Check design system**: Verify text styles + color styles exist in Figma. If not, read `skills/design-system-toolkit/governance.md` and run the design system initialization first.
3. **Load the build skill**: Read `skills/design-system-toolkit/skills/04-figma-to-code.md` (Build Mode) from this plugin directory.
4. **If the user specified what to build** via `$ARGUMENTS`, start building immediately.
5. **If no arguments were provided**, ask the user what they want to build or fix in Figma.
6. **After completing a build session**, recommend: "Run `/design:figma-autolayout-audit` to validate the auto layout structure."

## User's Request

$ARGUMENTS
```

- [ ] **Step 7: Commit all command files**

```bash
git add -f commands/
git commit -m "feat: add 6 sub-command files for direct workflow routing"
```

---

### Task 3: Update the base `/design` command

> **DEPENDENCY:** Execute this task AFTER Tasks 4-6 (discovery.md, router.md, governance.md must exist first, since this command references them).

**Files:**
- Modify: `commands/design.md`

- [ ] **Step 1: Update `commands/design.md` to reference new architecture**

Replace the entire file content with:

```markdown
---
name: design
description: >
  Launch the Design System Toolkit. Use this command to start a new design project,
  design screens, audit UX flows, audit visual quality, build in Figma, or export to code.
  Accepts an optional prompt describing what you want to do.

  For direct access to specific workflows, use sub-commands:
    /design:ux-audit — Audit a UX flow (7 phases)
    /design:ui-audit — Audit UI visuals (11 layers)
    /design:full-design-audit — Combined UX + UI audit
    /design:figma-autolayout-audit — Audit Figma auto layout
    /design:screen — Design a specific screen
    /design:figma-build — Build or fix in Figma

  Examples:
    /design Start a new mobile app project
    /design Audit this checkout flow
    /design Check if these screens look modern
---

# Design System Toolkit — Slash Command

You have been invoked via the `/design` command.

## Instructions

1. **Read the skill entry point**: Load `skills/design-system-toolkit/SKILL.md` from this plugin directory.
2. **Check for existing project context**: If `skills/design-system-toolkit/project/APP_PLAN.md` exists, read it.
   - If APP_PLAN.md exists → skip discovery, load `skills/design-system-toolkit/router.md` to classify the user's intent and route to the correct sub-skill.
   - If APP_PLAN.md does NOT exist → load `skills/design-system-toolkit/discovery.md` and run the 6 core questions one at a time. After discovery, generate APP_PLAN.md, then route.
3. **If the user provided a prompt after `/design`**, use `$ARGUMENTS` as their request and route accordingly via `router.md`.
4. **If no prompt was provided**, present the interactive menu:
   - Start a new design project → discovery.md
   - Design a screen or feature → `/design:screen`
   - Audit a UX flow → `/design:ux-audit`
   - Audit visual quality → `/design:ui-audit`
   - Full design audit (UX + UI) → `/design:full-design-audit`
   - Audit Figma auto layout → `/design:figma-autolayout-audit`
   - Build or fix in Figma → `/design:figma-build`
   - Export a design to code → skill 04 (Export Mode)

## User's Request

$ARGUMENTS
```

- [ ] **Step 2: Commit**

```bash
git add commands/design.md
git commit -m "feat: update base /design command with sub-command references and new routing"
```

---

## Chunk 2: Split agents.md Into Modules

### Task 4: Create discovery.md (progressive questions)

**Files:**
- Create: `skills/design-system-toolkit/discovery.md`

- [ ] **Step 1: Write `skills/design-system-toolkit/discovery.md`**

This file replaces Step 1 + Step 2 of `agents.md` (lines 74-283) with a progressive one-at-a-time question flow. Write the following content:

```markdown
# Project Discovery — Progressive Flow

> This module handles project setup through progressive questioning.
> Questions are asked ONE AT A TIME, each waiting for the user's response before proceeding.
> Already-known answers from APP_PLAN.md are always skipped.

---

## How Discovery Works

1. Check if `project/APP_PLAN.md` exists
   - **Exists and complete** → Skip discovery entirely, return to caller
   - **Exists but incomplete** → Ask only the missing questions
   - **Does not exist** → Run full core discovery below

2. Ask core questions one at a time (see below)
3. After all core questions answered → Generate `project/APP_PLAN.md`
4. Derive 3-5 design principles from the answers
5. Return control to the caller (router.md or command file)

---

## Core Questions (Always Asked, One at a Time)

Ask these 6 questions sequentially. Each question is its own message — NEVER combine questions. Prefer multiple choice when possible.

### Question 1: Project Name & Description

Ask:
> "What is the project name, and what does it do? (1-2 sentences)"

Store: `project.name` and `project.description`

---

### Question 2: Platform

Ask:
> "What platform(s) are you designing for?
>
> A) Mobile only (iOS / Android / Both)
> B) Web only (Desktop / Responsive)
> C) Cross-platform (Mobile + Web)
> D) Other (Tablet app, Watch, TV, etc.)"

If they choose A, follow up (still one at a time):
> "Which mobile platform takes priority?
>
> A) iOS first (Apple HIG conventions)
> B) Android first (Material Design conventions)
> C) Both equally (neutral pattern)"

Store: `platform.type` and `platform.mobile_priority`

---

### Question 3: Target Audience

Ask:
> "Who is the target audience? (e.g., 'young professionals 25-35', 'small business owners', 'gamers')"

Store: `project.audience`

---

### Question 4: Brand Colors & Visual Tone

Ask:
> "What are your brand colors and visual tone?
>
> **Colors:** Primary color (hex or description), secondary/accent color (hex or 'derive from primary')
>
> **Tone — pick one:**
> A) Clean & Minimal — lots of white space, subtle colors
> B) Bold & Vibrant — strong colors, high contrast, energetic
> C) Dark & Sleek — dark surfaces, glowing accents, premium feel
> D) Warm & Friendly — rounded shapes, soft colors, approachable
> E) Corporate & Professional — structured, neutral tones, formal
> F) Playful & Fun — bright colors, illustrations, casual"

Store: `branding.primary_color`, `branding.secondary_color`, `branding.visual_tone`

---

### Question 5: Navigation Pattern

Ask:
> "How many primary sections does the app have, and what navigation pattern do you prefer?
>
> **Sections:** A) 2-3  B) 4-5  C) 6+  D) Single flow (no persistent nav)
>
> **Mobile nav:** A) Bottom tab bar  B) Top tabs  C) Drawer/hamburger  D) Let me recommend
>
> **Desktop nav** (if applicable): A) Top bar  B) Sidebar  C) Both  D) Let me recommend"

Store: `navigation.section_count`, `navigation.mobile_pattern`, `navigation.desktop_pattern`

---

### Question 6: Priority Flows/Screens

Ask:
> "What are the first 2-3 screens or flows you want to focus on? (e.g., 'onboarding flow', 'home screen', 'checkout')"

Store: `requirements.priority_flows`

---

## Deep Questions (On-Demand)

These are NOT asked during initial discovery. They are asked one at a time, only when the current task triggers them. If the answer is already known from APP_PLAN.md, skip.

| Trigger Context | Question to Ask |
|----------------|----------------|
| **Building in Figma with existing icon assets** | "Which Figma page contains your project's icons? (e.g., 'UI Icons', 'Assets/Icons')" → Store: `branding.icon_page_name` |
| **Designing text-heavy screens** | "What font(s) are you using? (Primary font for headings, secondary for body — or 'recommend for me')" → Store: `branding.brand_fonts` |
| **Designing text-heavy screens** | "What base body text size? A) 14px (compact) B) 16px (standard) C) 18px (spacious)" → Store: `design_system.base_text_size` |
| **Setting up design system** | "What corner radius style? A) Sharp (0-4px) B) Moderate (8-12px) C) Rounded (16-24px) D) Pill/capsule E) Recommend for me" → Store: `design_system.corner_radius` |
| **Setting up design system** | "What spacing density? A) Compact B) Comfortable (recommended) C) Spacious" → Store: `design_system.spacing_density` |
| **Setting up design system** | "Do you have an existing design system or component library? A) Yes (Figma library exists) B) Partial C) None D) Using a public system" → Store: `design_system.status` |
| **Building responsive layouts** | "What target screen sizes? Mobile: 375/393/430px, Tablet: 768/820/1024px, Desktop: 1280/1440/1920px" → Store: `platform.screen_sizes` |
| **Building responsive layouts** | "What is the Figma file structure? A) Single file B) Multi-file C) Not set up yet" |
| **Scoping special needs** | "Any special requirements? (Accessibility, i18n, offline, dark mode, animation-heavy, data-heavy, e-commerce, social, admin)" → Store: `requirements.special`, `branding.dark_mode` |
| **Scoping special needs** | "What user personas should be considered? (List names and roles)" → Store: `requirements.personas` |

---

## After Discovery: Generate APP_PLAN.md

After all core questions are answered:

1. Compile answers into the `project/APP_PLAN.md` YAML block (use the APP_PLAN.md template)
2. Derive 3-5 design principles from the answers (tone + audience + requirements)
3. Set `project.stage` using inference rules in step 5 below
4. Set defaults for unanswered deep questions:
   - `icon_set`: "Not set"
   - `icon_page_name`: "Not set"
   - `dark_mode`: "Later"
   - `design_system.status`: "None"
   - `corner_radius`: "Moderate 8-12"
   - `spacing_density`: "Comfortable"
   - `base_text_size`: "16px"
5. **Infer `project.stage`** from context:
   - If user mentions "redesign", "existing app", "revamp" → set to "Redesign"
   - If user mentions "extending", "adding features", "existing design system" → set to "Extending"
   - If user has existing Figma files with components → set to "Extending"
   - Otherwise → set to "New"
6. Return control to the caller

---

## Rules

- **One question per message** — never combine questions
- **Multiple choice preferred** — easier to answer than open-ended when possible
- **Skip known answers** — if APP_PLAN.md already has the answer, don't re-ask
- **Store answers immediately** — update APP_PLAN.md after each answer if the file already exists
- **Respect the user's pace** — wait for their response before the next question
```

- [ ] **Step 2: Commit**

```bash
git add skills/design-system-toolkit/discovery.md
git commit -m "feat: create discovery.md with progressive one-at-a-time question flow"
```

---

### Task 5: Create router.md (intent dispatcher)

**Files:**
- Create: `skills/design-system-toolkit/router.md`

- [ ] **Step 1: Write `skills/design-system-toolkit/router.md`**

```markdown
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
> 8. **Export to code** — Convert Figma designs to React/HTML/CSS/Tailwind

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
| "design screen", "create screen", "new screen", "design the", "build screen" | Load `skills/01-mobile-web-ui-ux-design-guide.md` |
| "build in Figma", "create in Figma", "fix Figma", "add auto layout", "build component" | Load `skills/04-figma-to-code.md` (Build Mode) |
| "export", "convert to code", "to React", "to HTML", "generate code" | Load `skills/04-figma-to-code.md` (Export Mode) |
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
```

- [ ] **Step 2: Commit**

```bash
git add skills/design-system-toolkit/router.md
git commit -m "feat: create router.md for intent classification and menu routing"
```

---

### Task 6: Create governance.md (design system governance)

**Files:**
- Create: `skills/design-system-toolkit/governance.md`

- [ ] **Step 1: Extract governance content from agents.md**

Read `skills/design-system-toolkit/agents.md` lines 286-556 (Step 3: Initialize Design System in Figma, including enforcement rules) and lines 768-991 (Design System Governance + Versioning & Changelog).

Write `skills/design-system-toolkit/governance.md` containing:

1. **Design System Initialization** — The complete Step 3 from agents.md (type scale generation, color system generation, Figma creation instructions, enforcement rules) — lines 286-556
2. **Gate Check** — "No designing until text styles + color styles exist in Figma"
3. **Design System Governance** — Ownership model, contribution model, component lifecycle, proposal template, deprecation process — lines 768-897
4. **Versioning & Changelog** — Semantic versioning rules, changelog template, communication methods, migration guide template — lines 900-991

The file header should be:

```markdown
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
```

Copy the content from agents.md sections verbatim (they're well-written), just reorganize under this header.

- [ ] **Step 2: Commit**

```bash
git add skills/design-system-toolkit/governance.md
git commit -m "feat: create governance.md extracted from agents.md Steps 3 + governance sections"
```

---

### Task 7: Update SKILL.md entry point

**Files:**
- Modify: `skills/design-system-toolkit/SKILL.md`

- [ ] **Step 1: Update SKILL.md to reference new module structure**

Replace the entire content of `skills/design-system-toolkit/SKILL.md` with:

```markdown
---
name: design-system-toolkit
description: >
  Master entry point for all UI/UX design work. Import ONLY this file — it automatically loads
  the correct sub-skills based on your project context. Covers design principles, UX flow auditing,
  UI visual auditing, Figma auto layout auditing, and design-to-code export for mobile, web, or cross-platform projects.

  Trigger when the user says anything related to: designing screens, creating UI, reviewing UX,
  auditing a design, checking if a design is modern, building a user flow, starting a new design
  project, setting up a design system, "use the design toolkit", "start design", "design this",
  "audit this", "review my design", "check this flow", "is this UI good", or any Figma design task.

  IMPORTANT: This is the ONLY file you need to read first. It will tell you which files to load next.
---

# Design System Toolkit — Entry Point

> **One import. Five skills. Any project.**

## First Steps

1. **Read `project/APP_PLAN.md`** (if it exists) — The session state with project context and history.
2. **Starting from scratch?** → Read `discovery.md` — Runs progressive discovery (6 core questions, one at a time). Generates APP_PLAN.md.
3. **Need to route?** → Read `router.md` — Classifies user intent and routes to the correct sub-skill. Only needed for base `/design` command.
4. **Design system setup?** → Read `governance.md` — Handles design system initialization in Figma, component lifecycle, versioning.
5. **Load sub-skills on demand** from `skills/` — Only when the task requires them. Never load all upfront.

## File Manifest

```
design-system-toolkit/
│
├── SKILL.md                 ← You are here (entry point — read this first)
├── discovery.md             ← Progressive project discovery (6 core Q's, one at a time)
├── router.md                ← Intent classification & menu (base /design only)
├── governance.md            ← Design system init, lifecycle, versioning
│
├── project/                 ← YOUR project state (Claude updates, you can edit)
│   └── APP_PLAN.md          ← Session state (context, decisions, progress)
│
└── skills/                  ← Reference library (do not edit)
    ├── 01-mobile-web-ui-ux-design-guide.md   ← Design principles & patterns
    ├── 02-ux-flow-audit.md                   ← UX flow validation (7 phases)
    ├── 03-ui-visual-audit.md                 ← UI visual inspection (11 layers)
    ├── 04-figma-to-code.md                   ← Figma production & code export
    └── 05-figma-autolayout-audit.md          ← Figma auto layout audit (6 phases)
```

## Quick Reference

| What the user says | What to do |
|---|---|
| "Start a new design project" | `discovery.md` → Create `project/APP_PLAN.md` → `governance.md` Step 3 → Route to sub-skill |
| "Design [screen/feature]" | Load `project/APP_PLAN.md` → Load `skills/01` |
| "Audit this flow" / "Check this UX" | Load `project/APP_PLAN.md` (if exists) → Load `skills/02` |
| "Audit this UI" / "Check these screens" | Load `project/APP_PLAN.md` (if exists) → Load `skills/03` |
| "Full design review" | Load `project/APP_PLAN.md` (if exists) → Load `skills/02` then `skills/03` |
| "Audit auto layout" / "Check layout" | Load `project/APP_PLAN.md` (if exists) → Load `skills/05` |
| "Build this in Figma" | Load `project/APP_PLAN.md` → Load `skills/04` (Build Mode) |
| "Convert to code" / "Export to React" | Load `project/APP_PLAN.md` → Load `skills/04` (Export Mode) |
| "Fix my Figma file" / "Add auto layout" | Load `project/APP_PLAN.md` → Load `skills/04` |
| "Set up styles" / "Create variables" | `governance.md` → Load `skills/04` for execution |
| "Show project context" | Read `project/APP_PLAN.md` and display |
| "Update project context" | `discovery.md` → Re-run specific questions → Update `project/APP_PLAN.md` |
| "Switch to [project name]" | Load that project's APP_PLAN from `project/` |
```

- [ ] **Step 2: Commit**

```bash
git add skills/design-system-toolkit/SKILL.md
git commit -m "feat: update SKILL.md entry point for new modular architecture"
```

---

## Chunk 3: APP_PLAN.md, Icon Integration & Modern Design Rules

### Task 8: Update APP_PLAN.md template with icon_page_name field

**Files:**
- Modify: `skills/design-system-toolkit/project/APP_PLAN.md`

- [ ] **Step 1: Add `icon_page_name` field to the branding section**

In `skills/design-system-toolkit/project/APP_PLAN.md`, find the `branding:` YAML block (around line 35-43) and add the `icon_page_name` field after `icon_set`:

Find:
```yaml
  icon_set: "[Phosphor / Lucide / Material / SF Symbols / Custom]"
  dark_mode: "[Yes / No / Later]"
```

Replace with:
```yaml
  icon_set: "[Phosphor / Lucide / Material / SF Symbols / Custom]"
  icon_page_name: "[Name of Figma page containing project icons — e.g., 'UI Icons']"
  dark_mode: "[Yes / No / Later]"
```

- [ ] **Step 2: Commit**

```bash
git add skills/design-system-toolkit/project/APP_PLAN.md
git commit -m "feat: add icon_page_name field to APP_PLAN.md template"
```

---

### Task 9: Update skill 01 — icon page integration + modern patterns

**Files:**
- Modify: `skills/design-system-toolkit/skills/01-mobile-web-ui-ux-design-guide.md`

- [ ] **Step 1: Update Section 22 ("Icon Source") to use dynamic icon page name**

In `skills/design-system-toolkit/skills/01-mobile-web-ui-ux-design-guide.md`, find Section 22 (line 2155). The current text hardcodes "UI Icons" as the page name. Update it to reference APP_PLAN.md's `icon_page_name` field instead.

Find the section header and first paragraph (around lines 2155-2161):
```
## 22. Icon Source — The "UI Icons" Page

When building screens in Figma, **always pull icons from the "UI Icons" page** in the same Figma file. This is the single source of truth for all icons used in the project.

### How It Works

The Figma file has a dedicated page named **"UI Icons"** that contains all icon components for the project. When you need an icon:
```

Replace with:
```
## 22. Icon Source — The Project Icons Page

When building screens in Figma, **always pull icons from the project's designated icon page** in the same Figma file. This is the single source of truth for all icons used in the project.

### Determining the Icon Page Name

1. **Check `project/APP_PLAN.md`** for the `branding.icon_page_name` field
2. If set → use that page name (e.g., "UI Icons", "Assets/Icons", "Iconography")
3. If NOT set → **ask the user**: "Which Figma page contains your project's icons?" Store the answer in APP_PLAN.md's `branding.icon_page_name` field before proceeding.
4. **Never assume "UI Icons"** — always check APP_PLAN.md first

### How It Works

The Figma file has a dedicated page (named per `icon_page_name` in APP_PLAN.md) that contains all icon components for the project. When you need an icon:
```

Also update the Table of Contents entry (around line 45) from:
`22. [Icon Source — The "UI Icons" Page](#22-icon-source--the-ui-icons-page)` to:
`22. [Icon Source — The Project Icons Page](#22-icon-source--the-project-icons-page)`

Also update ALL hardcoded references to `"UI Icons"` in the JavaScript code blocks in this section (lines 2167-2227) to use a variable pattern:

Find (around line 2176):
```javascript
  const iconsPage = figma.root.children.find(p => p.name === "UI Icons");
  if (!iconsPage) return { error: "No 'UI Icons' page found" };
```

Replace with:
```javascript
  // Use the icon_page_name from APP_PLAN.md (default: "UI Icons")
  const ICON_PAGE_NAME = "{{icon_page_name}}"; // Replace with actual value from APP_PLAN.md
  const iconsPage = figma.root.children.find(p => p.name === ICON_PAGE_NAME);
  if (!iconsPage) return { error: `No '${ICON_PAGE_NAME}' page found. Check branding.icon_page_name in APP_PLAN.md` };
```

Do the same for the second code block (around line 2212):
```javascript
const iconsPage = figma.root.children.find(p => p.name === "UI Icons");
if (!iconsPage) return { error: "No 'UI Icons' page found in this file" };
```

Replace with:
```javascript
// Use the icon_page_name from APP_PLAN.md (default: "UI Icons")
const ICON_PAGE_NAME = "{{icon_page_name}}"; // Replace with actual value from APP_PLAN.md
const iconsPage = figma.root.children.find(p => p.name === ICON_PAGE_NAME);
if (!iconsPage) return { error: `No '${ICON_PAGE_NAME}' page found. Check branding.icon_page_name in APP_PLAN.md` };
```

Also update the section "Rules for Using Icons" (line 2235) to reference the dynamic page name:

Find:
```
5. **The "UI Icons" page is the only icon source** — never import icons from external libraries, URLs, or other Figma files unless the user explicitly asks
```

Replace with:
```
5. **The designated icon page (per APP_PLAN.md) is the only icon source** — never import icons from external libraries, URLs, or other Figma files unless the user explicitly asks
```

- [ ] **Step 2: Replace ALL remaining hardcoded "UI Icons" references in Section 22**

In the same file, search for all remaining `"UI Icons"` references in Section 22 (lines 2155-2244) and replace them with the dynamic pattern. Specifically update:

- Line 2163: `Find the icon on the "UI Icons" page` → `Find the icon on the designated icon page (per APP_PLAN.md)`
- Line 2172: `Find the icon component on the "UI Icons" page` → `Find the icon component on the designated icon page`
- Line 2211: `list all icon components on the "UI Icons" page` → `list all icon components on the designated icon page`
- Line 2229: `### Rules for Using Icons from "UI Icons"` → `### Rules for Using Icons from the Project Icon Page`
- Line 2232: `If an icon doesn't exist on the "UI Icons" page` → `If an icon doesn't exist on the designated icon page`
- Line 2240: `add them to "UI Icons" first` → `add them to the icon page first`
- Line 2243: `add a new variant on the "UI Icons" page` → `add a new variant on the icon page`

- [ ] **Step 3: Add modern design patterns section**

After the existing Section 25 (Spacing & Sizing Enforcement, which ends around line 2822 — insert before the `---` divider preceding the Quick Decision Framework section). Note: Section 25 already exists — the new section is numbered **26**.

Also update the Table of Contents (around lines 20-48) to add an entry for the new section:
```
26. [Modern Design Patterns (2024-2026)](#26-modern-design-patterns-2024-2026)
```

Add the section content:

```markdown
## 26. Modern Design Patterns (2024-2026)

These patterns define what "current" looks like in mobile and web UI. Apply these by default when designing new screens.

### Anti-Crowding Rules

Crowded designs are the #1 reason screens look dated or unprofessional. Apply these rules strictly:

| Rule | Severity if Violated | Guidance |
|------|---------------------|----------|
| **Bottom sheet density** | 🔴 Critical | Max 2 content sections in a bottom sheet. If you need service types + driver list + navigation bar, split into a scrollable sheet or move secondary content to a separate view. |
| **Card information density** | 🟠 Major | Max 3 data points per compact card. Prioritize: name + primary metric as prominent, secondary info as subtle. Hide tertiary details (plate numbers, IDs) until drill-down/detail view. |
| **Map marker clustering** | 🟠 Major | When map markers overlap or cluster within 40px of each other, show a cluster indicator with count instead of individual markers. Declutter at zoom levels where icons touch. |
| **Section spacing** | 🔴 Critical | Minimum 24px between distinct content sections. Minimum 16px between items within a section. No content touching device edges — 16px minimum screen padding on all sides. |

### Visual Breathing Room

- Content cards: 16px internal padding minimum on all sides
- Between major sections: 24-32px gap
- Headers: clear separation from content below (16-24px)
- List items: 12-16px vertical padding
- Screen edges: 16px minimum horizontal padding (never 0)

### Progressive Disclosure

- Show essential info upfront, details on tap
- Expandable cards for secondary information
- Drill-down views for detailed data
- Peek-and-reveal patterns for preview → full view
- Never show everything at once on a single screen

### Subtle Elevation & Depth

- Prefer soft shadows: `0 2px 8px rgba(0,0,0,0.08)` for cards, `0 4px 12px rgba(0,0,0,0.08)` for elevated elements
- Avoid hard borders (1px solid #ccc) as the primary surface separation — use shadows or background contrast instead
- Layer surfaces with slight elevation differences
- Glass/blur effects for overlays and bottom sheets

### Typography Hierarchy

- Clear size jumps: heading (24px) / body (16px) / caption (12px)
- Use font weight contrast: Bold title, Regular body, Medium labels
- Don't rely on size alone — combine size + weight + color for hierarchy
- Minimum body text: 16px on mobile, 14px on desktop

### Rounded, Soft UI

- Cards: 12-16px corner radius
- Buttons: 8-12px corner radius
- Chips/tags: 20-24px corner radius (near-pill)
- Inputs: 8-12px corner radius
- Sharp corners (0-4px) look dated in modern mobile UI — avoid unless the brand tone is "Corporate" or "Sharp"

### Color Restraint

- 1 primary action color used for CTAs and key interactive elements
- 1 secondary color for accents and supporting elements
- Neutral backgrounds (white, light gray, off-white)
- Use color for meaning: status indicators, action states, alerts
- Avoid 3+ competing brand colors on a single screen
```

- [ ] **Step 4: Commit**

```bash
git add skills/design-system-toolkit/skills/01-mobile-web-ui-ux-design-guide.md
git commit -m "feat: add dynamic icon page + modern design patterns to skill 01"
```

---

### Task 10: Update skill 02 — stricter UX audit checks

**Files:**
- Modify: `skills/design-system-toolkit/skills/02-ux-flow-audit.md`

- [ ] **Step 1: Add new checks to Phase 2 (Screen-by-Screen)**

In `skills/design-system-toolkit/skills/02-ux-flow-audit.md`, find the Phase 2 section. After the "Consistency" check table (after check 2.15, around line 180), add:

```markdown

**Content Density (Anti-Crowding):**
| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 2.16 | Does any bottom sheet contain 3 or more distinct content sections? (🟠 Major if yes — split or make scrollable) | | |
| 2.17 | Does the screen show all information at once without visual hierarchy or progressive disclosure? (🟠 Major if yes — prioritize and layer) | | |
```

- [ ] **Step 2: Add new checks to Phase 7 (Cognitive Load)**

In the same file, find the Phase 7 "Cognitive Load Checks" table (around line 399-412). After check 7.10, add:

```markdown
| 7.11 | Is progressive disclosure used for data-dense screens? (🟡 Minor if not — consider expandable sections) | | |
| 7.12 | Does the user need to process 4+ distinct data categories simultaneously on a single screen? (🟠 Major if yes — group or paginate) | | |
```

- [ ] **Step 3: Add the new checks to the fix patterns tables**

In Phase 2's "Screen-Level Fix Patterns" table (around line 182-191), add:

```markdown
| Bottom sheet has 3+ sections | Split content: keep primary actions in sheet, move lists/secondary content to separate view or scrollable area |
| All information shown at once | Apply progressive disclosure: show summary → expand for details. Use cards, accordions, or tabs to layer information |
```

In Phase 7's "Clarity Fix Patterns" table (around line 428-437), add:

```markdown
| No progressive disclosure | Add expandable sections, "Show more" patterns, or drill-down views for detailed information |
| 4+ data categories on one screen | Group related data into tabs or sections. Show summary metrics with drill-down for details |
```

- [ ] **Step 4: Commit**

```bash
git add skills/design-system-toolkit/skills/02-ux-flow-audit.md
git commit -m "feat: add anti-crowding and progressive disclosure checks to UX audit"
```

---

### Task 11: Update skill 03 — stricter UI visual audit checks

**Files:**
- Modify: `skills/design-system-toolkit/skills/03-ui-visual-audit.md`

- [ ] **Step 1: Add new checks to Layer 1 (Spacing & Alignment)**

In `skills/design-system-toolkit/skills/03-ui-visual-audit.md`, find the "Grid & Spacing Checks" table in Layer 1 (around line 79-88). After check 1.6, add:

```markdown
| 1.7 | Is there at least 16px spacing between distinct content sections? (🔴 Critical if less — sections must breathe) | | |
| 1.8 | Does any content touch the device edges with 0px screen padding? (🔴 Critical — minimum 16px on all sides) | | |
```

- [ ] **Step 2: Add new checks to Layer 5 (Icons & Imagery)**

Find Layer 5's "Image Consistency" table (around line 350-359). After check 5.13 (the last image check, line 359), add:

```markdown
| 5.14 | On map screens, do markers/icons cluster within 40px without a grouping indicator? (🟠 Major — use cluster indicators) | | |
| 5.15 | Do compact cards display 4+ data points? (🟠 Major — max 3 data points per compact card, hide rest in detail view) | | |
```

- [ ] **Step 3: Add new check to Layer 6 (Surfaces & Elevation)**

Find the "Shadow & Border Consistency" table in Layer 6 (around line 388-394). After check 6.10, add:

```markdown
| 6.11 | Are hard borders (1px solid) used as the primary surface separation instead of subtle shadows? (🟡 Minor — prefer soft shadows for modern look) | | |
```

- [ ] **Step 4: Add new checks to Layer 8 (Modern Design Patterns)**

Find the "Modernity Checks" table in Layer 8 (around line 473-484). After check 8.10, add:

```markdown
| 8.11 | Is the corner radius under 8px on cards or buttons? (🟡 Minor — modern mobile UI uses 8-16px radius) | | |
| 8.12 | Are data-dense screens missing progressive disclosure patterns? (🟠 Major — show summary first, details on interaction) | | |
```

- [ ] **Step 5: Update the Layer 8 "Outdated vs Modern Reference" table**

Find the table at around line 488-504. Update the year reference on line 469:

Find: `Does the interface feel current (2024–2025+) or does it look like it was designed in 2016?`
Replace with: `Does the interface feel current (2024–2026+) or does it look like it was designed in 2016?`

- [ ] **Step 6: Commit**

```bash
git add skills/design-system-toolkit/skills/03-ui-visual-audit.md
git commit -m "feat: add anti-crowding and modern pattern checks to UI visual audit"
```

---

### Task 11.5: Update skill 04 — icon page integration + auto layout audit recommendation

**Files:**
- Modify: `skills/design-system-toolkit/skills/04-figma-to-code.md`

- [ ] **Step 1: Add icon page reference to skill 04**

In `skills/design-system-toolkit/skills/04-figma-to-code.md`, find the section that handles component building/instantiation. Add a note near the top of the Build Mode section:

```markdown
### Icon Page Integration

When building screens that include icons:
1. Read `icon_page_name` from `project/APP_PLAN.md`
2. If set, use `get_design_context` to fetch available icons from that page
3. Match needed icons to available icons by name/purpose
4. Use the project's actual icon components — never substitute generic/emoji icons
5. If an icon doesn't exist, flag it as missing and suggest adding it
6. If `icon_page_name` is not set, ask the user which page contains their icons
```

- [ ] **Step 2: Add auto layout audit recommendation**

At the end of the Build Mode section in skill 04, add:

```markdown
### Post-Build Validation

After completing a build session, recommend to the user:
> "Build complete! Run `/design:figma-autolayout-audit` to validate the auto layout structure of what we just built."
```

- [ ] **Step 3: Commit**

```bash
git add skills/design-system-toolkit/skills/04-figma-to-code.md
git commit -m "feat: add icon page integration and auto layout audit recommendation to skill 04"
```

---

## Chunk 4: New Auto Layout Audit Skill & Cleanup

### Task 12: Create skill 05 — Figma Auto Layout Audit

**Files:**
- Create: `skills/design-system-toolkit/skills/05-figma-autolayout-audit.md`

- [ ] **Step 1: Write the full auto layout audit skill**

Create `skills/design-system-toolkit/skills/05-figma-autolayout-audit.md` with the following content:

```markdown
# Figma Auto Layout Audit — 6-Phase Inspection

> Validates Figma auto layout structure and best practices. Inspects layout direction,
> sizing modes, padding/spacing, alignment, nesting depth, and absolute positioning.
>
> **When to use:** After building frames in Figma, when troubleshooting layout issues,
> or as part of a design quality review.
>
> **Input:** A Figma URL or frame selection to audit.
> **Output:** Structured report with severity-tagged findings and fix recommendations.

---

## How to Run This Audit

1. **Get the Figma frame to audit**: Ask the user for a Figma URL or frame description
2. **Use Figma MCP tools** to inspect the frame structure:
   - `get_design_context` for overall structure
   - `get_metadata` for detailed node properties (auto layout settings, sizing modes, padding)
3. **Run all 6 phases** in order
4. **Present findings** grouped by phase, tagged with severity
5. **Provide fix recommendations** with specific Figma property values to change

---

## Severity Ratings

| Severity | Meaning | Action Required |
|----------|---------|-----------------|
| 🔴 Critical | Layout breaks on different screen sizes | Must fix before handoff |
| 🟠 Major | Wasted space, wrong sizing mode, visual issues | Should fix before handoff |
| 🟡 Minor | Off-grid spacing, unnecessary nesting | Fix when convenient |
| 🔵 Suggestion | Could simplify structure | Optional improvement |

---

## Phase 1: Layout Direction & Wrapping

Check that auto layout direction matches the content flow.

### Checks

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 1.1 | Is auto layout direction (H or V) correct for the content flow? | | |
| 1.2 | Are horizontal items that should stack vertically on mobile set to WRAP? | | |
| 1.3 | Are tag/chip/badge groups using WRAP for overflow? | | |
| 1.4 | Is the main page structure vertical (V) with horizontal (H) rows inside? | | |
| 1.5 | Are form fields stacked vertically, not horizontally? (Unless inline label + input) | | |

### Common Issues

| Issue | Symptom | Fix |
|-------|---------|-----|
| Horizontal layout for vertical content | Items overflow off-screen or overlap | Change auto layout direction to Vertical |
| Missing WRAP on tag groups | Tags overflow in a single line, get cut off | Enable WRAP on the auto layout container |
| Vertical layout for a row of buttons | Buttons stack when they should be side-by-side | Change to Horizontal auto layout |
| No auto layout at all | Elements positioned absolutely, break on resize | Add auto layout to the parent frame |

---

## Phase 2: Sizing Modes (HUG vs FILL vs FIXED)

The most impactful check. Wrong sizing modes cause the majority of layout bugs.

### Checks

| # | Check | Severity | Pass/Fail | Notes |
|---|-------|----------|-----------|-------|
| 2.1 | Is the screen content area (between header and footer) set to FILL height? | 🔴 Critical | | Footer floats mid-screen if HUG |
| 2.2 | Are buttons set to HUG height? (Not fixed height with wasted space) | 🔴 Critical | | |
| 2.3 | Are cards set to HUG height? (Must grow/shrink with content) | 🟠 Major | | |
| 2.4 | Are text containers set to HUG height? (Text must not clip or leave empty space) | 🟠 Major | | |
| 2.5 | Are input fields FIXED height? (Standard: 48px — inputs should NOT be HUG) | 🟡 Minor | | |
| 2.6 | Are icons and avatars FIXED square dimensions? (24×24, 40×40 — never HUG) | 🟡 Minor | | |
| 2.7 | Are full-width elements (buttons, inputs, cards in a list) set to FILL width? | 🔴 Critical | | Fixed width breaks on different screen sizes |
| 2.8 | Is the parent FILL but ALL children FIXED width? | 🟠 Major | | Wasted space on larger screens |
| 2.9 | Are navigation bars set to FILL width? | 🔴 Critical | | Must stretch to screen width |
| 2.10 | Are modals/bottom sheets set to appropriate width? (FILL with max-width, or FIXED centered) | 🟠 Major | | |

### Sizing Mode Reference

| Element Type | Width | Height | Why |
|-------------|-------|--------|-----|
| Screen frame | FIXED (device width) | FIXED (device height) | Represents the device |
| Content area | FILL | FILL | Stretches to fill between header/footer |
| Header / Footer | FILL | HUG | Full width, height from content |
| Buttons | FILL or HUG | HUG | Width depends on context, height from text |
| Cards | FILL | HUG | Width fills container, height from content |
| Text blocks | FILL | HUG | Width fills container, height from text |
| Input fields | FILL | FIXED (48px) | Width fills container, height standardized |
| Icons | FIXED | FIXED | Always square, never resizes |
| Avatars | FIXED | FIXED | Always square or circular |
| Navigation tabs | FILL (per tab) | HUG | Each tab fills equal space |

---

## Phase 3: Padding & Spacing Consistency

All values should follow the 8px grid: 2, 4, 8, 12, 16, 20, 24, 32, 40, 48, 64.

### Checks

| # | Check | Severity | Pass/Fail | Notes |
|---|-------|----------|-----------|-------|
| 3.1 | Are ALL padding values on the 8px grid? (No 13px, 17px, 22px, etc.) | 🟡 Minor | | |
| 3.2 | Do buttons have ≥16px horizontal, ≥12px vertical padding? | 🔴 Critical | | Zero padding = text touches edge |
| 3.3 | Do cards have ≥12px padding on all sides? | 🔴 Critical | | |
| 3.4 | Do inputs have ≥12px horizontal padding? | 🟠 Major | | |
| 3.5 | Does the screen have ≥16px left/right padding? | 🔴 Critical | | Content touching device edge |
| 3.6 | Are gaps between sibling elements consistent? | 🟡 Minor | | |
| 3.7 | Are gaps between major sections ≥24px? | 🟠 Major | | Sections need breathing room |
| 3.8 | Is the same padding used for all instances of the same component type? | 🟠 Major | | Button A padding ≠ Button B padding |
| 3.9 | Are bottom sheets using extra bottom padding for device safe area? | 🟠 Major | | Content hidden behind home indicator |

---

## Phase 4: Alignment & Distribution

### Checks

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 4.1 | Is the primary alignment (top, center, bottom) appropriate for the content? | | |
| 4.2 | Is the counter alignment (cross-axis) correct? (e.g., text left-aligned in a centered container) | | |
| 4.3 | Is space-between used only when there are 2+ children? (Space-between with 1 child = no effect, misleading) | | |
| 4.4 | Are sibling frames using consistent alignment? (All cards in a list should align the same way) | | |
| 4.5 | Is center alignment used intentionally? (Not as a default when left-align would be more readable) | | |
| 4.6 | Are text elements left-aligned for readability? (Center-aligned body text is hard to read) | | |

### Common Issues

| Issue | Symptom | Fix |
|-------|---------|-----|
| Centered body text | Long paragraphs are hard to scan | Use left alignment for body text |
| Space-between with 1 child | Single element sits at the top with empty space | Change to start alignment or add items |
| Mismatched alignment across cards | Cards in a list have different text alignment | Set consistent alignment across all instances |
| Center-aligned in left-aligned parent | Element looks off-center or misplaced | Match child alignment to parent flow |

---

## Phase 5: Nesting Depth & Structure

Deep nesting makes Figma files hard to maintain and can cause performance issues.

### Checks

| # | Check | Severity | Pass/Fail | Notes |
|---|-------|----------|-----------|-------|
| 5.1 | Is auto layout nesting deeper than 5 levels? | 🟡 Minor | | Simplify structure |
| 5.2 | Are there unnecessary wrapper frames? (Single child inside auto layout parent) | 🟡 Minor | | Remove redundant wrappers |
| 5.3 | Are there frames WITHOUT auto layout that contain multiple children? | 🟠 Major | | Add auto layout for responsive behavior |
| 5.4 | Is the component structure flat enough for variant swapping? | 🔵 Suggestion | | |
| 5.5 | Are similarly structured elements using the same nesting pattern? | 🟡 Minor | | Consistency aids maintenance |

### Simplification Patterns

| Before | After | Why |
|--------|-------|-----|
| Frame > Frame > Frame > Text | Frame > Text | Remove wrappers that don't add padding or alignment |
| Frame (no auto layout) with 3 children | Frame (auto layout V, gap 8) with 3 children | Auto layout ensures consistent spacing |
| 7-level deep card component | 3-4 level deep card | Flatten by combining padding layers |

---

## Phase 6: Absolute Positioning Audit

Absolute positioning should be the exception, not the rule. Most elements should be in auto layout flow.

### Checks

| # | Check | Severity | Pass/Fail | Notes |
|---|-------|----------|-----------|-------|
| 6.1 | Are there elements using absolute position that could be in auto layout flow? | 🟠 Major | | Move to auto layout |
| 6.2 | Do absolute-positioned elements have proper constraints? (Pin to edges/corners) | 🔴 Critical | | Without constraints, elements break on resize |
| 6.3 | Are overlapping elements intentional? (Badge on avatar = OK, text over text = bug) | 🟠 Major | | |
| 6.4 | Are floating elements (FAB, toast, tooltip) using absolute position correctly? | 🔵 Suggestion | | These should be absolute |
| 6.5 | Would any absolute-positioned element break on a different screen size? | 🔴 Critical | | Test at 375px and 430px |

### When Absolute Positioning Is OK

| Element | Absolute OK? | Why |
|---------|-------------|-----|
| FAB (Floating Action Button) | Yes | Floats over content, pinned to corner |
| Badge/notification dot | Yes | Overlaps parent element intentionally |
| Toast/snackbar | Yes | Temporary overlay, not in page flow |
| Tooltip | Yes | Appears relative to trigger element |
| Everything else | No | Should be in auto layout flow |

---

## Report Template

After running all 6 phases, present findings in this format:

```
## Auto Layout Audit Report

**Frame audited:** [Frame name or Figma URL]
**Date:** [Date]

### Summary
| Severity | Count |
|----------|-------|
| 🔴 Critical | [X] |
| 🟠 Major | [X] |
| 🟡 Minor | [X] |
| 🔵 Suggestion | [X] |

### Critical Findings (Fix Immediately)
| # | Phase | Finding | Current Value | Recommended Value |
|---|-------|---------|---------------|-------------------|
| 1 | Phase 2 | Content area is HUG height | Height: HUG | Height: FILL |

### Major Findings (Fix Before Handoff)
[Same table format]

### Minor Findings & Suggestions
[Same table format]

### Recommendation
After fixing: run `/design:figma-autolayout-audit` again to verify all issues resolved.
```
```

- [ ] **Step 2: Commit**

```bash
git add skills/design-system-toolkit/skills/05-figma-autolayout-audit.md
git commit -m "feat: create skill 05 — Figma auto layout audit with 6-phase inspection"
```

---

### Task 13: Delete agents.md

**Files:**
- Delete: `skills/design-system-toolkit/agents.md`

> **IMPORTANT:** This is the LAST step. Only do this after Tasks 4-7 are verified working.

- [ ] **Step 1: Verify all replacement files exist and are complete**

Check that these files exist and have content:
- `skills/design-system-toolkit/discovery.md`
- `skills/design-system-toolkit/router.md`
- `skills/design-system-toolkit/governance.md`

Read each file to verify it has the expected content.

- [ ] **Step 2: Verify no remaining references to agents.md**

Search the codebase for any references to `agents.md`:

```bash
grep -r "agents.md" /Users/ruelitoytac/Documents/Development/ytac-designs/ --include="*.md" -l
```

Update ALL files that still reference `agents.md` using this mapping:

| File | Line(s) | Current Reference | Replace With |
|------|---------|-------------------|-------------|
| `skills/01-mobile-web-ui-ux-design-guide.md` | ~1470, 1474, 1475 | "agents.md Step 3" | "governance.md" |
| `skills/04-figma-to-code.md` | ~75 | "agents.md" | "governance.md" (for init) or "router.md" (for routing) |
| `project/APP_PLAN.md` | ~3 | "generated by... `agents.md`" | "generated by the Design System Toolkit (`discovery.md`)" |
| `README.md` | ~176, 197, 228 | "agents.md" | "discovery.md", "router.md", or "governance.md" as appropriate |

- [ ] **Step 3: Delete agents.md**

```bash
rm skills/design-system-toolkit/agents.md
```

- [ ] **Step 4: Commit**

```bash
git add -A
git commit -m "refactor: remove agents.md — content fully distributed to discovery.md, router.md, governance.md"
```

---

### Task 14: Update plugin version

**Files:**
- Modify: `.claude-plugin/plugin.json`

- [ ] **Step 1: Bump version to 2.0.0**

This is a major version bump because:
- New sub-commands change the user-facing interface
- agents.md is removed (breaking change for any existing references)
- New skill file added (05)

Update `.claude-plugin/plugin.json`:

```json
{
  "name": "ruel-system-design-ui-ux-figma",
  "description": "Turns Claude into a senior UI/UX design consultant — covering design systems, UX flow auditing, visual QA, Figma auto layout auditing, Figma automation, and design-to-code export. Use /design for the main menu, or sub-commands like /design:ux-audit, /design:ui-audit, /design:screen for direct access.",
  "version": "2.0.0"
}
```

- [ ] **Step 2: Commit**

```bash
git add .claude-plugin/plugin.json
git commit -m "chore: bump version to 2.0.0 for modular architecture redesign"
```

---

## Execution Order Summary

| Task | Description | Dependencies |
|------|-------------|-------------|
| 1 | Validate colon naming | None |
| 2 | Create 6 sub-command files | Task 1 (naming format) |
| 4 | Create discovery.md | None |
| 5 | Create router.md | None |
| 6 | Create governance.md | None |
| 3 | Update base /design command | Tasks 2, 4, 5, 6 (references router.md + discovery.md) |
| 7 | Update SKILL.md | Tasks 4, 5, 6 |
| 8 | Update APP_PLAN.md template | None |
| 9 | Update skill 01 (icons + modern patterns) | Task 8 |
| 10 | Update skill 02 (stricter UX checks) | None |
| 11 | Update skill 03 (stricter UI checks) | None |
| 11.5 | Update skill 04 (icon page + audit recommendation) | Task 8 |
| 12 | Create skill 05 (auto layout audit) | None |
| 13 | Delete agents.md + update all references | Tasks 3, 4, 5, 6, 7 |
| 14 | Update plugin version | All tasks |

**Parallelizable groups:**
- Group A: Tasks 1-2 (commands) in parallel with Tasks 4-6 (module split) and Tasks 8-12 (skill updates)
- Group B: Tasks 3, 7 (depend on Group A: 4, 5, 6)
- Task 13 depends on Group B
- Task 14 is last
