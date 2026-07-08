# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Miami Yacht Collective** — A static yacht rental lead-generation website for Miami.
- **Domain**: `https://miamiyachtcollective.com`
- **Goal**: Convert visitors into booking leads via Call or WhatsApp. No backend, no forms, no payment processing.
- **Origin**: Migrated from a Webflow "YachtLux" template. Currently in polish phase.
- **Stack**: Pure HTML/CSS/JS — no build tools, no frameworks. The `package.json` and `app/` directory contain an unused Next.js setup; the live site is the root HTML files served statically.

## Dev Server

```bash
python3 -m http.server 8000
# Open http://localhost:8000
```

## Architecture

### Two Layers (only the static layer is live)

1. **Static HTML (root)** — The actual production site. Each page is a standalone `.html` file with inline CSS for the booking modal, Webflow CSS (`public/css/`), and Webflow JS (`public/js/`).
2. **Next.js app (`app/`)** — Wraps the static HTML via `StaticPage` component that reads from `app/_static-pages/`. Not currently deployed or used. Ignore unless explicitly asked.

### Asset Locations

- `public/css/` — Webflow stylesheets (`miami-yacht-collective.webflow.css`, `normalize.css`, `webflow.css`)
- `public/js/` — `webflow.js` (Webflow runtime), `tracking.js` (GA4 event tracking)
- `public/images/` — Template images (backgrounds, avatars, icons) in `.avif`/`.svg`
- `public/photos/` — Yacht-specific photo galleries organized by boat (e.g., `ferretti-75/`, `isabella-48/`)
- `public/videos/` — Hero and section background videos (`.mp4`, `.webm` with poster `.jpg`)
- `Boat Photos/` — Source/legacy yacht photos (`.avif`, `.jpg`)

### Key Pattern: Booking Modal

Every page must include the booking modal markup and JS inline. It's not in an external file — it's copy-pasted into each HTML page's `<head>` (styles) and `<body>` (markup + script).

- CTA buttons use `data-modal` attribute to trigger the modal
- Modal offers: Call (787) 664-5040 or WhatsApp (wa.me/17876645040)
- `public/js/tracking.js` tracks `modal_open`, `call_click`, `whatsapp_click` via GA4

### SEO Validation

- `seo-reference.json` — Ground truth for titles, meta descriptions, headings, images, and links per page
- `seo_validate.py` — Validates HTML files in an `output/` directory against `seo-reference.json`
- Run: `python3 seo_validate.py` (expects files in `output/` subdirectory)

## Pages

| Page | File | Notes |
|------|------|-------|
| Homepage | `index.html` | Hero video, fleet grid (9 yachts), FAQ, CTA |
| Yacht detail pages | `isabella.html`, `maxum.html`, `ferretti.html`, `azimut.html`, `acgua-alberti.html`, `azimut-lchaim.html`, `deep-blue.html`, `anvera.html`, `axopar-brabus.html` | Photo galleries, specs, booking CTAs |
| Gallery | `gallery.html` | Photo grid with lightbox |
| Blog | `blog.html` | Blog listing |
| About | `about.html` | Company info |
| Services | `services.html` | Services overview |
| Contact | `contact.html` | Contact info |
| Booking | `booking.html` | Booking page |
| Fleet listing | `yacht.html` | Indexable fleet page (de-noindexed 2026-07-08) |
| SEO landing pages | `yacht-party-miami.html`, `birthday-yacht-party-miami.html`, `bachelorette-yacht-party-miami.html`, `private-sunset-cruise-miami.html`, `miami-yacht-rental-prices.html` | Intent pages targeting party/sunset/price searches. Keep facts in sync with llms.txt |
| Yamaha detail | `yamaha-255xd.html` | 10th fleet vessel (25' jet boat, $1,000/4hr) |
| Error pages | `401.html`, `404.html` | Error states |
| Blog articles | None yet (no `blog/` directory with posts) | — |

## Do NOT

- Add npm, webpack, or any build step
- Rename any HTML files
- Change any `<title>`, `<meta description>`, heading text (`h1`/`h2`/`h3`), image `alt` text, image `src` paths, or internal link `href` values without explicit instruction (2026-07-08: titles/metas/schema were deliberately optimized in an owner-requested SEO pass — see SEO-NOTES.md before "fixing" them back)
- Break the booking modal flow (Call + WhatsApp must remain on every page)
- Replace existing `.avif` or image references with different paths
- Modify `sitemap.xml` without keeping it in sync with the real page set (new pages must be added)
- State unverified amenities in copy (BYOB, fuel, catering, towels) — only captain+crew included, 13-guest max, Miami River departure and per-yacht prices are owner-confirmed facts
- `git add -A` — the working tree carries intentional uncommitted deletions (assets moved to untracked `public/` for a dormant Next.js experiment); stage files explicitly
- Add payment forms, login, or backend functionality

## Image Path Gotcha

Production runs on case-sensitive Linux. File names must be lowercase and URL-safe. Past bugs were caused by case mismatches between HTML `src` attributes and actual filenames on disk (macOS is case-insensitive, Linux is not).

## Social

- Instagram: `https://www.instagram.com/miamiyachtcollective/`
