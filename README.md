# Claude System — an AI software organization for any project

A reusable framework that makes Claude Code work like a small engineering organization: a **Lead** routes each task to **specialist agents**, and they work with durable **project knowledge** and their own **learned experience**.

Everything project-agnostic lives in `template/`. Nothing in it describes a specific product.

## Core concepts

```text
Agent  = worker (a role with responsibilities)     .claude/agents/
Part   = knowledge about a real project component  .claude/parts/
Task   = a temporary work request                  (the current prompt)
Memory = reusable lessons one agent has learned    .claude/agent-memory/
```

A worker's context for a task is:

```text
CLAUDE.md + <agent>.md + relevant Part doc(s) + <agent>/MEMORY.md + task
```

Flow:

```text
User request → Lead → identify Parts → identify agents → load context
→ work → review → update Part (if the architecture changed) → update Memory (if a lesson was learned)
```

## What's in `template/`

```text
template/
├── CLAUDE.md                 organization-wide rules, Part schema, memory format, learning promotion
├── AGENTS.md                 team map: roles, when to involve whom, handoff, review, skills per agent
└── .claude/
    ├── agents/               11 agents (Claude Code subagents, with name/description/skills frontmatter)
    │   ├── lead.md
    │   ├── product/  architecture/  frontend/  backend/  ai/
    │   └── database/  qa/  security/  resilience/  devops/
    ├── parts/                empty team folders; Parts are discovered from the real project
    │   └── product/ architecture/ frontend/ backend/ ai/ database/ infrastructure/ shared/
    ├── agent-memory/         one empty MEMORY.md per agent
    ├── rules/                always-loaded shared standards
    │   ├── verification.md       evidence before claims, controls, independent sources
    │   ├── change-discipline.md  smallest correct change, root cause, one source of truth
    │   └── proactive-behavior.md what to notice and act on without being told
    └── skills/
        ├── sql-performance/      measured DB/query optimization playbook (Database Engineer)
        └── resilience-audit/     full production-resilience audit playbook (Resilience Engineer)
```

Every agent file has the same sections: Role, Mission, Responsibilities, Scope, Required Context, Workflow, Collaboration, Proactive Behavior, Restrictions, Review Requirements, Definition of Done, Memory Rules.

### Where each kind of knowledge goes

| Location | Holds |
|---|---|
| `CLAUDE.md` | Only critical rules that apply to every agent |
| `AGENTS.md` | Who does what, and how they coordinate |
| `.claude/agents/` | How each worker behaves, and what it's responsible for noticing |
| `.claude/parts/` | Current state of real project components (living specs) |
| `.claude/agent-memory/` | One agent's reusable lessons |
| `.claude/rules/` | Standards shared across agents |
| `.claude/skills/` | Reusable procedures |

Learning moves upward: `MEMORY.md` → `.claude/rules/` if it applies broadly → `CLAUDE.md` if it's critical for everyone.

## Required plugins / skills

Agents reference these in their `skills:` frontmatter. Install them in Claude Code first:

| Skill | Source | Used by |
|---|---|---|
| `ponytail:ponytail` | ponytail plugin | all agents |
| `superpowers:systematic-debugging`, `superpowers:verification-before-completion` | superpowers plugin | all engineering agents |
| `superpowers:test-driven-development` | superpowers plugin | frontend, backend, AI, database |
| `superpowers:writing-plans`, `superpowers:dispatching-parallel-agents`, `superpowers:subagent-driven-development` | superpowers plugin | lead |
| `superpowers:brainstorming` | superpowers plugin | product |
| `impeccable` | impeccable | frontend |
| `security-review` | built into Claude Code | security |

Optional third-party and stack-specific skills are listed in `template/AGENTS.md` → Skills.

## Applying it to a project

Clone this repo anywhere on your machine:

```bash
git clone https://github.com/aytaha/claude-system.git
```

Then, from your project, tell Claude:

> Apply the Claude system from `<path-to>/claude-system` to this project.

Replace `<path-to>` with the directory you cloned into.

### Instructions for Claude (follow these when asked to apply the system)

1. **Read this README and every file in `template/`** before copying anything.
2. **Inspect the target project first.** Look for an existing `CLAUDE.md`, `AGENTS.md`, `.claude/agents/`, `.claude/rules/`, and `.claude/skills/`.
3. **Never overwrite existing files.** If `CLAUDE.md` or `AGENTS.md` exists, merge the system's content into it, or ask the user. **On Windows and macOS, file names are case-insensitive**, so an existing `Agents.md` *is* `AGENTS.md`. Check before writing.
4. **Existing project agents:** if one overlaps a template agent, merge its content into that agent (long playbooks become a skill owned by that agent) rather than keeping two agents for one role. Ask if unsure.
5. **Copy** `template/CLAUDE.md`, `template/AGENTS.md`, and `template/.claude/*` into the project root.
6. **Adapt to the real stack. Don't invent anything:**
   - If the project has a principles document, record its path in `AGENTS.md`, and in `architect.md` Required Context.
   - Drop skills and agents that clearly don't apply (e.g. no database → keep the agent but don't add DB-specific skills).
   - Check that every skill in the agents' `skills:` frontmatter is actually installed. Report any that are missing. Don't install third-party skills without the user's approval.
7. **Leave `.claude/parts/` empty.** Parts are created only when a task touches a component that genuinely exists, using the schema in `CLAUDE.md` §5. Don't generate sample or guessed Parts. The user may separately ask to "discover Parts". Then inspect the repository and write Parts only for what the code shows, marking unknowns `Unknown` / `TODO`.
8. **Leave `MEMORY.md` files empty.** Memory is earned, not seeded.
9. **Verify:** list the final tree and confirm every agent, memory file, and rule exists. Report what was merged, skipped, or needs the user's decision.

## Customizing

- **Add an agent:** create `.claude/agents/<team>/<name>.md` with the same sections and frontmatter, add `.claude/agent-memory/<name>/MEMORY.md`, and add a row to `AGENTS.md`.
- **Add a rule:** keep it short. Everything in `.claude/rules/` is loaded into every session.
- **Improve the system itself:** when a project teaches a lesson that is **generic**, promote it back into `claude-system/template` (via a PR if you forked it) so future projects start with it. Keep project-specific facts out of this folder.
