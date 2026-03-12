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

---

## Step 3: Initialize Design System in Figma

**⚠️ MANDATORY: Do this BEFORE designing any screens.** Every text layer and color fill must reference a style — never raw values. This step creates the foundational text styles and color styles in Figma so that all subsequent design work is clean from the start.

### When to Run This Step

| Situation | Action |
|-----------|--------|
| New project, empty Figma file | Run full initialization (text styles + color styles) |
| Existing project, no text/color styles | Run full initialization |
| Existing project, partial styles | Audit what exists, fill gaps |
| Existing project, complete styles | Skip — proceed to Step 4 |

### Pre-Check: What's Already in Figma?

Before creating anything, check the current state of the Figma file:

1. **Check for existing text styles**: Open the Figma file → look at local styles (Text Styles panel). Are there named text styles?
2. **Check for existing color styles**: Look at local styles (Color Styles panel) or Variable collections. Are there named color styles/variables?
3. **Check for raw values**: Select text layers — do they reference a text style or are they raw font/size settings? Select colored elements — do fills reference a style/variable or are they hardcoded hex?

If the file has raw values everywhere, run the full initialization below.

### 3A: Generate the Type Scale

Using the project context from `project/APP_PLAN.md` (brand fonts, base text size, platform, density), generate a complete type scale and create it as **Figma Text Styles**.

**Type Scale Generation Rules:**

1. Start from the project's `base_text_size` (default: 16px)
2. Use the project's `brand_fonts.primary` for all styles (or secondary where specified)
3. Apply platform conventions:
   - **Mobile**: Body min 16px, heading sizes slightly smaller than desktop
   - **Web**: Can go larger on headings, same body minimum
4. Compute line heights: body = 1.5× font size, headings = 1.2–1.3× font size
5. Round all values to whole pixels

**Required Text Styles (minimum set):**

Create ALL of these as named Figma Text Styles:

```
Typography / Display          → [Font], 48px, Bold, line-height: 56px, letter-spacing: -0.5px
Typography / H1               → [Font], 32px, Bold, line-height: 40px, letter-spacing: -0.25px
Typography / H2               → [Font], 24px, Semibold, line-height: 32px
Typography / H3               → [Font], 20px, Semibold, line-height: 28px
Typography / H4               → [Font], 18px, Medium, line-height: 24px
Typography / Body Large        → [Font], 18px, Regular, line-height: 28px
Typography / Body              → [Font], 16px, Regular, line-height: 24px
Typography / Body Bold         → [Font], 16px, Semibold, line-height: 24px
Typography / Body Small        → [Font], 14px, Regular, line-height: 20px
Typography / Body Small Bold   → [Font], 14px, Semibold, line-height: 20px
Typography / Caption           → [Font], 12px, Regular, line-height: 16px
Typography / Caption Bold      → [Font], 12px, Medium, line-height: 16px
Typography / Overline          → [Font], 11px, Medium, line-height: 16px, letter-spacing: 0.5px, UPPERCASE
Typography / Button Large      → [Font], 16px, Semibold, line-height: 24px
Typography / Button            → [Font], 14px, Semibold, line-height: 20px
Typography / Button Small      → [Font], 12px, Medium, line-height: 16px
Typography / Label             → [Font], 14px, Medium, line-height: 20px
Typography / Helper            → [Font], 12px, Regular, line-height: 16px
```

**Adjustments based on project context:**

| Context | Adjustment |
|---------|-----------|
| `base_text_size: 14px` | Shift entire scale down by 2px (Body = 14px, Body Small = 12px, etc.) |
| `base_text_size: 18px` | Shift entire scale up by 2px |
| `density: Compact` | Tighten line heights by ~2px per level |
| `density: Spacious` | Loosen line heights by ~2px per level |
| `brand_fonts.secondary` exists | Apply secondary font to Body/Caption/Helper; keep primary for headings |
| `platform: Mobile` only | Skip Display size or reduce to 36px |

**How to create in Figma (using Figma MCP tools):**

Use `figma_execute` with the following script to create all 18 styles programmatically:

```javascript
// Via figma_execute — create all text styles
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

// ⚠️ Replace "Inter" with the project's actual brand font from APP_PLAN.md
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

After creation, verify all 18 styles appear in Figma's local styles panel.

### 3B: Generate the Color System

Using the project context from `project/APP_PLAN.md` (primary color, secondary color, visual tone, dark mode setting), generate a complete color system and create it as **Figma Color Styles** or **Figma Color Variables**.

**Color System Generation Rules:**

1. Start from the project's `branding.primary_color` and `branding.secondary_color`
2. Generate tints and shades: 50, 100, 200, 300, 400, 500 (base), 600, 700, 800, 900, 950
3. Generate neutral grays from the project's visual tone (warm → warm grays, cool → cool grays)
4. Generate semantic colors: success (green), warning (amber), error (red), info (blue)
5. Map all colors to semantic tokens for Light mode
6. If `dark_mode: Yes`, generate the Dark mode mapping

**Required Color Styles/Variables (minimum set):**

Create ALL of these as named Figma Color Styles or Color Variables:

```
PRIMITIVE PALETTE (raw colors — designers reference these rarely, tokens reference them):

  Primary / 50              → Lightest tint of primary
  Primary / 100
  Primary / 200
  Primary / 300
  Primary / 400
  Primary / 500             → Base primary color (from project context)
  Primary / 600
  Primary / 700
  Primary / 800
  Primary / 900
  Primary / 950             → Darkest shade of primary

  Secondary / 50–950        → Same scale for secondary color

  Neutral / 0               → #FFFFFF (pure white)
  Neutral / 50              → Lightest gray
  Neutral / 100
  Neutral / 200
  Neutral / 300
  Neutral / 400
  Neutral / 500
  Neutral / 600
  Neutral / 700
  Neutral / 800
  Neutral / 900
  Neutral / 950             → Near black
  Neutral / 1000            → #000000 (pure black)

  Success / 50–900          → Green scale
  Warning / 50–900          → Amber scale
  Error / 50–900            → Red scale
  Info / 50–900             → Blue scale (skip if primary is already blue)

SEMANTIC TOKENS (what you actually apply to designs):

  Light Mode:
    Background / Primary     → Neutral/0 (#FFFFFF)
    Background / Secondary   → Neutral/50
    Background / Tertiary    → Neutral/100
    Background / Inverse     → Neutral/900

    Text / Primary           → Neutral/900
    Text / Secondary         → Neutral/600
    Text / Tertiary          → Neutral/400
    Text / Inverse           → Neutral/0
    Text / Link              → Primary/600
    Text / Error             → Error/600
    Text / Success           → Success/600

    Border / Default         → Neutral/200
    Border / Strong          → Neutral/300
    Border / Focus           → Primary/500

    Surface / Card           → Neutral/0
    Surface / Elevated       → Neutral/0 (with shadow)
    Surface / Overlay        → Neutral/900 at 50% opacity
    Surface / Disabled       → Neutral/100

    Action / Primary         → Primary/500
    Action / Primary Hover   → Primary/600
    Action / Primary Active  → Primary/700
    Action / Secondary       → Neutral/100
    Action / Secondary Hover → Neutral/200
    Action / Destructive     → Error/500
    Action / Destructive Hover → Error/600
    Action / Disabled        → Neutral/300

    Feedback / Success BG    → Success/50
    Feedback / Success Text  → Success/700
    Feedback / Warning BG    → Warning/50
    Feedback / Warning Text  → Warning/700
    Feedback / Error BG      → Error/50
    Feedback / Error Text    → Error/700
    Feedback / Info BG       → Info/50
    Feedback / Info Text     → Info/700

  Dark Mode (if applicable):
    Background / Primary     → Neutral/950
    Background / Secondary   → Neutral/900
    Background / Tertiary    → Neutral/800
    Background / Inverse     → Neutral/50

    Text / Primary           → Neutral/50
    Text / Secondary         → Neutral/400
    Text / Tertiary          → Neutral/500
    Text / Inverse           → Neutral/900
    Text / Link              → Primary/300
    Text / Error             → Error/400
    Text / Success           → Success/400

    Border / Default         → Neutral/700
    Border / Strong          → Neutral/600
    Border / Focus           → Primary/400

    Surface / Card           → Neutral/900
    Surface / Elevated       → Neutral/800
    Surface / Overlay        → Neutral/950 at 60% opacity
    Surface / Disabled       → Neutral/800

    Action / Primary         → Primary/400
    Action / Primary Hover   → Primary/300
    Action / Primary Active  → Primary/200
    (... same pattern, lighter values for dark mode)

    Feedback / Success BG    → Success/950
    Feedback / Success Text  → Success/300
    (... same pattern)
```

**How to implement in Figma:**

Option A — **Figma Variables** (preferred, supports mode switching):

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

⚠️ **Replace all hex values above with the project's actual colors from APP_PLAN.md.** The values shown are Tailwind-based defaults.

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

This allows instant Light <> Dark theme switching in Figma by changing the variable mode on any frame.

Option B — **Figma Color Styles** (simpler, no mode switching):

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

**⚠️ Always replace the default hex values in the scripts with the actual colors from `project/APP_PLAN.md`.** The defaults are Tailwind-based starting points.

**After creation**, run this verification script via `figma_execute`:

```javascript
// Verify all expected color styles exist
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

### 3C: Verify & Record

After creating styles in Figma:

1. **Verify text styles**: Confirm all 18 text styles appear in the local styles panel
2. **Verify color styles**: Confirm semantic tokens are available and correctly mapped
3. **Verify border radius tokens**: Confirm FLOAT variables exist: radius/none (0), radius/xs (2), radius/sm (4), radius/md (8), radius/lg (12), radius/xl (16), radius/2xl (24), radius/full (9999). See 01-guide §21.
4. **Verify icon set**: Confirm icon components are imported (minimum: search, close, back, check, error, info, warning, user, star, home, settings, notification, location). All icons must be 24×24 square frames with vector children. See 01-guide §20.
5. **Update APP_PLAN.md**: Record the design system initialization in the Design System Inventory section:

```yaml
# Add to project/APP_PLAN.md → Design System Inventory
Token Collections:
  - Typography: 18 text styles (Typography / [Name])
  - Primitives: [N] color variables (Primary, Secondary, Neutral, Success, Warning, Error, Info)
  - Semantic: [N] color variables with Light + Dark modes
  - Border Radius: 8 FLOAT variables (radius/none through radius/full)
  - Icon Set: [Library Name] — [N] icons imported as components (Icon / [Name])
  - Icon Sizes: 12/16/20/24/32/40/48 (24px default)
```

6. **Add to Decision Log**: Record the type scale, color, radius, and icon library decisions

### 3D: Gate Check — No Designing Without Styles

**🚫 DO NOT proceed to Step 4 (designing screens) until:**

- [ ] All text styles exist in Figma
- [ ] All semantic color tokens exist in Figma
- [ ] Border radius FLOAT variables exist in Figma
- [ ] Icon set imported as components (no text-character icons)
- [ ] `icon_page_name` is set in APP_PLAN.md — if NOT set, ASK: "Which Figma page contains your project's icons?" and store the answer before proceeding
- [ ] APP_PLAN.md inventory is updated

**When designing screens after this step:**
- Every text layer MUST reference a Text Style — never set font/size manually
- Every color fill MUST reference a Color Style or Variable — never hardcode hex values
- Every border radius MUST reference a radius variable — never use raw numbers
- Every icon MUST be a component instance from the icon library — never use text characters (◉, ★, →)
- Icon frames MUST be square (24×24 default) — never non-square dimensions
- Every interactive element (button, input, card, chip, list item) MUST have non-zero padding — zero-padding is a 🔴 Critical bug
- All padding and gap values MUST be on the spacing token scale (2/4/8/12/16/20/24/32/48/64) — off-grid values are bugs
- Buttons, cards, text containers MUST use HUG height — fixed height on content elements is a 🔴 Critical bug
- Content areas (between header and footer) MUST use FILL height — HUG on content area causes footer to float
- Screen content frames MUST have ≥16px left and right padding — zero-margin screens are a 🔴 Critical bug
- If you need a style that doesn't exist, CREATE the style first, then apply it

---

## Design System Governance

As a design system grows, it needs rules for how it evolves — who decides what, how changes are made, and how to keep the system healthy over time.

### Ownership Model

| Role | Responsibility | Who |
|------|---------------|-----|
| **System Owner** | Final decisions on additions/removals, maintains quality bar | Lead designer or design system team |
| **Contributors** | Propose new components, report bugs, submit variants | Any designer or developer on the team |
| **Consumers** | Use the system as-is, request changes through proper channels | Product designers, developers |
| **Reviewers** | Review proposals for consistency, accessibility, feasibility | System owner + senior designer + lead dev |

### Contribution Model

External teams (product designers, developers) contribute to the design system through this process:

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  IDENTIFY    │ →  │  PROPOSE     │ →  │  REVIEW      │ →  │  INTEGRATE   │
│              │    │              │    │              │    │              │
│ Contributor  │    │ Submit       │    │ Reviewers    │    │ System Owner │
│ finds a gap  │    │ proposal     │    │ evaluate     │    │ merges into  │
│ or pattern   │    │ (template    │    │ (2-week      │    │ library      │
│ used 2+      │    │ below)       │    │ review       │    │ + publishes  │
│ times        │    │              │    │ cycle)       │    │              │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
```

**Review Cadence:**
- New proposals reviewed bi-weekly (or weekly during active development)
- Reviewers: System Owner + 1 senior designer + 1 lead developer (minimum)
- Decision within 2 review cycles (max 4 weeks)
- Contributor notified of decision with actionable feedback

**What Qualifies for Contribution:**
- Pattern used in 2+ products or screens
- Addresses a gap not covered by existing components
- Includes all required states (default, hover, active, focus, disabled, loading, error)
- Follows existing token architecture
- Meets accessibility requirements

### Component Lifecycle

Every component goes through these stages:

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│ PROPOSED  │ →  │ DRAFT    │ →  │ BETA     │ →  │ STABLE   │ →  │ DEPRECATED│
│           │    │          │    │          │    │          │    │           │
│ Idea or   │    │ Designed │    │ In use   │    │ Fully    │    │ Scheduled │
│ request   │    │ not yet  │    │ by 1-2   │    │ adopted, │    │ for       │
│ submitted │    │ in prod  │    │ teams    │    │ tested   │    │ removal   │
└──────────┘    └──────────┘    └──────────┘    └──────────┘    └──────────┘
```

**Stage Transitions:**
- Proposed → Draft: Approved by reviewers, designer assigned
- Draft → Beta: Design complete, all states defined, tokens used, accessibility verified
- Beta → Stable: Used by 2+ teams for 30+ days with no critical issues
- Stable → Deprecated: Replacement available, deprecation notice published

### Proposing a New Component

Before creating a new component, answer these:

| Question | Required Answer |
|----------|----------------|
| Does a similar component already exist? | No (or existing one doesn't cover the use case) |
| Will 2+ teams/screens use this? | Yes — don't systemize one-off patterns |
| Is the pattern stable? | Yes — it's been used successfully in at least one product |
| Are all states designed? | Default, hover, active, focus, disabled, loading, error |
| Is it accessible? | Meets WCAG AA, keyboard navigable, screen reader tested |
| Is it responsive? | Works across target breakpoints |
| Are tokens used? | All values reference design tokens (no hardcoded hex/px) |

### Proposal Template

```
## Component Proposal: [Name]

**Proposer**: [Name, Role]
**Date**: [Date]
**Status**: Proposed

### Problem
What user or team need does this solve?

### Use Cases
Where will this be used? (List 2+ screens/products)

### Existing Alternatives
Why can't an existing component cover this?

### Design Spec
[Link to Figma frame with all states]

### Accessibility
- Keyboard behavior: [describe]
- Screen reader: [describe ARIA approach]
- Contrast: [verified AA/AAA]

### Token Usage
List all tokens this component references.

### Decision
- [ ] Approved → Move to Draft
- [ ] Needs revision → [feedback]
- [ ] Rejected → [reason]
```

### Deprecating a Component

| Step | Action |
|------|--------|
| 1 | Mark as deprecated in Figma (add ⚠️ prefix, move to "Deprecated" page) |
| 2 | Announce to team with migration path ("Use [New Component] instead") |
| 3 | Add deprecation notice to component description in Figma |
| 4 | Set removal date (minimum 30 days from announcement) |
| 5 | After removal date: delete from library, clean up instances |

### Audit Schedule

| Frequency | What to Review |
|-----------|---------------|
| **Weekly** | New component requests, bug reports |
| **Monthly** | Token consistency, unused components, detached instances |
| **Quarterly** | Full visual audit (03-ui-visual-audit.md), accessibility audit, performance review |
| **Annually** | Design trend review, competitive benchmark, major version consideration |

---

## Design System Versioning & Changelog

When the design system changes, everyone needs to know what changed, why, and what to do about it.

### Versioning Scheme

Use **Semantic Versioning** adapted for design:

```
MAJOR.MINOR.PATCH

MAJOR (1.0 → 2.0): Breaking changes
  - Component removed or renamed
  - Token values changed significantly
  - Layout structure changed (affects all consumers)

MINOR (1.0 → 1.1): New features, backward-compatible
  - New component added
  - New variant added to existing component
  - New token added

PATCH (1.0.0 → 1.0.1): Fixes, no behavior change
  - Color value adjusted slightly
  - Spacing fix
  - Accessibility fix
  - Bug fix in component
```

### Changelog Template

Maintain a changelog page in your Figma library or documentation site:

```
# Design System Changelog

## v1.3.0 — 2026-03-01

### ✨ Added
- Button: New "ghost" variant for secondary actions
- Card: Added "compact" size variant
- New component: Tag/Chip (with removable option)

### 🔄 Changed
- color/primary: Updated from #2563EB to #2557E8 (slightly warmer)
- spacing/lg: Changed from 20px to 24px (aligns to 8px grid)

### 🐛 Fixed
- Input: Focus ring was 1px, now 2px (accessibility fix)
- Avatar: Image didn't cover full circle on Safari

### ⚠️ Deprecated
- OldButton: Use Button instead. Removal in v2.0.
- color/accent is now color/secondary. Old token will be removed in v2.0.

### 💥 Breaking (major versions only)
- Removed: LegacyCard component (replaced by Card)
- Renamed: all `bg-*` tokens to `surface-*`
```

### Communicating Changes

| Change Type | Communication Method |
|-------------|---------------------|
| Patch (fix) | Figma library update notification (automatic) |
| Minor (new feature) | Slack/Teams message + changelog entry |
| Major (breaking) | Team meeting or email + migration guide + 30-day notice |
| Deprecation | In-component warning + announcement + deadline |

### Migration Guide Template (for breaking changes)

```
## Migration Guide: v1.x → v2.0

### What Changed
[Brief description of breaking changes]

### Step-by-Step Migration

1. **Update Token References**
   - Old: `color/accent` → New: `color/secondary`
   - Old: `bg-card` → New: `surface/card`

2. **Replace Deprecated Components**
   - Old: `<LegacyCard>` → New: `<Card variant="default">`
   - Old: `<OldButton>` → New: `<Button variant="primary">`

3. **Test**
   - Run visual regression tests
   - Check all breakpoints
   - Verify dark mode
   - Run accessibility audit
```
