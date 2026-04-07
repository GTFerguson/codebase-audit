---
name: pitch-deck
description: Create, update, or generate pitch decks for Cognetic products. Use when the user wants to build a new pitch deck, update an existing one, refine the pitch deck design bible with new learnings, or generate a brand-specific pitch notes file. Reads from the general design bible, brand-specific notes, product repo design bibles, and PROVEN reference docs.
---

# Pitch Deck

Create high-quality, brand-consistent pitch decks for any Cognetic product, or update the pitch deck knowledge base with new learnings.

## Arguments

`<args>` is freeform. Extract:
- **Mode** — generate, update-bible, update-brand, or new-brand
- **Product** — which product (tensyl, house-finder, dream-decks, skillcracked)
- **Competition** — which competition/application this deck is for
- Any specific feedback or learnings to capture

## Modes

### Mode 1: Generate a Deck

**Trigger:** "make a pitch deck for X", "generate deck", "build slides for Y competition"

#### Process

1. **Read the general bible**
   - `docs/reference/pitch-deck-design-bible.md` in business-manager repo

2. **Read the brand-specific notes**
   - `docs/reference/pitch-brands/{product}.md` in business-manager repo
   - If it doesn't exist, trigger Mode 4 (new-brand) first

3. **Read the product's design bible** (if it exists)
   - Check `docs/architecture/design-system.md` or `docs/reference/design-system.md` in the product repo
   - This gives you voice, tone, and full brand identity

4. **Read the product's theme CSS**
   - Find the theme file in the product's frontend source
   - Extract colour tokens, font imports, any effects

5. **Gather content**
   - Check if an application guide exists in `applications/{competition}/`
   - Check for existing draft answers or content outlines
   - Ask the user what the product does, the target competition, and any specific angles — but only ask what's missing from existing docs

6. **Research market figures**
   - Every number in the deck must be PROVEN — named source, date, verifiable
   - Check existing reference docs in `docs/reference/` for sourced figures
   - If figures are missing, research them and save to a reference doc BEFORE using in the deck

7. **Build the deck**
   - Single HTML file following the general bible's structure (10 slides + sources)
   - Use the brand's colour tokens, fonts, and effects
   - Follow all typography, emphasis, and layout rules from the bible
   - Export to PDF via Playwright (1280×720, printBackground, zero margins)

8. **Iterate with variants**
   - Don't try to get every slide right first time
   - Build A/B/C variants for slides that need exploration
   - Present variants side by side for the user to choose

#### File output
```
applications/{competition}/
├── pitch-deck.html
├── pitch-deck.pdf
├── export-pdf.sh
├── slide-variants*.html (if created)
└── {images}
```

---

### Mode 2: Update the General Bible

**Trigger:** "update pitch bible", "add learning", "we learned that X", "new pattern for slides"

#### Process

1. Read `docs/reference/pitch-deck-design-bible.md`
2. Ask the user (or extract from conversation) what was learned:
   - New pattern that worked?
   - Anti-pattern discovered?
   - Layout rule refined?
   - Content principle clarified?
3. Update the relevant section of the bible
4. If the learning is brand-specific, update the brand doc instead (Mode 3)
5. Commit with a message explaining what was learned and why

#### What belongs in the general bible
- Slide structure patterns (new slide types, layout approaches)
- Typography rules that apply across products
- Emphasis and readability rules
- Anti-patterns that apply regardless of brand
- Export workflow improvements
- Content principles (what to claim, citation format)
- Variant/iteration workflow improvements

#### What does NOT belong
- Brand-specific tokens or colours
- Product-specific content decisions
- One-off fixes for a particular deck

---

### Mode 3: Update Brand Notes

**Trigger:** "update tensyl pitch notes", "add this to the house finder brand", "we learned X about the tensyl deck"

#### Process

1. Read `docs/reference/pitch-brands/{product}.md`
2. Update with the new learning:
   - New colour decision or readability adjustment?
   - Screenshot that works well?
   - Content decision (what to claim, what not to)?
   - Lesson learned from iteration?
   - Slide-specific CSS that's worth preserving?
3. Commit

#### What belongs in brand notes
- Product colour tokens (with any readability adjustments for slides)
- Font specifics and logo treatment
- Background effect values (bg-mesh gradients, glow colours)
- Screenshots inventory and which work best where
- Lessons learned from that product's deck iterations
- Content decisions specific to that product (market figures used, claims made)
- Slide-specific CSS (engine ring params, competition table styling)

---

### Mode 4: Create New Brand Notes

**Trigger:** "set up pitch notes for house finder", "new brand for dream decks", first time generating a deck for a product

#### Process

1. **Find the product's theme**
   - Search the product repo for theme CSS, CSS variables, Tailwind config
   - Check `docs/architecture/design-system.md` if it exists

2. **Find the product's visual identity**
   - Check the cv-site page: `../cv-site/src/pages/projects/{product}.astro`
   - Look for mockups, screenshots, landing pages

3. **Extract tokens**
   - Background, surface, border colours
   - Text hierarchy colours
   - Accent and semantic colours
   - Font families, weights, logo treatment

4. **Adjust for slide readability**
   - Secondary text often needs bumping brighter for slides vs app use
   - Test mentally: will this be legible at slide size in a PDF?

5. **Find screenshots**
   - Check `../cv-site/public/screenshots/{product}/`
   - Note what each shows and which slide it's best for

6. **Write the brand notes doc**
   - Follow the structure of existing brand docs (e.g. `pitch-brands/tensyl.md`)
   - Sections: Colour Tokens, Typography, Logo Treatment, Background Effects, Screenshots, Reference Docs
   - Leave Lessons Learned empty — it fills up through iteration

7. **Save to** `docs/reference/pitch-brands/{product}.md`
8. **Update** `docs/reference/pitch-deck-design-bible.md` brand list

---

## Key Principles (from 50+ rounds of Seedcorn iteration)

These are the hard-won rules. Violating any of them produces a worse deck:

1. **Every number must be sourced.** No figure goes in without author + year. Research first, cite inline, full refs on sources slide.

2. **Frame for investors.** "Why invest?" not "Why use this?" Every slide, every word.

3. **Emphasise substance, not framing.** "No app **adjusts**" not "**No app** adjusts". One or two words max.

4. **No jargon.** If a judge can't understand it without domain expertise, rewrite it. Citations serve plain language, not the other way round.

5. **No overclaiming.** If it can't survive scrutiny, cut it. "Working product" is strong. "4 products in 2 years" invites questions.

6. **No cringe.** "One person built all of this. Imagine what investment accelerates." — delete on sight.

7. **Use the product's own design system.** Never generic templates. The deck should look like the product.

8. **Iterate with variants.** A/B/C side by side beats 20 rounds of tweaking one version.

9. **The display font lies about numbers.** Always use the body font with lining-nums for data.

10. **Test at actual size.** What looks fine in dev tools may be invisible in the exported PDF.
