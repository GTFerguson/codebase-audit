# Codebase Audit Framework

This repo contains prompts and methodology for AI-driven codebase audits. The prompts are in `prompts/` (audit methodology) and `playbook/` (execution methodology).

## Running an Audit

1. Start with `prompts/code-review.md` for a single codebase, or `prompts/code-audit.md` for multiple codebases
2. The prompts are self-guiding — they define what to do, how to do it, what to output, and quality criteria
3. Output goes into an `output/` directory (create as needed)

## Output Conventions

- YAML frontmatter on every document (title, created, updated, status, tags)
- Blank line before all tables (Obsidian requirement)
- Obsidian wiki-links `[[path/to/doc]]` for cross-references
- Callouts: `> [!CRITICAL]`, `> [!WARNING]`, `> [!TIP]`, `> [!NOTE]`, `> [!IMPORTANT]`
- Mermaid diagrams for architecture, data flow, dependency graphs, timelines
- Issue IDs: `CRIT-001`, `HIGH-001`, `MED-001`, `LOW-001`
- Code evidence always includes file path and line number
