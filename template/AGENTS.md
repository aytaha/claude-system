# AGENTS.md — Organization Map

Who works here, what each person owns, and how work moves between them. Operating rules are in `CLAUDE.md`. Each agent's detailed behavior is in `.claude/agents/`.

```text
Agent  = worker                        .claude/agents/
Part   = project knowledge / spec      .claude/parts/
Task   = work request                  (the current prompt)
Memory = learned experience            .claude/agent-memory/
```

## Teams and agents

| Team | Agent | File | Owns |
|---|---|---|---|
| Coordination | Lead | `agents/lead.md` | Routing, planning, choosing workers and reviewers, closing the loop |
| Product | Product Agent | `agents/product/product-agent.md` | Requirements, scope, acceptance criteria, user value |
| Architecture | Architect | `agents/architecture/architect.md` | System structure, boundaries, cross-Part interfaces, technical decisions |
| Frontend | Frontend Engineer | `agents/frontend/frontend-engineer.md` | UI, client state, client-side integration, accessibility |
| Backend | Backend Engineer | `agents/backend/backend-engineer.md` | Services, APIs, business logic, server-side integrations |
| AI | AI Engineer | `agents/ai/ai-engineer.md` | Models, prompts, agents, retrieval, evaluation |
| Database | Database Engineer | `agents/database/database-engineer.md` | Schema, migrations, queries, data integrity |
| QA | QA Engineer | `agents/qa/qa-engineer.md` | Test strategy, test coverage, verification of done |
| Security | Security Engineer | `agents/security/security-engineer.md` | Threats, authn/authz, secrets, input trust boundaries |
| Resilience | Resilience Engineer | `agents/resilience/resilience-engineer.md` | Failure behavior, timeouts, retries, recovery, degradation |
| DevOps | DevOps Engineer | `agents/devops/devops-engineer.md` | Build, CI/CD, environments, deployment, observability plumbing |

Part directories: `product`, `architecture`, `frontend`, `backend`, `ai`, `database`, `infrastructure` (DevOps), `shared` (cross-cutting, owned by Architect unless a Part says otherwise).

Bundled playbook skills: `sql-performance` (Database Engineer), `resilience-audit` (Resilience Engineer).

## Skills

Each agent's `skills:` header lists the skills it loads. Baseline for engineering agents: `ponytail`, `systematic-debugging`, `verification-before-completion`. Implementation agents (frontend, backend, AI, database) add `test-driven-development`. `using-superpowers` is deliberately not assigned: the Lead orchestrates, not a meta-skill.

Optional third-party skills (install, then add to the agent's `skills:` list):

| Agent | Skills |
|---|---|
| Product | `product-manager` |
| Architect | `architecture-decision-records`, `api-design-principles`, `c4-architecture` |
| Frontend | `web-design-guidelines` |
| Backend | `api-design-principles` |
| AI | `rag-implementation`, `ai-evals`, `prompt-engineering` |
| QA | `playwright`, `api-test-suite-builder`, `k6-perf-test-website` |
| Security | `security-threat-model`, `owasp-security` |
| Resilience | `observability-engineer`, `qa-observability`, `incident-responder` |
| DevOps | `github-actions`, `docker-production` |

Stack-specific, add only when the project uses the technology: React/Next.js skills (Frontend), Postgres/Supabase skills (Database), Terraform skills (DevOps).

## When to involve supporting agents

| Situation | Involve |
|---|---|
| Requirements unclear or scope changing | Product Agent |
| Change crosses Part boundaries or alters a public interface | Architect |
| Schema, migration, or query-performance change | Database Engineer |
| Auth, secrets, user input, external input, permissions | Security Engineer |
| New network call, dependency, queue, retry, or long-running work | Resilience Engineer |
| New service, env var, build step, or deployment change | DevOps Engineer |
| LLM, prompt, retrieval, or evaluation change | AI Engineer |
| Any change that claims to be done | QA Engineer |

## Handoff

A handoff between agents always includes:

1. **Task:** what is being asked, in one or two sentences.
2. **Parts:** which Part documents apply, or "none exists yet".
3. **State:** what was done, what was verified (with evidence), what remains.
4. **Open questions / risks.**

No handoff says "done" without evidence.

## Review

- The **primary agent** does the work and runs its own checks.
- **QA** verifies behavior against the definition of done.
- **Domain reviewers** are added by the Lead based on the table above (e.g. Security for auth changes).
- The reviewer reports findings. The primary agent fixes them. Disagreements go to the Lead, then to the user.
- After review: the primary agent updates the Part if its current state changed, and its own `MEMORY.md` if a reusable lesson was learned.
