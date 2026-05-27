---
name: backend
description: 'Design and generate external backend API code (any language/framework — Go, Rust, Python, PHP, JavaScript, Ruby, etc.) that serves a Next.js TSX frontend. Use for API endpoints, auth, validation, and data modeling on a separate backend server.'
license: MIT
---

> Read `.agents/software-principles/SKILL.md` first.

## Pre-Code Checklist

1. Resource + HTTP method + action
2. Request → response contract (input shape, output shape, errors)
3. Auth requirement (public / authenticated / role-gated)
4. Failure scenarios + validation rules

## Response Envelope

Shape may vary per project or company convention — follow existing convention if one exists. If starting fresh, pick one shape and apply it consistently throughout the entire project. Never mix shapes across endpoints.

Required regardless of shape:

- Success and error responses must be distinguishable (boolean flag, HTTP status, or dedicated error field)
- Validation errors must include field-level detail, not just a generic message
- Error responses must never expose stack traces, query strings, or internal paths
- Type the envelope — never return untyped `any` or raw ORM objects

## Design Rules

- **Versioning** — prefix all routes `/api/v1/`.
- **Endpoints** — plural nouns, kebab-case: `/api/v1/posts`, `/api/v1/post-categories`.
- **Query params** — `page`, `limit`, `sort`, `filter[key]=value`.
- **Auth** — `Authorization: Bearer <token>` (JWT/opaque) or HTTP-only cookie (same parent domain).
- **Pagination** — all list endpoints: `data: { items: T[], pagination: { page, limit, totalItems, totalPages } }`.
- **CORS** — exact origin only (`http://localhost:3000` dev, exact domain prod). Never `*` with credentials.
- **Status codes** — use `422` (not `400`) for validation failures; `500` responses must never include stack traces or internal details.
- **Naming** — pick camelCase or snake_case; consistent throughout the project.
- **Docs** — expose OpenAPI spec at `/api/docs` or `/openapi.json` when feasible.

## Architecture

- Route handlers thin — business logic in service/use-case layer.
- Global error handler → catches all unhandled errors → safe 500.
- DB connection pooling. Async/non-blocking I/O.

## Security (non-negotiable)

- Secrets via env vars only — never hardcoded.
- Parameterized queries/ORM — no string-concatenated SQL.
- Hash passwords (bcrypt/argon2 or language equivalent).
- Rate-limit auth endpoints (login, register, password reset).
- Limit request body size
- Security headers in production (X-Frame-Options, CSP, HSTS).
- Validate all input at boundary before processing.

## Never Do

- Trust unvalidated client input or hardcode secrets.
- Business logic directly in route handlers.
