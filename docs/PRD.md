# PRD — Next.js TSX Boilerplate

## 1. Ringkasan

Next.js TSX Boilerplate adalah starter template siap pakai untuk memulai proyek
Next.js (App Router) dengan TypeScript. "Produk" di sini bukan aplikasi
end-user, melainkan **fondasi proyek** yang dipakai developer untuk mulai
membangun aplikasi nyata tanpa mengulang setup dasar dari nol.

## 2. Masalah yang Diselesaikan

Memulai proyek Next.js baru biasanya menghabiskan waktu untuk hal yang
berulang di setiap proyek:

- Konfigurasi TypeScript, ESLint, Prettier dari nol
- Menyusun struktur folder yang konsisten (dan sering berubah-ubah antar proyek)
- Memasang git hooks + commit convention
- Menyiapkan pipeline CI/CD dasar
- Menentukan konvensi coding agar AI coding agent (Claude, dll.) menghasilkan
  kode yang konsisten dengan gaya proyek

Boilerplate ini menyelesaikan itu sekali di awal, supaya proyek baru bisa
langsung fokus ke fitur.

## 3. Target Pengguna

- Developer (solo atau tim kecil) yang ingin memulai proyek Next.js + TypeScript baru
- Tim yang ingin konvensi struktur folder & code quality yang seragam di banyak proyek
- Pengguna yang coding dibantu AI agent (Claude Code) dan butuh aturan main yang eksplisit (`AGENTS.md`, `.agents/*/SKILL.md`) supaya output AI konsisten

## 4. Tujuan (Goals)

- Proyek baru bisa `pnpm install && pnpm dev` dan langsung jalan tanpa konfigurasi tambahan
- Struktur folder `src/*` sudah punya konvensi jelas (kapan pakai `services/` vs `lib/`, `utils/` vs `hooks/`, dst.) — didokumentasikan lewat `README.md` di tiap folder
- Code quality terjaga otomatis: format & lint jalan saat commit (Husky + lint-staged), commit message tervalidasi (commitlint), dan CI menolak build yang gagal lint/format
- Aturan desain & arsitektur untuk AI coding agent terdefinisi eksplisit di `.agents/` supaya kode yang di-generate konsisten secara arsitektur, penamaan, dan estetika
- Type-safety penuh dengan TypeScript strict mode

## 5. Non-Goals

- **Bukan** UI kit / component library siap pakai — tidak menyediakan komponen visual jadi (Button, Modal, dsb.), hanya konvensi di mana komponen tersebut seharusnya diletakkan
- **Bukan** aplikasi contoh (demo app) — halaman default masih scaffold bawaan `create-next-app`
- Tidak mengunci pilihan data-fetching library, ORM, atau auth provider tertentu — folder (`api/`, `lib/`, `services/`, `validations/`) disiapkan strukturnya, tapi library-nya dipilih sendiri oleh proyek turunan sesuai kebutuhan
- Tidak menyediakan backend/server terpisah — hanya pola Route Handler & Server Action di dalam repo Next.js yang sama (lihat `.agents/fullstack/SKILL.md`)

## 6. Ruang Lingkup (Fitur)

| Area                                                               | Status                                                                    |
| ------------------------------------------------------------------ | ------------------------------------------------------------------------- |
| Next.js 16 App Router + React 19 + TypeScript strict               | Selesai                                                                   |
| Tailwind CSS 4                                                     | Terpasang, token warna/font default (belum dikustomisasi)                 |
| Struktur folder `src/*` dengan panduan per folder                  | Selesai (`README.md` di tiap folder)                                      |
| ESLint + Prettier terintegrasi                                     | Selesai                                                                   |
| Husky (pre-commit, commit-msg, pre-push, post-merge) + lint-staged | Selesai                                                                   |
| Commitlint (Conventional Commits)                                  | Selesai                                                                   |
| CI (format check, lint strict, build)                              | Selesai — `.github/workflows/ci.yml`                                      |
| CD (Vercel / VPS / Docker, pilih salah satu)                       | Template tersedia, perlu diaktifkan manual — `.github/workflows/cd.yml`   |
| Panduan AI coding agent (`AGENTS.md`, `.agents/*/SKILL.md`)        | Selesai — prinsip software, frontend, frontend-design, backend, fullstack |
| Dokumentasi produk/arsitektur/desain (`docs/`)                     | Sedang disusun (dokumen ini)                                              |

## 7. Kriteria Sukses

- Proyek baru yang di-clone dari boilerplate ini bisa mulai coding fitur pada hari pertama, tanpa waktu tambahan untuk setup tooling
- Developer baru di tim bisa paham ke mana kode baru harus diletakkan hanya dengan membaca `README.md` di folder terkait, tanpa bertanya
- `pnpm lint:strict` dan `pnpm format:check` selalu lolos di `main` (dijaga oleh Husky pre-push + CI)
- Kode yang dihasilkan AI coding agent mengikuti struktur dan prinsip yang sama seperti kode yang ditulis manual

## 8. Maintainer

Hilmi Fawwaz Sa'ad — [github.com/hilmifawwazsaad/NextJS-Boilerplate](https://github.com/hilmifawwazsaad/NextJS-Boilerplate)
