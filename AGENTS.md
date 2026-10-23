# Agent Instructions

## Project

Beginner-friendly Next.js website template (`mwijanarko1/template`). Stack: Next.js 16 App Router, React 19, TypeScript, Tailwind CSS 4, Vitest, Oxlint (with anti-slop), Bun.

This file is meant to be customized when the template is cloned into a real project. Replace every `YOUR_*` placeholder below before remote git, deploy, or SaaS CLI work.

## GitHub account

Use the **`YOUR_GITHUB_LOGIN`** GitHub CLI profile for every git and `gh` operation in this repository.

```bash
env -u GITHUB_TOKEN gh auth switch --user YOUR_GITHUB_LOGIN
gh api user --jq .login   # must print YOUR_GITHUB_LOGIN
```

- Remote owner: `YOUR_GITHUB_LOGIN/YOUR_REPO` (this template ships as `mwijanarko1/template`)
- Do not push, open PRs, or create issues as a different logged-in account
- If `GITHUB_TOKEN` is set in the shell, it overrides the keyring user; unset it when switching

## CLI profiles (Cloudflare, Convex, Clerk, Resend)

These services are optional until the project adds them. When they are in use, always target the named profile or instance for this repo. Never use a personal default profile by accident.

| Tool | Profile / instance placeholder | Typical check |
|------|--------------------------------|---------------|
| Cloudflare (`cf` CLI) | `YOUR_CF_PROFILE` | `cf auth whoami --profile YOUR_CF_PROFILE` |
| Convex (`convex-profile`) | `YOUR_CONVEX_PROFILE` | `convex-profile list` then `convex-profile switch YOUR_CONVEX_PROFILE` |
| Clerk CLI | `YOUR_CLERK_INSTANCE` (`dev` or `prod`) | `clerk config pull --instance YOUR_CLERK_INSTANCE` |
| Resend CLI | `YOUR_RESEND_PROFILE` | `resend auth list` / `resend whoami -p YOUR_RESEND_PROFILE` |

### Cloudflare (`cf`)

Prefer the `cf` CLI over bare Wrangler for auth and account ops. Pass the profile on every call:

```bash
cf auth create YOUR_CF_PROFILE --no-browser
cf auth whoami --profile YOUR_CF_PROFILE
cf <command> --profile YOUR_CF_PROFILE
```

Optional: bind the profile to this checkout so later commands inherit it:

```bash
cf auth activate YOUR_CF_PROFILE
```

If a deploy step needs Wrangler (for example OpenNext), pass the matching profile there too (`bunx wrangler deploy --profile YOUR_CF_PROFILE`).

### Convex

Convex CLI login is global. Use the local `convex-profile` helper (not a project dependency) to save and switch named logins under `~/.convex/profiles/`:

```bash
convex-profile list
convex-profile switch YOUR_CONVEX_PROFILE
bunx convex dev
```

Save a new login after `bunx convex login --force` with `convex-profile save YOUR_CONVEX_PROFILE`. Do not switch while another Convex command is writing credentials.

### Clerk

Clerk CLI auth is account-level; instances are selected per command:

```bash
clerk auth login
clerk config pull --instance YOUR_CLERK_INSTANCE
# production env files are often pulled separately, e.g.:
# bunx clerk env pull --instance prod --file .env.production.local
```

Replace `YOUR_CLERK_INSTANCE` with `dev` or `prod` (or the instance name used for this project). Keep production secrets out of git.

### Resend

```bash
resend login -p YOUR_RESEND_PROFILE
resend auth switch YOUR_RESEND_PROFILE
resend whoami -p YOUR_RESEND_PROFILE
```

You can also set `RESEND_PROFILE=YOUR_RESEND_PROFILE` in the shell, or pass `-p YOUR_RESEND_PROFILE` on each command.

## Working guidelines

- Keep changes small and limited to the requested scope.
- Follow existing patterns in `src/` before introducing new abstractions or dependencies.
- Use **Bun** for package management and project commands (`bun install`, `bun add`, `bun run …`). Do not use npm or yarn unless the user explicitly asks.
- Never commit secrets. Use `.env.local` for local values (see `.env.example`).
- Preserve unrelated worktree changes.
- Respect `~/.bunfig.toml` `minimumReleaseAge` (7 days). Do not disable the age gate for routine dependency bumps.
- Primary lint is Oxlint (`bun run lint`), including anti-slop rules under `tools/oxlint/anti-slop`. Use `bun run lint:eslint` when Next/ESLint rules matter.

## Commands

- `bun install` installs dependencies.
- `bun run dev` starts the local development server (Turbopack).
- `bun run lint` runs Oxlint.
- `bun run lint:eslint` runs ESLint (Next core-web-vitals + TypeScript).
- `bun run typecheck` runs `tsc --noEmit`.
- `bun run test` / `bun run test:run` runs Vitest.
- `bun run build` creates a production build.
- `bun run start` serves the production build.

## Verification

Run the narrowest relevant check first. Before finishing, review `git diff` and confirm every changed line belongs to the request.

Before a public launch or after adding auth, forms, payments, trackers, or a custom domain, walk the docs checklists (or load the matching skills for a deeper pass):

| Checklist | Skill(s) | When |
|-----------|----------|------|
| `docs/security-checklist.md` | `security-audit`, `security-vulnerability-mitigation` | Secrets, headers, authz, injection, supply chain, deploy |
| `docs/web-compliance-checklist.md` | `website-compliance` | Privacy, a11y, cookies, consumer surfaces |
| `docs/technical-seo-checklist.md` | `technical-seo` (+ `pagespeed` for live CWV) | robots/sitemap, canonicals, metadata, crawl/index |

Compliance gaps are not automatically vulnerabilities. Full repository security audits stay read-only under `security-audit` and need independent verification; do not probe production without explicit scope.

## Project references

Read these before architectural, product, launch, or UI-structure changes:

- `README.md`
- `docs/CODEBASE_MAP.md` (refresh with the cartographer skill when module layout changes)
- `docs/security-checklist.md`
- `docs/web-compliance-checklist.md`
- `docs/technical-seo-checklist.md`
