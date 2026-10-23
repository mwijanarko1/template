---
last_mapped: 2026-10-23T21:05:00Z
---

# Codebase Map

## System Overview

Starter Next.js App Router site for cloning into new projects. Minimal surface: one home page with a Hero section, Zod-validated env helper, Vitest + Testing Library, Oxlint (anti-slop plugin) plus ESLint, Tailwind CSS 4, Bun.

No auth, Convex, Clerk, Cloudflare Workers, or email integration ships in the tree yet. Those are documented as optional CLI profile placeholders in `AGENTS.md`.

Local checkout: `template`. GitHub template repo: `mwijanarko1/template`.

## Directory Guide

| Path | Role |
|------|------|
| `src/app/layout.tsx` | Root layout: Geist fonts, viewport theme colors, metadata / Open Graph, skip link, body shell |
| `src/app/page.tsx` | Home route; renders `Hero` |
| `src/app/globals.css` | Tailwind v4 entry (`@import "tailwindcss"`), `@theme` typography tokens, blob animation utilities |
| `src/app/favicon.ico` | App favicon |
| `src/components/Hero.tsx` | Full-viewport gradient hero with animated blobs |
| `src/components/Hero.test.tsx` | Hero smoke tests (Vitest + Testing Library) |
| `src/lib/env.ts` | `getEnv()` + Zod schema (`NODE_ENV`, optional `NEXT_PUBLIC_APP_URL`) |
| `src/test/setup.ts` | Vitest setup (`@testing-library/jest-dom`) |
| `tools/oxlint/anti-slop/` | Vendored anti-slop Oxlint JS plugin (shared across Mikhail Builds repos) |
| `oxlint.config.mjs` | Primary linter config; wires anti-slop rules |
| `eslint.config.mjs` | Flat ESLint via `eslint-config-next` (core-web-vitals + typescript) |
| `vitest.config.mts` | Vitest + React plugin, `@` alias to `src/` |
| `next.config.mjs` | `reactStrictMode`, security headers / CSP |
| `postcss.config.mjs` | `@tailwindcss/postcss` + autoprefixer |
| `tailwind.config.js` | Content paths, blob keyframes, `@tailwindcss/typography` plugin |
| `package.json` | Scripts and Bun-managed dependencies (`packageManager` field) |
| `AGENTS.md` | Agent rules, GitHub + SaaS CLI profile placeholders, pointers to launch checklists |
| `README.md` | Human getting-started |
| `docs/CODEBASE_MAP.md` | This map |
| `docs/security-checklist.md` | Security gate (from `security-audit` / mitigation skills) |
| `docs/web-compliance-checklist.md` | Privacy / a11y / consumer (from `website-compliance`) |
| `docs/technical-seo-checklist.md` | Crawl / index / metadata (from `technical-seo`) |
| `.env.example` | Documented env keys for local setup |

## Routes

| Route | Source |
|-------|--------|
| `/` | `src/app/page.tsx` → `Hero` |
| `/_not-found` | Next.js default not-found |

## Key Workflows

1. **Local loop:** `bun install`, `bun run dev`, edit under `src/`, `bun run test:run`, `bun run lint`, `bun run typecheck`, `bun run build`.
2. **New UI section:** add a component under `src/components/`, compose it from `src/app/page.tsx` (or a new `src/app/<route>/page.tsx`). Prefer Tailwind utilities already used in `Hero`.
3. **New public route:** add `src/app/<route>/page.tsx`, update metadata in that page or via shared helpers if you introduce them, extend tests when behaviour matters.
4. **Env vars:** extend the Zod schema in `src/lib/env.ts`, document in `.env.example`, call `getEnv()` from server code only.
5. **Lint:** `bun run lint` (Oxlint + anti-slop) is the default gate. Use `bun run lint:eslint` for Next/ESLint rules. Do not lint `tools/oxlint/anti-slop` via TypeScript project checks (excluded in `tsconfig.json`).
6. **CLI / deploy accounts:** before `gh`, Cloudflare, Convex, Clerk, or Resend work, replace and use the placeholders in `AGENTS.md` (`YOUR_GITHUB_LOGIN`, `YOUR_CF_PROFILE`, `YOUR_CONVEX_PROFILE`, `YOUR_CLERK_INSTANCE`, `YOUR_RESEND_PROFILE`).
7. **Dependency bumps:** stay inside Bun's 7-day `minimumReleaseAge`. Pair `oxlint` and `@oxlint/plugins` on the same version.
8. **Launch / compliance pass:** walk `docs/security-checklist.md`, `docs/web-compliance-checklist.md`, and `docs/technical-seo-checklist.md` (or load the matching skills for a deeper audit).

## Tests

| File | Covers |
|------|--------|
| `src/components/Hero.test.tsx` | Hero renders heading, supporting copy, and main landmark |

## Known Risks

- `NEXT_PUBLIC_APP_URL` defaults metadata base to `http://localhost:3000` when unset; set it before production SEO.
- CSP in `next.config.mjs` is strict (`script-src 'self'`). Adding analytics, Clerk, or Convex clients will need deliberate CSP / header updates.
- `@tailwindcss/typography` is pulled for the Tailwind plugin; an override pins `postcss-selector-parser` for audit cleanliness. Removing typography means revisiting that override.
- Anti-slop lives under `tools/` and is excluded from `tsc`; breaking the plugin only shows up when Oxlint loads it.
- README / AGENTS profile placeholders must be filled per clone; leaving `YOUR_*` values will point agents at the wrong GitHub or Cloudflare account.
