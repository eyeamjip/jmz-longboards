# Longboards Wraps & Bowls — Operations CRM Prototype

Pitch prototype built by **JMZ Technology Solutions** for Longboards Wraps & Bowls, demonstrating automated expense processing across all 8 KC-metro locations.

**Live preview:** [https://eyeamjip.github.io/jmz-longboards/](https://eyeamjip.github.io/jmz-longboards/)

---

## What's in this repo

| File | Description |
|---|---|
| `prototypes/index.html` | Pitch preview landing page — links to both versions |
| `prototypes/crm-focused.html` | **Version A — Quick Win**: Amber's daily expense workflow (3 active screens + 3 Phase 2 stubs) |
| `prototypes/crm-full.html` | **Version B — Full Ops CRM**: Complete back-office platform (6 fully active screens) |

---

## Version A — Quick Win (`crm-focused.html`)

Focused on the immediate pain point: Amber currently processes all expense receipts manually for 8 locations. This version shows what day-to-day automation looks like for her.

| Screen | What it shows |
|---|---|
| Overview | KPI strip (queue size, processed today, time saved) + recent activity feed |
| Expense Upload | Click to upload a receipt → OCR simulation fills vendor, amount, category, location, date |
| Processing Queue | Full queue with location/category/status filters + inline Approve / Flag actions |
| Locations, Vendors, Reports | Phase 2 stubs with roadmap badge |

## Version B — Full Ops CRM (`crm-full.html`)

The complete platform vision — everything Version A has, plus multi-location dashboards, vendor directory, reports, and settings.

| Screen | What it shows |
|---|---|
| Overview | KPIs + 8-location spend bars (Kansas City near-budget warning) |
| Expense Processing | Split pane: upload on left, live queue updates on right after OCR completes |
| Locations | 8 location cards with MTD spend + click-to-expand receipt history |
| Vendors | Vendor directory with live search filter |
| Reports | Month / Quarter / Year spend charts by category and location |
| Settings | Category management, user profile, QuickBooks / Toast POS / Google Drive integrations |

---

## Locations

| Location | Address |
|---|---|
| Lawrence, KS | 2540 Iowa St Suite N |
| Kansas City, MO | 6269 North Oak Trafficway |
| Overland Park S, KS | 8150 W 135th St |
| Overland Park, KS | 8775 W 95th St |
| Liberty, MO | 1173 West Kansas Street |
| Mission, KS | 5415 Johnson Dr |
| Lee's Summit, MO | 506M SE MO-291 HWY |
| St. Joseph, MO | 106 S 7th St |

---

## Tech

- **HTML5** — single-file prototypes, no build step
- **Tailwind CSS** — Play CDN with custom brand tokens
- **Lucide Icons** — CDN
- **Google Fonts** — Fraunces (display) + Inter (body)
- **Vanilla JS** — hash router, sidebar toggle, OCR animation, inline filters

### Brand colors

| Token | Hex | Use |
|---|---|---|
| Primary pink | `#E1195F` | Buttons, active nav, badges |
| Dark navy | `#282561` | Sidebar background |
| Orange | `#F57F38` | KPI highlights, Phase 2 badges |
| Teal | `#2FBDBC` | Status/info badges |
| Peach | `#FAE3D2` | Page background |

---

## Mobile

Both prototypes are fully responsive. On phones (≤1023px):
- Sidebar is replaced by a fixed bottom navigation bar
- Icons only (26px), horizontally scrollable — swipe left/right to reach all screens
- Active screen highlighted in pink

## Open locally

Double-click any `.html` file in `prototypes/` — no server needed.

---

*Built by JMZ Technology Solutions · Confidential pitch prototype · 2026*
