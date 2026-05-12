<div align="center">

<h1>Next.js TypeScript Boilerplate</h1>

Boilerplate Next.js siap produksi dengan TypeScript, Tailwind CSS, dan berbagai alat quality code yang telah dikonfigurasi sejak awal.

</div>

## Tech Stack

- **Framework**: Next.js 16 (App Router)
- **Language**: TypeScript
- **Styling**: Tailwind CSS 4
- **Package Manager**: pnpm
- **Code Quality**: ESLint + Prettier
- **Git Hooks**: Husky + lint-staged
- **Commit Convention**: Commitlint (Conventional Commits)

## Memulai

### 1. Clone atau Unduh Repository

```bash
# Clone repository
git clone https://github.com/hilmifawwazsaad/NextJS-Boilerplate.git
cd nextjs-tsx-boilerplate

# Atau unduh ZIP dan ekstrak
```

### 2. Install Dependensi

```bash
pnpm install
```

### 3. Jalankan Development Server

```bash
pnpm dev
```

Buka [http://localhost:3000](http://localhost:3000) untuk melihat hasilnya.

### 4. Script yang Tersedia

```bash
# Development
pnpm dev              # Jalankan dev server
pnpm build            # Build untuk produksi
pnpm start            # Jalankan production server

# Code Quality
pnpm lint             # Jalankan ESLint
pnpm lint:strict      # ESLint dengan maksimal 0 warning
pnpm lint:fix         # Perbaiki error ESLint secara otomatis
pnpm format:write     # Format dengan Prettier
pnpm format:check     # Periksa formatting
pnpm format           # Format + Lint + Strict check (semua sekaligus)
```

## Konvensi Penamaan File

- `.tsx` - Komponen React yang mengembalikan JSX
- `.ts` - File TypeScript (utils, services, types)
- `.css` - Stylesheet

## Fitur

### Konfigurasi TypeScript

- Strict mode diaktifkan untuk keamanan tipe yang lebih baik
- Path alias terkonfigurasi: `@/*` mengarah ke `./src/*`
- Contoh: `import Button from '@/components/Button'`

### Auto-formatting saat Menyimpan

Project ini menggunakan Prettier + ESLint dengan auto-formatting:

- **Saat Simpan**: VSCode otomatis memformat (jika dikonfigurasi di `.vscode/settings.json`)
- **Saat Commit**: Husky + lint-staged otomatis memformat file yang di-stage
- **Manual**: Jalankan `pnpm format`

### Git Hooks (Husky)

Git hooks yang telah dikonfigurasi untuk menjaga kualitas kode:

- **pre-commit**: Menjalankan lint-staged (format + lint file yang di-stage)
- **commit-msg**: Memvalidasi format pesan commit
- **pre-push**: Menjalankan strict lint check sebelum push
- **post-merge**: Otomatis install dependensi setelah merge

### Konvensi Pesan Commit

Project ini menggunakan [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: tambah fitur baru
fix: perbaiki bug
docs: perbarui dokumentasi
style: format kode
refactor: restrukturisasi kode
test: tambah pengujian
chore: perbarui dependensi
ci: perbarui CI/CD
perf: peningkatan performa
revert: kembalikan perubahan
```

Commitlint akan menolak commit yang tidak mengikuti format ini.

## Pengaturan Code Quality

### Integrasi ESLint + Prettier

- ESLint memvalidasi kualitas kode dan mendeteksi error
- Prettier memformat kode secara konsisten
- Keduanya bekerja bersama tanpa konflik
- File TypeScript otomatis di-lint dan diformat

### Integrasi VSCode

`.vscode/settings.json` telah dikonfigurasi dengan:

- Auto-format saat menyimpan
- ESLint auto-fix saat menyimpan
- TypeScript intellisense
- Tailwind CSS autocomplete

## Pelajari Lebih Lanjut

- [Dokumentasi Next.js](https://nextjs.org/docs)
- [Dokumentasi TypeScript](https://www.typescriptlang.org/docs/)
- [Dokumentasi Tailwind CSS](https://tailwindcss.com/docs)
- [Dokumentasi pnpm](https://pnpm.io/)

## Deploy di Vercel

Cara termudah untuk mendeploy aplikasi Next.js adalah menggunakan [Platform Vercel](https://vercel.com/new?utm_medium=default-template&filter=next.js&utm_source=create-next-app&utm_campaign=create-next-app-readme).

Lihat [dokumentasi deployment Next.js](https://nextjs.org/docs/app/building-your-application/deploying) untuk detail lebih lanjut.
