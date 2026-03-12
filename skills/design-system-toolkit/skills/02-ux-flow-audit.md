---
name: ux-flow-validator
description: >
  Validates and audits user flows for usability, accessibility, and modern UX best practices. Use this skill
  whenever reviewing, critiquing, or improving a user flow, screen sequence, prototype, or interaction design.
  Trigger when the user says "check this flow", "review my UX", "is this user-friendly", "audit this design",
  "validate this flow", "improve this experience", "flow review", "UX feedback", "usability check", or shares
  screens/flows for review. Also trigger when the user finishes designing a flow and wants to verify quality
  before handoff. This skill runs a structured pass/fail audit, identifies friction points, and suggests
  concrete fixes grounded in the modern-ui-ux-design skill principles.
  
  IMPORTANT: Read project/APP_PLAN.md for project context and skills/01-design-guide.md for the design principles
  this audit references. If unavailable, the audit criteria below are self-contained.
---

# UX Flow Validator & Audit System

> **Purpose**: Systematically review any user flow to find friction, confusion, and usability failures
> before real users encounter them. Think of this as a "linter" for UX design.

---

## Table of Contents

1. [How to Run a Flow Audit](#1-how-to-run-a-flow-audit)
2. [Phase 1: Flow Structure Audit](#2-phase-1-flow-structure-audit)
3. [Phase 2: Screen-by-Screen Audit](#3-phase-2-screen-by-screen-audit)
4. [Phase 3: Mobile Usability Audit](#4-phase-3-mobile-usability-audit)
5. [Phase 4: Interaction & Feedback Audit](#5-phase-4-interaction--feedback-audit)
6. [Phase 5: Edge Case & State Coverage](#6-phase-5-edge-case--state-coverage)
7. [Phase 6: Accessibility Audit](#7-phase-6-accessibility-audit)
8. [Phase 7: Cognitive Load & Clarity](#8-phase-7-cognitive-load--clarity)
9. [Severity Rating System](#9-severity-rating-system)
10. [Fix Recommendation Framework](#10-fix-recommendation-framework)
11. [Flow Audit Report Template](#11-flow-audit-report-template)
12. [Common Flow Patterns & Benchmarks](#12-common-flow-patterns--benchmarks)
13. [Re-Validation After Fixes](#13-re-validation-after-fixes)
14. [User Research Integration](#14-user-research-integration)

---

## Nielsen's 10 Usability Heuristics — Quick Reference

Every finding in this audit should map to one or more of these heuristics. Industry-standard UX audits always cite which heuristic a finding violates — it gives findings universal credibility and makes reports understandable across teams.

| ID | Heuristic | What It Means | Audit Phases That Check It |
|----|-----------|---------------|---------------------------|
| **H1** | Visibility of System Status | The system should always keep users informed about what's going on through appropriate feedback within reasonable time. | Phase 4 (Interaction & Feedback), Phase 5 (Edge Cases) |
| **H2** | Match Between System and Real World | The system should speak the users' language, with words, phrases, and concepts familiar to the user rather than system-oriented terms. | Phase 7 (Cognitive Load), Phase 2 (Screen-by-Screen) |
| **H3** | User Control and Freedom | Users often perform actions by mistake. They need a clearly marked "emergency exit" to leave the unwanted action without having to go through an extended process. | Phase 1 (Flow Structure), Phase 2 (Screen-by-Screen) |
| **H4** | Consistency and Standards | Users should not have to wonder whether different words, situations, or actions mean the same thing. Follow platform and industry conventions. | Phase 2 (Screen-by-Screen), Phase 3 (Mobile Usability) |
| **H5** | Error Prevention | Even better than good error messages is a careful design that prevents problems from occurring in the first place. | Phase 5 (Edge Cases), Phase 2 (Screen-by-Screen) |
| **H6** | Recognition Rather Than Recall | Minimize the user's memory load by making elements, actions, and options visible. The user should not have to remember information from one part of the interface to another. | Phase 7 (Cognitive Load), Phase 2 (Screen-by-Screen) |
| **H7** | Flexibility and Efficiency of Use | Accelerators — unseen by the novice user — may speed up the interaction for the expert user so that the system can cater to both inexperienced and experienced users. | Phase 1 (Flow Structure), Phase 7 (Cognitive Load) |
| **H8** | Aesthetic and Minimalist Design | Interfaces should not contain information that is irrelevant or rarely needed. Every extra unit of information in an interface competes with the relevant units and diminishes their relative visibility. | Phase 7 (Cognitive Load), Phase 2 (Screen-by-Screen) |
| **H9** | Help Users Recognize, Diagnose, and Recover from Errors | Error messages should be expressed in plain language (no error codes), precisely indicate the problem, and constructively suggest a solution. | Phase 5 (Edge Cases), Phase 4 (Interaction & Feedback) |
| **H10** | Help and Documentation | Even though it is better if the system can be used without documentation, it may be necessary to provide help and documentation. Any such information should be easy to search, focused on the user's task, list concrete steps, and not be too large. | Phase 7 (Cognitive Load) |

### Using Heuristics in This Audit

When logging findings, always include the **Heuristic ID** in the fix template:

```
FINDING:    [What's wrong]
HEURISTIC:  [H1–H10 — which heuristic is violated]
SEVERITY:   [🔴 Critical / 🟠 Major / 🟡 Minor / 🔵 Suggestion]
...
```

This makes your audit reports speak the universal language of UX professionals worldwide.

---

## 1. How to Run a Flow Audit

### Input Required
To audit a flow, you need:
- The **screens / frames** in sequence (Figma frames, screenshots, or descriptions)
- The **user goal** — what is the user trying to accomplish?
- The **user persona** — who is doing this? (experience level, context, device)
- The **entry point** — how does the user arrive at the first screen?

### Audit Process
Run the audit in this order. Each phase catches different types of problems:

```
Phase 1: Flow Structure      → Is the journey logical and complete?
Phase 2: Screen-by-Screen    → Does each screen do its job?
Phase 3: Mobile Usability    → Does it work on a phone with one thumb?
Phase 4: Interaction Design  → Does the user get proper feedback?
Phase 5: Edge Cases & States → What happens when things go wrong?
Phase 6: Accessibility       → Can everyone use this?
Phase 7: Cognitive Load      → Is it mentally exhausting or effortless?
```

After each phase, log findings with a severity rating (Critical / Major / Minor / Suggestion).
Then compile into the audit report template at the end.

---

## 2. Phase 1: Flow Structure Audit

This phase checks the overall journey — the path from start to finish.

### Checklist

**Entry & Exit:**
| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 1.1 | Is there a clear entry point? (How does the user arrive here?) | | |
| 1.2 | Is the user's goal obvious within 5 seconds of entering the flow? | | |
| 1.3 | Is there a clear completion state? (How does the user know they're done?) | | |
| 1.4 | After completion, is the user guided to a logical next step? | | |
| 1.5 | Are there no dead-end screens? (Every screen leads somewhere) | | |

**Path Efficiency:**
| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 1.6 | Can the user complete the goal in 5 steps or fewer? | | |
| 1.7 | Is every step necessary? (No redundant or filler screens) | | |
| 1.8 | Could any two screens be combined without overloading? | | |
| 1.9 | Is pre-existing data reused? (No re-entering known information) | | |
| 1.10 | Are smart defaults provided where applicable? | | |

**Escape & Recovery:**
| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 1.11 | Can the user go back at every step? | | |
| 1.12 | Can the user cancel/exit the flow at any point? | | |
| 1.13 | If they exit mid-flow, is progress saved? (For long flows) | | |
| 1.14 | Is there an undo for destructive actions? | | |
| 1.15 | Do confirmation screens exist before irreversible actions? | | |

### Red Flags to Watch For
- **Step count > 5** for a common task → Look for shortcuts or screen consolidation
- **Linear-only flow** with no way to jump to a specific step → Add step navigation for long flows
- **No back button** on any screen → Critical usability failure
- **Dead ends** where the user has to start over → Always provide a next action
- **Data re-entry** — asking for info the system already has → Pre-fill automatically

### How to Fix Structure Issues
- Map the flow as boxes and arrows. If the diagram looks complex, the flow IS complex
- Apply the "3-tap rule" — can the user reach their goal in 3 taps from entry? If not, why?
- Ask: "What if I removed this screen entirely?" If nothing breaks, remove it
- For multi-step flows: Can any steps run in parallel or be deferred to later?

---

## 3. Phase 2: Screen-by-Screen Audit

Review each individual screen in the flow against these criteria.

### For EVERY Screen, Check:

**Clarity:**
| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 2.1 | Is the screen's purpose immediately obvious? | | |
| 2.2 | Is there ONE clear primary action? | | |
| 2.3 | Is the primary action visually the most prominent element? | | |
| 2.4 | Can the user tell where they are in the flow? (Progress indicator, title, breadcrumb) | | |
| 2.5 | Is there a clear page/screen title? | | |

**Visual Hierarchy:**
| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 2.6 | Is there a clear visual hierarchy? (Primary → Secondary → Tertiary) | | |
| 2.7 | Is the most important content/action above the fold (visible without scrolling)? | | |
| 2.8 | Is content scannable? (Not a wall of text) | | |
| 2.9 | Are related items visually grouped together? | | |
| 2.10 | Is there adequate whitespace? (Not cramped, not too sparse) | | |

**Consistency:**
| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 2.11 | Does this screen use the same navigation pattern as the rest of the app? | | |
| 2.12 | Are button styles consistent with other screens? | | |
| 2.13 | Is the spacing system (8px grid) followed? | | |
| 2.14 | Are fonts/sizes consistent with the type scale? | | |
| 2.15 | Are colors using design tokens, not hardcoded? | | |

**Content Density (Anti-Crowding):**
| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 2.16 | Does any bottom sheet contain 3 or more distinct content sections? (🟠 Major if yes — split or make scrollable) | | |
| 2.17 | Does the screen show all information at once without visual hierarchy or progressive disclosure? (🟠 Major if yes — prioritize and layer) | | |

### Screen-Level Fix Patterns

| Problem Found | Fix |
|--------------|-----|
| No clear primary action | Make ONE button visually dominant (filled, contrasting color, largest) |
| Too many competing actions | Demote secondary actions to text links, icon buttons, or overflow menu |
| Wall of text | Break into sections with headings, add bullet points, truncate with "Read more" |
| User doesn't know where they are | Add progress stepper, breadcrumb, or clear title |
| Important action below the fold | Move it up, or add a sticky bottom bar with the CTA |
| Inconsistent with other screens | Reference the design system — use the same component variants |
| Bottom sheet has 3+ sections | Split content: keep primary actions in sheet, move lists/secondary content to separate view or scrollable area |
| All information shown at once | Apply progressive disclosure: show summary → expand for details. Use cards, accordions, or tabs to layer information |

---

## 4. Phase 3: Mobile Usability Audit

If the flow includes mobile screens, run this phase. Even web flows should be checked if responsive.

### Touch & Reach

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 3.1 | Are all touch targets at least 44×44px? | | |
| 3.2 | Is there at least 8px spacing between touch targets? | | |
| 3.3 | Is the primary CTA in the thumb zone (bottom 1/3 of screen)? | | |
| 3.4 | Are frequently used actions reachable with one hand? | | |
| 3.5 | Can the flow be completed one-handed? | | |

### Layout & Content

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 3.6 | Is the layout single-column on mobile? | | |
| 3.7 | Is body text at least 16px? | | |
| 3.8 | Is there no unintentional horizontal scrolling? | | |
| 3.9 | Are safe areas respected? (Notch, Dynamic Island, home indicator) | | |
| 3.10 | Does content not get hidden by the keyboard when inputs are focused? | | |

### Mobile Navigation

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 3.11 | Is the navigation pattern appropriate for the app complexity? | | |
| 3.12 | Does the bottom tab bar have 5 items or fewer? | | |
| 3.13 | Do all nav items have icon + text label? | | |
| 3.14 | Is the active tab clearly distinguished? | | |
| 3.15 | Does the back gesture (swipe from left edge) work? (Not blocked by UI) | | |

### Mobile Interactions

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 3.16 | Are there no hover-dependent interactions? | | |
| 3.17 | Do gesture-based actions have a visible button alternative? | | |
| 3.18 | Are modals/sheets dismissible by swiping down? | | |
| 3.19 | Is pull-to-refresh available for content that updates? | | |
| 3.20 | Are form inputs using the correct mobile keyboard type? | | |

### Mobile Fix Patterns

| Problem Found | Fix |
|--------------|-----|
| CTA at top of screen | Move to sticky bottom bar or lower section |
| Tiny touch targets | Increase button/tap area to 48px minimum, add padding to icon buttons |
| Horizontal overflow | Switch to stacked layout, use horizontal scroll carousel intentionally |
| Keyboard covers input | Scroll active input into view, ensure "Next/Done" button is visible |
| No bottom navigation | Add bottom tab bar if app has 3–5 sections |
| Gesture-only action | Add visible button alongside gesture (e.g., trash icon + swipe-to-delete) |

---

## 5. Phase 4: Interaction & Feedback Audit

Does the interface respond to the user's actions properly?

### Feedback Checks

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 4.1 | Does every tap/click produce visible feedback within 100ms? | | |
| 4.2 | Do buttons have a pressed/active state? | | |
| 4.3 | Is there a loading indicator when an action takes > 1 second? | | |
| 4.4 | Are long operations (> 3 seconds) showing progress, not just a spinner? | | |
| 4.5 | Does the user receive confirmation after completing an action? | | |

### Form Feedback

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 4.6 | Are form errors shown inline below the relevant field? | | |
| 4.7 | Do error messages explain what's wrong AND how to fix it? | | |
| 4.8 | Is validation happening on blur (not only on submit)? | | |
| 4.9 | Are success indicators shown for correctly filled fields? | | |
| 4.10 | Is the submit button enabled/disabled state clear? If disabled, does it explain why? | | |

### Transition & Navigation Feedback

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 4.11 | Do transitions between screens feel logical? (Push for deeper, slide back for return) | | |
| 4.12 | Is there a visual transition, not an abrupt jump, between screens? | | |
| 4.13 | After a destructive action, is an "Undo" toast shown? | | |
| 4.14 | Are notification badges updated in real-time? | | |
| 4.15 | Is the user notified of background task completion? (Toast, banner, badge) | | |

### Feedback Fix Patterns

| Problem Found | Fix |
|--------------|-----|
| No tap feedback | Add pressed state variant (darken/scale-down) to all interactive components |
| No loading indicator | Add skeleton screen for content loads, spinner for action loads |
| Errors only on submit | Switch to inline validation on blur for each field |
| Generic error message | Rewrite to: "[Specific problem]. [How to fix it]." |
| No success confirmation | Add success screen, toast, or checkmark animation after completion |
| No undo for destructive action | Add "Undo" snackbar that appears for 5 seconds after delete/archive |

---

## 6. Phase 5: Edge Case & State Coverage

This is where most flows fall apart. Check if the design accounts for real-world scenarios.

### State Coverage Matrix

For each screen in the flow, verify these states are designed:

| State | Designed? | Notes |
|-------|-----------|-------|
| **Default** (data present, happy path) | | |
| **Loading** (data being fetched) | | |
| **Empty** (no data to show) | | |
| **Error** (something went wrong) | | |
| **Partial** (some data, not all) | | |
| **Offline** (no internet connection) | | |
| **First-time** (new user, no history) | | |
| **Permission denied** (locked feature) | | |
| **Single item** (only one result) | | |
| **Overflow** (too much data, long text) | | |

### Scenario Checks

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 5.1 | What happens if the API/data fails to load? Is there an error screen with retry? | | |
| 5.2 | What happens if the user has zero items/data? Is there an empty state? | | |
| 5.3 | What happens if text content is very long? Does it truncate or overflow gracefully? | | |
| 5.4 | What happens if text content is very short? Does the layout still look balanced? | | |
| 5.5 | What happens if an image fails to load? Is there a fallback placeholder? | | |
| 5.6 | What happens if the user goes offline mid-flow? | | |
| 5.7 | What happens if the session expires during the flow? | | |
| 5.8 | What happens if the user is brand new (no history, empty states)? | | |
| 5.9 | What happens if there are hundreds/thousands of items? (Pagination, performance) | | |
| 5.10 | What happens if the user rotates their phone to landscape? | | |

### Edge Case Fix Patterns

| Problem Found | Fix |
|--------------|-----|
| No empty state | Design: Illustration + Headline + Description + CTA button |
| No loading state | Design: Skeleton screen matching the layout shape of actual content |
| No error state | Design: Friendly message + Retry button + "Contact support" link |
| Long text breaks layout | Add text truncation with "Show more" or ellipsis overflow |
| No offline handling | Show cached data with "Offline" banner, or show offline screen with retry |
| No image fallback | Add neutral placeholder (gray box with icon) for broken/missing images |
| Session expiry not handled | Design re-authentication modal that preserves user's place in the flow |

---

## 7. Phase 6: Accessibility Audit

Can ALL users complete this flow, including those with disabilities?

### Visual Accessibility

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 6.1 | Does all text meet 4.5:1 contrast ratio? (3:1 for large text) | | |
| 6.2 | Do UI elements (icons, borders, inputs) meet 3:1 contrast? | | |
| 6.3 | Is information conveyed by more than just color? (Color + icon + text) | | |
| 6.4 | Is the interface usable at 200% zoom? | | |
| 6.5 | Are error states indicated with icon + text, not just red color? | | |

### Motor Accessibility

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 6.6 | Are all touch targets at least 44×44px? | | |
| 6.7 | Is there at least 8px (ideally 12px) between targets? | | |
| 6.8 | Is there a non-gesture alternative for every swipe/drag action? | | |
| 6.9 | Are there no time-limited interactions? (Or can the user extend time?) | | |
| 6.10 | Can the entire flow be completed via keyboard alone? (Web) | | |

### Cognitive Accessibility

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 6.11 | Is language plain and simple? (No jargon unless domain-specific) | | |
| 6.12 | Are error messages helpful? (Not just "Error" or codes) | | |
| 6.13 | Is the flow consistent with other flows in the app? | | |
| 6.14 | Are multi-step processes showing progress? | | |
| 6.15 | Are destructive actions clearly differentiated from safe actions? | | |

### Screen Reader Readiness (Annotation Check)

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 6.16 | Do headings follow logical hierarchy? (H1 → H2 → H3, no skips) | | |
| 6.17 | Are meaningful images annotated with alt text descriptions? | | |
| 6.18 | Are decorative images marked to be skipped? | | |
| 6.19 | Is the reading/tab order logical? (Left→right, top→bottom) | | |
| 6.20 | Do icon-only buttons have text labels or annotations for screen readers? | | |

---

## 8. Phase 7: Cognitive Load & Clarity

Is the flow mentally effortless or exhausting?

### Cognitive Load Checks

| # | Check | Pass/Fail | Notes |
|---|-------|-----------|-------|
| 7.1 | Are there 7 or fewer choices/options per screen? (Hick's Law) | | |
| 7.2 | Is only the necessary information shown at each step? (Progressive disclosure) | | |
| 7.3 | Are related items grouped visually? (Proximity principle) | | |
| 7.4 | Do familiar patterns from popular apps apply? (Jakob's Law) | | |
| 7.5 | Can the user predict what happens next at every step? | | |
| 7.6 | Is the primary action at each step unmistakable? | | |
| 7.7 | Is the user ever forced to remember info from a previous screen? | | |
| 7.8 | Are decisions binary when possible? (Yes/No rather than choose from 10) | | |
| 7.9 | Is the language action-oriented? ("Save changes" not "Submit") | | |
| 7.10 | Would a first-time user understand every screen without help text? | | |
| 7.11 | Is progressive disclosure used for data-dense screens? (🟡 Minor if not — consider expandable sections) | | |
| 7.12 | Does the user need to process 4+ distinct data categories simultaneously on a single screen? (🟠 Major if yes — group or paginate) | | |

### Cognitive Load Scoring

Count the "mental tasks" on each screen:
- Reading a label = 1 unit
- Making a choice = 2 units
- Entering data = 3 units
- Recalling information from memory = 4 units
- Understanding a new concept = 5 units

**Score per screen:**
- ✅ Under 10 units = Light cognitive load (good)
- ⚠️ 10–20 units = Moderate (look for simplification)
- ❌ Over 20 units = Heavy (split the screen or reduce content)

### Clarity Fix Patterns

| Problem Found | Fix |
|--------------|-----|
| Too many options | Group into categories, hide advanced options behind "More", use smart defaults |
| User must remember info from previous screen | Show a summary/context bar, or carry forward key info |
| New concept introduced without explanation | Add a brief tooltip, helper text, or inline explanation |
| Button label is vague ("Submit", "Continue") | Use action-specific labels ("Place Order", "Send Message", "Create Account") |
| User can't predict the outcome | Add a preview, summary step, or explanatory text before the action |
| Multiple actions competing for attention | Make ONE primary (filled, prominent), rest secondary (outlined or text) |
| No progressive disclosure | Add expandable sections, "Show more" patterns, or drill-down views for detailed information |
| 4+ data categories on one screen | Group related data into tabs or sections. Show summary metrics with drill-down for details |

---

## 9. Severity Rating System

Rate every finding on this scale:

### 🔴 Critical (Must fix before launch)
- User cannot complete the flow at all
- Data loss or destructive action with no confirmation
- Accessibility blocker (screen reader users can't proceed, contrast fails completely)
- No error handling — blank screen or crash scenario
- Security/privacy issue in the flow

### 🟠 Major (Should fix before launch)
- User can complete the flow but with significant confusion or frustration
- Missing important state (empty state, loading state, error state)
- Touch targets below 44px on mobile
- Primary action is not obvious or is buried
- Inconsistent behavior between similar screens
- More than 7 steps for a simple task

### 🟡 Minor (Fix in next iteration)
- User can complete the flow but the experience isn't polished
- Minor spacing or alignment inconsistencies
- Missing microinteractions (no tap feedback, no transitions)
- Slightly unclear copy or labels
- Missing "nice to have" states (first-time experience, offline)
- Visual inconsistencies with design system

### 🔵 Suggestion (Enhancement opportunity)
- Ideas that would delight users but aren't blocking
- Animation and motion polish
- Advanced personalization opportunities
- Performance optimizations for edge cases
- "Would be nice" features

---

## 10. Fix Recommendation Framework

When recommending a fix, always structure it like this:

### Fix Template
```
FINDING:    [What's wrong — describe the specific problem]
SCREEN:     [Which screen(s) in the flow]
SEVERITY:   [🔴 Critical / 🟠 Major / 🟡 Minor / 🔵 Suggestion]
HEURISTIC:  [H1–H10 — which Nielsen heuristic is violated]
IMPACT:     [Who is affected and how — what happens to the user]
UX RULE:    [Which UX principle is violated]
FIX:        [Specific, actionable design change]
REFERENCE:  [Link to relevant section in design guide]
```

### Example Findings

**Example 1:**
```
FINDING:    Primary "Submit" button is at the top of the screen on mobile
SCREEN:     Dispute Form (Screen 3 of 5)
SEVERITY:   🟠 Major
HEURISTIC:  H7 — Flexibility and Efficiency of Use
IMPACT:     Users must stretch to the top of the screen to submit.
            One-handed users will struggle. Increases form abandonment.
UX RULE:    Fitts's Law — important actions should be big and close.
            Thumb Zone — primary actions belong in the bottom 1/3.
FIX:        Move the Submit button to a sticky bottom bar, 48px tall,
            full width, with safe area padding below.
REFERENCE:  Mobile-First Design Strategy → Thumb Zone
```

**Example 2:**
```
FINDING:    Error state shows blank white screen when API call fails
SCREEN:     Order History (Screen 2 of 3)
SEVERITY:   🔴 Critical
HEURISTIC:  H9 — Help Users Recognize, Diagnose, and Recover from Errors
IMPACT:     User sees nothing. No explanation, no recovery path.
            User assumes the app is broken and abandons.
UX RULE:    Every screen needs an error state with recovery action.
FIX:        Design error state with: Friendly illustration +
            "Something went wrong" headline + "Tap to retry" button +
            "Contact support" text link.
REFERENCE:  Empty, Error & Edge States → Edge Case Checklist
```

**Example 3:**
```
FINDING:    Form validates only on submit — 6 fields, all errors appear at once
SCREEN:     Registration Form (Screen 1 of 2)
SEVERITY:   🟠 Major
HEURISTIC:  H5 — Error Prevention
IMPACT:     User fills entire form, hits submit, sees 4 errors at once.
            Frustrating — feels like the form is fighting them.
UX RULE:    Validate on blur, show errors inline per field.
FIX:        Add inline validation per field on blur. Show error text
            below each field with red border + error icon + message.
            Validate email format, password strength, etc. per field.
REFERENCE:  Forms & Data Input → Validation
```

---

## 11. Flow Audit Report Template

After running all phases, compile findings into this report structure:

### Report Structure

```
# UX Flow Audit Report
## Flow: [Name of the flow]
## Date: [Audit date]
## Auditor: [Name]

---

## Flow Overview
- **User Goal**: [What the user is trying to accomplish]
- **Entry Point**: [How the user arrives at this flow]
- **Screen Count**: [Number of screens in the flow]
- **Target Device**: [Mobile / Web / Both]
- **Persona**: [Who is the user — brief context]

---

## Summary Score

| Phase | Score | Critical | Major | Minor | Suggestion |
|-------|-------|----------|-------|-------|------------|
| Flow Structure | ✅ / ⚠️ / ❌ | 0 | 0 | 0 | 0 |
| Screen Quality | ✅ / ⚠️ / ❌ | 0 | 0 | 0 | 0 |
| Mobile Usability | ✅ / ⚠️ / ❌ | 0 | 0 | 0 | 0 |
| Interaction Design | ✅ / ⚠️ / ❌ | 0 | 0 | 0 | 0 |
| Edge Case Coverage | ✅ / ⚠️ / ❌ | 0 | 0 | 0 | 0 |
| Accessibility | ✅ / ⚠️ / ❌ | 0 | 0 | 0 | 0 |
| Cognitive Load | ✅ / ⚠️ / ❌ | 0 | 0 | 0 | 0 |
| **TOTAL** | | **0** | **0** | **0** | **0** |

### Overall Verdict
- ✅ **PASS** — 0 Critical, ≤ 2 Major findings
- ⚠️ **CONDITIONAL PASS** — 0 Critical, 3–5 Major findings
- ❌ **FAIL** — Any Critical findings, or 6+ Major findings

---

## Heuristic Violation Distribution

| Heuristic | Violations | Highest Severity |
|-----------|-----------|-----------------|
| H1: Visibility of System Status | 0 | — |
| H2: Match System ↔ Real World | 0 | — |
| H3: User Control & Freedom | 0 | — |
| H4: Consistency & Standards | 0 | — |
| H5: Error Prevention | 0 | — |
| H6: Recognition Over Recall | 0 | — |
| H7: Flexibility & Efficiency | 0 | — |
| H8: Aesthetic & Minimalist Design | 0 | — |
| H9: Error Recovery | 0 | — |
| H10: Help & Documentation | 0 | — |

_Patterns: If 3+ violations cluster on a single heuristic, it indicates a systemic issue — not just isolated bugs._

---

## Detailed Findings

### 🔴 Critical Issues
[List each with the fix template]

### 🟠 Major Issues
[List each with the fix template]

### 🟡 Minor Issues
[List each with the fix template]

### 🔵 Suggestions
[List each with the fix template]

---

## Recommended Fix Priority

### Immediate (Before next review)
1. [Fix 1]
2. [Fix 2]

### Next Sprint
1. [Fix 3]
2. [Fix 4]

### Backlog
1. [Fix 5]
2. [Fix 6]

---

## State Coverage Summary

| Screen Name | Default | Loading | Empty | Error | Offline |
|-------------|---------|---------|-------|-------|---------|
| Screen 1    | ✅/❌   | ✅/❌   | ✅/❌ | ✅/❌ | ✅/❌   |
| Screen 2    | ✅/❌   | ✅/❌   | ✅/❌ | ✅/❌ | ✅/❌   |
| Screen 3    | ✅/❌   | ✅/❌   | ✅/❌ | ✅/❌ | ✅/❌   |

---

## Notes for Development Handoff
[Any specific notes about interaction behavior, animations,
 conditional logic, or edge cases that developers need to know]
```

---

## 12. Common Flow Patterns & Benchmarks

Use these benchmarks to evaluate whether a flow is above or below standard.

### Step Count Benchmarks

| Flow Type | Ideal Steps | Max Acceptable | Over This = Problem |
|-----------|-------------|----------------|-------------------|
| Sign up / Registration | 1–2 | 3 | 4+ |
| Login | 1 | 2 (with 2FA) | 3+ |
| Search → View result | 2 | 3 | 4+ |
| Add to cart → Checkout | 2–3 | 4 | 5+ |
| Create a new item/post | 1–2 | 3 | 4+ |
| Edit profile info | 1 | 2 | 3+ |
| File a dispute/complaint | 3–4 | 5 | 6+ |
| Onboarding tutorial | 3 | 5 | 6+ |
| Settings change | 1–2 | 3 | 4+ |
| Payment/transaction | 2–3 | 4 | 5+ |

### Screen Density Benchmarks

| Screen Type | Ideal Elements | Max Before Overload |
|-------------|---------------|-------------------|
| Landing / Home | 5–7 sections | 10 |
| List view | 5–10 visible items | 15 |
| Detail view | 4–6 content blocks | 8 |
| Form screen | 3–5 fields | 7 (then split into steps) |
| Settings | 5–8 options per group | 10 per group |
| Modal / Bottom sheet | 1–3 actions | 5 |
| Dashboard | 3–5 data widgets | 8 |

### Conversion Benchmarks (Approximate Drop-Off)

Each step in a flow typically loses 10–20% of users:
```
Step 1:  100% of users start
Step 2:   80-90% continue
Step 3:   65-80% continue
Step 4:   50-70% continue
Step 5:   40-60% continue
Step 6+:  Significant abandonment risk
```

This means a 6-step flow may lose 40–60% of users before completion.
Every step you remove improves conversion.

### Industry Competitive Benchmarks

When auditing a flow, compare against these real-world benchmarks from top apps:

**Authentication:**
| Metric | Industry Best | Acceptable | Below Standard |
|--------|-------------|------------|----------------|
| Time to sign up | < 30 seconds | 30–60 seconds | > 60 seconds |
| Social login offered | Yes (Google/Apple) | One option | None |
| Fields in sign-up | 2–3 (email, password) | 4–5 | 6+ |
| Password reset steps | 2 (email → reset) | 3 | 4+ |
| Biometric login | Yes (Face ID / fingerprint) | Optional | Not available |

**E-Commerce:**
| Metric | Industry Best (Amazon, Shopify) | Acceptable | Below Standard |
|--------|-------------------------------|------------|----------------|
| Add to cart → Purchase | 1–2 taps (Buy Now) | 3–4 steps | 5+ steps |
| Guest checkout available | Yes | Behind account wall | No |
| Saved payment methods | Yes + Apple/Google Pay | Card entry | Manual every time |
| Order confirmation | Immediate + email | Screen only | No confirmation |
| Return/refund initiation | Self-service, 2 steps | Contact required | No clear path |

**Search & Discovery:**
| Metric | Industry Best | Acceptable | Below Standard |
|--------|-------------|------------|----------------|
| Search to first result | < 1 second | 1–3 seconds | > 3 seconds |
| Search suggestions | Auto-complete + recent | Auto-complete only | None |
| Empty search results | Suggestions + alternatives | "No results" message | Blank screen |
| Filter count | 3–5 relevant filters | 1–2 | None or 10+ |

**Onboarding:**
| Metric | Industry Best | Acceptable | Below Standard |
|--------|-------------|------------|----------------|
| Steps to first value | ≤ 3 | 4–5 | 6+ |
| Skip option | Always available | After first screen | Not available |
| Progress indicator | Yes | Partial | None |
| Personalization | Adapts to user choices | Static | One-size-fits-all |

**Messaging & Communication:**
| Metric | Industry Best | Acceptable | Below Standard |
|--------|-------------|------------|----------------|
| Send message flow | Type → Send (1 step) | 2 steps | 3+ steps |
| Read receipts / status | Real-time indicators | Delayed | None |
| Attachment sharing | In-line + preview | Upload only | Not supported |
| Notification → content | 1 tap to relevant screen | Opens app home | Wrong destination |

**Task Completion (General):**
| Metric | Industry Best | Acceptable | Below Standard |
|--------|-------------|------------|----------------|
| Task success rate | > 90% | 70–90% | < 70% |
| Time on task (simple) | < 30 seconds | 30–90 seconds | > 90 seconds |
| Error rate per task | < 5% | 5–15% | > 15% |
| User satisfaction (SUS) | > 80 | 68–80 | < 68 |

Use these benchmarks in audit findings:
```
FINDING:    Checkout flow requires 6 steps from cart to purchase
BENCHMARK:  Industry best = 1-2 steps; Acceptable = 3-4 steps
SEVERITY:   🟠 Major — Below industry standard
```

---

## 13. Re-Validation After Fixes

After making changes based on the audit, run a quick re-validation:

### Quick Re-check Checklist

| # | Check | Pass? |
|---|-------|-------|
| 1 | All 🔴 Critical findings addressed? | |
| 2 | All 🟠 Major findings addressed or documented as accepted risk? | |
| 3 | New changes don't introduce NEW issues? | |
| 4 | Flow step count same or reduced? | |
| 5 | All screens still have clear primary action? | |
| 6 | State coverage (empty, loading, error) still complete? | |
| 7 | Mobile usability checks still passing? | |
| 8 | Accessibility checks still passing? | |
| 9 | Changes are consistent with the design system? | |
| 10 | Prototype updated and tested on device? | |

### When to Re-run Full Audit
- After significant flow restructuring (screens added, removed, or reordered)
- After adding a new branch/path to the flow
- Before major development handoff
- After user testing reveals issues
- When the design system changes significantly

### Final Validation: The "Mom Test"
Imagine handing your phone to someone unfamiliar with the app:
- Could they figure out what to do WITHOUT you explaining anything?
- Would they get stuck or confused at any screen?
- Would they know when they've completed the task?
- Would they feel confident using it, or anxious about making a mistake?

If you have to explain anything, the design isn't clear enough yet.

---

## Quick Reference: The 7 Questions

For any flow, at any point, ask yourself these 7 questions:

1. **Can the user tell where they are?** (Context / location in the flow)
2. **Can the user tell what to do?** (Primary action visibility)
3. **Can the user tell what happened?** (Feedback after actions)
4. **Can the user recover from mistakes?** (Undo, back, cancel)
5. **Can the user complete this with one hand on mobile?** (Thumb zone)
6. **Can a user with disabilities complete this?** (Accessibility)
7. **Would a first-time user understand this without help?** (Cognitive load)

If any answer is "no" → that's your next fix.

---

## 14. User Research Integration

Design audits catch design problems. User research catches **user** problems. The strongest UX practice combines both. This section covers when and how to validate your flows with real users.

### When to Run Each Research Method

| Method | When to Use | Time | Cost | What It Reveals |
|--------|------------|------|------|----------------|
| **5-Second Test** | Early concepts, landing pages | 10 min setup | Free (UsabilityHub) | First impressions, value prop clarity |
| **Unmoderated Usability Test** | Prototypes, existing flows | 1–2 hours setup | $50–200/session (UserTesting, Maze) | Task completion, confusion points |
| **Moderated Usability Test** | Complex flows, sensitive topics | 2–4 hours per session | $100–300/session | Deep "why" insights, emotional responses |
| **A/B Test** | Two design options, live product | Days to weeks | Free (Google Optimize, PostHog) | Statistical winner for conversions |
| **Card Sort** | Information architecture, navigation | 30 min setup | Free (OptimalSort) | How users expect content organized |
| **Tree Test** | Navigation structure validation | 30 min setup | Free (Treejack) | Can users find things without the UI? |
| **Heatmap/Session Recording** | Live product analysis | Always-on | $0–100/mo (Hotjar, FullStory) | Where users click, scroll, and rage-click |
| **Survey (In-App)** | Post-task feedback, satisfaction | 30 min setup | Free (Typeform, in-app) | Subjective satisfaction, NPS, SUS |

### System Usability Scale (SUS) Scoring

The SUS is the industry standard for measuring perceived usability. After users complete tasks, they rate 10 statements on a 1–5 scale. Score interpretation:

| SUS Score | Grade | Interpretation |
|-----------|-------|---------------|
| 80.3+ | A | Excellent — users love it |
| 68–80.3 | B–C | Good to acceptable — room for improvement |
| 51–68 | D | Below average — significant usability issues |
| Below 51 | F | Poor — major redesign needed |

**Industry average SUS score: 68**. Target 75+ for consumer apps, 68+ for enterprise.

### Task Success Metrics

When running usability tests, measure these for each flow:

| Metric | How to Measure | Good | Acceptable | Problem |
|--------|---------------|------|------------|---------|
| **Task Completion Rate** | % of users who finish successfully | > 90% | 70–90% | < 70% |
| **Time on Task** | Seconds from start to completion | Under benchmark | 1.5× benchmark | 2× or more |
| **Error Rate** | Errors per task attempt | < 1 per task | 1–2 per task | 3+ per task |
| **Misclick Rate** | Taps/clicks on wrong elements | < 5% | 5–15% | > 15% |
| **Assistance Requests** | Times user asks "what do I do?" | 0 | 1 | 2+ |
| **Recovery Rate** | % of users who recover from errors | > 95% | 80–95% | < 80% |

### Connecting Research to Audit Findings

After running user research, map observations back to this audit:

```
RESEARCH FINDING:  3 of 5 users couldn't find the "Edit Profile" option
OBSERVED BEHAVIOR: Users tapped the avatar expecting a menu, but it was decorative
AUDIT PHASE:       Phase 2 (Screen-by-Screen) — Check 2.3: Primary action visibility
HEURISTIC:         H6 — Recognition Rather Than Recall
SEVERITY:          🟠 Major
FIX:               Make avatar tappable → opens profile sheet with edit option.
                   Add "Edit" text link next to profile section as secondary path.
```

### Research Planning by Project Stage

| Project Stage | Recommended Methods | Focus |
|---------------|-------------------|-------|
| **Discovery** | Interviews, card sorts, competitor analysis | Understand user needs and mental models |
| **Early Design** | 5-second tests, concept testing | Validate direction before committing |
| **Prototyping** | Unmoderated usability tests, tree tests | Find friction before building |
| **Pre-Launch** | Moderated usability tests, accessibility testing | Catch critical issues |
| **Post-Launch** | Heatmaps, session recordings, SUS surveys, A/B tests | Optimize based on real behavior |
| **Ongoing** | Analytics review, NPS tracking, support ticket analysis | Continuous improvement |
