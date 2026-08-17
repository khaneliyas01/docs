# Agents

We run a fleet of opencode agents. Each is a specialist that gets delegated
work by the lead agent (or invoked directly).

## The cast

| Agent | Role | Mode | Model |
| ----- | ---- | ---- | ----- |
| `lead` | Orchestrator: plans, delegates, synthesizes | primary | qwen3.8:27b-mlx |
| `coder` | Implementation: writes/edits code, builds, tests | subagent | qwen3-coder:30b |
| `reviewer` | Read-only review: correctness, security, style | subagent | qwen3.8:27b-mlx |
| `ops` | DevOps: deploy, CI/CD, env, infra, release | subagent | qwen3.8:27b-mlx |
| `docs` | **Writes this knowledge base** after tasks | subagent | qwen3.8:27b-mlx |

## Skills

| Skill | Purpose |
| ----- | ------- |
| `planning` | Decompose tasks, pick subagents, sequence work |
| `general-coding` | Read conventions, minimal edits, build/lint/test |
| `code-review` | Severity-grouped review reports with go/no-go |
| `devops` | Deploy, CI/CD, secrets, hosting |
| `customize-opencode` | Editing opencode's own config/agents/skills |

## How delegation works

- The **lead** is the default entry point. It plans, then delegates to
  `coder` / `reviewer` / `ops` / `docs` as appropriate.
- Independent sub-tasks run as **parallel background subagents**.
- After a task completes, **the `docs` agent is always called** to record the
  outcome here (see [Workflow](workflow.md)).

## The `docs` agent

The newest member. It is the guardian of this site:

- Knows the layout of the `docs` repo (`~/docs`) and its conventions.
- Pulls, writes/updates the relevant page, verifies with `mkdocs build`, then
  commits and pushes.
- **Never writes secrets** — no tokens, keys, credentials, or private data.
- Any agent can invoke it via the opencode Task tool
  (`subagent_type: docs`).

Config lives at `~/.config/opencode/agent/`. See the individual agent files
for full prompts.