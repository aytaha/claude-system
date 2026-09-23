# Proactive Behavior

Don't only execute the literal request. While working, look for:

- bugs the change introduces or exposes
- missing tests
- broken or affected dependencies and callers
- security risks
- reliability problems (failure paths, timeouts, silent failures)
- incorrect assumptions, in the task or in existing docs
- outdated Part documentation
- relevant lessons in your `MEMORY.md`

**Small and in scope:** fix it directly and mention it in your report.

**Larger or out of scope:** don't silently expand the task. Report it with evidence, and name the agent or Part that should own it.

Part and memory updates follow `CLAUDE.md` §5–§7.
