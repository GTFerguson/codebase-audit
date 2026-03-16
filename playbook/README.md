---
title: Execution Playbook
created: 2026-03-16
updated: 2026-03-16
status: active
tags: [playbook, orchestration, execution]
---

# Execution Playbook

Methodology for AI-assisted execution of audit remediation, migrations, and refactoring work. Uses a hierarchical orchestrator pattern to manage complex multi-phase projects.

## When to Use

Use this playbook when moving from audit findings to implementation — fixing security issues, extracting shared packages, migrating components, or running a multi-phase remediation program.

## Architecture

The playbook uses a 3-layer orchestrator hierarchy:

```
Master Orchestrator
├── Phase Sub-Orchestrator (Phase 0: Security)
│   ├── Task: Fix CRIT-001
│   ├── Task: Fix CRIT-002
│   └── Task: Fix CRIT-003
├── Phase Sub-Orchestrator (Phase 1: Refactoring)
│   ├── Task Orchestrator: Extract shared auth package
│   │   ├── Subtask: Design API
│   │   ├── Subtask: Implement
│   │   └── Subtask: Test
│   └── Task: Decompose god object
└── Phase Sub-Orchestrator (Phase 2: Migration)
    └── ...
```

See [[architecture]] for full details.

## Prompt Templates

| Prompt | When to Use |
|--------|-------------|
| [[prompts/master-orchestrator-init]] | Starting a new remediation program |
| [[prompts/phase-orchestrator-init]] | Beginning a new phase within the program |
| [[prompts/security-fix]] | Fixing a specific security vulnerability |
| [[prompts/package-extraction]] | Extracting shared code into a reusable package |
| [[prompts/component-migration]] | Migrating a component to use shared packages |
| [[prompts/test-generation]] | Generating tests for a component |
| [[prompts/context-restore]] | Restoring context after a session break |

## Quality Gates

See [[quality-gates]] for checkpoint definitions and pass criteria.

## Quick Start

1. Complete a code audit/review first (see `prompts/code-audit.md` and `prompts/code-review.md`)
2. Create an implementation plan from the findings
3. Initialize the Master Orchestrator using [[prompts/master-orchestrator-init]]
4. Let it decompose the work into phases and tasks
5. Execute tasks using the appropriate prompt templates
6. Validate against quality gates at each checkpoint

## See Also

- [[architecture]] — Orchestrator hierarchy and delegation rules
- [[quality-gates]] — Checkpoint definitions and pass criteria
- [[context-management]] — Managing AI context across sessions
- [[docs-protocol]] — State file maintenance
