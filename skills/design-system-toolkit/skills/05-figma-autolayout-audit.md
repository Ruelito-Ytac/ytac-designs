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
| 3.7 | Is the inner gap of grouped items smaller than the outer gap between groups? (Proximity clustering: inner < outer) | 🟠 Major | | Groups merge visually if inner = outer |
| 3.8 | Are gaps between major sections ≥24px? | 🟠 Major | | Sections need breathing room |
| 3.9 | Is the same padding used for all instances of the same component type? | 🟠 Major | | Button A padding ≠ Button B padding |
| 3.10 | Are bottom sheets using extra bottom padding for device safe area? | 🟠 Major | | Content hidden behind home indicator |

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
