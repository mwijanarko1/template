# Security checklist

Pre-launch and periodic security checklist for this Next.js site. Derived from the `security-audit` and `security-vulnerability-mitigation` skills.

This is a project gate, not a full independently verified repository audit. For a whole-repo vulnerability hunt, load `security-audit` (pstack Understand, read-only, independent verification). For implementing a confirmed fix, load `security-vulnerability-mitigation`. Do not write exploit PoCs against systems you do not own.

Mark items `[x]` when done. Use `n/a` in notes when a surface does not exist yet (for example no auth, no uploads).

## How to run

1. Inventory entry points: App Router pages, route handlers, server actions, webhooks, env usage, third-party SDKs.
2. Walk the sections below against source and config (and a deployed preview when available).
3. Record findings as `confirmed` only with source evidence; otherwise `needs_validation` with the missing fact.
4. Fix at trust boundaries; add a safe regression test when behaviour is non-trivial.
5. Re-check `bun audit`, lockfile pins, and headers after dependency or deploy changes.

## Secrets and configuration

- [ ] No API keys, tokens, passwords, or connection strings in git (check `.env*`, commits, CI logs)
- [ ] `.env.example` documents keys without real values; secrets only in `.env.local` / host secret store
- [ ] Client bundles only receive `NEXT_PUBLIC_*` values that are safe to expose
- [ ] Production debug tooling and verbose error pages are off
- [ ] CORS (if any) is closed to intended origins; no `*` with credentials
- [ ] Dependency installs use the lockfile; no `@latest` or unpinned fetch installs in scripts/CI

## HTTP, headers, and transport

- [ ] Production is HTTPS-only; HTTP redirects to HTTPS without long chains
- [ ] Security headers present and reviewed (`next.config.mjs` or edge/host config): at least `X-Content-Type-Options`, `Referrer-Policy`, framing control, HSTS in production
- [ ] Content-Security-Policy is intentional; third-party scripts (Clerk, analytics, Convex client) are allowlisted deliberately, not via broad `unsafe-inline` / `*`
- [ ] No mixed content on production pages
- [ ] Cookies that carry session or auth use `Secure`, `HttpOnly`, and an appropriate `SameSite`

## AuthN, AuthZ, and sessions (when auth exists)

- [ ] Authentication is not treated as authorization; every mutating path checks resource ownership or role
- [ ] Object-level checks on read/update/delete (IDOR-safe); bulk/export paths enforce per-item permissions
- [ ] CSRF protection for cookie-authenticated mutations (framework defaults or explicit tokens)
- [ ] Session/token lifetime, refresh, and logout/revoke behaviour are correct
- [ ] Account recovery, invites, and magic links do not leak whether an email/user exists (enumeration)
- [ ] Clerk / OAuth / JWT issuer config matches the deployment (dev keys never on production)

## Input, output, and injection

- [ ] Untrusted input validated at the boundary (Zod or equivalent) before use
- [ ] No string-built SQL; parameterized queries / ORM binds only
- [ ] HTML output is encoded by default; `dangerouslySetInnerHTML` is rare, reviewed, and sanitized
- [ ] Redirects and links do not accept open external URLs without an allowlist
- [ ] File paths from user input cannot escape intended directories (path traversal)
- [ ] SSRF: server-side fetches of user-controlled URLs are allowlisted; block link-local / metadata IPs
- [ ] Webhooks verify signatures and reject unsigned or replayed payloads
- [ ] Shell / child-process APIs are avoided; if required, arguments are not concatenated from user input

## Data, privacy-adjacent, and abuse

- [ ] Multi-tenant data is scoped by tenant/user on every query (no cross-tenant leakage)
- [ ] Exports, backups, search, and admin tools cannot return data above the caller's privilege
- [ ] Uploads (if any) enforce auth, type allowlist, size limits, and safe storage paths
- [ ] Rate limiting or equivalent controls on expensive or auth-sensitive endpoints
- [ ] Logs and error responses do not print secrets, tokens, or full PII payloads

## Supply chain and release

- [ ] `bun audit` (or equivalent) is clean or has documented accepted risk
- [ ] `oxlint` / `@oxlint/plugins` and other security-sensitive pins move as matching pairs
- [ ] CI does not inject host credentials into untrusted build steps
- [ ] Deploy uses the correct Cloudflare / host profile (`YOUR_CF_PROFILE`); secrets via host secret store (`wrangler secret`, dashboard), not committed files
- [ ] Preview deployments cannot reach production data stores unless intentionally designed and reviewed

## Attack-class coverage (when doing a deeper pass)

Select only classes that match the current app surface. Full audits follow `security-audit` companions; this list is a reminder for Next.js sites:

- [ ] Injection (SQL, HTML/XSS, shell, path, header/log)
- [ ] Access control (auth bypass, IDOR, inconsistent routes)
- [ ] Resource and file handling (traversal, SSRF, uploads, archives)
- [ ] Cryptography and secrets (hardcoded secrets, weak tokens, bad crypto)
- [ ] Business logic (skipped steps, races, numeric abuse, unsafe defaults)
- [ ] Feature abuse / data leakage (export, search oracles, draft/preview leakage)
- [ ] Client-side (DOM XSS, `postMessage`, storage, CORS) when shipping interactive clients
- [ ] Supply chain and release (deps, CI, deploy config)
- [ ] Cloud / edge misconfig (Workers bindings, public buckets, overly broad tokens)

## Mitigation rules of thumb

When fixing a confirmed issue (`security-vulnerability-mitigation`):

1. Threat model: who can call this, what they control, impact.
2. Fix at the boundary: validate/encode once where data enters or leaves a trust zone.
3. Least privilege: narrow tokens, roles, network, and filesystem access.
4. Verify with a regression test or safe exploit-path check that fails before and passes after.
5. Secrets: never commit; rotate if exposed; scrub logs.

## Finding format

```
[critical|major|minor] area — location
  status: confirmed | needs_validation | n/a
  evidence: ...
  impact: ...
  fix: ...
  verify: ...
```

## Related

- Skill: `security-audit` (discovery + independent verification)
- Skill: `security-vulnerability-mitigation` (implement fixes)
- Skill: `website-compliance` (privacy / legal / a11y; not the same as vulns)
- This template: `next.config.mjs` headers/CSP, `src/lib/env.ts`, `.env.example`
