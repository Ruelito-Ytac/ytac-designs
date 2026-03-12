---
name: figma-repair
description: >
  Detect and fix structural issues in Figma files. Includes detection scripts for spacers,
  placeholder heights, raw values, off-grid spacing, and wrong sizing modes. Plus bulk fix
  scripts and the 11-phase file audit procedure. Use when the user says "fix Figma", "clean up",
  "audit file", "find issues", or "repair layout".

  IMPORTANT: Always run detection FIRST, present findings, then fix with user approval.
---

# Figma File Repair & Cleanup

> **Purpose**: Detect and fix structural issues in Figma files. Runs detection scripts first,
> presents findings, then applies batch fixes with user approval.
>
> **Philosophy**: Detect → Report → Confirm → Fix → Verify. Never auto-fix without showing
> the user what will change first.

---

## Repair Workflow

1. **Detect** — Run detection scripts to find issues (spacers, placeholder heights, raw values, etc.)
2. **Report** — Present findings with counts and examples
3. **Confirm** — Ask user which fixes to apply: "I found 12 spacer frames and 8 placeholder heights. Want me to fix both?"
4. **Fix** — Run batch fix scripts for approved issues
5. **Verify** — ONE screenshot per screen to confirm fixes look correct

### Smart Verification

- After fixes, take ONE screenshot per affected screen — not per fixed element.
- If fixing multiple screens, batch screenshots.
- Only re-screenshot screens where fixes were actually applied.

---

## Auto Layout Repair Workflow

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

---

## The Grouping Strategy for Mixed Gaps

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

---

## Detection Scripts

### Find All Spacer Frames

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

### Find All Placeholder-Height Elements

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

### Detect Zero-Padding, Off-Grid Spacing, and Wrong Sizing Modes

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

### Find All Raw Text (No Style Binding)

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

### Find All Raw Colors (No Variable Binding)

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

### Find Off-Scale Border Radii

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

## Bulk Fix Scripts

### Remove Spacers and Set Gaps

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

### Convert Placeholder Heights to HUG

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

### Set Minimum Padding on Zero-Padded Elements

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

## Full File Audit Procedure

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
  → Run spacing detection script via figma_execute
  → Flag: ALL elements with zero horizontal or vertical padding that should have padding
    (buttons, inputs, cards, list items, chips, modals, bottom sheets, toasts, tooltips)
  → Flag: Screen content frames with 0px left or right padding (content touches device edge)
  → Flag: ALL off-grid padding/gap values (not on 2/4/8/12/16/20/24/32/48/64 scale)
  → Flag: Buttons, cards, text containers, chips at FIXED height (should be HUG)
  → Flag: Content areas (between header + footer) at HUG height (should be FILL)
  → Flag: Input fields at inconsistent heights (should all be same FIXED height)
  → Fix zero-padding: Set minimum padding per element type (see minimums table)
  → Fix off-grid values: Snap to nearest spacing token
  → Fix sizing: Convert FIXED → HUG on content elements, HUG → FILL on content areas

PHASE 4: Icon Audit
  → Run icon detection script — find text-character icons, non-square frames, off-scale sizes
  → Replace text-character icons with proper vector icon component instances
  → Fix non-square icon frames (resize to square, e.g., 40×19 → 40×40 with centered content)
  → Snap off-scale sizes to the icon scale (12/16/20/24/32/40/48)
  → Verify all icon containers are auto layout, center-center, fixed square dimensions

PHASE 5: Border Radius Audit
  → Run radius detection script — find off-scale radius values
  → Snap each off-scale value to nearest token (e.g., 14px → 12px or 16px, 18px → 16px, 22px → 24px)
  → Verify nesting rule: inner radius < outer radius (inner = outer − gap)
  → Check bottom sheets use top-only rounding
  → Bind all radius values to FLOAT variables (no raw numbers)

PHASE 6: Style Compliance Audit
  → Run the raw text scanner
  → Run the raw color scanner
  → For each raw value: match to closest existing style and apply

PHASE 7: Component Audit
  → Flag: repeated identical structures that aren't components
  → Flag: detached instances (should be re-linked)
  → Flag: components without descriptions

PHASE 8: Naming Audit
  → Flag: any layer named "Frame ###" or "Group ###" or "Rectangle ###"
  → Suggest semantic names based on content/position

PHASE 9: Data Flow Audit
  → For each screen: verify data source annotation exists ([DATA] format)
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

---

## Data Flow Detection Scripts

### Detect Missing State Frames

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

### Detect Missing Data Annotations

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

---

## Common Figma Problems & Fixes

| Problem | Detection | Fix |
|---------|-----------|-----|
| **Spacer frames between siblings (`_sp` pattern)** | Parent has `itemSpacing: 0`, empty frames named `_sp`/`Spacer` between every child. Spacers often have wrong width (e.g., 100px fixed). | Delete all spacer frames. Set `itemSpacing` on parent to the most common spacer height. Group items with different gaps into sub-frames. See Grouping Strategy above. |
| **Elements stuck at placeholder height (e.g., 100px)** | Multiple auto layout frames have identical fixed heights regardless of content. Buttons, inputs, cards, list items all 100px tall with content only filling half. | Set `layoutSizingVertical = "HUG"` on each element. Only screen frames, images, icons, avatars, dividers should be fixed height. |
| **Icons are text characters (special characters)** | Icon frames contain TEXT nodes with unicode symbols instead of VECTOR paths. Renders inconsistently across platforms, not accessible, can't be styled. | Replace with proper vector icons from a component library (Lucide, Phosphor, etc.). Create icon components at 24x24 with vector children. |
| **Non-square icon frames** | Icon containers have mismatched width/height (e.g., 40x19, 13x15). Usually caused by HUG sizing on a text-character child. | Resize frame to square (40x40), set fixed WxH, auto layout center-center, replace text child with vector icon. |
| **Inconsistent border radii** | Same element type uses different radii across screens (buttons at 8px, 12px, 16px randomly). No radius tokens defined. | Define radius token scale (0/2/4/8/12/16/24/9999), snap all values to nearest token, bind to FLOAT variables. |
| **Zero-padding button/input** | Button or input text touches or nearly touches the frame edge. `paddingLeft` and/or `paddingRight` is 0. Common when frames are created without setting padding. | Set auto layout padding: buttons >=16px H / >=12px V, inputs >=12px H / >=12px V. Run spacing detection script. |
| **Zero-margin screen content** | Content runs to the device edge on left/right. Screen content frame has `paddingLeft: 0` and `paddingRight: 0`. | Set `paddingLeft = 16` and `paddingRight = 16` on the screen's content area frame. |
| **Button/card stuck at fixed height** | Button at 100px with 14px text — huge empty space. Card at 200px clipping long content or wasting space. `layoutSizingVertical = "FIXED"` on an element that should grow. | Set `layoutSizingVertical = "HUG"`. Only screen frames, images, icons, avatars, inputs, dividers should be FIXED height. |
| **Content area not FILL** | Footer floats in middle of screen. Content area (between header and footer) has `layoutSizingVertical = "HUG"` instead of `"FILL"`. | Set `layoutSizingVertical = "FILL"` on the content area. It must expand to push footer down. |
| **Off-grid spacing values** | Padding at 13px, gap at 17px, margin at 22px. Not on the spacing token scale. Causes visual inconsistency. | Snap to nearest token: 13->12, 17->16, 22->24. All spacing must be on the 4/8-based scale. |
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

## Flow Connectors & Annotations in Figma

This section covers the Figma implementation of user flows and annotation systems.

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
body.characters = "Swipe left to reveal delete button.\nThreshold: 80px horizontal.";
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

**Batch annotation pattern -- after an audit:**

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
