# OpensourceKD — Org Governance

This repository contains org-wide GitHub Copilot governance: coding standards, mode-specific prompts, and a reusable Copilot agent workflow.

---

## How It Works

```
Member repo issue/PR assigned or opened
        │
        ▼
.github/workflows/copilot-agent-<mode>.yml   ← in the member repo (from workflow template)
        │  calls
        ▼
OpensourceKD/.github/.github/workflows/copilot-agent.yml   ← reusable workflow (this repo)
        │  loads prompts from governance/ checkout, then posts
        ▼
@github-copilot comment on the issue/PR with full base + mode prompt
```

---

## Wiring Up a New Member Repo

### 1 — Add the calling workflow

Go to the member repo → **Actions** → **New workflow** → search for **"Copilot Agent"**. GitHub will suggest the matching template from this repo. Pick the correct mode:

| Repo type | Template to use |
|---|---|
| Shell UI host app | `Copilot Agent — Shell UI` |
| Micro-frontend | `Copilot Agent — MFE` |
| AWS Lambda / API | `Copilot Agent — API Lambda` |

Or copy the relevant file from [`.github/workflow-templates/`](./.github/workflow-templates/) manually into `.github/workflows/copilot-agent.yml` inside the member repo.

### 2 — Add `copilot-instructions.md`

Copy [`templates/copilot-instructions.md`](./templates/copilot-instructions.md) to `.github/copilot-instructions.md` in the member repo. This ensures the Copilot Coding Agent always sees the org standards, even before the workflow fires.

### 3 — Required permissions

The calling workflow declares these permissions (already included in the templates):

```yaml
permissions:
  contents: write       # agent may commit code
  pull-requests: write  # agent may comment on PRs
  issues: write         # agent may comment on issues and apply labels
```

---

## Repository Structure

```
.github/
  copilot-instructions.md          # org-wide Copilot Chat instructions (auto-applied)
  workflow-templates/              # GitHub-suggested workflow templates for member repos
    copilot-agent-shell-ui.yml
    copilot-agent-mfe.yml
    copilot-agent-api-lambda.yml
  workflows/
    copilot-agent.yml              # reusable governance workflow (called by member repos)
config/
  copilot.json                     # agent configuration
prompts/
  base.md                          # base agent prompt (identity, governance, PR rules)
  modes/
    shell-ui.md                    # Shell UI context and review focus
    mfe.md                         # MFE context and review focus
    api-lambda.md                  # API / Lambda / IaC context and review focus
templates/
  copilot-instructions.md          # copy into member repo .github/ as fallback instructions
```