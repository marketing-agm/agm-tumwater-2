# White Heather Apartments — Owner Presentation Micro-Site

**CONFIDENTIAL — owner meeting material. Private repository. Do not make public.**

Digital presentation for the owners of White Heather Apartments (12556 15th Ave NE, Seattle, WA 98125), prepared by AGM Real Estate Group for the Chen family. Built to be driven live in the owner meeting and left up afterward as a reference.

Single-file static site (`index.html`). No build step, no dependencies. Based on the AGM micro-site template (Tumwater format).

## Local preview
Open `index.html` in a browser. That's it.

## Pre-meeting checklist (content still to load)
Every pending item is flagged in-page with a yellow "to load / to confirm" note:
- **Financials** — budget, actuals, forecast, insurance figures
- **Leasing** — concession structure, comp table from Jerry's market comp report
- **Marketing** — channel costs and lead estimates
- **Gallery** — all photos (before / damage / progress / design options), as single data-URI `src` images
- **Agenda** — presenter names and team blurbs
- **Owner Portal** — set the AppFolio portal URL (`#portalLink`), confirm Chen family logins, upload documents to the filing cabinet
- **Overview** — confirm year built and the amenities list

Rent roll figures baked into the page are from the AppFolio rent roll dated June 11, 2026.

## Deploy — Cloudflare Pages (AGM standard pattern)
1. Cloudflare dashboard → **Workers & Pages → Create → Pages → Connect to Git** → select this repo.
2. Settings: Framework preset **None** · Build command **(empty)** · Build output directory **/**
3. Every push to `main` auto-deploys production; every branch/PR gets its own preview URL.

## REQUIRED before sharing any URL: Cloudflare Access
Zero Trust → Access → Applications → **Add self-hosted application**:
- Application domain: `<project>.pages.dev` **and** `*.<project>.pages.dev` (covers preview deployments — previews are otherwise public).
- Policy: Allow → Emails → AGM team + Chen family. One-time PIN works without SSO setup.

## Operational notes
- `_headers` enforces `noindex` and security headers at the edge.
- Analytics events are stubbed on `window.dataLayer` / `posthog` — add the PostHog snippet in `<head>` and set the project key to go live.
- OG image: set `og:image` to an absolute hosted URL at deploy (generic exterior only — link previews escape Access).
- To add imagery: each photo slot is a `.frame` div; convert the photo to a base64 data URI and swap the placeholder for `<div class="frame has-img"><img src="data:..." alt="..."/></div>`.

Managed by AGM Real Estate Group · 12330 Northup Way, Bellevue, WA 98005.
