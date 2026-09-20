# ARCHITECTURE — Next.js TSX Boilerplate

## 1. Tech Stack

| Layer             | Pilihan                           | Catatan                                   |
| ----------------- | --------------------------------- | ----------------------------------------- |
| Framework         | Next.js 16 (App Router)           | `src/app/`                                |
| Bahasa            | TypeScript 5 (strict mode)        | `tsconfig.json`                           |
| UI                | React 19                          | Server Components by default              |
| Styling           | Tailwind CSS 4                    | via `@tailwindcss/postcss`                |
| Package manager   | pnpm 10                           | workspace tunggal (`pnpm-workspace.yaml`) |
| Lint              | ESLint 9 (`eslint-config-next`)   | flat config, `eslint.config.mjs`          |
| Format            | Prettier 3                        | `.prettierrc`                             |
| Git hooks         | Husky 9 + lint-staged             | `.husky/`                                 |
| Commit convention | commitlint (Conventional Commits) | `commitlint.config.js`                    |
| CI/CD             | GitHub Actions                    | `.github/workflows/`                      |

Tidak ada library data-fetching, ORM, atau auth bawaan — dipilih sendiri oleh
proyek turunan sesuai kebutuhan (lihat folder `api/`, `lib/`, `services/`).

## 2. Prinsip Arsitektur

1. **Server Component sebagai default.** Client Component (`'use client'`)
   hanya dipakai sedekat mungkin ke leaf node yang butuh interaktivitas/browser
   API — bukan di level halaman.
2. **Layered separation** — tiap layer punya satu tanggung jawab:
   - `app/` — routing & composition (page, layout, loading, error)
   - `components/` — UI murni, tanpa logika bisnis
   - `hooks/` — state & logic sisi klien yang reusable
   - `services/` — logika bisnis & akses data (DB/API), dipanggil dari Route Handler atau Server Action
   - `lib/` — setup/konfigurasi library pihak ketiga & helper teknis infrastruktur
   - `utils/` — pure function tanpa side effect
   - `validations/` — schema validasi, dipakai bersama di client & server
3. **Fail fast** — validasi di boundary (input form, response API eksternal), bukan di tengah alur.
4. **Single source of truth** — satu tempat otoritatif untuk tiap jenis data: tipe di `types/`, validasi di `validations/`, konstanta di `constants/`.

Detail lengkap prinsip (SRP, DRY, KISS, dst.) ada di
`.agents/software-principles/SKILL.md`.

## 3. Struktur Folder

```
src/
├── app/            # Routing App Router: page, layout, loading, error, route handler
├── api/            # HTTP client ke API eksternal (BUKAN app/api/ route handler)
├── components/     # Komponen UI reusable, dikelompokkan per kategori (ui/, layout/, form/)
├── config/         # Baca process.env, ekspos sebagai objek terstruktur
├── constants/      # Nilai statis (routes, roles, status) — tidak bergantung env
├── contexts/       # React Context + Provider untuk state global (auth, tema)
├── hooks/          # Custom hook (prefix `use`), logika stateful reusable
├── lib/            # Setup library pihak ketiga & singleton (Prisma client, NextAuth, dsb.)
├── services/       # Logika bisnis & akses data — dipanggil dari Route Handler/Server Action
├── types/          # Definisi tipe TypeScript lintas aplikasi
├── utils/          # Pure function (formatter, string/array helper)
└── validations/    # Schema validasi (mis. Zod), dipakai di client & server
```

Setiap folder punya `README.md` sendiri berisi kegunaan, struktur yang
disarankan, dan contoh kode — baca sebelum menambah file baru di folder
tersebut. Perbedaan folder yang mirip:

| Dibandingkan              | Perbedaan                                                                                                          |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| `lib/` vs `services/`     | `lib/` = setup teknis generik (tidak spesifik domain bisnis). `services/` = logika bisnis spesifik domain.         |
| `config/` vs `constants/` | `config/` = baca `process.env`. `constants/` = nilai literal statis, tidak bergantung environment.                 |
| `src/api/` vs `app/api/`  | `src/api/` = client-side memanggil API eksternal. `app/api/` = Route Handler (server-side endpoint) milik Next.js. |
| `utils/` vs `hooks/`      | `utils/` = pure function tanpa state. `hooks/` = logika stateful yang terikat siklus hidup komponen React.         |

## 4. Alur Data (Fullstack, dalam satu repo Next.js)

```
Form / Client Component
        │  (validasi client, schema dari validations/)
        ▼
Server Action ('use server')  atau  Route Handler (app/api/**/route.ts)
        │  1. validasi input ulang (validations/)
        │  2. cek auth/session
        │  3. panggil services/*.ts
        ▼
services/*.ts  ──►  lib/*.ts (Prisma, dsb.)  ──►  DB / API eksternal
        │
        ▼
revalidatePath()/revalidateTag()  →  UI ter-update
```

Aturan lengkap Server/Client boundary, bentuk response envelope, dan security
checklist ada di `.agents/fullstack/SKILL.md`.

## 5. TypeScript & Path Alias

- `strict: true` — tidak ada implicit `any`.
- Alias `@/*` → `./src/*` (dikonfigurasi di `tsconfig.json`), contoh:
  `import { formatCurrency } from '@/utils/format'`.
- Props komponen selalu lewat `interface`/`type` eksplisit, bukan inferensi implisit.

## 6. Tooling & Quality Gate

| Tahap                            | Alat                                    | Perintah                                 |
| -------------------------------- | --------------------------------------- | ---------------------------------------- |
| Saat menyimpan file (editor)     | Prettier + ESLint auto-fix              | via `.vscode/settings.json`              |
| Pre-commit                       | lint-staged                             | format + lint file yang di-stage         |
| Commit-msg                       | commitlint                              | validasi format Conventional Commits     |
| Pre-push                         | ESLint strict                           | `pnpm lint:strict` (0 warning toleransi) |
| Post-merge                       | pnpm install otomatis                   | sinkronisasi dependency setelah merge    |
| CI (push/PR ke `main`)           | format check → lint strict → build      | `.github/workflows/ci.yml`               |
| CD (setelah CI sukses di `main`) | deploy (Vercel/VPS/Docker — pilih satu) | `.github/workflows/cd.yml`               |

## 7. Environment Variables

Dikelola lewat `.env.example` sebagai referensi (tidak di-commit `.env`
sungguhan). Opsi yang disiapkan strukturnya: `NEXT_PUBLIC_API_URL`, JWT,
API key server-to-server, NextAuth, OAuth provider — aktifkan sesuai
kebutuhan proyek turunan. Variabel sensitif **tidak boleh** memakai prefix
`NEXT_PUBLIC_` (akan ter-bundle ke client).

## 8. Panduan Khusus AI Coding Agent

`AGENTS.md` di root memetakan domain tugas ke skill file yang wajib dibaca
sebelum generate kode:

| Domain                                    | Skill file                             |
| ----------------------------------------- | -------------------------------------- |
| Semua tugas (wajib selalu)                | `.agents/software-principles/SKILL.md` |
| UI murni                                  | `.agents/frontend/SKILL.md`            |
| UI dengan penekanan visual/estetika       | `.agents/frontend-design/SKILL.md`     |
| Server API terpisah (bukan repo ini)      | `.agents/backend/SKILL.md`             |
| UI + server logic dalam satu repo Next.js | `.agents/fullstack/SKILL.md`           |

Lihat [`docs/DESIGN.md`](./DESIGN.md) untuk ringkasan prinsip desain UI/UX.
