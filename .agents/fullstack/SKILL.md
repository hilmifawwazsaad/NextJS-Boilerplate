---
name: fullstack
description: Build fullstack features in a single Next.js (App Router) TypeScript project — spanning UI, Route Handlers, Server Actions, and/or DB. Not for pure UI (use frontend skill) or external API servers (use backend skill).
license: MIT
---

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

```ts
'use server';
// 1. Validate input (zod or manual)
// 2. Check auth/session
// 3. Call service layer — no business logic inline
// 4. revalidatePath() or revalidateTag() after mutation
// Return: { success: boolean, data?: T, errors?: Record<string, string[]> }
```

## Route Handlers (`app/api/**/route.ts`)

Response envelope (type it):

```ts
// Success: { success: true,  data: T,    message: string, error: null }
// Error:   { success: false, data: null, message: string, error: { code: string, details?: {field: string, message: string}[] } }
```

HTTP codes: `200/201/204` success · `422` validation · `401` unauth · `403` forbidden · `409` conflict · `500` no stack trace.
Handler pattern: validate → auth check → service call → return envelope.

## Data Layer (`lib/db.ts` · `services/*.ts`)

- All DB access in service layer — never inline in handlers or components.
- Connection pooling always on. Parameterized queries or ORM — no string SQL.
- `unstable_cache()` or `cache()` for expensive repeated reads.
- `revalidateTag()` preferred over `revalidatePath()` for precision.

## TypeScript

- Type all request/response shapes with exported `interface` or `type`.
- `NextRequest` / `NextResponse` from `next/server` for Route Handlers.
- No `any` — `unknown` + narrowing for untyped external data.

## Security (non-negotiable)

- Auth check first in every Server Action and Route Handler — before any logic.
- `middleware.ts` guards routes via `cookies()` or token check.
- Secrets server-only — never `NEXT_PUBLIC_` for sensitive values.
- Parameterized queries. Hash passwords (bcrypt/argon2).
- Rate-limit auth and mutation-heavy endpoints.

## Never Do

- DB queries or secrets in Client Components / `NEXT_PUBLIC_` vars.
- Trust unvalidated input in actions or handlers.
- `useEffect` for data when a Server Component works.
- Sensitive data as Client Component props (exposed in JS bundle).
- Business logic inline in route handlers or page components.
- Skip any of the four UI states.
- `any` type · `.jsx` / `.js` extensions.
