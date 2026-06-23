# Scope — Interactive Access & Mobility Map (v1)

**Project:** Tumwater Brewery microsite — Location tab
**Module:** "Access & Mobility" interactive map
**Decisions locked:** Embed in existing `index.html` (vanilla JS, no build) · MapLibre GL JS + free vector tiles (no Mapbox account/token) · v1 = Access & Mobility only

---

## 1. Goal

Turn the current collapsible Access & Mobility rows into a **map-linked experience**: an interactive map sits alongside the existing rows, and selecting a category (vehicular, walkability, bike, transit, arrival) highlights the corresponding geography — routes, the site parcel, and points of interest — with popups and photo lightboxes. The result reads as a designed, on-brand module, not a generic embed.

Non-goal for v1: the regional corridor map, Visitor/Planning view modes, and scroll-triggered arrival storytelling (deferred to v2 — see §8).

---

## 2. Approach

- A single `<div id="amMap">` rendered by **MapLibre GL JS** (loaded from CDN — one CSS + one JS file), initialized only when the Location tab is shown and lazy-loaded on first view to protect page weight.
- The existing six `.amx` collapsible rows stay. Each row gets a `data-layer` attribute. Opening/clicking a row → the map flies to that layer's bounds, shows its GeoJSON, and dims the others. A small legend/toggle bar over the map mirrors the categories for direct map-side control.
- All geography comes from **local GeoJSON** committed to the repo (`/data/*.geojson`). No live API calls at runtime beyond tile fetches.
- Map anchored to the site: **110 Deschutes Way SW, Tumwater, WA 98501** (single source-of-truth center constant).
- Brand styling: river-blue routes, forest-green trails, industrial-neutral base; markers reuse the existing accent color and the icon set already in the tab.

---

## 3. v1 Feature List

| # | Feature | Detail |
|---|---------|--------|
| 1 | Base map | MapLibre GL, free vector tiles, custom minimal style (muted, on-brand), site marker pinned at the address |
| 2 | Layer toggles | Bar of 5–6 toggles (Vehicular / On-site walk / Off-site walk / Bike / Transit / Arrival) with the tab's icons |
| 3 | Row ↔ map sync | Expanding a `.amx` row highlights its geography and frames it; clicking a toggle expands the matching row |
| 4 | GeoJSON layers | Routes (lines), site parcel (polygon), POIs (points): I-5/Capitol Blvd approach, river/woodland/Capitol Lake trails, regional bike routes, transit stops + Amtrak, arrival path |
| 5 | Popups | Click a route/POI → small popup with name + one line of context |
| 6 | Photo lightbox | POI/category photos open in the **existing site lightbox** (reuse current component + arrow nav) |
| 7 | Responsive | Map stacks above the rows on mobile; toggles wrap; touch-friendly |
| 8 | Performance | Lazy-init on tab view; assets only load when Location is opened; respects reduced-motion |
| 9 | Fallback | If WebGL/map fails, the existing collapsible rows + static placeholders remain fully usable |

---

## 4. Data Model (committed GeoJSON)

```
/data/
  site.geojson          # parcel polygon + address point (the anchor)
  vehicular.geojson     # I-5, Capitol Blvd, Tumwater Blvd, entry points
  walk-onsite.geojson   # internal paths, riverfront trail, bridges
  walk-offsite.geojson  # Brewery Park, Tumwater Falls, Capitol Lake links + gaps
  bike.geojson          # Deschutes trails, Woodland Trail, Capitol Lake loop
  transit.geojson       # Intercity Transit stops, Olympia hub, Amtrak point
  arrival.geojson       # ordered arrival path (3 waypoints)
  poi.geojson           # labeled anchors (Falls, Brewery Park, Capitol, downtown)
```
Each feature carries `category`, `name`, `blurb`, optional `image`. **Dependency:** someone needs to produce/approve the actual route geometry (see §7).

---

## 5. Integration with existing tab

- Inserts directly under the §3 intro, above the `.amx` rows (or as a two-column layout on wide screens: map left, rows right).
- Reuses: existing icons, `.frame`/lightbox, color tokens, type scale. Adds one scoped CSS block + one JS module — consistent with how `pi-*` / `amx` styles were added.
- No change to other tabs; map code guarded so it only runs on the Location page.

---

## 6. Milestones & rough effort

| Phase | Work | Est. |
|-------|------|------|
| A | MapLibre integration, on-brand style, site marker, lazy-load plumbing | ~0.5 day |
| B | Draft GeoJSON for all layers (approximate, from public references) + layer rendering | ~1 day |
| C | Row ↔ map sync, toggles, popups, lightbox wiring | ~1 day |
| D | Responsive, reduced-motion, fallback, polish, cross-browser check | ~0.5 day |
| **Total** | **v1, embedded** | **~3 days** |

(Effort assumes I draft approximate geodata; precise/surveyed geometry would add review cycles.)

---

## 7. Dependencies & open questions

1. **Geodata source** — OK for me to author *approximate* routes/POIs from public maps for v1, to be refined later? Or do you have authoritative GeoJSON/KML (e.g., from KPFF civil work, City trail data)?
2. **Tile provider** — MapLibre needs a vector-tile source. Free options: a no-key demo style, or a free-tier key from a provider (e.g., Stadia/MapTiler free plan). Confirm whether *any* third-party key is acceptable, or strictly key-less.
3. **Real photos** — POI/arrival lightbox images: use placeholders until you supply photos.
4. **Layout preference** — map-above-rows (stacked) vs. side-by-side map + rows on desktop.

---

## 8. Deferred to v2 (not in this build)

- Regional **corridor map** (Portland↔Seattle hospitality spine)
- **Visitor View / Planning View** mode toggle
- **Scroll-triggered arrival storytelling** (animated 3-image sequence tied to scroll)
- Dynamic filters ("Visitor Experience / Infrastructure / Opportunity")
- Framer Motion / GSAP motion layer (only relevant if we ever move to the React app path)

---

## 9. Risks

- **Page weight:** MapLibre + tiles add ~200–300 KB; mitigated by lazy-loading only on the Location tab.
- **Geodata accuracy:** approximate v1 routes must be clearly labeled "illustrative" until verified.
- **Tile dependency:** free tile sources can change terms; keep the style/source swappable behind one config constant.
