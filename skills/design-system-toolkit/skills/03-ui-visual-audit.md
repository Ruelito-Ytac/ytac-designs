---
name: ui-visual-auditor
description: >
  Audits the visual quality, consistency, and modern design standards of UI screens for web and mobile apps.
  Use this skill to check if an interface looks broken, outdated, inconsistent, or unpolished. Trigger when
  the user says "check this UI", "audit my design", "does this look right", "is this modern", "review my
  screens", "check consistency", "visual QA", "design QA", "UI review", "polish this", "something looks off",
  "fix the design", "make it look better", "check my components", "visual audit", or shares screens and asks
  for design quality feedback. This skill runs a structured visual inspection across spacing, typography, color,
  components, alignment, responsiveness, and modern design patterns — then provides actionable fixes.

  IMPORTANT: Read project/APP_PLAN.md for project context and skills/01-mobile-web-ui-ux-design-guide.md for the design system principles
  this audit references. If unavailable, the audit criteria below are self-contained.
---

# UI Visual Auditor

> **Purpose**: Catch every visual inconsistency, broken layout, outdated pattern, and polish issue
> before users or developers see it. This is design QA — a pixel-level inspection of your interface.

---

## Table of Contents

1. [How to Run a UI Audit](#1-how-to-run-a-ui-audit)
2. [Layer 1: Spacing & Alignment](#2-layer-1-spacing--alignment)
3. [Layer 2: Typography](#3-layer-2-typography)
4. [Layer 3: Color & Contrast](#4-layer-3-color--contrast)
5. [Layer 4: Component Consistency](#5-layer-4-component-consistency)
6. [Layer 5: Icons & Imagery](#6-layer-5-icons--imagery)
7. [Layer 6: Elevation, Borders & Surfaces](#7-layer-6-elevation-borders--surfaces)
8. [Layer 7: Responsive & Breakpoint Check](#8-layer-7-responsive--breakpoint-check)
9. [Layer 8: Modern Design Patterns](#9-layer-8-modern-design-patterns)
10. [Layer 9: Dark Mode Integrity](#10-layer-9-dark-mode-integrity)
11. [Layer 10: Motion & Transitions](#11-layer-10-motion--transitions)
12. [Layer 11: Design System Compliance](#12-layer-11-design-system-compliance)
13. [Visual Severity Rating](#13-visual-severity-rating)
14. [Fix Recommendation Framework](#14-fix-recommendation-framework)
15. [UI Audit Report Template](#15-ui-audit-report-template)
16. [The "Outdated vs Modern" Reference](#16-the-outdated-vs-modern-reference)
17. [Re-Audit After Fixes](#17-re-audit-after-fixes)

---

## 1. How to Run a UI Audit

### What You Need
- The **screens / frames** to audit (Figma frames, screenshots, or live app)
- The **design system / style guide** if one exists (tokens, components, variables)
- The **target platforms** — Mobile, Web, or both
- Whether **Dark Mode** should be audited too

### Audit Approach

Inspect each screen layer by layer. This order catches issues from structural (spacing) down to polish (motion):

```
Layer 1:  Spacing & Alignment      → Is the grid consistent? Is everything aligned?
Layer 2:  Typography               → Is the type scale correct? Is text readable?
Layer 3:  Color & Contrast         → Are tokens used? Does contrast pass WCAG?
Layer 4:  Component Consistency    → Are components from the system? Are states complete?
Layer 5:  Icons & Imagery          → Consistent style? Proper sizing? Fallbacks?
Layer 6:  Surfaces & Elevation     → Shadows, borders, layers — consistent depth?
Layer 7:  Responsive Breakpoints   → Does it hold together at all screen sizes?
Layer 8:  Modern Design Patterns   → Does it feel current or dated?
Layer 9:  Dark Mode Integrity      → Not just inverted — properly designed?
Layer 10: Motion & Transitions     → Appropriate, smooth, purposeful?
Layer 11: Design System Compliance → Everything from the system, nothing rogue?
```

Log every finding with a severity rating, then compile into the report template.

---

## 2. Layer 1: Spacing & Alignment

The fastest way to spot an unpolished design is inconsistent spacing. This layer catches it all.

### Grid & Spacing Checks

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 1.1 | Is an 8px spacing grid used consistently? (4px for fine adjustments) | | |
| 1.2 | Are page margins consistent? (16px mobile, 24px tablet, 32–64px desktop) | | |
| 1.3 | Is internal padding consistent within similar components? (All cards same padding) | | |
| 1.4 | Are gaps between sections consistent? (Same spacing between all major sections) | | |
| 1.5 | Are gaps between items within a group consistent? | | |
| 1.6 | Is spacing proportional to hierarchy? (More space between major sections, less within groups) | | |
| 1.7 | Is there at least 16px spacing between distinct content sections? (🔴 Critical if less — sections must breathe) | | |
| 1.8 | Does any content touch the device edges with 0px screen padding? (🔴 Critical — minimum 16px on all sides) | | |

### Internal Padding Checks (🔴 Zero-Padding = Critical)

These checks catch the most common spacing bug: elements with missing or zero internal padding. Reference 01-guide §23 for the full minimum padding table.

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 1.13 | Do ALL buttons have ≥16px horizontal padding and ≥12px vertical padding? (No text touching edges) | | |
| 1.14 | Do ALL text inputs have ≥12px horizontal padding? (Placeholder text not touching left edge) | | |
| 1.15 | Do ALL cards have ≥12px padding on all sides? (Content doesn't touch card border/edge) | | |
| 1.16 | Do ALL list items have ≥16px horizontal padding? (Content doesn't touch screen edge) | | |
| 1.17 | Does the screen content area have ≥16px left AND right padding? (Content never touches device edge) | | |
| 1.18 | Do ALL modals/dialogs have ≥20px padding on all sides? | | |
| 1.19 | Do ALL bottom sheets have ≥16px horizontal padding and extra bottom padding for safe area? | | |
| 1.20 | Do ALL chips/tags/badges have ≥8px horizontal padding? | | |
| 1.21 | Are padding values identical for all instances of the same element type? (Button A padding = Button B padding) | | |
| 1.22 | Are all padding values on the spacing token scale? (2/4/8/12/16/20/24/32/48/64 — no 13px, 17px, 22px) | | |

**Severity:**
- Any button, input, card, or list item with 0px horizontal padding → 🔴 Critical
- Screen content area with 0px side padding → 🔴 Critical (content touches device edge)
- Inconsistent padding between same element types → 🟠 Major
- Off-grid padding values → 🟡 Minor

### Sizing Mode Checks (HUG vs FIXED vs FILL)

These checks catch the second most common spacing bug: wrong sizing mode causing broken layouts.

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 1.23 | Are ALL buttons set to HUG height? (Never fixed height unless the entire row is standardized) | | |
| 1.24 | Are ALL cards set to HUG height? (Cards must grow/shrink with content) | | |
| 1.25 | Are ALL text containers set to HUG height? (Text must never clip or leave empty space) | | |
| 1.26 | Is the content area (between header and footer) set to FILL height? (Footer must be pushed to bottom) | | |
| 1.27 | Are ALL input fields a FIXED, consistent height? (48px standard — inputs should NOT be HUG) | | |
| 1.28 | Are ALL icons and avatars FIXED square dimensions? (24×24, 40×40 — never HUG) | | |
| 1.29 | Are all items in the SAME list the SAME sizing mode? (Not some HUG, some FIXED) | | |
| 1.30 | Do any elements have a fixed height with visible empty space inside? (Content much shorter than container) | | |
| 1.31 | Do any elements have content clipped/overflowing due to a fixed height that's too small? | | |

**Severity:**
- Button or card at fixed height (100px) with content only using 48px → 🔴 Critical (visually broken)
- Content area at HUG instead of FILL (footer floats mid-screen) → 🔴 Critical
- Text container at fixed height (text clips or wastes space) → 🟠 Major
- Input fields at varying heights → 🟡 Minor
- Inconsistent sizing mode within the same element type → 🟠 Major

### Alignment Checks

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 1.32 | Do all elements align to the layout grid? (Figma columns: 4 mobile, 8 tablet, 12 desktop) | | |
| 1.33 | Are left edges aligned consistently across stacked elements? | | |
| 1.34 | Are text baselines aligned across elements in the same row? | | |
| 1.35 | Are icons vertically centered with adjacent text? | | |
| 1.36 | Are buttons and inputs the same width within a given context? | | |
| 1.37 | Is center alignment used sparingly? (Only for short labels, headers, or hero text) | | |

### Common Spacing Defects

| Defect | What It Looks Like | Fix |
|--------|-------------------|-----|
| Zero-padding button | Text touches or nearly touches the button edge — looks broken | Add ≥16px horizontal, ≥12px vertical padding. Set auto layout padding on the button frame. |
| Zero-margin screen | Content runs right up to the device edge on left/right | Set ≥16px paddingLeft + paddingRight on the screen content frame |
| Inconsistent padding | Card A has 16px padding, Card B has 20px, Card C has 12px | Standardize: pick one value from the spacing token scale and apply to ALL cards |
| Fixed-height button | Button stuck at e.g. 100px with only 14px text inside — huge empty space | Set button to HUG height. Height = text line-height + vertical padding. |
| Fixed-height card | Short content = wasted space; long content = clipped text | Set card to HUG height. Card grows/shrinks with its content. |
| Content area not FILL | Footer floats in the middle of the screen when content is short | Set content area (scroll region) to FILL height. It pushes footer to bottom. |
| Uneven margins | Left margin is 16px but right margin is 14px | Align both to the same token value (16px) |
| Cramped layout | Elements feel squeezed together, no breathing room | Increase spacing between sections to at least 24–32px |
| Floating elements | Something sits at an odd position, not snapped to the grid | Snap to nearest grid line, verify auto layout is applied |
| Mixed spacing values | 13px here, 17px there, 22px somewhere else | Replace all with token-scale values: 4, 8, 12, 16, 20, 24, 32 |
| Misaligned items in a row | Text and icon next to each other, but icon is 2px too high | Set the row to auto layout with center vertical alignment |
| Doubled-up padding | Card has 16px padding, inner section also has 16px padding = 32px from edge | Remove inner section padding or set it to 0 |

### How to Inspect in Figma
- Select two elements → Hold **Option/Alt** to see spacing between them
- Turn on **Layout Grids** on frames to verify column alignment
- Use the **Figma "Inspect" panel** to check exact padding and margins
- Run the spacing detection script from 01-guide §23 via `figma_execute` to auto-detect zero-padding, off-grid values, and wrong sizing modes
- Plugins like **"Design Lint"** can auto-detect spacing violations

---

## 3. Layer 2: Typography

### Type Scale Compliance

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 2.1 | Is a defined type scale used? (No arbitrary font sizes) | | |
| 2.2 | Is body text at least 16px on mobile? | | |
| 2.3 | Is NO text smaller than 12px anywhere? | | |
| 2.4 | Are line heights set properly? (1.4–1.6× for body, 1.1–1.3× for headings) | | |
| 2.5 | Is paragraph/text block max width between 45–75 characters? | | |
| 2.6 | Is paragraph spacing consistent? (At least 1.5× line height between paragraphs) | | |

### Font Usage

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 2.7 | Are at most 2 font families used across the app? | | |
| 2.8 | Are at most 3 font weights used per screen? (Regular, Medium/Semibold, Bold) | | |
| 2.9 | Are all text elements using Figma Text Styles (not manually styled)? | | |
| 2.10 | Is ALL CAPS used sparingly? (Only labels, overlines, buttons — never full sentences) | | |
| 2.11 | Is italic avoided for body text? | | |
| 2.12 | Is letter-spacing intentional? (Slightly tracked for uppercase overlines, normal for body) | | |

### Hierarchy Clarity

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 2.13 | Can you distinguish H1, H2, H3, Body, Caption at a glance? | | |
| 2.14 | Is there enough size difference between heading levels? (At least 2–4px steps) | | |
| 2.15 | Is hierarchy created through size + weight + color (not just one)? | | |
| 2.16 | Do headings stand out clearly from body text? | | |
| 2.17 | Is secondary/helper text visually muted compared to primary text? | | |
| 2.18 | Do timestamps, metadata, and captions look distinctly lower-priority? | | |

### Typography Defect Catalog

| Defect | Visual Symptom | Fix |
|--------|---------------|-----|
| Arbitrary font sizes | 15px here, 17px there, 19px somewhere else | Align all to defined type scale tokens |
| Flat hierarchy | Everything looks the same weight and size | Increase contrast between levels: bigger/bolder headings, muted secondary text |
| Text too small on mobile | Body text at 14px or below | Increase to 16px minimum for body text |
| Line height too tight | Lines of text look squeezed, hard to read | Set body to 1.5× (e.g., 16px font → 24px line height) |
| Text runs too wide | Lines stretch edge-to-edge on desktop | Cap text containers at ~65ch or 560–640px max-width |
| Inconsistent text styles | Same content type uses different fonts or sizes across screens | Replace all with the correct Figma Text Style |
| Orphaned words | Single word sitting alone on the last line of a heading | Adjust width, add a soft break, or rewrite |

---

## 4. Layer 3: Color & Contrast

### Token Usage

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 3.1 | Are all colors from the design system? (No rogue hex values) | | |
| 3.2 | Are colors applied via Figma Variables / Styles (not hardcoded per element)? | | |
| 3.3 | Are semantic tokens used correctly? (Success = green, Error = red, not swapped) | | |
| 3.4 | Is the accent/brand color used consistently for primary actions? | | |
| 3.5 | Are neutral/background tones creating clear surface separation? | | |
| 3.6 | Is there a limited color palette? (Not a rainbow of arbitrary colors) | | |

### Contrast Compliance (WCAG 2.2)

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 3.7 | Body text contrast ≥ 4.5:1 against background? | | |
| 3.8 | Large text (18px+ bold or 24px+ regular) contrast ≥ 3:1? | | |
| 3.9 | Icon/UI element contrast ≥ 3:1 against background? | | |
| 3.10 | Input field borders ≥ 3:1 against background? | | |
| 3.11 | Placeholder text readable? (Not too faint) | | |
| 3.12 | Text on images has a scrim/overlay ensuring readability? | | |
| 3.13 | Disabled elements are distinguishable from enabled but still readable? | | |
| 3.14 | Focus rings are clearly visible against all backgrounds? | | |

### Color Independence

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 3.15 | Error states use icon + text + color (not color alone)? | | |
| 3.16 | Success states use icon + text + color (not color alone)? | | |
| 3.17 | Status indicators (online/offline, active/inactive) don't rely only on color? | | |
| 3.18 | Charts and data visualizations work for colorblind users? (Pattern/label alternatives) | | |
| 3.19 | Selected/active items are distinguishable without color? (Border, weight, fill pattern) | | |

### Color Defect Catalog

| Defect | Visual Symptom | Fix |
|--------|---------------|-----|
| Hardcoded hex values | Element uses #3B82F6 directly instead of the "Accent/Primary" variable | Replace with the design system variable/token |
| Low contrast text | Light gray text on white, or mid-gray on slightly darker gray | Increase contrast to meet 4.5:1 (use a contrast checker plugin) |
| Contrast failure on images | White text on a light photo — unreadable in some areas | Add a dark gradient scrim behind text or use a solid card overlay |
| Inconsistent accent usage | Primary button is blue on one screen, green on another | Standardize to one accent color for all primary actions |
| Too many colors | 6+ distinct hues on one screen with no clear reason | Reduce to primary, secondary, accent + semantic states |
| Disabled state invisible | Disabled button is only slightly lighter — hard to tell it's disabled | Use 40–50% opacity AND a muted color shift |
| Color-only indicator | Red dot for errors, green dot for success — no icon or text | Add icon (⚠️ ✓ ✕) and text label alongside the color |

### Tools in Figma
- **Stark** plugin — Full accessibility suite with contrast checking
- **Contrast** plugin — Quick contrast ratio between two layers
- **Color Blind** plugin — Simulate different color vision deficiencies
- **Design Lint** — Flags colors not from your library

---

## 5. Layer 4: Component Consistency

### Component Usage

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 4.1 | Are all instances using design system components (not detached copies)? | | |
| 4.2 | Are component overrides appropriate? (Text and content changed, not structure) | | |
| 4.3 | Are no components detached just to make a one-off adjustment? | | |
| 4.4 | Do similar elements across screens use the SAME component? (Not look-alikes) | | |
| 4.5 | Are all component names following the naming convention? | | |

### Button Consistency

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 4.6 | Is there only ONE primary button style used across the app? | | |
| 4.7 | Is button hierarchy consistent? (Primary = filled, Secondary = outlined, Tertiary = text/ghost) | | |
| 4.8 | Are button heights consistent? (Same size variant used in the same context) | | |
| 4.9 | Are button corner radii identical across all instances? | | |
| 4.10 | Do buttons have consistent padding? (Horizontal padding same for all buttons of same size) | | |
| 4.11 | Are destructive buttons visually distinct? (Red/destructive color variant) | | |
| 4.12 | Are all button states designed? (Default, Hover, Pressed, Disabled, Loading) | | |

### Input Field Consistency

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 4.13 | Are all input fields the same height? | | |
| 4.14 | Are all input fields the same border style? (Not mixing bordered, underline, and filled) | | |
| 4.15 | Are input corner radii consistent? | | |
| 4.16 | Are labels positioned consistently? (All top-aligned OR all floating — not mixed) | | |
| 4.17 | Are error states styled identically across all inputs? | | |
| 4.18 | Is focus state consistent? (Same ring/border color and thickness) | | |

### Card & Container Consistency

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 4.19 | Do all cards have the same corner radius? | | |
| 4.20 | Do all cards have the same internal padding? | | |
| 4.21 | Do all cards use the same border or shadow treatment? | | |
| 4.22 | Is the card content order consistent? (Image → Title → Desc → Action) | | |
| 4.23 | Are card grid gaps uniform? | | |

### Component Defect Catalog

| Defect | Visual Symptom | Fix |
|--------|---------------|-----|
| Detached component | Element looks like a button but isn't an instance | Swap back to the component library instance |
| Mixed button styles | Some screens use rounded buttons, others use square | Pick one radius, update the component, propagate |
| Inconsistent input heights | Login form inputs are 48px, profile form inputs are 40px | Standardize to one height (48px recommended) |
| Missing states | Button exists but has no disabled or loading variant | Add variants to the component set in the library |
| Rogue component | A one-off card that doesn't match the system cards | Replace with system card component or extend the system |
| Mixed border styles | Cards have borders on Screen A, shadows on Screen B, nothing on Screen C | Pick one surface treatment and apply everywhere |

---

## 6. Layer 5: Icons & Imagery

### Icon Consistency

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 5.1 | Are all icons from the same icon set/library? | | |
| 5.2 | Is the icon style consistent? (All outlined OR all filled — not mixed randomly) | | |
| 5.3 | Are icon sizes consistent? (16px, 20px, 24px — from a defined size scale) | | |
| 5.4 | Are icons optically aligned with adjacent text? (Visually centered, not mathematically offset) | | |
| 5.5 | Do icons have sufficient color contrast against their background? (3:1 minimum) | | |
| 5.6 | Are icons meaningful? (Not decorative only — they communicate function) | | |
| 5.7 | Do icon-only buttons have a text label or tooltip accessible to screen readers? | | |

### Image Consistency

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 5.8 | Do all images have a consistent aspect ratio within the same context? (All cards same ratio) | | |
| 5.9 | Do images have a placeholder/fallback for loading or broken states? | | |
| 5.10 | Are image corners consistent? (All rounded same radius or all square) | | |
| 5.11 | Is image quality sufficient? (No blurry or pixelated images at display size) | | |
| 5.12 | Do avatars have a fallback? (Initials or generic icon when no photo) | | |
| 5.13 | Are illustrations/empty-state graphics consistent in style? | | |
| 5.14 | On map screens, do markers/icons cluster within 40px without a grouping indicator? (🟠 Major — use cluster indicators) | | |
| 5.15 | Do compact cards display 4+ data points? (🟠 Major — max 3 data points per compact card, hide rest in detail view) | | |

### Icon & Image Defect Catalog

| Defect | Visual Symptom | Fix |
|--------|---------------|-----|
| Mixed icon sets | Some icons are Phosphor, some Material, some custom | Standardize to one library (or a defined subset) |
| Mixed fill/outline | Tabs use filled icons for active but nav uses outlined for active | Define a rule: e.g., filled = active, outlined = inactive — apply everywhere |
| Icon size inconsistency | Search icon is 20px, settings icon is 24px, both in the same nav bar | Use the same size within the same context |
| Blurry images | Avatar or product image looks soft/pixelated | Replace with @2x resolution image or use vector/SVG |
| No image fallback | Broken image shows the browser broken-image icon | Design a gray placeholder with a subtle icon |
| Inconsistent avatar shape | Some avatars are circles, some are rounded squares | Pick one shape and apply to all avatars |

---

## 7. Layer 6: Elevation, Borders & Surfaces

### Surface Separation

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 6.1 | Is there a clear visual distinction between surface layers? (Background, card, overlay) | | |
| 6.2 | Is ONE method used for surface separation? (Shadows OR borders OR fills — not all three mixed) | | |
| 6.3 | Is the elevation system consistent? (Same shadow for same elevation level) | | |
| 6.4 | Are higher-priority surfaces raised above lower ones? (Modals above cards above background) | | |
| 6.5 | Are dividers/borders used sparingly? (Prefer spacing over divider lines when possible) | | |

### Shadow & Border Consistency

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 6.6 | Are shadow values from defined tokens? (sm, md, lg — not arbitrary) | | |
| 6.7 | Is shadow direction consistent? (All shadows cast downward, same angle) | | |
| 6.8 | Are border widths consistent? (1px for subtle, 2px for emphasis — not random) | | |
| 6.9 | Are border colors from the token palette? | | |
| 6.10 | Is corner radius consistent across components at the same level? | | |
| 6.11 | Are hard borders (1px solid) used as the primary surface separation instead of subtle shadows? (🟡 Minor — prefer soft shadows for modern look) | | |

### Corner Radius Consistency

Check that radius values follow a defined scale and match across similar elements:

| Element Type | Expected Radius | Actual | Match? |
|-------------|----------------|--------|--------|
| Buttons | [defined value] | | |
| Input fields | [defined value] | | |
| Cards | [defined value] | | |
| Modals / Bottom sheets | [defined value] | | |
| Avatars | Full circle or [defined value] | | |
| Tags / Chips / Badges | [defined value] | | |
| Tooltips | [defined value] | | |
| Images within cards | [defined value] | | |

**Rule of thumb**: Inner elements should have a smaller radius than their container. If a card has 16px radius, the image inside should have ~12px radius (container radius minus padding).

### Surface Defect Catalog

| Defect | Visual Symptom | Fix |
|--------|---------------|-----|
| Mixed separation methods | Some cards have shadows, some have borders, some have both | Pick ONE approach: shadow-based (modern, Material) or border-based (clean, flat) |
| Inconsistent shadows | Card A has a soft large shadow, Card B has a hard small shadow | Define 2–3 shadow tokens (sm, md, lg) and use only those |
| Too many borders | Every element has a visible border — feels "cagey" and dated | Remove unnecessary borders, use spacing and background contrast instead |
| Mismatched corner radii | Buttons are 8px, cards are 12px, inputs are 6px, tags are 16px | Define a radius scale (sm: 6, md: 8, lg: 12, xl: 16, full: 9999) and assign per element type |
| Inner radius doesn't nest | Card has 16px radius, inner image also has 16px — looks awkward | Inner radius = outer radius − padding (16 − 4 = 12px) |
| Flat surfaces with no hierarchy | Modal appears but doesn't look "above" the content behind it | Add overlay/scrim behind modal + shadow on the modal itself |

---

## 8. Layer 7: Responsive & Breakpoint Check

Verify the design holds at all screen sizes.

### Breakpoint Audit

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 7.1 | Does the layout work at 375px? (Smallest common phone) | | |
| 7.2 | Does the layout work at 393px? (Current common iPhone) | | |
| 7.3 | Does the layout work at 430px? (Large phone) | | |
| 7.4 | Does the layout adapt at 768px? (Tablet — not just stretched mobile) | | |
| 7.5 | Does the layout adapt at 1024px? (Small laptop) | | |
| 7.6 | Does the layout look good at 1440px? (Standard desktop) | | |
| 7.7 | Does the layout not break at 1920px+? (Large monitor — content shouldn't stretch infinitely) | | |

### Responsive Behavior

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 7.8 | Do columns increase from 1 (mobile) → 2–3 (tablet) → 3–4 (desktop)? | | |
| 7.9 | Does navigation change appropriately? (Bottom tabs on mobile, top bar or sidebar on desktop) | | |
| 7.10 | Do images scale proportionally? (Not stretched or cropped badly) | | |
| 7.11 | Is there no unintentional horizontal scrolling at any breakpoint? | | |
| 7.12 | Do text containers maintain readable line length? (Max ~65 characters on desktop) | | |
| 7.13 | Are touch targets still adequate at every size? | | |
| 7.14 | Does the content NOT just float in the center of a massive white space on large screens? | | |

### Responsive Defect Catalog

| Defect | Visual Symptom | Fix |
|--------|---------------|-----|
| Stretched mobile on tablet | Mobile single-column but now with massive margins on iPad | Adapt to 2-column grid or add a sidebar for tablet |
| Content stretches infinitely on desktop | Card grid stretches all the way to screen edges on 27" monitor | Add max-width container (1280–1440px) with centered layout |
| Overlapping elements | Two elements collide at a specific width between breakpoints | Use auto layout or flex-wrap to let elements reflow |
| Orphaned elements | One card alone on a row that should hold three | Use auto-fill grid or adjust minimum card widths |
| Navigation doesn't adapt | Bottom tab bar still visible on desktop, or hamburger on desktop | Switch nav patterns: bottom tabs → top bar or sidebar at desktop breakpoint |
| Tiny text on large screen | Text that was fine on mobile looks absurdly small on a 4K monitor | Use fluid type scale, or increase heading sizes at desktop breakpoints |

---

## 9. Layer 8: Modern Design Patterns

Does the interface feel current (2024–2026+) or does it look like it was designed in 2016?

### Modernity Checks

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 8.1 | Are rounded corners used consistently? (Soft, not sharp 90° everywhere) | | |
| 8.2 | Is there generous whitespace? (Not cramped wall-to-wall content) | | |
| 8.3 | Are shadows soft and subtle? (Not harsh drop shadows from 2012) | | |
| 8.4 | Is the color palette restrained? (2–3 tones + accent, not a rainbow) | | |
| 8.5 | Are interactions smooth and animated? (Not abrupt screen jumps) | | |
| 8.6 | Is the typography bold and clear? (Not tiny and thin everywhere) | | |
| 8.7 | Are surfaces layered with depth? (Cards float above background, modals above cards) | | |
| 8.8 | Is dark mode available and intentionally designed? | | |
| 8.9 | Are skeleton loading screens used instead of blank-then-pop? | | |
| 8.10 | Are form inputs large and touch-friendly? (Not tiny bordered rectangles) | | |
| 8.11 | Is the corner radius under 8px on cards or buttons? (🟡 Minor — modern mobile UI uses 8-16px radius) | | |
| 8.12 | Are data-dense screens missing progressive disclosure patterns? (🟠 Major — show summary first, details on interaction) | | |

### Outdated vs Modern Reference

| Element | ❌ Outdated Look | ✅ Modern Look |
|---------|-----------------|---------------|
| Buttons | Flat, small, sharp corners, gradient fills, inner shadow | Rounded (8–12px radius), 48px tall, solid fill, subtle hover elevation |
| Cards | Hard 1px border, no padding, sharp corners | Subtle shadow or light fill, 16–20px padding, 12–16px radius |
| Navigation | Top hamburger menu on mobile, tiny tabs | Bottom tab bar with icon + label, 56px tall, clear active state |
| Inputs | Tiny 32px height, underline only, no focus style | 48px height, bordered or filled, visible focus ring, floating label |
| Shadows | Hard, dark, offset (5px 5px 0 #000) | Soft, diffused, layered (0 4px 12px rgba(0,0,0,0.08)) |
| Typography | 12–13px body, thin weight, low contrast gray | 16px body, medium weight, high contrast, clear hierarchy |
| Spacing | Cramped, inconsistent, busy feeling | Generous, rhythmic, intentional breathing room |
| Colors | Flat gray everywhere, no depth, or too many bright colors | Layered neutrals with one strong accent, semantic color use |
| Icons | Mixed styles, inconsistent sizes, various line weights | Unified set, consistent size scale, same stroke weight |
| Loading | Blank page → content pops in, or spinning wheel | Skeleton shimmer → content fades in, smooth transitions |
| Empty states | Blank white area with no explanation | Illustration + headline + description + CTA button |
| Modals | Small floating box, no backdrop dim, no animation | Bottom sheet on mobile, centered modal on desktop, backdrop blur, smooth entry |
| Avatars | Square with sharp corners, no fallback | Circle or soft-rounded square, initials fallback, status dot indicator |
| Dividers | Thick visible lines between every element | Subtle thin lines only where needed, or spacing-based separation |
| Status indicators | Text-only ("Active") or color-only (green dot) | Pill badge with icon + text + color, or dot with tooltip |

---

## 10. Layer 9: Dark Mode Integrity

If the app has dark mode, audit it separately — it has its own failure modes.

### Dark Mode Checks

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 9.1 | Is the dark background NOT pure black (#000)? (Should be #0A0A0F to #1A1A2E) | | |
| 9.2 | Is the text NOT pure white (#FFF)? (Should be off-white #E1E1E6 to #F1F1F4) | | |
| 9.3 | Are accent colors adjusted for dark mode? (Lighter/desaturated, not same as light mode) | | |
| 9.4 | Are all text contrast ratios still passing? (Re-check on dark backgrounds) | | |
| 9.5 | Do shadows work? (On dark bg, shadows are invisible — use borders or lighter surfaces instead) | | |
| 9.6 | Is elevation shown through surface lightness? (Higher = lighter surface) | | |
| 9.7 | Are images not "blinding"? (Bright photos may need reduced brightness or dark overlay) | | |
| 9.8 | Are illustrations adapted for dark mode? (Not light-bg-only illustrations pasted on dark) | | |
| 9.9 | Are dividers and borders visible but not harsh? | | |
| 9.10 | Does the overall feel appear intentionally designed, NOT like a CSS filter invert? | | |

### Dark Mode Defect Catalog

| Defect | Visual Symptom | Fix |
|--------|---------------|-----|
| Pure black background | Feels like staring into a void, text "floats," OLED smearing | Use very dark gray (#0F0F15 to #1A1A2E) |
| Pure white text | Glaring, causes eye strain after prolonged use | Use off-white (#E1E1E6 to #F1F1F4) |
| Invisible shadows | Card shadows disappear against dark background | Replace shadows with subtle lighter borders (1px, rgba white 5–10%) |
| Same accent color | Blue accent looks different/harsh on dark background | Create a lighter, slightly desaturated variant for dark mode |
| Bright images shocking | Product photos or hero images are blindingly bright | Add subtle dark overlay (10–20% opacity) or reduce image brightness |
| Lost surface hierarchy | All surfaces look the same shade of dark | Create 3–4 dark surface levels: darkest bg, slightly lighter card, even lighter overlay |
| Borders too harsh | White or light borders are jarring on dark surfaces | Use very subtle borders: rgba(255,255,255,0.08) to rgba(255,255,255,0.15) |
| Illustrations break | Illustrations designed for light mode have white elements that disappear | Create dark-mode variants of illustrations, or add contained backgrounds |

---

## 11. Layer 10: Motion & Transitions

### Animation Checks

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 10.1 | Do screen transitions exist? (Not abrupt hard cuts between screens) | | |
| 10.2 | Is transition direction logical? (Right for deeper, left for back, up for modal, down for dismiss) | | |
| 10.3 | Are animations under 300ms? (Anything longer feels sluggish) | | |
| 10.4 | Is the same interaction animated the same way everywhere? (Consistent motion language) | | |
| 10.5 | Are loading animations smooth? (Skeleton shimmer, spinner, progress bar) | | |
| 10.6 | Is there a reduced-motion fallback? (For users with vestibular disorders) | | |
| 10.7 | Do animations serve a purpose? (Guiding attention, showing relationships, providing feedback) | | |
| 10.8 | Is there no gratuitous animation that slows down the user? | | |

### Motion Defect Catalog

| Defect | Visual Symptom | Fix |
|--------|---------------|-----|
| Hard cuts between screens | Screen A disappears, Screen B appears instantly — jarring | Add push/slide transition (200–300ms ease-out) |
| Wrong transition direction | Detail screen slides in from left (should be from right) | Match direction to hierarchy: deeper = left, back = right |
| Sluggish animation | Modal takes 800ms to appear, feels slow | Reduce to 200–300ms with appropriate easing |
| Inconsistent motion | Some modals slide up, others fade, others pop — no pattern | Define a motion system: modals always slide up, sheets always slide up, pages push |
| Distracting animation | Content bounces in with overshoot on every page load | Tone down to subtle fade or slide, save dramatic motion for special moments |
| No reduced motion | Animations play for all users including those sensitive to motion | Respect prefers-reduced-motion: simplify or remove animations |

---

## 12. Layer 11: Design System Compliance

### System Usage

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 11.1 | Are all colors from Figma Variables / Color Styles? (Zero hardcoded values) | | |
| 11.2 | Are all text elements using Figma Text Styles? | | |
| 11.3 | Are all components from the component library? (No detached or rogue elements) | | |
| 11.4 | Are spacing values from defined tokens? | | |
| 11.5 | Are shadow values from defined Effect Styles? | | |
| 11.6 | Are border radii from defined tokens? | | |
| 11.7 | Is the naming convention followed? (Layers, components, frames) | | |
| 11.8 | Are all frames using Auto Layout? (Not manual absolute positioning) | | |

### Library Health

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 11.9 | Are there any "missing" components? (Library component deleted or renamed) | | |
| 11.10 | Are component overrides reasonable? (Content changed, not structure hacked) | | |
| 11.11 | Are there unused styles or variables? (Bloat in the system) | | |
| 11.12 | Are variables organized in collections? (Primitives, Semantic, Component-level) | | |
| 11.13 | Do Light and Dark modes both exist in the variable structure? | | |

### How to Check in Figma
- **Selection Colors panel**: Select all elements → check if colors are linked to styles/variables
- **Design Lint plugin**: Scans for detached styles, rogue colors, missing components
- **Similayer plugin**: Find all elements that look alike but aren't using the same component
- **Instance Finder**: Find all component instances and check for overrides

### Token Architecture Audit

Verify the design system follows the 3-tier token structure. This is how mature systems (Polaris, Carbon, Material) are built:

**Tier 1 — Primitive Tokens (Raw Values)**
| Check | Pass/Fail | Notes |
|-------|-----------|-------|
| Color primitives defined (blue-500, gray-900, etc.) | | |
| Spacing primitives defined (4px, 8px, 12px, 16px, 24px, 32px, 48px) | | |
| Typography primitives defined (sizes, weights, families) | | |
| Primitives are NOT directly used in designs (only referenced by semantic tokens) | | |

**Tier 2 — Semantic Tokens (Meaning)**
| Check | Pass/Fail | Notes |
|-------|-----------|-------|
| Purpose-based color tokens exist (color-primary, color-error, color-surface, etc.) | | |
| Semantic tokens alias to primitives (not hardcoded values) | | |
| Light/Dark mode switching happens at THIS tier (not primitives, not components) | | |
| Text tokens exist (text-body, text-heading, text-caption, etc.) | | |
| Surface tokens exist (surface-default, surface-elevated, surface-sunken) | | |

**Tier 3 — Component Tokens (Scoped)**
| Check | Pass/Fail | Notes |
|-------|-----------|-------|
| Component-specific tokens exist for key components (button-bg, input-border, card-shadow) | | |
| Component tokens alias to semantic tokens (not primitives or hardcoded) | | |
| Overriding a semantic token cascades correctly to all component tokens | | |

**Token Health Score:**
```
Total tokens: ___
Properly aliased (tier 2 → tier 1): ___% 
Properly aliased (tier 3 → tier 2): ___%
Hardcoded values (not aliased): ___ ← These are technical debt
Unused tokens (defined but never applied): ___ ← Bloat, clean up
```

---

## 13. Visual Severity Rating

| Level | Label | Meaning | Examples |
|-------|-------|---------|---------|
| 🔴 | **Broken** | Something is visually wrong or unusable | Elements overlapping, text unreadable, contrast failure, broken images |
| 🟠 | **Inconsistent** | Works but doesn't match the system or other screens | Mixed button styles, different spacing on similar screens, rogue colors |
| 🟡 | **Unpolished** | Functional but looks amateur or outdated | Tight spacing, harsh shadows, missing transitions, no empty states |
| 🔵 | **Enhancement** | Opportunity to elevate from good to great | Better microinteractions, refined motion, illustration upgrades |

---

## 14. Fix Recommendation Framework

### Fix Template

```
FINDING:     [What's visually wrong — specific description]
SCREEN:      [Which screen(s) affected]
LAYER:       [Which audit layer caught this — Spacing, Typography, Color, etc.]
SEVERITY:    [🔴 Broken / 🟠 Inconsistent / 🟡 Unpolished / 🔵 Enhancement]
WHAT'S WRONG: [Visual description of the problem]
WHY IT MATTERS: [Impact on user perception or usability]
FIX:         [Specific, actionable design change with exact values]
TOKEN/STYLE: [Which design token or style to apply]
```

### Example Findings

**Example 1:**
```
FINDING:     Card corner radius inconsistency
SCREEN:      Home Feed, Search Results, Profile
LAYER:       Component Consistency (Layer 4) + Surfaces (Layer 6)
SEVERITY:    🟠 Inconsistent
WHAT'S WRONG: Cards on Home use 12px radius, Search uses 8px, Profile uses 16px
WHY IT MATTERS: Subtle but creates an "off" feeling — design looks uncoordinated
FIX:         Standardize all cards to 12px corner radius. Update the Card component
             in the library. Ensure all instances inherit from the master component.
TOKEN/STYLE: --radius-lg: 12px (applied to all Card variants)
```

**Example 2:**
```
FINDING:     Body text fails contrast in dark mode
SCREEN:      All screens in dark mode
LAYER:       Color & Contrast (Layer 3) + Dark Mode (Layer 9)
SEVERITY:    🔴 Broken
WHAT'S WRONG: Secondary body text (#6B6B80) on dark background (#0F0F15) = 3.2:1
             ratio. Fails WCAG AA requirement of 4.5:1.
WHY IT MATTERS: Text is hard to read in dark mode, especially in low light.
             Accessibility violation.
FIX:         Lighten Text/Secondary dark mode value from #6B6B80 to #9A9AB0
             (achieves 5.1:1 ratio). Update the Figma variable for Dark mode.
TOKEN/STYLE: Text/Secondary [Dark mode] → #9A9AB0
```

**Example 3:**
```
FINDING:     Navigation uses outdated hamburger-only pattern on mobile
SCREEN:      All mobile screens
LAYER:       Modern Design Patterns (Layer 8)
SEVERITY:    🟡 Unpolished (bordering on 🟠 Inconsistent with modern standards)
WHAT'S WRONG: Mobile app uses a top-left hamburger menu as the only navigation.
             All 5 main sections are hidden behind it.
WHY IT MATTERS: Hides primary navigation. Users don't explore features they can't see.
             Modern apps use bottom tab bars for primary navigation.
FIX:         Replace hamburger with a bottom tab bar containing the 5 main sections.
             Use icon + text label for each tab. 56px height, safe area padding.
             Move hamburger items (Settings, Help, About) to a Profile/More tab.
TOKEN/STYLE: Use Bottom Tab Bar component from design system
```

---

## 15. UI Audit Report Template

```
# UI Visual Audit Report
## App: [App Name]
## Date: [Audit Date]
## Auditor: [Name]
## Screens Reviewed: [Count and list]

---

## Summary Score

| Layer | Status | 🔴 | 🟠 | 🟡 | 🔵 |
|-------|--------|-----|-----|-----|-----|
| Spacing & Alignment | ✅ / ⚠️ / ❌ | 0 | 0 | 0 | 0 |
| Typography | ✅ / ⚠️ / ❌ | 0 | 0 | 0 | 0 |
| Color & Contrast | ✅ / ⚠️ / ❌ | 0 | 0 | 0 | 0 |
| Component Consistency | ✅ / ⚠️ / ❌ | 0 | 0 | 0 | 0 |
| Icons & Imagery | ✅ / ⚠️ / ❌ | 0 | 0 | 0 | 0 |
| Surfaces & Elevation | ✅ / ⚠️ / ❌ | 0 | 0 | 0 | 0 |
| Responsive | ✅ / ⚠️ / ❌ | 0 | 0 | 0 | 0 |
| Modern Patterns | ✅ / ⚠️ / ❌ | 0 | 0 | 0 | 0 |
| Dark Mode | ✅ / ⚠️ / ❌ / N/A | 0 | 0 | 0 | 0 |
| Motion | ✅ / ⚠️ / ❌ | 0 | 0 | 0 | 0 |
| Design System | ✅ / ⚠️ / ❌ | 0 | 0 | 0 | 0 |
| **TOTAL** | | **0** | **0** | **0** | **0** |

### Verdict
- ✅ **PASS** — 0 Broken, ≤ 3 Inconsistent
- ⚠️ **CONDITIONAL** — 0 Broken, 4–8 Inconsistent
- ❌ **FAIL** — Any Broken findings, or 9+ Inconsistent

---

## 🔴 Broken Issues (Fix Immediately)
[List with fix template]

## 🟠 Inconsistencies (Fix Before Handoff)
[List with fix template]

## 🟡 Polish Issues (Fix Next Iteration)
[List with fix template]

## 🔵 Enhancement Opportunities
[List with fix template]

---

## Fix Priority

### Immediate
1. [Fix — with exact tokens/values]
2. [Fix]

### Before Handoff
1. [Fix]
2. [Fix]

### Next Iteration
1. [Fix]
2. [Fix]

---

## Component Health Summary

| Component | Instances | Detached | Missing States | Issues |
|-----------|-----------|----------|----------------|--------|
| Button | [count] | [count] | [list] | [notes] |
| Input | [count] | [count] | [list] | [notes] |
| Card | [count] | [count] | [list] | [notes] |
| Nav Bar | [count] | [count] | [list] | [notes] |
| [etc.] | | | | |

---

## Token Compliance

| Token Type | Total Used | From System | Rogue/Hardcoded | Compliance % |
|-----------|-----------|-------------|-----------------|-------------|
| Colors | [n] | [n] | [n] | [%] |
| Text Styles | [n] | [n] | [n] | [%] |
| Spacing | [n] | [n] | [n] | [%] |
| Radii | [n] | [n] | [n] | [%] |
| Shadows | [n] | [n] | [n] | [%] |
```

---

## 16. The "Outdated vs Modern" Reference

Quick checklist to determine if a design feels current:

### Modern Design Fingerprint (2024–2026+)
- ✅ Generous whitespace and breathing room
- ✅ Soft, rounded corners (8–16px, not sharp 0px)
- ✅ Subtle, diffused shadows (not hard drop shadows)
- ✅ Bold typography with clear hierarchy
- ✅ Limited, intentional color palette with one strong accent
- ✅ Layered surfaces creating depth (background → card → overlay)
- ✅ Bottom tab navigation on mobile with icon + label
- ✅ Large, comfortable touch targets (48px+)
- ✅ Skeleton loading states
- ✅ Dark mode as a first-class experience
- ✅ Smooth, purposeful transitions between screens
- ✅ Empty states with illustration + action
- ✅ Consistent component usage from a design system
- ✅ Fluid, responsive layouts that adapt (not just shrink)
- ✅ One primary action clearly dominant per screen

### Dated Design Red Flags
- ❌ Sharp 0px corners on everything
- ❌ Hard, dark drop shadows
- ❌ Tiny text (12–13px body)
- ❌ Cramped spacing, content packed edge-to-edge
- ❌ Hamburger menu as sole mobile navigation
- ❌ Gradient fills on buttons (2010-era glass effect)
- ❌ Beveled or 3D button effects (skeuomorphism without purpose)
- ❌ Multiple bright colors competing for attention
- ❌ Visible thick borders around every element
- ❌ No loading states — content pops in abruptly
- ❌ No dark mode
- ❌ Desktop layout squeezed onto mobile (not redesigned)
- ❌ Inconsistent components (hand-made elements instead of system instances)
- ❌ Pure black and pure white with no intermediate tones
- ❌ Tiny inputs and buttons (under 36px tall)

---

## 17. Re-Audit After Fixes

### Quick Visual Re-check

| # | Check | Pass? |
|---|-------|-------|
| 1 | All 🔴 Broken findings resolved? | |
| 2 | All 🟠 Inconsistencies resolved or accepted? | |
| 3 | Fixes haven't introduced NEW visual issues? | |
| 4 | Spacing still consistent across all modified screens? | |
| 5 | Colors still using system tokens? | |
| 6 | Components still linked to library (not detached by fixes)? | |
| 7 | Dark mode still working after changes? | |
| 8 | Responsive behavior still correct at all breakpoints? | |
| 9 | All modified screens match the design system? | |
| 10 | Prototype still works end-to-end after changes? | |

### The Squint Test
Squint at your screen (or zoom out to 25% in Figma):
- Can you still see the visual hierarchy?
- Does one element stand out as the primary action?
- Do the screens look consistent with each other?
- Is the layout balanced?

If everything blurs into a uniform gray → the hierarchy needs work.
If one element pops clearly → the hierarchy is working.

### Cross-Screen Consistency Test
Place all screens in the flow side by side in Figma:
- Do they feel like they belong to the same app?
- Are spacing, typography, and color consistent across all?
- Does the navigation appear in the same position on every screen?
- Are similar elements (buttons, cards, inputs) visually identical?

If any screen "looks different" from the others, that's your next fix.
