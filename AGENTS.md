# AGENTS.md

Read the relevant skill file before writing any code.
Check `package.json` for available libraries — never import what isn't installed.

**Stack:** Next.js 16 · React 19 · TypeScript 5 (strict) · Tailwind CSS 4 · ESLint 9 · Prettier · Husky · commitlint — no data-fetching lib installed.

| Task domain | When to use                                                                | Skill file                   |
| ----------- | -------------------------------------------------------------------------- | ---------------------------- |
| Frontend    | Pure UI — components, pages, layouts, styling; no DB or server logic       | `.agents/frontend/SKILL.md`  |
| Backend     | External API server (Go, Python, PHP, etc.) separate from the Next.js repo | `.agents/backend/SKILL.md`   |
| Fullstack   | UI + server logic in one Next.js repo — Route Handlers, Server Actions, DB | `.agents/fullstack/SKILL.md` |

Task spans multiple domains → pick dominant, note overlap.
Convention missing from skill file → ask before inventing.
