---
name: figma-to-code-expert
description: >
  Figma production expert for building code-ready design files and converting Figma designs to clean,
  production-grade frontend code. Use this skill when the user says "build this in Figma", "create
  components in Figma", "set up auto layout", "fix my Figma file", "convert to code", "generate code
  from Figma", "export to React", "export to HTML", "make this responsive", "Figma to CSS",
  "Figma to Tailwind", "code this design", "implement this screen", "create a component set",
  "set up variants", "build the design system in Figma", or any task involving Figma file construction
  or design-to-code translation.

  IMPORTANT: Read project/APP_PLAN.md for project context first. This skill works with the Figma MCP
  tools (Figma Desktop Bridge + figma-console) to directly manipulate Figma files and with
  Figma:get_design_context to extract design data for code generation.
---

# Figma Production & Code Export Expert

> **Purpose**: Build production-ready Figma files using proper auto layout, styles, variables, and
> component architecture — then translate them into clean, maintainable frontend code.
>
> This skill has two modes:
> - **Build Mode** — Construct and fix Figma files (auto layout, components, styles, variables)
> - **Export Mode** — Convert Figma designs to production frontend code (React, HTML/CSS, Tailwind, Vue)

---

## Table of Contents

1. [Build Mode: Figma Production Rules](#1-build-mode-figma-production-rules)
2. [Auto Layout Mastery](#2-auto-layout-mastery)
3. [Component Construction](#3-component-construction)
4. [Variant & Property Architecture](#4-variant--property-architecture)
5. [Style & Variable Setup](#5-style--variable-setup)
6. [Figma File Repair & Cleanup](#6-figma-file-repair--cleanup)
7. [Flow Connectors & Annotations in Figma](#7-flow-connectors--annotations-in-figma)
8. [Export Mode: Design-to-Code Translation](#8-export-mode-design-to-code-translation)
9. [Screen-to-Code Workflow](#9-screen-to-code-workflow)
10. [Component-to-Code Workflow](#10-component-to-code-workflow)
11. [Token Export](#11-token-export)
12. [Code Quality Standards](#12-code-quality-standards)
13. [Framework-Specific Patterns](#13-framework-specific-patterns)
14. [Responsive Code from Figma](#14-responsive-code-from-figma)
15. [Figma MCP Tool Reference](#15-figma-mcp-tool-reference)

---

## 1. Build Mode: Figma Production Rules

### The Golden Rules

Every Figma file built or touched by this skill MUST follow these rules. No exceptions.

1. **100% Auto Layout** — Every frame from screen level to deepest child uses auto layout. Zero manual positioning.
2. **Zero Raw Values** — Every text layer references a Text Style. Every color fill references a Color Style or Variable. Every spacing value comes from the spacing scale.
3. **Component Everything** — If an element appears more than once, it's a component. If it has states, it's a variant set.
4. **Name Everything** — No "Frame 427", no "Rectangle 12". Every layer has a semantic name.
5. **Nest Intentionally** — Maximum 6 levels of nesting. Deeper = extract to component.
6. **No Spacer Frames** — Never use empty frames (`_sp`, `Spacer`, etc.) between siblings for spacing. Always use auto layout `gap` (`itemSpacing`) on the parent. If children need different gaps, group related items into sub-frames.
7. **No Placeholder Heights** — Every element's height must match its content (HUG). Only screen frames, images, icons, avatars, and dividers get fixed heights. An input field at 100px with 48px of content is a bug.

### Pre-Flight Check

Before building anything in Figma, verify the project has:

```
[ ] Text Styles created (Typography / Display through Typography / Helper)
[ ] Color Variables or Styles created (Primitives + Semantic tokens)
[ ] Spacing scale defined (4/8/12/16/24/32/48/64)
[ ] Border radius tokens set (FLOAT variables: 0/2/4/8/12/16/24/9999)
[ ] Icon set chosen and imported as components (Lucide, Phosphor, Material Symbols, etc.)
[ ] Icon sizing scale confirmed (12/16/20/24/32/40/48)
[ ] All icon frames are square (width = height), auto layout center-center

Missing any of these? → Run governance.md Step 3 first.
See 01-guide §20 (Iconography) and §21 (Border Radius) for full rules.
```

### Tool Selection Guide

| Task | Primary Tool | Fallback |
|------|-------------|----------|
| Read file structure | `figma_get_file_data` | `Figma:get_metadata` |
| Read a specific node's design | `Figma:get_design_context` | `figma_get_component` |
| See what's selected | `figma_get_selection` | Ask user for node ID |
| Create shapes/frames/text | `figma_create_child` | `figma_execute` |
| Create component variants | `figma_execute` + `figma_arrange_component_set` | Manual |
| Set text content | `figma_set_text` | `figma_execute` |
| Set colors | `figma_set_fills` | `figma_execute` |
| Move/resize nodes | `figma_move_node` / `figma_resize_node` | `figma_execute` |
| Create variables | `figma_batch_create_variables` | `figma_create_variable` |
| Set up full token system | `figma_setup_design_tokens` | Batch create |
| Instantiate components | `figma_instantiate_component` | `figma_execute` |
| Set component properties | `figma_set_instance_properties` | `figma_execute` |
| Screenshot for verification | `figma_capture_screenshot` | `figma_take_screenshot` |
| Get code-ready data | `figma_get_component_for_development` | `Figma:get_design_context` |
| Read variables/tokens | `figma_get_variables` | `figma_get_token_values` |
| Read styles | `figma_get_styles` | `Figma:get_variable_defs` |
| Check design parity | `figma_check_design_parity` | Manual comparison |

### Icon Page Integration

When building screens that include icons:
1. Read `icon_page_name` from `project/APP_PLAN.md`
2. If set, use `get_design_context` to fetch available icons from that page
3. Match needed icons to available icons by name/purpose
4. Use the project's actual icon components — never substitute generic/emoji icons
5. If an icon doesn't exist, flag it as missing and suggest adding it
6. If `icon_page_name` is not set, ask the user which page contains their icons

---

## 2. Auto Layout Mastery

### Every Frame Is Auto Layout

This is non-negotiable. When building or fixing any Figma file:

```javascript
// Check: Is this frame auto layout?
// If node.layoutMode === "NONE" → it needs to be converted

// To convert via figma_execute:
const node = figma.getNodeById("NODE_ID");
node.layoutMode = "VERTICAL"; // or "HORIZONTAL"
node.primaryAxisSizingMode = "FIXED"; // or "AUTO" for hug
node.counterAxisSizingMode = "FIXED"; // or "AUTO" for hug
node.itemSpacing = 16; // gap between children
node.paddingTop = 16;
node.paddingBottom = 16;
node.paddingLeft = 16;
node.paddingRight = 16;
```

### Screen Construction Template

When building a new screen from scratch, use this exact sequence:

```
STEP 1: Create Screen Frame
  → figma_create_child: FRAME, name "Screen / [Name]"
  → figma_execute: Set layoutMode = "VERTICAL", width = 393 (mobile), height = 852
  → figma_execute: Set primaryAxisSizingMode = "FIXED" (fixed height)
  → figma_execute: Set counterAxisSizingMode = "FIXED" (fixed width)
  → figma_execute: Set clipsContent = true

STEP 2: Create Header
  → figma_create_child: FRAME inside screen, name "Header"
  → figma_execute: Set layoutMode = "VERTICAL"
  → Set sizing: width = FILL, height = HUG
  → Set padding: 16 left/right, 54 top (status bar), 12 bottom

STEP 3: Create Content Area
  → figma_create_child: FRAME inside screen, name "Content"
  → figma_execute: Set layoutMode = "VERTICAL"
  → Set sizing: width = FILL, height = FILL ← CRITICAL
  → Set clipsContent = true (enables scrolling)
  → Set itemSpacing = gap between content sections

STEP 4: Create Footer / Bottom Navigation (if applicable)
  → figma_create_child: FRAME inside screen, name "Footer"
  → figma_execute: Set layoutMode = "HORIZONTAL"
  → Set sizing: width = FILL, height = HUG
  → Set padding: 8 top, 34 bottom (home indicator safe area)
  → Set counterAxisAlignItems = "CENTER"

STEP 5: Verify
  → figma_capture_screenshot to confirm structure
  → Check: Header at top, Content fills middle, Footer at bottom
```

### Auto Layout Property Reference

| Figma UI | API Property | Values | Notes |
|----------|-------------|--------|-------|
| Direction | `layoutMode` | `"VERTICAL"` / `"HORIZONTAL"` | V for stacks, H for rows |
| Gap | `itemSpacing` | Number (px) | Space between children |
| Padding (all) | `paddingTop/Right/Bottom/Left` | Number (px) | Internal spacing |
| Primary axis align | `primaryAxisAlignItems` | `"MIN"` / `"CENTER"` / `"MAX"` / `"SPACE_BETWEEN"` | Along flow direction |
| Counter axis align | `counterAxisAlignItems` | `"MIN"` / `"CENTER"` / `"MAX"` / `"STRETCH"` | Perpendicular to flow |
| Width sizing | `layoutSizingHorizontal` | `"FIXED"` / `"HUG"` / `"FILL"` | How wide this frame is |
| Height sizing | `layoutSizingVertical` | `"FIXED"` / `"HUG"` / `"FILL"` | How tall this frame is |
| Clip content | `clipsContent` | `true` / `false` | Enables scroll behavior |
| Wrap | `layoutWrap` | `"WRAP"` / `"NO_WRAP"` | For tag clouds, chip groups |
| Absolute position | `layoutPositioning` | `"AUTO"` / `"ABSOLUTE"` | Only for overlays |

### Sizing Decision Tree

For every frame/element, ask:

```
Should this element stretch to fill its parent?
├─ YES → layoutSizingHorizontal = "FILL" (or vertical)
│   Examples: buttons in a card, text in a row, content area height
│
├─ NO, it should be exactly its content size
│  → layoutSizingHorizontal = "HUG"
│   Examples: tags, badges, icons, headers, footers
│
└─ NO, it needs a specific pixel size
   → layoutSizingHorizontal = "FIXED" + set explicit width
    Examples: avatars (40×40), screen width (393px), dividers (1px height)
```

### Common Patterns Built with Auto Layout

**Horizontal Row with Space Between:**
```
Row (H, fill W, hug H)
  primaryAxisAlignItems = "SPACE_BETWEEN"
  ├── Left Content (hug)
  └── Right Content (hug)
```
Use for: headers with title + actions, list items with label + value, price rows.

**Centered Stack:**
```
Container (V, fill W, fill H)
  primaryAxisAlignItems = "CENTER"
  counterAxisAlignItems = "CENTER"
  └── Content (hug W, hug H)
```
Use for: empty states, loading screens, modals, dialogs.

**Tag/Chip Cloud (Wrap):**
```
Tag Container (H, fill W, hug H)
  layoutWrap = "WRAP"
  itemSpacing = 8
  counterAxisSpacing = 8
  ├── Tag 1 (hug)
  ├── Tag 2 (hug)
  └── Tag N... (wraps to next line)
```
Use for: filter chips, skill tags, category labels.

**Card with Fixed Image + Flexible Content:**
```
Card (V, fill W, hug H)
  padding = 0 (image bleeds to edges)
  ├── Image (fill W, fixed H: 200)
  │     → Set objectScaleMode to "FILL" for cover behavior
  └── Content (V, fill W, hug H, padding: 16)
      ├── Title (fill W)
      ├── Description (fill W)
      └── Action Row (H, fill W, hug H)
```

**Sticky Footer Pattern:**
```
Screen (V, fixed W, fixed H)
  ├── Scroll Content (V, fill W, FILL H, clip + scroll)
  │   ├── Section 1...
  │   ├── Section N...
  │   └── Bottom Spacer (fill W, fixed H: 80) ← prevents content hiding behind footer
  └── Footer (V, fill W, hug H, padding: 16)
      └── CTA Button (fill W, fixed H: 48)
```

### Auto Layout Repair Workflow

When fixing an existing file that has broken or missing auto layout:

```
STEP 1: Audit Structure
  → figma_get_file_data with verbosity "standard", depth 2
  → Identify frames with layoutMode = "NONE"
  → List all manually positioned elements

STEP 2: Detect Spacer Anti-Pattern
  → Run the spacer detection script (below)
  → Flag all frames named "_sp", "Spacer", or empty frames used solely for spacing
  → Note each spacer's height and its parent container
  → Check: does the parent have itemSpacing = 0? (confirms spacer pattern)

STEP 3: Detect Placeholder Heights
  → Run the placeholder height detection script (below)
  → Flag all auto layout frames with fixed height that should be HUG
  → Common tell: many elements at the same round number (100px, 200px)
  → These elements have content shorter than their fixed height

STEP 4: Fix Spacers (per parent container)
  For each parent that contains spacers:
    a. Analyze spacer heights — are they all the same or mixed?
    b. ALL SAME → simple: delete spacers, set parent itemSpacing = that height
    c. MIXED (most common) → use the GROUPING STRATEGY:
       1. Identify the most common spacer height → this becomes the parent gap
       2. Items separated by a DIFFERENT gap → group them into a sub-frame section
       3. Delete all spacer frames
       4. Set parent itemSpacing to the most common gap
       5. Set each sub-frame's itemSpacing to its section's gap

STEP 5: Fix Placeholder Heights
  → For each flagged element: set layoutSizingVertical = "HUG"
  → ONLY keep fixed height on: screen frames, images, icons, avatars, dividers, status bar

STEP 6: Fix Remaining Auto Layout Issues (bottom-up)
  → Start from DEEPEST children, work outward
  → Convert non-auto-layout frames
  → Set child sizing (fill/hug/fixed)

STEP 7: Verify
  → figma_capture_screenshot
  → Compare: layout should be tighter (no wasted space), same visual hierarchy
```

### The Grouping Strategy for Mixed Gaps

When a container has children with different spacing needs (the `_sp` pattern always signals this):

```
BEFORE (anti-pattern — spacer frames):
Container (gap: 0)
├── Title
├── _sp (h: 8)       ← 8px gap after title
├── Subtitle
├── _sp (h: 20)      ← 20px gap after subtitle
├── Content Card
├── _sp (h: 20)      ← 20px gap after card
└── Button

AFTER (correct — grouped with auto layout gap):
Container (gap: 20)                ← most common gap becomes parent gap
├── Text Stack (gap: 8)           ← group items that need tighter spacing
│   ├── Title
│   └── Subtitle
├── Content Card
└── Button
```

**Rules for grouping:**
1. Find the most frequent gap → set as parent `itemSpacing`
2. Items with tighter gap → wrap in a sub-frame with that gap
3. Sub-frame must be: auto layout, fill width, hug height, zero padding, transparent fill
4. Name the sub-frame semantically (e.g., "Text Stack", "Header Section", "Form Fields")
5. Maximum one level of grouping — don't nest groups inside groups

### Detection Scripts

**Find all spacer frames:**

```javascript
// Via figma_execute — detect spacer anti-pattern
function findSpacers(node, results = []) {
  if (node.type === 'FRAME' && (
    node.name === '_sp' || 
    node.name === 'Spacer' || 
    node.name.startsWith('_sp') ||
    (node.name.match(/^Frame \d+$/) && node.children?.length === 0 && node.width > 0 && node.height > 0 && node.height <= 64)
  )) {
    results.push({
      id: node.id,
      name: node.name,
      height: Math.round(node.height),
      width: Math.round(node.width),
      parentId: node.parent?.id,
      parentName: node.parent?.name,
      parentGap: node.parent?.itemSpacing
    });
  }
  if ('children' in node) {
    for (const child of node.children) findSpacers(child, results);
  }
  return results;
}
const spacers = findSpacers(figma.currentPage);
return { count: spacers.length, spacers: spacers.slice(0, 30) };
```

**Find all placeholder-height elements:**

```javascript
// Via figma_execute — detect elements stuck at placeholder heights
function findPlaceholderHeights(node, results = []) {
  if (node.type === 'FRAME' && 
      node.layoutMode && node.layoutMode !== 'NONE' &&
      node.layoutSizingVertical === 'FIXED' &&
      node.name !== '_sp' && node.name !== 'Spacer') {
    // Check if content is shorter than frame
    let contentHeight = 0;
    if ('children' in node && node.children.length > 0) {
      const padding = (node.paddingTop || 0) + (node.paddingBottom || 0);
      if (node.layoutMode === 'VERTICAL') {
        const gaps = Math.max(0, node.children.length - 1) * (node.itemSpacing || 0);
        contentHeight = padding + gaps;
        for (const c of node.children) contentHeight += c.height;
      } else {
        contentHeight = padding + Math.max(...node.children.map(c => c.height));
      }
    }
    const wasted = Math.round(node.height) - Math.round(contentHeight);
    if (wasted > 10) { // More than 10px of wasted space
      results.push({
        id: node.id,
        name: node.name,
        height: Math.round(node.height),
        contentHeight: Math.round(contentHeight),
        wastedPx: wasted,
        parent: node.parent?.name
      });
    }
  }
  if ('children' in node) {
    for (const child of node.children) findPlaceholderHeights(child, results);
  }
  return results;
}
const placeholders = findPlaceholderHeights(figma.currentPage);
return { count: placeholders.length, items: placeholders.slice(0, 30) };
```

**Bulk fix — remove spacers and set gaps:**

```javascript
// Via figma_execute — automated spacer removal
// WARNING: Run detection first, review results, then run this fix
const page = figma.currentPage;
const parentMap = new Map(); // parentId → [spacer heights]

function collectSpacers(node) {
  if (node.type === 'FRAME' && (node.name === '_sp' || node.name === 'Spacer' || node.name.startsWith('_sp'))) {
    const pid = node.parent?.id;
    if (pid) {
      if (!parentMap.has(pid)) parentMap.set(pid, []);
      parentMap.get(pid).push({ node, height: Math.round(node.height) });
    }
  }
  if ('children' in node) {
    for (const child of node.children) collectSpacers(child);
  }
}
for (const c of page.children) collectSpacers(c);

const fixes = [];
for (const [parentId, spacers] of parentMap) {
  const parent = await figma.getNodeByIdAsync(parentId);
  if (!parent) continue;
  
  // Find most common height
  const freq = {};
  spacers.forEach(s => freq[s.height] = (freq[s.height] || 0) + 1);
  const bestGap = Number(Object.entries(freq).sort((a, b) => b[1] - a[1])[0][0]);
  
  // Remove all spacers
  spacers.forEach(s => s.node.remove());
  
  // Set parent gap
  parent.itemSpacing = bestGap;
  
  fixes.push({ parent: parent.name, removedSpacers: spacers.length, newGap: bestGap });
}
return fixes;
```

**Bulk fix — convert placeholder heights to HUG:**

```javascript
// Via figma_execute — convert fixed-height elements to HUG
const page = figma.currentPage;
const fixed = [];

function fixHeights(node) {
  if (node.type === 'FRAME' && 
      node.layoutMode && node.layoutMode !== 'NONE' &&
      node.layoutSizingVertical === 'FIXED' &&
      node.name !== '_sp' && !node.name.includes('Spacer') &&
      !node.name.includes('Divider') && !node.name.includes('Map') &&
      !node.name.includes('Image') && !node.name.includes('Avatar') &&
      !node.name.includes('Icon') && !node.name.includes('Progress') &&
      !node.name.includes('Status Bar') &&
      !node.parent?.parent?.type === 'PAGE') { // Don't change screen frames
    // Only convert if it's likely a placeholder (content < frame height)
    let maxChildH = 0;
    if ('children' in node && node.children.length > 0) {
      maxChildH = Math.max(...node.children.map(c => c.height));
    }
    if (node.height > maxChildH + (node.paddingTop || 0) + (node.paddingBottom || 0) + 20) {
      node.layoutSizingVertical = 'HUG';
      fixed.push({ id: node.id, name: node.name, oldH: Math.round(node.height) });
    }
  }
  if ('children' in node) {
    for (const child of node.children) fixHeights(child);
  }
}
for (const c of page.children) fixHeights(c);
return { fixedCount: fixed.length, items: fixed.slice(0, 30) };
```

**Detect zero-padding, off-grid spacing, and wrong sizing modes:**

```javascript
// Via figma_execute — comprehensive spacing & sizing audit
// Detects: zero padding on interactive elements, off-grid values, wrong FIXED/HUG
const validSpacing = [0, 2, 4, 8, 12, 16, 20, 24, 32, 48, 64];
const page = figma.currentPage;
const issues = { zeroPadding: [], offGrid: [], wrongSizing: [] };

const mustHavePadding = ['button', 'btn', 'input', 'card', 'chip', 'tag', 'badge',
  'modal', 'dialog', 'sheet', 'toast', 'snackbar', 'dropdown', 'menu', 'tooltip',
  'list item', 'listitem', 'nav item', 'tab'];

const mustHugHeight = ['button', 'btn', 'card', 'chip', 'tag', 'badge', 'modal',
  'dialog', 'sheet', 'toast', 'snackbar', 'accordion', 'section header'];

const mustFixedHeight = ['input', 'text field', 'search bar', 'select'];

function audit(node, depth = 0) {
  if (depth > 15) return;
  if (node.type === 'FRAME' && node.layoutMode && node.layoutMode !== 'NONE') {
    const name = node.name.toLowerCase();
    const pL = node.paddingLeft || 0;
    const pR = node.paddingRight || 0;
    const pT = node.paddingTop || 0;
    const pB = node.paddingBottom || 0;

    // Zero-padding check
    const needsPadding = mustHavePadding.some(kw => name.includes(kw));
    if (needsPadding) {
      if (pL === 0 || pR === 0) {
        issues.zeroPadding.push({
          id: node.id, name: node.name, type: 'horizontal',
          padding: { L: pL, R: pR, T: pT, B: pB }
        });
      }
      if (pT === 0 || pB === 0) {
        issues.zeroPadding.push({
          id: node.id, name: node.name, type: 'vertical',
          padding: { L: pL, R: pR, T: pT, B: pB }
        });
      }
    }

    // Off-grid check (padding + gap)
    const allVals = [
      { prop: 'paddingLeft', val: pL }, { prop: 'paddingRight', val: pR },
      { prop: 'paddingTop', val: pT }, { prop: 'paddingBottom', val: pB },
      { prop: 'itemSpacing', val: node.itemSpacing || 0 }
    ];
    for (const { prop, val } of allVals) {
      if (val > 0 && !validSpacing.includes(val)) {
        const nearest = validSpacing.reduce((a, b) => Math.abs(b - val) < Math.abs(a - val) ? b : a);
        issues.offGrid.push({ id: node.id, name: node.name, prop, value: val, snap: nearest });
      }
    }

    // Sizing mode check
    const shouldHug = mustHugHeight.some(kw => name.includes(kw));
    if (shouldHug && node.layoutSizingVertical === 'FIXED') {
      issues.wrongSizing.push({
        id: node.id, name: node.name,
        current: 'FIXED', expected: 'HUG', height: Math.round(node.height),
        reason: 'Content element should grow with its content'
      });
    }
    // Content area check — should be FILL, not HUG
    if ((name.includes('content') || name.includes('body') || name.includes('scroll'))
        && node.layoutSizingVertical === 'HUG'
        && node.parent?.layoutMode === 'VERTICAL') {
      issues.wrongSizing.push({
        id: node.id, name: node.name,
        current: 'HUG', expected: 'FILL', height: Math.round(node.height),
        reason: 'Content area between header/footer must FILL to push footer down'
      });
    }
  }
  if ('children' in node) {
    for (const child of node.children) audit(child, depth + 1);
  }
}
for (const c of page.children) audit(c);
return {
  zeroPadding: { count: issues.zeroPadding.length, items: issues.zeroPadding.slice(0, 20) },
  offGrid: { count: issues.offGrid.length, items: issues.offGrid.slice(0, 20) },
  wrongSizing: { count: issues.wrongSizing.length, items: issues.wrongSizing.slice(0, 20) }
};
```

**Bulk fix — set minimum padding on zero-padded elements:**

```javascript
// Via figma_execute — fix zero-padding interactive elements
// WARNING: Run detection first, review results, then run this fix
const page = figma.currentPage;
const fixed = [];

const minimums = {
  'button': { h: 16, v: 12 }, 'btn': { h: 16, v: 12 },
  'input': { h: 12, v: 12 }, 'text field': { h: 12, v: 12 },
  'card': { h: 16, v: 16 }, 'chip': { h: 8, v: 4 },
  'tag': { h: 8, v: 4 }, 'badge': { h: 8, v: 4 },
  'modal': { h: 20, v: 20 }, 'dialog': { h: 20, v: 20 },
  'sheet': { h: 16, v: 16 }, 'toast': { h: 16, v: 12 },
  'list item': { h: 16, v: 12 }, 'listitem': { h: 16, v: 12 },
  'dropdown': { h: 12, v: 8 }, 'menu': { h: 12, v: 8 },
  'tooltip': { h: 8, v: 6 }, 'tab': { h: 12, v: 8 }
};

function fixPadding(node) {
  if (node.type === 'FRAME' && node.layoutMode && node.layoutMode !== 'NONE') {
    const name = node.name.toLowerCase();
    for (const [kw, min] of Object.entries(minimums)) {
      if (name.includes(kw)) {
        let changed = false;
        if ((node.paddingLeft || 0) < min.h) { node.paddingLeft = min.h; changed = true; }
        if ((node.paddingRight || 0) < min.h) { node.paddingRight = min.h; changed = true; }
        if ((node.paddingTop || 0) < min.v) { node.paddingTop = min.v; changed = true; }
        if ((node.paddingBottom || 0) < min.v) { node.paddingBottom = min.v; changed = true; }
        if (changed) {
          fixed.push({
            id: node.id, name: node.name,
            newPadding: { L: node.paddingLeft, R: node.paddingRight, T: node.paddingTop, B: node.paddingBottom }
          });
        }
        break;
      }
    }
  }
  if ('children' in node) {
    for (const child of node.children) fixPadding(child);
  }
}
for (const c of page.children) fixPadding(c);
return { fixedCount: fixed.length, items: fixed.slice(0, 30) };
```

---

## 3. Component Construction

### When to Componentize

| Situation | Action |
|-----------|--------|
| Element appears 2+ times on same screen | Make it a component |
| Element appears across multiple screens | Make it a component (publish to library) |
| Element has different states (hover, disabled, etc.) | Make it a component set with variants |
| Section of a screen is reused (e.g., card, list item) | Make it a component |
| Navigation bar, header, footer | Always a component |

### Building a Component

```
STEP 1: Build the visual structure
  → Create all frames with auto layout
  → Apply text styles and color variables
  → Set all sizing (fill/hug/fixed)
  → Name all layers semantically

STEP 2: Convert to component
  → figma_execute: const comp = figma.currentPage.selection[0];
    const component = figma.createComponentFromNode(comp);

STEP 3: Add component properties
  → figma_add_component_property for each dynamic element:
    - TEXT props for editable labels
    - BOOLEAN props for show/hide toggles (e.g., "Show Icon")
    - INSTANCE_SWAP props for swappable icons or avatars
    - VARIANT props for state selection

STEP 4: Set description
  → figma_set_description: Include usage notes, dos/don'ts

STEP 5: Name with convention
  → "[Category] / [Name] / [Variant]"
  → Examples: "Button / Primary / Large", "Card / Product / Default"
```

### Component Naming Convention

```
[Category] / [Subcategory] / [Name]

Navigation / Bottom Nav
Navigation / Top Bar
Navigation / Breadcrumb

Button / Primary
Button / Secondary
Button / Ghost
Button / Destructive

Input / Text
Input / Select
Input / Checkbox
Input / Radio
Input / Toggle
Input / Search

Card / Product
Card / User
Card / Notification

Feedback / Toast
Feedback / Alert
Feedback / Badge
Feedback / Progress

Layout / Divider
Layout / Section Header

Modal / Dialog
Modal / Bottom Sheet
Modal / Full Screen
```

---

## 4. Variant & Property Architecture

### Building a Component Set

When a component needs multiple states or sizes:

```
STEP 1: Create each variant as a separate component
  → Button Default, Button Hover, Button Active, Button Disabled, Button Loading

STEP 2: Select all variants
  → figma_execute:
    const variants = [nodeId1, nodeId2, ...].map(id => figma.getNodeById(id));
    const componentSet = figma.combineAsVariants(variants, figma.currentPage);

STEP 3: Set variant properties
  → Each variant gets property values in its name:
    "State=Default", "State=Hover", "Size=Large", "Type=Primary"

STEP 4: Arrange cleanly
  → figma_arrange_component_set: organizes into grid with labels

STEP 5: Set default variant
  → The first variant in the set becomes the default when instantiated
```

### Property Types & When to Use Them

| Property Type | Use Case | Example |
|---|---|---|
| **VARIANT** | Discrete visual states that change the structure | State: Default/Hover/Active/Disabled, Size: SM/MD/LG, Type: Primary/Secondary |
| **BOOLEAN** | Show/hide optional elements | Show Icon: true/false, Show Badge: true/false, Has Divider: true/false |
| **TEXT** | Editable text content | Label: "Button", Heading: "Title", Helper: "Enter your email" |
| **INSTANCE_SWAP** | Swappable child components | Icon: [any icon component], Avatar: [any avatar component] |

### Variant Matrix Design

For complex components, plan the variant matrix:

```
Button Component Set:
┌─────────────┬─────────┬─────────┬──────────┬──────────┬──────────┐
│             │ Default │ Hover   │ Active   │ Disabled │ Loading  │
├─────────────┼─────────┼─────────┼──────────┼──────────┼──────────┤
│ Primary SM  │    ✓    │    ✓    │    ✓     │    ✓     │    ✓     │
│ Primary MD  │    ✓    │    ✓    │    ✓     │    ✓     │    ✓     │
│ Primary LG  │    ✓    │    ✓    │    ✓     │    ✓     │    ✓     │
│ Secondary SM│    ✓    │    ✓    │    ✓     │    ✓     │    ✓     │
│ Secondary MD│    ✓    │    ✓    │    ✓     │    ✓     │    ✓     │
│ Secondary LG│    ✓    │    ✓    │    ✓     │    ✓     │    ✓     │
│ Ghost SM    │    ✓    │    ✓    │    ✓     │    ✓     │    ✓     │
│ Ghost MD    │    ✓    │    ✓    │    ✓     │    ✓     │    ✓     │
│ Ghost LG    │    ✓    │    ✓    │    ✓     │    ✓     │    ✓     │
└─────────────┴─────────┴─────────┴──────────┴──────────┴──────────┘

Properties: Type (Primary/Secondary/Ghost) × Size (SM/MD/LG) × State (Default/Hover/Active/Disabled/Loading)
BOOLEAN: Show Icon, Icon Only
TEXT: Label
INSTANCE_SWAP: Icon
```

---

## 5. Style & Variable Setup

### Creating Text Styles via API

```javascript
// Via figma_execute — create a text style programmatically
async function createTextStyle(name, fontFamily, fontSize, fontWeight, lineHeight, letterSpacing) {
  const style = figma.createTextStyle();
  style.name = name;
  await figma.loadFontAsync({ family: fontFamily, style: fontWeight });
  style.fontName = { family: fontFamily, style: fontWeight };
  style.fontSize = fontSize;
  style.lineHeight = { value: lineHeight, unit: "PIXELS" };
  if (letterSpacing) {
    style.letterSpacing = { value: letterSpacing, unit: "PIXELS" };
  }
  return style;
}

// Create the full type scale
await createTextStyle("Typography / Display", "Inter", 48, "Bold", 56, -0.5);
await createTextStyle("Typography / H1", "Inter", 32, "Bold", 40, -0.25);
await createTextStyle("Typography / H2", "Inter", 24, "SemiBold", 32, 0);
await createTextStyle("Typography / H3", "Inter", 20, "SemiBold", 28, 0);
await createTextStyle("Typography / H4", "Inter", 18, "Medium", 24, 0);
await createTextStyle("Typography / Body Large", "Inter", 18, "Regular", 28, 0);
await createTextStyle("Typography / Body", "Inter", 16, "Regular", 24, 0);
await createTextStyle("Typography / Body Bold", "Inter", 16, "SemiBold", 24, 0);
await createTextStyle("Typography / Body Small", "Inter", 14, "Regular", 20, 0);
await createTextStyle("Typography / Body Small Bold", "Inter", 14, "SemiBold", 20, 0);
await createTextStyle("Typography / Caption", "Inter", 12, "Regular", 16, 0);
await createTextStyle("Typography / Caption Bold", "Inter", 12, "Medium", 16, 0);
await createTextStyle("Typography / Overline", "Inter", 11, "Medium", 16, 0.5);
await createTextStyle("Typography / Button Large", "Inter", 16, "SemiBold", 24, 0);
await createTextStyle("Typography / Button", "Inter", 14, "SemiBold", 20, 0);
await createTextStyle("Typography / Button Small", "Inter", 12, "Medium", 16, 0);
await createTextStyle("Typography / Label", "Inter", 14, "Medium", 20, 0);
await createTextStyle("Typography / Helper", "Inter", 12, "Regular", 16, 0);
```

**Important:** Replace `"Inter"` with the project's actual brand font from APP_PLAN.md.

### Creating Color Variables via MCP

Color styles require two collections: **Primitives** (the raw palette) and **Semantic** (the tokens you actually apply). Create primitives first, then semantic tokens that reference them.

**Step 1: Create the Primitives collection**

```
Tool: figma_setup_design_tokens
Parameters:
  collectionName: "Primitives"
  modes: ["Value"]
  tokens: [
    { name: "Primary/50",  resolvedType: "COLOR", values: { "Value": "#EFF6FF" } },
    { name: "Primary/100", resolvedType: "COLOR", values: { "Value": "#DBEAFE" } },
    { name: "Primary/200", resolvedType: "COLOR", values: { "Value": "#BFDBFE" } },
    { name: "Primary/300", resolvedType: "COLOR", values: { "Value": "#93C5FD" } },
    { name: "Primary/400", resolvedType: "COLOR", values: { "Value": "#60A5FA" } },
    { name: "Primary/500", resolvedType: "COLOR", values: { "Value": "#3B82F6" } },
    { name: "Primary/600", resolvedType: "COLOR", values: { "Value": "#2563EB" } },
    { name: "Primary/700", resolvedType: "COLOR", values: { "Value": "#1D4ED8" } },
    { name: "Primary/800", resolvedType: "COLOR", values: { "Value": "#1E40AF" } },
    { name: "Primary/900", resolvedType: "COLOR", values: { "Value": "#1E3A8A" } },
    { name: "Primary/950", resolvedType: "COLOR", values: { "Value": "#172554" } },

    { name: "Neutral/0",    resolvedType: "COLOR", values: { "Value": "#FFFFFF" } },
    { name: "Neutral/50",   resolvedType: "COLOR", values: { "Value": "#F9FAFB" } },
    { name: "Neutral/100",  resolvedType: "COLOR", values: { "Value": "#F3F4F6" } },
    { name: "Neutral/200",  resolvedType: "COLOR", values: { "Value": "#E5E7EB" } },
    { name: "Neutral/300",  resolvedType: "COLOR", values: { "Value": "#D1D5DB" } },
    { name: "Neutral/400",  resolvedType: "COLOR", values: { "Value": "#9CA3AF" } },
    { name: "Neutral/500",  resolvedType: "COLOR", values: { "Value": "#6B7280" } },
    { name: "Neutral/600",  resolvedType: "COLOR", values: { "Value": "#4B5563" } },
    { name: "Neutral/700",  resolvedType: "COLOR", values: { "Value": "#374151" } },
    { name: "Neutral/800",  resolvedType: "COLOR", values: { "Value": "#1F2937" } },
    { name: "Neutral/900",  resolvedType: "COLOR", values: { "Value": "#111827" } },
    { name: "Neutral/950",  resolvedType: "COLOR", values: { "Value": "#030712" } },

    { name: "Success/50",  resolvedType: "COLOR", values: { "Value": "#F0FDF4" } },
    { name: "Success/500", resolvedType: "COLOR", values: { "Value": "#22C55E" } },
    { name: "Success/600", resolvedType: "COLOR", values: { "Value": "#16A34A" } },
    { name: "Success/700", resolvedType: "COLOR", values: { "Value": "#15803D" } },
    { name: "Success/900", resolvedType: "COLOR", values: { "Value": "#14532D" } },

    { name: "Warning/50",  resolvedType: "COLOR", values: { "Value": "#FFFBEB" } },
    { name: "Warning/500", resolvedType: "COLOR", values: { "Value": "#F59E0B" } },
    { name: "Warning/600", resolvedType: "COLOR", values: { "Value": "#D97706" } },
    { name: "Warning/700", resolvedType: "COLOR", values: { "Value": "#B45309" } },

    { name: "Error/50",  resolvedType: "COLOR", values: { "Value": "#FEF2F2" } },
    { name: "Error/500", resolvedType: "COLOR", values: { "Value": "#EF4444" } },
    { name: "Error/600", resolvedType: "COLOR", values: { "Value": "#DC2626" } },
    { name: "Error/700", resolvedType: "COLOR", values: { "Value": "#B91C1C" } },

    { name: "Info/50",  resolvedType: "COLOR", values: { "Value": "#EFF6FF" } },
    { name: "Info/500", resolvedType: "COLOR", values: { "Value": "#3B82F6" } },
    { name: "Info/600", resolvedType: "COLOR", values: { "Value": "#2563EB" } },
    { name: "Info/700", resolvedType: "COLOR", values: { "Value": "#1D4ED8" } }
  ]
```

⚠️ **Replace all hex values above with the project's actual colors from APP_PLAN.md.** The values shown are Tailwind-based defaults — your project will have different brand colors.

**Step 2: Create the Semantic collection with Light + Dark modes**

```
Tool: figma_setup_design_tokens
Parameters:
  collectionName: "Semantic"
  modes: ["Light", "Dark"]
  tokens: [
    { name: "Background/Primary",   resolvedType: "COLOR", values: { "Light": "#FFFFFF", "Dark": "#030712" } },
    { name: "Background/Secondary", resolvedType: "COLOR", values: { "Light": "#F9FAFB", "Dark": "#111827" } },
    { name: "Background/Tertiary",  resolvedType: "COLOR", values: { "Light": "#F3F4F6", "Dark": "#1F2937" } },
    { name: "Background/Inverse",   resolvedType: "COLOR", values: { "Light": "#111827", "Dark": "#F9FAFB" } },

    { name: "Text/Primary",    resolvedType: "COLOR", values: { "Light": "#111827", "Dark": "#F9FAFB" } },
    { name: "Text/Secondary",  resolvedType: "COLOR", values: { "Light": "#4B5563", "Dark": "#9CA3AF" } },
    { name: "Text/Tertiary",   resolvedType: "COLOR", values: { "Light": "#9CA3AF", "Dark": "#6B7280" } },
    { name: "Text/Inverse",    resolvedType: "COLOR", values: { "Light": "#FFFFFF", "Dark": "#111827" } },
    { name: "Text/Link",       resolvedType: "COLOR", values: { "Light": "#2563EB", "Dark": "#60A5FA" } },
    { name: "Text/Error",      resolvedType: "COLOR", values: { "Light": "#DC2626", "Dark": "#F87171" } },
    { name: "Text/Success",    resolvedType: "COLOR", values: { "Light": "#16A34A", "Dark": "#4ADE80" } },

    { name: "Border/Default",  resolvedType: "COLOR", values: { "Light": "#E5E7EB", "Dark": "#374151" } },
    { name: "Border/Strong",   resolvedType: "COLOR", values: { "Light": "#D1D5DB", "Dark": "#4B5563" } },
    { name: "Border/Focus",    resolvedType: "COLOR", values: { "Light": "#3B82F6", "Dark": "#60A5FA" } },

    { name: "Surface/Card",     resolvedType: "COLOR", values: { "Light": "#FFFFFF", "Dark": "#111827" } },
    { name: "Surface/Elevated", resolvedType: "COLOR", values: { "Light": "#FFFFFF", "Dark": "#1F2937" } },
    { name: "Surface/Disabled", resolvedType: "COLOR", values: { "Light": "#F3F4F6", "Dark": "#1F2937" } },

    { name: "Action/Primary",       resolvedType: "COLOR", values: { "Light": "#3B82F6", "Dark": "#60A5FA" } },
    { name: "Action/Primary Hover",  resolvedType: "COLOR", values: { "Light": "#2563EB", "Dark": "#93C5FD" } },
    { name: "Action/Primary Active", resolvedType: "COLOR", values: { "Light": "#1D4ED8", "Dark": "#BFDBFE" } },
    { name: "Action/Secondary",      resolvedType: "COLOR", values: { "Light": "#F3F4F6", "Dark": "#374151" } },
    { name: "Action/Secondary Hover", resolvedType: "COLOR", values: { "Light": "#E5E7EB", "Dark": "#4B5563" } },
    { name: "Action/Destructive",       resolvedType: "COLOR", values: { "Light": "#EF4444", "Dark": "#F87171" } },
    { name: "Action/Destructive Hover", resolvedType: "COLOR", values: { "Light": "#DC2626", "Dark": "#FCA5A5" } },
    { name: "Action/Disabled",          resolvedType: "COLOR", values: { "Light": "#D1D5DB", "Dark": "#4B5563" } },

    { name: "Feedback/Success BG",   resolvedType: "COLOR", values: { "Light": "#F0FDF4", "Dark": "#14532D" } },
    { name: "Feedback/Success Text", resolvedType: "COLOR", values: { "Light": "#15803D", "Dark": "#4ADE80" } },
    { name: "Feedback/Warning BG",   resolvedType: "COLOR", values: { "Light": "#FFFBEB", "Dark": "#78350F" } },
    { name: "Feedback/Warning Text", resolvedType: "COLOR", values: { "Light": "#B45309", "Dark": "#FCD34D" } },
    { name: "Feedback/Error BG",     resolvedType: "COLOR", values: { "Light": "#FEF2F2", "Dark": "#7F1D1D" } },
    { name: "Feedback/Error Text",   resolvedType: "COLOR", values: { "Light": "#B91C1C", "Dark": "#FCA5A5" } },
    { name: "Feedback/Info BG",      resolvedType: "COLOR", values: { "Light": "#EFF6FF", "Dark": "#1E3A8A" } },
    { name: "Feedback/Info Text",    resolvedType: "COLOR", values: { "Light": "#1D4ED8", "Dark": "#93C5FD" } }
  ]
```

**Step 3 (Alternative): Create Color Styles via figma_execute**

If the project doesn't need mode switching (no dark mode), use Color Styles instead of Variables:

```javascript
// Via figma_execute — create color styles programmatically
function hexToRgb(hex) {
  hex = hex.replace('#', '');
  return {
    r: parseInt(hex.slice(0, 2), 16) / 255,
    g: parseInt(hex.slice(2, 4), 16) / 255,
    b: parseInt(hex.slice(4, 6), 16) / 255
  };
}

async function createColorStyle(name, hex) {
  const style = figma.createPaintStyle();
  style.name = name;
  const rgb = hexToRgb(hex);
  style.paints = [{ type: 'SOLID', color: rgb }];
  return style;
}

// Primitives
await createColorStyle("Primary / 50", "#EFF6FF");
await createColorStyle("Primary / 100", "#DBEAFE");
await createColorStyle("Primary / 200", "#BFDBFE");
await createColorStyle("Primary / 300", "#93C5FD");
await createColorStyle("Primary / 400", "#60A5FA");
await createColorStyle("Primary / 500", "#3B82F6");
await createColorStyle("Primary / 600", "#2563EB");
await createColorStyle("Primary / 700", "#1D4ED8");
await createColorStyle("Primary / 800", "#1E40AF");
await createColorStyle("Primary / 900", "#1E3A8A");
await createColorStyle("Primary / 950", "#172554");

await createColorStyle("Neutral / 0",   "#FFFFFF");
await createColorStyle("Neutral / 50",  "#F9FAFB");
await createColorStyle("Neutral / 100", "#F3F4F6");
await createColorStyle("Neutral / 200", "#E5E7EB");
await createColorStyle("Neutral / 300", "#D1D5DB");
await createColorStyle("Neutral / 400", "#9CA3AF");
await createColorStyle("Neutral / 500", "#6B7280");
await createColorStyle("Neutral / 600", "#4B5563");
await createColorStyle("Neutral / 700", "#374151");
await createColorStyle("Neutral / 800", "#1F2937");
await createColorStyle("Neutral / 900", "#111827");
await createColorStyle("Neutral / 950", "#030712");

await createColorStyle("Success / 50",  "#F0FDF4");
await createColorStyle("Success / 500", "#22C55E");
await createColorStyle("Success / 600", "#16A34A");
await createColorStyle("Success / 700", "#15803D");

await createColorStyle("Warning / 50",  "#FFFBEB");
await createColorStyle("Warning / 500", "#F59E0B");
await createColorStyle("Warning / 600", "#D97706");
await createColorStyle("Warning / 700", "#B45309");

await createColorStyle("Error / 50",  "#FEF2F2");
await createColorStyle("Error / 500", "#EF4444");
await createColorStyle("Error / 600", "#DC2626");
await createColorStyle("Error / 700", "#B91C1C");

await createColorStyle("Info / 50",  "#EFF6FF");
await createColorStyle("Info / 500", "#3B82F6");
await createColorStyle("Info / 600", "#2563EB");
await createColorStyle("Info / 700", "#1D4ED8");

// Semantic tokens (Light mode)
await createColorStyle("Background / Primary",   "#FFFFFF");
await createColorStyle("Background / Secondary",  "#F9FAFB");
await createColorStyle("Background / Tertiary",   "#F3F4F6");
await createColorStyle("Background / Inverse",    "#111827");

await createColorStyle("Text / Primary",   "#111827");
await createColorStyle("Text / Secondary", "#4B5563");
await createColorStyle("Text / Tertiary",  "#9CA3AF");
await createColorStyle("Text / Inverse",   "#FFFFFF");
await createColorStyle("Text / Link",      "#2563EB");
await createColorStyle("Text / Error",     "#DC2626");
await createColorStyle("Text / Success",   "#16A34A");

await createColorStyle("Border / Default", "#E5E7EB");
await createColorStyle("Border / Strong",  "#D1D5DB");
await createColorStyle("Border / Focus",   "#3B82F6");

await createColorStyle("Surface / Card",     "#FFFFFF");
await createColorStyle("Surface / Elevated", "#FFFFFF");
await createColorStyle("Surface / Disabled", "#F3F4F6");

await createColorStyle("Action / Primary",         "#3B82F6");
await createColorStyle("Action / Primary Hover",   "#2563EB");
await createColorStyle("Action / Primary Active",  "#1D4ED8");
await createColorStyle("Action / Secondary",       "#F3F4F6");
await createColorStyle("Action / Secondary Hover", "#E5E7EB");
await createColorStyle("Action / Destructive",       "#EF4444");
await createColorStyle("Action / Destructive Hover", "#DC2626");
await createColorStyle("Action / Disabled",          "#D1D5DB");

await createColorStyle("Feedback / Success BG",   "#F0FDF4");
await createColorStyle("Feedback / Success Text", "#15803D");
await createColorStyle("Feedback / Warning BG",   "#FFFBEB");
await createColorStyle("Feedback / Warning Text", "#B45309");
await createColorStyle("Feedback / Error BG",     "#FEF2F2");
await createColorStyle("Feedback / Error Text",   "#B91C1C");
await createColorStyle("Feedback / Info BG",      "#EFF6FF");
await createColorStyle("Feedback / Info Text",    "#1D4ED8");

return { message: "Created all color styles" };
```

**Important:** Replace all hex values with the project's actual colors from APP_PLAN.md.

### Verifying Color Styles After Creation

```javascript
// Via figma_execute — verify all expected color styles exist
const styles = figma.getLocalPaintStyles();
const expected = [
  "Primary / 500", "Neutral / 0", "Neutral / 900",
  "Background / Primary", "Text / Primary", "Text / Secondary",
  "Border / Default", "Action / Primary", "Surface / Card",
  "Feedback / Success BG", "Feedback / Error BG"
];
const found = styles.map(s => s.name);
const missing = expected.filter(e => !found.some(f => f === e));
return {
  totalStyles: styles.length,
  expectedFound: expected.length - missing.length,
  missing: missing,
  allStyleNames: found.sort()
};
```

### Applying Styles to Existing Elements

When fixing raw text/colors on existing screens:

```javascript
// Via figma_execute — apply text style to a text node
const textNode = figma.getNodeById("NODE_ID");
const styles = figma.getLocalTextStyles();
const bodyStyle = styles.find(s => s.name === "Typography / Body");
if (bodyStyle) {
  textNode.textStyleId = bodyStyle.id;
}

// Apply color variable to a frame's fill
const frame = figma.getNodeById("NODE_ID");
const variables = figma.variables.getLocalVariables("COLOR");
const bgVar = variables.find(v => v.name === "Background/Primary");
if (bgVar) {
  const fills = [figma.variables.setBoundVariableForPaint(
    { type: "SOLID", color: { r: 1, g: 1, b: 1 } },
    "color",
    bgVar
  )];
  frame.fills = fills;
}
```

### Bulk Audit: Find All Raw Values

```javascript
// Via figma_execute — scan for text nodes without styles
function findRawText(node, results = []) {
  if (node.type === "TEXT" && !node.textStyleId) {
    results.push({ id: node.id, name: node.name, fontSize: node.fontSize });
  }
  if ("children" in node) {
    for (const child of node.children) {
      findRawText(child, results);
    }
  }
  return results;
}
const raw = findRawText(figma.currentPage);
return { rawTextCount: raw.length, items: raw.slice(0, 20) };
```

```javascript
// Via figma_execute — scan for nodes with hardcoded colors (no variable binding)
function findRawColors(node, results = []) {
  if ("fills" in node && node.fills.length > 0) {
    const hasBoundVar = node.fills.some(f =>
      f.boundVariables && f.boundVariables.color
    );
    if (!hasBoundVar && node.fills[0].type === "SOLID") {
      const c = node.fills[0].color;
      const hex = '#' + [c.r, c.g, c.b].map(v =>
        Math.round(v * 255).toString(16).padStart(2, '0')
      ).join('');
      results.push({ id: node.id, name: node.name, color: hex });
    }
  }
  if ("children" in node) {
    for (const child of node.children) {
      findRawColors(child, results);
    }
  }
  return results;
}
const raw = findRawColors(figma.currentPage);
return { rawColorCount: raw.length, items: raw.slice(0, 20) };
```

### Creating Border Radius Tokens via MCP

```
Tool: figma_setup_design_tokens
Parameters:
  collectionName: "Border Radius"
  modes: ["Default"]
  tokens: [
    { name: "radius/none", resolvedType: "FLOAT", values: { "Default": 0 } },
    { name: "radius/xs",   resolvedType: "FLOAT", values: { "Default": 2 } },
    { name: "radius/sm",   resolvedType: "FLOAT", values: { "Default": 4 } },
    { name: "radius/md",   resolvedType: "FLOAT", values: { "Default": 8 } },
    { name: "radius/lg",   resolvedType: "FLOAT", values: { "Default": 12 } },
    { name: "radius/xl",   resolvedType: "FLOAT", values: { "Default": 16 } },
    { name: "radius/2xl",  resolvedType: "FLOAT", values: { "Default": 24 } },
    { name: "radius/full", resolvedType: "FLOAT", values: { "Default": 9999 } }
  ]
```

Then bind every `cornerRadius` in the file to one of these variables instead of raw numbers.

### Setting Up the Icon System in Figma

**Step 1: Import an icon library**

Use a community icon plugin (Iconify, Phosphor, Lucide) or import SVGs manually. Each icon becomes a component:

```
Icon / Arrow Left    (24×24 component)
Icon / Arrow Right   (24×24 component)
Icon / Search        (24×24 component)
Icon / Close         (24×24 component)
Icon / Check         (24×24 component)
Icon / Star          (24×24 component)
... (import all needed icons)
```

**Step 2: Create icon container components for list items**

```javascript
// Via figma_execute — create a reusable icon container component
const container = figma.createComponent();
container.name = "Icon Container / Default";
container.resize(40, 40);
container.layoutMode = "VERTICAL";
container.primaryAxisAlignItems = "CENTER";
container.counterAxisAlignItems = "CENTER";
container.cornerRadius = 8; // radius-md
container.fills = [{ type: "SOLID", color: { r: 0.96, g: 0.96, b: 0.97 }, opacity: 1 }];
container.layoutSizingHorizontal = "FIXED";
container.layoutSizingVertical = "FIXED";
// The icon instance goes inside this container
```

**Step 3: Audit and replace broken icons**

Run the icon audit script from 01-guide §20 to find text-character icons, non-square frames, and off-scale sizes. Then replace each one with a proper vector icon component instance.

### Bulk Audit: Find Off-Scale Border Radii

```javascript
// Via figma_execute — find all corner radii not on the scale
const validScale = [0, 2, 4, 8, 12, 16, 24, 9999];

function findOffScaleRadii(node, results = []) {
  const r = node.cornerRadius;
  if (typeof r === 'number' && r > 0 && node.type !== 'ELLIPSE') {
    if (!validScale.includes(Math.round(r))) {
      results.push({
        id: node.id, name: node.name,
        radius: Math.round(r),
        nearest: validScale.reduce((a, b) => Math.abs(b - r) < Math.abs(a - r) ? b : a),
        size: `${Math.round(node.width)}×${Math.round(node.height)}`,
        parent: node.parent?.name
      });
    }
  }
  if ('children' in node) {
    for (const child of node.children) findOffScaleRadii(child, results);
  }
  return results;
}
const offScale = findOffScaleRadii(figma.currentPage);
return { offScaleCount: offScale.length, items: offScale.slice(0, 30) };
```

---

## 6. Figma File Repair & Cleanup

### Full File Audit Procedure

Run this when inheriting an existing file or after building screens:

```
PHASE 1: Structure Audit
  → figma_get_file_data (depth 2, verbosity "standard")
  → Check: Are all top-level frames properly named?
  → Check: Are pages organized (Cover, Flows, Screens, Components, Tokens)?

PHASE 2: Spacer & Height Anti-Pattern Scan
  → Run spacer detection script — find all _sp / Spacer / empty spacer frames
  → Run placeholder height detection script — find elements with wasted fixed height
  → Fix spacers FIRST (delete spacers, set parent gaps, group where needed)
  → Fix placeholder heights SECOND (convert fixed → HUG)
  → These two anti-patterns are the #1 cause of "broken" Figma files

PHASE 3: Auto Layout Audit
  → For each screen: check layoutMode on every frame
  → Flag: frames with layoutMode = "NONE" that have children
  → Flag: content areas not set to FILL height
  → Fix: bottom-up conversion (deepest children first)

PHASE 3B: Spacing & Sizing Audit
  → Run spacing detection script (01-guide §23) via figma_execute
  → Flag: ALL elements with zero horizontal or vertical padding that should have padding
    (buttons, inputs, cards, list items, chips, modals, bottom sheets, toasts, tooltips)
  → Flag: Screen content frames with 0px left or right padding (content touches device edge)
  → Flag: ALL off-grid padding/gap values (not on 2/4/8/12/16/20/24/32/48/64 scale)
  → Flag: Buttons, cards, text containers, chips at FIXED height (should be HUG)
  → Flag: Content areas (between header + footer) at HUG height (should be FILL)
  → Flag: Input fields at inconsistent heights (should all be same FIXED height)
  → Fix zero-padding: Set minimum padding per element type (see 01-guide §23 table)
  → Fix off-grid values: Snap to nearest spacing token
  → Fix sizing: Convert FIXED → HUG on content elements, HUG → FILL on content areas

PHASE 4: Icon Audit
  → Run icon detection script (01-guide §20) — find text-character icons, non-square frames, off-scale sizes
  → Replace text-character icons with proper vector icon component instances
  → Fix non-square icon frames (resize to square, e.g., 40×19 → 40×40 with centered content)
  → Snap off-scale sizes to the icon scale (12/16/20/24/32/40/48)
  → Verify all icon containers are auto layout, center-center, fixed square dimensions

PHASE 5: Border Radius Audit
  → Run radius detection script (01-guide §21) — find off-scale radius values
  → Snap each off-scale value to nearest token (e.g., 14px → 12px or 16px, 18px → 16px, 22px → 24px)
  → Verify nesting rule: inner radius < outer radius (inner = outer − gap)
  → Check bottom sheets use top-only rounding
  → Bind all radius values to FLOAT variables (no raw numbers)

PHASE 6: Style Compliance Audit
  → Run the raw text scanner (Section 5)
  → Run the raw color scanner (Section 5)
  → For each raw value: match to closest existing style and apply

PHASE 7: Component Audit
  → Flag: repeated identical structures that aren't components
  → Flag: detached instances (should be re-linked)
  → Flag: components without descriptions

PHASE 8: Naming Audit
  → Flag: any layer named "Frame ###" or "Group ###" or "Rectangle ###"
  → Suggest semantic names based on content/position

PHASE 9: Data Flow Audit
  → For each screen: verify data source annotation exists ([DATA] format from 01-guide §22)
  → Check: Does every screen with dynamic content have empty + loading + error variants?
  → Check: Are screen-to-screen data passing patterns documented? (parameter / payload / shared state)
  → Check: Do multi-step forms preserve data on back navigation?
  → Check: Are real-time screens annotated with update mechanism? (polling / WebSocket / SSE)
  → Check: Are pagination strategies documented for list screens?
  → Flag: Screens with API dependencies but no loading frame sibling
  → Flag: Screens with list content but no empty frame sibling
  → Flag: Screens in a multi-step flow with no back-navigation data preservation noted
  → Run: data state coverage script (below) to detect missing state frames

PHASE 10: Flow & Annotation Completeness
  → Check: Does the file have a dedicated "User Flows" page?
  → Check: Are all flows organized in Figma Sections with title frames?
  → Check: Do all screen frames have screen number badges (FLOW-##)?
  → Check: Are all connector arrows color-coded and labeled?
  → Check: Do all screens have status badges (Draft / Ready for Dev / etc.)?
  → Check: Are there [INTERACTION] annotations on all interactive elements?
  → Check: Are there [LOGIC] annotations on all conditional rendering?
  → Check: Are there [DATA] annotations on all data-dependent elements?
  → Check: Are all Figma comments resolved or assigned?
  → Flag: Orphan screens (no incoming or outgoing connections)

PHASE 11: Screenshot Verification
  → figma_capture_screenshot on each screen
  → Visual check: does everything still look correct after fixes?
```

### Data Flow Detection Scripts

**Detect missing state frames:**

```javascript
// Via figma_execute — find screens that likely need loading/empty/error states but don't have them
const page = figma.currentPage;
const screens = page.children.filter(n => n.type === "FRAME" || n.type === "SECTION");

const results = { missingStates: [], wellCovered: [] };

for (const screen of screens) {
  const name = screen.name.toLowerCase();
  // Skip non-screen frames (components, tokens, etc.)
  if (name.includes("component") || name.includes("token") || name.includes("style")) continue;

  const siblings = screens.map(s => s.name.toLowerCase());
  const baseName = screen.name.replace(/[-_\s]?(default|populated|loaded|filled)/gi, '').trim();

  const hasLoading = siblings.some(s =>
    s.includes(baseName.toLowerCase()) && (s.includes("loading") || s.includes("skeleton"))
  );
  const hasEmpty = siblings.some(s =>
    s.includes(baseName.toLowerCase()) && (s.includes("empty") || s.includes("no data") || s.includes("no items") || s.includes("zero"))
  );
  const hasError = siblings.some(s =>
    s.includes(baseName.toLowerCase()) && (s.includes("error") || s.includes("failed") || s.includes("retry"))
  );

  // Check if screen likely has dynamic content (lists, grids, cards)
  let hasDynamicContent = false;
  function checkChildren(node) {
    if (node.name && (
      node.name.toLowerCase().includes("list") ||
      node.name.toLowerCase().includes("card") ||
      node.name.toLowerCase().includes("item") ||
      node.name.toLowerCase().includes("feed") ||
      node.name.toLowerCase().includes("result")
    )) {
      hasDynamicContent = true;
    }
    if ("children" in node && !hasDynamicContent) {
      for (const child of node.children) checkChildren(child);
    }
  }
  checkChildren(screen);

  if (hasDynamicContent) {
    const missing = [];
    if (!hasLoading) missing.push("loading");
    if (!hasEmpty) missing.push("empty");
    if (!hasError) missing.push("error");

    if (missing.length > 0) {
      results.missingStates.push({
        screen: screen.name,
        id: screen.id,
        missing: missing,
        dynamicChildren: hasDynamicContent
      });
    } else {
      results.wellCovered.push(screen.name);
    }
  }
}
return {
  screensNeedingStates: results.missingStates.length,
  items: results.missingStates.slice(0, 25),
  wellCovered: results.wellCovered.length
};
```

**Detect missing data annotations:**

```javascript
// Via figma_execute — find screens without [DATA] annotations in descriptions
const page = figma.currentPage;
const screens = page.children.filter(n => n.type === "FRAME" && n.width > 300);
const results = { annotated: [], missing: [] };

for (const screen of screens) {
  const desc = screen.description || "";
  if (desc.includes("[DATA]") || desc.includes("[data]") || desc.includes("Source:") || desc.includes("API")) {
    results.annotated.push(screen.name);
  } else {
    results.missing.push({ name: screen.name, id: screen.id });
  }
}
return {
  totalScreens: screens.length,
  annotated: results.annotated.length,
  missingAnnotation: results.missing.length,
  screens: results.missing.slice(0, 20)
};
```

### Common Figma Problems & Fixes

| Problem | Detection | Fix |
|---------|-----------|-----|
| **Spacer frames between siblings (`_sp` pattern)** | Parent has `itemSpacing: 0`, empty frames named `_sp`/`Spacer` between every child. Spacers often have wrong width (e.g., 100px fixed). | Delete all spacer frames. Set `itemSpacing` on parent to the most common spacer height. Group items with different gaps into sub-frames. See Grouping Strategy above. |
| **Elements stuck at placeholder height (e.g., 100px)** | Multiple auto layout frames have identical fixed heights regardless of content. Buttons, inputs, cards, list items all 100px tall with content only filling half. | Set `layoutSizingVertical = "HUG"` on each element. Only screen frames, images, icons, avatars, dividers should be fixed height. |
| **Icons are text characters (◉, ★, →)** | Icon frames contain TEXT nodes with unicode symbols instead of VECTOR paths. Renders inconsistently across platforms, not accessible, can't be styled. | Replace with proper vector icons from a component library (Lucide, Phosphor, etc.). Create icon components at 24×24 with vector children. |
| **Non-square icon frames** | Icon containers have mismatched width/height (e.g., 40×19, 13×15). Usually caused by HUG sizing on a text-character child. | Resize frame to square (40×40), set fixed W×H, auto layout center-center, replace text child with vector icon. |
| **Inconsistent border radii** | Same element type uses different radii across screens (buttons at 8px, 12px, 16px randomly). No radius tokens defined. | Define radius token scale (0/2/4/8/12/16/24/9999), snap all values to nearest token, bind to FLOAT variables. |
| **Zero-padding button/input** | Button or input text touches or nearly touches the frame edge. `paddingLeft` and/or `paddingRight` is 0. Common when frames are created without setting padding. | Set auto layout padding: buttons ≥16px H / ≥12px V, inputs ≥12px H / ≥12px V. Run spacing detection script (01-guide §23). |
| **Zero-margin screen content** | Content runs to the device edge on left/right. Screen content frame has `paddingLeft: 0` and `paddingRight: 0`. | Set `paddingLeft = 16` and `paddingRight = 16` on the screen's content area frame. |
| **Button/card stuck at fixed height** | Button at 100px with 14px text — huge empty space. Card at 200px clipping long content or wasting space. `layoutSizingVertical = "FIXED"` on an element that should grow. | Set `layoutSizingVertical = "HUG"`. Only screen frames, images, icons, avatars, inputs, dividers should be FIXED height. |
| **Content area not FILL** | Footer floats in middle of screen. Content area (between header and footer) has `layoutSizingVertical = "HUG"` instead of `"FILL"`. | Set `layoutSizingVertical = "FILL"` on the content area. It must expand to push footer down. |
| **Off-grid spacing values** | Padding at 13px, gap at 17px, margin at 22px. Not on the spacing token scale. Causes visual inconsistency. | Snap to nearest token: 13→12, 17→16, 22→24. All spacing must be on the 4/8-based scale. |
| Text overflows container | Text node wider than parent | Set text to `layoutSizingHorizontal = "FILL"` |
| Footer floats in middle | Content area `height = HUG` | Set content area `layoutSizingVertical = "FILL"` |
| Items overlap | Mixed auto layout + absolute position | Remove absolute positioning, integrate into auto layout |
| Inconsistent spacing | Different gaps between similar elements | Set `itemSpacing` on parent, remove per-child margins |
| Broken component | Detached instance | Find original component, replace with fresh instance |
| Colors don't match theme | Hardcoded hex values | Bind to color variables |
| Text doesn't match scale | Raw font sizes | Apply text styles |
| Screen doesn't scroll | Content area missing `clipsContent` | Set `clipsContent = true` on content frame |
| **Screen has no loading/empty/error state frames** | Run data state coverage script (Phase 9). Screen with list/card/feed children has no sibling frames named "Loading", "Empty", "Error". | Create state variant frames: Skeleton (loading), illustration + CTA (empty), error message + retry (error). Name them `ScreenName-Loading`, `ScreenName-Empty`, `ScreenName-Error`. |
| **No data source annotations on screens** | Run data annotation detection script (Phase 9). Screen frames have empty `description` field with no [DATA] tags. | Add [DATA] annotation to frame description: Source (API endpoint), Trigger (on mount/interaction), Empty/Error frame references. Use `figma_set_description` via MCP. |
| **Multi-step form has no draft persistence documentation** | Form flow has 3+ steps but no annotation about data preservation on back navigation. | Annotate each step frame: "[DATA] Draft saved to local storage on step change. Back navigation restores previous input." |
| **List screen has no pagination documentation** | Screen shows a list but no annotation about scroll behavior, load-more, or infinite scroll. | Annotate list frame: "[DATA] Pagination: infinite scroll / cursor-based / page size 20. Loading indicator at bottom. End state: 'No more items'." |

---

## 7. Flow Connectors & Annotations in Figma

This section covers the Figma implementation of user flows and annotation systems defined in 01-guide §2 and §2B.

### Building Flow Connectors

Figma Design files don't have native connectors like FigJam. Use one of these methods:

**Method 1: FigJam Arrows (Recommended)**

Copy-paste from FigJam into Figma Design. They remain editable, auto-route around objects, and support labels.

```
Steps:
1. Open a FigJam board
2. Draw connectors between sticky notes (placeholders)
3. Select all connectors → Cmd+C
4. Switch to Figma Design file → Cmd+V
5. Position arrows between your screen frames
6. Connectors auto-update when you move connected frames
```

**Method 2: Autoflow Plugin**

Free plugin that draws smart connectors between selected frames:

```
Steps:
1. Install Autoflow from Figma Community
2. Select two frames to connect
3. Run plugin → arrow auto-draws with obstacle avoidance
4. Customize stroke color, weight, and style
5. Free for up to 50 flows per file
```

**Method 3: Manual Line Components (Full Control)**

Build reusable arrow components for maximum consistency:

```javascript
// Via figma_execute — create a flow arrow component set
// Run this once to generate the arrow library

const colors = {
  happy: { r: 0.13, g: 0.73, b: 0.35 },    // Green
  error: { r: 0.91, g: 0.22, b: 0.22 },     // Red
  warning: { r: 0.96, g: 0.66, b: 0.07 },   // Amber
  edge: { r: 0.62, g: 0.64, b: 0.67 },      // Gray
  system: { r: 0.23, g: 0.51, b: 0.96 }     // Blue
};

const arrows = [];
for (const [name, color] of Object.entries(colors)) {
  const line = figma.createLine();
  line.name = `Flow / Arrow / ${name.charAt(0).toUpperCase() + name.slice(1)}`;
  line.resize(200, 0);
  line.strokes = [{ type: 'SOLID', color: color }];
  line.strokeWeight = 2;
  line.strokeCap = 'ARROW_EQUILATERAL';  // arrowhead
  arrows.push({ name: line.name, id: line.id });
}
return arrows;
```

### Flow Label Component

Create a label component to sit on top of connector arrows:

```javascript
// Via figma_execute — create a flow label component
const label = figma.createComponent();
label.name = "Flow / Arrow Label";
label.layoutMode = "HORIZONTAL";
label.primaryAxisAlignItems = "CENTER";
label.counterAxisAlignItems = "CENTER";
label.paddingLeft = 8;
label.paddingRight = 8;
label.paddingTop = 4;
label.paddingBottom = 4;
label.cornerRadius = 4;
label.fills = [{ type: 'SOLID', color: { r: 1, g: 1, b: 1 }, opacity: 0.9 }];
label.strokes = [{ type: 'SOLID', color: { r: 0.85, g: 0.85, b: 0.87 } }];
label.strokeWeight = 1;
label.layoutSizingHorizontal = "HUG";
label.layoutSizingVertical = "HUG";

const text = figma.createText();
await figma.loadFontAsync({ family: "Inter", style: "Medium" });
text.fontName = { family: "Inter", style: "Medium" };
text.fontSize = 11;
text.characters = "Tap Submit";
text.fills = [{ type: 'SOLID', color: { r: 0.38, g: 0.39, b: 0.42 } }];
label.appendChild(text);

return { id: label.id, name: label.name };
```

### Screen Number Badge Component

Place inside every screen frame at top-left per the numbering convention:

```javascript
// Via figma_execute — create a screen number badge component
const badge = figma.createComponent();
badge.name = "Annotation / Screen Number";
badge.layoutMode = "HORIZONTAL";
badge.primaryAxisAlignItems = "CENTER";
badge.counterAxisAlignItems = "CENTER";
badge.paddingLeft = 8;
badge.paddingRight = 8;
badge.paddingTop = 4;
badge.paddingBottom = 4;
badge.cornerRadius = 4;
badge.fills = [{ type: 'SOLID', color: { r: 0.15, g: 0.16, b: 0.18 }, opacity: 0.75 }];
badge.layoutSizingHorizontal = "HUG";
badge.layoutSizingVertical = "HUG";

const text = figma.createText();
await figma.loadFontAsync({ family: "Inter", style: "Bold" });
text.fontName = { family: "Inter", style: "Bold" };
text.fontSize = 10;
text.characters = "AUTH-01";
text.fills = [{ type: 'SOLID', color: { r: 1, g: 1, b: 1 } }];
text.letterSpacing = { value: 0.5, unit: "PIXELS" };
badge.appendChild(text);

return { id: badge.id, name: badge.name };
```

### Status Badge Component

```javascript
// Via figma_execute — create status badge component set
const statuses = [
  { name: "Draft",       emoji: "⚪", bg: { r: 0.93, g: 0.93, b: 0.93 }, text: { r: 0.4, g: 0.4, b: 0.4 } },
  { name: "In Review",   emoji: "🟡", bg: { r: 1, g: 0.95, b: 0.8 },     text: { r: 0.6, g: 0.45, b: 0 } },
  { name: "Needs Changes", emoji: "🔴", bg: { r: 1, g: 0.9, b: 0.9 },   text: { r: 0.7, g: 0.15, b: 0.15 } },
  { name: "Ready for Dev", emoji: "🟢", bg: { r: 0.88, g: 0.97, b: 0.88 }, text: { r: 0.1, g: 0.5, b: 0.2 } },
  { name: "In Dev",       emoji: "🔵", bg: { r: 0.88, g: 0.92, b: 1 },   text: { r: 0.15, g: 0.35, b: 0.7 } },
  { name: "Complete",     emoji: "✅", bg: { r: 0.9, g: 0.96, b: 0.9 },  text: { r: 0.2, g: 0.55, b: 0.25 } }
];

const badges = [];
for (const s of statuses) {
  const comp = figma.createComponent();
  comp.name = `Annotation / Status / ${s.name}`;
  comp.layoutMode = "HORIZONTAL";
  comp.primaryAxisAlignItems = "CENTER";
  comp.counterAxisAlignItems = "CENTER";
  comp.paddingLeft = 10; comp.paddingRight = 10;
  comp.paddingTop = 6; comp.paddingBottom = 6;
  comp.cornerRadius = 9999; // pill
  comp.fills = [{ type: 'SOLID', color: s.bg }];
  comp.layoutSizingHorizontal = "HUG";
  comp.layoutSizingVertical = "HUG";

  const txt = figma.createText();
  await figma.loadFontAsync({ family: "Inter", style: "Semi Bold" });
  txt.fontName = { family: "Inter", style: "Semi Bold" };
  txt.fontSize = 11;
  txt.characters = `${s.emoji} ${s.name}`;
  txt.fills = [{ type: 'SOLID', color: s.text }];
  comp.appendChild(txt);
  badges.push({ name: comp.name, id: comp.id });
}
return badges;
```

### Callout Note Component

For canvas annotations with category color bar:

```javascript
// Via figma_execute — create callout annotation component
const callout = figma.createComponent();
callout.name = "Annotation / Callout Note";
callout.layoutMode = "HORIZONTAL";
callout.counterAxisAlignItems = "MIN";
callout.itemSpacing = 0;
callout.cornerRadius = 8;
callout.fills = [{ type: 'SOLID', color: { r: 0.98, g: 0.98, b: 0.99 } }];
callout.strokes = [{ type: 'SOLID', color: { r: 0.88, g: 0.88, b: 0.9 } }];
callout.strokeWeight = 1;
callout.clipsContent = true;
callout.resize(220, 1); // width fixed, height hug
callout.layoutSizingHorizontal = "FIXED";
callout.layoutSizingVertical = "HUG";

// Color bar (left edge)
const bar = figma.createFrame();
bar.name = "Category Bar";
bar.resize(4, 1);
bar.fills = [{ type: 'SOLID', color: { r: 0.23, g: 0.51, b: 0.96 } }]; // blue = interaction
bar.layoutSizingHorizontal = "FIXED";
bar.layoutSizingVertical = "FILL";

// Content area
const content = figma.createFrame();
content.name = "Content";
content.layoutMode = "VERTICAL";
content.itemSpacing = 4;
content.paddingTop = 8; content.paddingBottom = 8;
content.paddingLeft = 10; content.paddingRight = 10;
content.fills = [];
content.layoutSizingHorizontal = "FILL";
content.layoutSizingVertical = "HUG";

await figma.loadFontAsync({ family: "Inter", style: "Semi Bold" });
await figma.loadFontAsync({ family: "Inter", style: "Regular" });

const title = figma.createText();
title.fontName = { family: "Inter", style: "Semi Bold" };
title.fontSize = 11;
title.characters = "INTERACTION";
title.fills = [{ type: 'SOLID', color: { r: 0.23, g: 0.51, b: 0.96 } }];

const body = figma.createText();
body.fontName = { family: "Inter", style: "Regular" };
body.fontSize = 11;
body.characters = "Swipe left to reveal delete button.\\nThreshold: 80px horizontal.";
body.fills = [{ type: 'SOLID', color: { r: 0.35, g: 0.36, b: 0.4 } }];
body.layoutSizingHorizontal = "FILL";

content.appendChild(title);
content.appendChild(body);
callout.appendChild(bar);
callout.appendChild(content);

return { id: callout.id, name: callout.name };
```

### Managing Comments Programmatically

```
Read all active comments on a file:
  Tool: figma_get_comments
  → Returns: threads with id, message, author, created_at, pinned node_id

Post a comment pinned to a specific node:
  Tool: figma_post_comment
  → message: "[LOGIC] Show error state when items.length === 0"
  → node_id: "4:328" (pins comment to that element)

Reply to an existing comment thread:
  Tool: figma_post_comment
  → message: "Fixed — empty state added. See DISPUTE-01-Empty"
  → reply_to_comment_id: "abc123"

Delete a resolved comment:
  Tool: figma_delete_comment
  → comment_id: "abc123"

Add Dev Mode descriptions to components:
  Tool: figma_set_description
  → nodeId: "4:500"
  → description: "Primary CTA button. Use variant=primary for main actions."
  → descriptionMarkdown: "**Primary CTA**\n\n- Use `variant=primary` for main page actions\n- Always pair with a secondary option"
```

**Batch annotation pattern — after an audit:**

```javascript
// Via figma_execute — batch-add descriptions to all components
const page = figma.currentPage;
const components = page.findAll(n => n.type === 'COMPONENT' || n.type === 'COMPONENT_SET');

for (const comp of components) {
  if (!comp.description || comp.description.trim() === '') {
    comp.description = `[TODO] Add description for ${comp.name}`;
  }
}
return { updated: components.length };
```

### Flow & Annotation Audit Checklist

The complete flow & annotation checks are embedded in the Full File Audit Procedure (Phase 10). The checks include:

- Dedicated "User Flows" page exists
- Flows organized in Figma Sections with title frames
- Screen number badges (FLOW-##) on all frames
- Connector arrows color-coded and labeled
- Status badges on all screens
- [INTERACTION], [LOGIC], and [DATA] annotations on all relevant elements
- All Figma comments resolved or assigned
- No orphan screens (every screen has incoming or outgoing connections)
- Component descriptions populated for Dev Mode

### Post-Build Validation

After completing a build session, recommend to the user:
> "Build complete! Run `/design:figma-autolayout-audit` to validate the auto layout structure of what we just built."

---

## 8. Export Mode: Design-to-Code Translation

### The Translation Principles

1. **Auto layout → Flexbox** (CSS) or **Flex** (React Native). Every auto layout frame becomes a flex container.
2. **Fill → flex: 1** or **width: 100%**. Figma's "fill container" maps to flex-grow or percentage widths.
3. **Hug → width: auto** or **flex-shrink: 0**. Content-sized elements.
4. **Fixed → explicit px values**. Only for icons, avatars, and images.
5. **Figma Variables → CSS Variables** or **Tailwind config** or **theme tokens**.
6. **Text Styles → CSS classes** or **typography utilities**.
7. **Components → React/Vue components**. One Figma component = one code component.
8. **Variants → Props**. Figma variant properties become component props.

### Figma-to-CSS Mapping

| Figma Property | CSS Property | Notes |
|---|---|---|
| `layoutMode: "VERTICAL"` | `display: flex; flex-direction: column;` | |
| `layoutMode: "HORIZONTAL"` | `display: flex; flex-direction: row;` | |
| `itemSpacing: 16` | `gap: 16px;` | Or `gap: var(--spacing-md)` |
| `paddingTop/Right/Bottom/Left` | `padding: T R B L;` | Use shorthand when symmetric |
| `primaryAxisAlignItems: "CENTER"` | `justify-content: center;` | Along main axis |
| `primaryAxisAlignItems: "SPACE_BETWEEN"` | `justify-content: space-between;` | |
| `counterAxisAlignItems: "CENTER"` | `align-items: center;` | Cross axis |
| `counterAxisAlignItems: "STRETCH"` | `align-items: stretch;` | Default in CSS flex |
| `layoutSizingHorizontal: "FILL"` | `flex: 1; width: 100%;` | Or `flex-grow: 1` |
| `layoutSizingHorizontal: "HUG"` | `width: auto; flex-shrink: 0;` | |
| `layoutSizingHorizontal: "FIXED"` | `width: 393px;` | Explicit value |
| `layoutSizingVertical: "FILL"` | `flex: 1; min-height: 0;` | `min-height: 0` prevents overflow |
| `clipsContent: true` | `overflow: hidden;` | Or `overflow-y: auto` for scroll |
| `layoutWrap: "WRAP"` | `flex-wrap: wrap;` | |
| `cornerRadius` | `border-radius` | Use token: `var(--radius-md)` |
| `fills[0].color` | `background-color` | Use token: `var(--bg-primary)` |
| `strokes` | `border` | `border: 1px solid var(--border-default)` |
| `effects (drop shadow)` | `box-shadow` | Map offset, blur, spread, color |
| `opacity` | `opacity` | |

### Figma-to-Tailwind Mapping

| Figma Property | Tailwind Class | Notes |
|---|---|---|
| Vertical auto layout | `flex flex-col` | |
| Horizontal auto layout | `flex flex-row` or `flex` | |
| Gap: 8px | `gap-2` | 8/4 = 2 in Tailwind scale |
| Gap: 16px | `gap-4` | |
| Padding: 16px | `p-4` | |
| Padding: 16px H, 24px V | `px-4 py-6` | |
| Fill width | `w-full` or `flex-1` | |
| Hug | `w-auto shrink-0` | |
| Fixed 393px | `w-[393px]` | Arbitrary value |
| Center main axis | `justify-center` | |
| Space between | `justify-between` | |
| Center cross axis | `items-center` | |
| Stretch | `items-stretch` | |
| Wrap | `flex-wrap` | |
| Clip content | `overflow-hidden` | |
| Scroll | `overflow-y-auto` | |
| Border radius 8px | `rounded-lg` | |
| Border radius full | `rounded-full` | |

---

## 9. Screen-to-Code Workflow

### Full Screen Export Procedure

```
STEP 1: Extract Design Data
  → Figma:get_design_context (for the screen frame node ID)
  → This returns the full node tree with styles, layout, and properties

STEP 2: Extract Variables & Styles
  → figma_get_variables (for color/spacing tokens)
  → figma_get_styles (for text styles, effect styles)

STEP 3: Map Structure
  → Walk the node tree top-down
  → Each auto layout frame → flex container
  → Each text node → text element with style class
  → Each rectangle → div with background/border
  → Each component instance → component import

STEP 4: Generate Code (choose framework)
  → See Framework-Specific Patterns (Section 12)

STEP 5: Verify
  → Compare generated code output to figma_capture_screenshot
  → Check: layout matches, colors match, typography matches, spacing matches
```

### Node-to-Element Mapping

| Figma Node Type | HTML Element | React Component |
|---|---|---|
| FRAME (auto layout) | `<div>` with flex styles | `<div>` or `<Box>` |
| TEXT | `<p>`, `<h1>`-`<h6>`, `<span>` | Based on text style name |
| RECTANGLE | `<div>` | `<div>` with background |
| ELLIPSE | `<div>` with border-radius: 50% | `<div>` |
| COMPONENT INSTANCE | Component import | `<ComponentName />` |
| IMAGE fill | `<img>` or `background-image` | `<Image />` |
| VECTOR / ICON | `<svg>` inline or icon component | `<Icon name="..." />` |
| GROUP | Unwrap — don't create wrapper divs | Flatten |

### Text Style to HTML Element

| Text Style Name | HTML Element | Semantic Meaning |
|---|---|---|
| Typography / Display | `<h1>` | Hero headline |
| Typography / H1 | `<h1>` | Page title |
| Typography / H2 | `<h2>` | Section heading |
| Typography / H3 | `<h3>` | Subsection heading |
| Typography / H4 | `<h4>` | Card title |
| Typography / Body Large | `<p class="text-lg">` | Lead paragraph |
| Typography / Body | `<p>` | Default paragraph |
| Typography / Body Small | `<p class="text-sm">` or `<span>` | Secondary text |
| Typography / Caption | `<span class="text-xs">` | Metadata, timestamps |
| Typography / Overline | `<span class="overline">` | Category label |
| Typography / Button * | Inside `<button>` | Button label |
| Typography / Label | `<label>` | Form label |
| Typography / Helper | `<span class="helper">` | Form helper text |

---

## 10. Component-to-Code Workflow

### Exporting a Single Component

```
STEP 1: Get Component Data
  → figma_get_component_for_development (includes image + layout data)
  → Or: Figma:get_design_context (for full node tree)

STEP 2: Analyze Properties
  → List all VARIANT properties → become component props (string enums)
  → List all BOOLEAN properties → become component props (boolean)
  → List all TEXT properties → become component props (string)
  → List all INSTANCE_SWAP properties → become component props (ReactNode or slot)

STEP 3: Generate Component Code

Example — Button component from Figma:

  Figma properties:
    Type: Primary | Secondary | Ghost       → prop: variant
    Size: SM | MD | LG                      → prop: size
    State: Default | Hover | Active | Disabled | Loading → handled by CSS states + disabled/loading props
    Show Icon: true | false                 → prop: showIcon / icon prop
    Label: "Button"                         → prop: children or label

  Generated React:
    interface ButtonProps {
      variant?: "primary" | "secondary" | "ghost";
      size?: "sm" | "md" | "lg";
      disabled?: boolean;
      loading?: boolean;
      icon?: React.ReactNode;
      children: React.ReactNode;
      onClick?: () => void;
    }

STEP 4: Map Visual Properties to Styles
  → Extract from each variant's fills, text styles, padding, border radius
  → Create style variants (CSS classes, styled-components, or Tailwind)

STEP 5: Handle States in Code
  → Figma "Hover" variant → CSS :hover pseudo-class
  → Figma "Active" variant → CSS :active pseudo-class
  → Figma "Focus" variant → CSS :focus-visible pseudo-class
  → Figma "Disabled" variant → [disabled] attribute + styles
  → Figma "Loading" variant → loading prop + spinner + pointer-events: none
```

### Component Props Mapping

| Figma Property | Code Prop | Type |
|---|---|---|
| Variant: Type (Primary/Secondary) | `variant` | `"primary" \| "secondary"` |
| Variant: Size (SM/MD/LG) | `size` | `"sm" \| "md" \| "lg"` |
| Boolean: Show Icon | `icon` (presence) | `ReactNode \| undefined` |
| Boolean: Show Badge | `showBadge` | `boolean` |
| Text: Label | `children` or `label` | `string \| ReactNode` |
| Text: Helper Text | `helperText` | `string` |
| Instance Swap: Icon | `icon` | `ReactNode` |
| Instance Swap: Avatar | `avatar` | `ReactNode` |
| Variant: State (Hover/Active/Disabled) | CSS states + `disabled` prop | `:hover`, `:active`, `[disabled]` |

---

## 11. Token Export

### CSS Custom Properties Export

```css
/* Generated from Figma Variables */
:root {
  /* Colors - Light Mode */
  --color-bg-primary: #FFFFFF;
  --color-bg-secondary: #F5F5F7;
  --color-text-primary: #111111;
  --color-text-secondary: #666666;
  --color-action-primary: #2563EB;
  --color-action-primary-hover: #1D4ED8;
  --color-border-default: #E5E7EB;
  --color-error: #DC2626;
  --color-success: #16A34A;

  /* Spacing */
  --spacing-xs: 4px;
  --spacing-sm: 8px;
  --spacing-md: 16px;
  --spacing-lg: 24px;
  --spacing-xl: 32px;
  --spacing-2xl: 48px;

  /* Border Radius */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 16px;
  --radius-full: 9999px;

  /* Shadows */
  --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px rgba(0, 0, 0, 0.07);
  --shadow-lg: 0 10px 15px rgba(0, 0, 0, 0.1);

  /* Typography */
  --font-family-primary: 'Inter', sans-serif;
  --font-size-xs: 12px;
  --font-size-sm: 14px;
  --font-size-base: 16px;
  --font-size-lg: 18px;
  --font-size-xl: 24px;
  --font-size-2xl: 32px;
  --font-size-3xl: 48px;
  --line-height-tight: 1.2;
  --line-height-normal: 1.5;
  --line-height-relaxed: 1.75;
}

/* Dark Mode */
@media (prefers-color-scheme: dark) {
  :root {
    --color-bg-primary: #0F0F13;
    --color-bg-secondary: #1A1A22;
    --color-text-primary: #F1F1F4;
    --color-text-secondary: #A0A0AB;
    --color-action-primary: #60A5FA;
    --color-border-default: #2D2D3A;
  }
}
```

### Tailwind Config Export

```javascript
// tailwind.config.js — generated from Figma Variables
module.exports = {
  theme: {
    extend: {
      colors: {
        bg: {
          primary: 'var(--color-bg-primary)',
          secondary: 'var(--color-bg-secondary)',
        },
        text: {
          primary: 'var(--color-text-primary)',
          secondary: 'var(--color-text-secondary)',
        },
        action: {
          primary: 'var(--color-action-primary)',
          'primary-hover': 'var(--color-action-primary-hover)',
        },
        border: {
          DEFAULT: 'var(--color-border-default)',
        },
      },
      spacing: {
        'xs': 'var(--spacing-xs)',
        'sm': 'var(--spacing-sm)',
        'md': 'var(--spacing-md)',
        'lg': 'var(--spacing-lg)',
        'xl': 'var(--spacing-xl)',
        '2xl': 'var(--spacing-2xl)',
      },
      borderRadius: {
        'sm': 'var(--radius-sm)',
        'md': 'var(--radius-md)',
        'lg': 'var(--radius-lg)',
        'full': 'var(--radius-full)',
      },
      fontFamily: {
        'primary': ['Inter', 'sans-serif'],
      },
    },
  },
};
```

### TypeScript Token Export

```typescript
// tokens.ts — generated from Figma Variables
export const colors = {
  bg: {
    primary: '#FFFFFF',
    secondary: '#F5F5F7',
  },
  text: {
    primary: '#111111',
    secondary: '#666666',
  },
  action: {
    primary: '#2563EB',
    primaryHover: '#1D4ED8',
  },
} as const;

export const spacing = {
  xs: 4,
  sm: 8,
  md: 16,
  lg: 24,
  xl: 32,
  '2xl': 48,
} as const;

export type ColorToken = typeof colors;
export type SpacingToken = typeof spacing;
```

### Automated Token Export via MCP

```
Tool: figma_get_variables
Parameters:
  format: "full"
  enrich: true
  export_formats: ["css", "tailwind", "typescript"]
  resolveAliases: true

→ Returns all variables with resolved values AND generated code for each format.
```

---

## 12. Code Quality Standards

### Generated Code Must

1. **Use semantic HTML** — `<nav>`, `<main>`, `<article>`, `<section>`, `<header>`, `<footer>`, not div soup
2. **Reference tokens** — `var(--spacing-md)` not `16px`, `var(--color-text-primary)` not `#111`
3. **Include accessibility** — `aria-label`, `role`, `tabindex`, alt text, focus styles
4. **Match Figma exactly** — spacing, colors, typography, border radius, shadows
5. **Be responsive** — if Figma has multiple screen sizes, code handles breakpoints
6. **Handle states** — hover, focus, active, disabled mapped from Figma variants
7. **Name classes semantically** — `.card-header` not `.flex-row-1`

### Code Review Checklist

```
STRUCTURE:
[ ] Semantic HTML elements used (not all divs)
[ ] Component structure matches Figma layer hierarchy
[ ] No unnecessary wrapper divs
[ ] Proper heading hierarchy (h1 → h2 → h3, not random)

STYLES:
[ ] All colors use CSS variables / Tailwind tokens
[ ] All spacing uses the spacing scale
[ ] All typography uses text style classes
[ ] All border radius uses radius tokens
[ ] No magic numbers (random px values)
[ ] Responsive breakpoints match Figma frames

INTERACTION:
[ ] Hover states match Figma hover variants
[ ] Focus styles visible and accessible
[ ] Disabled states match Figma disabled variants
[ ] Loading states implemented
[ ] Transitions/animations specified (duration, easing)

ACCESSIBILITY:
[ ] All images have alt text
[ ] All interactive elements are keyboard accessible
[ ] Color contrast meets WCAG AA (4.5:1 for text)
[ ] Focus order is logical
[ ] ARIA labels on icon-only buttons
[ ] Form inputs have associated labels

COMPLETENESS:
[ ] All Figma component properties mapped to code props
[ ] Edge cases handled (empty, error, max content)
[ ] Dark mode supported (if Figma has dark mode variables)
```

---

## 13. Framework-Specific Patterns

### React (JSX + CSS Modules or Tailwind)

```jsx
// Screen component — from Figma screen frame
export function DisputeDetailScreen() {
  return (
    <div className="screen">
      <Header title="Dispute Details" showBack />
      <main className="content">
        <DisputeCard dispute={dispute} />
        <EvidenceSection items={evidence} />
        <ActionButtons onResolve={handleResolve} />
      </main>
      <BottomNav active="disputes" />
    </div>
  );
}

// Styles mapping from Figma auto layout
.screen {
  display: flex;
  flex-direction: column;
  width: 393px; /* or 100% for responsive */
  height: 100vh;
}

.content {
  flex: 1;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  gap: var(--spacing-lg);
  padding: var(--spacing-md);
}
```

### React with Tailwind

```jsx
export function DisputeDetailScreen() {
  return (
    <div className="flex flex-col w-full h-screen">
      <Header title="Dispute Details" showBack />
      <main className="flex-1 overflow-y-auto flex flex-col gap-6 p-4">
        <DisputeCard dispute={dispute} />
        <EvidenceSection items={evidence} />
        <ActionButtons onResolve={handleResolve} />
      </main>
      <BottomNav active="disputes" />
    </div>
  );
}
```

### Vue 3 (Composition API)

```vue
<template>
  <div class="flex flex-col w-full h-screen">
    <AppHeader title="Dispute Details" show-back />
    <main class="flex-1 overflow-y-auto flex flex-col gap-6 p-4">
      <DisputeCard :dispute="dispute" />
      <EvidenceSection :items="evidence" />
      <ActionButtons @resolve="handleResolve" />
    </main>
    <BottomNav active="disputes" />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
// ...
</script>
```

### HTML + CSS (No Framework)

```html
<div class="screen">
  <header class="header">
    <button class="back-btn" aria-label="Go back">←</button>
    <h1 class="header-title">Dispute Details</h1>
    <button class="action-btn" aria-label="More options">⋯</button>
  </header>
  <main class="content">
    <!-- sections here -->
  </main>
  <nav class="bottom-nav" aria-label="Main navigation">
    <!-- nav items -->
  </nav>
</div>

<style>
.screen {
  display: flex;
  flex-direction: column;
  height: 100vh;
  max-width: 393px;
  margin: 0 auto;
}
.header {
  display: flex;
  align-items: center;
  padding: var(--spacing-md);
  gap: var(--spacing-sm);
  flex-shrink: 0;
}
.content {
  flex: 1;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  gap: var(--spacing-lg);
  padding: var(--spacing-md);
}
.bottom-nav {
  display: flex;
  justify-content: space-around;
  padding: var(--spacing-sm) var(--spacing-md);
  padding-bottom: calc(var(--spacing-sm) + env(safe-area-inset-bottom));
  flex-shrink: 0;
  border-top: 1px solid var(--color-border-default);
}
</style>
```

---

## 14. Responsive Code from Figma

### Multi-Breakpoint Translation

If the Figma file has screens at multiple widths (e.g., 393px mobile, 768px tablet, 1440px desktop):

```
STEP 1: Extract each breakpoint's design data
  → Figma:get_design_context for mobile frame
  → Figma:get_design_context for tablet frame
  → Figma:get_design_context for desktop frame

STEP 2: Identify what changes between breakpoints
  → Layout direction changes (vertical → horizontal)
  → Column count changes (1 → 2 → 3)
  → Elements that show/hide at different widths
  → Spacing scale changes
  → Navigation pattern changes (bottom tab → sidebar)

STEP 3: Generate responsive CSS
  → Mobile-first: base styles = mobile
  → @media (min-width: 768px) { tablet overrides }
  → @media (min-width: 1280px) { desktop overrides }
```

### Common Responsive Patterns from Figma

| Figma Pattern | CSS Implementation |
|---|---|
| Single column (mobile) → 2 columns (tablet) → 3 columns (desktop) | CSS Grid: `grid-template-columns: 1fr` → `repeat(2, 1fr)` → `repeat(3, 1fr)` |
| Bottom tabs (mobile) → Sidebar (desktop) | Hide/show with media queries + flex-direction change |
| Stacked cards (mobile) → Horizontal cards (desktop) | `flex-direction: column` → `flex-direction: row` |
| Full-width button (mobile) → Auto-width (desktop) | `width: 100%` → `width: auto` |
| Single hero (mobile) → Split hero + image (desktop) | Single column → `grid-template-columns: 1fr 1fr` |

---

## 15. Figma MCP Tool Reference

### Quick Reference — Which Tool for What

**Reading (non-destructive):**

| Goal | Tool |
|------|------|
| Get full design data for a node | `Figma:get_design_context` |
| Get screenshot of current state | `figma_capture_screenshot` |
| Get screenshot via REST API | `figma_take_screenshot` |
| Get component details for coding | `figma_get_component_for_development` |
| Get component metadata | `figma_get_component` |
| Search for components | `figma_search_components` |
| Get all variables/tokens | `figma_get_variables` |
| Get all styles | `figma_get_styles` |
| Get file structure | `figma_get_file_data` |
| Get what's selected | `figma_get_selection` |
| Get design system overview | `figma_get_design_system_summary` |
| Check design vs code parity | `figma_check_design_parity` |

**Writing (modifies the file):**

| Goal | Tool |
|------|------|
| Create frames/shapes/text | `figma_create_child` |
| Set text content | `figma_set_text` |
| Set fill colors | `figma_set_fills` |
| Set strokes/borders | `figma_set_strokes` |
| Move a node | `figma_move_node` |
| Resize a node | `figma_resize_node` |
| Rename a node | `figma_rename_node` |
| Clone a node | `figma_clone_node` |
| Delete a node | `figma_delete_node` |
| Set component instance properties | `figma_set_instance_properties` |
| Instantiate a component | `figma_instantiate_component` |
| Add component property | `figma_add_component_property` |
| Set descriptions | `figma_set_description` |
| Create variables (single) | `figma_create_variable` |
| Create variables (batch) | `figma_batch_create_variables` |
| Update variables (batch) | `figma_batch_update_variables` |
| Full token setup | `figma_setup_design_tokens` |
| Arrange component set | `figma_arrange_component_set` |
| Run any Figma API code | `figma_execute` |

**Generating documentation:**

| Goal | Tool |
|------|------|
| Generate component docs | `figma_generate_component_doc` |
| Export token code (CSS/Tailwind/TS) | `figma_get_variables` with `export_formats` |
| Export style code | `figma_get_styles` with `enrich: true` |
| Audit design system | `figma_audit_design_system` |
| Browse tokens interactively | `figma_browse_tokens` |
