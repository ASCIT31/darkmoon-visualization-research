# Design system recap for the campaign bar

> Part of the campaign-bar design study (2026-11-20), reproduced as written. « Screens NN » are numbered owner review screenshots (not published). Boards are **mock-ups**: [campaign-bar-boards](../campaign-bar-boards/README.md). The owner finally chose **Board 13**, not the study's first choice.

These are the tokens the boards reuse. They were read from the Pro client source (read-only, internal) and copied into the boards' stylesheet.

## Fonts

| Role | Font | Source |
|---|---|---|
| UI (everything) | **Google Sans Flex** (variable, weights 1–1000) | `styles/fonts/GoogleSansFlex-var-latin.woff2`, declared in `styles/nothing-theme.css` (`--font-sans`) |
| Code (IDs, kbd hints) | **Google Sans Code** 400 | `styles/fonts/GoogleSansCode-400.woff2` (`--font-mono`) |
| ~~Display mono~~ | ~~Space Mono~~ | **Being removed site-wide (owner decision 20/11).** Still referenced in `styles.scss` (`--space-mono`, `.ig-*`) and in `.ux-grade` (`ux-fit.scss`). The boards never use it. |

Weights used in the boards: 450 (counters), 480–520 (labels), 540–560 (titles), 600 (current view, primary button).
Sizes: 13–14 px for controls, 11–12 px for phone cell labels and counters, 17–22 px for the campaign as title (28 px in the object-header variant).

## Colour tokens

| Token (board name) | Dark | Light | App source |
|---|---|---|---|
| page | `#000000` | `#f4f4f4` | `--nt-page` |
| tile / card | `#1a1a1a` | `#ffffff` | `--nt-tile` |
| control / track | `#1a1a1a` | `#ededed` | `--nt-control` |
| ink | `#ffffff` | `#0d0d0d` / `#000` | `--nt-ink`, `--oa-ink` |
| ink-2 (secondary) | `#a1a1a6` | `#5d5d5d` | `--tr-g40` (dark), `--oa-ink-2` (light) |
| ink-3 (tertiary) | `#808085` | `#6e6e73` | `--tr-g70` |
| pill fill | `#000000` | `#ffffff` | `--oa-fill` |
| pill edge | `#3a3a3a` | `rgba(0,0,0,.08–.10)` | `--oa-border` |
| active (current) | `#262626` | `#efefef` | `--oa-active` |
| hairline | `rgba(255,255,255,.15)` | `rgba(0,0,0,.10)` | owner rule « bord 1px .15 partout » + `--oa-border` |
| **primary** | `#F4FA6A` on `#000` | **`#000` on `#fff`** | `--dm-btn-primary` (dark); black in light per the current app and screenshots |
| critical / high / medium / low | `#EF1600` / `#FF9A3C` / `#F4FA6A` / `#50ADFD` | `#C41200` / `#B65A00` / `#8A8F00` / `#1A6FC4` | `--nt-red` … (light variants in `nothing-theme.css`) |
| focus ring | `#fff` | `#000` | `--fx-focus` |

Rule: severity colours never appear on navigation chrome, only on data (the exposure dot, the severity split in board 11).

## Geometry

- **Pills:** 40 px tall, radius 20 (`chatgpt-ui.css` min-height, `openai-tabs.css`). Phone targets are 44 px.
- **Concentric radii** (`ux-fit.scss` §1): container radius = pill radius + padding (+1 px edge). 40 px pill + 4 px padding → radius 24/25. A 36 px segment in a 3 px track → radius 11 inside 14.
- **Card radii:** `--fx-r-xl` 24, `--fx-r-lg` 18, `--fx-r-md` 12, small = 999.
- **Hairlines:** 1 px, drawn as `box-shadow: inset 0 0 0 1px` so they never add layout width. **One edge per component** (lesson of screens 36–39).
- **Gaps:** 4 px between pills inside a track, 8 px between free-standing controls, 12 px between rows.

## Existing components in the same family

These all share the selectors in `openai-tabs.css` / `ux-fit.scss` §1:

| Selector | Where |
|---|---|
| `.cb`, `.cb__seg`, `.cb__pick`, `.cb__primary`, `.ov-more` | campaign bar (campaign, finding, frameworks pages) |
| `.kit-bar`, `.kit-seg`, `.kit-cap` | map toolbar |
| `.pj-tabs` | project Infrastructure / Overview tabs |
| `.dv-fw__tabs` | frameworks page tabs |
| `.dlc-seg`, `.ov-seg`, `.fa-seg`, `[role=tablist]` | in-page sub-views (e.g. « Findings 68 · Treatment plan 14 ») |

**Primary button:** `.cb__primary` / `.dm-btn-primary`: 40 px, padding 0 16–18, weight 600, single line (ux-fit §3).

## Implementation warning (for whoever builds the chosen board)

The bar is currently styled by **five stacked layers**: `openai-tabs.css`, `chatgpt-ui.css`, `no-rings.css`, `coherence.css` and `ux-fit.scss`. They use `!important` with specificity chains of up to 24 `:not(#id)` selectors. Adding another override layer would repeat the cause of the pass-0/pass-1 defects. The replacement should be a **fresh component with its own class names**. The old `.cb*` rules should be deleted from those layers, not out-ranked.
