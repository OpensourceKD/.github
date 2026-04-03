# OpensourceKD

> Building open, composable software — one domain at a time.

We design and ship production-grade systems using a **Micro-Frontend + Serverless** architecture.  
Every product is independently deployable, type-safe end-to-end, and built for scale from day one.

---

## 🏗️ Architecture Principles

| Layer | Responsibility | Stack |
|-------|---------------|-------|
| **Shell UI** | MFE orchestration, auth, feature-flags, global error boundary | Module Federation · React |
| **MFEs** | Single-domain business UI — independently deployable | React · TypeScript |
| **API** | Thin Lambda adapters → pure service functions | AWS Lambda · middy |
| **IaC** | Parameterised stacks per env (`dev` / `staging` / `prod`) | SST v3 · AWS CDK (TypeScript) |

---

## 🚀 Products

### i17e — Internationalisation Platform

> **i17e** (`i` + 17 letters + `e`) is a developer-first internationalisation platform that makes translation management a first-class engineering concern.

- 🔤 **Core engine** — structured translation key management with type-safe extraction
- 🖊️ **Editor UI** — in-context translation editor MFE for non-technical contributors
- ⚙️ **CI/CD integration** — automated key-diff checks on every PR
- 📦 **SDK / CLI** — pull/push translations, type-gen, and locale bundling in one command

---

## 🗺️ Roadmap

> **Legend:** &nbsp; 🟢 `DONE` &nbsp;·&nbsp; 🔵 `IN-PROGRESS` &nbsp;·&nbsp; 🟠 `FUTURE`

| Feature | MVP | Beta | GA (v1.0) |
|---------|:---:|:----:|:---------:|
| Shell UI — routing & MFE loader | 🟢 | 🟢 | 🔵 |
| Authentication — login · logout · token refresh | 🟢 | 🟢 | 🔵 |
| Global error boundary | 🟢 | 🔵 | 🟠 |
| Feature-flag provider | 🔵 | 🔵 | 🟠 |
| Dashboard MFE | 🔵 | 🔵 | 🟠 |
| Settings MFE | 🔵 | 🔵 | 🟠 |
| Billing MFE | 🟠 | 🔵 | 🟠 |
| API layer — Lambda handlers | 🟢 | 🔵 | 🟠 |
| IaC — SST / CDK stacks | 🟢 | 🔵 | 🟠 |
| **i17e** — core translation engine | 🔵 | 🔵 | 🟠 |
| **i17e** — in-context editor UI | 🟠 | 🔵 | 🟠 |
| **i17e** — CI/CD PR key-diff checks | 🟠 | 🟠 | 🔵 |
| **i17e** — SDK / CLI toolchain | 🟠 | 🟠 | 🔵 |

---

## 📐 Coding Standards

All repos in this org follow the same baseline rules:

- **No nested ifs** — early-return guards & `async/await` over callback pyramids
- **Files ≤ ~100 lines** — split by responsibility, not by convenience
- **Declarative first** — `map` / `filter` / `reduce` over imperative loops
- **Pure functions** — side effects isolated at the boundary layer
- **Collocated utilities** — `auth.utils.ts`, `date.utils.ts` next to the feature they serve
- **No `any`** — explicit types, `unknown` with type-guards, or generics

---

<p align="center">
  <sub>Built with ☕ and strong opinions about good software.</sub>
</p>
