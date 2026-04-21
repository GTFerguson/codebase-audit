---
name: handoff
description: Write a structured session-handoff brief to docs/plans/handoff/<slug>.md in the current project so the work can be resumed in a new session with zero rediscovery cost. Use when the user is ending a session mid-task, context is getting full, or they explicitly ask to "hand off" / "resume later" / "create a resumption prompt".
---

# Session Handoff

Persist a single self-contained brief that a fresh Claude session can be pointed at to continue the work immediately. No file searching, no guessing, no orientation lag.

## Where to write

Always write to `docs/plans/handoff/<slug>.md` inside the current project root, where `<slug>` is a short kebab-case name derived from the goal (e.g. `combat-equippeditem-refactor.md`). Create the directory if it doesn't exist. If a handoff already exists for this work, overwrite it (don't version them — the whole point is there's one current truth).

After writing, output *only* the absolute path in a single fenced block so the user can copy it. No conversational tail.

## Handoff lifecycle (this is the contract with the next session)

The handoff file is **persistent working memory for one task, across sessions**. Treat it like a plan doc's in-flight state:

- The resuming session reads this file first.
- The resuming session **updates it** as work progresses (strike through completed next-actions, add new gotchas).
- When the task ships, the resuming session **graduates** the durable knowledge (design decisions → architecture docs, research → reference docs) and then **deletes** `docs/plans/handoff/<slug>.md`.
- A handoff file existing means there is still in-flight work. No handoff → no task mid-air.

Include this contract verbatim in the "Contract" section of the brief so the next agent honours it.

## Operating rules

- **Write from memory of this session, not by re-exploring.** You already know what's going on. Don't re-grep the codebase to build the handoff — that wastes context on information you already have.
- **Be ruthlessly terse.** Every line must earn its place. If a detail doesn't affect the next action, cut it.
- **Absolute paths, not descriptions.** `/home/gary/projects/foo/bar.cpp:42` beats "that file we edited earlier".
- **Name the exact next step.** Not "continue the refactor" — "edit `combat_dispatch.cpp:222` to change `it->second` → `it->second.archetype_id`".
- **Don't recap what's committed.** Git has that. Only recap what's in the *working tree* or in the user's head.

## Required sections (in this order, skip ones that don't apply)

```markdown
---
title: <Short goal sentence>
created: <YYYY-MM-DD>
status: in-flight
---

# Resume: <one-line goal>

## Active plans
<List the plan docs that govern this work, from broadest to narrowest. These are the long-term anchors — the next session must read them to know what "done" looks like and what comes after.>
- **Roadmap**: `docs/plans/roadmap.md` — phase sequencing and current phase
- **Phase plan**: `docs/plans/<phase>.md` — what this phase adds and its open questions
- **Feature plan**: `docs/plans/feature-systems/<feature>.md` — the specific slice being implemented

Only list levels that exist and are relevant. Skip tiers that don't apply (e.g. no feature plan if the work is phase-level).

## Contract — how to use this file

This file is persistent working memory for one in-flight task. Procedure:

1. **Execute** — read this file first, then resume the Next actions below.
2. **Use as persistent reference while building** — update it as you go: tick off next-actions, add new gotchas, revise file lists. This is the single source of truth for "where is this work right now".
3. **Graduate on completion** — once the task ships, lift the durable knowledge out: design decisions → `docs/architecture/`, research findings → `docs/reference/`. Don't leave it stranded in here.
4. **Delete this file** — after graduation, `rm` the handoff. Its existence means work is still in the air; absence means done.

## Where we are
<1–3 sentences on the goal and phase. Link the plan doc if there is one.>

## Worktree / branch
- Path: <absolute path>
- Branch: <branch name>
- Last commit: <hash + subject>

## Shipped this session
- <bullet per commit, hash + one-line summary>

## In flight (uncommitted work)
<Only the edits already made to the working tree. File paths with line numbers. What was changed, not why.>

## Next actions (ordered)
1. <precise file:line edit or command>
2. ...

## Key design decisions
<Bullets on decisions made in conversation that aren't obvious from the code. E.g. "EquippedItem uses optional<container_id> instead of a separate map — slot-scoped lifetime matches item lifetime.">

## Files touched / to touch
<Grouped list with absolute paths.>

- Headers:
  - `/abs/path/a.hpp` — done
  - `/abs/path/b.hpp` — pending, add X
- Source:
  - ...
- Tests:
  - ...
- Docs:
  - ...

## Build & verify
```
<exact command(s) to configure/build the smallest useful target>
<exact command(s) to run the relevant tests>
```

## Gotchas encountered
<Only things that cost time this session and would cost time again. Skip if nothing meaningful.>
```

## What NOT to include

- Recap of the product vision or architecture already in CLAUDE.md.
- File contents — the new session can `Read` them. Just name them.
- Your reasoning or debate. Only the resolved decision.
- Warnings about things that didn't happen.
- Step-by-step build logs. One command per verification target.

## After writing

Output only the absolute path of the file you just wrote, in a single fenced block. Nothing else.
