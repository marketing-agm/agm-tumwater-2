# PostHog Analytics — Tracking Reference & Dashboard Spec

This microsite is a single-page app with hash routing (`#/<tab>`). PostHog is initialized in
`index.html` `<head>` and all custom events go through the `track(event, props)` helper.

## Init configuration (already live)

- `autocapture: true` — every element click/link is captured automatically
- `capture_pageview: false` — SPA pageviews are fired manually per tab in `showRoute()`
- `capture_pageleave: true` — time-on-page / exits
- `enable_heatmaps: true` — click + scroll heatmaps
- session recording **on**, all inputs masked (`maskAllInputs: true`)
- `person_profiles: 'always'`

## Super-property (set on every tab change)

`showRoute()` calls `posthog.register({ tab, tab_title })`, so **every event** (autocapture and
custom) carries the **active tab**. Segment any metric by `tab` to see "what happened on each page."

## Custom events

| Event | Props | Fires when |
|---|---|---|
| `$pageview` | `route` | Tab shown (native PostHog pageview) |
| `page_view` | `page` | Tab shown |
| `page_clicked` | `to` | A `data-route` nav/button is clicked |
| `photo_clicked` | `label`, `photo` (filename) | A gallery image opens in the lightbox |
| `es_gallery` | `src` | Hero gallery thumbnail switched |
| `map_marker_click` | `kind` (corridor_market / comparable / site), `name`, `segment`/`category` | A corridor-map pin is clicked |
| `corridor_filter` | `segment`, `on` | A corridor-map legend layer is toggled |
| `outbound_link_click` | `href`, `text` | Any external link is clicked (comparable sites, sources) |
| `timeline_open` | `era`/title | Heritage timeline card opened |
| `building_open` | building | Property building-inventory card/pin opened |
| `plan_hotspot` / `map_pin` | id | Floor-plan / map hotspots |
| `faq_open` | question | A `<details class="faq">` opened |
| `history_toggle`, `pp_section`, `theme_toggled` | — | Misc UI |
| `scroll_depth` | `page`, `pct` (25/50/75/100) | Scroll milestones per tab |
| `page_dwell` | `page`, `seconds` | Time spent before leaving a tab |
| `request_info_submitted` | form fields (masked) | Lead form submitted |

> Note: existing event names are kept as-is to preserve historical continuity. Cross-event
> segmentation is handled by the `tab` super-property rather than renaming props.

## Dashboard / Insight definitions (build in the PostHog UI)

1. **Tab clicks per session** — Insight → Trends → event `page_clicked`, breakdown by `to`,
   measured as **"per session" (count per session)** or **unique sessions**. Add a table view for
   a ranked list. (Or use `$pageview` broken down by `tab`.)
2. **Time per tab** — Trends → `page_dwell`, average of `seconds`, breakdown by `page`.
3. **Photo engagement leaderboard** — Trends → `photo_clicked`, breakdown by `photo` (filename),
   total count. Add a second series broken down by `tab` to see where photos get clicked.
4. **Map engagement** — Trends → `map_marker_click` breakdown by `name`; and `corridor_filter`
   breakdown by `segment`. Shows which markets/comparables draw interest.
5. **Outbound interest** — Trends → `outbound_link_click` breakdown by `text` (which comparable
   hotels / sources visitors click through to).
6. **Lead funnel** — Funnel → `$pageview` (executive-summary) → any `page_clicked` →
   `request_info_submitted`. Conversion to data-room request.
7. **Scroll depth by tab** — Trends → `scroll_depth` breakdown by `page`, filtered to `pct = 100`,
   to see which tabs get read to the end.
8. **Qualified-visitor replays** — Session Replay, filtered to sessions containing
   `request_info_submitted` or `map_marker_click`, to watch high-intent visits.

All of the underlying events above are already being captured; items 1–8 are saved Insights/Replay
filters configured in PostHog, not code changes.
