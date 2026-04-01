# Shell UI Mode

You are operating in the context of the **Shell UI** — the top-level host application in a Micro-Frontend (MFE) architecture.

## Context

The Shell UI is responsible for:
- **MFE orchestration**: registering, routing to, and mounting/unmounting remote MFE modules via Module Federation.
- **Shared infrastructure**: exposing a versioned shared scope (React, React-DOM, design system tokens, i18n provider, etc.) so MFEs do not bundle duplicates.
- **Authentication**: owning the full auth lifecycle (login, logout, silent token refresh, session expiry). Auth state is shared with MFEs through the Shell's typed context or event bus — MFEs never call an auth provider directly.
- **Global error boundary**: catching unhandled MFE errors and rendering a fallback without taking down the entire shell.
- **Feature flags**: providing a single feature-flag context that all MFEs read from.

## What the Shell Must NOT Do

- Implement business logic or domain-specific UI. Delegate everything to the appropriate MFE.
- Import internal modules of any MFE. Depend only on each MFE's public entrypoint.

## Coding Standards

Apply all [organization-wide coding standards](.github/copilot-instructions.md) plus:
- The `ShellAPI` type contract must be explicitly typed — no `any`.
- Auth tokens must never be stored in `localStorage`; use in-memory storage with a `httpOnly` cookie refresh flow.
- Route configuration must be data-driven (a typed config array), not a chain of `if/else` or `switch` statements.
- Shared module versions must be pinned in the Module Federation config to prevent version skew.

## Review / Fix Focus

When reviewing or fixing Shell UI code:
1. Confirm auth token handling does not expose tokens to JS (XSS risk).
2. Confirm MFE loading errors are caught and do not crash the shell.
3. Confirm no MFE internals are imported by the shell.
4. Confirm shared scope versions are explicit and consistent.
