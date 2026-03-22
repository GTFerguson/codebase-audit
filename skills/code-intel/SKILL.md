---
name: code-intel
description: Enforce nkrdn usage for code understanding. Use when an agent should orient with the knowledge graph before diving into grep/read. Invoke with /code-intel to remind the agent to use nkrdn.
---

# Use nkrdn for Code Intelligence

Stop grepping blindly. Use the nkrdn knowledge graph to orient first.

## Rules

1. **Before reading source files**, run `nkrdn lookup <name>` or `nkrdn search "<query>"` to understand the symbol's place in the codebase -- what it relates to, what depends on it, where it sits in the hierarchy.

2. **Before making changes**, run `nkrdn usages <uri>` to understand what would be affected. Don't discover callers by grepping -- the graph already knows.

3. **For design questions**, run `nkrdn search "<topic>" --source docs` to find relevant documentation before forming opinions.

4. **For cross-repo work**, the workspace graph at `~/.nkrdn/workspace/graph.ttl` has all repos indexed. Run `nkrdn workspace diff --since 7d` to see recent changes across projects, or query symbols that span repos.

## Available Commands

```
nkrdn lookup <name>              # find symbols by name
nkrdn search "<query>"           # search symbols + docs
nkrdn search "<query>" --source docs   # docs only
nkrdn details <uri>              # relationships, inheritance
nkrdn usages <uri>               # what depends on this
nkrdn scope <name>               # explore a namespace/package
nkrdn tree <uri>                 # inheritance hierarchy
nkrdn context <filepath>         # structure + related docs for a file
nkrdn workspace diff --since 7d  # cross-repo changes
nkrdn workspace list             # indexed repos
```

## Budget

1-3 nkrdn calls to orient, then switch to Grep/Read for implementation detail. The graph has structure and relationships, not source code -- use it for the map, then read the territory.
