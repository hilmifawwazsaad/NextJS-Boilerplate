# Next.js TypeScript Boilerplate

A production-ready Next.js boilerplate with TypeScript, Tailwind CSS, and comprehensive code quality tools configured out of the box.

## Tech Stack

- **Framework**: Next.js 16 (App Router)
- **Language**: TypeScript
- **Styling**: Tailwind CSS 4
- **Package Manager**: pnpm
- **Code Quality**: ESLint + Prettier
- **Git Hooks**: Husky + lint-staged
- **Commit Convention**: Commitlint (Conventional Commits)

## Getting Started

### 1. Clone or Download Repository

```bash
# Clone the repository
git clone https://github.com/hilmifawwazsaad/NextJS-Boilerplate.git
cd nextjs-tsx-boilerplate

# Or download ZIP and extract it
```

### 2. Install Dependencies

```bash
pnpm install
```

### 3. Run Development Server

```bash
pnpm dev
```

Open [http://localhost:3000](http://localhost:3000) to see the result.

### 4. Available Scripts

```bash
# Development
pnpm dev              # Start dev server
pnpm build            # Build for production
pnpm start            # Start production server

# Code Quality
pnpm lint             # Run ESLint
pnpm lint:strict      # ESLint with max 0 warnings
pnpm lint:fix         # Auto-fix ESLint errors
pnpm format:write     # Format with Prettier
pnpm format:check     # Check formatting
pnpm format           # Format + Lint + Strict check (all-in-one)
```

## Project Structure

```
├── src/
│   ├── app/              # App Router pages
│   │   ├── page.tsx      # Home page (/)
│   │   ├── layout.tsx    # Root layout
│   │   └── globals.css   # Global styles
│   ├── components/       # Reusable components (.tsx)
│   ├── lib/              # Utility functions (.ts)
│   ├── types/            # TypeScript type definitions
│   └── services/         # API services (.ts)
├── public/               # Static assets
└── .husky/               # Git hooks
```

## File Naming Convention

- `.tsx` - React components that return JSX
- `.ts` - TypeScript files (utils, services, types)
- `.css` - Stylesheets

## Features

### ✅ TypeScript Configuration

- Strict mode enabled for better type safety
- Path aliases configured: `@/*` maps to `./src/*`
- Example: `import Button from '@/components/Button'`

### ✅ Auto-formatting on Save

This project uses Prettier + ESLint with auto-formatting:

- **On Save**: VSCode auto-formats (if configured in `.vscode/settings.json`)
- **On Commit**: Husky + lint-staged auto-format staged files
- **Manual**: Run `pnpm format`

### ✅ Git Hooks (Husky)

Pre-configured git hooks for code quality:

- **pre-commit**: Runs lint-staged (format + lint staged files)
- **commit-msg**: Validates commit message format
- **pre-push**: Runs strict lint check before pushing
- **post-merge**: Auto-installs dependencies after merge

### ✅ Commit Message Convention

This project uses [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: add new feature
fix: resolve bug
docs: update documentation
style: format code
refactor: restructure code
test: add tests
chore: update dependencies
ci: update CI/CD
perf: performance improvements
revert: revert changes
```

Commitlint will reject commits that don't follow this format.

## Code Quality Setup

### ESLint + Prettier Integration

- ESLint validates code quality and catches errors
- Prettier formats code consistently
- Both work together without conflicts
- TypeScript files are automatically linted and formatted

### VSCode Integration

The `.vscode/settings.json` is pre-configured with:

- Auto-format on save
- ESLint auto-fix on save
- TypeScript intellisense
- Tailwind CSS autocomplete

## Learn More

- [Next.js Documentation](https://nextjs.org/docs)
- [TypeScript Documentation](https://www.typescriptlang.org/docs/)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- [pnpm Documentation](https://pnpm.io/)

## Deploy on Vercel

The easiest way to deploy your Next.js app is to use the [Vercel Platform](https://vercel.com/new?utm_medium=default-template&filter=next.js&utm_source=create-next-app&utm_campaign=create-next-app-readme).

Check out the [Next.js deployment documentation](https://nextjs.org/docs/app/building-your-application/deploying) for more details.
