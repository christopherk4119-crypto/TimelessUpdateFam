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

## Wave 1 corrections (accuracy + IA restructure)

This wave fixed several claims that didn't match reality, and reorganized the
service pages. Everything below **supersedes** the "Round 2" section above
where they conflict.

- **Removed self-serving review markup.** Deleted `aggregateRating` and the
  `review` array from the homepage's JSON-LD entirely — Google doesn't allow
  review rich-result markup for reviews a business hosts about itself, and
  the numbers (5.0 from 3) didn't match the real Google Business Profile
  (4.6 from 9). The 3 testimonials are still visible on the page, just no
  longer in structured data.
- **Hero stats corrected** — `5★` → `4.6★` (matches GBP), and the unverified
  `200+ Projects completed` stat was replaced with `Bathrooms / Our
  specialty`.
- **Social URLs fixed** — Instagram is now `instagram.com/timelessinteriorsyyc`,
  Facebook is now `facebook.com/profile.php?id=61560009476247`, in both the
  footer and `sameAs`. The previous URLs in this doc's "Please verify"
  section above were wrong — this is the correct, current information.
- **Business name matches the Google Business Profile exactly** — `Timeless
  Interiors Ltd` (no trailing period) everywhere, including titles, meta
  tags, JSON-LD `name` fields, footer, and alt text. Added `alternateName`
  (`Timeless Interiors YYC`, `Timeless Interiors`) to the business schema to
  preserve the older name variants used in existing citations. The footer
  copyright line keeps a period, but on a rewritten sentence (`Timeless
  Interiors Ltd — All rights reserved.`) so it isn't reusing the abbreviation's
  own period as the sentence-ender.
- **Saturday hours reconciled** — now `9am–5pm` everywhere (was inconsistently
  "By appointment" / 9–3 in different places); Google Business Profile is the
  source of truth.
- **Service pages restructured under `/renovation/`:**
  - `/renovation/` — new hub page
  - `/renovation/bathroom-renovation-calgary/` — moved (was `/bathroom-renovation-calgary/`)
  - `/renovation/basement-finishing-calgary/` — moved (was `/basement-finishing-calgary/`)
  - `/general-contracting-calgary/` — **retired**, 301-redirects to `/renovation/`
  - `_redirects` file added (Netlify/Cloudflare Pages syntax) for the above
  - Every internal link across the site was repointed to the new URLs directly (not through the redirect)
- **Location pages noindexed** — `/airdrie/`, `/cochrane/`, `/chestermere/`,
  `/okotoks/` are now `noindex, follow` and removed from `sitemap.xml`. They're
  thin, near-duplicate template pages (the doorway-page pattern Google's spam
  policies name explicitly) with no real local content yet. They're still
  live and still linked from the footer — just not asking Google to index
  them until each one has genuine local content (a completed project photo
  from that city, that municipality's permit process, etc.). **To bring one
  back:** remove its `noindex` meta tag and the HTML comment at the top of
  the file, then re-add its `<url>` entry to `sitemap.xml`.
- **Two scaffold pages added, not live yet:** `/renovation/secondary-suite-calgary/`
  and `/painting/`. Both are `noindex`, not linked from anywhere, not in the
  sitemap, and contain only placeholder/TODO content — they exist so the
  page structure (nav, footer, schema shape) is ready once real content is
  written. **Do not deploy these as customer-facing pages as-is** — every
  visible section and FAQ answer is a `TODO` placeholder pending an owner
  interview. See the HTML comments in each file for exactly what's still
  unanswered (for secondary suites: who pulls permits, who cuts egress
  windows, Secondary Suite Registry handling, suite count, pricing; for
  painting: interior vs. exterior, cabinet refinishing, and popcorn-ceiling/
  asbestos-testing implications for pre-1990 homes).

## Please verify before/at launch

- **Domain** — all canonical/Open Graph URLs assume `https://timelessinteriorsltd.com`. If that ever changes, it needs a find-and-replace across every HTML file plus `sitemap.xml`.
- **Lead form** — the GoHighLevel embed (form ID `RhsEyIreH89gn81AYr6M`) is unchanged from the original — confirm it's still the correct form/account.
- **Pre-existing title/description length overruns** — not touched this wave (out of scope), but worth knowing about: `blog/index.html`, both blog post pages, and `faq/index.html` have titles over 60 characters; `index.html`, `airdrie/`, `cochrane/`, `chestermere/`, `okotoks/`, and `renovation/bathroom-renovation-calgary/` have meta descriptions over 155 characters.

## After launch (off-site — can't be done from the code)

1. **Google Search Console** — submit `sitemap.xml`, request indexing for `/renovation/` and the two moved service URLs specifically (they changed addresses).
2. **Google Business Profile** — claim/verify it, post weekly, add photos, get reviews.
3. **Directory listings** — HomeStars, Yelp, BBB, YellowPages.ca. Keep the business name (`Timeless Interiors Ltd`, no period), phone number, and service area identical everywhere — Google cross-checks this.
4. **Share new pages on social** after launch to speed up indexing.
5. **Run the homepage, `/renovation/`, and one service page through Google's Rich Results Test** before/after the sitemap resubmission, and wait for recrawl before shipping a next wave.

## Full page list

| URL | Purpose | Indexed? |
|---|---|---|
| `/` | Homepage | Yes |
| `/renovation/` | Renovation services hub | Yes |
| `/renovation/bathroom-renovation-calgary/` | Bathroom renovation service page | Yes |
| `/renovation/basement-finishing-calgary/` | Basement finishing service page | Yes |
| `/renovation/secondary-suite-calgary/` | Secondary suite — **scaffold, TODO content** | No (`noindex`) |
| `/painting/` | Interior painting — **scaffold, TODO content** | No (`noindex`) |
| `/airdrie/` | Airdrie service-area page | No (`noindex`) — thin content |
| `/cochrane/` | Cochrane service-area page | No (`noindex`) — thin content |
| `/chestermere/` | Chestermere service-area page | No (`noindex`) — thin content |
| `/okotoks/` | Okotoks service-area page | No (`noindex`) — thin content |
| `/faq/` | Full FAQ page | Yes |
| `/blog/` | Blog index | Yes |
| `/blog/signs-your-bathroom-needs-a-renovation/` | Blog post | Yes |
| `/blog/basement-finishing-cost-guide-calgary/` | Blog post | Yes |
| `/blog/fixed-price-vs-hourly-contractor-quotes/` | Blog post | Yes |

`/general-contracting-calgary/` no longer exists — it 301-redirects to `/renovation/` via `_redirects`.
