# Design System — "Crisp"

Brand & UI system reference for every screen/component design under `designs/`. Read this fully before designing anything — it is the single source of truth every screen must stay consistent with.

> **Starter design system.** This file was seeded by the Sdlc.Planner package and is yours to edit — it is never overwritten. Adapt the product description, brand colours, logo and navigation below to this project; the **designer** agent keeps it current. Replace the bracketed placeholders first.

[Product name] is [one-line description of the product and its users] (see [architecture.md](../architecture.md)). The system is tuned for **density, contrast and speed** during long working sessions, not promotional impact.

## Product Usage Context

- Users are professional operators in long continuous sessions (up to 8 h/day), frequently on multi-monitor desks. *(Adjust to your real users.)*
- UX target: high-throughput, low-friction workflows — fewer clicks, keyboard-first, predictable placements.
- Visual target: **crisp, high-contrast, modern** (Linear / Vercel / Datadog calibre) — neutral surfaces, saturated accents that pop, no pastel washes.
- Platform: desktop only, 1920×1080 primary target. *(Adjust if your product targets other platforms.)*

## Brand Foundation

The palette below is the starter identity, tuned for on-screen legibility in dense UIs. Replace the `brand` values with your organisation's brand colour if you have one, keeping the contrast rules (see Accessibility Standards) intact.

### Logo

*(Define the product mark here.)* The in-app lockup is the mark + the product wordmark set as HTML text (Inter 700, -0.02em, `fg`/white), anchored in the sidebar header (top-left); never repeated per module. Minimum size 16px. Keep logo files under `logos/` (mark only for the sidebar/favicon/collapsed rail; mark + wordmark lockup; optional loading, error and offline variants) and list them in a table here.

### Colour — brand & semantic

Solid fills are **constant across themes**. Each role also has a `*-fg` text/icon tint that swaps per theme to keep WCAG AA on that theme's surfaces.

| Token | Fill (both themes) | `*-fg` light | `*-fg` dark | Use |
|---|---|---|---|---|
| `brand` | `#00C46A` (hover `#1FD880`, text-on-fill `on-brand` `#03150C`) | `#007F3F` | `#2EE58C` | Primary actions, active nav, focus ring, selection, positive delta |
| `success` | `#00C46A` | `#007F3F` | `#2EE58C` | Healthy / active / created |
| `danger` | `#F2365F` | `#D0103F` | `#FF5C7F` | Errors, failed, destructive, negative delta |
| `warning` | `#F5A524` | `#9A5B00` | `#FFB636` | At-risk, time pressure, reconnecting |
| `info` | `#3B82F6` | `#1D5FD6` | `#6AA8FF` | In-progress, informational |
| `accent` | `#06B6D4` | `#0B7285` | `#38D6EC` | Chart series 1, mapping/link cues |
| `violet` | `#8B5CF6` | `#6D3FE0` | `#B197FC` | Chart series 2, secondary category |

- `brand` is a bright green (`#00C46A`) that stays legible on dark surfaces. Primary buttons are **green with near-black text** (`on-brand`), not white text — white on this green fails contrast.
- Success and brand share one green on purpose: green = good/positive. Use a pill or a dot for status, a solid button for action — shape disambiguates.

### Colour — neutrals (theme-swapped)

Pure, un-tinted neutrals (no blue cast). Every value below is exposed as a Tailwind colour utility (`bg-canvas`, `text-fg-muted`, `border-line`, …) whose value swaps with the theme — **no `dark:` pairs are needed for neutral or `*-fg` colours**.

| Token | Light | Dark | Role |
|---|---|---|---|
| `canvas` | `#EDEEF1` | `#08080A` | App background behind panels |
| `surface` | `#FFFFFF` | `#111114` | Panels, grids, drawers, page head, top bar |
| `raised` | `#F7F8FA` | `#17171B` | Grid header, drawer footer, buttons, floating menus |
| `sunken` | `#FFFFFF` | `#0B0B0D` | Input fill |
| `hover` | `#F1F2F5` | `#1C1C21` | Row/item hover |
| `line` | `#E2E4E9` | `#232329` | Default 1px divider / panel border |
| `line-strong` | `#CDD0D7` | `#34343C` | Control borders, floating-element borders |
| `fg` | `#0B0C0E` | `#F4F4F6` | Primary text |
| `fg-muted` | `#474B55` | `#A9A9B3` | Secondary text, labels |
| `fg-subtle` | `#6B7080` | `#80808A` | Meta, placeholders, column headers (all ≥ 4.5:1 on `surface`) |
| `nav` | `#0D0E11` | `#0C0C0F` | Sidebar — **dark in both themes** (the light theme's strongest anchor) |

### Typography

- **Inter** (UI) with `cv11`, `ss01`, `ss03` features; **JetBrains Mono** for IDs, times, counts, keycaps and numeric columns. Both via Google Fonts in mockups; the production UI kit must self-host them.
- Inter's tall x-height and tabular figures read well at 12px in dense grids.
- All numbers in data contexts use `tabular-nums` (the `.dg` grid applies it globally).

| Use | Size / weight | Class |
|---|---|---|
| Page title | 15px / 600, -0.01em | `.page-title` |
| Drawer / dialog title | 16px / 600 | — |
| Section / panel title | 12–13px / 600 | `.panel-title` |
| Body / chrome | 13px / 400–500 | body default |
| Grid cell | 12px / 400 (primary column 500) | `.dg td` |
| Grid header | 10.5px / 600, uppercase, +0.05em, `fg-subtle` | `.dg th` |
| Label | 11px / 500, `fg-muted` | `.label` |
| Eyebrow | 10.5px / 600, uppercase, +0.07em | `.eyebrow` |
| Mono data | 11.5px JetBrains Mono | `.mono` |
| KPI figure | 22px / 600, -0.02em, tabular | `.kpi-value` |

## Design Principles

1. **Crisp over soft** — neutral surfaces, 1px lines that always read, saturated semantic colour. No pastel fills for primary surfaces, no blue-tinted greys, no decorative gradients.
2. **Every pixel works** — full-width layouts, 28px grid rows, 44px page head, 40px top bar. Space is for data; whitespace is only used to separate structure.
3. **Colour is signal** — neutrals carry structure; brand/semantic colour appears only where it encodes action, state or series. Because it is rare, it pops.
4. **Keyboard-first** — every list supports ↑/↓, Enter, Esc; drawers support J/K; global search is Ctrl+K; nav collapses with `[`. Show shortcuts inline with `.kbd`.
5. **Consistent patterns** — every screen is assembled from `_shared/theme.js` component classes and the `shell.js` chrome; no per-screen restyling.
6. **State-complete** — default, loading (skeleton), empty, error, and live/reconnecting states designed for every region.
7. **Dark-first, light-equal** — dark is the default theme for long working sessions; light is a first-class, equally reviewed variant.
8. **Accessible** — AA contrast in both themes, visible 2px green focus ring, text labels alongside every colour cue.

## Design Tokens & Runtime

### Single source of truth: `_shared/theme.js`

> `theme.js` and `shell.js` are not shipped with the package. The **designer** agent creates them under `docs/designs/_shared/` from the tokens and component classes in this file before the first mockup, and keeps them in lockstep with this document.

Every mockup's `<head>` contains only:

```html
<script src="../_shared/theme.js"></script>
```

That script injects fonts, Font Awesome 6.6, the Tailwind v4 browser build, and one `<style type="text/tailwindcss">` block holding `@custom-variant dark`, the `@theme inline` tokens above (neutral and `*-fg` tokens map to `--app-*` CSS variables defined for `:root` and `.dark`), and the component classes below. **Mockups never declare their own `@theme`** — per-file copies drift. The production UI kit's `theme.css` should mirror this block 1:1 (see Assumptions).

Theme: dark by default; `?theme=light` in the URL or the top-bar toggle (`AppTheme.toggle()`, persisted in `localStorage`) switches it.

### Component classes (defined in `theme.js`)

| Class | Spec |
|---|---|
| `.page-head` / `.page-title` | 44px `surface` strip under the top bar: title + scope selects + quick filters + live state + page actions, all on one line |
| `.panel` / `.panel-head` / `.panel-title` | `surface`, 1px `line`, 4px radius; 36px header |
| `.btn` (+ `.btn-primary`, `.btn-danger`, `.btn-ghost`, `.btn-link`, `.btn-icon`, `.btn-lg`) | 28px (32px `lg`), 4px radius, 12px/500. Primary = `brand` fill + `on-brand` text |
| `.field` (+ `.field-lg`, `.field-select`, `select.field`) | 28px (32px `lg`) input, `line-strong` border, `sunken` fill, focus = `brand` border + 2px `brand/25` ring; `[readonly]` → `raised`; `[aria-invalid=true]` → `danger` border + ring |
| `.label`, `.help`, `.error-text` | Label row is flex so a right-aligned hint (e.g. feed value) fits on the same line |
| `.pill` + `.pill-{neutral,success,danger,warning,info,accent,violet}` | 20px, 11px/600, 12–14% tint + saturated `*-fg` text + 40% inset ring |
| `.pill-solid-{danger,warning,success,info}` | Solid fill for states that demand action (Failed, Critical, Blocked) |
| `.dot` | 6px `currentColor` status dot |
| `.chip` / `.chip-x` | Applied-filter chip (brand tint) with remove button |
| `.seg` | Segmented control / quick-filter group; `aria-pressed="true"` = selected |
| `.tab` | 36px tab; selected has a 2px `brand` underline |
| `.kbd` | Keycap hint |
| `.count` | Mono 10.5px count inside seg/tab/nav |
| `.dg` | Data-grid `<table>`: sticky 28px header (`raised`), optional sticky `tr.filter` row, 28px body rows, hover `hover`, `[aria-selected]` = `brand/10` + 2px inset `brand` bar, `[aria-disabled]` = `fg-subtle` text, `.num` right-aligned mono. `tr.leg` = 24px full-width detail row under an expanded parent (e.g. child rows of a parent record), with `td.leg-first` drawing the tree connector |
| `.masked` | Personal data hidden by permission: mono, `fg-muted`, small lock glyph, `help` cursor + tooltip. Never used for "no value" (that stays `—`). See [masked-value](_shared/masked-value.md) |
| `.icon-chip` | 24px solid-fill square with white glyph; fill must be a semantic/brand role |
| `.switch` | 28×16 toggle, `role="switch"` + `aria-checked` |
| `.kpi-value` | KPI figure |
| `.skeleton` | Pulsing placeholder block |
| `.live` | Green dot with ping — live/streaming state |
| `.floating` | Menus, popovers, alert cards, dialogs: `raised` + `line-strong` border + theme shadow |
| `.eyebrow`, `.mono`, `.vr` | Utility text styles and a 16px vertical divider for toolbars |

### Radius & elevation

- **4px** (`rounded-sm`) for everything rectangular: buttons, inputs, panels, pills, chips; 2px (`rounded-xs`) for keycaps and seg items; `rounded-full` only for avatars, dots and switches.
- Flat by default. Only `.floating` elements get a shadow (`--app-shadow`, much stronger in dark mode with a 1px white/4% outline).
- Panels separate by a 1px `line` border on the `canvas` background; nested elements inside a panel use `line` at 70% or no border at all.
- **Borders always name a colour** (`border-line`, `border-line-strong`, or a semantic role) — Tailwind v4 defaults bare `border` to `currentColor`.

### Spacing & density

- 4px base scale. Page padding **8px** (`p-2`) where panels sit on `canvas`; **0** where a grid fills the region edge-to-edge. Gutters between panels **8px**.
- Control heights: 24px (inline filter cells), **28px** toolbars, **32px** forms/drawers.
- Grid rows **28px**. A 1920×944 viewport shows ~25 rows alongside a docked 560px drawer.
- Section spacing inside drawers/forms: 12px vertical, 12px column gap.

### Iconography

Font Awesome 6 (solid for UI, regular for top-bar actions). In React, `react-icons/fa6` is the equivalent. Icons encode state or trigger actions; never decorative. **Never put `hidden`/`dark:hidden`/`flex` utilities directly on an `<i class="fa-…">`** — FA's unlayered `display` rule overrides Tailwind's layered utilities; wrap the icon in a `<span>` instead.

## Layout System

```
┌ nav 224px (dark) ┬ top bar 40px: breadcrumb · search (Ctrl K) · env · UTC clock · bell · theme · user ┐
│ logo 40px        ├ page head 44px: title · scope · quick filters · live · actions                    ┤
│ modules          │ content — full width, no max-width cap                                           │
│  └ submenus      │   grid screens: edge-to-edge grid [+ 560px docked drawer]                        │
│ collapse [       │   dashboards/forms: panels on canvas, p-2, gap-2                                  │
└──────────────────┴──────────────────────────────────────────────────────────────────────────────────┘
```

- **Sidebar** (`shell.js`): one column, 224px, dark `nav` in both themes. Header row = logo + wordmark. Each module is a 32px row (icon + label + chevron); the active module expands inline to show its submenus (28px rows, indented under a 1px guide line) with optional mono counts and a red dot for alerts. Active submenu = `white/9%` fill + 2px inset green bar + white text. Collapse control at the bottom (shortcut `[`) → 48px icon rail.
- A module with zero visible submenus is hidden — nothing is disabled or greyed out.
- **Top bar** (40px, `surface`): breadcrumb (current page bold) · global-search trigger (320px, opens `_shared/global-search.md`) · name-only environment pill · browser UTC clock (mono, optional) · notifications bell with count (opens `_shared/alert-center.md`) · theme toggle · user menu.
- **Page head** (44px): the page's own title and every page-scope control on one line. Never a two-line hero title + subtitle.
- **Content**: always full width. Grid-first screens put the grid edge-to-edge (no panel frame, no padding) and dock detail/edit drawers on the right (non-modal, 480–560px). Dashboard/form screens place `.panel`s on `canvas` with 8px padding and gaps.
- **Desktop only**; the layout is designed at 1920 and must remain usable (horizontal scroll inside grids, not the page) down to 1440.

## Mockup Authoring Conventions

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<title>Product — Module · Screen</title>
<script src="../_shared/theme.js"></script>
</head>
<body data-module="orders" data-route="list" data-crumbs="Orders|All orders">
<div id="page" class="flex-1 min-h-0 flex flex-col">
  <div class="page-head">…</div>
  …screen…
</div>
<script src="../_shared/shell.js"></script>
</body>
</html>
```

- `shell.js` renders the sidebar + top bar and moves `#page` into the content area. `data-module`/`data-route` pick the active nav item (the module and route ids are defined in the `MODULES` array in `shell.js`, one entry per module in `docs/reqs/`). Optional `data-nav="collapsed"`, `data-bell="0"`.
- Script paths above assume the mockup sits in `docs/designs/<domain>/`. Mockups inside a proposal (`docs/proposals/<slug>/designs/<domain>/`) point at the real files instead: `../../../../designs/_shared/theme.js` and `shell.js`.
- Component specimens that are not full screens (e.g. `status-pill`, `field-primitives`) omit `shell.js` and render on `bg-canvas` with a small header containing a theme toggle button calling `AppTheme.toggle()`.
- Use component classes first; Tailwind utilities with token colours second (`bg-surface`, `text-fg-subtle`, `border-line`, `bg-danger/12`, `text-warning-fg`). **No hex values and no `dark:` variants for colours** in screen markup — the tokens already swap. `dark:` is only for show/hide of theme-specific content.
- Repetitive rows may be generated from a small inline JS data array (as in a domain's list screen) — keep the row template readable.
- Tailwind v4 rules still apply: arbitrary values separate with `_` (`grid-cols-[2fr_1fr_120px]`); `shadow-xs`/`rounded-xs` naming; no `tailwind.config`.
- Sample data must be realistic for the domain (real names, plausible IDs, one fixed mockup date and time per project).

## Screenshot & Viewport Conventions

- Layout target **1920×944 CSS px** (1920×1080 screen minus 88px browser chrome and 48px taskbar).
- Capture both `<slug>-dark.png` (default) and `<slug>.png` (`?theme=light`). Embed dark first in the `.md`.
- The integrated browser is a real window smaller than 1920px, so capture with: CDP `Emulation.setDeviceMetricsOverride({width:1920,height:944,deviceScaleFactor:1})`, load the page, set `document.documentElement.style.zoom` so the 1920px layout fits the real window, and clip the screenshot to the drawn area. The PNG is the full 1920×944 layout, uniformly scaled.

## Core Component Inventory

All under `designs/_shared/`, each with `.md` + `.html` + light/dark PNGs, and a live section in the `design-guide.html` component catalog (created by the designer agent when the first shared component is built).

| Component | Purpose |
|---|---|
| `theme.js` | Tokens + component classes runtime (not a visual component) |
| `shell.js` / `app-shell` | Sidebar + top bar chrome, expanded and collapsed |
| `data-grid` (`.dg`) | Dense table: sticky header, filter row, 28px rows, selection, disabled rows, numeric columns; spec lives in `theme.js` |
| `filter-bar` | Page-scope filters, applied-filter chips, quick-filter `seg` with counts, saved views |
| `window-quick-picker` | Config-driven `seg` of rolling time-window presets (e.g. 7d / 30d / 90d / All) with URL persistence |
| `field-primitives` | Input/select/textarea/switch geometry and full state matrix |
| `card` | Dashboard widget / summary panel (`.panel`) with header actions and KPI variant |
| `status-pill` | Soft + solid semantic pills, dots, live indicator |
| `masked-value` | Permission-masked personal data (`.masked`), distinct from "no value" |
| `pagination` | 36px grid footer: range, rows-per-page, first/prev/page/next/last |
| `alert-center` | App-shell-owned bell popover, popup stack, history |
| `global-search` | Ctrl+K cross-module search overlay grouped by module |
| `modal` / `drawer` | Docked non-modal drawer and centered modal (`.floating`); full dialog, confirm, pending and toast reference lives in `_shared/dialogs.md` |
| `icon-chip`, `tabs`, `seg`, `kbd`, `switch`, `toast` | Small primitives; dialog and toast usage reference in `_shared/dialogs.md` |

This is the starter inventory: build each component under `_shared/` as the first screen needs it, and add or drop rows to match the project.

## Interaction & State Conventions

- **Loading**: `.skeleton` rows/blocks shaped like the content; counts show "—".
- **Empty**: one line of copy + the primary action, centered in the region.
- **Error**: region-scoped `danger` banner (1px `danger/40` border, `danger/10` fill, 3px inset `danger` bar) + Retry. Never a full-page failure for partial data.
- **Live data**: `.live` dot + "Live" + last-update time in the page head; reconnecting = `warning` pill "Reconnecting… polling 10 s".
- **Selection**: one selected row, 2px inset green bar + brand tint; Enter opens the docked drawer, Esc closes, J/K step.
- **Grids**: header click sorts (multi-sort index shown), filter row under the header, filter chips above, pagination footer.
- **App-shell alerts**: see `_shared/alert-center.md` — non-actionable auto-dismiss; actionable persist; merge by `alertId`; remote-resolved remove immediately.
- **Forms**: validate on blur; invalid field = `danger` border + ring + `.error-text`; sticky footer shows a status summary on the left and actions on the right; primary shows its shortcut (Ctrl+Enter).

## Accessibility Standards

- All `*-fg` tokens and `fg-subtle` meet 4.5:1 on `surface` in their theme; `on-brand` on `brand` ≈ 9:1.
- 2px `brand` focus outline (`:focus-visible`) on every interactive element; fields use border + ring instead.
- Grids: `role="grid"`, `aria-rowcount`, `aria-sort`, `aria-selected`, full arrow-key navigation.
- Colour is never the only cue: pills carry text, dots carry `aria-label`/`title`, icon buttons carry `aria-label`.

## External Quality References

- Linear, Vercel — neutral-first crisp chrome, keyboard-first flows.
- Datadog — module navigation + dense filter/search + content triad: <https://docs.datadoghq.com/dashboards/>
- Microsoft Fluent 2 (interaction consistency): <https://fluent2.microsoft.design/>
- Apple HIG (clarity, hierarchy): <https://developer.apple.com/design/human-interface-guidelines/>
- Data-ink ratio (Tufte): <https://infovis-wiki.net/wiki/Data-Ink_Ratio>

These are quality benchmarks, not identities to copy.

## Assumptions & Open Questions

- **Brand colours**: the starter palette is not an official brand palette. If the organisation has one, record the source and any deliberate deviations here (e.g. a brighter fill for dark-surface contrast) and get the brand owner to countersign.
- **Production UI kit**: it must mirror `theme.js` (tokens, `--app-*` variables, component classes → components in the frontend framework chosen in `architecture.md`), with Inter/JetBrains Mono self-hosted. That is an implementation story for the product-owner, not a design task.
- **Sidebar is dark in light theme** — a deliberate anchor for the light theme; revisit only if users report glare.
- **Nav counts and alert dots** next to submenus need a lightweight count from each module. If the backend doesn't supply one, flag it as a requirements/architecture gap; the shell must render nothing when a module doesn't supply a count.
- **Nav placement**: left sidebar, single column; formalize in architecture.md.
- **Desktop only**; no responsive breakpoints.
