# Codebase Audit Framework

A prompt-driven framework for systematic codebase audits and remediation. Point it at a codebase, and the prompts guide the AI through a complete audit — from initial scan to component-level security review to actionable remediation plan.

## Structure

```
prompts/                           # Core audit methodology
├── code-audit.md                  # Multi-codebase audit orchestrator
│                                    Phase 1: Individual audits
│                                    Phase 2: Cross-codebase analysis
│                                    Phase 3: Consolidation proposals
└── code-review.md                 # Systematic code quality review
                                     Phase 1: Discovery & analysis plan
                                     Phase 2: Output structure
                                     Phase 3: Per-component 4-dimension review
                                     Phase 4: Integration analysis
                                     Phase 5: Recommendations
                                     Phase 6: Executive synthesis

playbook/                          # Execution methodology (post-audit)
├── README.md                      # Quick start
├── architecture.md                # 3-layer orchestrator hierarchy
├── quality-gates.md               # Checkpoint definitions
├── context-management.md          # Managing AI context across sessions
├── docs-protocol.md               # State file maintenance
└── prompts/                       # Task-level prompt templates
    ├── master-orchestrator-init.md
    ├── phase-orchestrator-init.md
    ├── security-fix.md
    ├── package-extraction.md
    ├── component-migration.md
    ├── test-generation.md
    └── context-restore.md
```

## Two Layers

### 1. Audit (`prompts/`)

**`code-audit.md`** — For auditing one or more codebases. Identifies architecture patterns, duplication, and consolidation opportunities. Produces audit reports, cross-codebase analysis, and improvement proposals.

**`code-review.md`** — For deep-diving into a single codebase. Systematic component-by-component review covering security, SOLID principles, code smells, performance, and error handling. Produces scored component reviews, issue tracking by severity, integration analysis, and a remediation roadmap.

### 2. Execution (`playbook/`)

Once you have audit findings, the playbook manages the remediation work. It uses a hierarchical orchestrator pattern (Master → Phase → Task) with quality gates, state management across sessions, and specific prompt templates for common tasks (security fixes, package extraction, migration, test generation).

## Usage

### Quick audit of a single codebase

Use `prompts/code-review.md`. Point the AI at your codebase, and the prompt guides it through discovery, component reviews, integration analysis, and synthesis.

### Multi-codebase audit

Use `prompts/code-audit.md`. Define your codebases, and the prompt orchestrates individual audits, cross-codebase comparison, and consolidation recommendations.

### Remediation program

After an audit, use the `playbook/`. Initialize a Master Orchestrator, define phases from the audit findings, and use the task prompts to execute fixes, extractions, and migrations.

## Origin

These prompts were battle-tested across multiple real codebase audits, producing hundreds of pages of structured findings across 6 codebases. The methodology, scoring rubrics, and output structures are proven — they're extracted and generalized here for reuse.
