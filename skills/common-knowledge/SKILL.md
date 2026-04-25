---
name: common-knowledge
description: Two-layer PROVEN research pipeline. Builds foundational cross-domain research in /home/gary/projects/common-knowledge/ first, then project-specific research in the current project's docs/reference/. Use when researching a topic that has both cross-domain foundations (theory, evidence, patterns) and project-specific application (target keywords, competitors, niche data). Manually invoked — does not fire automatically.
---

# Common Knowledge — Shared Research Pipeline

This skill extends `/proven-research` with a two-layer pipeline. Use it when researching a topic that has both cross-domain foundations and project-specific applications.

## The Two-Layer Model

```
Layer 1 — common-knowledge/          ← build this first
  Foundational, cross-domain research.
  Applies across projects.
  Stored at /home/gary/projects/common-knowledge/

Layer 2 — <project>/docs/reference/  ← build this second
  Project-scoped research.
  Built on Layer 1, wiki-links back to it.
  Stored in the current project's reference directory.
```

## When to put something in common-knowledge vs project reference

| Goes in common-knowledge | Goes in project reference |
|--------------------------|---------------------------|
| How Google's ranking systems work | Target keywords for this product's niche |
| Evidence on Core Web Vitals as a ranking factor | Competitor SERP analysis for this product |
| Search intent classification theory | Content gap analysis for this audience |
| Structured data schema standards | Specific pages to build for this product |
| E-E-A-T signal evidence | This product's programmatic SEO plan |
| Programmatic SEO as a pattern | Borough/neighbourhood keyword data (goodlet) |

If a finding would be equally useful in a completely different project, it belongs in common-knowledge.

## Research Pipeline

### Step 1 — Audit what already exists

```bash
nkrdn search "<topic>" --source docs
```

Check both common-knowledge and the current project's reference for existing coverage before researching. Avoid duplicating what's already there.

### Step 2 — Build the common layer first

Research foundational, cross-domain concepts using the PROVEN methodology (alphaxiv → PubMed → WebSearch). Write docs to `/home/gary/projects/common-knowledge/<cluster>/`.

Each doc must:
- Cover one coherent topic
- Have citations with author, year, journal, sample size where applicable
- Label evidence tiers (Tier 1 meta-analysis through Tier 5 practitioner opinion)
- Follow the standard PROVEN frontmatter format

Update `/home/gary/projects/common-knowledge/<cluster>/README.md` and the root `/home/gary/projects/common-knowledge/README.md` with a pointer to the new doc.

### Step 3 — Build the project layer second

Research product-specific aspects that build on the common layer. Write docs to `<project>/docs/reference/<cluster>/`.

Wiki-link back to common-knowledge where relevant:

```markdown
The ranking factor evidence base is at [[ranking-factors]].
Applying that to goodlet's target queries: ...
```

Update the project's reference README with a pointer.

### Step 4 — Commit common-knowledge

Commit the new docs to the common-knowledge repo so the history is clean and any agent pulling the repo gets the index pre-built:

```bash
cd /home/gary/projects/common-knowledge
git add -A
git commit -m "<topic cluster>: <one-line summary of what was added>"
```

One commit per research session or topic cluster. Do not batch unrelated topics into a single commit.

### Step 5 — Rebuild nkrdn index

After committing, rebuild so the workspace graph reflects the new knowledge:

```bash
nkrdn workspace rebuild common-knowledge
nkrdn workspace rebuild <current-project>
```

## File format

Standard PROVEN frontmatter — same as project reference docs:

```markdown
---
name: <topic name>
description: <one-line description for search relevance>
type: reference
tags: [<topic>, <subtopic>]
---

# <Topic>

<content with inline citations>

## References

- Author et al. (Year). Title. *Journal* Vol(Issue):Pages.
```

## Invocation

This skill is manually invoked — it does not fire automatically alongside `/proven-research`. Call it explicitly when you want the two-layer pipeline for a topic that warrants shared foundational research.
