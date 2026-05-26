# jmz-longboards

CRM prototype for Longboards Wraps & Bowls — pitch deck for Glenn to show expense automation.

## Context
- 8 locations (KC metro), 100+ employees
- Amber Macapagal (wife of owner Gilbert) handles all accounting manually
- Pitch goal: show Glenn that automation can eliminate manual receipt processing

## Files
- `prototypes/index.html` — preview landing page
- `prototypes/crm-focused.html` — Version A: Quick Win (3 active screens + 3 Phase 2 stubs)
- `prototypes/crm-full.html` — Version B: Full Ops CRM (6 active screens)

## Design
- Brand: #E1195F pink, #282561 navy, #F57F38 orange, #2FBDBC teal, #FAE3D2 peach
- Font: Fraunces (display) + Inter (body)
- Desktop max-width: max-w-[2800px] mx-auto
- Reference: Z:\AI Brain\projects\Realtor-CRM\prototypes\index.html

## Mobile layout pattern (locked 2026-05-26)
- Sidebar hidden on mobile (≤1023px) — `display: none`
- Fixed bottom nav bar: icon-only, 26px icons, 64px tall, navy (#282561) background
- `flex: 1 0 80px` on items — fills bar with few icons, scrolls horizontally with many
- Active icon turns pink (#E1195F)
- `body { display: flex }` in CSS (NOT as a Tailwind class) — avoids specificity bug where Tailwind `.flex` overrides `body { display: block }` in mobile media query
- Tables wrapped in `.table-scroll { overflow-x: auto }` + `overflow-hidden` removed from card parents
- iOS safe-area: `padding-bottom: env(safe-area-inset-bottom, 0px)` on bottom nav

## Live demo
https://eyeamjip.github.io/jmz-longboards/

## Living docs
Update docs/handoff.md as build progresses.
