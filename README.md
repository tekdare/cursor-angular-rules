# cursor-angular-rules

> 🎯 Hierarchical Cursor AI rules for safe Angular monorepos

A structured set of `.cursor/rules/*.mdc` files that constrain Cursor AI to generate **modern Angular v20** code — standalone components, Signals, typed RxJS — with zero rule conflicts.

---

## 📁 Rule Hierarchy

```
.cursor/rules/
├── 000-global-naming.mdc      # alwaysApply: true  — global naming + TypeScript baseline
├── 100-angular-components.mdc # globs: **/*.component.{ts,html,css}
└── 200-rxjs-signals.mdc       # globs: **/*.service.ts, **/*.component.ts
```

| File | `alwaysApply` | `globs` | Purpose |
|------|:---:|---|---|
| `000-global-naming.mdc` | ✅ true | `apps/sandbox/src/**/*` | Naming conventions + TypeScript strict baseline |
| `100-angular-components.mdc` | ❌ false | `**/*.component.{ts,html,css}` | Standalone decorator, template control flow, BEM styles, testing |
| `200-rxjs-signals.mdc` | ❌ false | `**/*.service.ts`, `**/*.component.ts` | RxJS subscriptions, Signal API, RxJS↔Signals bridge |

---

## 🚀 Quick Start

### 1. Copy rules into your Nx monorepo

```bash
cp -r .cursor/rules/ <your-nx-monorepo>/.cursor/rules/
```

### 2. Verify Cursor loads the rules

In Cursor → **Settings → Rules** you should see all three files listed.

### 3. Run the sandbox to test generated code

```bash
npx nx serve sandbox        # dev server
npx nx lint sandbox         # ESLint check
npx nx test sandbox         # Vitest unit tests
```

---

## 🏗️ Rule Design Principles

### Semantic segmentation with Markdown headings

Each file uses `##` / `###` headings to divide semantically distinct concerns, maximising Cursor MDC precision and preventing AI from mixing rules across domains.

### Three-layer cascade

```
000 (always)  ←─ foundation every file inherits
  │
  ├── 100 (component files)  ←─ adds component-specific constraints
  │     └── decoration, template, style, test rules
  │
  └── 200 (service + component files)  ←─ adds reactive state constraints
        └── RxJS naming, subscription management, Signals API
```

### Conflict prevention

- **000** only governs naming & TypeScript — never prescribes implementation patterns.
- **100** only governs component structure — never touches service or RxJS rules.
- **200** only governs reactive state — cross-references 100 but never contradicts it.

---

## 📐 Key Rules at a Glance

### Naming
- `UserProfileComponent` / `app-user-profile` / `user-profile.component.ts`
- Service: `AuthService` / `auth.service.ts`
- RxJS stream: `data$` (Finnish notation, `$` suffix)
- Angular Signal: `count`, `isActive` (**no** `$` suffix)
- Injection Token: `DATE_FORMAT` (CAPS_SNAKE)

### Components (100)
- Standalone by default — no NgModule
- `ChangeDetectionStrategy.OnPush` always
- `inject()` for DI, `input()` / `output()` for I/O
- `signal()` + `computed()` for state
- New control flow: `@if`, `@for`, `@switch`, `@defer`

### Reactive State (200)
- `takeUntilDestroyed(destroyRef)` — never manual `unsubscribe`
- `shareReplay({ bufferSize: 1, refCount: true })` for hot streams
- `signal.update()` / `signal.set()` — **never** `signal.mutate()`
- `toSignal()` / `toObservable()` as the RxJS↔Signals bridge

---

## 🔗 References

- [Angular v20 Docs](https://angular.dev)
- [Cursor MDC Rules](https://docs.cursor.com/context/rules)
- [Nx Monorepo](https://nx.dev)
