# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Miami Yacht Collective** — A static yacht rental lead-generation website.
Goal: Convert visitors into booking leads via Call or WhatsApp.
No build tools, no frameworks, no package manager. Pure HTML/CSS/JS served directly from files.

## Dev Server

```bash
python3 -m http.server 8000
# Open http://localhost:8000
```

## Business Context

- **Primary CTA**: Every "Book" button triggers a modal with two options:
  - Call: (787) 664-5040
  - WhatsApp: links to wa.me/17876645040
- **Conversion goal**: Drive phone/WhatsApp contact — that IS the booking flow.
- **No backend**: No forms, no database, no payment processing. Intentionally lead-gen only.

## Architecture

Every HTML page is **self-contained** — CSS and JS are inlined. There are no shared stylesheets or script files. Booking modal markup is intentionally duplicated across all pages.

### Page Types

| Type | Pages | Layout |
|------|-------|--------|
| **Landing** | `index.html` | Hero, fleet grid, CTA, FAQ, footer, modal |
| **Yacht Detail** | `isabella.html`, `maxum.html`, `ferretti.html`, `azimut.html`, `acgua-alberti.html` | Static hardcoded content per yacht (carousel, specs, description, modal) |
| **Legacy Detail** | `yacht-details.html` | JS-driven detail page via `?yacht=` param (kept for backwards compat) |
| **Service Landing** | `yacht-parties-miami.html`, `boat-tours-miami.html`, `yacht-catering-miami.html`, `bachelorette-yacht-miami.html`, `corporate-yacht-miami.html` | Keyword-targeted service pages (content card, CTAs, modal) |
| **Gallery** | `gallery.html` | Navy background, masonry photo grid, fullscreen lightbox, modal |
| **Blog Landing** | `blog.html` | Navy background, article card grid |
| **Blog Articles** | `blog/*.html` | White content card on #f5f5f5, article body with H2 sections, modal |

### Key Pattern: Individual yacht pages have hardcoded HTML content (not JS-loaded) so Google can crawl all text. This was an intentional SEO decision — do not convert back to JS-driven content.

## Fleet Data

| Yacht | File | Image | Price | Guests |
|-------|------|-------|-------|--------|
| Isabella 48' W/ Jetski | `isabella.html` | `Boat Photos/Isabella.avif` | $1,100 | 13 |
| Maxum 46' | `maxum.html` | `Boat Photos/maxum.avif` | $899 | 10 |
| Ferretti 75' | `ferretti.html` | `Boat Photos/ferreti.avif` | $1,890 | 20 |
| 55' Azimut | `azimut.html` | `Boat Photos/azimut.avif` | $1,400 | 15 |
| 90' Acgua Alberti | `acgua-alberti.html` | `Boat Photos/acgua-alberti.avif` | $2,500 | 30 |
| 25' Yamaha 255XD | `yamaha-255xd.html` | `25' Yamaha 255XD Easy/Easyfowey-1.jpg` | $TBD | 10 |

Note: `ferreti.avif` is intentionally spelled without the double 't' — that's the actual filename.

## Design System

```css
--cyan: #00BFFF        /* primary accent */
--cyan-hover: #00a8e0
--navy: #0D1B4B        /* dark backgrounds */
--heading-font: 'Barlow Condensed', sans-serif
--yacht-name-font: 'Bebas Neue', sans-serif
--body-font: system stack
--card-radius: 20px
```

Breakpoints: `768px` (tablet), `480px` (mobile)

## Navbar Structure

All pages share the same navbar pattern (duplicated per file):
- Logo: `MiamiYachtCollective Logo.png` → links to `index.html`
- Blog button (outlined white pill, `btn-gallery-nav`)
- Gallery button (outlined white pill, `btn-gallery-nav`)
- Book button (cyan, triggers modal via `data-modal`)
- Blog article pages in `blog/` use `../` prefix for all asset and link paths

## Conventions

- Booking modal triggered by `data-modal` attribute on any element
- Modal offers: Call (787) 664-5040 | WhatsApp deep link
- Images use `.avif` format (performance-optimized)
- CSS/JS inlined per file — do not externalize
- Navbar adds `.scrolled` class on scroll (>60px) for shadow effect
- `sitemap.xml` must be updated when adding/removing pages
- Deployment: `git push origin main` (auto-deploys)
- Domain: `https://miamiyachtcollective.com`
- Social: Instagram at `https://www.instagram.com/miamiyachtcollective/`

## Do NOT

- Add npm, webpack, or any build step
- Create shared CSS/JS files — each page is self-contained
- Break the booking modal flow
- Replace `.avif` images with heavier formats
- Convert static yacht pages back to JS-driven content loading
- Add payment forms, login, or backend functionality
