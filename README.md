# cursor-rules

> 🎯 Hierarchical Cursor AI rules for safe full-stack Angular & .NET monorepos

A structured set of `.cursor/rules/*.mdc` files and modern **Nx Monorepo** architecture designed to constrain Cursor AI to generate **modern Angular v20** code (standalone components, Signals, typed RxJS), clean **C#/.NET** microservices, and optimized **SQL** databases with zero rule conflicts.

---

## ✨ Key Features & Tech Stack

- **Modern Nx Monorepo (v21)**: Fully integrated workspace utilizing `@nx/angular`, `@nx/vite`, and `@nx/workspace` for seamless cross-layer development and shared library management.
- **Angular v20 Frontend**: High-performance frontend powered by Vite 6 and AnalogJS plugins. Embraces Standalone components, Signals, reactive state management, and strict TypeScript/ESLint 9 standards.
- **C# / .NET Clean Architecture**: Advanced rules governing C# microservices, Minimal APIs, robust domain modeling, observability (logging/tracing/health checks), and enterprise security resilience.
- **Multi-Dialect SQL Optimization**: Isolated rule sets for MSSQL, Oracle, PostgreSQL, MySQL, and SQLite to ensure optimized indexing, safe migrations, and precise SQL dialect generation.
- **Next-Gen Quality Assurance**: Fast unit testing powered by Vitest 3 (`vitest.workspace.ts`) and modern flat-config ESLint (`eslint.config.mjs`).
- **Zero-Conflict AI Guardrails**: Multi-layer rule cascade with strict glob matching to prevent AI token bloating and cross-domain instruction conflicts.

---

## 🏛️ Project Structure

Our monorepo cleanly separates applications and shared libraries to enforce clear boundary definitions and clean architecture:

```text
cursor-rules/
├── .cursor/                  # Global Cursor AI rules (alwaysApply: true)
│   └── rules/                # Domain-specific Cursor AI rules (alwaysApply: false)
├── apps/
│   ├── backend/              # Backend application container / microservice entry
│   └── frontend/             # Modern Angular v20 frontend application (Vite/AnalogJS)
├── libs/
│   ├── backend/
│   │   ├── auth/             # Backend authentication library
│   │   └── database/         # Database integration & infrastructure library
│   ├── frontend/
│   │   ├── data-access/      # Frontend state management, services & API clients
│   │   └── ui/               # Reusable UI components & design system
│   └── shared/
│       ├── types/            # Shared TypeScript/domain contract definitions
│       └── utils/            # Cross-cutting utility functions
├── MDC_RULES_ANALYSIS.md     # In-depth analysis of MDC loading & glob matching
├── project_structure.md      # Detailed breakdown of file trees & project structure
├── eslint.config.mjs         # Modern flat ESLint 9 configuration
├── vitest.workspace.ts       # Vitest 3 workspace configuration
└── nx.json                   # Nx monorepo configuration
```

*For more in-depth details on the repository layout and rule loading analysis, see [project_structure.md](project_structure.md) and [MDC_RULES_ANALYSIS.md](MDC_RULES_ANALYSIS.md).*

---

## 📁 Rule Hierarchy & Loading Mechanism

The rules are divided into two main layers: project-wide global rules under `.cursor/`, and domain-specific rules under `.cursor/rules/`.

### 1. Global Rules (`.cursor/`)
These rules are loaded globally (`alwaysApply: true`) to provide core baseline guidelines across all files in the repository.

| File | `alwaysApply` | `globs` | Purpose |
|------|:---:|---|---|
| `architecture-monorepo.mdc` | ✅ true | All | Monorepo architecture & cross-layer boundary rules |
| `project-overview.mdc` | ✅ true | All | Overall project overview & AI collaboration guidelines |
| `safety-security.mdc` | ✅ true | All | Safety, data protection, verification, and testing baselines |

### 2. Domain Rules (`.cursor/rules/`)
These rules target specific technologies and file patterns based on glob matching (`alwaysApply: false`). This ensures Cursor AI only loads relevant context, preventing token bloating and instruction pollution.

| Category / File | `alwaysApply` | `globs` | Purpose |
|---|:---:|---|---|
| **Global Naming & TS** | | | |
| `000-global-naming.mdc` | ❌ false | `apps/frontend/src/**/*` | Modern naming conventions & TypeScript baseline for frontend assets |
| **Angular Frontend** | | | |
| `100-angular-naming-and-structure.mdc` | ❌ false | `apps/**/*.ts`, `libs/**/*.ts` | Angular naming, file structures & TypeScript coding standards |
| `110-angular-components.mdc` | ❌ false | `apps/**/*.component.{ts,html,css}`, `libs/**/*.component.{ts,html,css}` | Standalone component decoration, templates, style guide & A11y |
| `120-angular-rxjs-signals.mdc` | ❌ false | `apps/**/*.{component,service}.ts`, `libs/**/*.{component,service}.ts` | Reactive state management, RxJS and Signals integration & boundaries |
| `130-angular-routing-forms-http.mdc` | ❌ false | `apps/**/*.ts`, `libs/**/*.ts` | Angular routing, forms, HTTP & error handling |
| **C# Backend** | | | |
| `200-csharp-backend-naming-and-structure.mdc` | ❌ false | `services/**/*.cs`, `src/**/*.cs`, `api/**/*.cs` | C# naming, microservice project structures & coding standards |
| `210-csharp-api-and-minimalapi.mdc` | ❌ false | `**/*Controller.cs`, `api/**/*.cs`, `**/Program.cs` | Web APIs, Minimal APIs, request validation & response formatting |
| `220-csharp-application-domain-infra.mdc` | ❌ false | `services/**/*.cs`, `src/**/*.cs` | Clean Architecture layers (Application, Domain, Infrastructure) |
| `230-csharp-observability-security.mdc` | ❌ false | `services/**/*.cs`, `src/**/*.cs`, `**/Program.cs` | Observability (logging, trace, health check) & security resilience |
| **SQL Database** | | | |
| `300-sql-global-data-rules.mdc` | ❌ false | `database/**/*.sql`, `sql/**/*.sql`, `migrations/**/*.sql`, `**/*.sql` | Common SQL, safety, naming, indexing & migration standards |
| `310-mssql-rules.mdc` | ❌ false | `database/mssql/**/*.sql`, `sql/mssql/**/*.sql`, `**/*mssql*.sql` | Microsoft SQL Server specific dialect & optimization rules |
| `320-oracle-rules.mdc` | ❌ false | `database/oracle/**/*.sql`, `sql/oracle/**/*.sql`, `**/*oracle*.sql` | Oracle Database specific dialect & optimization rules |
| `330-postgresql-rules.mdc` | ❌ false | `database/postgresql/**/*.sql`, `sql/postgresql/**/*.sql`, `**/*postgres*.sql`, `**/*pgsql*.sql` | PostgreSQL specific dialect & optimization rules |
| `340-mysql-rules.mdc` | ❌ false | `database/mysql/**/*.sql`, `sql/mysql/**/*.sql`, `**/*mysql*.sql` | MySQL specific dialect & optimization rules |
| `350-sqlite-rules.mdc` | ❌ false | `database/sqlite/**/*.sql`, `sql/sqlite/**/*.sql`, `**/*sqlite*.sql` | SQLite specific dialect & optimization rules |
| **DevOps** | | | |
| `400-devops-ci-monorepo.mdc` | ❌ false | `**/Dockerfile`, `**/*.yml`, `**/*.yaml`, `.github/**/*.yml`, `azure-pipelines*.yml`, `docker-compose*.yml` | DevOps pipelines, CI/CD, Docker container configuration & monorepo deployment |

---

## 🚀 Quick Start

### 1. Copy rules into your monorepo

```bash
cp -r .cursor/ <your-monorepo>/.cursor/
```

### 2. Verify Cursor loads the rules

In Cursor, you should see the rules taking effect based on the files you are currently editing. Global rules will always be in context, while domain rules dynamically activate per file type.

---

## 🏗️ Rule Design Principles

### Semantic segmentation with Markdown headings

Each file uses `##` / `###` headings to divide semantically distinct concerns, maximising Cursor MDC precision and preventing AI from mixing rules across domains.

### Multi-layer Cascade

```
Global Rules (.cursor/*.mdc)
  ├── Frontend Domain (1xx-angular-*.mdc, active on apps/libs)
  ├── Backend Domain (2xx-csharp-*.mdc, active on services/src/api)
  ├── Database Domain (3xx-sql-*.mdc, active on database/sql)
  └── DevOps Domain (4xx-devops-*.mdc, active on YAML/Docker)
```

### Conflict prevention

- **Global Rules** only govern naming, monorepo boundaries, & general safety—never prescribing specific domain implementation details.
- **Frontend / Backend / Database / DevOps Rules** are isolated via `globs` filters so they never load simultaneously onto unrelated file scopes (preventing token bloating and LLM instruction conflicts).
- **Domain files (e.g., C# vs TS)** will never mix up instructions, ensuring highly contextual and precise AI code generation.

---

## 🔗 References & Documentation

- [MDC Rules Analysis Report](MDC_RULES_ANALYSIS.md)
- [Project Structure Documentation](project_structure.md)
- [Angular v20 Docs](https://angular.dev)
- [Cursor MDC Rules](https://docs.cursor.com/context/rules)
- [Nx Monorepo](https://nx.dev)
