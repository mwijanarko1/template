# Technical SEO checklist

Crawlability, indexation, rendering, performance, and machine-readable site infrastructure. Derived from the `technical-seo` skill.

Different from content/AEO work (`ai-search-optimization`). For Core Web Vitals measurement on a live URL, use the `pagespeed` skill. Pair privacy/legal checks with `docs/web-compliance-checklist.md`.

Mark items `[x]` when done.

## How to run

1. Inventory representative templates/URLs: home, listing, detail, legal, search, pagination, app shells.
2. Audit the checks below; separate site-wide, template-wide, and URL-specific findings.
3. On deployed sites with performance in scope, run `pagespeed` (mobile first).
4. Fix crawl/index blockers first; batch by template.
5. Verify with fetched headers, source HTML, `robots.txt`, and XML sitemaps (not only the rendered UI).
6. After deploy, re-crawl and check Search Console when access exists.

## Crawlability and indexation

- [ ] Important canonical pages are reachable from nav or contextual links (shallow click depth)
- [ ] No accidental orphan pages or crawl traps (infinite params, faceted URL explosions)
- [ ] `robots.txt` blocks only low-value surfaces (admin, cart, internal search, unsafe params); never blocks core content, canonicals, or the XML sitemap by accident
- [ ] XML sitemap lists only canonical, indexable, successful URLs; referenced from `robots.txt` when present
- [ ] `noindex` is limited to genuinely non-indexable surfaces; no `noindex` vs `robots.txt` conflicts
- [ ] Clean, stable URLs; query/duplicate variants have an intentional canonical policy
- [ ] Primary content and links exist in crawlable HTML or reliably rendered output (not an empty client shell)

## Architecture and duplicates

- [ ] One preferred HTTPS host (apex or `www`); alternatives 301/308 with no chains or loops
- [ ] `NEXT_PUBLIC_APP_URL` / metadataBase match the production origin
- [ ] Indexable pages have self-canonicals; canonical targets are reachable and equivalent
- [ ] Logical URL hierarchy; descriptive anchors; breadcrumbs where they help
- [ ] Pagination (if any) uses discoverable URLs and a deliberate canonical policy (not everything → page one)
- [ ] Hreflang (if used) is reciprocal, self-referencing, and points at canonical equivalents

## On-page and rendering

- [ ] Each indexable template has a unique, useful title and meta description
- [ ] One H1 and a coherent heading hierarchy per template
- [ ] Open Graph / Twitter cards resolve (real `og-image` under `public/`, absolute URLs via metadataBase)
- [ ] Server-rendered or prerendered metadata, primary content, and structured data are available to crawlers where required
- [ ] Status codes are correct; no soft 404s for missing URLs; custom `not-found` returns 404
- [ ] No blocked critical assets; no hydration failures that strip content/links
- [ ] HTTPS consistent; mixed content does not block rendering

## Performance (technical SEO lens)

- [ ] LCP / INP / CLS checked for important templates when data exists; each failure traced to a cause (media, long tasks, layout shift, blocking resources)
- [ ] CrUX field data reported separately from Lighthouse lab data
- [ ] Performance called out as SEO only when it hurts crawl, rendering, or meaningful UX

## Structured data and machine surfaces

- [ ] JSON-LD (if any) uses the right type, matches visible content, and does not invent reviews/prices/availability
- [ ] `Organization.sameAs` (if used) only lists URLs that unambiguously represent the same entity; no directory padding
- [ ] `llms.txt` / docs entry points are accurate when shipped; public API URLs stay stable
- [ ] No cloaking: crawlers and users see materially the same content

## Template wiring checklist (this repo)

Implement or confirm these App Router pieces as the site grows:

- [ ] `src/app/robots.ts` (or equivalent) for `robots.txt`
- [ ] `src/app/sitemap.ts` (or equivalent) for the XML sitemap
- [ ] `src/app/not-found.tsx` for real 404s
- [ ] Per-route `metadata` / `generateMetadata` with unique titles
- [ ] `public/og-image.png` (or updated path) matching layout metadata
- [ ] Production indexing policy explicit (preview/staging should stay `noindex`)

## Finding format

```
[critical|major|minor] issue — area — url/template
  evidence: what was observed
  impact: why it matters
  fix: exact change (snippet if useful)
  verify: how to confirm
```

Areas: `Crawl`, `Index`, `Architecture`, `Rendering`, `Performance`, `Structured data`, `Hygiene`.

Prioritize crawl/index blockers, then architecture/rendering, then performance and enhancements. Separate observed evidence from assumptions.

## Related

- Skill: `technical-seo`
- Skill: `pagespeed` (live CWV / lab measurements)
- Skill: `website-compliance` (`docs/web-compliance-checklist.md`)
- This template: `src/app/layout.tsx` metadata, `next.config.mjs` redirects/headers
