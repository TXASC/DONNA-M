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
| `n8n/home-valuation-lead.json` | Export of the n8n workflow that emails Mallory. |
| `index.html` | Redirects the site root to the landing page. |

### How a lead flows

```
Client scans QR -> landing page (GitHub Pages)
   -> POST https://jarvis.tailb14d90.ts.net/webhook/home-valuation   (Tailscale Funnel, /webhook/ path only)
   -> n8n on the JARVIS tower: "DONNA-M — Home Valuation Lead -> Email Mallory"
        1. Webhook (CORS locked to https://txasc.github.io)
        2. Validate + Format Lead: drops honeypot/bot and incomplete submissions, HTML-escapes input
        3. Gmail -> mallorya@kw.com, Reply-To set to the lead's email
```

If the webhook can't be reached (tower off, n8n down), the page opens a
pre-filled email to mallorya@kw.com in the visitor's own mail app, so the lead
is not silently lost.

### Payload the page sends

```json
{
  "fullName": "Jane Smith",
  "phone": "(214) 555-0100",
  "email": "jane@email.com",
  "address": "123 Main St, Prosper, TX 75078",
  "timeframe": "1-3 months",
  "notes": "",
  "website": "",
  "source": "home-valuation-landing-page",
  "submitted_at": "2026-09-17T21:00:00.000Z"
}
```

`website` is a hidden honeypot field; real visitors never fill it.

### Operating notes

- **Leads depend on the tower being up.** n8n runs in Docker on JARVIS; the
  public path is `tailscale funnel --bg --set-path /webhook/ http://127.0.0.1:5678/webhook/`.
  The n8n editor itself is not exposed. Run it from PowerShell, or from Git Bash with
  `MSYS_NO_PATHCONV=1` set; otherwise Git Bash rewrites `/webhook/` to
  `C:/Program Files/Git/webhook/` and the route silently points nowhere.
- **If the n8n editor won't load but the container is "Up":** Docker's port
  forward went stale (empty reply on :5678 while `docker exec n8n wget -qO- http://127.0.0.1:5678/healthz`
  says ok). `docker restart n8n` restores it.
- **Re-importing the workflow:** `docker cp n8n/home-valuation-lead.json n8n:/tmp/hv.json`
  then `docker exec n8n n8n import:workflow --input=/tmp/hv.json` (from Git Bash,
  set `MSYS_NO_PATHCONV=1` first). Re-attach the Gmail credential afterwards.
- **Not yet wired:** logging the lead into the DONNA-M CRM (cloud Supabase,
  schema `mallory`). Add a node after "Validate + Format Lead" when wanted.
