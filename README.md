# DONNA-M — public pages for Mallory Adams, REALTOR®

Static pages served by GitHub Pages at **https://txasc.github.io/DONNA-M/**.

## Home Valuation landing page

**Live URL (printed on the QR code — do not move or rename):**
https://txasc.github.io/DONNA-M/home-valuation/

| Path | What it is |
|------|------------|
| `home-valuation/index.html` | The landing page: headline, form, KW logo + headshot band, KW-red page border. Mobile-first. |
| `home-valuation/qr.html` | Browser QR generator, pre-filled with the live URL. |
| `home-valuation/assets/home-valuation-qr.png` / `.svg` | Print-ready QR code (PNG 1320px; SVG for large-format signs). |
| `home-valuation/assets/kw-logo-white.png` | White KW Dallas Preston Road logo (needs a dark background). |
| `home-valuation/assets/mallory-headshot.webp` | Transparent headshot, resized to 640px (62 KB) from the 1.7 MB original. |
| `index.html` | Redirects the site root to the landing page. |

## HNA 2027 membership page (`/hna/`)

Same pattern as the home valuation page, for The Highlands Neighborhood Association.
Print-ready QR: `hna/assets/hna-qr.png` / `.svg`.

## Where the form goes

Both forms post to a private automation that emails the right person. If it
can't be reached, the page opens a pre-filled email in the visitor's own mail
app, so nothing is lost. Setup and operating notes are kept outside this
public repo.
