---
name: modern-ui-ux-design
description: >
  Design-focused guide for creating modern, user-friendly UI/UX in Figma for web and mobile applications.
  This is NOT a coding skill — it is a UX design thinking and UI pattern skill for designers. Use this skill
  whenever designing screens, user flows, wireframes, prototypes, or design systems in Figma. Trigger when the
  user mentions "user flow", "screen design", "mobile design", "UX review", "usability", "design feedback",
  "Figma", "wireframe", "prototype", "navigation pattern", "layout", "responsive", "mobile-friendly",
  "design system", "component design", or asks to review, critique, or improve any interface design.
  Ensures every design decision prioritizes real user needs, modern patterns, and cross-device usability.
---

# Modern UI/UX Design Guide for Figma

> **Philosophy**: Design for people first. Every screen should answer three questions:
> Where am I? What can I do here? What should I do next?

---

## Table of Contents

1. [UX Thinking Framework](#1-ux-thinking-framework)
2. [User Flow Design](#2-user-flow-design)
    - [Annotations & Comments System](#2b-annotations--comments-system)
3. [Mobile-First Design Strategy](#3-mobile-first-design-strategy)
4. [Screen Layout Principles](#4-screen-layout-principles)
5. [Navigation Patterns](#5-navigation-patterns)
6. [Touch & Interaction Design](#6-touch--interaction-design)
7. [Typography & Readability](#7-typography--readability)
8. [Color, Contrast & Theming](#8-color-contrast--theming)
9. [Forms & Data Input](#9-forms--data-input)
10. [Feedback, States & Microinteractions](#10-feedback-states--microinteractions)
11. [Empty, Error & Edge States](#11-empty-error--edge-states)
12. [Accessibility in Design](#12-accessibility-in-design)
13. [Platform Conventions (iOS / Android / Web)](#13-platform-conventions)
14. [Design Review Checklist](#14-design-review-checklist)
15. [Figma Workflow Best Practices](#15-figma-workflow-best-practices)
    - [Full Auto Layout Architecture](#full-auto-layout-architecture)
16. [Design-to-Dev Handoff](#16-design-to-dev-handoff)
17. [Content & UX Writing](#17-content--ux-writing)
18. [Design Performance Budget](#18-design-performance-budget)
19. [Common UX Anti-Patterns](#19-common-ux-anti-patterns)
20. [Iconography System](#20-iconography-system)
21. [Placeholder Images — Unsplash in Figma](#21-placeholder-images--unsplash-in-figma)
22. [Icon Source — The Project Icons Page](#22-icon-source--the-project-icons-page)
23. [Border Radius System](#23-border-radius-system)
24. [Data Flow Design](#24-data-flow-design)
25. [Spacing & Sizing Enforcement](#25-spacing--sizing-enforcement)
26. [Modern Design Patterns (2024-2026)](#26-modern-design-patterns-2024-2026)

---

## 1. UX Thinking Framework

Before designing any screen, work through these questions:

### The 5-Second Test
If someone saw this screen for 5 seconds and it disappeared, could they tell you:
- What the screen is for?
- What the main action is?
- Where they are in the app?

If the answer to any of these is "no," the screen needs simplification.

### Key UX Laws to Internalize

**Hick's Law — Fewer choices = faster decisions**
- Limit visible options per screen to 5–7 actions maximum
- When there are many options, group them into categories
- Highlight the ONE primary action the user should take
- Use progressive disclosure — reveal complexity only when needed

**Fitts's Law — Important things should be big and close**
- Primary actions should be the largest interactive element on screen
- On mobile, place primary actions in the bottom third (thumb zone)
- On desktop, place them where the user's cursor naturally rests
- Never make critical buttons small or tucked into corners

**Miller's Law — People hold ~7 items in working memory**
- Chunk information into digestible groups (3–5 items per group)
- Use section headings, dividers, and whitespace to create visual groups
- Long lists need search, filters, or categorization

**Jakob's Law — Users spend most time on OTHER apps**
- People expect your app to work like apps they already know
- Use familiar patterns. Innovation in navigation = confusion for users
- Standard patterns: bottom tab bar, pull-to-refresh, swipe-to-delete, etc.

**Aesthetic-Usability Effect — Pretty things feel easier to use**
- Users forgive minor usability issues if the design is visually appealing
- But a beautiful app that's confusing to use still fails
- Invest in visual polish after the UX foundation is solid

### Progressive Disclosure
Don't show everything at once. Layer information so users get:
1. **Level 1**: The essential — what they need right now
2. **Level 2**: The useful — available on demand (tap to expand, scroll to see)
3. **Level 3**: The advanced — tucked in settings or secondary screens

Example: A product listing shows name + price + image (Level 1). Tapping reveals full description, specs, reviews (Level 2). "Report item" or "Share" are in a menu (Level 3).

---

## 2. User Flow Design

### Flow Design Principles

**Every flow should have:**
- A clear entry point (how does the user get here?)
- A clear goal (what are they trying to accomplish?)
- A clear exit / completion state (how do they know they're done?)
- Escape routes at every step (back button, cancel, close)

**Reduce steps ruthlessly:**
- Every additional step in a flow loses 10–20% of users
- Combine screens when possible without overloading any single screen
- Pre-fill data you already have (don't make users re-enter information)
- Offer smart defaults (most common choice pre-selected)

### Flow Mapping Approach

When designing flows in Figma:

1. **Start with a flow diagram** (FigJam or simple boxes/arrows) before any UI
2. **Map the happy path first** — the ideal journey with no errors
3. **Then map edge cases**: What if the user has no data? What if the API fails? What if they go back mid-flow?
4. **Identify decision points** — where does the flow branch?
5. **Count the steps** — if more than 3–5 for a common task, look for shortcuts

### Flow Vocabulary — Standard Symbols

Use consistent symbols across all flow diagrams so every team member reads them the same way:

| Symbol | Shape | Meaning | Example |
|---|---|---|---|
| **Screen / State** | Rectangle (rounded) | A UI screen or distinct view state | "Home", "Login / Error" |
| **Decision** | Diamond | A conditional branch in the flow | "Is user logged in?" |
| **Action / Trigger** | Arrow label | What the user does to move forward | "Tap Submit", "Swipe left" |
| **Entry Point** | Filled circle | Where the user enters the flow | App launch, deep link, push notification |
| **End / Success** | Double circle | Flow completion | "Order confirmed", "Account created" |
| **Error / Dead End** | Red octagon or red rectangle | Something went wrong | API failure, validation error, denied permission |
| **Sub-Flow** | Rectangle with side bars | References another flow (don't inline it) | "See: Payment Flow" |
| **System Action** | Dashed rectangle | Something the system does (not the user) | "Send verification email", "Process payment" |
| **Delay / Wait** | Hourglass or clock icon | Time passes | "Wait for SMS", "Loading 2-3s" |

### Flow Path Color Coding

Color-code every connector arrow by path type. This is non-negotiable for complex flows:

| Color | Path Type | Use |
|---|---|---|
| 🟢 **Green** | Happy path | The ideal journey — no errors, no edge cases. Design this first. |
| 🔴 **Red** | Error path | What happens when something fails: validation, API, permissions. |
| 🟡 **Yellow / Amber** | Warning / Confirmation | Destructive actions, unsaved changes, "are you sure?" |
| ⚪ **Gray** | Edge case / Alternate | Paths that work but aren't the primary journey (back, skip, optional steps). |
| 🔵 **Blue** | System / Background | Things the system does without user action (notifications, data sync, timers). |

### Screen-to-Screen Transitions

Document the transition type on every connector so developers know the animation:

| Transition | When to Use | Figma Prototype Setting |
|---|---|---|
| **Push right (→)** | Moving deeper: list → detail, home → settings | Smart Animate or Slide In from Right |
| **Push left (←) / Back** | Returning to previous level | Slide In from Left |
| **Modal / Bottom sheet (↑)** | Temporary focused task: confirm, select, filter, edit | Slide In from Bottom, overlay |
| **Dismiss (↓)** | Closing a modal or bottom sheet | Slide Out to Bottom |
| **Fade / Replace** | Switching between peer-level tabs | Dissolve, 200ms |
| **Expand / Collapse** | Revealing more detail in-place (accordion, expandable card) | Smart Animate |
| **Full-screen replace** | Authentication redirects, onboarding completion | Dissolve or Instant |

Label format on flow arrows: `"Tap [element]" → [Transition type]`
Example: `"Tap 'Place Order'" → Push right` or `"Swipe down" → Dismiss`

### Flow Organization in Figma

#### Page Structure

Dedicate a separate Figma page for user flows, distinct from screen designs:

```
📄 Cover
📄 User Flows          ← Flow diagrams live here
📄 Screens             ← Actual designed screens
📄 Components
📄 Design Tokens
📄 Archive
```

#### Flow Page Layout

Organize the flow page with a clear reading direction:

```
Flow Page Layout (left to right, top to bottom):

┌─────────────────────────────────────────────────────────────┐
│ SECTION: [Feature Name] Flow                                 │
│ Status: 🟢 Ready / 🟡 In Progress / 🔴 Needs Review         │
│ Last updated: [date] | Owner: [name]                         │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ● Entry ──→ [ Screen 1 ] ──→ [ Screen 2 ] ──→ ◉ Success   │
│                    │                                          │
│                    ▼ (error)                                  │
│              [ Error State ]                                  │
│                                                               │
│  Legend:                                                      │
│  🟢 Happy path  🔴 Error  🟡 Warning  ⚪ Edge case  🔵 System│
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

**Rules:**
1. One flow per Figma **Section** (the purple grouping container) — never mix multiple flows in one section
2. Flow reads **left to right** for the happy path, branches go **downward**
3. Every section has a **title frame** at top-left with: flow name, status emoji, last-updated date, owner name
4. Include a **legend** in each section explaining the color codes
5. Screens in the flow are either: (a) full-size design frames, or (b) scaled-down thumbnails with a link to the full screen on the Screens page
6. Maximum **one level of branching** visible per diagram. If a branch is complex, extract it as a sub-flow section

#### Flow Numbering Convention

Number every screen in the flow for easy reference in conversations, comments, and tickets:

```
[Flow Prefix]-[Step Number]-[State]

Examples:
  AUTH-01-Default       → Login screen, default state
  AUTH-02-Loading       → Login screen, loading state
  AUTH-02-Error         → Login screen, error state
  AUTH-03-Success       → Dashboard after login

  CHECKOUT-01-Cart      → Cart review
  CHECKOUT-02-Shipping  → Shipping form
  CHECKOUT-03-Payment   → Payment entry
  CHECKOUT-04-Confirm   → Order confirmation

  DISPUTE-01-List       → Dispute list
  DISPUTE-02-Detail     → Dispute detail
  DISPUTE-03-Respond    → Response form
```

Use this numbering in:
- Frame names in Figma
- Jira/Linear ticket references
- Figma comments
- Slack discussions
- Dev handoff documents

#### Connector Arrow Standards

When drawing flow connectors between frames:

```
Arrow Specs:
  Stroke weight:     2px (1.5px for dense flows)
  Stroke color:      Follow path color coding (green/red/yellow/gray/blue)
  Arrowhead:         Filled triangle on the destination end
  Start:             No arrowhead (or small dot for entry points)
  Label position:    Centered on arrow, offset above/below the line
  Label font:        Caption size (12px), same color as arrow
  Label content:     "Tap [Element]", "Swipe [Direction]", "System: [Action]"
  Corner routing:    90° bends (not diagonal), with 8px corner radius on bends
  Dashed arrows:     Use for optional/conditional paths
  Spacing:           Minimum 40px gap between arrow and nearest frame edge
```

**Tools for drawing connectors:**
- **FigJam arrows** — Draw in FigJam, copy-paste into Figma Design file. They remain editable and auto-route around objects.
- **Autoflow plugin** — Select two frames, plugin draws a smart connector between them. Free for up to 50 flows per file.
- **Manual lines** — Use Figma's line tool (L) with arrowhead property. Less flexible but fully native.

### Flow Completeness Checklist

Before a flow is considered complete, verify every item:

| # | Check | ✓ |
|---|-------|---|
| 1 | Happy path is fully connected from entry to success | |
| 2 | Every error state is shown (validation, API, permission, network) | |
| 3 | Every decision diamond has both YES and NO paths | |
| 4 | Back/cancel behavior is documented at every step | |
| 5 | Loading states exist between every async action | |
| 6 | Empty states exist for any screen with dynamic content | |
| 7 | All connectors are labeled with trigger action and transition type | |
| 8 | Color coding is applied consistently (green/red/yellow/gray/blue) | |
| 9 | Screens are numbered with flow prefix convention | |
| 10 | Flow section has title, status, date, owner, and legend | |
| 11 | No orphan screens (every frame has at least one incoming or outgoing connection) | |
| 12 | Sub-flows are extracted if any branch exceeds 3 screens | |

### Flow Anti-Patterns

- ❌ **Spaghetti flows** — arrows crossing everywhere with no visual hierarchy. Extract sub-flows.
- ❌ **Happy path only** — no error states, loading states, or empty states. The happy path is 30% of the work.
- ❌ **Unlabeled arrows** — connectors with no trigger action. Every arrow must say WHAT the user did.
- ❌ **No color coding** — all arrows are black/gray. Impossible to distinguish happy from error paths.
- ❌ **Mixing flows and screens** — flow diagrams and final screen designs on the same page. Keep them separate.
- ❌ **Giant monolith flow** — one diagram with 30+ screens. Break into sub-flows of 5–8 screens each.
- ❌ **Dead-end error states** — error screens with no recovery path. Every error must show how to get back.
- ❌ **No numbering** — screen names like "Frame 47" or "Untitled". Use the flow numbering convention.

---

## 2B. Annotations & Comments System

Annotations and comments are how design intent survives the journey from designer to developer. Without them, developers guess — and they guess wrong. This section defines the annotation system.

### The Three Layers of Design Communication

| Layer | What It Is | Where It Lives | Who Reads It |
|---|---|---|---|
| **1. Figma Native Annotations** | Property callouts, measurements, free-text notes attached to specific layers | Figma's annotation tool (Shift+T), visible in Dev Mode | Developers inspecting in Dev Mode |
| **2. Figma Comments** | Threaded discussions pinned to canvas locations | Figma's comment mode (C key), notification-driven | Team discussions: feedback, questions, decisions |
| **3. Canvas Annotations** | Styled sticky notes, callout components placed on the canvas | Custom annotation components on an overlay layer | Anyone viewing the design file |

Use ALL THREE — they serve different purposes and different audiences.

### Layer 1: Figma Native Annotations (Shift+T)

Native annotations are the primary handoff tool. They're visible in Dev Mode and update automatically when the design changes.

**When to annotate:**
- Any interaction that can't be shown in a static frame (gestures, animations, conditional logic)
- Responsive behavior that changes across breakpoints
- Accessibility requirements (ARIA roles, focus order, screen reader text)
- Content rules (character limits, truncation behavior, dynamic content sources)
- Edge cases (what happens at 0 items, 1 item, 100 items, max characters)

**Annotation Categories:**

Set up these categories in every file (Figma lets you create custom categories with colors):

| Category | Color | Use For |
|---|---|---|
| **Interaction** | 🔵 Blue | Gestures, tap behavior, hover effects, animation specs |
| **Responsive** | 🟣 Purple | Breakpoint changes, reflow behavior, min/max widths |
| **Accessibility** | 🟢 Green | ARIA roles, focus order, screen reader announcements, contrast notes |
| **Content** | 🟡 Yellow | Character limits, truncation rules, dynamic data sources, localization notes |
| **Logic** | 🔴 Red | Conditional rendering, if/else states, permission checks, feature flags |
| **Dev Note** | ⚪ Gray | Implementation hints, API endpoints, known tech debt, "ask [name] about this" |

**Annotation Format:**

Write annotations in a consistent, scannable format:

```
[CATEGORY] Brief title

Details:
- Specific behavior or requirement
- Edge case or exception
- Reference to another frame or flow

Example:
[INTERACTION] Swipe-to-delete

Details:
- Swipe left on list item reveals delete button (64px wide, red fill)
- Threshold: 80px horizontal swipe
- Animation: Content slides left, 200ms ease-out
- Tap delete → confirmation bottom sheet (see frame DISPUTE-05-Confirm)
- Tap outside or swipe right → dismiss, snap back 300ms ease-in-out
```

**What to annotate on every screen:**

| Element Type | Required Annotations |
|---|---|
| **Buttons (primary CTA)** | Tap behavior, loading state, disabled conditions, analytics event name |
| **Lists / Scrollable areas** | Pagination type (infinite scroll / load more / pages), empty state reference, pull-to-refresh behavior |
| **Forms** | Validation rules per field, keyboard type (email, phone, number), auto-focus behavior, submit trigger |
| **Modals / Bottom sheets** | Dismiss behavior (tap outside, swipe down, X button), entry animation, backdrop opacity |
| **Navigation** | Deep link support, back button behavior (pop vs. replace), tab persistence (does it reset scroll?) |
| **Images / Media** | Aspect ratio, fallback/placeholder, loading strategy (lazy/eager), max file size |
| **Dynamic content** | Data source (API endpoint / CMS field), refresh trigger, cache behavior, error fallback |

### Layer 2: Figma Comments (C Key)

Comments are for team discussions, not for dev handoff. They're ephemeral — they get resolved and disappear.

**Comment Conventions:**

| Type | Format | Example |
|---|---|---|
| **Feedback request** | `@[name] — [question]?` | `@Fernando — Should the cancel button be destructive red or neutral gray?` |
| **Decision record** | `DECISION: [what was decided] — [date]` | `DECISION: Using bottom sheet instead of full-screen modal for filters — Mar 1` |
| **Bug / Issue** | `BUG: [description]` | `BUG: Button text truncates at 20 chars on small screens` |
| **TODO** | `TODO: [action item] — [owner]` | `TODO: Add empty state for dispute list — Armando` |
| **Resolved note** | Resolve with summary | `Resolved: Went with neutral gray per PM feedback` |

**Comment Rules:**
1. **Always @mention** the person who needs to respond — don't leave unaddressed comments
2. **Resolve comments** when the action is complete — don't leave stale threads
3. **Pin comments to the specific element**, not to blank canvas space — so they move with the design
4. **One topic per comment thread** — don't bundle multiple questions in one comment
5. **Include frame references** when discussing a specific screen: "See AUTH-03-Success"
6. **Never use comments for specs** — specs go in native annotations (they survive in Dev Mode; comments don't)

### Layer 3: Canvas Annotation Components

For information that needs to be visually prominent on the canvas — not hidden behind Dev Mode or comment bubbles.

**Build these as reusable components in the Components page:**

```
Annotation / Status Badge
├── Status text ("🟢 Ready for Dev", "🟡 In Progress", "🔴 Needs Review", "⚪ Archived")
├── Fixed width: 160px, auto height
├── Style: pill shape, colored fill matching status, white text
└── Place on top-right corner of every screen frame

Annotation / Callout Note
├── Category label (Interaction, Responsive, A11y, Content, Logic, Dev Note)
├── Body text (the actual note)
├── Connector line to the target element
├── Style: 200px wide, auto height, 8px radius, light fill, 1px border
├── Category color bar on left edge (4px wide, full height)
└── Place adjacent to the element being annotated

Annotation / Flow Label
├── Flow prefix + step number (e.g., "AUTH-03")
├── Screen name
├── Style: top-left corner badge, small text, semi-transparent background
└── Place inside every screen frame at top-left

Annotation / Version Stamp
├── Version number, date, author
├── Style: bottom-right corner, muted text
└── Place inside the flow section title frame
```

**Canvas annotation placement rules:**
1. Annotations sit OUTSIDE the screen frame, connected by a thin line (1px, gray)
2. Never overlap the screen content — annotations must not obscure the design
3. Group all annotations for one screen together, to the right or below the frame
4. Use a consistent offset: 40px gap between the screen frame and the annotation column

### Status System

Every screen frame and flow section gets a status badge. This is how teams know what's ready.

| Status | Emoji | Meaning | Who Sets It |
|---|---|---|---|
| **Draft** | ⚪ | Work in progress, not ready for review | Designer |
| **Design Review** | 🟡 | Ready for design feedback from peers or stakeholders | Designer |
| **Needs Changes** | 🔴 | Reviewed, changes requested | Reviewer |
| **Ready for Dev** | 🟢 | Design is final, annotated, and approved | Designer (after review) |
| **In Development** | 🔵 | Developer is actively building this | Developer |
| **Dev Complete** | ✅ | Built, ready for design QA | Developer |
| **Archived** | 🗄️ | Deprecated or superseded by a newer version | Anyone |

**Rules:**
1. Use Figma's native **"Ready for dev"** status (Section → right-click → Mark ready for dev) in addition to the visual badge — this integrates with Dev Mode's filtering
2. Status badges are placed on the **flow section title** AND on **individual screen frames**
3. Never hand off a screen without 🟢 status — it signals to developers that annotations are complete
4. When updating a design after dev has started (🔵), add a Figma comment explaining what changed

### Measurement Annotations (Shift+M)

Figma's native measurement tool creates persistent spacing callouts visible in Dev Mode.

**When to add measurements:**
- Non-obvious spacing that isn't captured by auto layout gap values
- Margins between sections that a developer might misinterpret
- Fixed dimensions that matter (e.g., "this image must be exactly 343×200")
- Minimum tap target areas around icons or small elements

**Don't over-measure** — if the design uses auto layout with proper gap values, Dev Mode already shows the spacing. Only add measurements for things that auto layout doesn't make obvious.

### Comments via Figma MCP Tools

When working with Figma programmatically, use these MCP tools for comments and annotations:

```
Reading comments:
  Tool: figma_get_comments
  → Returns all comment threads with author, message, timestamps, pinned node locations
  → Use include_resolved: true to also see resolved threads

Posting comments:
  Tool: figma_post_comment
  → message: The comment text (supports basic formatting)
  → node_id: Pin to a specific element (e.g., "4:328")
  → reply_to_comment_id: Thread a reply to an existing comment

Deleting comments:
  Tool: figma_delete_comment
  → comment_id: Get ID from figma_get_comments

Setting descriptions (for Dev Mode visibility):
  Tool: figma_set_description
  → nodeId: The component or frame
  → description: Plain text shown in Dev Mode inspect panel
  → descriptionMarkdown: Rich text with markdown formatting
```

**Automation patterns:**
- After an audit, post comments on each flagged element explaining the issue
- After a design review, batch-post TODO comments on screens that need changes
- Use `figma_set_description` on components to embed documentation that developers see in Dev Mode without leaving the inspect panel

### Annotation Checklist — Before Marking "Ready for Dev"

| # | Check | ✓ |
|---|-------|---|
| 1 | Screen has a status badge (🟢 Ready for Dev) | |
| 2 | Screen is numbered with flow prefix convention (AUTH-03-Success) | |
| 3 | All interactive elements have [INTERACTION] annotations | |
| 4 | All conditional rendering has [LOGIC] annotations | |
| 5 | Responsive behavior annotated if this screen has breakpoint changes | |
| 6 | Accessibility notes added: focus order, ARIA roles, screen reader text | |
| 7 | Content rules documented: character limits, truncation, data source | |
| 8 | Edge cases annotated: empty, loading, error, max content, min content | |
| 9 | All Figma comments resolved or addressed (no stale threads) | |
| 10 | Measurements added for non-obvious spacing | |
| 11 | Component descriptions set for any custom/modified components | |
| 12 | Flow diagram on the User Flows page is updated and matches final screens | |

---

## 3. Mobile-First Design Strategy

**Always design the mobile version first.** It forces you to prioritize because you have less space. Then expand for tablet and desktop.

### Why Mobile-First Matters
- Over 60% of web traffic is mobile
- Mobile constraints force clarity — if it works on 375px wide, it works everywhere
- It's easier to add space and features going up than to cram things going down
- Touch-first thinking improves usability even on desktop

### Frame Sizes in Figma

**Mobile frames:**
- iPhone SE: 375 × 667 (minimum viable mobile)
- iPhone 14/15: 393 × 852 (most common current size)
- iPhone Pro Max: 430 × 932 (large phone)
- Android common: 360 × 800

**Tablet frames:**
- iPad Mini: 744 × 1133
- iPad Air/Pro: 820 × 1180

**Desktop frames:**
- Small laptop: 1280 × 800
- Standard desktop: 1440 × 900 (most common design frame)
- Large desktop: 1920 × 1080

### Mobile Design Rules

**Content priority:**
- Single column layout — no side-by-side content on mobile unless they're very short items (like two buttons)
- Most important content at the top
- Primary CTA visible without scrolling (above the fold)
- Secondary actions below or in menus

**The Thumb Zone:**
```
┌──────────────────────┐
│                      │
│   ⚠️ HARD TO REACH   │  Top 1/3: Status info, titles, 
│                      │  secondary actions, back button
│                      │
│   ✅ COMFORTABLE      │  Middle 1/3: Scrollable content,
│                      │  lists, cards, media
│                      │
│   🎯 EASY / NATURAL   │  Bottom 1/3: Primary actions,
│                      │  navigation tabs, FAB button
└──────────────────────┘
        👍 Thumb
```

Place your most-used actions in the bottom third of the screen where the thumb naturally rests. This is critical for one-handed use.

**Adapting for larger screens:**
- Tablet: Introduce side-by-side panels (list + detail), wider cards, more whitespace
- Desktop: Multi-column grids, persistent sidebars, hover interactions, keyboard shortcuts
- Don't just stretch the mobile layout — restructure it to take advantage of space

---

## 4. Screen Layout Principles

### Visual Hierarchy
Every screen needs a clear hierarchy so users know what to look at first, second, third:

1. **Primary element** — The main content or action (largest, boldest, most prominent)
2. **Secondary elements** — Supporting information (medium weight, normal size)
3. **Tertiary elements** — Metadata, timestamps, helper text (smallest, muted color)

Create hierarchy through:
- **Size**: Larger = more important
- **Weight**: Bolder = more important
- **Color**: High contrast = more important, muted = less important
- **Position**: Top and center = more important, bottom and edges = less
- **Whitespace**: More space around something = more importance (it breathes)

### Spacing & Alignment

**The 8-point grid:**
All spacing and sizing should be multiples of 8px (or 4px for fine adjustments):
- 4px — Tight spacing (between related text elements)
- 8px — Small spacing (between items in a group)
- 12px — Compact spacing
- 16px — Standard spacing (default gap between sections)
- 24px — Comfortable spacing (between card sections)
- 32px — Large spacing (between major page sections)
- 48px — Extra large (page margins, section separators)
- 64px+ — Dramatic spacing (hero sections, breathing room)

**Alignment rules:**
- Everything should align to something — never "eyeball" placement
- Use Figma's layout grids (columns: 4 on mobile, 8 on tablet, 12 on desktop)
- Left-align text in most cases (centered text is hard to scan beyond 2–3 lines)
- Consistent padding: If cards have 16px internal padding, ALL cards have 16px
- Page margins: 16px on mobile, 24px on tablet, 32–64px on desktop

### Card Design
Cards are the most versatile UI pattern. Good card design:
- Clear visual boundary (subtle shadow, border, or background difference)
- Consistent internal padding (16–20px)
- Logical content order: Image → Title → Description → Metadata → Action
- One primary action per card
- Touch target is the entire card for navigation (not just a tiny "View" link)
- Consistent card height in grids (or use masonry layout intentionally)

### Whitespace
Whitespace is not wasted space — it's a design tool:
- More whitespace = more elegance and focus
- Group related items by reducing space between them
- Separate unrelated items by increasing space between them
- Generous whitespace around CTAs makes them more noticeable
- Cramped layouts feel stressful; spacious layouts feel premium

---

## 5. Navigation Patterns

### Mobile Navigation

**Bottom Tab Bar (Recommended for most apps)**
- 3–5 tabs maximum (5 is the absolute limit)
- Always use icon + text label (icons alone are ambiguous)
- Active tab clearly highlighted (color change, filled icon, indicator dot/line)
- Fixed at bottom, always visible during scroll
- Each tab tap should return to the top of that section's stack
- Badge indicators for notifications/counts

**When to use bottom tabs:**
- App has 3–5 equally important top-level sections
- Users switch between sections frequently
- Examples: Instagram, Spotify, Banking apps

**When to use something else:**
- 2 sections → Segmented control or top tabs
- 6+ sections → Bottom tabs (top 4–5) + "More" tab or hamburger for the rest
- Content-focused single-flow → No persistent nav, use back arrows
- Utility/tool app → Contextual toolbar

**Top Tab Bar / Segmented Control**
- Use for switching between related content within a section
- Swipeable between tabs (horizontal swipe gesture)
- Limit to 2–5 tabs
- Good for: Inbox/Sent/Drafts, For You/Following, All/Active/Completed

**Hamburger Menu (Use Sparingly)**
- Hides navigation — users forget what's available
- OK for secondary/settings navigation
- Never use as the ONLY navigation on mobile
- If you must use it, add key shortcuts to the main screen

**Bottom Sheet**
- Great for temporary selections, confirmations, filters
- Feels native on mobile (iOS and Android both use this pattern)
- Should be dismissible by swiping down or tapping backdrop
- Sizes: Small (partial screen), Medium (half screen), Full (expanded)

### Desktop Navigation

**Top Navigation Bar**
- Logo on the left
- Primary nav items center or right-aligned
- Search bar if the app has content discovery
- User avatar / account menu in top-right
- Height: 56–64px (keep it slim)
- Sticky on scroll for long pages

**Side Navigation (for complex apps)**
- Collapsible sidebar (expanded: icon + label, collapsed: icon only)
- Section groupings with dividers
- Active item clearly highlighted
- Works well for: Dashboards, admin panels, design tools, email clients
- Width: 240–280px expanded, 64–72px collapsed

**Breadcrumbs**
- For deep hierarchies (3+ levels deep)
- Shows the user's location in the structure
- Each level is clickable to navigate back
- Place below the top nav bar

### Navigation Picker by App Complexity

| App Complexity | Pattern | Real-World Examples |
|----------------|---------|---------------------|
| Simple (2–5 sections) | Bottom Tab Bar | Instagram, Spotify |
| Medium (5–8 sections) | Bottom Tabs + Hamburger for overflow | Gmail |
| Complex (8+ sections) | Tab Bar + Nested Stacks | Banking apps |
| Content-heavy | Tab Bar + Search + Pull-to-refresh | Reddit, Twitter/X |
| Utility/Tool | Contextual toolbar + gestures | Camera, Maps |
| Dashboard/Admin (web) | Sidebar + Top bar | Notion, Figma, Jira |

### Navigation Transitions

| Transition | When to Use |
|-----------|-------------|
| Push right → left | Going deeper (list → detail) |
| Push left → right | Going back |
| Bottom sheet up | Temporary task, filter, selection |
| Modal / overlay | Focused task, confirmation |
| Fade | Switching between tabs at same level |
| Shared element | Expanding a card to detail view |

---

## 6. Touch & Interaction Design

### Touch Target Sizes (Critical for Mobile)

**Minimum sizes:**
- WCAG 2.2 AA minimum: 24 × 24px
- Apple HIG recommendation: 44 × 44pt (points)
- Material Design recommendation: 48 × 48dp
- Best practice for primary buttons: 48px height minimum

**What this means in Figma:**
- Buttons: Minimum 44px tall, ideally 48–52px for primary actions
- List items: Minimum 48px row height for tappable items
- Icon buttons: Even if the icon is 24px, the tap area frame should be 44×44px
- Tab bar items: Each tab target should be at least 48px wide and 48px tall
- Checkboxes/Radios: The tap area should be the entire row, not just the tiny circle

**Spacing between touch targets:**
- Minimum 8px gap between tappable elements
- Recommended 12–16px for comfortable spacing
- Never let two tappable items touch or overlap — users will mis-tap

### Designing for Touch

**Finger, not cursor:**
- A finger covers ~44px of screen. A cursor is 1px precise. Design accordingly.
- No hover states exist on mobile — everything must work with tap only
- Swipe gestures should have clear affordances (visual hints that swiping is possible)

**Common touch gestures to design for:**

| Gesture | Common Use | Design Consideration |
|---------|-----------|---------------------|
| Tap | Select, activate, navigate | Clear tap targets, visual feedback |
| Long press | Secondary actions, reorder | Show tooltip or context menu |
| Swipe horizontal | Delete, archive, reveal actions | Peek hint showing action behind |
| Swipe vertical | Scroll, pull-to-refresh | Loading indicator on pull |
| Pinch | Zoom in/out | Mainly for images and maps |
| Double tap | Zoom, like/favorite | Quick visual feedback |

**Gesture discoverability:**
Users won't find gestures unless you hint at them:
- Show a slight peek of swipeable content at the edge
- Use onboarding hints for non-obvious gestures (once, not every time)
- Always provide a visible button alternative for gesture-only actions

### Interactive Element States

Every interactive element in your Figma design should have these variant states:

1. **Default** — resting state
2. **Hover** — cursor over it (desktop only)
3. **Pressed / Active** — being tapped/clicked (slight scale down or color change)
4. **Focused** — selected via keyboard (visible focus ring — important for accessibility)
5. **Disabled** — not currently available (reduced opacity, ~40–50%)
6. **Loading** — processing an action (spinner or progress indicator replacing label)

Create these as **Figma component variants** so every instance automatically has all states.

### Advanced Interaction Patterns

**Focus Management (Modals, Drawers, Dialogs):**
| Rule | Implementation |
|------|---------------|
| Trap focus inside open modals | Tab cycles through modal elements only — never escapes to background |
| Return focus on close | Focus returns to the element that triggered the modal |
| Close on Escape key | Always — no exceptions |
| Close on overlay click | Yes for non-critical modals. Disable for destructive confirmations |
| Prevent background scroll | `body { overflow: hidden }` when modal is open |
| Announce to screen readers | `role="dialog"` + `aria-modal="true"` + `aria-labelledby` |

**Keyboard Shortcut Design:**
| Category | Pattern | Examples |
|----------|---------|---------|
| Navigation | Single letter or arrow keys | `J/K` scroll items, `←→` switch tabs |
| Actions | Modifier + letter | `Cmd+S` save, `Cmd+Enter` submit |
| Search | `/` to focus search bar | Gmail, Slack, GitHub, Figma |
| Cancel | `Escape` | Close modals, cancel editing, deselect |
| Discovery | `?` shows shortcut list | Gmail, YouTube, Notion |

Rules: Never hijack browser defaults. Always show shortcuts in tooltips. Provide a shortcut reference panel (`?` key).

**Drag & Drop:**
| Element | Specification |
|---------|--------------|
| Grab handle | Visible grip icon (6 dots or ≡), min 24×44px touch target |
| Drag preview | Ghost/shadow of dragged item at 50% opacity following cursor |
| Drop zone | Highlight valid targets with border + background color change |
| Invalid zone | Show "not allowed" cursor, no highlight |
| Reorder feedback | Animate other items sliding apart to show insertion point |
| Drop confirmation | Item snaps into position with 200ms ease-out animation |
| Accessibility alt | "Move up" / "Move down" buttons as keyboard alternative |

**Gesture Conflict Resolution:**
| Gesture | Potential Conflict | Resolution |
|---------|--------------------|------------|
| Horizontal swipe on card | Conflicts with page swipe / back gesture | Add 20px dead zone from screen edges |
| Pull to refresh | Conflicts with scroll-up in long list | Only trigger at scroll position 0 (top) |
| Long press | Conflicts with text selection | Disable text selection on interactive long-press elements |
| Pinch to zoom | Conflicts with page zoom on mobile web | Use `touch-action: manipulation` on zoomable elements |
| Two-finger scroll | May conflict with map pan | Require "Use two fingers to scroll map" overlay on embedded maps |

**Scroll-Linked Behaviors:**
| Pattern | When to Use | Implementation |
|---------|------------|---------------|
| Sticky header | Navigation should always be reachable | `position: sticky; top: 0` with shadow on scroll |
| Hide-on-scroll header | More content space on mobile | Show on scroll up, hide on scroll down; always show at top |
| Parallax | Hero sections, marketing pages | Keep subtle (0.3–0.5 ratio). Disable on mobile for performance |
| Infinite scroll | Feeds, discovery content | Add "Back to top" FAB after 3+ scrolls. Show skeleton loaders |
| Snap scrolling | Carousels, stories, onboarding | `scroll-snap-type: x mandatory` with visible pagination dots |

---

## 7. Typography & Readability

### Type Scale

Use a consistent type scale across your entire design:

| Token | Size | Use Case |
|-------|------|----------|
| Display | 36–60px | Hero headlines, marketing pages |
| H1 | 28–36px | Page titles |
| H2 | 22–28px | Section headings |
| H3 | 18–22px | Sub-section headings |
| H4 | 16–18px | Card titles, list group headings |
| Body Large | 18px | Lead paragraphs, emphasis |
| Body | 16px | Default body text (MINIMUM on mobile) |
| Body Small | 14px | Secondary text, captions, metadata |
| Caption | 12px | Timestamps, labels, helper text |
| Overline | 11–12px | Category labels, section markers (usually uppercase + tracked out) |

Define each as a **Figma Text Style** with font family, size, weight, line height, and letter spacing baked in.

### Typography Rules

**Readability first:**
- Body text: **16px minimum on mobile** — non-negotiable. Anything smaller causes eye strain and triggers iOS auto-zoom on form inputs
- Line height: 1.4–1.6× the font size for body text (16px font → 24px line height)
- Line length: 45–75 characters per line for comfortable reading (~280–500px wide depending on font size)
- Paragraph spacing: At least 1.5× the line height between paragraphs

**Hierarchy through typography:**
- Use 2 font weights maximum in most interfaces (Regular + Semibold or Medium + Bold)
- Size difference between hierarchy levels should be noticeable — at least 2–4px steps
- ALL CAPS sparingly — only for short labels, overlines, or buttons (never for sentences)
- Avoid italic for body text on screens — it's harder to read at small sizes

**Font pairing:**
- One font family is usually enough for app UI (different weights create hierarchy)
- If using two fonts: one for headings, one for body — ensure they complement each other
- Sans-serif fonts are standard for digital interfaces (better screen rendering)
- Serif fonts work well for editorial/reading-heavy apps or to add personality to headings

**Responsive typography:**
- Mobile: Slightly smaller heading sizes, same body size (never below 16px)
- Desktop: Larger headings, more generous spacing
- Don't just scale everything proportionally — headings can grow more, body stays stable

---

## 8. Color, Contrast & Theming

### Building a Color System

Design with **semantic tokens** — name colors by purpose, not by hue:

| Token Name | Purpose | Example Use |
|-----------|---------|-------------|
| Background / Primary | Main screen background | Page bg, card bg |
| Background / Secondary | Subtle contrast areas | Sidebar bg, input fill, section bg |
| Background / Tertiary | Even subtler fills | Hover states, tag fills |
| Text / Primary | Main readable text | Body text, headings |
| Text / Secondary | Supporting text | Descriptions, timestamps |
| Text / Tertiary | Least important text | Placeholders, disabled text |
| Accent / Primary | Main brand action color | Primary buttons, links, active tabs |
| Accent / Hover | Slightly darker accent | Button hover state |
| Border / Default | Subtle separators | Card borders, input borders |
| Border / Strong | More visible separators | Active input borders, dividers |
| Success | Positive outcomes | Success messages, checkmarks, confirmation |
| Warning | Caution states | Warning banners, low stock indicators |
| Error / Destructive | Negative outcomes | Error messages, delete buttons |
| Info | Informational states | Help text, info banners |

Set these up as **Figma Variables** with Light and Dark modes so you can switch themes instantly.

### Contrast Rules (WCAG 2.2)

| Element | Minimum Contrast Ratio |
|---------|----------------------|
| Regular body text (under 18px bold / 24px regular) | 4.5:1 |
| Large text (18px+ bold or 24px+ regular) | 3:1 |
| UI components (icons, borders, form fields) | 3:1 |
| Enhanced / AAA for regular text | 7:1 |
| Decorative elements | No requirement |

**Common contrast failures to watch for:**
- Light gray text on white background
- White text on light-colored buttons (yellow, light green, light blue)
- Placeholder text that's too faint to read
- Disabled states that look identical to enabled states
- Text overlaid on photographs without a scrim/overlay

Use Figma plugins like **"Contrast"** or **"Stark"** to check ratios directly in your designs.

### Dark Mode Design

**Dark mode is NOT just inverting colors:**
- Backgrounds: Very dark gray (#0A0A0F to #1A1A2E), NOT pure black (#000000). Pure black causes harsh eye strain and "smearing" on OLED screens
- Text: Off-white (#E1E1E6 to #F1F1F4), NOT pure white (#FFFFFF). Pure white on dark backgrounds causes glare
- Accent colors: Slightly desaturate and lighten them for dark mode
- Shadows: Don't work well on dark backgrounds. Use subtle lighter borders or surface elevation instead
- Elevation: In dark mode, higher elevation = lighter surface (opposite of light mode where elevation = more shadow)
- Images: Consider slightly reducing brightness or adding a subtle dark overlay so they don't "blast" users

---

## 9. Forms & Data Input

Forms are where users work hardest. Making forms easy = making users happy.

### Form Design Rules

**Labels:**
- Always show a persistent label — NEVER use placeholder text as the only label
- Labels go above the input field (top-aligned labels have fastest completion times in studies)
- Labels should be concise but descriptive ("Email address" not "Enter your email address here")
- Mark required fields with asterisk (*) or "Required" text
- Mark optional fields with "(Optional)" if most fields are required

**Input fields:**
- Full width on mobile (single column)
- Minimum 48px height for comfortable tapping
- Clear visual boundary (border or background fill contrast)
- Active/focus state clearly different from default (color change, thicker border, glow)
- Avoid underline-only inputs — they have poor discoverability and break visual rhythm

**Validation:**
- Validate on blur (when user leaves the field), not on every keystroke
- Show errors inline, directly below the field — not in a banner at the page top
- Error messages should say WHAT went wrong and HOW to fix it
  - ❌ Bad: "Invalid input"
  - ✅ Good: "Please enter a valid email address (e.g., name@example.com)"
- Use color + icon + text for errors (never color alone — colorblind users exist)
- Show success/check for fields that are correctly filled (especially passwords, usernames)

**Mobile keyboard optimization:**
Design the right keyboard for each input type:

| Input | Keyboard Type | Design Notes |
|-------|--------------|-------|
| Email | Email keyboard (with @) | Also enable autocomplete |
| Phone | Numeric keypad | Format number as user types |
| Money | Decimal keypad | Show currency symbol inline |
| Search | Search keyboard (with Search key) | Add clear (X) button |
| Password | Text + show/hide toggle | Show strength and requirements |
| Date | Native date picker | Don't make users type dates manually |
| Quantity | Stepper control (+ / −) | Show current value between buttons |

**Multi-step forms:**
- Break long forms (6+ fields) into 2–5 logical steps
- Show clear progress (Step 2 of 4, or a progress bar)
- Allow going back without losing entered data
- Show a summary/review screen before final submission
- Save progress automatically when possible

**Smart defaults:**
- Pre-select the most common option
- Auto-detect data when possible (location, timezone, country)
- Pre-fill from previously entered data
- Use toggles instead of checkboxes for clear on/off choices
- Date fields: default to today or the most likely date

---

## 10. Feedback, States & Microinteractions

### Every Screen Needs States

When designing any screen in Figma, design ALL of these states as separate frames:

**Loading state:**
- Skeleton screens (gray placeholder shapes mimicking the layout) are better than spinners
- Show skeleton screens for content that takes more than ~500ms to load
- Spinners are OK for small focused actions (button loading, submitting a form)
- Progress bars for operations with known duration (uploading a file, syncing)

**Empty state:**
- Never show a blank screen
- Include: Illustration or icon + Clear headline + Brief description + CTA button
- Example: "No messages yet" → "Start a conversation with a seller" → [Send Message] button

**Error state:**
- Explain what happened in simple language (no error codes or tech jargon)
- Suggest how to fix it or provide a retry action
- Don't blame the user ("Something went wrong" not "You made an error")
- For connection errors: Show cached data if possible, offer retry

**Success state:**
- Confirm the action was completed clearly
- Animated checkmark or brief celebration moment adds delight
- Guide the user to the next step ("Payment complete! View your order →")
- Never dead-end — always provide a next action or path back

### Microinteractions

Small animations that make the interface feel alive and responsive:

**Essential microinteractions to design:**
- Button press: Slight scale-down and color darkening on tap
- Toggle switch: Smooth slide animation with color transition
- Pull-to-refresh: Animated loading indicator
- Swipe actions: Color reveal behind the swiped item
- Tab switch: Indicator bar slides between tabs
- Like/favorite: Heart fill animation
- Add to cart: Item icon animates toward cart

**Rules for microinteractions:**
- Keep them under 300ms (longer feels sluggish)
- They should communicate feedback, not just decorate
- Same action = same animation everywhere in the app
- Consider users who prefer reduced motion (provide non-animated fallback)

### Toast / Snackbar Notifications
- Position: Bottom of screen on mobile, bottom-right on desktop
- Auto-dismiss: 3–5 seconds for info, persistent for errors
- Include undo for destructive actions (delete, archive)
- Maximum 1–2 lines of text
- Don't stack more than 2 at once

---

## 11. Empty, Error & Edge States

These are the most overlooked screens. Covering them is what separates good UX from great UX.

### Edge Case Checklist

For every screen you design, ask:

| Question | State to Design |
|----------|----------------|
| What if there's no data? | Empty state |
| What if data is loading? | Skeleton / loading state |
| What if loading fails? | Error state with retry |
| What if there's only 1 item? | Single-item layout |
| What if there are 1,000+ items? | Pagination, infinite scroll, or load-more |
| What if the text is very long? | Truncation with "Show more" |
| What if the text is very short? | Layout shouldn't break or look empty |
| What if an image doesn't load? | Placeholder / fallback image |
| What if the user has no permission? | Locked / upgrade prompt |
| What if the user is offline? | Offline state with cached data or message |
| What if the session expires? | Re-authentication prompt |
| What if it's the user's first time? | Onboarding or first-run experience |

### Empty State Anatomy
```
┌─────────────────────────────────┐
│                                 │
│      [ Illustration / Icon ]    │
│                                 │
│      Headline (Bold, Clear)     │
│                                 │
│    Brief supporting text that   │
│    explains what this area is   │
│    for and what they can do     │
│                                 │
│      [ Primary CTA Button ]    │
│                                 │
└─────────────────────────────────┘
```

**Good empty state examples:**
- Shopping cart: "Your cart is empty" + [Browse Popular Items]
- Search results: "No results for 'xyz'" + suggested keywords + popular searches
- Chat/Messages: "No conversations yet" + [Start a Chat]
- Notifications: "You're all caught up!" + [Browse What's New]

---

## 12. Accessibility in Design

Accessibility isn't an add-on — it's a design requirement. ~15–20% of people have some form of disability.

### Accessible Design Checklist

**Color & Contrast:**
- [ ] Text meets 4.5:1 contrast ratio against background
- [ ] Large text (18px+ bold) meets 3:1 ratio
- [ ] UI elements (icons, borders, inputs) meet 3:1 ratio
- [ ] Color is NEVER the only indicator — always pair with icon, text, or pattern
- [ ] Errors use red + icon + text, not just red color
- [ ] Success uses green + icon + text, not just green color

**Touch & Motor:**
- [ ] All touch targets at least 44×44px
- [ ] At least 8px spacing between touch targets
- [ ] No time-limited interactions without extension option
- [ ] Gesture-only actions have a visible button alternative
- [ ] Drag-and-drop has a non-drag alternative

**Vision:**
- [ ] Body text at least 16px, no text below 12px anywhere
- [ ] Interface works zoomed to 200%
- [ ] Icons and imagery are clear at small sizes
- [ ] No critical text embedded in images
- [ ] Dark mode available

**Cognitive:**
- [ ] Clear, simple language (no jargon unless domain-appropriate)
- [ ] Consistent navigation and layout patterns across screens
- [ ] Error messages explain the problem AND suggest a fix
- [ ] Destructive actions require confirmation
- [ ] Multi-step processes show progress indicators

**Screen Reader Annotations (for developer handoff):**
- Note heading hierarchy (H1 → H2 → H3, no skipping levels)
- Note alt text descriptions for meaningful images
- Icons that convey information need text labels (not just tooltip-on-hover)
- Mark decorative images so devs know to skip them
- Note the reading/focus order if it differs from visual layout

---

## 13. Platform Conventions

### iOS Design (Apple HIG)

**Navigation:** Bottom tab bar (up to 5 tabs), top nav bar with back (left) + title (center) + actions (right), large titles that shrink on scroll, swipe-from-left-edge to go back

**UI style:** Rounded rectangles (12–16px radius), SF Symbols for icons, blur/translucent overlays, action sheets from bottom for contextual choices, segmented controls for 2–5 toggles

**Behaviors:** Pull-to-refresh, swipe-to-delete, haptic feedback, respect Dynamic Type and safe areas (notch, Dynamic Island, home indicator)

### Android / Material Design 3

**Navigation:** Bottom nav bar (3–5 items) or navigation drawer, top app bar with left-aligned title, FAB for the primary action, system-level back button

**UI style:** Dynamic color from wallpaper (Material You), elevated surfaces with shadow, chips for filters/selections, bottom sheets, ripple effect on touch

**Behaviors:** Edge-to-edge content behind system bars, predictive back gesture preview, snackbar notifications at bottom

### Web (Desktop)

**Navigation:** Horizontal top nav or sidebar for complex apps, breadcrumbs for deep pages, prominent search bar

**Interactions:** Hover states on all interactive elements, keyboard shortcuts, tooltips on icon-only buttons, right-click context menus

**Layout:** Design at 1440px standard, test at 1280px minimum, cap content width at 1200–1440px on large screens

---

## 14. Design Review Checklist

### Before handoff, verify:

**UX:**
- [ ] Primary action is immediately obvious on every screen
- [ ] User can complete their goal in minimum steps
- [ ] Back / Cancel / Close available on every screen
- [ ] No dead-end screens
- [ ] Error, empty, and loading states all designed
- [ ] Destructive actions require confirmation
- [ ] Content is scannable (clear hierarchy, not walls of text)

**UI:**
- [ ] Consistent 8px-grid spacing
- [ ] Clear typography hierarchy
- [ ] Color contrast passes WCAG AA
- [ ] Touch targets ≥ 44px with adequate spacing
- [ ] Consistent corner radius, icon style, and shadow usage
- [ ] Pixel-perfect alignment throughout

**Mobile:**
- [ ] Works at 375px width
- [ ] Primary actions in thumb zone
- [ ] Body text ≥ 16px
- [ ] No hover-dependent interactions
- [ ] Safe areas accounted for
- [ ] Keyboard doesn't cover active inputs

**Responsive:**
- [ ] Mobile (375px): Single column, touch-first
- [ ] Tablet (768px): Adapted layout, possibly two panels
- [ ] Desktop (1440px): Full grid, sidebars, hover states
- [ ] No unintentional overflow at any breakpoint

**Handoff:**
- [ ] Components use design system variants/instances
- [ ] Full auto layout from screen frame to deepest child (see §15 Auto Layout Architecture)
- [ ] Layers named clearly (not "Frame 427")
- [ ] Annotations for non-obvious interactions
- [ ] Design tokens / Figma variables used for colors, spacing, type
- [ ] All component states designed and labeled

---

## 15. Figma Workflow Best Practices

### File Organization
- **Pages**: Cover, User Flows, Screens, Components, Design Tokens, Archive
- **Sections**: Group frames by feature or flow within each page
- **Naming**: "Login / Default", "Login / Error State", "Login / Loading"
- **Status markers**: 🟢 Ready for dev | 🟡 In progress | 🔴 Needs review | ⚪ Archived

### Component Architecture
- Build components BEFORE screens — design system first, then compose
- Use **variants** for states (Default, Hover, Active, Disabled, Error, Loading)
- Use **component properties** for flexible content (text overrides, boolean show/hide, instance swap)
- Name with categories: "Button / Primary / Large", "Input / Text / Error"
- Apply **auto layout** to every frame — see the full auto layout architecture below

### Full Auto Layout Architecture

**The #1 Rule: Every screen must be 100% auto layout from the outermost frame to the deepest child. Zero manual positioning. If you're dragging a layer to position it, you're doing it wrong.**

Auto layout makes designs responsive, developer-friendly, and maintainable. A screen without full auto layout is a screen that breaks when content changes.

#### Screen Anatomy — The 3-Layer Stack

Every screen follows this structure. The outermost frame IS the screen, and it's a vertical auto layout container:

```
📱 Screen Frame (vertical auto layout, fill container)
│
├── 🔝 Top Bar / Header (fixed height, hug or fill width)
│   └── horizontal auto layout: back button + title + actions
│
├── 📜 Content Area (fill container — takes all remaining space)
│   └── vertical auto layout with scrollable content
│       ├── Section 1 (vertical auto layout)
│       ├── Section 2 (vertical auto layout)
│       └── Section N...
│
└── 🔽 Bottom Bar / Footer (fixed height, hug or fill width)
    └── horizontal auto layout: nav items or action buttons
```

**Key sizing rules:**
- Screen frame: Fixed width (e.g., 393px for mobile), fill height or fixed to device height
- Header: Fill width, hug height (shrinks to content)
- Content area: Fill width, **fill height** (this is critical — it takes the remaining space between header and footer)
- Footer: Fill width, hug height
- Set content area to **clip content** + **vertical scrolling** for overflow

#### Auto Layout Property Cheat Sheet

Every auto layout frame has 6 decisions. Here's how to set them:

| Property | What It Controls | Common Settings |
|---|---|---|
| **Direction** | Vertical or horizontal stacking | Screens/sections = Vertical. Rows/toolbars = Horizontal |
| **Gap** | Space between children | Use spacing tokens: 4/8/12/16/24/32px. Never 0 with manual padding hacks |
| **Padding** | Internal space around all children | Symmetric: 16px all sides. Or asymmetric: top/bottom differ from left/right |
| **Primary axis alignment** | How children stack along the flow direction | Top (vertical) or Left (horizontal) for most cases. Center for hero sections |
| **Counter axis alignment** | How children align perpendicular to flow | Stretch for full-width children. Left-align for mixed-width content |
| **Sizing** | How the frame itself sizes | Hug = shrink to content. Fill = expand to parent. Fixed = explicit pixel value |

#### Width & Height Sizing — When to Use What

| Sizing Mode | When to Use | Example |
|---|---|---|
| **Fill container** | Element should stretch to match its parent's width or height | Content area height, full-width buttons, card widths in a list |
| **Hug contents** | Element should shrink to fit its children exactly | Tags, badges, inline labels, headers, footers |
| **Fixed** | Element has an explicit size that shouldn't change | Screen width (393px), avatars (40×40), icon containers (24×24), images |

**The critical pattern most people get wrong:** The content area between header and footer MUST be `fill height` (not hug, not fixed). This makes it take the remaining space. Combined with clip content + scroll, it creates a proper scrollable body.

#### Common Screen Patterns — Auto Layout Blueprints

**List Screen (e.g., inbox, notifications, transactions):**
```
📱 Screen (V, fill)
├── Header (V, hug H, fill W) — padding: 16 top, 12 bottom
│   ├── Status Bar Spacer (fixed H: 54px, fill W)
│   └── Title Row (H, hug H, fill W) — gap: 8
│       ├── Title Text (fill W)
│       └── Action Icons (H, hug) — gap: 12
├── Search/Filter Bar (H, hug H, fill W) — padding: 12, 16
│   └── Search Input (fill W)
├── List Content (V, fill H, fill W) — gap: 0, clip + scroll
│   ├── List Item 1 (H, hug H, fill W) — padding: 16, gap: 12
│   │   ├── Avatar (fixed 40×40)
│   │   ├── Text Stack (V, hug H, fill W) — gap: 4
│   │   │   ├── Title (fill W)
│   │   │   └── Subtitle (fill W)
│   │   └── Metadata (hug)
│   ├── Divider (fixed H: 1px, fill W)
│   ├── List Item 2...
│   └── List Item N...
└── Bottom Nav (H, hug H, fill W) — padding: 8, 16 bottom + safe area
    ├── Nav Item (V, hug, fill W) — gap: 4, center aligned
    ├── Nav Item...
    └── Nav Item...
```

**Detail Screen (e.g., product detail, profile, article):**
```
📱 Screen (V, fill)
├── Header (H, hug H, fill W) — padding: 16, 12
│   ├── Back Button (fixed 44×44)
│   ├── Title (fill W, center)
│   └── Action Menu (fixed 44×44)
├── Scrollable Content (V, fill H, fill W) — gap: 24, clip + scroll
│   ├── Hero Image (fixed H: 240, fill W)
│   ├── Title Section (V, hug H, fill W) — padding: 0 16, gap: 8
│   │   ├── Title Text (fill W)
│   │   ├── Subtitle Row (H, hug H, fill W) — gap: 8
│   │   └── Tag Row (H, wrap, hug H, fill W) — gap: 8
│   ├── Description (V, hug H, fill W) — padding: 0 16, gap: 12
│   │   └── Body Text (fill W)
│   ├── Section Card (V, hug H, fill W) — padding: 16, gap: 12, margin: 0 16
│   │   ├── Section Header (H, fill W)
│   │   └── Section Content (V, fill W)
│   └── Spacer (fixed H: 100, fill W) — room above sticky footer
└── Sticky Action Bar (H, hug H, fill W) — padding: 16
    └── CTA Button (fill W, fixed H: 48)
```

**Form Screen (e.g., sign up, settings, data entry):**
```
📱 Screen (V, fill)
├── Header (H, hug H, fill W)
│   ├── Cancel (hug)
│   ├── Title (fill W, center)
│   └── Save (hug)
├── Form Content (V, fill H, fill W) — gap: 24, padding: 24 16, clip + scroll
│   ├── Field Group 1 (V, hug H, fill W) — gap: 16
│   │   ├── Group Label (fill W)
│   │   ├── Input Field (V, hug H, fill W) — gap: 4
│   │   │   ├── Label (fill W)
│   │   │   ├── Input (fill W, fixed H: 48) — padding: 12 16
│   │   │   └── Helper/Error Text (fill W)
│   │   └── Input Field 2...
│   ├── Divider (fixed H: 1, fill W)
│   ├── Field Group 2 (V, hug H, fill W) — gap: 16
│   │   └── ...
│   └── Bottom Spacer (fixed H: 32)
└── Action Footer (V, hug H, fill W) — padding: 16, gap: 8
    ├── Primary Button (fill W, fixed H: 48)
    └── Secondary Link (fill W, hug H, center)
```

#### Naming Convention for Auto Layout Frames

Every frame should communicate its purpose and layout direction at a glance:

| Layer Name | What It Is |
|---|---|
| `Screen / [Name]` | Top-level screen frame |
| `Header`, `Footer`, `Bottom Nav` | Fixed structural regions |
| `Content` or `Scroll Content` | The fill-height scrollable body |
| `Section / [Name]` | Grouped content block |
| `Row / [Name]` | Horizontal auto layout container |
| `Stack / [Name]` | Vertical auto layout container |
| `Field / [Name]` | Form input with label + input + helper |

**⚠️ Do NOT create spacer frames (`_sp`, `Spacer`, etc.) between siblings.** Use auto layout `gap` (`itemSpacing`) on the parent frame instead. If children need different spacing, group them into sub-frames (sections) with their own gap.

**Never leave default names.** "Frame 427" is tech debt. Name it as you build it.

#### The Auto Layout Audit Checklist

When reviewing any screen, check ALL of these:

```
STRUCTURE:
- [ ] Screen frame is auto layout (not free-form)
- [ ] Direction is correct (vertical for most screens)
- [ ] Header: fill width, hug height
- [ ] Content area: fill width, FILL HEIGHT (not hug)
- [ ] Footer: fill width, hug height
- [ ] Content area has clip content ON and vertical scroll enabled

SIZING:
- [ ] No child has fixed width inside a fill-width parent (should be fill)
- [ ] Text layers are fill width (not fixed width with hardcoded values)
- [ ] Icons/avatars are fixed size (the only things that should be fixed)
- [ ] No element is wider than its parent (causes overflow)
- [ ] **No placeholder heights** — inputs, buttons, cards, list items must be HUG height, not stuck at a default (e.g., 100px)
- [ ] Only screen frames, images, icons, avatars, dividers, and status bar spacers should have fixed height

SPACING:
- [ ] All gaps use spacing tokens (4/8/12/16/24/32) — no arbitrary values
- [ ] Padding is set on the frame, not faked with spacer frames
- [ ] **No spacer frames between siblings** — parent must use `itemSpacing` (gap), not empty _sp frames between children
- [ ] When siblings need different gaps, group related items into sub-frames with their own gap value
- [ ] Consistent padding across same-level sections
- [ ] No nested padding that doubles up (16px padding inside 16px padding = 32px visual margin)

RESPONSIVENESS:
- [ ] Screen resizes correctly when width changes (test: drag the frame wider/narrower)
- [ ] Text reflows properly — no overflow or clipping
- [ ] Buttons and inputs stretch to fill available width
- [ ] Images maintain aspect ratio (use fill with crop, not stretch)

NESTING:
- [ ] Every frame is auto layout — zero free-positioned children
- [ ] Nesting depth ≤ 6 levels (deeper = too complex, refactor into components)
- [ ] No absolute-positioned layers breaking the auto layout chain
- [ ] Constraints are not used as a substitute for auto layout

CLEAN-UP:
- [ ] No hidden "ghost" layers throwing off spacing
- [ ] No zero-height or zero-width frames used as hacks
- [ ] No negative margins or overlap tricks
- [ ] Every frame has a descriptive name (no "Frame 427")
```

#### Common Auto Layout Mistakes & Fixes

| Mistake | What Breaks | Fix |
|---|---|---|
| Content area set to `hug height` instead of `fill` | Screen doesn't fill the device height; footer floats mid-screen | Set content area to **fill container** on height |
| Text set to `fixed width` | Text clips or doesn't reflow when parent resizes | Set text to **fill container** width, hug height |
| Using constraints instead of auto layout | Layout doesn't adapt to content changes | Convert to auto layout. Constraints are for absolute overlays only |
| Spacer frames for padding | Extra unnecessary layers; spacing changes need hunting through layers | Remove spacers, use **padding** on the parent frame instead |
| Spacer frames between siblings (e.g., `_sp` frames) | Parent gap is 0, spacing is faked with empty frames between every element. Creates dozens of ghost layers, makes gap changes tedious, spacers often have wrong width (e.g., 100px fixed). | **Delete all spacer frames.** Set `itemSpacing` (gap) on the parent. If siblings need different gaps, group related items into sub-frames with their own gap values. |
| Elements stuck at placeholder height (e.g., 100px) | Input fields, buttons, cards, list items are all the same height (100px) regardless of content. Creates massive empty space inside elements. | Change `layoutSizingVertical` from `FIXED` to `HUG` so elements shrink-wrap their content. Only screen frames, images, and explicit fixed-size elements (avatars, icons) should have fixed heights. |
| Deeply nested auto layout (8+ levels) | Hard to find where spacing is wrong; slow performance | Flatten by extracting middle layers into components |
| Items overflowing parent | Horizontal scroll appears or content is clipped | Check child widths — something is `fixed` that should be `fill` |
| Different gaps on same-level items | Visual inconsistency | Set gap on the parent, not margins on individual children |
| Absolute position layer inside auto layout | That layer is ignored by the auto layout flow | Re-integrate it into the auto layout structure, or use a separate overlay frame |

#### When to Use Absolute Position (The Only Exceptions)

Full auto layout means auto layout EVERYWHERE — except these specific cases where absolute position is correct:

1. **Floating action buttons (FABs)** — positioned over content, not in the flow
2. **Badges/notification dots** — overlaid on icons or avatars
3. **Overlays and modals** — sit above the screen content
4. **Toast/snackbar notifications** — float above everything
5. **Decorative elements** — background gradients, illustrations behind content

For these, use a **wrapping frame** with the content as auto layout AND the overlay as absolute positioned. Never break the main content's auto layout chain to accommodate an overlay.

```
📱 Screen Wrapper (no auto layout — or auto layout with absolute child)
├── Screen Content (V, auto layout, fill) ← This is your full auto layout screen
└── FAB (absolute position, bottom-right) ← Only this breaks out
```

### Design Tokens as Figma Styles & Variables

**⚠️ Design system setup (text styles + color styles) MUST happen before designing any screens. See `governance.md` for the full initialization procedure.**

Once initialized, the design tokens in Figma should include:

- **Text Styles**: 18 named styles covering Display through Helper (see Step 3A in governance.md)
- **Color Variables**: Primitive palette (50–950 per hue) + Semantic tokens with Light/Dark modes (see Step 3B in governance.md)
- **Spacing Variables**: FLOAT variables for consistent padding and gaps (4, 8, 12, 16, 24, 32, 48, 64)
- **Border Radius Variables**: FLOAT variables matching the project's corner radius setting
- **Shadow Styles**: Effect styles for elevation levels (sm, md, lg, xl)
- Use **variable collections** organized by purpose: Primitives → Semantic → Component-level (3-tier architecture)

**The rule: NO raw values on any design element.**
- Text layer → must reference a Text Style
- Color fill → must reference a Color Style or Color Variable
- Spacing/padding → should use spacing variables (or at minimum, 8px grid values)
- Border radius → should reference a radius variable

### Prototyping
- Connect screens with interaction flows
- Use **Smart Animate** for smooth transitions between similar frames
- Prototype the happy path AND at least one error path
- Set the correct device frame for mobile prototypes
- **Test on a real phone** using Figma Mirror — emulators miss real touch feel
- Set scroll behavior: clip content, define scroll regions

### Collaboration
- Annotate with comments or sticky notes for developer context
- Document interactions: "On swipe left, reveal delete action"
- Document conditional logic: "Show badge only when count > 0"
- Link to flow diagrams for bigger-picture context
- **Review with developers before finalizing** — they catch feasibility issues early

---

## 16. Design-to-Dev Handoff

A beautiful design file is worthless if developers can't implement it accurately. This section ensures your handoff is complete, unambiguous, and developer-friendly.

### Token Naming Conventions

Use a consistent naming system that maps directly to code. Developers should never have to guess what a design value maps to.

**Naming Format**: `category/property/variant/state`

| Design Token | CSS Variable | Value |
|-------------|-------------|-------|
| `color/primary/default` | `--color-primary-default` | `#2563EB` |
| `color/primary/hover` | `--color-primary-hover` | `#1D4ED8` |
| `color/text/primary` | `--color-text-primary` | `#111827` |
| `color/text/secondary` | `--color-text-secondary` | `#6B7280` |
| `color/surface/default` | `--color-surface-default` | `#FFFFFF` |
| `color/surface/elevated` | `--color-surface-elevated` | `#F9FAFB` |
| `color/border/default` | `--color-border-default` | `#E5E7EB` |
| `color/error/default` | `--color-error-default` | `#DC2626` |
| `color/success/default` | `--color-success-default` | `#16A34A` |
| `spacing/xs` | `--spacing-xs` | `4px` |
| `spacing/sm` | `--spacing-sm` | `8px` |
| `spacing/md` | `--spacing-md` | `16px` |
| `spacing/lg` | `--spacing-lg` | `24px` |
| `spacing/xl` | `--spacing-xl` | `32px` |
| `spacing/2xl` | `--spacing-2xl` | `48px` |
| `radius/sm` | `--radius-sm` | `4px` |
| `radius/md` | `--radius-md` | `8px` |
| `radius/lg` | `--radius-lg` | `16px` |
| `radius/full` | `--radius-full` | `9999px` |
| `shadow/sm` | `--shadow-sm` | `0 1px 2px rgba(0,0,0,0.05)` |
| `shadow/md` | `--shadow-md` | `0 4px 6px rgba(0,0,0,0.07)` |
| `shadow/lg` | `--shadow-lg` | `0 10px 15px rgba(0,0,0,0.1)` |
| `font/size/xs` | `--font-size-xs` | `12px` |
| `font/size/sm` | `--font-size-sm` | `14px` |
| `font/size/base` | `--font-size-base` | `16px` |
| `font/size/lg` | `--font-size-lg` | `18px` |
| `font/size/xl` | `--font-size-xl` | `24px` |
| `font/size/2xl` | `--font-size-2xl` | `32px` |
| `font/weight/normal` | `--font-weight-normal` | `400` |
| `font/weight/medium` | `--font-weight-medium` | `500` |
| `font/weight/semibold` | `--font-weight-semibold` | `600` |
| `font/weight/bold` | `--font-weight-bold` | `700` |

### 3-Tier Token Architecture

Industry-standard systems use three layers — Figma Variables should mirror this:

```
┌─────────────────────────────────────────────┐
│  TIER 1: Primitive Tokens (raw values)      │
│  blue-500: #2563EB                          │
│  gray-900: #111827                          │
│  space-4: 16px                              │
│  These are the actual values. Designers     │
│  and devs rarely reference these directly.  │
├─────────────────────────────────────────────┤
│  TIER 2: Semantic Tokens (meaning)          │
│  color-primary: {blue-500}                  │
│  color-text-body: {gray-900}               │
│  spacing-content: {space-4}                 │
│  These describe purpose. Used in design     │
│  specs and component props.                 │
├─────────────────────────────────────────────┤
│  TIER 3: Component Tokens (scoped)          │
│  button-bg: {color-primary}                 │
│  button-text: {color-on-primary}            │
│  button-padding-x: {spacing-content}        │
│  These are component-specific. Used in      │
│  component code and Figma component props.  │
└─────────────────────────────────────────────┘
```

**Why this matters**: When you rebrand (change blue to purple), you update ONE primitive token. Semantic and component tokens cascade automatically. Without this, devs do find-and-replace across the entire codebase.

### Component API Spec

For every component you hand off, document its API — what props a developer needs to implement:

```
## Component: Button

### Props
| Prop       | Type                              | Default   | Description                    |
|------------|-----------------------------------|-----------|--------------------------------|
| variant    | "primary" | "secondary" | "ghost" | "primary" | Visual style                   |
| size       | "sm" | "md" | "lg"               | "md"      | Button size                    |
| label      | string                            | required  | Button text                    |
| icon       | IconName | null                   | null      | Optional leading icon          |
| iconOnly   | boolean                           | false     | Icon without text              |
| disabled   | boolean                           | false     | Disabled state                 |
| loading    | boolean                           | false     | Shows spinner, disables click  |
| fullWidth  | boolean                           | false     | Stretches to container width   |
| onClick    | function                          | required  | Click handler                  |

### States
- Default → Hover → Active → Focus → Disabled → Loading

### Keyboard
- Enter/Space: Activate button
- Tab: Focus next element
- Focus ring: 2px offset, primary color
```

### Annotation Standards

Every handoff frame should include these annotations:

**Spacing Annotations:**
- Red lines between elements showing exact pixel values
- Call out when spacing should use a token (e.g., "spacing/md = 16px")
- Mark auto-layout direction and gap values

**Interaction Annotations:**
Document behaviors Figma can't show:
```
INTERACTION SPEC:
- Element: "Swipe item left"
- Trigger: Horizontal swipe gesture (threshold: 80px)
- Action: Reveal delete button (red, 64px wide)
- Animation: Slide content left, 200ms ease-out
- Tap delete: Show confirmation bottom sheet
- Tap outside / swipe right: Dismiss, snap back 300ms ease-in-out
```

**Conditional Logic Annotations:**
```
CONDITIONAL LOGIC:
- IF user.verified === true → Show green badge + "Verified" label
- IF user.verified === false → Show "Verify now" CTA
- IF items.length === 0 → Show empty state (see frame: "Empty State")
- IF items.length > 99 → Show "99+" badge
- IF error → Show error state (see frame: "Error State")
```

**Responsive Behavior Annotations:**
```
RESPONSIVE SPEC:
- Mobile (< 768px): Single column, cards stack vertically
- Tablet (768–1024px): 2-column grid, sidebar collapses to drawer
- Desktop (> 1024px): 3-column grid, fixed sidebar, max-width 1280px
- Breakpoint: Cards reflow at 768px, not scale
```

### Dev Handoff Checklist

Before marking any design "ready for development":

| # | Check | Done? |
|---|-------|-------|
| 1 | All colors reference design tokens (no hardcoded hex in specs) | |
| 2 | All spacing uses the spacing scale (no arbitrary values) | |
| 3 | All text styles are named and documented | |
| 4 | Component states designed: default, hover, active, focus, disabled, loading, error | |
| 5 | Responsive behavior annotated for each breakpoint | |
| 6 | Interaction behavior documented (what Figma can't prototype) | |
| 7 | Conditional logic documented (if/then/else states) | |
| 8 | Edge cases handled: empty, loading, error, max content, min content | |
| 9 | Accessibility notes: ARIA roles, labels, keyboard behavior, focus order | |
| 10 | Assets exported: icons (SVG), images (2x), illustrations | |
| 11 | Component API specified (props, types, defaults) | |
| 12 | Animation/transition specs documented (duration, easing, properties) | |
| 13 | Dark mode variants designed (if applicable) | |
| 14 | Real content tested (not lorem ipsum — actual max-length strings) | |
| 15 | Developer reviewed the designs before handoff | |

---

## 17. Content & UX Writing

Good UX writing is invisible — users never notice it. Bad UX writing causes confusion, support tickets, and abandonment. Every word in your UI is a design decision.

### Voice & Tone Framework

Define these for every project:

| Attribute | What to Decide | Example |
|-----------|---------------|---------|
| **Voice** | Your brand's personality (constant across all screens) | Confident, Friendly, Expert, Playful |
| **Tone** | How the voice adapts to context (varies by situation) | Celebratory on success, Empathetic on errors, Direct on forms |
| **Person** | First person (I/My), Second person (You/Your), or Third | "Your order" vs "The order" vs "My order" |
| **Formality** | Casual, Conversational, Professional, Formal | "Got it!" vs "Acknowledged" vs "Your request has been received" |

### Microcopy Rules

**Buttons & Actions:**
| ✅ Do | ❌ Don't | Why |
|-------|---------|-----|
| "Save changes" | "Submit" | Tells user what will happen |
| "Delete account" | "Proceed" | Specific > vague |
| "Add to cart — $29" | "Add to cart" | Reduces surprise |
| "Continue to payment" | "Next" | Describes destination |
| "Sign up free" | "Register" | Removes friction word + adds incentive |
| "Undo" (after action) | "Are you sure?" (before) | Less friction, more forgiving |

**Primary vs secondary action labels:**
- Primary: Verb + noun ("Save changes", "Send message", "Create account")
- Secondary: Verb only or noun only ("Cancel", "Back", "Skip")
- Destructive: Be explicit ("Delete this file", not "Remove" or "Delete")

**Form Labels & Placeholders:**
| Element | Rule | Good | Bad |
|---------|------|------|-----|
| Label | Always visible, above field | "Email address" | (no label) |
| Placeholder | Show format hint only | "name@example.com" | "Enter your email" |
| Helper text | Clarify requirements | "Must be 8+ characters with a number" | "Invalid" |
| Error text | Say what's wrong + how to fix | "Password needs at least one number" | "Invalid password" |

**Error Messages:**
Follow this structure: **What happened** + **Why** + **What to do**

| Scenario | ✅ Good | ❌ Bad |
|----------|--------|-------|
| Wrong password | "That password doesn't match. Try again or reset it." | "Error: Invalid credentials" |
| Network failure | "Can't connect right now. Check your connection and try again." | "Error 503" |
| Permission denied | "You need admin access to do this. Contact your team owner." | "Access denied" |
| File too large | "This file is 12MB — max is 5MB. Try compressing it first." | "File exceeds limit" |
| Required field | "First name is required" | "This field is required" |
| Invalid format | "Phone number needs 10 digits (e.g., 555-123-4567)" | "Invalid input" |

**Empty States:**
Every empty state needs three things:
1. **What this space is for** — "This is where your orders will appear"
2. **Why it's empty** — "You haven't placed any orders yet"
3. **What to do** — "Browse products" (actionable CTA)

| Context | Copy Pattern |
|---------|-------------|
| First-time user | Welcoming + instructive: "Welcome! Start by adding your first project." |
| No results | Helpful + alternative: "No results for 'xyz'. Try a different search or browse categories." |
| Completed list | Celebratory: "All caught up! ✓ No pending tasks." |
| No permission | Explanatory: "You'll see shared files here once your team adds you." |

**Confirmation & Success:**
| Action Type | Copy Pattern |
|-------------|-------------|
| Save | "Changes saved" (brief, auto-dismiss) |
| Send | "Message sent to [name]" (confirms recipient) |
| Delete | "File deleted. Undo" (with recovery option) |
| Purchase | "Order confirmed! Check your email for details." (next step) |
| Account action | "Password updated successfully" (confirms what changed) |

**Notification & Alert Copy:**
| Type | Tone | Example |
|------|------|---------|
| Info | Neutral, factual | "Your subscription renews on March 15" |
| Warning | Clear, not alarming | "You're running low on storage (90% used)" |
| Error | Empathetic, actionable | "Payment failed. Update your card to continue." |
| Success | Brief, positive | "Invite sent!" |

### Content Hierarchy Rules

1. **Headlines**: Max 6 words. State the benefit or action, not the feature name.
   - ✅ "Track your deliveries" → ❌ "Delivery Management Module"
2. **Subheadings**: Max 12 words. Add context to the headline.
3. **Body text**: 1–2 sentences per block. One idea per paragraph on mobile.
4. **Lists**: Max 5–7 items before grouping. Front-load the key word.
5. **Legal/fine print**: Below the fold, smaller but still readable (12px min).

### Truncation Rules

| Element | Max Length | Truncation Method |
|---------|-----------|-------------------|
| Card title | 2 lines | Ellipsis after line 2 |
| List item label | 1 line | Ellipsis |
| Navigation label | 1–2 words | Abbreviate or icon-only |
| Toast/snackbar | 1 line (~40 chars) | Truncate with "View details" link |
| Table cell | Fit column | Ellipsis + tooltip on hover |
| Tag/chip | ~20 chars | Ellipsis |
| Filename | ~30 chars | Middle truncation: "my_docume…ment.pdf" |

### Localization-Ready Writing

If i18n is planned or possible:

| Rule | Reason |
|------|--------|
| Avoid idioms ("piece of cake", "heads up") | Don't translate well |
| Don't concatenate strings ("You have " + count + " items") | Word order varies by language |
| Use ICU MessageFormat for plurals | "{count, plural, one {# item} other {# items}}" |
| Leave 30–40% extra space in UI | German/French text is 30% longer than English |
| Don't embed text in images | Can't be translated |
| Use ISO date formats or spell out month | "03/04" is ambiguous (March 4 vs April 3) |
| Avoid directional words ("click left", "above") | RTL languages flip layout |

### UX Writing Checklist

Run this check on every screen:

| # | Check | Pass? |
|---|-------|-------|
| 1 | Every button says what it does (verb + noun) | |
| 2 | No jargon, technical terms, or internal terminology | |
| 3 | Error messages include how to fix the problem | |
| 4 | Empty states have a clear CTA | |
| 5 | Confirmation messages confirm what specifically happened | |
| 6 | Labels are above fields (not just placeholders) | |
| 7 | Destructive actions state what will be destroyed | |
| 8 | Numbers include units ("5 MB", not "5") | |
| 9 | Dates are unambiguous (month spelled out or ISO) | |
| 10 | Tone matches the context (empathetic on errors, brief on success) | |
| 11 | No walls of text — content is scannable | |
| 12 | Longest possible content fits the layout (test with max-length strings) | |

---

## 18. Design Performance Budget

Design decisions directly impact load time. Designers should work within performance constraints — not just aesthetic ones. These budgets ensure your designs are buildable without compromising speed.

### Asset Budgets

| Asset Type | Budget Per Page | Notes |
|-----------|----------------|-------|
| **Total page weight** | < 1.5 MB (mobile), < 3 MB (desktop) | Includes all assets, scripts, fonts |
| **Images (total)** | < 500 KB | Use WebP/AVIF, compress at 80% quality |
| **Single hero image** | < 200 KB | Lazy-load below-the-fold images |
| **Icons** | SVG only, < 2 KB each | Use icon sprite or icon font for sets of 10+ |
| **Illustrations** | < 100 KB each | SVG preferred, compress PNGs |
| **Fonts** | ≤ 2 font families, ≤ 4 weights total | Each weight ≈ 20–50 KB. Subset to used characters |
| **Video (embedded)** | Poster image + lazy-load | Never autoplay on mobile without user consent |

### Font Loading Budget

| Guideline | Why |
|-----------|-----|
| Max 2 font families | Each family adds HTTP requests + render blocking |
| Max 4 weights (e.g., 400, 500, 600, 700) | Each weight is a separate file (~30 KB) |
| Always specify `font-display: swap` | Prevents invisible text during load |
| Subset fonts to Latin (or needed character sets) | Can reduce file by 50–80% |
| Prefer variable fonts if available | One file covers all weights (~50–80 KB total) |

### Animation Performance Budget

| Property | Performance | Use For |
|----------|------------|---------|
| `transform` | ✅ GPU-accelerated, smooth | Move, scale, rotate animations |
| `opacity` | ✅ GPU-accelerated, smooth | Fade in/out transitions |
| `background-color` | ⚠️ Triggers repaint | Hover states (keep transitions short: ≤ 200ms) |
| `width`, `height` | ❌ Triggers reflow | Avoid animating. Use `transform: scale()` instead |
| `box-shadow` | ❌ Expensive to repaint | Use `filter: drop-shadow()` or pre-render |
| `border-radius` | ❌ Triggers repaint | Set once, don't animate |

**Duration Guidelines:**
| Animation Type | Duration | Easing |
|---------------|----------|--------|
| Micro-interaction (button press) | 100–150ms | ease-out |
| Fade in/out | 150–250ms | ease-in-out |
| Slide/expand | 200–350ms | ease-out |
| Page transition | 250–400ms | ease-in-out |
| Complex sequence | 400–600ms max | custom cubic-bezier |

Rule: If you can't perceive the animation ending, it's too long. Users notice delays over 400ms.

### Complexity Budget Per Screen

| Element | Budget | Over Budget = Problem |
|---------|--------|----------------------|
| DOM elements | < 1,500 nodes | > 3,000 = performance issues on mobile |
| Simultaneous animations | ≤ 3 | > 5 = jank on mid-range devices |
| Scroll listeners | ≤ 2 (debounced) | Unthrottled = scroll jank |
| Third-party scripts | ≤ 3 | Each adds 50–200ms to load |
| API calls on page load | ≤ 3 | Stagger or batch if more needed |

### Designer's Performance Checklist

| # | Check | Done? |
|---|-------|-------|
| 1 | Hero images have a max-width constraint (not 4000px originals) | |
| 2 | Icon library uses SVGs, not PNGs/JPGs | |
| 3 | No more than 2 font families specified | |
| 4 | Animations use only transform/opacity where possible | |
| 5 | Below-the-fold content is designed to lazy-load | |
| 6 | Complex illustrations are SVG, not raster | |
| 7 | No autoplay video without user consent | |
| 8 | Shadow/blur effects are minimal on repeated elements (cards in lists) | |
| 9 | Design works without images loaded (skeleton states exist) | |
| 10 | Design tested on mid-range device, not just MacBook Pro | |

---

## 19. Common UX Anti-Patterns

### Navigation
- ❌ Hamburger menu as the ONLY mobile navigation
- ❌ More than 5 items in a bottom tab bar
- ❌ Icon-only navigation without text labels
- ❌ Inconsistent back behavior across screens
- ❌ Deep nesting (4+ levels) without breadcrumbs
- ❌ Navigation that moves or changes layout between screens

### Content & Layout
- ❌ Walls of text with no hierarchy or visual breaks
- ❌ Text on images without a contrast scrim/overlay
- ❌ Unintentional horizontal scrolling
- ❌ Cards with no clear tap target
- ❌ Modal on top of modal
- ❌ Auto-playing media
- ❌ Pop-ups on page load before any user interaction

### Forms
- ❌ Placeholder-only labels (disappear on focus)
- ❌ Validation only on submit (too late, too frustrating)
- ❌ Generic error messages ("Invalid input")
- ❌ Disabled submit with no explanation of what's missing
- ❌ No password visibility toggle
- ❌ Forcing manual input for data you could auto-detect

### Touch & Mobile
- ❌ Touch targets smaller than 44px
- ❌ Interactive elements with no visual affordance
- ❌ Gesture-only actions with no button fallback
- ❌ Tiny close (X) buttons on modals
- ❌ Primary actions at the top (out of thumb reach)
- ❌ Font sizes below 16px for body text on mobile

### Feedback & States
- ❌ Blank screens during loading
- ❌ No empty state design
- ❌ Success with no confirmation feedback
- ❌ Destructive actions with no undo or confirmation
- ❌ Error states with no recovery path

### Visual
- ❌ Low contrast text (especially light gray on white)
- ❌ Inconsistent spacing (16px here, 13px there, 18px somewhere else)
- ❌ Inconsistent border radius across similar elements
- ❌ Color as the only means of conveying information
- ❌ Pure black (#000) backgrounds in dark mode (causes OLED smear and eye strain)

---

## 20. Iconography System

Icons are the visual shorthand of any UI. Broken, inconsistent, or poorly sized icons make an app feel amateur instantly. This section defines the rules.

### Icon Sizing Scale

All UI icons MUST use sizes from this scale. No arbitrary dimensions.

| Token Name | Size | Use Case |
|---|---|---|
| `icon-xs` | 12×12 px | Inline indicators (dots, status pips, chevrons inside tight spaces) |
| `icon-sm` | 16×16 px | Dense UI: table cells, compact list metadata, breadcrumb separators |
| `icon-md` | 20×20 px | Secondary icons: form field trailing icons, small action icons, tags |
| `icon-base` | 24×24 px | **Standard — use this by default.** Nav icons, list leading icons, action bar icons, toolbar, most UI icons |
| `icon-lg` | 32×32 px | Emphasis icons: section headers, feature highlights, onboarding |
| `icon-xl` | 40×40 px | Icon containers in list items (icon sits inside a 40×40 bg container with padding) |
| `icon-2xl` | 48×48 px | Large feature icons, empty state illustrations, prominent actions |

**The rule:** Always use the standard 24×24 as your default. Only deviate when there's a clear reason. Icons at 15px, 19px, 22px, or 37px are bugs — snap to the scale.

### Icon Container Pattern

Icons in lists, cards, and navigation often need a background container. The container is always **square** and the icon is centered inside it.

```
Container sizes and their icon sizes:

  32×32 container → 16×16 icon (50% ratio)
  40×40 container → 20×20 or 24×24 icon (50–60% ratio)
  48×48 container → 24×24 icon (50% ratio)
  56×56 container → 28×28 or 32×32 icon (50–57% ratio)

Structure in Figma:
  Icon Container (FRAME, auto layout, center-center, fixed W×H, fill color, corner radius)
  └── Icon (VECTOR or INSTANCE, fixed W×H)
```

**Rules for icon containers:**
- Container is ALWAYS square (width = height)
- Container uses auto layout with center alignment on both axes
- Icon is 50–60% of the container size
- Container border radius: use the radius scale token (usually `radius-sm` = 8px for square, `radius-full` for circle)
- Container fill comes from a color variable (not raw hex)

**Common anti-patterns (NEVER do these):**
- ❌ Text characters as icons ("◉", "★", "→", "✕") — use vector SVG icons from a proper icon set
- ❌ Non-square icon frames (40×19, 13×15) — always square
- ❌ Icon frame set to HUG height with text child — creates non-square dimensions
- ❌ Emoji as UI icons — renders differently per platform, not accessible
- ❌ Mixing icon libraries (Feather + Material + FA in the same app)
- ❌ Inconsistent stroke widths across the icon set

### Touch Targets for Icons

An icon may be 24×24, but the tappable area must be larger:

| Context | Minimum Touch Target | Implementation |
|---|---|---|
| Mobile — primary actions (nav, toolbar, FAB) | 44×44 px (iOS) / 48×48 dp (Android) | Icon frame with padding, or wrapping button frame |
| Mobile — secondary actions (inline, list trailing) | 40×40 px minimum | Padding around the icon |
| Mobile — dense areas (compact lists, data tables) | 32×32 px minimum | Acceptable if items are far apart |
| Web — mouse-primary interaction | 32×32 px minimum | 24×24 icon with 4px padding |
| Web — touch-capable (hybrid) | 44×44 px | Same as mobile |

**Implementation:** The icon itself stays at 24×24. The parent frame (the touch target) adds padding to reach the minimum. In Figma: create a frame with auto layout, center-center, set padding so total size = 44×44.

```
Touch Target Frame (44×44, auto layout, center-center, pad: 10)
└── Icon Vector (24×24, fixed)
```

### Choosing an Icon Set

For production-quality UI, use a proper vector icon library. Recommended sets:

| Icon Set | Style | Best For | Sizes |
|---|---|---|---|
| **Lucide** (Feather fork) | Stroke, 2px, rounded | Modern minimal apps | 24×24 base |
| **Phosphor** | Stroke + filled variants | Versatile, 6 weights | 16–32px |
| **Material Symbols** | Stroke, variable weight | Android / Material apps | 20–48px |
| **SF Symbols** | Apple system style | iOS apps | Dynamic sizing |
| **Heroicons** | Stroke (outline) + Solid | Tailwind/React projects | 20×20, 24×24 |
| **Tabler Icons** | Stroke, 2px | Clean web dashboards | 24×24 base |

**Rules for icon sets:**
1. Choose ONE icon library for the entire project — no mixing
2. Ensure the set has all icons you need (check: search, settings, back, close, plus, minus, check, error, info, warning, user, heart, star, filter, sort, share, edit, delete, home, notification, message, camera, location)
3. Use stroke (outline) icons for default state, solid/filled for active/selected state
4. Consistent stroke width across all icons (typically 1.5px or 2px)
5. Import as Figma components (not as individual vectors pasted in)

### Icon Figma Structure Rules

Every icon in Figma must follow this structure:

```
Icon / [Name] (COMPONENT, 24×24 fixed, auto layout center-center)
├── Vector (the actual icon path, constrained to content area)

For icon containers in list items:
Icon Container / [Name] (COMPONENT, 40×40 fixed, auto layout center-center, fill + radius)
└── Icon / [Name] (INSTANCE, 24×24 fixed)
```

**Non-negotiables:**
1. Icon frames are ALWAYS square (24×24, not 24×19 or 20×22)
2. Icon frames use auto layout with `CENTER` on both axes
3. Icon sizing is `FIXED` width and `FIXED` height (icons don't stretch)
4. Vector content stays within a 2px padding "safe zone" (20×20 content area in a 24×24 frame)
5. All icons are Figma components, organized under `Icon / [Name]`
6. Never use text characters ("★") as icons — always use vector paths

### Detection Script — Find Broken Icons

```javascript
// Via figma_execute — detect icon anti-patterns
function auditIcons(node, results = { textIcons: [], nonSquare: [], wrongSize: [], noComponent: [] }) {
  // Text characters used as icons (common anti-pattern)
  if (node.type === 'TEXT' && node.characters && node.characters.length <= 2) {
    const char = node.characters;
    const iconChars = ['◉', '★', '☆', '●', '○', '→', '←', '↑', '↓', '✕', '✓', '✗', '▶', '◀', '▼', '▲', '⚡', '♥', '☎', '✎', '⊕', '⊖'];
    if (iconChars.includes(char) || /[\u2600-\u27BF\u2B50-\u2B55]/.test(char)) {
      results.textIcons.push({
        id: node.id, char: char, parent: node.parent?.name,
        w: Math.round(node.width), h: Math.round(node.height)
      });
    }
  }
  // Non-square icon containers
  if (node.type === 'FRAME' && node.name.toLowerCase().includes('icon')) {
    const w = Math.round(node.width);
    const h = Math.round(node.height);
    if (w !== h && w <= 64 && h <= 64) {
      results.nonSquare.push({ id: node.id, name: node.name, w: w, h: h, parent: node.parent?.name });
    }
    // Not on the sizing scale
    const validSizes = [12, 16, 20, 24, 32, 40, 48, 56, 64];
    if (!validSizes.includes(w) || !validSizes.includes(h)) {
      results.wrongSize.push({ id: node.id, name: node.name, w: w, h: h, parent: node.parent?.name });
    }
  }
  if ('children' in node) {
    for (const child of node.children) auditIcons(child, results);
  }
  return results;
}
const r = auditIcons(figma.currentPage);
return { textIcons: r.textIcons.length, nonSquare: r.nonSquare.length, wrongSize: r.wrongSize.length, details: r };
```

---

## 21. Placeholder Images — Unsplash in Figma

When building screens in Figma, **never leave image areas empty or filled with solid gray**. Always insert a contextually relevant placeholder image using Unsplash. This makes designs feel real during reviews, testing, and handoff.

### How to Insert Placeholder Images

Use the **Unsplash Figma plugin** (built-in, free):

1. Select the rectangle or frame where the image should go
2. Open Unsplash: **Resources panel → Plugins → Unsplash** (or search "Unsplash" in Resources, shortcut `Shift + I`)
3. Search for a keyword relevant to the context (e.g. "trading cards", "headshot", "landscape", "food")
4. Click the image thumbnail — it fills the selected shape automatically

**If no shape is selected**, Unsplash drops the image onto the canvas at full size. Always select the target frame first.

### When to Use Placeholder Images

| Context | Search Keyword Examples | Notes |
|---|---|---|
| User avatars | "portrait", "headshot", "face" | Use diverse faces — vary age, ethnicity, gender |
| Product cards | Match the product type — "sneakers", "trading cards", "electronics" | Keep images consistent in style across a listing |
| Hero banners | "nature", "abstract", "cityscape" | Choose images that won't fight with overlay text |
| Background images | "texture", "gradient", "minimal background" | Low-contrast images work best behind UI |
| Empty state illustrations | Use icons or illustrations instead | Unsplash photos rarely work for empty states |
| Thumbnails | Match content type — "food", "travel", "gaming" | Crop tightly — the Figma fill mode handles this |

### Rules for Placeholder Images in Figma

1. **Always use image fill mode `FILL` (crop), not `FIT` (letterbox)** — images should fill their container without white bars
2. **Set the image on the frame/rectangle fill**, not as a child image node floating inside
3. **Maintain consistent aspect ratios** within a list or grid — if one card image is 16:9, all card images must be 16:9
4. **Use contextually appropriate images** — a food delivery app should show food, not random landscapes
5. **Vary the images** — don't use the same placeholder in every card. Search and pick 3–5 different images for a realistic feel
6. **For avatars**, ensure diverse representation — not all the same person or demographic
7. **Never ship Unsplash placeholders to production** — they are for design only. Mark in handoff notes that real content/API images will replace them

### Image Frame Structure in Figma

```
Card Frame (auto layout, vertical, gap: 0)
├── Image Frame (FILL width, FIXED height: 200, clip content: true)
│   └── fill: Image (Unsplash, fill mode: FILL, no stretch)
├── Content Frame (auto layout, vertical, pad: 16, gap: 8)
│   ├── Title Text
│   ├── Description Text
│   └── Action Button
```

**Key settings on the image frame:**
- `layoutSizingHorizontal`: `FILL` (stretches to card width)
- `layoutSizingVertical`: `FIXED` (explicit height — e.g., 200px, 180px, 240px depending on card size)
- `clipsContent`: `true` (crops overflow)
- Fill type: `IMAGE` with `scaleMode: FILL`

### What NOT to Do

- ❌ Leave image areas as empty gray rectangles — always insert a placeholder
- ❌ Use `FIT` scale mode — creates letterboxing with white/gray bars
- ❌ Paste raw images as child nodes inside frames — use image fills on the frame itself
- ❌ Use the same placeholder image in every card in a list
- ❌ Use random/unrelated images (stock photo of a sunset in a fintech app)
- ❌ Use images with embedded text that competes with your UI text
- ❌ Forget to note in handoff that images are placeholders

---

## 22. Icon Source — The Project Icons Page

When building screens in Figma, **always pull icons from the project's designated icon page** in the same Figma file. This is the single source of truth for all icons used in the project.

### Determining the Icon Page Name

1. **Check `project/APP_PLAN.md`** for the `branding.icon_page_name` field
2. If set → use that page name (e.g., "UI Icons", "Assets/Icons", "Iconography")
3. If NOT set → **ask the user**: "Which Figma page contains your project's icons?" Store the answer in APP_PLAN.md's `branding.icon_page_name` field before proceeding.
4. **Never assume "UI Icons"** — always check APP_PLAN.md first

### How It Works

The Figma file has a dedicated page (named per `icon_page_name` in APP_PLAN.md) that contains all icon components for the project. When you need an icon:

1. **Find the icon on the designated icon page (per APP_PLAN.md)** — icons are organized as components, named `Icon / [Name]` (e.g., `Icon / Search`, `Icon / Close`, `Icon / Home`)
2. **Create an instance** of the icon component on your design page — drag it from the Assets panel or use `figma_instantiate_component` via MCP
3. **Never copy-paste raw vectors** — always use component instances so they stay linked and update globally

### Via Figma MCP (Programmatic)

When Claude is building screens via MCP, use this workflow:

```
Step 1: Find the icon component on the designated icon page
─────────────────────────────────────────────────────
Use figma_execute to search the designated icon page for the component by name:

  // Use the icon_page_name from APP_PLAN.md (default: "UI Icons")
  const ICON_PAGE_NAME = "{{icon_page_name}}"; // Replace with actual value from APP_PLAN.md
  const iconsPage = figma.root.children.find(p => p.name === ICON_PAGE_NAME);
  if (!iconsPage) return { error: `No '${ICON_PAGE_NAME}' page found. Check branding.icon_page_name in APP_PLAN.md` };

  await figma.loadAllPagesAsync();

  // Find a specific icon by name (e.g., "search", "close", "home")
  function findIcon(page, iconName) {
    const results = [];
    function walk(node) {
      if (node.type === "COMPONENT" && node.name.toLowerCase().includes(iconName.toLowerCase())) {
        results.push({ id: node.id, name: node.name, w: Math.round(node.width), h: Math.round(node.height) });
      }
      if ("children" in node) node.children.forEach(walk);
    }
    walk(page);
    return results;
  }

  return findIcon(iconsPage, "search");

Step 2: Instantiate the icon on the target page
────────────────────────────────────────────────
Use figma_instantiate_component with the component ID from Step 1.

Step 3: Position and resize the instance
────────────────────────────────────────
Place the icon instance inside the target frame using figma_move_node,
then resize if needed using figma_resize_node (default: keep at 24×24).
```

### Listing All Available Icons

To see every icon available in the file:

```javascript
// Via figma_execute — list all icon components on the designated icon page
// Use the icon_page_name from APP_PLAN.md (default: "UI Icons")
const ICON_PAGE_NAME = "{{icon_page_name}}"; // Replace with actual value from APP_PLAN.md
const iconsPage = figma.root.children.find(p => p.name === ICON_PAGE_NAME);
if (!iconsPage) return { error: `No '${ICON_PAGE_NAME}' page found. Check branding.icon_page_name in APP_PLAN.md` };

await figma.loadAllPagesAsync();

const icons = [];
function walk(node) {
  if (node.type === "COMPONENT") {
    icons.push({ id: node.id, name: node.name, w: Math.round(node.width), h: Math.round(node.height) });
  }
  if ("children" in node) node.children.forEach(walk);
}
walk(iconsPage);

return { total: icons.length, icons: icons.slice(0, 50) };
```

### Rules for Using Icons from the Project Icon Page

1. **Always use instances, never detach** — detached icons don't update when the source component changes
2. **If an icon doesn't exist on the designated icon page**, tell the user. Do not substitute a text character or emoji. Ask if they want to create the missing icon or use an alternative.
3. **Match the icon size to the sizing scale** (see §20 Icon Sizing Scale) — default is 24×24
4. **Respect fill color** — icon instances should inherit color from the parent or use a color variable. Don't hard-code hex values on individual icons.
5. **The designated icon page (per APP_PLAN.md) is the only icon source** — never import icons from external libraries, URLs, or other Figma files unless the user explicitly asks

### What NOT to Do

- ❌ Copy-paste raw SVG vectors instead of using component instances
- ❌ Create new one-off icon components on the design page — add them to the icon page first
- ❌ Use text characters ("✕", "★", "→") as icon substitutes
- ❌ Pull icons from a different page or external source without asking
- ❌ Detach icon instances to "customize" them — instead, add a new variant on the icon page

---

## 23. Border Radius System

Border radius is one of the most visible design tokens. Inconsistent radii make an interface look unfinished. This section defines a systematic approach.

### The Border Radius Scale

Use a fixed scale of radius values. Every rounded corner in the project must reference one of these tokens.

| Token Name | Value | Use Case |
|---|---|---|
| `radius-none` | 0 px | Sharp corners: dividers, table cells, full-bleed sections |
| `radius-xs` | 2 px | Subtle rounding: progress bars, thin UI elements, code blocks |
| `radius-sm` | 4 px | Small elements: tags, badges, chips, tooltips, small buttons |
| `radius-md` | 8 px | **Default — use for most elements.** Inputs, standard buttons, cards, dropdowns, icon containers |
| `radius-lg` | 12 px | Larger elements: modal headers, image thumbnails, large cards |
| `radius-xl` | 16 px | Prominent containers: bottom sheets, dialogs, floating cards, search bars |
| `radius-2xl` | 24 px | Large containers: full-screen sheets, page-level cards |
| `radius-full` | 9999 px | Pill shapes: pill buttons, avatars, circular icon containers, tags with rounded ends |

**The rule:** Pick your project's "personality radius" (`radius-md` = 8px is safe modern default) and derive everything from the scale. Never use arbitrary values like 5px, 7px, 14px, or 18px.

### Radius Assignment by Element Type

| Element Category | Recommended Token | Typical Value | Rationale |
|---|---|---|---|
| **Buttons — standard** | `radius-md` | 8 px | Friendly, modern, not too round |
| **Buttons — pill** | `radius-full` | 9999 px | Used for CTAs that need to pop |
| **Input fields** | `radius-md` | 8 px | Match button radius for visual consistency |
| **Cards** | `radius-lg` or `radius-xl` | 12–16 px | Larger elements get proportionally larger radius |
| **Bottom sheets** | `radius-xl` or `radius-2xl` | 16–24 px (top corners only) | Only top-left and top-right are rounded |
| **Modals / Dialogs** | `radius-xl` | 16 px | Prominent floating containers |
| **Dropdowns / Menus** | `radius-md` | 8 px | Match input radius (they appear near inputs) |
| **Tags / Chips** | `radius-sm` or `radius-full` | 4 px or pill | Depends on brand style |
| **Avatars** | `radius-full` | 9999 px (circle) | Industry standard |
| **Icon containers** | `radius-sm` or `radius-full` | 4–8 px or circle | Square with rounded corners or full circle |
| **Images / Thumbnails** | `radius-md` or `radius-lg` | 8–12 px | Softens edges without distorting content |
| **Tooltips** | `radius-sm` | 4 px | Small and precise |
| **Switches / Toggles** | `radius-full` | 9999 px | Always pill-shaped tracks |
| **Progress bars** | `radius-xs` or `radius-full` | 2 px or pill | Thin bars round subtly or fully |
| **Navigation bar** | `radius-none` | 0 px | Full-width bars don't need rounding |
| **Dividers** | `radius-none` | 0 px | Never rounded |

### The Nesting Rule (Inner Radius Formula)

When a rounded element is nested inside another rounded element, the inner radius must be smaller to look optically correct.

```
Inner radius = Outer radius − Gap between them

Example:
  Card (radius: 16px, padding: 12px)
  └── Button inside card → radius: 16 − 12 = 4px (use radius-sm)

  Bottom Sheet (radius: 24px, padding: 16px)  
  └── Input inside sheet → radius: 24 − 16 = 8px (use radius-md)

  Modal (radius: 16px, padding: 24px)
  └── Card inside modal → radius: max(16 − 24, 0) = 0px or radius-sm (4px) for visual softness
```

**Rule:** If the computed inner radius is ≤ 0, use either `radius-none` (0px) or `radius-xs` (2px) for a subtle softness. Never use the SAME radius on parent and child — it looks off.

### Platform-Specific Radius Conventions

| Platform | Default Personality | Common Values | Notes |
|---|---|---|---|
| **iOS (HIG)** | Rounded, soft | 10–13px (system), up to 20px for cards | Apple's "continuous corner" (squircle) — Figma's "Smooth corners" approximates this |
| **Android (Material 3)** | Mixed, expressive | 8–28px depending on element class | Material uses "shape tokens" (Extra Small → Extra Large) |
| **Web — SaaS/Dashboard** | Subtle, professional | 4–8px | Too much rounding feels unserious |
| **Web — Consumer/Social** | Friendly, rounded | 12–16px | More rounded = more approachable |
| **Web — Enterprise** | Sharp or minimal | 0–4px | Corporate, serious, dense |

**Tip:** In Figma, enable "Smooth corners" (the iOS squircle toggle in the corner radius inspector) for any element with `radius-lg` (12px) or higher. It produces a more natural curve that matches Apple's design language.

### Common Anti-Patterns

- ❌ **Random radii** — 5px on one button, 12px on another, 8px on a third
- ❌ **Same radius everywhere** — 16px on a tiny tag AND a full-screen sheet
- ❌ **Inner radius = outer radius** on nested elements (looks bloated)
- ❌ **Radius larger than half the element height** — A 32px-tall element with 20px radius looks like a pill when it shouldn't
- ❌ **Radius on only some corners inconsistently** — unless it's a bottom sheet (top only) or table cells (specific corners)
- ❌ **Raw values instead of tokens** — every radius must reference a variable

### Detection Script — Find Inconsistent Radii

```javascript
// Via figma_execute — audit border radius usage
function auditRadii(node, results = {}) {
  const r = node.cornerRadius;
  if (typeof r === 'number' && r > 0 && node.type !== 'ELLIPSE') {
    const rounded = Math.round(r);
    if (!results[rounded]) results[rounded] = [];
    if (results[rounded].length < 5) {
      results[rounded].push({
        name: node.name,
        size: `${Math.round(node.width)}×${Math.round(node.height)}`,
        parent: node.parent?.name
      });
    }
  }
  if ('children' in node) {
    for (const child of node.children) auditRadii(child, results);
  }
  return results;
}

const radii = auditRadii(figma.currentPage);
const validScale = [0, 2, 4, 8, 12, 16, 24, 9999];
const offScale = {};
for (const [r, items] of Object.entries(radii)) {
  if (!validScale.includes(Number(r))) {
    offScale[r + 'px (OFF SCALE)'] = items;
  }
}
return { allRadiiUsed: Object.keys(radii).map(r => r + 'px'), offScaleValues: offScale, full: radii };
```

---

## 24. Data Flow Design

User flows show where the user goes. Data flows show what information travels with them, where it comes from, and what happens when it's missing. Without data flow documentation, developers guess at API contracts, designers forget loading states, and users hit dead ends. This section makes data flow a first-class design concern.

### Why Designers Must Own Data Flow

Designers aren't just pushing pixels — they're implicitly designing data contracts every time they put content on a screen. If a screen shows a user's name, avatar, rating, and last-active timestamp, the designer has defined a data requirement. If the design doesn't account for what happens when that data is missing, slow, stale, or wrong, the implementation will be broken.

### Screen Data Dependency Map

For every screen in the project, document what data it needs, where that data comes from, and what triggers a refresh.

**Format — one row per screen:**

| Screen | Data Needed | Source | Trigger | Depends On |
|---|---|---|---|---|
| Home | User profile (name, avatar), Recent places list, Current location | GET /user/profile, GET /places/recent, Device GPS | Screen mount, Pull-to-refresh | Auth token |
| Set Destination | Saved places, Search results, Recent places | Local cache, GET /places/search?q=, GET /places/recent | Screen mount (saved/recent), Keystroke debounce (search) | Auth token |
| Confirm Ride | Route (origin, dest, polyline), Fare estimate, Driver ETA | GET /rides/estimate | Screen mount from destination selection | Selected origin + destination |
| Finding Driver | Ride request status, Driver location (live) | POST /rides/request, WS /rides/{id}/status | On enter (POST), WebSocket stream | Ride ID from POST response |
| Trip Complete | Trip summary, Fare charged, Driver info, Rating | GET /rides/{id}/summary | Screen mount | Ride ID |

**Rules:**
1. Every screen gets a row — no exceptions, even simple screens
2. "Source" must be specific: API endpoint, local storage key, device sensor, or hardcoded
3. "Trigger" tells the developer WHEN to fetch: on mount, on interaction, on timer, on WebSocket event
4. "Depends On" identifies prerequisite data — this exposes hidden dependencies between screens

### Data States Per Screen

Every piece of data on every screen can be in one of these states. The design must account for ALL of them.

**The Data State Machine:**

```
                    ┌─────────┐
       ┌───────────→│ Loading │──── Success ────→ ┌──────────┐
       │            └─────────┘                   │ Populated│
       │                 │                        └────┬─────┘
       │            Failure                       Refresh│Stale
       │                 │                             │
       │                 ▼                             ▼
  ┌─────────┐     ┌───────────┐              ┌──────────────┐
  │  Empty   │     │   Error   │              │ Refreshing   │
  │(no data) │     │ (failed)  │              │(stale + load)│
  └─────────┘     └───────────┘              └──────────────┘
```

| State | UI Treatment | Design Required? |
|---|---|---|
| **Empty** — no data exists yet (new user, empty list, no history) | Illustration + message + CTA ("Add your first item") | ✅ Always |
| **Loading** — data is being fetched for the first time | Skeleton screen matching content shape, or spinner for actions | ✅ Always |
| **Populated** — data present, happy path | The default design | ✅ Always (this is what you design first) |
| **Refreshing** — showing stale data while fresh data loads | Show existing content + subtle refresh indicator (pull-to-refresh spinner, top progress bar) | ✅ If data can go stale |
| **Error** — fetch failed (network, server, auth) | Error message + retry button + fallback to cached data if available | ✅ Always |
| **Partial** — some data loaded, some failed | Show what loaded, inline error for what failed, retry per section | ⚠️ If screen has multiple independent data sources |
| **Stale** — data loaded but may be outdated | Show data with "Last updated X ago" label, auto-refresh or manual refresh | ⚠️ If data changes frequently |
| **Offline** — no network connection | Show cached data + offline banner, or offline screen with retry | ⚠️ If app should work offline |
| **Permission denied** — user lacks access | Lock screen with explanation + upgrade/request CTA | ⚠️ If features are gated |
| **Rate limited / Throttled** — too many requests | "Please wait" message with cooldown timer | ⚠️ For high-frequency actions |

### Data Flow Between Screens

When the user navigates from Screen A to Screen B, data must travel with them. Document how:

**Three data passing patterns:**

| Pattern | How It Works | When to Use | Design Implication |
|---|---|---|---|
| **Pass by parameter** | Screen A sends an ID; Screen B fetches full data using that ID | Detail screens (tap list item → fetch full item by ID) | Screen B needs its own loading state — it doesn't have the data yet |
| **Pass by payload** | Screen A sends the full data object to Screen B | Quick previews, confirmations (cart → checkout already has items) | Screen B can render immediately — no loading state needed for passed data |
| **Shared state** | Both screens read from the same global store (e.g., Redux, Zustand, context) | User profile, cart, auth status — used across many screens | Changes on one screen should reflect on all others without re-navigation |

**Document each transition:**

```
Screen A → Screen B:
  Passes: { rideId: "abc123" }
  Pattern: Pass by parameter
  Screen B fetches: GET /rides/abc123
  Loading: Show skeleton for 1-2s
  Error: "Couldn't load ride details" + Retry

Screen A → Screen B:
  Passes: { item: { name, price, image, description } }
  Pattern: Pass by payload
  Screen B renders: Immediately, no loading
  Missing fields: If image is null → show placeholder
```

### Multi-Step Form Data Flow

For flows with multiple input screens (onboarding, checkout, booking), document how form data persists across steps:

| Concern | Rule | Design Impact |
|---|---|---|
| **Draft persistence** | Save form data locally after each step so the user can return | If the user navigates back, their input is preserved — never clear without confirmation |
| **Step validation** | Validate each step before allowing navigation to the next | Show inline errors on the current step; disable "Next" until valid |
| **Summary before submit** | Show all collected data on a confirmation screen before the final action | Editable summary: tap any section to jump back to that step |
| **Partial submission** | What if the app crashes mid-flow? | Auto-save draft to local storage; on return, offer "Continue where you left off?" |
| **Back navigation** | User taps Back in step 3 — does step 2 still have their data? | Always preserve. Never reset a form step on back navigation. |
| **Cross-step dependencies** | Step 3 options depend on Step 1 choices (e.g., address determines available shipping methods) | Show loading in Step 3 if it needs to re-fetch based on Step 1 changes |

### Real-Time Data & Optimistic Updates

For screens with live data (chat, tracking, notifications, collaborative editing):

| Pattern | What It Means | Design Requirement |
|---|---|---|
| **Polling** | Client asks server for updates on a timer (every 5s, 30s) | Show "Last updated" timestamp, refresh indicator on poll |
| **WebSocket / SSE** | Server pushes updates to client in real time | Animate new data in (slide, fade), don't jump the scroll position |
| **Optimistic update** | UI shows the change immediately, rolls back if server rejects | Show the action as done instantly; if it fails, revert + show error toast |
| **Pessimistic update** | UI waits for server confirmation before showing the change | Show loading state on the element (button spinner, skeleton) until confirmed |

**When to use which:**
- **Optimistic** → Low-risk actions: liking a post, toggling a setting, adding to favorites
- **Pessimistic** → High-risk actions: payments, deletions, sending messages, booking confirmations

**Design for rollback:** If an optimistic update fails, the UI must gracefully revert. Design the "undo" or "error" state for every optimistic action.

### Data Annotation System for Figma

Use these annotation conventions to document data flow directly on design frames:

**Annotation format for data-dependent elements:**

```
[DATA] Element name
Source: GET /endpoint or "local cache" or "passed from previous screen"
Refresh: On mount | Pull-to-refresh | Every 30s | WebSocket
Empty: "No items yet" (see frame SCREEN-01-Empty)
Error: "Couldn't load" + Retry (see frame SCREEN-01-Error)
Loading: Skeleton (see frame SCREEN-01-Loading)
```

**Color coding for data annotations (add to the annotation category system in §2B):**

| Category | Color | Use For |
|---|---|---|
| **Data Source** | 🟠 Orange | API endpoint, local storage, device sensor, passed parameter |
| **Data State** | 🟤 Brown | Loading/empty/error/stale/offline state references |

### Data Flow Diagram in Figma

Create a dedicated data flow diagram on the User Flows page showing how data moves between screens:

```
Data Flow Page Layout:

┌──────────────────────────────────────────────────────────────┐
│ SECTION: Data Flow — [Feature Name]                           │
├──────────────────────────────────────────────────────────────┤
│                                                                │
│  ┌──────────┐   rideId    ┌──────────────┐  WebSocket   ┌───────────┐
│  │  Confirm  │───────────→│  Finding     │─────────────→│ En Route  │
│  │  Ride     │            │  Driver      │              │           │
│  └──────────┘            └──────────────┘              └───────────┘
│       │                        │                            │
│       │ POST /rides/request    │ WS /rides/{id}/status      │ GET /rides/{id}
│       ▼                        ▼                            ▼
│  ┌──────────┐            ┌──────────────┐              ┌───────────┐
│  │  Server  │            │   Server     │              │  Server   │
│  └──────────┘            └──────────────┘              └───────────┘
│                                                                │
│  Legend:                                                       │
│  ──→ Data passed between screens (label = what's passed)      │
│  ──▼ API call (label = method + endpoint)                     │
│  🟠 Real-time connection                                      │
└──────────────────────────────────────────────────────────────┘
```

### Data Flow Audit Checklist

Run this checklist for every screen and every flow:

| # | Check | ✓ |
|---|-------|---|
| 1 | Screen Data Dependency Map exists — every screen has source, trigger, dependencies documented | |
| 2 | Every data-dependent element has all states designed: empty, loading, populated, error | |
| 3 | Refreshing/stale state designed for any data that can change while the screen is visible | |
| 4 | Data passing pattern documented for every screen-to-screen transition (parameter, payload, or shared state) | |
| 5 | Multi-step forms preserve data on back navigation — no silent data loss | |
| 6 | Multi-step forms have a summary/confirmation screen before final submission | |
| 7 | Real-time data screens show connection status (connected/reconnecting/disconnected) | |
| 8 | Optimistic updates have rollback/error states designed | |
| 9 | Offline behavior documented: what's cached, what shows "offline" banner, what blocks entirely | |
| 10 | No screen assumes data will always be present — every nullable field has a fallback | |
| 11 | Pagination/infinite scroll documented for all list screens (type, page size, "load more" UX) | |
| 12 | Data annotations added to Figma frames using [DATA] format from §22 | |
| 13 | Data flow diagram exists on the User Flows page showing inter-screen data movement | |
| 14 | API error codes mapped to user-facing messages (400 → validation, 401 → re-login, 403 → locked, 500 → retry) | |

### Data Flow Anti-Patterns

- ❌ **Designing only the populated state** — every screen needs empty, loading, and error at minimum
- ❌ **Assuming instant data** — no loading states between screens that require API calls
- ❌ **Silent data loss** — user fills a form, navigates back, returns, and their input is gone
- ❌ **God screen** — one screen that fetches from 6+ independent API calls (split into sections with independent loading)
- ❌ **No offline story** — app shows a blank white screen when there's no network
- ❌ **Stale data without indication** — showing 2-hour-old data with no "last updated" signal
- ❌ **Optimistic everything** — using optimistic updates for payments or irreversible actions
- ❌ **Pagination afterthought** — list shows 10 items in the design but the API returns 500; no scroll, no "load more"
- ❌ **Undocumented dependencies** — Screen B requires data from Screen A, but neither screen documents this; developer discovers it at build time
- ❌ **No error recovery** — error state says "Something went wrong" with no retry button or alternative path

---

## 25. Spacing & Sizing Enforcement

Inconsistent padding and broken fixed heights are the two most common reasons a design looks "off." This section defines strict, auditable rules for internal spacing (padding), external spacing (gaps), and sizing modes (HUG vs FIXED vs FILL) so that every element is predictable and nothing breaks when content changes.

### Spacing Token Scale

All spacing values MUST come from this scale. No exceptions.

| Token | Value | Common Use |
|---|---|---|
| `spacing-2xs` | 2px | Hairline gaps (icon-to-badge) |
| `spacing-xs` | 4px | Tight element gaps (label to input, icon to text) |
| `spacing-sm` | 8px | Compact element gaps (items in a dense list) |
| `spacing-md` | 12px | Default element gap (form fields, list items) |
| `spacing-content` | 16px | Standard content padding, card padding, screen margins |
| `spacing-lg` | 20px | Comfortable section gaps |
| `spacing-xl` | 24px | Section dividers, larger card padding |
| `spacing-2xl` | 32px | Major section separators |
| `spacing-3xl` | 48px | Page-level separators |
| `spacing-4xl` | 64px | Hero/header spacing |

**The rule:** If a spacing value doesn't appear in this table, it's wrong. 13px, 17px, 22px, 15px — all wrong. Snap to the nearest token.

### Element-Level Minimum Padding Rules

Every interactive or container element has a **minimum internal padding** that must never be violated. Zero-padding on a button or input is always a bug.

| Element | Min Horizontal Padding (left + right) | Min Vertical Padding (top + bottom) | Recommended | Notes |
|---|---|---|---|---|
| **Button (primary, secondary)** | 16px each side | 12px each side | H: 24px, V: 14px | Text must never touch the button edge. If icon-only, use equal padding all sides. |
| **Button (small)** | 12px each side | 8px each side | H: 16px, V: 10px | Even small buttons need breathing room |
| **Button (icon-only)** | 8px all sides | 8px all sides | 12px all sides | Frame should be square (40×40 or 48×48) |
| **Text input / Select** | 12px each side | 12px each side | H: 16px, V: 14px | Placeholder text must not touch edges |
| **Textarea** | 12px each side | 12px top/bottom | H: 16px, V: 14px | Same as input but allows vertical scroll |
| **Chip / Tag / Badge** | 8px each side | 4px each side | H: 12px, V: 6px | Compact but readable |
| **Card** | 12px each side | 12px top/bottom | 16px all sides | Content inside a card must always have padding |
| **List item** | 16px each side | 12px each side | H: 16px, V: 14px | Full-width tap target, content doesn't touch screen edges |
| **Tab bar item** | 12px each side | 8px each side | H: 16px, V: 12px | Tap target at least 44px total height |
| **Modal / Dialog** | 20px each side | 20px top/bottom | 24px all sides | More padding than cards — modals need room |
| **Bottom sheet** | 16px each side | 16px top, 24px bottom | H: 16px, V: 20px/32px | Extra bottom padding for home indicator |
| **Toast / Snackbar** | 16px each side | 12px each side | H: 16px, V: 14px | Brief messages still need padding |
| **Dropdown menu** | 12px each side (per item) | 8px each side (per item) | H: 16px, V: 12px | Each menu item has its own padding |
| **Navigation bar** | 16px each side | 12px top, safe-area bottom | H: 16px | Bottom nav must include safe area inset |
| **Screen frame** | 16px each side (content area) | Status bar top, safe area bottom | H: 16px, V: varies | Screen margins set the content boundary |
| **Table cell** | 8px each side | 8px each side | H: 12px, V: 10px | Dense but scannable |
| **Tooltip** | 8px each side | 6px each side | H: 12px, V: 8px | Tight but text doesn't touch edges |
| **Section header** | 16px each side | 12px top, 8px bottom | matches screen margins | Must align with content below |

**🚫 Zero-Padding Violations:**

If ANY of these elements has `0px` horizontal or vertical padding, it is a **🔴 Critical** bug:
- Buttons, inputs, chips, cards, list items, modals, bottom sheets, toasts, dropdowns, tooltips

If the screen frame's content area has `0px` left or right padding, it's a **🔴 Critical** bug — content will touch the device edge.

### Sizing Mode Rules (HUG vs FIXED vs FILL)

The wrong sizing mode is why UI breaks when content changes. These rules are non-negotiable:

**MUST be HUG (content-sized):**

| Element | Why |
|---|---|
| Buttons | Height must grow if text wraps (rare) or if font changes. Width hugs text + padding. |
| Text containers | Text length varies. Fixed height clips or wastes space. |
| Cards | Content inside varies (different descriptions, different numbers of tags). Height must adapt. |
| List items | Content varies per item (some have subtitles, some don't). |
| Chips / Tags / Badges | Text length varies ("Sale" vs "Limited Time Offer"). |
| Form field wrappers | Field + label + helper text + error — total height varies by state. |
| Modals / Dialogs | Content varies (short confirmation vs long terms). |
| Bottom sheets | Content varies. Use maxHeight constraint if needed, not fixed height. |
| Accordion items | Expanded vs collapsed changes height. |
| Toast / Snackbar | Message length varies. |
| Section containers | Number of children varies. |
| Nav items | Label length varies. Width should hug. |

**MUST be FIXED:**

| Element | Fixed Dimension | Why |
|---|---|---|
| Screen frame | Width (device) + Height (device) | Matches the device viewport exactly |
| Images / Thumbnails | Width + Height | Aspect ratio must be preserved. Use `fill` or `fit`. |
| Icons | Width + Height | Always square, always on the icon scale (24×24, etc.) |
| Avatars | Width + Height | Always square or circular, fixed size (32/40/48/56px) |
| Dividers / Separators | Height (1px or 2px) | Width fills parent. Height is always 1px. |
| Progress bars | Height | Fixed track height (4–8px). Width fills parent. |
| Input fields | Height only (48px standard) | Width FILLS parent. Height is standardized across the app. |
| Switches / Toggles | Width + Height | Fixed dimensions per spec. |
| Loading spinners | Width + Height | Fixed size, centered in container. |

**MUST be FILL (stretch to parent):**

| Element | Fill Dimension | Why |
|---|---|---|
| Content area (between header and footer) | Height | Must fill the remaining vertical space on screen |
| Input fields | Width | Must stretch to the form column width |
| Full-width buttons | Width | CTA buttons at bottom should span the full content width |
| Card in a grid | Width | Must fill the grid column |
| List items | Width | Must span the full list width |
| Image in a card | Width | Must fill the card width (height auto from aspect ratio OR fixed crop) |

**🚫 Broken Sizing Anti-Patterns:**

| Anti-Pattern | What Happens | Fix |
|---|---|---|
| **Button at fixed height (e.g., 100px)** | Massive empty space above/below the text. Looks broken. | Set to HUG. The button height = text line-height + vertical padding. |
| **Card at fixed height (e.g., 200px)** | Short content = wasted space. Long content = clipped text. | Set to HUG. Card grows/shrinks with its content. |
| **Text container at fixed height** | Text gets cut off or container is 80% empty. | Set to HUG. Text containers must always grow with their content. |
| **List item at fixed height (e.g., 100px)** | Every item same height regardless of content. Items with one line have huge empty gaps. Items with three lines get clipped. | Set to HUG. Each item sizes to its own content. |
| **Content area at HUG instead of FILL** | Footer floats up to middle of screen when content is short. | Content area MUST be FILL so it pushes footer down. |
| **Input at HUG height** | Height varies per input based on font rendering. Inputs look ragged. | Set to FIXED height (48px). All inputs same height. |
| **Screen margin at 0px** | Content touches the device edge. Looks broken on every device. | Set screen content frame padding: 16px left, 16px right. |

### Spacing Between Elements (Gap Rules)

Within auto layout containers, the `gap` (itemSpacing) should follow these patterns:

| Container Type | Recommended Gap | Token |
|---|---|---|
| Form fields (vertical stack) | 16px–24px | `spacing-content` to `spacing-xl` |
| List items | 0px (use dividers) OR 8px–12px (cardless list) | `spacing-sm` to `spacing-md` |
| Button group (horizontal) | 8px–12px | `spacing-sm` to `spacing-md` |
| Card grid | 12px–16px | `spacing-md` to `spacing-content` |
| Section to section | 24px–32px | `spacing-xl` to `spacing-2xl` |
| Icon to text (inline) | 4px–8px | `spacing-xs` to `spacing-sm` |
| Label to input | 4px–8px | `spacing-xs` to `spacing-sm` |
| Input to helper/error text | 4px | `spacing-xs` |
| Header to content | 12px–16px | `spacing-md` to `spacing-content` |
| Content to footer | 16px | `spacing-content` |
| Tab bar items (horizontal) | 0px (divide width equally) | Equal distribution |
| Chip group (horizontal) | 8px | `spacing-sm` |
| Chip group (wrap) | 8px horizontal, 8px vertical | `spacing-sm` both |

### Spacing Consistency Rules

These are non-negotiable consistency requirements:

1. **Same element type = same padding everywhere.** If Button A has 24px horizontal padding, every button of the same size variant has 24px. No exceptions.
2. **Same container type = same gap everywhere.** If card grid A uses 16px gap, every card grid uses 16px gap.
3. **Screen margins are identical on every screen.** If Home has 16px side margins, Settings has 16px. A screen with 12px side margins next to a screen with 16px is a bug.
4. **Nested padding doesn't double up.** A card with 16px padding inside a screen with 16px margin = 32px visual distance from edge. This is correct and intentional — don't add more padding to "fix" it.
5. **Symmetry by default.** Left padding = right padding. Top padding = bottom padding (unless semantically different, like a bottom sheet with more bottom padding for the home indicator).
6. **Off-grid values are bugs.** Any spacing value not on the 8px grid (or 4px for fine adjustments) needs justification. 13px, 17px, 22px — snap to the nearest token.

### Spacing Detection Script

```javascript
// Via figma_execute — detect elements with zero or off-grid padding
const validSpacing = [0, 2, 4, 8, 12, 16, 20, 24, 32, 48, 64];
const page = figma.currentPage;
const issues = { zeroPadding: [], offGrid: [], fixedHeightShouldHug: [] };

// Element types that MUST NOT have zero padding
const mustHavePadding = ['button', 'btn', 'input', 'card', 'chip', 'tag', 'badge',
  'modal', 'dialog', 'sheet', 'toast', 'snackbar', 'dropdown', 'menu', 'tooltip',
  'list item', 'listitem', 'nav', 'tab'];

// Element types that MUST be HUG height (never fixed)
const mustHugHeight = ['button', 'btn', 'card', 'chip', 'tag', 'badge', 'modal',
  'dialog', 'sheet', 'toast', 'snackbar', 'accordion', 'section'];

function audit(node, depth = 0) {
  if (depth > 15) return;
  if (node.type === 'FRAME' && node.layoutMode && node.layoutMode !== 'NONE') {
    const name = node.name.toLowerCase();
    const pL = node.paddingLeft || 0;
    const pR = node.paddingRight || 0;
    const pT = node.paddingTop || 0;
    const pB = node.paddingBottom || 0;

    // Check zero padding on elements that need it
    const needsPadding = mustHavePadding.some(kw => name.includes(kw));
    if (needsPadding && (pL === 0 || pR === 0)) {
      issues.zeroPadding.push({
        id: node.id, name: node.name,
        padding: { L: pL, R: pR, T: pT, B: pB },
        issue: 'Zero horizontal padding on interactive/container element'
      });
    }
    if (needsPadding && (pT === 0 || pB === 0)) {
      issues.zeroPadding.push({
        id: node.id, name: node.name,
        padding: { L: pL, R: pR, T: pT, B: pB },
        issue: 'Zero vertical padding on interactive/container element'
      });
    }

    // Check off-grid padding values
    [pL, pR, pT, pB].forEach((val, i) => {
      if (val > 0 && !validSpacing.includes(val)) {
        const sides = ['paddingLeft', 'paddingRight', 'paddingTop', 'paddingBottom'];
        const nearest = validSpacing.reduce((a, b) => Math.abs(b - val) < Math.abs(a - val) ? b : a);
        issues.offGrid.push({
          id: node.id, name: node.name,
          property: sides[i], value: val,
          nearestToken: nearest
        });
      }
    });

    // Check off-grid gap
    const gap = node.itemSpacing || 0;
    if (gap > 0 && !validSpacing.includes(gap)) {
      const nearest = validSpacing.reduce((a, b) => Math.abs(b - gap) < Math.abs(a - gap) ? b : a);
      issues.offGrid.push({
        id: node.id, name: node.name,
        property: 'itemSpacing (gap)', value: gap,
        nearestToken: nearest
      });
    }

    // Check fixed height on elements that should HUG
    const shouldHug = mustHugHeight.some(kw => name.includes(kw));
    if (shouldHug && node.layoutSizingVertical === 'FIXED') {
      issues.fixedHeightShouldHug.push({
        id: node.id, name: node.name,
        currentHeight: Math.round(node.height),
        issue: 'Fixed height on element that should HUG content'
      });
    }
  }
  if ('children' in node) {
    for (const child of node.children) audit(child, depth + 1);
  }
}
audit(figma.currentPage);
return {
  zeroPaddingCount: issues.zeroPadding.length,
  offGridCount: issues.offGrid.length,
  fixedHeightCount: issues.fixedHeightShouldHug.length,
  zeroPadding: issues.zeroPadding.slice(0, 20),
  offGrid: issues.offGrid.slice(0, 20),
  fixedHeightShouldHug: issues.fixedHeightShouldHug.slice(0, 20)
};
```

### Spacing Anti-Patterns

- ❌ **Zero-padding button** — Text touches the button edge. Always add ≥16px horizontal, ≥12px vertical padding.
- ❌ **Zero-margin screen** — Content touches the device edge. Always add ≥16px left/right padding on the screen content frame.
- ❌ **Fixed-height card** — Short content wastes space, long content gets clipped. Cards must be HUG height.
- ❌ **Fixed-height button** — Button stuck at 100px with 14px text inside. Set to HUG. Height = text + vertical padding.
- ❌ **Off-grid spacing** — 13px gap, 17px padding, 22px margin. Snap to nearest token (12, 16, 24).
- ❌ **Doubled-up padding** — Card has 16px padding, its child section also has 16px padding. Inner content is 32px from the card edge. Remove inner section padding or reduce to 0.
- ❌ **Inconsistent screen margins** — Home screen has 16px margins, Profile screen has 24px. All screens must share the same margin value.
- ❌ **Gap + padding mismatch** — Section gap is 32px but items within sections also have 16px bottom margin, creating 48px visual gap between sections.
- ❌ **HUG on content area** — The scrollable content area between header and footer is HUG instead of FILL. Footer floats up when content is short.
- ❌ **FILL on a button** — Button stretches to full height of its parent instead of sizing to its content. Button should be HUG height.

---

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

---

## Quick Decision Framework

When stuck on a design decision, use this priority:

1. **Does it help the user complete their task faster?** → Do it.
2. **Does it reduce confusion or cognitive load?** → Do it.
3. **Does it follow patterns users already know?** → Do it.
4. **Does it work for users with disabilities?** → It must.
5. **Does it look modern and polished?** → Make it so.
6. **Is it innovative or novel?** → Only if 1–5 are satisfied first.

> **When in doubt, choose the boring, proven pattern over the clever, novel one.**
> Users don't come to your app to admire navigation design. They come to get something done.
