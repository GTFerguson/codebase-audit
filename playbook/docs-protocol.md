---
title: Documentation Protocol
created: 2026-03-16
updated: 2026-03-16
status: active
tags: [playbook, documentation, protocol]
---

# Documentation Protocol

## State File Maintenance

State files are the source of truth for project progress. They must be kept current.

### When to Update

- After completing a task
- After a phase status change
- When a blocker is encountered or resolved
- When a decision is made
- At the start and end of every session

### Update Rules

1. Update the `updated` date in frontmatter
2. Move completed items from "Active" to "Completed"
3. Add new items to "Next Actions"
4. Record decisions with date and rationale
5. Never delete history — move to a "Completed" or "Archive" section

## Output Document Standards

All audit and review output follows Obsidian-compatible markdown:

- **YAML frontmatter**: title, created, updated, status, tags
- **Blank line before tables**: Required for Obsidian rendering
- **Wiki-links**: `[[path/to/doc]]` for cross-references
- **Callouts**: `> [!NOTE]`, `> [!WARNING]`, `> [!IMPORTANT]`, `> [!CRITICAL]`
- **Mermaid diagrams**: For architecture, data flow, timelines
- **Code references**: `path/to/file:line`
- **kebab-case filenames**: `cross-codebase-analysis.md`
- **Numbered prefixes for ordering**: `01-auth-system.md`

## See Also

- [[README]] — Playbook overview
- [[context-management]] — State file usage
