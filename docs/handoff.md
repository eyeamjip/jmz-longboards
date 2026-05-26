# Longboards CRM — Handoff Notes

## Design decisions
- Brand colors pulled directly from longboardswrapsandbowls.com CSS
- Single-file HTML prototypes, no build step — open directly in browser
- Upload animation: JS types-in field values sequentially to simulate OCR (~2.5s)
- Version A stubs show "Phase 2" orange badge to signal roadmap, not incompleteness

## How to open
Double-click any `.html` file in `prototypes/` or use Live Server in VS Code.
Live demo: https://eyeamjip.github.io/jmz-longboards/

## Build complete — 2026-05-25

| File | Screens | Notes |
|---|---|---|
| `index.html` | 1 | Preview landing page, links to both versions |
| `crm-focused.html` | 3 active + 3 Phase 2 stubs | Overview, Upload (OCR animation), Queue |
| `crm-full.html` | 6 active | Overview, Expenses (split-pane), Locations, Vendors, Reports, Settings |

### Known limitations
- OCR animation is simulated (no real API call) — by design for pitch
- Location detail data is hardcoded (loc-1 through loc-8 only)
- Reports are pre-rendered bar charts, not live queries

---

## Mobile polish — 2026-05-26

### Root cause of layout offset bug
`<body class="flex">` caused Tailwind Play CDN to generate `.flex { display: flex }` at class specificity (0-1-0), which beat `body { display: block }` (element selector, 0-0-1) in the mobile media query. Fix: removed `flex` from the HTML class, added `body { display: flex }` directly in the `<style>` block so both rules share element-level specificity and the media query wins on mobile.

### Table scroll fix
`.card.overflow-hidden` clipped `.table-scroll` wrappers, preventing horizontal scroll. Fixed by removing `overflow-hidden` from all table-containing card divs. Tables wrapped in `<div class="table-scroll">` with `overflow-x: auto`.

### Mobile navigation — bottom nav bar (replaces sidebar drawer)
- Sidebar hidden entirely on mobile (`display: none`) — eliminates blank-space-at-top
- Fixed bottom nav: icon-only, 26px Lucide icons, 64px tall, #282561 navy background
- `flex: 1 0 80px` — items fill bar evenly with few icons (Version A: 3), scroll horizontally with many (Version B: 6)
- Active icon highlights pink (#E1195F)
- iOS safe-area inset applied to bottom nav padding
- Version A bottom nav: Overview · Upload · Queue · Locations · Vendors · Reports (Phase 2 stubs scroll into view)
- Version B bottom nav: Overview · Expenses · Locations · Vendors · Reports · Settings (all active, swipe to reach last 2)

### Deployment
Files served from root of `gh-pages` branch (not `prototypes/` subdirectory).
