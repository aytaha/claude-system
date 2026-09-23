# CLAUDE.md — Organization Operating Rules

This repository is worked on by an AI software organization: specialized agents, each with a role, coordinated by a Lead Agent. This file holds the rules that apply to **every** agent on **every** task. Keep it short. Detail belongs in the files listed below.

## 1. Knowledge map — where each kind of knowledge lives

| Location | Holds | Changes when |
|---|---|---|
| `CLAUDE.md` | Organization-wide operating rules (this file) | A lesson is critical for every agent |
| `AGENTS.md` | Team map: roles, ownership, handoffs, review | Roles or collaboration model change |
| `.claude/agents/` | How each worker behaves | A role's behavior changes |
| `.claude/parts/` | Durable knowledge about real project components | The project's architecture or current state changes |
| `.claude/agent-memory/` | Reusable experience learned by one agent | That agent learns a reusable lesson |
| `.claude/rules/` | Reusable engineering rules and standards | A lesson applies across agents |
| `.claude/skills/` | Reusable procedures / capabilities | A repeatable procedure is worth packaging |

Do not duplicate information across these. Link instead.

If the project has its own principles document (constitution, engineering standards), it takes precedence over anything here. Record its path in `AGENTS.md`.

## 2. Core concepts

```text
Agent  = worker          (.claude/agents/)
Part   = project knowledge / specification   (.claude/parts/)
Task   = temporary work request              (not stored)
Memory = learned experience                  (.claude/agent-memory/)
```

A task is executed with this context:

```text
CLAUDE.md + <agent>.md + relevant Part doc(s) + <agent>/MEMORY.md + current task = worker context
```

## 3. Standard flow

```text
User request → Lead Agent → identify Part(s) → identify Agent(s) → load context
→ perform work → review → update Part (if architecture/current state changed)
→ update Memory (if a reusable lesson was learned)
```

## 4. Global rules

1. **Inspect, don't assume.** If the repository can answer a question, read it. Never invent architecture, components, fields, APIs, or requirements.
2. **Stay in scope.** Work within your role. If a change crosses into another agent's area, involve that agent (see `AGENTS.md`).
3. **Evidence before claims.** Do not report something as done, fixed, or passing without running the check that shows it. Report failures and skipped steps as they are.
4. **Unknown is a valid answer.** Write `Unknown`, `Not yet defined`, or `TODO:` rather than guessing.
5. **Confirm before irreversible or outward-facing actions** (deleting data, force-pushing, deploying, calling paid or external services) unless explicitly authorized.
6. **Never write secrets** (keys, passwords, tokens, personal data) into Parts, Memory, rules, or logs.
7. **Smallest correct change.** No speculative abstractions or scaffolding for later.

## 5. Parts — living specifications

Whenever a meaningful project part (component, domain, subsystem, feature area, or infrastructure area) **genuinely exists** in the project, it gets its own document:

```text
.claude/parts/<team>/<part-name>.md
```

Teams: `product`, `architecture`, `frontend`, `backend`, `ai`, `database`, `infrastructure`, `shared`.

- Parts are **discovered from the real project**, never predefined or invented.
- A Part describes the **current known state**, not a wish list.
- Before substantial work, agents read the relevant Part(s). If none exists for an area being changed substantially, create it from what the repository shows.
- **Update the Part** when a change affects architecture, interfaces, dependencies, data model, invariants, ownership, failure behavior, security, resilience, or an important implementation decision. Small details that don't affect future understanding need no update.

Part schema (fill what is known; mark the rest `Unknown` / `Not yet defined` / `TODO`):

```markdown
# <Part Name>

## Purpose

## Ownership

Primary team:
Primary agent:

Supporting agents:

## Code Locations

## Responsibilities

## Does Not Own

## Architecture

## Public Interfaces

## Data / State

## Dependencies

## Invariants

## Failure Cases

## Testing Requirements

## Security Requirements

## Resilience Requirements

## Observability Requirements

## Current Implementation

## Known Issues

## Related Parts

## Definition of Done
```

## 6. Agent memory

Each agent owns `.claude/agent-memory/<agent>/MEMORY.md`. Memory is **not** project documentation — it is reusable experience.

Record a lesson as:

```markdown
### LESSON-XXX — Short title

Mistake:

Root cause:

Correct approach:

Prevention rule:
```

Number lessons sequentially per agent. Do **not** record trivial one-time failures, temporary debugging info, task status, secrets/credentials, or personal sensitive information.

## 7. Learning promotion

```text
Agent-specific lesson
        ↓
Agent MEMORY.md
        ↓
Applies broadly?  → yes → .claude/rules/<topic>.md
        ↓
Critical organization-wide rule?  → yes → CLAUDE.md
```

When promoting, move the rule and leave a one-line pointer in the source memory — don't keep two copies.
