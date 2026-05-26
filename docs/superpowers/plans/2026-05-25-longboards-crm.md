# jmz-longboards CRM Prototype — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use `superpowers:subagent-driven-development` (recommended) or `superpowers:executing-plans` to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a pitch-ready prototype for Longboards Wraps & Bowls — a preview landing page plus two CRM HTML files (Version A focused expense workflow, Version B full ops CRM) using their actual brand colors.

**Architecture:** Three standalone single-file HTML prototypes. No build step. Tailwind Play CDN + Lucide icons + Google Fonts. Hash router (~40 lines vanilla JS) adapted from the Realtor-CRM reference. Version A has 3 active screens + 3 Phase 2 stubs; Version B has 6 fully active screens.

**Tech Stack:** HTML5, Tailwind CSS (Play CDN), Lucide Icons (CDN), Google Fonts (Fraunces + Inter), Vanilla JS

**Reference files (read before building):**
- `Z:\AI Brain\projects\tcg-project\docs\index.html` — preview page pattern
- `Z:\AI Brain\projects\Realtor-CRM\prototypes\index.html` — hash router + sidebar JS to adapt
- `Z:\AI Brain\templates\admin-dashboard\plan.md` — design system reference
- `Z:\AI Brain\skills\ux-ui-design.md` — load for any UX decisions

---

## Brand Tokens

```
Primary pink:   #E1195F   (buttons, active nav, badges)
Dark navy:      #282561   (sidebar background)
Orange accent:  #F57F38   (KPI highlights, Phase 2 badges)
Teal:           #2FBDBC   (status/info badges)
Peach tint:     #FAE3D2   (page background)
Ink:            #1C1C1C   (body text)
Border:         #E5E0D8   (card borders)
Surface:        #FFFFFF   (card backgrounds)

Status:
  Approved:  emerald-600  #16a34a
  Pending:   amber-500    #f59e0b
  Flagged:   rose-600     #e11d48
  Info:      blue-500     #3b82f6
```

## 8 Locations (real data)

```
1. Lawrence, KS        — 2540 Iowa St Suite N
2. Kansas City, MO     — 6269 North Oak Trafficway
3. Overland Park S, KS — 8150 W 135th St
4. Overland Park, KS   — 8775 W 95th St
5. Liberty, MO         — 1173 West Kansas Street
6. Mission, KS         — 5415 Johnson Dr
7. Lee's Summit, MO    — 506M SE MO-291 HWY
8. St. Joseph, MO      — 106 S 7th St
```

## Expense Categories

Food & Beverage · Supplies · Packaging · Uniforms · Utilities · Equipment · Other

## Sample Vendors

Sysco Foods · US Foods · Restaurant Depot · Cintas · AmeriGas · Gordon Food Service

---

## File Structure

```
Z:\AI Brain\projects\jmz-longboards\
├── plan.md
├── CLAUDE.md
├── prototypes\
│   ├── index.html             Task 2
│   ├── crm-focused.html       Tasks 3–7
│   └── crm-full.html          Tasks 8–14
└── docs\
    ├── handoff.md             Task 1
    └── superpowers\
        ├── specs\
        │   └── 2026-05-25-longboards-crm-design.md   Task 1
        └── plans\
            └── 2026-05-25-longboards-crm.md          Task 1 (copy of this file)
```

---

## Shared HTML Shell (reuse in Tasks 3 + 8)

This boilerplate goes at the top of both CRM files. Swap nav items per version.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
  <title>Longboards Operations CRM</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            brand:    '#E1195F',
            'brand-dark': '#282561',
            'brand-orange': '#F57F38',
            'brand-teal': '#2FBDBC',
            'brand-peach': '#FAE3D2',
            'brand-ink': '#1C1C1C',
            line: '#E5E0D8',
          },
          fontFamily: {
            display: ['Fraunces', 'Georgia', 'serif'],
            sans: ['Inter', 'system-ui', 'sans-serif'],
          }
        }
      }
    }
  </script>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Fraunces:wght@700;900&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <script src="https://unpkg.com/lucide@latest"></script>
  <style>
    * { box-sizing: border-box; }
    body { background: #FAE3D2; font-family: 'Inter', sans-serif; }

    /* Sidebar */
    .sidebar {
      width: 256px; min-height: 100dvh; background: #282561;
      display: flex; flex-direction: column;
      transition: width 0.2s ease; flex-shrink: 0;
    }
    .sidebar.is-collapsed { width: 72px; }
    .sidebar.is-collapsed .sb-hide { display: none; }
    .sb-toggle {
      position: absolute; top: 56px; right: -12px;
      width: 24px; height: 24px; border-radius: 50%;
      background: white; border: 1px solid #E5E0D8;
      display: flex; align-items: center; justify-content: center;
      cursor: pointer; z-index: 10;
    }
    .sidebar.is-collapsed .sb-toggle i { transform: rotate(180deg); }

    /* Nav items */
    .nav-item {
      display: flex; align-items: center; gap: 10px;
      padding: 8px 12px; border-radius: 8px; margin: 1px 0;
      color: rgba(255,255,255,0.65); font-size: 13px; font-weight: 500;
      cursor: pointer; text-decoration: none; transition: all 0.15s;
      white-space: nowrap;
    }
    .nav-item:hover { background: rgba(255,255,255,0.08); color: white; }
    .nav-item.active { background: #E1195F; color: white; }
    .nav-item.stub { opacity: 0.6; cursor: pointer; }
    .nav-label {
      font-size: 10px; font-weight: 600; letter-spacing: 0.08em;
      text-transform: uppercase; color: rgba(255,255,255,0.3);
      padding: 12px 12px 4px;
    }
    .sidebar.is-collapsed .nav-label { display: none; }

    /* Views */
    .view { display: none; }
    .view.active { display: block; }

    /* Cards */
    .card { background: white; border-radius: 12px; border: 1px solid #E5E0D8; }

    /* KPI tiles */
    .kpi { background: white; border-radius: 12px; border: 1px solid #E5E0D8; padding: 16px 20px; }
    .kpi-value { font-family: 'Fraunces', serif; font-size: 28px; font-weight: 700; color: #282561; }
    .kpi-label { font-size: 12px; color: #6b7280; margin-top: 2px; }

    /* Status badges */
    .badge { display: inline-flex; align-items: center; gap: 4px; padding: 2px 8px; border-radius: 99px; font-size: 11px; font-weight: 600; }
    .badge-approved { background: #dcfce7; color: #16a34a; }
    .badge-pending   { background: #fef3c7; color: #d97706; }
    .badge-flagged   { background: #fee2e2; color: #dc2626; }
    .badge-info      { background: #dbeafe; color: #2563eb; }
    .badge-phase2    { background: #fff1e6; color: #F57F38; }

    /* Table */
    .data-table { width: 100%; border-collapse: collapse; }
    .data-table th { font-size: 11px; font-weight: 600; text-transform: uppercase;
      letter-spacing: 0.05em; color: #9ca3af; padding: 8px 12px;
      border-bottom: 1px solid #E5E0D8; text-align: left; }
    .data-table td { font-size: 13px; padding: 10px 12px; border-bottom: 1px solid #f3f0ec; color: #374151; }
    .data-table tr:last-child td { border-bottom: none; }
    .data-table tr:hover td { background: #fdf8f5; }

    /* Buttons */
    .btn-primary { background: #E1195F; color: white; padding: 8px 16px; border-radius: 8px;
      font-size: 13px; font-weight: 600; border: none; cursor: pointer; transition: background 0.15s; }
    .btn-primary:hover { background: #c4164f; }
    .btn-ghost { background: transparent; color: #374151; padding: 6px 12px; border-radius: 8px;
      font-size: 12px; font-weight: 500; border: 1px solid #E5E0D8; cursor: pointer; }
    .btn-ghost:hover { background: #f9f5f0; }

    /* Upload zone */
    .upload-zone {
      border: 2px dashed #E5E0D8; border-radius: 12px; padding: 40px;
      text-align: center; cursor: pointer; transition: all 0.2s;
      background: #fffaf7;
    }
    .upload-zone:hover, .upload-zone.active { border-color: #E1195F; background: #fff0f5; }

    /* Mobile overlay */
    #mobileOverlay {
      display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.4); z-index: 40;
    }
    #mobileOverlay.open { display: block; }

    @media (max-width: 1023px) {
      .sidebar { position: fixed; left: -256px; z-index: 50; transition: left 0.25s ease; min-height: 100dvh; }
      .sidebar.mobile-expanded { left: 0; }
      .main-layout { margin-left: 0 !important; }
    }
  </style>
</head>
<body class="flex">
<div id="mobileOverlay"></div>

<!-- SIDEBAR (swap nav items per version) -->
<aside class="sidebar relative" id="sidebar">
  <!-- Logo -->
  <div class="flex items-center gap-3 px-4 py-4 border-b border-white/10">
    <div class="w-9 h-9 rounded-xl bg-brand flex items-center justify-center shrink-0">
      <span class="text-white font-bold text-sm font-display">LB</span>
    </div>
    <div class="sb-hide leading-tight overflow-hidden">
      <div class="text-white font-bold text-sm font-display truncate">Longboards</div>
      <div class="text-white/40 text-[10px] uppercase tracking-wider">Operations CRM</div>
    </div>
  </div>
  <!-- Collapse toggle -->
  <button class="sb-toggle" id="sbToggle">
    <i data-lucide="chevron-left" class="w-3 h-3 text-gray-400"></i>
  </button>
  <!-- Nav — INSERT PER VERSION BELOW -->
  <nav class="flex-1 overflow-y-auto py-3 px-2">
    <!-- nav items here -->
  </nav>
  <!-- User footer -->
  <div class="px-3 py-4 border-t border-white/10">
    <div class="flex items-center gap-2.5">
      <div class="w-8 h-8 rounded-full bg-brand flex items-center justify-center shrink-0">
        <span class="text-white text-xs font-bold">AJ</span>
      </div>
      <div class="sb-hide overflow-hidden">
        <div class="text-white text-xs font-semibold truncate">Amber Johnson</div>
        <div class="text-white/40 text-[10px] truncate">Head of Accounting</div>
      </div>
    </div>
  </div>
</aside>

<!-- MAIN CONTENT -->
<main class="flex-1 min-w-0">
  <div class="p-4 sm:p-6 max-w-[2800px] mx-auto">

    <!-- Top bar -->
    <div class="flex items-center justify-between mb-6">
      <div class="flex items-center gap-3">
        <button class="lg:hidden p-2 rounded-lg bg-white border border-line" id="mobileMenuBtn">
          <i data-lucide="menu" class="w-4 h-4 text-brand-ink"></i>
        </button>
        <h1 class="text-lg font-bold text-brand-ink" id="breadcrumb">Overview</h1>
      </div>
    </div>

    <!-- VIEWS GO HERE -->

  </div>
</main>

<script>
  // ROUTER — adapt viewMap per version
  const viewMap = { /* per version */ };
  const builtViews = new Set([ /* per version */ ]);

  function nav(view) {
    document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
    const el = document.getElementById('view-' + view);
    if (el && builtViews.has(view)) {
      el.classList.add('active');
    } else {
      document.getElementById('view-stub')?.classList.add('active');
      const stubName = document.getElementById('stub-name');
      if (stubName) stubName.textContent = viewMap[view] || view;
    }
    document.getElementById('breadcrumb').textContent = viewMap[view] || view;
    document.querySelectorAll('[data-view]').forEach(el => {
      el.classList.toggle('active', el.dataset.view === view);
    });
    window.scrollTo({ top: 0, behavior: 'smooth' });
    window.location.hash = view;
  }

  window.addEventListener('hashchange', () => {
    const v = location.hash.replace('#','') || 'overview';
    nav(viewMap[v] ? v : 'overview');
  });
  nav(location.hash.replace('#','') || 'overview');

  // SIDEBAR TOGGLE
  (function(){
    const sb = document.getElementById('sidebar');
    const btn = document.getElementById('sbToggle');
    const overlay = document.getElementById('mobileOverlay');
    const mq = window.matchMedia('(max-width: 1023px)');
    if (localStorage.getItem('lb:sb') === '1' && !mq.matches) sb.classList.add('is-collapsed');
    mq.addEventListener('change', e => {
      if (e.matches) sb.classList.remove('is-collapsed');
      else { sb.classList.remove('mobile-expanded'); overlay.classList.remove('open'); document.body.style.overflow=''; }
    });
    btn.addEventListener('click', () => {
      if (mq.matches) {
        const expanded = sb.classList.toggle('mobile-expanded');
        overlay.classList.toggle('open', expanded);
        document.body.style.overflow = expanded ? 'hidden' : '';
      } else {
        const c = sb.classList.toggle('is-collapsed');
        localStorage.setItem('lb:sb', c ? '1' : '0');
      }
    });
    overlay.addEventListener('click', () => {
      sb.classList.remove('mobile-expanded'); overlay.classList.remove('open'); document.body.style.overflow='';
    });
    document.getElementById('mobileMenuBtn')?.addEventListener('click', () => {
      const expanded = sb.classList.toggle('mobile-expanded');
      overlay.classList.toggle('open', expanded);
      document.body.style.overflow = expanded ? 'hidden' : '';
    });
    sb.querySelectorAll('a[data-view]').forEach(el => el.addEventListener('click', () => {
      if (sb.classList.contains('mobile-expanded')) {
        sb.classList.remove('mobile-expanded'); overlay.classList.remove('open'); document.body.style.overflow='';
      }
    }));
  })();

  // Init Lucide icons
  lucide.createIcons();
</script>
</body>
</html>
```

---

## Task 1: Project Scaffold

**Files to create:**
- `Z:\AI Brain\projects\jmz-longboards\` (directory)
- `Z:\AI Brain\projects\jmz-longboards\CLAUDE.md`
- `Z:\AI Brain\projects\jmz-longboards\plan.md`
- `Z:\AI Brain\projects\jmz-longboards\docs\handoff.md`
- `Z:\AI Brain\projects\jmz-longboards\docs\superpowers\specs\2026-05-25-longboards-crm-design.md`
- `Z:\AI Brain\projects\jmz-longboards\docs\superpowers\plans\2026-05-25-longboards-crm.md`
- `Z:\AI Brain\projects\jmz-longboards\prototypes\` (directory)

- [ ] **Create directory structure**

```powershell
New-Item -ItemType Directory -Force "Z:\AI Brain\projects\jmz-longboards\prototypes"
New-Item -ItemType Directory -Force "Z:\AI Brain\projects\jmz-longboards\docs\superpowers\specs"
New-Item -ItemType Directory -Force "Z:\AI Brain\projects\jmz-longboards\docs\superpowers\plans"
```

- [ ] **Write `CLAUDE.md`**

```markdown
# jmz-longboards

CRM prototype for Longboards Wraps & Bowls — pitch deck for Glenn to show expense automation.

## Context
- 8 locations (KC metro), 100+ employees
- Amber Johnson (wife of owner Gilbert) handles all accounting manually
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

## Living docs
Update docs/handoff.md as build progresses.
```

- [ ] **Write `plan.md`**

```markdown
# Longboards CRM — Project Plan

## What we're building
Pitch prototype for Longboards Wraps & Bowls to demonstrate expense automation.
Two CRM prototypes + a preview landing page.

## Key people
- Glenn — pitch target
- Gilbert — owner
- Amber Johnson (Gilbert's wife) — Head of Accounting, primary user persona

## Pain point
All 8 locations' expenses processed manually by Amber. No automation, no categorization tools.
One person handling everything for 100+ employees across 8 locations.

## Deliverables
1. `prototypes/index.html` — preview page linking to both versions
2. `prototypes/crm-focused.html` — Version A: focused on Amber's daily expense workflow
3. `prototypes/crm-full.html` — Version B: full back-office platform

## Locations
Lawrence KS · Kansas City MO · Overland Park S KS · Overland Park KS ·
Liberty MO · Mission KS · Lee's Summit MO · St. Joseph MO

## Status
[ ] index.html
[ ] crm-focused.html (Version A)
[ ] crm-full.html (Version B)
```

- [ ] **Write `docs/handoff.md`** (stub — fill as build progresses)

```markdown
# Longboards CRM — Handoff Notes

## Design decisions
- Brand colors pulled directly from longboardswrapsandbowls.com CSS
- Single-file HTML prototypes, no build step — open directly in browser
- Upload animation: JS types-in field values sequentially to simulate OCR (~2.5s)
- Version A stubs show "Phase 2" orange badge to signal roadmap, not incompleteness

## How to open
Double-click any .html file in prototypes/ or use Live Server in VS Code.

## Deployment (future)
GH Pages — push prototypes/ contents to gh-pages branch.
```

- [ ] **Write design spec** to `docs/superpowers/specs/2026-05-25-longboards-crm-design.md` — copy the full content from `C:\Users\jpman\.claude\plans\i-want-to-start-elegant-mountain.md` (the approved design doc)

- [ ] **Copy this plan file** to `docs/superpowers/plans/2026-05-25-longboards-crm.md`

- [ ] **Verify** — open File Explorer, confirm all directories and files exist
