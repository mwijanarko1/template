# Web compliance checklist

Privacy, accessibility, site-surface security, and consumer-protection checklist. Derived from the `website-compliance` skill.

Not legal advice. Flag gaps for human counsel when regions, payments, or special categories of data are in scope. Compliance gaps are not automatically security vulnerabilities; pair with `docs/security-checklist.md` and the `security-audit` skill for source vulns.

Mark items `[x]` when done. Note intentional deferrals.

## How to run

1. **Scope** — regions served, data collected, payments, cookies/trackers, user-generated content.
2. **Inventory** — forms, analytics, third parties, auth, stored PII.
3. **Audit** against the gates below.
4. **Fix** product-owned gaps; document intentional deferrals.
5. **Verify** with page checks and network/cookie inspection when relevant.

## Privacy

- [ ] Clear privacy notice is published and linked from the footer and every collection point
- [ ] Terms of use (or equivalent) linked where the product needs them
- [ ] Lawful basis / purpose stated before collection where required for the target regions
- [ ] Cookie / consent banner when non-essential trackers run; reject is honored (no dark patterns)
- [ ] Data subject paths documented or implemented: access, delete, export
- [ ] PII minimized; retention limits defined; no surprise third-party sharing
- [ ] Third-party processors (Clerk, Convex, Resend, analytics, fonts CDNs) listed in the privacy notice when used
- [ ] Contact path for privacy requests is real and monitored

## Accessibility (baseline)

- [ ] Interactive controls are keyboard reachable; focus is visible
- [ ] Form inputs have associated labels; errors are announced appropriately
- [ ] Meaningful images have alt text; decorative images are marked appropriately
- [ ] Text meets contrast AA; meaning is not conveyed by color alone
- [ ] Landmarks and headings are in a sensible order (one H1 per page template)
- [ ] Skip link to main content works (template ships one in `layout.tsx`; keep it)
- [ ] Live regions used for async errors/status when needed
- [ ] Motion / animation respects `prefers-reduced-motion` where motion is non-essential

## Security (site surface)

These overlap the security checklist; check them here for the public site posture:

- [ ] HTTPS everywhere in production
- [ ] Cookies that authenticate use `Secure`, `HttpOnly`, and appropriate `SameSite`
- [ ] CSRF protection for cookie-auth mutations
- [ ] No secrets in client bundles
- [ ] CSP present and feasible for the scripts the site actually loads
- [ ] File uploads (if any) enforce type/size limits and auth

## Consumer / e-commerce (when applicable)

- [ ] Pricing, shipping, and refund information are clear before payment
- [ ] Business / operator identity is clear (legal name, contact)
- [ ] Age gates only when product or law requires them
- [ ] Marketing claims match what the product delivers

## Forms and communications

- [ ] Contact / waitlist / newsletter forms state what happens to submitted data
- [ ] Transactional email (Resend, etc.) uses verified domains and clear from-identity
- [ ] No pre-ticked marketing consent where unfair under target rules

## Finding format

```
[critical|major|minor] area — location
  gap / required fix
  deferral rationale (if any)
```

## Related

- Skill: `website-compliance`
- Skill: `technical-seo` (`docs/technical-seo-checklist.md`) for crawl/index
- Skill: `security-audit` / `docs/security-checklist.md` for source vulnerabilities
- Pair audits: privacy + a11y here; CSP/secrets/auth in the security checklist
