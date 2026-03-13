---
name: figma-build-core
description: >
  Lean Figma build skill. Screen construction, auto layout mastery, and batch-first build patterns.
  Use when the user says "build in Figma", "create screen", "add auto layout", or any task involving
  Figma frame construction. For component creation with variants/properties, load 04b-component-architect.md.
  For code export, load 04c-code-export.md. For file repair, load 04d-figma-repair.md.

  IMPORTANT: Read project/APP_PLAN.md for project context first.
---

# Figma Build Core

> **Purpose**: Build production-ready Figma screens using proper auto layout, batch-first construction,
> and the Golden Rules. This is the lean build skill — no component architecture, no code export, no repair workflows.

---

## 1. Build Mode: Figma Production Rules

### The Golden Rules

Every Figma file built or touched by this skill MUST follow these rules. No exceptions.

1. **100% Auto Layout** — Every frame from screen level to deepest child uses auto layout. Zero manual positioning.
2. **Reuse First, Zero Raw Values** — Before building anything, query the file's existing text styles (`figma_get_styles`), color variables (`figma_get_variables`), and components (`figma_search_components`). Use what already exists. Every text layer references an existing Text Style. Every color fill references an existing Color Style or Variable. Every spacing value comes from the spacing scale. Only create new styles, variables, or components when nothing suitable exists — and when you do, follow the existing naming conventions.
3. **Component Everything** — If an element appears more than once, it's a component. If it has states, it's a variant set.
4. **Name Everything** — No "Frame 427", no "Rectangle 12". Every layer has a semantic name.
5. **Nest Intentionally** — Maximum 6 levels of nesting. Deeper = extract to component.
6. **No Spacer Frames** — Never use empty frames (`_sp`, `Spacer`, etc.) between siblings for spacing. Always use auto layout `gap` (`itemSpacing`) on the parent. If children need different gaps, group related items into sub-frames with their own gap value — and ensure the sub-frame's inner gap is smaller than the parent's outer gap (proximity clustering).
7. **No Placeholder Heights** — Every element's height must match its content (HUG). Only screen frames, images, icons, avatars, and dividers get fixed heights. An input field at 100px with 48px of content is a bug.
8. **Equal Width × Height on Circular/Square Elements** — Any frame intended to be circular or square MUST have `width = height`. Use `FIXED` on BOTH axes. Never `HUG`. This applies to ALL such elements: avatars, icon containers, dots, badges, input cells meant to be square, stat containers, any frame with `cornerRadius ≥ 9999`. A 40×19 "circle" is a bug. See build pattern below.
9. **No Truncated Labels** — Every text label must be fully visible. Use `fill width` on text inside auto layout. If a label shows "Date Of Bi..." instead of "Date of Birth", the text sizing is broken. Labels must never clip.
10. **Multi-Step Flows Need Progress Indicators** — Any flow with 2+ sequential screens (carousels, wizards, onboarding) MUST show the user's position (pagination dots, step bar, progress indicator). Missing indicators = the user is lost.
11. **Consistent Navigation Across a Flow** — Back buttons, headers, and navigation elements must use the same pattern on every screen within a flow. If screen A uses "← Back" text, screen B cannot use just "←". Pick one pattern and apply it everywhere.
12. **80px Between Screen Frames** — When building multiple screens (user flows, multi-step flows), every new screen frame must be placed 80px to the right of the previous screen. Never stack screens on top of each other. Before creating a new screen, find the rightmost existing screen on the page and offset the new frame by `previous.x + previous.width + 80`.

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
[ ] icon_page_name is set in APP_PLAN.md — if NOT set, ASK the user NOW: "Which Figma page contains your project's icons? (e.g., 'UI Icons', 'Assets/Icons')" and store in APP_PLAN.md before proceeding

Missing any of these? → Run governance.md Step 3 first.
⚠️ If icon_page_name is not set, you MUST ask the user before building any screen. Do not skip this.
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

### Icon Page Integration (HARD GATE)

**🛑 This is a HARD GATE. You CANNOT build any screen until icons are discovered and mapped. No exceptions.**

**Before the first screen in any session:**

1. Read `icon_page_name` from `project/APP_PLAN.md`
2. **If NOT set → STOP. Ask the user:** "Which Figma page contains your project's icons? (e.g., 'UI Icons', 'Assets/Icons')" — Store in APP_PLAN.md, then continue.
3. **Fetch the full icon inventory** — Run the "List All Available Icons" script from §22 in `01-ref-design-patterns.md` via `figma_execute`. Store the results (icon names + IDs).
4. **For each screen:** Before building, list every icon the screen needs. Match each to the icon inventory. Present the mapping to the user and wait for confirmation.
5. **Use ONLY `figma_instantiate_component`** with the icon component IDs from the inventory. Never create shapes, text characters, or emoji as icon substitutes.
6. If an icon doesn't exist on the page → tell the user. Ask: "Should I create this icon on the icon page, or use an alternative?" Do NOT silently skip it or substitute.
7. **For subsequent screens in the same flow:** Reuse the stored icon inventory (no re-fetch needed). Still present the icon mapping for each new screen so the user can confirm.

**Why this is a hard gate:** Without it, screens get built with fake icons (emoji, text characters, raw shapes) that don't update with the design system and look broken in Dev Mode handoff.

### Building Circular / Square Elements (Golden Rule 8)

Any frame that should appear as a circle or square MUST have `width = height`. This applies universally — not just to icons.

```javascript
// Via figma_execute — the ONLY correct way to create any circular/square element
const el = figma.createFrame();
const SIZE = 40; // pick the appropriate size

// ⚠️ width and height MUST be identical
el.resize(SIZE, SIZE);

// Auto layout for centering children
el.layoutMode = "VERTICAL";
el.primaryAxisAlignItems = "CENTER";
el.counterAxisAlignItems = "CENTER";

// ⚠️ BOTH axes MUST be FIXED — never HUG on circular/square elements
el.layoutSizingHorizontal = "FIXED";
el.layoutSizingVertical = "FIXED";

// Circle = 9999, Rounded square = 8
el.cornerRadius = 9999;
```

**Why this breaks:** Setting `layoutSizingVertical = "HUG"` or `layoutSizingHorizontal = "HUG"` on a frame with children causes the frame to resize to its content. A 40×40 circle becomes 40×19 (oval) when the child is shorter than the container. Text children are especially dangerous — they make height vary with font size.

**The rules:**
1. Always `.resize(SIZE, SIZE)` with identical W and H values
2. Always set BOTH `layoutSizingHorizontal` and `layoutSizingVertical` to `"FIXED"`
3. Never use `HUG` on any frame that should be circular or square
4. Center children via auto layout alignment — never by changing the container dimensions
5. If you need text next to a circular element, put the text OUTSIDE in a wrapper frame — never inside the circle

**Anti-patterns:**
- ❌ `el.resize(40, 24)` — non-square
- ❌ `layoutSizingVertical = "HUG"` on a circle — collapses height
- ❌ Setting only width OR only height — the other stays at default
- ❌ Adding text as a child of a circular frame — text height varies, breaks the square

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
STEP 0: Query Existing Assets
  → figma_get_styles (load all text styles + effect styles)
  → figma_get_variables (load all color + spacing variables)
  → figma_search_components (load available components)
  → Fetch icon inventory from the icon page (see Icon Discovery below)
  → Store ALL of these in memory — reference them in every subsequent step
  → NEVER create a new style/variable/component if a matching one already exists

STEP 0.5: Icon Discovery & Mapping (HARD GATE — do NOT skip)
  → Read icon_page_name from project/APP_PLAN.md
  → If NOT set → STOP. Ask the user: "Which Figma page has your icons?"
     Store in APP_PLAN.md before continuing.
  → figma_execute: List ALL icon components on the icon page (see §22 script)
  → Analyze the screen being built — list every icon it will need
  → Present the mapping to the user:
     "This screen needs these icons:
      - Back arrow → Icon / Chevron Left (found ✓)
      - Search → Icon / Search (found ✓)
      - Settings → ⚠️ Not found on icon page
      Should I proceed with the found icons? What about Settings?"
  → Wait for user confirmation before proceeding
  → Store the confirmed icon mapping — use ONLY these icons when building
  → For subsequent screens in the same flow, reuse the stored icon inventory
     (no need to re-fetch, but still present the mapping for each new screen)

STEP 1: Create Screen Frame (with canvas positioning)
  → figma_create_child: FRAME, name "Screen / [Name]"
  → figma_execute: Set layoutMode = "VERTICAL", width = 393 (mobile), height = 852
  → figma_execute: Set primaryAxisSizingMode = "FIXED" (fixed height)
  → figma_execute: Set counterAxisSizingMode = "FIXED" (fixed width)
  → figma_execute: Set clipsContent = true
  → figma_execute: Position the frame 80px to the right of the last screen:
     const page = figma.currentPage;
     const screens = page.children.filter(n => n.type === "FRAME" && n.width >= 300);
     if (screens.length > 1) {
       const prev = screens[screens.length - 2]; // previous screen
       const newScreen = screens[screens.length - 1]; // just created
       newScreen.x = prev.x + prev.width + 80;
       newScreen.y = prev.y; // same vertical position
     }
  → NEVER place a screen at (0,0) if other screens already exist on the page

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
Is this element meant to be circular or square (W must equal H)?
├─ YES → BOTH layoutSizingHorizontal AND layoutSizingVertical = "FIXED"
│   → resize(SIZE, SIZE) — width MUST equal height. Never HUG.
│   → See "Building Circular / Square Elements" section above.
│
Should this element stretch to fill its parent?
├─ YES → layoutSizingHorizontal = "FILL" (or vertical)
│   Examples: buttons in a card, text in a row, content area height
│
├─ NO, it should be exactly its content size
│  → layoutSizingHorizontal = "HUG"
│   Examples: tags, badges, headers, footers
│
└─ NO, it needs a specific pixel size
   → layoutSizingHorizontal = "FIXED" + set explicit width
    Examples: screen width (393px), dividers (1px height)
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

---

## 3. Batch-First Build Templates

**The key innovation:** Instead of making 5+ separate MCP calls to create one element, use a single `figma_execute` call that builds the complete element in one shot. This dramatically reduces round-trips, avoids partial state, and makes builds faster.

### Button (Primary)

```javascript
// BATCH: Create a complete button in ONE call
const btn = figma.createFrame();
btn.name = "Button / Primary";
btn.layoutMode = "HORIZONTAL";
btn.primaryAxisAlignItems = "CENTER";
btn.counterAxisAlignItems = "CENTER";
btn.paddingLeft = 16; btn.paddingRight = 16;
btn.paddingTop = 12; btn.paddingBottom = 12;
btn.cornerRadius = 8;
btn.itemSpacing = 8;
btn.layoutSizingHorizontal = "HUG";
btn.layoutSizingVertical = "HUG";
btn.fills = [{ type: 'SOLID', color: { r: 0.23, g: 0.51, b: 0.96 } }];

await figma.loadFontAsync({ family: "Inter", style: "Semi Bold" });
const label = figma.createText();
label.fontName = { family: "Inter", style: "Semi Bold" };
label.fontSize = 14;
label.characters = "Button Label";
label.fills = [{ type: 'SOLID', color: { r: 1, g: 1, b: 1 } }];
btn.appendChild(label);
return { id: btn.id, name: btn.name };
```

### Header Row

```javascript
// BATCH: Create a complete header row in ONE call
const header = figma.createFrame();
header.name = "Header";
header.layoutMode = "HORIZONTAL";
header.primaryAxisAlignItems = "SPACE_BETWEEN";
header.counterAxisAlignItems = "CENTER";
header.paddingLeft = 16; header.paddingRight = 16;
header.paddingTop = 54; header.paddingBottom = 12;
header.layoutSizingHorizontal = "FILL";
header.layoutSizingVertical = "HUG";
header.fills = [{ type: 'SOLID', color: { r: 1, g: 1, b: 1 } }];

await figma.loadFontAsync({ family: "Inter", style: "Semi Bold" });
const title = figma.createText();
title.fontName = { family: "Inter", style: "Semi Bold" };
title.fontSize = 18;
title.characters = "Screen Title";
title.fills = [{ type: 'SOLID', color: { r: 0.067, g: 0.094, b: 0.153 } }];
header.appendChild(title);

// Right action placeholder (icon container)
const action = figma.createFrame();
action.name = "Action";
action.resize(24, 24);
action.layoutMode = "VERTICAL";
action.primaryAxisAlignItems = "CENTER";
action.counterAxisAlignItems = "CENTER";
action.layoutSizingHorizontal = "FIXED";
action.layoutSizingVertical = "FIXED";
action.fills = [];
header.appendChild(action);

return { id: header.id, name: header.name };
```

### List Item

```javascript
// BATCH: Create a complete list item in ONE call
const item = figma.createFrame();
item.name = "List Item";
item.layoutMode = "HORIZONTAL";
item.primaryAxisAlignItems = "SPACE_BETWEEN";
item.counterAxisAlignItems = "CENTER";
item.paddingLeft = 16; item.paddingRight = 16;
item.paddingTop = 12; item.paddingBottom = 12;
item.layoutSizingHorizontal = "FILL";
item.layoutSizingVertical = "HUG";
item.itemSpacing = 12;
item.fills = [{ type: 'SOLID', color: { r: 1, g: 1, b: 1 } }];

// Left section: icon + text stack
const leftSection = figma.createFrame();
leftSection.name = "Left Content";
leftSection.layoutMode = "HORIZONTAL";
leftSection.counterAxisAlignItems = "CENTER";
leftSection.itemSpacing = 12;
leftSection.layoutSizingHorizontal = "FILL";
leftSection.layoutSizingVertical = "HUG";
leftSection.fills = [];

// Icon container
const iconContainer = figma.createFrame();
iconContainer.name = "Icon Container";
iconContainer.resize(40, 40);
iconContainer.layoutMode = "VERTICAL";
iconContainer.primaryAxisAlignItems = "CENTER";
iconContainer.counterAxisAlignItems = "CENTER";
iconContainer.layoutSizingHorizontal = "FIXED";
iconContainer.layoutSizingVertical = "FIXED";
iconContainer.cornerRadius = 8;
iconContainer.fills = [{ type: 'SOLID', color: { r: 0.96, g: 0.96, b: 0.97 } }];
leftSection.appendChild(iconContainer);

// Text stack
const textStack = figma.createFrame();
textStack.name = "Text Stack";
textStack.layoutMode = "VERTICAL";
textStack.itemSpacing = 2;
textStack.layoutSizingHorizontal = "FILL";
textStack.layoutSizingVertical = "HUG";
textStack.fills = [];

await figma.loadFontAsync({ family: "Inter", style: "Medium" });
await figma.loadFontAsync({ family: "Inter", style: "Regular" });

const primary = figma.createText();
primary.fontName = { family: "Inter", style: "Medium" };
primary.fontSize = 16;
primary.characters = "List Item Title";
primary.fills = [{ type: 'SOLID', color: { r: 0.067, g: 0.094, b: 0.153 } }];
primary.layoutSizingHorizontal = "FILL";
textStack.appendChild(primary);

const secondary = figma.createText();
secondary.fontName = { family: "Inter", style: "Regular" };
secondary.fontSize = 14;
secondary.characters = "Secondary text";
secondary.fills = [{ type: 'SOLID', color: { r: 0.42, g: 0.45, b: 0.49 } }];
secondary.layoutSizingHorizontal = "FILL";
textStack.appendChild(secondary);

leftSection.appendChild(textStack);
item.appendChild(leftSection);

// Right chevron placeholder
const chevron = figma.createFrame();
chevron.name = "Chevron";
chevron.resize(20, 20);
chevron.layoutMode = "VERTICAL";
chevron.primaryAxisAlignItems = "CENTER";
chevron.counterAxisAlignItems = "CENTER";
chevron.layoutSizingHorizontal = "FIXED";
chevron.layoutSizingVertical = "FIXED";
chevron.fills = [];
item.appendChild(chevron);

return { id: item.id, name: item.name };
```

### Card

```javascript
// BATCH: Create a complete card in ONE call
const card = figma.createFrame();
card.name = "Card";
card.layoutMode = "VERTICAL";
card.layoutSizingHorizontal = "FILL";
card.layoutSizingVertical = "HUG";
card.cornerRadius = 12;
card.clipsContent = true;
card.fills = [{ type: 'SOLID', color: { r: 1, g: 1, b: 1 } }];
card.strokes = [{ type: 'SOLID', color: { r: 0.898, g: 0.906, b: 0.922 } }];
card.strokeWeight = 1;

// Image area
const imageArea = figma.createFrame();
imageArea.name = "Card Image";
imageArea.resize(361, 200);
imageArea.layoutSizingHorizontal = "FILL";
imageArea.layoutSizingVertical = "FIXED";
imageArea.fills = [{ type: 'SOLID', color: { r: 0.95, g: 0.96, b: 0.97 } }];
card.appendChild(imageArea);

// Content area
const content = figma.createFrame();
content.name = "Card Content";
content.layoutMode = "VERTICAL";
content.paddingLeft = 16; content.paddingRight = 16;
content.paddingTop = 16; content.paddingBottom = 16;
content.itemSpacing = 8;
content.layoutSizingHorizontal = "FILL";
content.layoutSizingVertical = "HUG";
content.fills = [];

await figma.loadFontAsync({ family: "Inter", style: "Semi Bold" });
await figma.loadFontAsync({ family: "Inter", style: "Regular" });

const title = figma.createText();
title.fontName = { family: "Inter", style: "Semi Bold" };
title.fontSize = 18;
title.characters = "Card Title";
title.fills = [{ type: 'SOLID', color: { r: 0.067, g: 0.094, b: 0.153 } }];
title.layoutSizingHorizontal = "FILL";
content.appendChild(title);

const desc = figma.createText();
desc.fontName = { family: "Inter", style: "Regular" };
desc.fontSize = 14;
desc.characters = "Card description text goes here. This can wrap to multiple lines.";
desc.fills = [{ type: 'SOLID', color: { r: 0.294, g: 0.333, b: 0.388 } }];
desc.layoutSizingHorizontal = "FILL";
content.appendChild(desc);

card.appendChild(content);
return { id: card.id, name: card.name };
```

### Input Field

```javascript
// BATCH: Create a complete input field in ONE call
const inputWrapper = figma.createFrame();
inputWrapper.name = "Input / Text";
inputWrapper.layoutMode = "VERTICAL";
inputWrapper.itemSpacing = 6;
inputWrapper.layoutSizingHorizontal = "FILL";
inputWrapper.layoutSizingVertical = "HUG";
inputWrapper.fills = [];

await figma.loadFontAsync({ family: "Inter", style: "Medium" });
await figma.loadFontAsync({ family: "Inter", style: "Regular" });

// Label
const label = figma.createText();
label.fontName = { family: "Inter", style: "Medium" };
label.fontSize = 14;
label.characters = "Label";
label.fills = [{ type: 'SOLID', color: { r: 0.067, g: 0.094, b: 0.153 } }];
label.layoutSizingHorizontal = "FILL";
inputWrapper.appendChild(label);

// Input box
const inputBox = figma.createFrame();
inputBox.name = "Input Box";
inputBox.layoutMode = "HORIZONTAL";
inputBox.counterAxisAlignItems = "CENTER";
inputBox.paddingLeft = 12; inputBox.paddingRight = 12;
inputBox.paddingTop = 12; inputBox.paddingBottom = 12;
inputBox.cornerRadius = 8;
inputBox.layoutSizingHorizontal = "FILL";
inputBox.layoutSizingVertical = "HUG";
inputBox.fills = [{ type: 'SOLID', color: { r: 1, g: 1, b: 1 } }];
inputBox.strokes = [{ type: 'SOLID', color: { r: 0.816, g: 0.831, b: 0.855 } }];
inputBox.strokeWeight = 1;

const placeholder = figma.createText();
placeholder.fontName = { family: "Inter", style: "Regular" };
placeholder.fontSize = 16;
placeholder.characters = "Placeholder text";
placeholder.fills = [{ type: 'SOLID', color: { r: 0.612, g: 0.639, b: 0.686 } }];
placeholder.layoutSizingHorizontal = "FILL";
inputBox.appendChild(placeholder);

inputWrapper.appendChild(inputBox);
return { id: inputWrapper.id, name: inputWrapper.name };
```

### Bottom Navigation Item

```javascript
// BATCH: Create a complete bottom nav item in ONE call
const navItem = figma.createFrame();
navItem.name = "Nav Item";
navItem.layoutMode = "VERTICAL";
navItem.primaryAxisAlignItems = "CENTER";
navItem.counterAxisAlignItems = "CENTER";
navItem.itemSpacing = 4;
navItem.layoutSizingHorizontal = "FILL";
navItem.layoutSizingVertical = "HUG";
navItem.fills = [];

// Icon placeholder
const icon = figma.createFrame();
icon.name = "Nav Icon";
icon.resize(24, 24);
icon.layoutMode = "VERTICAL";
icon.primaryAxisAlignItems = "CENTER";
icon.counterAxisAlignItems = "CENTER";
icon.layoutSizingHorizontal = "FIXED";
icon.layoutSizingVertical = "FIXED";
icon.fills = [];
navItem.appendChild(icon);

await figma.loadFontAsync({ family: "Inter", style: "Medium" });
const navLabel = figma.createText();
navLabel.fontName = { family: "Inter", style: "Medium" };
navLabel.fontSize = 11;
navLabel.characters = "Tab";
navLabel.fills = [{ type: 'SOLID', color: { r: 0.612, g: 0.639, b: 0.686 } }];
navItem.appendChild(navLabel);

return { id: navItem.id, name: navItem.name };
```

---

## 4. Verification Strategy

Verification is critical but expensive. Every screenshot and file read is a round-trip. Follow these rules to verify without wasting calls:

### Screenshot Discipline

- Screenshot ONCE after completing a logical section (header done, content area done, full screen done) — NOT after every single element.
- Group related construction steps together, then verify the group with one screenshot.
- If building a full screen: screenshot after header, screenshot after content area is populated, screenshot after footer. That is 3 screenshots max, not 15.

### File Structure Reads

- Read file structure ONCE at the start of a build session using `figma_get_file_data`. Store node IDs in your working memory.
- Do NOT re-call `figma_get_file_data` unless you have added major new structure (a new screen, a new section) and need the new node IDs.
- If you only need one node's ID, use `figma_get_selection` or reference the ID returned from your creation call.

### Batch Verification

If you need to verify alignment, spacing, or properties on multiple elements, batch the checks into one `figma_execute` call instead of taking multiple screenshots:

```javascript
// BATCH: Verify multiple elements in ONE call
const ids = ["NODE_ID_1", "NODE_ID_2", "NODE_ID_3"];
const results = ids.map(id => {
  const n = figma.getNodeById(id);
  return {
    name: n.name,
    width: Math.round(n.width),
    height: Math.round(n.height),
    layoutMode: n.layoutMode,
    sizingH: n.layoutSizingHorizontal,
    sizingV: n.layoutSizingVertical,
    padding: { L: n.paddingLeft, R: n.paddingRight, T: n.paddingTop, B: n.paddingBottom },
    gap: n.itemSpacing
  };
});
return results;
```

---

## 5. When to Load the Component Architect

If you are about to create the same visual structure more than once (e.g., 3 list items, 5 nav tabs, multiple cards), **STOP**. Tell the user:

> "I notice this pattern repeats — let me create a proper component with variants and properties first."

Then load `04b-component-architect.md`.

**Signals that you need the Component Architect:**
- You are copy-pasting a batch template from Section 3 more than twice
- The user's screen has 3+ visually identical items (list rows, grid cards, tab items)
- You need state variations (default, hover, active, disabled) for any element
- The same element appears across multiple screens in the build

Building instances of a component is always better than building N independent frames. Components stay in sync, reduce file size, and make future edits trivial.
