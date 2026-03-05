# CLAUDE.md

## Project Overview

**Miami Yacht Collective** — A static yacht rental lead-generation website.
Goal: Convert visitors into booking leads via Call or WhatsApp.
No build tools, no frameworks, no package manager. Pure HTML/CSS/JS.

## Business Context

- **Primary CTA**: Every "Book" button triggers a modal with two options:
  - Call: (787) 664-5040
  - WhatsApp: links to wa.me with pre-filled message
- **Conversion goal**: Drive phone/WhatsApp contact — that IS the booking flow.
- **No backend**: No forms, no database, no payment processing.

## Dev Server
```bash
python3 -m http.server 8000
# Open http://localhost:8000
```

## File Structure

- `index.html` — Landing page: hero, fleet grid, CTA sections, footer, booking modal
- `detail.html` — Yacht detail: image carousel, specs, booking modal. Yacht selected via `?yacht=` param
- `Boat Photos/` — Yacht images in `.avif` format
- `MiamiYachtCollective Logo.png` — Navbar logo

Both pages are **self-contained** (CSS + JS inlined). No shared files. Booking modal markup is intentionally duplicated.

## Fleet Keys

| Key | Yacht |
|-----|-------|
| `isabella` | Isabella |
| `maxum` | Maxum |
| `ferretti` | Ferretti |
| `azimut` | Azimut |
| `acgua-alberti` | Acgua Alberti |

Links format: `detail.html?yacht=<key>`

## Design System
```css
--cyan: /* primary accent */
--navy: /* dark background */
--heading-font: 'Barlow Condensed'
--yacht-name-font: 'Bebas Neue'
--body-font: system stack
--card-radius: /* card border radius */
```

Breakpoints: `768px` (tablet), `480px` (mobile)

## Key Conventions

- Booking modal triggered by `data-modal` attribute on any Book button
- Modal offers: Call (787) 664-5040 | WhatsApp deep link
- Never add a payment form, login, or backend — this is intentionally lead-gen only
- Preserve `.avif` image format (performance-optimized)
- Keep CSS/JS inlined per file — do not externalize

## Do NOT

- Add npm, webpack, or any build step
- Create shared CSS/JS files
- Break the booking modal flow
- Replace `.avif` images with heavier formats
