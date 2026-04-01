# API / Lambda Mode

You are operating in the context of the **API & IaC layer** — AWS Lambda functions and co-located infrastructure code (MVP).

## Context

### API (Lambda)
- All server-side logic is implemented as **AWS Lambda functions** (Node.js / TypeScript).
- **One Lambda per route or bounded context** — handlers must be small and single-purpose.
- The Lambda handler is a thin adapter only: parse event → call service → serialize response. Business logic lives in pure-function service modules, not in the handler.
- Cross-cutting concerns (auth, input validation, error handling, structured logging) are applied via **middleware composition** (e.g. `middy`). Do not inline these in every handler.
- All responses follow a consistent shape; use a shared `respond` helper.

### IaC (co-located with API for MVP)
- Infrastructure is defined in a dedicated `infra/` directory using **SST v3** or **AWS CDK (TypeScript)**.
- Every stack is parameterized by environment (`dev`, `staging`, `prod`) — no hard-coded ARNs, account IDs, or region strings.
- Secrets and config values are read from **SSM Parameter Store** or **Secrets Manager** — never committed to source.
- IaC code follows the same coding standards as application code.

## Coding Standards

Apply all [organization-wide coding standards](.github/copilot-instructions.md) plus:
- Lambda handlers must not exceed ~50 lines (they are adapters, not services).
- Service modules must be pure functions; inject all dependencies (e.g. DB client) as arguments rather than importing global singletons.
- Use a typed event/response schema (e.g. `zod`) for input validation at the handler boundary.
- No `any` in TypeScript — use generated types from API schemas or explicit interfaces.
- Errors must be caught at the middleware layer; service functions throw typed errors, they do not return mixed success/error shapes.

## Review / Fix Focus

When reviewing or fixing API / IaC code:
1. Confirm the handler is a thin adapter (parse → service → respond) with no inline business logic.
2. Confirm cross-cutting concerns (auth, validation, logging) are handled by middleware, not inlined.
3. Confirm no secrets, ARNs, or account IDs are hard-coded.
4. Confirm service functions are pure and dependencies are injected.
5. Confirm input is validated with a typed schema before use.
6. Confirm IaC stacks are environment-parameterized.
