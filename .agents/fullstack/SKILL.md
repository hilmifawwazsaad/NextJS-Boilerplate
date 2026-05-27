---
name: fullstack
description: Build fullstack features in a single Next.js (App Router) TypeScript project — spanning UI, Route Handlers, Server Actions, and/or DB. Not for pure UI (use frontend skill) or external API servers (use backend skill).
license: MIT
---

> Read `.agents/software-principles/SKILL.md` first.

## Pre-Code Checklist

1. Map data flow: origin (DB / external API / form input)
2. Mutation strategy: Server Action (form/page-tied) vs. Route Handler (REST/webhook)
3. Auth requirement at every server entry point
4. All four UI states: loading · error · empty · ideal

## Server/Client Boundary

| Need                    | Solution                                         | Location                        |
| ----------------------- | ------------------------------------------------ | ------------------------------- |
| Fetch + render data     | `async` Server Component                         | `app/**/page.tsx`               |
| Form / page mutation    | Server Action `'use server'`                     | `app/**/actions.ts`             |
| REST endpoint / webhook | Route Handler                                    | `app/api/**/route.ts`           |
| Interactivity / hooks   | Client Component `'use client'` — push to leaves | `components/`                   |
| Auth + route guard      | Middleware                                       | `middleware.ts`                 |
| Shared business logic   | Services / utilities                             | `lib/` · `services/` · `utils/` |

Extensions: `.tsx` JSX · `.ts` everything else.

## UI Layer

Apply all rules from the **frontend skill**. Key additions:

- Pass only serializable, non-sensitive data as props to Client Components.
- Wrap slow `async` Server Components in `<Suspense>`.
- `error.tsx` must be `'use client'`.
- `redirect()` throws — never call inside `try/catch`.

## Server Actions (`app/**/actions.ts`)

File must start with `'use server'` directive. Required order:

1. Validate input (zod or manual)
2. Check auth/session
3. Call service layer — no business logic inline
4. `revalidatePath()` or `revalidateTag()` after mutation
5. Return a typed result — follow project convention for shape (success flag, errors map, or throw)

## Route Handlers (`app/api/**/route.ts`)

Response shape — follow existing project convention. If none exists, pick one and apply consistently. Never mix shapes across handlers. Type the envelope — never return raw untyped objects.

Required regardless of shape: success/error must be distinguishable · validation errors include field-level detail · no stack traces or internal paths exposed.

Use `422` (not `400`) for validation failures. Handler order: validate input → auth check → service call → return envelope.

## Data Layer (`lib/db.ts` · `services/*.ts`)

- All DB access in service layer — never inline in handlers or components.
- Connection pooling always on. Parameterized queries or ORM — no string SQL.
- `unstable_cache()` or `cache()` for expensive repeated reads.
- `revalidateTag()` preferred over `revalidatePath()` for precision.

## TypeScript

- Type all request/response shapes with exported `interface` or `type`.
- `NextRequest` / `NextResponse` from `next/server` for Route Handlers.

## Security (non-negotiable)

- Auth check first in every Server Action and Route Handler — before any logic.
- `middleware.ts` guards routes via `cookies()` or token check.
- Secrets server-only — never `NEXT_PUBLIC_` for sensitive values.
- Parameterized queries. Hash passwords (bcrypt/argon2).
- Rate-limit auth and mutation-heavy endpoints.

## Never Do

- DB queries or secrets in Client Components / `NEXT_PUBLIC_` vars.
- Trust unvalidated input in actions or handlers.
- Sensitive data as Client Component props (exposed in JS bundle).
- Business logic inline in route handlers or page components.
- Skip any of the four UI states.
