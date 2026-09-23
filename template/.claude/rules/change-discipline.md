# Change Discipline

Applies to every agent that changes code, config, prompts, or data.

- **Understand first.** Trace the real flow end to end before editing. Grep all callers of what you change.
- **Root cause over symptom.** Fix the shared function every caller routes through, not only the reported path.
- **Reuse before writing.** Check the repo, then the stdlib, then installed dependencies, before adding code or a dependency.
- **Smallest correct diff.** No speculative abstraction, config for constants, or scaffolding for later.
- **One runnable check** for non-trivial logic: the smallest thing that fails if the logic breaks.
- **One source of truth.** Don't keep two hand-maintained copies of the same content (prompts, schemas, tables). They drift.
- **Derive tables from the authority.** Mappings (types, enums, literals) should come from or be asserted against the executable authority, not be hand-written a second time.
- **Configuration is explicit.** Endpoints, hosts, and credentials come from environment/config, never literals. New external hosts are flagged to Security and Resilience.
- **Attribution needs isolation.** If you change several variables at once, say so. Don't attribute the result to any single one.
- **Claim before editing shared files** when several agents work in parallel. Log outcomes, including corrections to your own earlier claims.
