# Project Discovery — Progressive Flow

> This module handles project setup through progressive questioning.
> Questions are asked ONE AT A TIME, each waiting for the user's response before proceeding.
> Already-known answers from APP_PLAN.md are always skipped.

---

## ⚠️ CRITICAL: One Question Per Message — STOP and WAIT

**You MUST ask exactly ONE question per message, then STOP and WAIT for the user's response before asking the next question.**

- Do NOT list multiple questions in a single message
- Do NOT preview upcoming questions
- Do NOT say "I'll ask you 6 questions" and then list them
- After asking a question, your message ENDS. Period. No "next I'll ask about..."
- Wait for the user's reply, then ask the NEXT question in a NEW message

**If you combine questions, you are violating this rule. One question. One message. Wait.**

---

## How Discovery Works

1. Check if `project/APP_PLAN.md` exists
   - **Exists and complete** → Skip discovery entirely, return to caller
   - **Exists but incomplete** → Ask only the missing questions
   - **Does not exist** → Run full core discovery below

2. Ask core questions one at a time (see below)
3. After all core questions answered → Generate `project/APP_PLAN.md`
4. Derive 3-5 design principles from the answers
5. Return control to the caller (router.md or command file)

---

## Core Questions (Always Asked, One at a Time)

Ask these 6 questions sequentially. Each question is its own message — NEVER combine questions. Prefer multiple choice when possible.

### Question 1: Project Name & Description

Ask:
> "What is the project name, and what does it do? (1-2 sentences)"

Store: `project.name` and `project.description`

**⏸️ STOP. Send this question only. Wait for the user's response before continuing.**

---

### Question 2: Platform

Ask:
> "What platform(s) are you designing for?
>
> A) Mobile only (iOS / Android / Both)
> B) Web only (Desktop / Responsive)
> C) Cross-platform (Mobile + Web)
> D) Other (Tablet app, Watch, TV, etc.)"

If they choose A, follow up (still one at a time):
> "Which mobile platform takes priority?
>
> A) iOS first (Apple HIG conventions)
> B) Android first (Material Design conventions)
> C) Both equally (neutral pattern)"

Store: `platform.type` and `platform.mobile_priority`

**⏸️ STOP. Send this question only. Wait for the user's response before continuing.**

---

### Question 3: Target Audience

Ask:
> "Who is the target audience? (e.g., 'young professionals 25-35', 'small business owners', 'gamers')"

Store: `project.audience`

**⏸️ STOP. Send this question only. Wait for the user's response before continuing.**

---

### Question 4: Brand Colors & Visual Tone

Ask:
> "What are your brand colors and visual tone?
>
> **Colors:** Primary color (hex or description), secondary/accent color (hex or 'derive from primary')
>
> **Tone — pick one:**
> A) Clean & Minimal — lots of white space, subtle colors
> B) Bold & Vibrant — strong colors, high contrast, energetic
> C) Dark & Sleek — dark surfaces, glowing accents, premium feel
> D) Warm & Friendly — rounded shapes, soft colors, approachable
> E) Corporate & Professional — structured, neutral tones, formal
> F) Playful & Fun — bright colors, illustrations, casual"

Store: `branding.primary_color`, `branding.secondary_color`, `branding.visual_tone`

**⏸️ STOP. Send this question only. Wait for the user's response before continuing.**

---

### Question 5: Navigation Pattern

Ask:
> "How many primary sections does the app have, and what navigation pattern do you prefer?
>
> **Sections:** A) 2-3  B) 4-5  C) 6+  D) Single flow (no persistent nav)
>
> **Mobile nav:** A) Bottom tab bar  B) Top tabs  C) Drawer/hamburger  D) Let me recommend
>
> **Desktop nav** (if applicable): A) Top bar  B) Sidebar  C) Both  D) Let me recommend"

Store: `navigation.section_count`, `navigation.mobile_pattern`, `navigation.desktop_pattern`

**⏸️ STOP. Send this question only. Wait for the user's response before continuing.**

---

### Question 6: Priority Flows/Screens

Ask:
> "What are the first 2-3 screens or flows you want to focus on? (e.g., 'onboarding flow', 'home screen', 'checkout')"

Store: `requirements.priority_flows`

**⏸️ STOP. All 6 core questions complete — proceed to generate APP_PLAN.md.**

---

## Deep Questions (On-Demand)

These are NOT asked during initial discovery. They are asked one at a time, only when the current task triggers them. If the answer is already known from APP_PLAN.md, skip.

| Trigger Context | Question to Ask |
|----------------|----------------|
| **Building in Figma with existing icon assets** | "Which Figma page contains your project's icons? (e.g., 'UI Icons', 'Assets/Icons')" → Store: `branding.icon_page_name` |
| **Designing text-heavy screens** | "What font(s) are you using? (Primary font for headings, secondary for body — or 'recommend for me')" → Store: `branding.brand_fonts` |
| **Designing text-heavy screens** | "What base body text size? A) 14px (compact) B) 16px (standard) C) 18px (spacious)" → Store: `design_system.base_text_size` |
| **Setting up design system** | "What corner radius style? A) Sharp (0-4px) B) Moderate (8-12px) C) Rounded (16-24px) D) Pill/capsule E) Recommend for me" → Store: `design_system.corner_radius` |
| **Setting up design system** | "What spacing density? A) Compact B) Comfortable (recommended) C) Spacious" → Store: `design_system.spacing_density` |
| **Setting up design system** | "Do you have an existing design system or component library? A) Yes (Figma library exists) B) Partial C) None D) Using a public system" → Store: `design_system.status` |
| **Building responsive layouts** | "What target screen sizes? Mobile: 375/393/430px, Tablet: 768/820/1024px, Desktop: 1280/1440/1920px" → Store: `platform.screen_sizes` |
| **Building responsive layouts** | "What is the Figma file structure? A) Single file B) Multi-file C) Not set up yet" |
| **Scoping special needs** | "Any special requirements? (Accessibility, i18n, offline, dark mode, animation-heavy, data-heavy, e-commerce, social, admin)" → Store: `requirements.special`, `branding.dark_mode` |
| **Scoping special needs** | "What user personas should be considered? (List names and roles)" → Store: `requirements.personas` |

---

## After Discovery: Generate APP_PLAN.md

After all core questions are answered:

1. Compile answers into the `project/APP_PLAN.md` YAML block (use the APP_PLAN.md template)
2. Derive 3-5 design principles from the answers (tone + audience + requirements)
3. Set `project.stage` using inference rules in step 5 below
4. Set defaults for unanswered deep questions:
   - `icon_set`: "Not set"
   - `icon_page_name`: "Not set"
   - `dark_mode`: "Later"
   - `design_system.status`: "None"
   - `corner_radius`: "Moderate 8-12"
   - `spacing_density`: "Comfortable"
   - `base_text_size`: "16px"
5. **Infer `project.stage`** from context:
   - If user mentions "redesign", "existing app", "revamp" → set to "Redesign"
   - If user mentions "extending", "adding features", "existing design system" → set to "Extending"
   - If user has existing Figma files with components → set to "Extending"
   - Otherwise → set to "New"
6. Return control to the caller

---

## Rules

- **One question per message** — never combine questions
- **Multiple choice preferred** — easier to answer than open-ended when possible
- **Skip known answers** — if APP_PLAN.md already has the answer, don't re-ask
- **Store answers immediately** — update APP_PLAN.md after each answer if the file already exists
- **Respect the user's pace** — wait for their response before the next question
