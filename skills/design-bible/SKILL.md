---
name: design-bible
description: Create or update a design bible for a project — a living document that captures visual language, component patterns, voice, and design philosophy so that an agent can build cohesive, high-quality pages from scratch. Use when the user asks to create a design system doc, design bible, or wants to document their project's visual identity.
---

# Design Bible

Create a living design document that captures everything an agent needs to build pages that match the project's quality and character — on the first try, without iteration.

A good design bible isn't a CSS reference. It's a prompting guide that encodes the *thinking* behind decisions so that future agents inherit the taste, not just the tokens.

## Arguments

`<args>` is freeform. Extract:
- **Scope** — full bible from scratch, update existing, or focus on a specific section
- **Project context** — what the product is, who it's for
- Any specific decisions the user wants captured

## What Makes a Great Design Bible

The best design bibles share these qualities (learned from production projects):

1. **Philosophy before tokens.** Start with WHY the design looks this way, not WHAT the hex values are. "Warm, light, clean because the user is stressed about finding a home" tells you more than `--bg: #f8f7f4`.

2. **Anti-patterns alongside patterns.** What NOT to do is as important as what to do. "No italic inline. No serif in body text. No third emphasis type." prevents the most common drift.

3. **Voice and copy guidelines with examples.** Good/bad examples of copy are the single highest-leverage section. An agent that knows to write "busy, a bit scruffy, but lined with everything you need" instead of "vibrant and diverse area" will produce better pages than one that has every CSS variable memorised.

4. **Component patterns describe visual intent, not just CSS.** "Score strip: one unified card, divided by hairline gaps — compact, scannable, doesn't dominate the page" is more useful than a CSS snippet because it tells you WHEN and WHY to use it.

5. **Specific enough to rebuild from.** The test: could an agent who has never seen the app build a new page that looks like it belongs? If not, the bible is missing something.

## Process

### Phase 1: Discover What Exists

Before writing anything, understand the current state:

1. **Find existing design docs** — search for `design-system.md`, `design-bible.md`, `visual-design.md` in `docs/`, `docs/architecture/`, `docs/reference/`
2. **Find the CSS source of truth** — look for theme files, CSS custom properties, Tailwind config, or inline `:root` blocks in HTML templates
3. **Find mockups and templates** — look in `mockups/`, `scripts/mockups/`, `templates/` for the most recent approved designs
4. **Read the app's existing pages** — understand the actual visual language in use, not just what's documented

Build a mental model of: colour system, typography, spacing, component patterns, layout conventions, and any visual texture/effects.

### Phase 2: Interview for Philosophy

The code tells you WHAT. The user tells you WHY. Ask about (or extract from conversation history):

- **Who is this for?** The audience shapes everything. A power tool for data scientists feels different from a landing page for first-time renters.
- **What should it feel like?** Not "modern" or "clean" (meaningless). Ask for analogies: "Like reading a well-designed magazine article" or "Like a high-end terminal emulator." If the user struggles, offer contrasting pairs: editorial vs dashboard? Warm vs clinical? Dense vs spacious?
- **What mistakes were corrected during development?** These are gold. "We tried italic serif inline and it was too busy" becomes an anti-pattern rule that prevents future drift.
- **What decisions are load-bearing?** Some choices look arbitrary but aren't. "One font for emphasis, not two" might come from three rounds of iteration. Capture WHY.
- **What's the voice?** If the product has copy/text, how should it sound? Get examples of good and bad. Anchor this in the product's relationship with its user.

Don't ask all of these as a questionnaire. If context is already available from conversation history or existing docs, use it. Only ask what's missing.

### Phase 3: Write the Bible

Structure the document with these sections, adapting order and depth to the project:

#### 1. Identity & Philosophy

Open with what the product IS and who it's FOR. Then the design philosophy — the principles that drive every visual decision. This section should read like instructions to someone who's about to design a page they've never seen.

**Key questions this section answers:**
- What does this product feel like to use?
- What analogies describe its aesthetic?
- What is it explicitly NOT?

**Example (from a fitness coaching app):**
> "Evidence-based, transparent reasoning, copilot not autopilot. Confident, never aggressive. Shows reasoning, not just results. Speaks like a coach in conversation."

**Example (from a rental property finder):**
> "The product feels like a knowledgeable friend, not a database. Every page should feel like getting advice from someone who actually lives in the area."

#### 2. Colour System

Document tokens with **intent**, not just values. Group by role (backgrounds, text, accent, semantic, domain-specific). Reference the source of truth file.

**Critical:** Document colour RULES, not just colour VALUES:
- When to use each semantic colour (green = genuinely good, not "any positive thing")
- Colours reserved for specific contexts (e.g. gold only for achievements, never warnings)
- How many accent colours and when to use each

#### 3. Typography

Fonts, roles, weights, sizes. But more importantly:
- **Which font does what job** — "Outfit is the voice, Playfair is for titles only, never inline"
- **What NOT to use** — "No italic. No serif in body. No third typeface."
- **Size philosophy** — "15px for content pages (reading), 14px for dashboard (density)"

Include a sizing scale table with element → size → weight → context.

#### 4. Voice & Copy

This is the section that separates a good bible from a great one. Cover:

- **Tone** — with a concrete analogy ("like a knowledgeable friend", "like a coach in conversation")
- **Good/bad examples** — at least 3 pairs showing the voice in action
- **Emphasis rules** — how many types of emphasis, what each means, what's forbidden
- **Copy patterns** — how headings relate to subtitles, how sections are introduced
- **Domain-specific rules** — "walk times > distances", "name the actual place", "state the downsides"

**Example good/bad pair:**
> **Good:** "Mare Street is busy, a bit scruffy, but lined with everything you need."
> **Bad:** "Hackney is a vibrant and diverse area with excellent amenities."

#### 5. Component Patterns

Document each reusable pattern with:
- **What it looks like** (visual description, not just CSS)
- **When to use it** (what problem it solves)
- **Key details** (border radius, shadow level, hover behaviour)
- **Variations** if any

Prioritise components that carry the brand identity (the ones that make the app look like THIS app and not a generic template). For projects with many components, group by category.

Include CSS snippets or class references where useful, but lead with the visual intent.

#### 6. Layout & Spacing

Content widths, padding, breakpoints, grid systems. Include:
- Max content widths and WHY they differ by context
- Responsive breakpoints and what changes at each
- Spacing philosophy ("generous, not cramped" or "dense but organised")

#### 7. Animation & Motion

If the project uses animation:
- Named keyframes with timings
- Philosophy ("restrained and purposeful" or "dramatic reveals")
- Stagger patterns for lists
- Hover/active feedback

#### 8. Anti-Patterns

Explicitly list what breaks the design language. These prevent drift better than positive rules alone.

**Example:**
> - No rounded corners (zero border-radius everywhere except LED dots)
> - No emojis in UI — LED indicators and typography only
> - No `font-bold` — headings use `font-light`
> - No new chart types without updating this document

#### 9. Page Structure / Narrative

If the product has content pages (not just a dashboard), document:
- The narrative arc of a page (what order sections appear and why)
- Section anatomy (heading + subtitle + content pattern)
- How pages relate to each other (internal linking, navigation flow)

### Phase 4: Validate

After writing, check the bible against these criteria:

- [ ] Could an agent build a new page that looks like it belongs, using only this document?
- [ ] Are there good/bad examples for voice and copy?
- [ ] Are anti-patterns documented?
- [ ] Does the philosophy section explain WHY, not just WHAT?
- [ ] Are component patterns described by visual intent, not just CSS?
- [ ] Is the source of truth file referenced for tokens?
- [ ] Would the user recognise their product from the description?

### Phase 5: Place the Document

- If `docs/architecture/` exists → `docs/architecture/design-system.md`
- If `docs/reference/` exists → `docs/reference/design-system.md`
- If neither → `docs/design-system.md`
- Update the docs README index

## Updating an Existing Bible

When updating rather than creating:

1. Read the existing bible fully
2. Identify what's missing, outdated, or contradicted by the current codebase
3. Add new sections or patterns discovered during recent development
4. Capture any design feedback from the user that produced corrections
5. Don't restructure arbitrarily — maintain the existing organisation unless it's actively harmful

## What Does NOT Belong in a Design Bible

- **Implementation details** — how the build system works, file structure, deployment
- **Every CSS property** — reference the theme file for exact values, document INTENT here
- **Temporary state** — current bugs, in-progress work, unreleased plans
- **Generic design advice** — "use consistent spacing" adds nothing. Be specific to THIS project.
