# OpensourceKD — `.github`

Organization-wide Copilot configuration, reusable workflows, and prompt library for all repos under **OpensourceKD**.

---

## What's in here

| Path | Purpose |
|------|---------|
| `.github/copilot-instructions.md` | Org-wide coding standards automatically applied to GitHub Copilot in every repo |
| `.github/workflows/copilot-agent.yml` | Reusable workflow — call it from any repo in the org |
| `prompts/base.md` | Base agent identity & governance prompt |
| `prompts/modes/` | Mode-specific prompts (`fixer`, `shell-ui`, `mfe`, `api-lambda`, `reviewer`) |
| `config/copilot.json` | Agent configuration (limits, scope, prompt paths) |
| `workflow-templates/` | Starter workflow templates — appear in every repo's "Actions → New workflow" picker |

---

## How to use across your repos

### 1. Copilot coding standards (automatic)

The file `.github/copilot-instructions.md` is **automatically picked up by GitHub Copilot** in every repository under the `OpensourceKD` organization. No setup required — the JS/TS coding standards and vertical-context rules are active in all repos immediately.

---

### 2. Reusable workflow — call from any repo

Add a workflow file to your repo that calls the shared `copilot-agent.yml`:

```yaml
# .github/workflows/copilot-fixer.yml  (in YOUR repo)
name: Copilot Fixer

on:
  issues:
    types: [opened, labeled]
  pull_request:
    types: [opened, synchronize]

jobs:
  copilot-fixer:
    uses: OpensourceKD/.github/.github/workflows/copilot-agent.yml@main
    with:
      mode: fixer        # fixer | shell-ui | mfe | api-lambda | reviewer
    secrets: inherit
```

Available `mode` values:

| Mode | When to use |
|------|-------------|
| `fixer` | Fix issues or PR review comments — minimal, targeted changes |
| `reviewer` | Automated PR code review with structured feedback |
| `shell-ui` | Changes to the MFE host / Shell UI application |
| `mfe` | Changes to a Micro-Frontend repo |
| `api-lambda` | Changes to AWS Lambda functions or IaC (`infra/`) |

You can also supply a custom prompt file:

```yaml
    with:
      mode: fixer
      prompt_override: ".github/prompts/my-custom-mode.md"
```

---

### 3. Workflow starter templates (quickest path)

Every repo under `OpensourceKD` already sees the templates from this repo in **Actions → New workflow**. Look for entries named:

- **Copilot Fixer** — for issue & PR fix automation
- **Copilot Reviewer** — for automated PR review
- **Copilot Shell UI** — for shell / host app repos
- **Copilot MFE** — for Micro-Frontend repos
- **Copilot API Lambda** — for Lambda / IaC repos

Click "Configure", commit the generated file, and the workflow is live.

---

## Modes at a glance

### `fixer`
Implements targeted fixes for issues, CI failures, or review comments. Triggers on newly opened/labeled issues and on PR open/synchronize events. Makes the smallest change that fully resolves the problem. Stops if a fix would touch more than 3 files.

### `reviewer`
Reviews only the PR diff — flags bugs, security issues, logic errors, and standard violations. Outputs structured feedback with file, line range, severity, and suggestion.

### `shell-ui`
Applies Shell UI architectural rules: no business logic in the shell, auth tokens in-memory only, data-driven route config, explicit `ShellAPI` types, no MFE internals imported.

### `mfe`
Applies MFE rules: no re-bundled shared deps, typed `mount`/`unmount` entrypoint, all API calls through a service layer, no cross-MFE imports, auth read from Shell context.

### `api-lambda`
Applies Lambda rules: thin handler adapter, pure-function service modules, middleware for cross-cutting concerns, typed input schemas, environment-parameterized IaC stacks, no hard-coded secrets.

---

## Contributing

- Add new mode prompts to `prompts/modes/` and register them in `config/copilot.json` and `.github/workflows/copilot-agent.yml`.
- Keep `prompts/base.md` minimal — it is prepended to every agent run.
- Workflow templates in `workflow-templates/` require a `.yml` + `.properties.json` pair.
