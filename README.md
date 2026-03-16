# Claude Setup

Portable Claude Code configuration — rules, prompts, and execution playbook. Clone onto any machine, symlink into `~/.claude/`, and everything works.

## Structure

```
rules/                             # Global coding standards
├── code-comments.md               # WHY not WHAT, no marker comments
├── code-intel.md                  # nkrdn code knowledge graph usage
├── markdown-formatting.md         # Obsidian-compatible formatting
├── proven-documentation.md        # PROVEN principles for reference docs
└── test-driven-debugging.md       # Test-first debugging methodology

prompts/                           # Self-guiding methodology prompts
├── code-review.md                 # /review-codebase — systematic quality review
└── codebase-docs.md               # /document-codebase — architecture docs generator

playbook/                          # Execution methodology (post-review)
├── README.md                      # Quick start
├── architecture.md                # 3-layer orchestrator hierarchy
├── quality-gates.md               # Checkpoint definitions
├── context-management.md          # AI context across sessions
├── docs-protocol.md               # State file maintenance
└── prompts/                       # Task-level templates
    ├── security-fix.md
    ├── package-extraction.md
    ├── component-migration.md
    ├── test-generation.md
    ├── master-orchestrator-init.md
    ├── phase-orchestrator-init.md
    └── context-restore.md
```

## Installation

```bash
git clone git@github.com:GTFerguson/claude-setup.git ~/.claude-setup

# Symlink rules into Claude Code global rules
ln -s ~/.claude-setup/rules/* ~/.claude/rules/
```

## Rules

| Rule | What It Enforces |
|------|-----------------|
| [code-comments](rules/code-comments.md) | Comments explain WHY not WHAT, no internal references, self-contained |
| [code-intel](rules/code-intel.md) | nkrdn knowledge graph — when and how to query code structure |
| [markdown-formatting](rules/markdown-formatting.md) | Obsidian compatibility — blank lines before tables, callouts, mermaid |
| [proven-documentation](rules/proven-documentation.md) | PROVEN principles — provenance, research-first, verifiable, evidence-tiered |
| [test-driven-debugging](rules/test-driven-debugging.md) | Write tests to diagnose bugs, not read-and-guess |

## Prompts

| Prompt | Skill Name | Purpose |
|--------|------------|---------|
| [code-review](prompts/code-review.md) | `/review-codebase` | Systematic code quality review — security, SOLID, performance, scoring, remediation |
| [codebase-docs](prompts/codebase-docs.md) | `/document-codebase` | Generate architecture docs in standard `docs/` structure from codebase scan |

## Playbook

Orchestrated execution for acting on review findings. Manages multi-phase remediation across sessions with quality gates and state tracking.

| Task Prompt | Purpose |
|-------------|---------|
| [security-fix](playbook/prompts/security-fix.md) | Fix vulnerability with checklist and common patterns |
| [package-extraction](playbook/prompts/package-extraction.md) | Extract shared code into reusable package |
| [component-migration](playbook/prompts/component-migration.md) | Migrate component to shared packages |
| [test-generation](playbook/prompts/test-generation.md) | Generate tests for a component |
| [context-restore](playbook/prompts/context-restore.md) | Restore context after session break |
