---
name: software-principles
description: Engineering principles for all code in this Next.js TypeScript project. Required reading before any code generation.
license: MIT
---

## Pre-Code Checklist

1. One reason to change? If not — split it (SRP)
2. Simpler solution with same outcome? — use it (KISS)
3. Building for a future need that doesn't exist yet? — delete it (YAGNI)
4. Name reveals intent without generic words (`and`, `data`, `info`, `manager`, `handle`)? — if not, rethink the design

## Principles

| Principle                            | Rule                                                | Violation Signal                                         | Fix                                         |
| ------------------------------------ | --------------------------------------------------- | -------------------------------------------------------- | ------------------------------------------- |
| **SRP** — Single Responsibility      | One unit, one reason to change                      | `"and"` in name · file > 200 lines · function > 20 lines | Split into focused units                    |
| **OCP** — Open/Closed                | Extend behavior without modifying existing code     | Adding a variant by editing existing component internals | Variant props · composition · new component |
| **DIP** — Dependency Inversion       | Depend on abstractions, not concretions             | `new ConcreteService()` hardcoded inside logic           | Inject dependencies                         |
| **Composition > Inheritance**        | Compose behaviors via hooks/props, not class chains | Extending a component class · inheritance chain > 1      | Props + custom hooks instead                |
| **DRY** — Don't Repeat Yourself      | One source of truth per piece of logic              | Copy-paste logic across files                            | Extract to shared function or module        |
| **KISS** — Keep It Simple            | Simplest solution that correctly solves the problem | Unnecessary abstraction · deep indirection               | Remove layers · flatten                     |
| **YAGNI** — You Aren't Gonna Need It | Build only what is needed right now                 | Unused params · "might need later" code                  | Delete it                                   |
| **SoC** — Separation of Concerns     | Each module owns exactly one concern                | UI + data fetch + business logic in one component/file   | Separate into layers (page · hook · util)   |
| **LoD** — Law of Demeter             | Talk only to direct collaborators                   | `a.b.c.method()` chains                                  | Add intermediate method                     |
| **Fail Fast**                        | Surface errors at the earliest point                | Silent catch · late validation · nullable everywhere     | Validate at boundaries · throw early        |
| **SSOT** — Single Source of Truth    | One authoritative place per logic or data           | Same validation in multiple layers                       | Centralize · import everywhere              |

## Naming

Generic names destroy readability. Names must reveal intent.

| Concept    | Pattern                     | Good                                           | Bad                                     |
| ---------- | --------------------------- | ---------------------------------------------- | --------------------------------------- |
| Functions  | verb phrase                 | `getUserById`, `validateEmail`, `hashPassword` | `handle`, `process`, `doStuff`, `run`   |
| Booleans   | `is` / `has` / `can` prefix | `isActive`, `hasPermission`, `canDelete`       | `active`, `flag`, `check`, `status`     |
| Variables  | noun, specific              | `userId`, `paginatedUsers`, `hashedPassword`   | `data`, `result`, `info`, `temp`, `val` |
| Components | PascalCase, noun            | `UserCard`, `AuthGuard`, `ModalOverlay`        | `usercard`, `myComponent`, `Comp1`      |
| Hooks      | `use` prefix + verb phrase  | `useAuth`, `useFetchUser`, `useFormValidation` | `authHook`, `userData`, `myHook`        |
| Files      | `[domain].[layer].ts`       | `user.service.ts`, `auth.middleware.ts`        | `utils2.ts`, `misc.ts`, `helpers.ts`    |

Rules: no abbreviations (except `id`, `req`, `res`, `err`, `ctx`) · no single-letter names outside loop counters · name length proportional to scope.

## Function Design

| Rule                  | Limit                   | When exceeded                                |
| --------------------- | ----------------------- | -------------------------------------------- |
| Single responsibility | One action per function | Split into smaller functions                 |
| Length                | ≤ 20 lines              | Extract inner logic to named helper          |
| Parameters            | ≤ 3 params              | Group into an options object                 |
| Nesting               | ≤ 2 levels deep         | Extract or use early return (guard clause)   |
| Return paths          | Prefer single exit      | Guard clauses at top, one `return` at bottom |

## Applied to This Project

Next.js TSX boilerplate — frontend-first, API routes for server logic.

| Principle | Concrete example                                                                          |
| --------- | ----------------------------------------------------------------------------------------- |
| SRP       | `UserCard` renders one user — data fetching lives in `useUser` hook, not in the component |
| SoC       | Page components fetch data · UI components render · hooks own logic — never mix           |
| DRY       | Shared types in `types/` · validation schema once in `validations/` via `z.infer`         |
| Fail Fast | `config/env.ts` throws at startup if env vars missing · validate API response at boundary |
| SSOT      | Error messages → `constants/errors.ts` · API base URL → one config file                   |
| YAGNI     | No global state until local state is proven insufficient                                  |
| KISS      | Component calls one hook — no multi-source data orchestration inside JSX                  |
| DIP       | Components depend on hook interfaces, not fetch calls directly                            |

## Async Error Handling

- Catch only where you can meaningfully recover
- Never `catch` and return `null`/`undefined` — throw a typed error instead
- In React: use error boundaries or `error.tsx` for UI-level recovery

## Testing

- Unit test pure functions and hooks in isolation
- Integration test at API route boundaries — not implementation details
- Don't mock what you own; mock external services only
- One assertion per test concept — split tests rather than chain assertions

## Never Do

- Name anything `data`, `result`, `info`, `temp`, `manager`, `handleX`, `processX`
- Functions > 20 lines · parameters > 3 · nesting > 2 levels — split or group
- `as any` or `as unknown as T` to bypass type checks — fix the actual type issue
- Mutate state directly — always return new references (`{ ...prev, key: value }`, `[...arr, item]`)
- Put business logic inside JSX or component body — extract to hook or utility
