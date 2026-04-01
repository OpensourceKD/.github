# MFE (Micro-Frontend) Mode

You are operating in the context of a **Micro-Frontend (MFE)** — a self-contained, independently deployable UI module that plugs into the Shell.

## Context

Each MFE:
- Owns a **single business domain** (e.g. `dashboard`, `settings`, `billing`, `onboarding`).
- Is built and deployed independently; the Shell lazy-loads it at runtime via Module Federation.
- Consumes auth context, shared component library, and i18n from the Shell's shared scope — it never bundles its own copies of these.
- Exposes exactly **one public entrypoint** (`mount` / `unmount` functions) and a **typed public API** surface. Internal modules are private.
- Communicates with other MFEs only through the Shell event bus or a shared state slice provided by the Shell — never by direct import.

## What an MFE Must NOT Do

- Bundle shared dependencies (React, design system, auth) — consume them from the Shell's shared scope.
- Import internals of another MFE.
- Manage its own auth flow — read auth state from the Shell context.
- Assume a specific URL structure owned by the Shell.

## Coding Standards

Apply all [organization-wide coding standards](.github/copilot-instructions.md) plus:
- The public entrypoint (`mount`/`unmount`) must be explicitly typed.
- All API calls must go through a typed service layer — no raw `fetch` calls inside components.
- Component files must stay ≤ ~100 lines; extract sub-components or hooks when this limit is approached.
- Co-locate styles, tests, and utils with the component they belong to (feature-folder pattern).

## Review / Fix Focus

When reviewing or fixing MFE code:
1. Confirm no shared dependency is re-bundled (check webpack/vite externals / Module Federation config).
2. Confirm the public entrypoint is typed and nothing outside `src/index.ts` is exported.
3. Confirm there are no direct imports from sibling MFEs.
4. Confirm API calls go through a service layer, not directly from components.
5. Confirm the MFE reads auth from Shell context, not from its own auth logic.
