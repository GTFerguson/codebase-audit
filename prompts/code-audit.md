# Codebase Audit — Orchestrator Prompt

## Overview

You are tasked with auditing one or more codebases. The goal is to identify architecture patterns, code duplication, security issues, and opportunities for consolidation or improvement.

**Critical Constraint:** All existing functionality must be preserved. Recommendations should enable improvement without feature loss.

## Related Documents

- **[[code-review]]** — Detailed code quality review methodology (use for deep-dive component reviews)

## Audit Scope

### Core Phases (Required)

- **Phase 1:** Individual codebase audits with functionality inventory
- **Phase 2:** Cross-codebase pattern analysis (if auditing multiple codebases)
- **Phase 3:** Framework design recommendations (if consolidation applies)

### Extended Phases (Optional)

- **Phase 4:** Code quality review and technical debt assessment (see [[code-review]])
- **Phase 5:** Future improvements backlog

## Workspace Setup

Before starting, establish:

- **Source code location** — Where the target codebase(s) live (read-only)
- **Output location** — Where audit documentation will be written
- **Codebases to audit** — List each codebase with a short description
- **Exclusions** — Any directories or codebases to skip

---

## Phase 1: Individual Codebase Audits

For each codebase, perform a systematic audit. The instructions below can be delegated as independent tasks that run in parallel.

### Per-Codebase Audit Task

```
## Task: Audit {CODEBASE_NAME}

**Context:** Part of a codebase audit to identify patterns, issues, and improvement opportunities.

**Source:** {SOURCE_PATH}

**Instructions:**
1. Explore the complete directory structure
2. Identify main components, modules, and their responsibilities
3. Document architecture patterns (pipelines, data flow, config management)
4. Identify key abstractions and interfaces
5. Note hardcoded values vs configurable options
6. List dependencies and technologies used
7. Identify code suitable for generalization or extraction
8. Create a complete functionality inventory (all features, endpoints, capabilities)

**Output:** Create {OUTPUT_PATH}/{codebase}/audit-report.md with:
- Directory structure overview
- Functionality inventory (complete list of features/capabilities)
- Architecture patterns identified
- Key components and responsibilities
- Configuration patterns
- Dependencies
- Similarities to other codebases (if applicable)
- Generalization opportunities
- Code snippets illustrating key patterns

**Rules:**
- Read-only access to source code
- Do not modify existing code
```

### Audit Report Structure

Each audit report should contain:

```markdown
---
title: "Audit Report — {Codebase Name}"
created: {date}
updated: {date}
status: draft
tags: [audit, {codebase}]
---

# {Codebase Name} Audit Report

## Overview
{What it is, what it does, who uses it.}

## Tech Stack
| Layer | Technology |
|-------|------------|

## Directory Structure
{Annotated tree showing purpose of each area.}

## Functionality Inventory
{Complete list of features, endpoints, capabilities.}

## Architecture Patterns
{Pipeline patterns, data flow, config management, error handling, etc.}

## Key Components
| Component | Location | Responsibility | Lines |
|-----------|----------|----------------|-------|

## Configuration Patterns
{What is hardcoded vs configurable. Where configs live.}

## Dependencies
{External services, libraries, infrastructure.}

## Generalization Opportunities
{What could be extracted, shared, or consolidated.}

## Preliminary Issues
{Obvious problems spotted during the scan — security, quality, testing gaps.}
```

### Key Focus Areas

When auditing, pay special attention to:

1. **Pipeline Patterns** — How is data processed? What are the stages? How does data flow between them?
2. **Configuration Management** — What is hardcoded vs configurable? Where are configs stored?
3. **Authentication & Security** — How are users authenticated? Are credentials secure?
4. **Data Flow** — Input sources, intermediate storage, output formats, event patterns
5. **Domain-Specific Logic** — What is unique to this codebase vs what could be shared?
6. **External Integrations** — What services does it talk to and how?

---

## Phase 2: Cross-Codebase Pattern Analysis

After all individual audits are complete, analyse patterns across codebases.

```
## Task: Cross-Codebase Pattern Analysis

**Context:** Individual audits complete. Identify common patterns for improvement or consolidation.

**Input:** All audit reports from Phase 1.

**Instructions:**
1. Read all audit reports
2. Identify common patterns across codebases:
   - Pipeline / processing patterns
   - Data flow patterns
   - Configuration management approaches
   - Error handling strategies
   - Logging and monitoring
   - Storage and persistence patterns
   - API patterns
   - Authentication approaches
3. Identify code duplication and near-duplicates
4. Map domain-specific vs domain-agnostic components
5. Identify configuration points that vary between codebases

**Output:** Create {OUTPUT_PATH}/analysis/cross-codebase-analysis.md with:
- Common patterns matrix (pattern × codebase)
- Code duplication inventory with LOC estimates
- Domain-specific vs generic component mapping
- Configuration variability analysis
- Dependency overlap analysis
- Key insights and recommendations
```

---

## Phase 3: Framework Recommendations

Design recommendations for consolidation or improvement based on findings.

```
## Task: Framework / Improvement Recommendations

**Context:** Pattern analysis complete. Design recommendations.

**Input:**
- Cross-codebase analysis
- All individual audit reports

**Instructions:**
1. Design improvement recommendations that:
   - Address identified duplication and inconsistencies
   - Use configuration over code for variant behaviour
   - Provide clear extension points for future growth
   - Reduce maintenance burden
2. Define core abstractions to extract
3. Propose a migration path from current state
4. Identify quick wins vs long-term refactoring
5. Assess risks

**Output:** Create {OUTPUT_PATH}/proposals/ with:
- executive-pitch.md — Business case with ROI
- framework-proposal.md — Technical proposal with:
  - Architecture diagram (Mermaid)
  - Core abstractions and interfaces
  - Configuration schema
  - Extension patterns
  - Migration roadmap
  - Quick wins (low effort, high impact)
  - Risk assessment
```

---

## Documentation Standards

> [!IMPORTANT]
> All output documents must follow Obsidian-compatible formatting.

### Required

- **YAML frontmatter** on every document (title, created, updated, status, tags)
- **Blank line before all tables** (Obsidian rendering requirement)
- **Cross-references** using `[[wiki-links]]`
- **Callouts** for emphasis: `> [!NOTE]`, `> [!WARNING]`, `> [!IMPORTANT]`, `> [!CRITICAL]`, `> [!TIP]`
- **Mermaid diagrams** for architecture, data flow, dependency graphs
- **Code references** include file path and line number: `[name](path/file.ext:line)`

### File Naming

- kebab-case: `cross-codebase-analysis.md`
- Numbers for ordering: `01-auth-system.md`

---

## Success Criteria

The audit is successful when:

1. Each codebase has a comprehensive audit report with functionality inventory
2. Common patterns are clearly documented with evidence
3. Improvement recommendations are realistic and actionable
4. Quick wins are identified with effort estimates
5. Risks are assessed for each recommendation
6. A reader unfamiliar with the codebases can understand the current state and proposed improvements
