# Timeless Interiors — SEO Update Handoff

This is an SEO-optimized rebuild of the original single-page site handoff
(`timeless-interiors-handoff/index.html`). It's still a plain static site —
no build tools, no frameworks, nothing to install — so it deploys the same
way the original did.

## What changed vs. the original handoff

**Structure — 1 page became 13:**
- Homepage (same content/design, upgraded SEO)
- 3 service pages: `/bathroom-renovation-calgary/`, `/basement-finishing-calgary/`, `/general-contracting-calgary/`
- 4 service-area pages: `/airdrie/`, `/cochrane/`, `/chestermere/`, `/okotoks/`
- Blog: `/blog/` index + 3 posts (bathroom warning signs, basement cost guide, fixed-price vs. hourly quotes)
- `/faq/` — dedicated FAQ page

**Meta tags — every page now has:** a unique title (keyword + city), unique
meta description (with phone number), keywords, canonical URL, Open Graph
tags, and a Twitter Card.

**Structured data (JSON-LD) — every page now has:**
- `HomeAndConstructionBusiness` (homepage) with `aggregateRating` and the
  3 real reviews already shown on the page
- `Service` schema on each service page
- `FAQPage` schema on the homepage, FAQ page, and each service page (shows up as expandable Q&A in Google search results)
- `BlogPosting` schema on each blog post
- `BreadcrumbList` schema on every subpage

**Images:** pulled out of the base64 blob in the old file into real, compressed
`.jpg` + `.webp` files with descriptive names (e.g.
`calgary-kitchen-renovation-timeless-interiors.jpg`) and keyword+city alt text.
Favicons, apple-touch-icon, and a web manifest were generated from the logo.

**Technical files added:** `sitemap.xml` (lists all pages + images),
`robots.txt`, `site.webmanifest`, and `styles.css` (styles extracted out of
the inline `<style>` block so they're shared and cached across pages instead
of duplicated on every load).

## Two content fixes made along the way

1. **"25+ years" vs. "since 2023"** — the original file said both, which
   contradicted itself. Per direction from the business, everything now
   says **since 2023**, and the "25+ years" trust stat was replaced.
2. **Placeholder address removed** — the original JSON-LD had a fake street
   address (`1234 Renovation Ave NW`). Since there's no public storefront,
   the site is now marked up as a **service-area business** (Calgary +
   Airdrie, Cochrane, Chestermere, Okotoks) with no address, which is the
   correct schema.org pattern for contractors — and avoids Google flagging
   a fake address against the real Google Business Profile.

## Deployment

Nothing has changed about how this deploys — upload the whole folder to any
static host: Netlify, Vercel, GitHub Pages, Cloudflare Pages, or plain
FTP/SFTP. To preview locally:

```
npx serve .
```

One requirement: the host must serve `/some-page/index.html` when a visitor
requests `/some-page/` (clean folder-style URLs). Every static host listed
above does this by default — nothing extra to configure.

## Round 2 additions (Search Console + further SEO)

- **Google Search Console verification** — `googleccf56000b6b3a580.html` was
  added at the site root with the exact content Google's HTML-file
  verification method requires. Once this is deployed live, verification in
  Search Console should complete automatically (click "Verify" on the
  property). **After it's live, submit `sitemap.xml`** in Search Console →
  Sitemaps, and use URL Inspection → Request Indexing on the homepage and a
  couple of the new service pages to speed up first indexing.
- **`WebSite` schema** — added to every page (in addition to the existing
  `HomeAndConstructionBusiness`/`Service`/`FAQPage`/`BlogPosting`/
  `BreadcrumbList` schema), so Google has a clear entity for "the site
  itself" distinct from the business.
- **`ContactPoint` schema** — added inside the homepage's business schema
  (phone, email, service area) — this is what can power a "call" action
  button directly in search results/Knowledge Panel.
- **hreflang tags** — self-referencing `en-ca` + `x-default` added to every
  page. Minor, but correct practice and rules out any language/region
  ambiguity for Google.
- **Custom `404.html`** — replaces whatever generic "not found" page the
  host would otherwise show. Links back to home, two popular service pages,
  FAQ, and the blog, so lost visitors (and lost link equity from any broken
  backlinks) aren't a dead end. Marked `noindex, follow` so it never
  competes with real pages in search results.
- **Deeper internal linking** — each service page now links to its matching
  blog post; each blog post links to the other two posts; each location
  page links to the FAQ and blog. (Previously, blog↔service/location linking
  only ran one direction.)
- **`_headers` file** — sets long `Cache-Control` lifetimes (1 year) on
  images, CSS, and icons, and no-cache on HTML so page edits always show up
  immediately. Netlify and Cloudflare Pages read this file automatically. If
  hosting elsewhere (Vercel, plain Apache/Nginx, FTP host), the same
  cache rules need to be set in that host's own config — ask your host or
  developer how to add cache headers there; the values to use are in that
  file.

## Please verify before/at launch

- **Domain** — all canonical/Open Graph URLs assume `https://timelessinteriorsltd.com`. If that ever changes, it needs a find-and-replace across every HTML file plus `sitemap.xml`.
- **Social links** — `sameAs` in the JSON-LD points to `instagram.com/timelessinteriors` and `facebook.com/timelessinteriors`, carried over unchanged from the original file. Please confirm these are the real, live profiles.
- **Lead form** — the GoHighLevel embed (form ID `RhsEyIreH89gn81AYr6M`) is unchanged from the original — confirm it's still the correct form/account.
- **Reviews** — the `aggregateRating` (5.0 from 3 reviews) mirrors the 3 testimonials visibly shown on the homepage. If more reviews get added to the page, update the count/rating in the JSON-LD to match — Google penalizes structured data that doesn't match what's visible on the page.

## After launch (off-site — can't be done from the code)

1. **Google Search Console** — submit `sitemap.xml`, request indexing for each page.
2. **Google Business Profile** — claim/verify it, post weekly, add photos, get reviews.
3. **Directory listings** — HomeStars, Yelp, BBB, YellowPages.ca. Keep the business name, phone number, and address (or service-area listing) identical everywhere — Google cross-checks this.
4. **Share new pages on social** after launch to speed up indexing.

## Full page list

| URL | Purpose |
|---|---|
| `/` | Homepage |
| `/bathroom-renovation-calgary/` | Bathroom renovation service page |
| `/basement-finishing-calgary/` | Basement finishing service page |
| `/general-contracting-calgary/` | General contracting service page |
| `/airdrie/` | Airdrie service-area page |
| `/cochrane/` | Cochrane service-area page |
| `/chestermere/` | Chestermere service-area page |
| `/okotoks/` | Okotoks service-area page |
| `/faq/` | Full FAQ page |
| `/blog/` | Blog index |
| `/blog/signs-your-bathroom-needs-a-renovation/` | Blog post |
| `/blog/basement-finishing-cost-guide-calgary/` | Blog post |
| `/blog/fixed-price-vs-hourly-contractor-quotes/` | Blog post |
