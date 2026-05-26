# Longboards CRM — Handoff Notes

## Design decisions
- Brand colors pulled directly from longboardswrapsandbowls.com CSS
- Single-file HTML prototypes, no build step — open directly in browser
- Upload animation: JS types-in field values sequentially to simulate OCR (~2.5s)
- Version A stubs show "Phase 2" orange badge to signal roadmap, not incompleteness

## How to open
Double-click any .html file in prototypes/ or use Live Server in VS Code.

## Build complete — 2026-05-25

All 3 files built and QA'd (137 checks passed):

| File | Screens | Notes |
|---|---|---|
| `index.html` | 1 | Preview landing page, links to both versions |
| `crm-focused.html` | 3 active + 3 Phase 2 stubs | Overview, Upload (OCR animation), Queue |
| `crm-full.html` | 6 active | Overview, Expenses (split-pane), Locations, Vendors, Reports, Settings |

### Deviations from spec
- None. All screens match spec exactly.

### Known limitations
- OCR animation is simulated (no real API call) — by design for pitch
- Location detail data is hardcoded (loc-1 through loc-8 only)
- Reports are pre-rendered bar charts, not live queries

## Deployment (future)
GH Pages — push prototypes/ contents to gh-pages branch.
