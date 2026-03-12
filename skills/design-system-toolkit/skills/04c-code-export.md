---
name: code-export
description: >
  Convert Figma designs to production frontend code (React, Vue, HTML/CSS, Tailwind).
  Use when the user says "export to code", "convert to React", "generate code from Figma",
  "Figma to CSS", "Figma to Tailwind", "code this design", "implement this screen".

  IMPORTANT: Read project/APP_PLAN.md for project context first. Use Figma:get_design_context
  to extract design data before generating code.
---

# Design-to-Code Export

> **Purpose**: Convert Figma designs into clean, production-grade frontend code.
> Supports React, Vue, HTML/CSS, and Tailwind. Reads Figma via MCP tools and
> generates code that matches the design pixel-for-pixel.
>
> **Workflow**: Extract design data → Map structure → Generate code → Verify match

---

## Export Quick Start

```
1. Get design data:  Figma:get_design_context (nodeId, fileKey)
2. Get tokens:       figma_get_variables (if needed)
3. Map:              Walk node tree → flex containers, text elements, component imports
4. Generate:         Choose framework from user preference or APP_PLAN.md
5. Verify:           Compare output to Figma screenshot (ONE screenshot, not per-element)
```

---

## Translation Principles

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

## Screen-to-Code Workflow

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

## Component-to-Code Workflow

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

## Token Export

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

## Code Quality Standards

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

## Framework-Specific Patterns

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

## Responsive Code from Figma

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
