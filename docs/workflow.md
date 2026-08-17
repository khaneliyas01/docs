# Workflow

How this knowledge base is written, updated, and deployed.

## The golden rule

> Every opencode task ends by delegating to the **`docs` subagent**.

The global `AGENTS.md` and every per-project `AGENTS.md` carry this rule, so
**every agent, in every repo, knows**: once a task is done, the docs get
updated.

## How a doc update happens

1. A task completes in any repo.
2. The agent calls the `docs` subagent via the Task tool.
3. The `docs` agent:
   1. Pulls the latest `~/docs`.
   2. Locates the right page(s):
      - A project's page → `docs/projects/<repo>.md`
      - About the user/setup → `docs/about.md`
      - The agents themselves → `docs/agents.md`
      - Process notes → `docs/workflow.md`
      - New repo → creates a new page and adds it to `mkdocs.yml` + the index
   3. Writes/updates the content. **No secrets, ever.**
   4. Verifies with `mkdocs build` (via `~/docs/.venv`).
   5. Commits and pushes to `main`.
4. GitHub Actions builds the site and deploys it to **GitHub Pages**
   (`https://khaneliyas01.github.io/docs/`).

## Local dev

```bash
cd ~/docs
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
mkdocs serve    # preview at http://127.0.0.1:8000
mkdocs build    # static site into site/
```

## Structure

```
docs/                    # the repo
├── mkdocs.yml           # site config, theme, nav
├── requirements.txt     # mkdocs-material
├── docs/                # markdown content
│   ├── index.md         # home
│   ├── about.md         # about us
│   ├── projects/        # one page per repo
│   ├── agents.md        # the opencode agent fleet
│   └── workflow.md      # this page
└── .github/workflows/pages.yml  # build + deploy to Pages
```

## Rules of content

1. **No secrets.** If it's a token, key, credential, or private data — it does
   not go here. The repo is public.
2. **One page per project**, kept in `docs/projects/`.
3. **Keep it current.** Update on every meaningful change.
4. **Match the house style**: title, bulleted metadata block, "What it is /
   Purpose / Status" sections.
5. **Verify before pushing**: `mkdocs build` must pass.