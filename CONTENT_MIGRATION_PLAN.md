# Content Migration Plan — 12-Tab Investor Journey

Reorganizes the micro-site from a diligence-led layout into a 12-tab investor journey:
**why this matters → why now → what it can become → what is being acquired → how value is created → how to engage.**

All previously-built tabs (the v3 "Olympia Brewery District" set, the v2 narrative set, and the
original tabs) are preserved and remain reachable under a secondary **Archive / Old Tabs** dropdown
placed last in the navigation. Nothing is deleted.

## Classification buckets
1. **Lead Narrative** — rewrite lightly, place near top of relevant new tab.
2. **Proof Point** — convert to cards, stat blocks, tables, timelines, graphics.
3. **Diligence Detail** — move to Tab 8 / Tab 9, often in accordions.
4. **Archive** — keep in Old Tabs, not featured in the journey.

## New primary navigation (routes)
| # | Tab | Route |
|---|-----|-------|
| 1 | Executive Summary | `executive-summary` |
| 2 | The Investment Opportunity | `investment-opportunity` |
| 3 | Investment Thesis | `investment-thesis` |
| 4 | The City Is Investing First | `city-investing-first` |
| 5 | Development Vision | `dev-vision` |
| 6 | Location | `location-overview` |
| 7 | Regional Growth Drivers | `growth-drivers` |
| 8 | Development Advantages | `development-advantages` |
| 9 | Property Overview | `property-overview` |
| 10 | Master Plan Opportunities | `master-plan` |
| 11 | Financial Upside | `financial-upside` |
| 12 | Call to Action | `call-to-action` |

Secondary: **Archive / Old Tabs** dropdown → existing routes (`summary, relevance, thesis,
public-redevelopment, development-vision, location, growth` [v3]; `opportunity, vision,
public-investment, due-diligence, sources, request` [v2]; `home, presentation, use-resort,
use-club, use-tribal, use-investors, history, site, news, contact` [original]).

## Source → destination mapping
| Existing content (location) | Bucket | New destination | Action |
|---|---|---|---|
| Hero / at-a-glance (v3 `summary`) | Lead/Proof | Tab 1 | Rewrite to approved exec-summary copy; keep metric strip; add Why It Matters / Development Potential / Execution Considerations callouts |
| Historic asset / public-sector / regional cards (v3 `relevance`, `summary`) | Lead | Tab 2 | Three concise icon cards (Historic Asset, Public-Sector Alignment, Regional Growth) |
| Thesis pillars (v3 `thesis`) | Lead/Proof | Tab 3 | Five pillar cards (approved copy): Public Investment, Entitlement Fast Track, Historic Tax Credit, Regional Destination, Institutional Scale |
| Public investment table / incentives / estuary (v3 `public-redevelopment`, v2 `public-investment`) | Proof | Tab 4 | Public-investment flow graphic + alignment-theme cards; commitments table reused |
| Interactive **timeline** (12 eras) (v3 `relevance`) | Proof | **Tab 9** | Relocated intact (shared `#tlModal`) as the heritage timeline |
| Interactive **floor plan + building inventory** (v3 `relevance`) | Proof/Diligence | **Tab 9** | Relocated intact (shared `#bldgModal`); approved building copy |
| Vision board / program / "Suncadia Meets the Distillery District" (v3 `development-vision`) | Lead/Proof | Tab 5 & Tab 10 | Vision board + use mix in Tab 5; scenarios/phasing in Tab 10 |
| Regional map / nodes / proximity (v3 `location`) | Proof | Tab 6 | Map placeholder + node chips + proximity cards + adjacencies |
| Government / JBLM / tourism / logistics stats (v3 `growth`) | Proof | Tab 7 | Six driver icon cards with stats |
| Zoning / entitlement / environmental / floodplain / critical areas / infrastructure (v3 `due-diligence`) | Diligence | Tab 8 | Advantage cards + Supporting Diligence accordions |
| Parcels / acreage / buildings / utilities (v3 `due-diligence`, `relevance`) | Diligence | Tab 9 | Parcel table, site-at-a-glance, building inventory cards |
| Program mix / phasing / scenarios (v3 `growth`, `development-vision`) | Proof | Tab 10 | Scenario grid + program framework + phasing |
| Value creation / market capture / incentive stack / job creation (v3 `growth`, `public-redevelopment`) | Proof | Tab 11 | Incentive stack, value-creation timeline, capture tables (illustrative) |
| Contact / process / data room / NDA (v2 `request`, v3) | Lead | Tab 12 | Contact panel + data-room + confidentiality + CTAs |
| Everything previously built | Archive | Old Tabs dropdown | Kept accessible, secondary |

## Language
Global replacement of prohibited phrasing (`de-risk`, `de-risking`, `risk profile`, `risk sharing`,
`basis risk`, `risk a developer would otherwise carry alone`) with institutional alternatives
(`improves redevelopment readiness`, `creates investor clarity`, `defines the execution path`,
`public-private participation`, `improves basis through public-sector alignment`, etc.) across the
whole document, including archived pages.

## Notes
- Single-file static site (`index.html`); no build step. "Build/lint/test" = HTML tag-balance,
  CSS brace-balance, and `node --check` on the inline script; all run before commit.
- Interactive widgets are relocated (not copied) to keep element IDs unique; shared modals
  (`#tlModal`, `#bldgModal`, `#lightbox`) remain global.
