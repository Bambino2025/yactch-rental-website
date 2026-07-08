# SEO Work Notes — Miami Yacht Collective

Running log so future sessions build on this instead of repeating it. One lesson per entry, summary first.

---

## 2026-07-08 session (Claude, autonomous SEO run)

### Lesson: The live site and the local working tree have diverged — trust git, not the filesystem
The owner (or a Next.js experiment) moved `css/`, `js/`, `images/`, `photos/`, `videos/` into an untracked `public/` dir, leaving ~430 uncommitted deletions in the working tree. The live site still serves the committed root-level dirs. Never `git add -A` in this repo; stage files explicitly. I restored `css/ js/ images/` locally via `git checkout --` for verification only.

### Lesson: Owner standardized all guest capacities to "up to 13 guests" (uncommitted edits, likely USCG 12-passenger + captain rule)
index.html/yacht.html/detail pages all edited to 13. `llms.txt` still said 30/20/15 — fixed this session to match. Any new content must say max 13 guests per charter.

### Lesson: Prior session's audit (2026-05-17) already shipped the critical fixes
sitemap.xml, robots.txt, llms.txt, LocalBusiness+Service+Breadcrumb schema, template brand-name cleanup are all live and verified (curl 200s this session). Don't redo. Remaining gaps found this session:
- No FAQ content or FAQPage schema anywhere
- No Open Graph / Twitter Card tags on any page
- services/contact/booking missing meta descriptions; booking has no H1
- yacht.html (the nav "Yachts" fleet page!) was noindexed + robots-blocked as a "template artifact" but is linked 4x from homepage nav — wasted link equity
- yacht.html + detail_*.html linked to yamaha-255xd.html which 404s (boat photos exist in public/photos/yamaha-255xd-25/ but page was never created; no price/capacity facts available, so card removed rather than page invented)
- No dedicated intent pages for "yacht party miami" / "sunset cruise miami" (the two biggest non-brand intents we can win); party photos exist in public/photos/party/
- priceRange "$$$" instead of numeric; no hasOfferCatalog

### Lesson: Facts for content must come from llms.txt + yacht page schemas
Verified facts usable in copy: fleet 28–90 ft, from $899/4hr (Maxum) to $3,000/4hr (Deep Blue), licensed captain + crew included on every charter, departs 668 NW N River Dr on the Miami River (10 min Brickell / 15 min Miami Beach), 24/7, booking via call/WhatsApp (787) 664-5040, areas: Biscayne Bay, Star Island, Fisher Island, Stiltsville, Nixon's Sandbar. DO NOT invent: fuel policy, BYOB policy, cancellation policy, catering details — owner never confirmed these.

### Party photo filenames contain other businesses' names
public/photos/party/ has files like "best-miami-boat-rentals_-aquarius-boat-rental-...jpg" (scraped from Instagram/Pinterest). When using, copy + rename to neutral SEO names (miami-yacht-party-XX.jpg) and skip photos watermarked/branded by competitors.

### Lesson: Occasion pages are how small operators win Miami yacht SERPs (research-backed)
Fresh SERP research (July 2026, 15 searches): "birthday yacht party miami" and "bachelorette yacht miami" are won exclusively by small local operators with DEDICATED landing pages (feelingyachty, primeluxuryrentals, onkor, vistayachts) — zero marketplaces. "miami yacht rental prices" is won by standalone price-guide pages with real numbers and a year in the title. "sunset cruise miami" head term is Viator/$14-ticket territory — only winnable with the "private" modifier. "boat rental miami" is marketplace-walled; don't chase it. Head term "yacht rental miami" is winnable long-term (miamiyachtingcompany holds #1 as a local operator).

### Shipped this session (2026-07-08)
- 6 NEW pages: yacht-party-miami.html, birthday-yacht-party-miami.html, bachelorette-yacht-party-miami.html, private-sunset-cruise-miami.html, miami-yacht-rental-prices.html (2026 price table, all 10 yachts), yamaha-255xd.html (fixes the 404 fleet card)
- services.html REBUILT: was Webflow template junk ("Donut Ride"/"Banana Ride" cards hotlinking another site's CDN) → now the Experiences hub linking all occasion pages
- yacht.html rehabilitated: removed noindex + robots block, real title/meta/canonical, ItemList schema, H1 "Yachts & Boats for Rent in Miami"
- index.html: title → "Yacht Rental Miami | Private Charters from $899…", new meta, 6-question FAQ section + FAQPage schema, LocalBusiness priceRange "$899 - $3,000" + hasOfferCatalog (10 offers), "Yachtlux Club" artifact reworded
- Site-wide: "Experiences" nav link on all pages, footer "Experiences" column (6 links), full OG/Twitter tags on all 22 indexable pages (9 pages had stale duplicate og:description tags from Webflow — removed)
- contact/booking: meta descriptions + intent titles; booking got an H1 (promoted existing h2); gallery H1 aligned to party intent
- sitemap.xml: +7 URLs, lastmod 2026-07-08; robots.txt: yacht.html unblocked; llms.txt: 10 vessels, capacities fixed to 13, experience pages listed

### Lesson: f-string page generation ate a JS brace — always console-check generated pages
Generated pages had `WebFont.load({...})` missing a closing brace (f-string `{{` escaping), which threw "Unexpected token ')'" on every new page. Caught only via Playwright console. Any future template generation: load the page headless and assert zero console errors (excluding local-only 404s for /_vercel/insights and /videos/*).

### Lesson: verify with a fresh-context agent — it caught what self-review missed
The adversarial verifier found: stale duplicate og:description tags (first-tag-wins for WhatsApp/FB previews — the old one advertised "onboard catering", an unverified amenity), the "Yachtlux Club" template artifact, and an implied-champagne phrase. All fixed. It also confirmed: all 25 JSON-LD blocks parse with no duplicate keys, all prices/capacities consistent across text+schema+llms.txt, all links/sitemap/robots/modals clean.

### Open items / caveats for next session
- seo-reference.json is now STALE (predates this pass) — seo_validate.py comparisons against it will fail; regenerate reference before using
- Party photos (photos/party/) are Pinterest/Instagram-sourced third-party images the owner collected. Copyright + authenticity risk. Owner should replace with real charter photos ASAP; keep filenames/dimensions to avoid touching HTML
- Owner's uncommitted "up to 13 guests" edits on index/yacht/detail pages were included in this commit (consistent, intentional). detail_*.html template pages still carry uncommitted edits — left unstaged (robots-blocked artifacts)
- Titles run 77–91 chars (keyword+price front-loaded, brand suffix truncates in SERPs) — deliberate
- Email in schema/llms.txt is still sebastian@mcaiconsulting.com — action plan item 15 (business-domain email) still open
- Off-site work still owned by Sebastian: GBP services/photos/reviews cadence, Yelp/TripAdvisor listings, Bing Places/Apple Business Connect, 305/786 number
