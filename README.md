# The Tumwater Brewery — Marketing Micro-Site (v2)

**CONFIDENTIAL — pre-market offering material. Private repository. Do not make public.**

Single-file static site (`index.html`, all imagery embedded as data URIs). No build step, no dependencies.

## Local preview
Open `index.html` in a browser. That's it.

## Deploy — Cloudflare Pages (AGM standard pattern)
1. Cloudflare dashboard → **Workers & Pages → Create → Pages → Connect to Git** → select this repo.
2. Settings: Framework preset **None** · Build command **(empty)** · Build output directory **/**
3. Every push to `main` auto-deploys production; every branch/PR gets its own preview URL.

## REQUIRED before sharing any URL: Cloudflare Access
Zero Trust → Access → Applications → **Add self-hosted application**:
- Application domain: `<project>.pages.dev` **and** `*.<project>.pages.dev` (covers preview deployments — previews are otherwise public).
- Policy: Allow → Emails → leadership list. One-time PIN works without SSO setup.

## Operational notes
- `_headers` enforces `noindex` and security headers at the edge.
- Analytics events are stubbed on `window.dataLayer` / `posthog` — add the PostHog snippet in `<head>` and set the project key to go live.
- OG image: set `og:image` to an absolute hosted URL at deploy (generic image only — link previews escape Access).
- News & Developments page is structured for a scheduled research routine to append dated entries and push (auto-deploys on push).
- To swap imagery (e.g., unwatermarked GCH renderings, commissioned photography): each image is a single `src` data URI in `index.html`.

Selling principal: Troy Gessel, AGM Real Estate Group.
