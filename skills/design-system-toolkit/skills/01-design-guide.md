---
name: design-guide-core
description: >
  Core UI/UX design principles for creating modern, user-friendly interfaces in Figma. Covers UX thinking,
  user flows, mobile-first strategy, layout, navigation, touch design, typography, color, forms,
  feedback states, edge cases, accessibility, and platform conventions (iOS/Android/Web).

  For reference material (review checklists, Figma workflow, iconography, spacing enforcement,
  modern patterns), load 01-ref-design-patterns.md on demand.

  IMPORTANT: Read project/APP_PLAN.md for project context first.
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

> **Reference material** (§14-26): Review checklists, Figma workflow, content writing, iconography,
> spacing enforcement, and modern design patterns are in `01-ref-design-patterns.md` — load on demand.

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
