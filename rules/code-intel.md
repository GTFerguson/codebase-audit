# nkrdn — Code Knowledge Graph

nkrdn indexes code structure (symbols, relationships, inheritance) and project documentation. Use it to orient before reading source.

## When to Use

| Question type | Do this |
|---|---|
| **Design/conceptual** — "How does X work?", "Why is X designed this way?" | `nkrdn search "X" --source docs` → read the design doc → verify in code |
| **Structure/relationships** — "What depends on X?", "What would break if I changed X?", "What does X inherit?", "How is the code organized?" | `nkrdn lookup X` → then `usages`, `details`, `tree`, or `scope` as needed |
| **Location/implementation** — "Find where X is", "How is X implemented?" | Grep or Read directly — nkrdn has structure, not code |

## Commands

| Command | Purpose |
|---|---|
| `search "query"` | Search code symbols + documentation |
| `search "query" --source docs` | Search documentation only |
| `search "query" --source code` | Search code symbols only |
| `lookup <name>` | Find symbols by name (returns URIs) |
| `details <uri>` | Relationships, inheritance, containment for a symbol |
| `usages <uri>` | Everything that depends on a symbol |
| `scope <name>` | Explore a namespace or package (auto-detects) |
| `module <name>` | List contents of a module |
| `tree <uri>` | Full inheritance hierarchy |
| `stats` | Entity counts and graph overview |
| `context <filepath>` | Code structure + related docs for a file |

## Budget

1-3 nkrdn calls for orientation, then switch to Grep/Read for the rest. Spending more than ~25% of your turns on nkrdn calls means you're over-exploring the graph.
