# Recommendation: top 3 of the 20 boards

> Part of the campaign-bar design study (2026-11-20), reproduced as written. « Screens NN » are numbered owner review screenshots (not published). Boards are **mock-ups**: [campaign-bar-boards](../campaign-bar-boards/README.md). The owner finally chose **Board 13**, not the study's first choice.

**How the choice was made:**
- `ui-ux-pro-max` (it decides) supplied the hard rules: CRITICAL accessibility and touch first (44 px targets, 8 px spacing), then « no horizontal scroll » (High).
- `design-review` supplied the craft bar: focal point, one depth strategy, no structural hacks.
- Apple HIG and Material guidance came through the research (`VEILLE.md`).
- Every board was rendered (`boards/png/`) and inspected at 1440 / 834 / 390 / 320, in light and dark.

Owner complaints used as the test:
1. **(V) Void:** empty space inside the box (screen 45).
2. **(B) Borders:** edges drawn over each other, button pushed down (36–39).
3. **(W) Wrapping:** random row order.
4. **(M) Mobile:** « you can't see everything, not ergonomic » (54).

## Scoring summary

✔ = solved, ~ = partly, ✗ = not solved.

| Board | V | B | W | M (all views visible at 320, no sideways scroll) | HIG / a11y risk | Effort |
|---|---|---|---|---|---|---|
| **02 Grey track segmented + split primary** | ✔ fills row | ✔ fills only | ✔ | ✔ 5–6 cells, 54 px tall | 6 segments = HIG upper limit | M |
| **01 Breadcrumb title + underline tabs** | ✔ | ✔ one hairline | ✔ | ~ scrolls with fade | none | S–M |
| **07 Phone view switcher + sheet** | ~ | ✔ | ✔ | ✔ all 8 in the sheet, 1 tap away | needs a sheet component | M |
| 13 Fixed tabs in a card | ✔ | ✔ | ✔ | ✔ | heavy on desktop | S–M |
| 14 Screen 48 hugging capsule | ✔ hugs | ✔ | ✔ | ~ scrolls | pill-in-pill | S |
| 03 Priority+ | ✔ | ✔ | ✔ | ~ only 2–3 views visible | priority order must be signed off | M |
| 06 / 15 Bottom bar / dock | ✔ | ✔ | ✔ | ✔ | second nav system next to the hamburger | M |
| 10 / 16 Location bar / crumb menus | ✔ | ✔ | ✔ | ✔ | views 2 clicks deep | M–L |
| 17 Sidebar tree | ✔ | ✔ | ✔ | ✔ via drawer | app-shell change | L |
| 04, 05, 08, 09, 11, 12, 18, 19, 20 | see boards | | | ~ or ✗ | | |

## 1st: Board 02, grey track segmented control + split primary

**Why it wins:**
- It is the only concept that answers all four complaints with one markup and no sideways scrolling at 320 px.
- On desktop the segments **fill the row** (no void) and are separated by **fill only** (no borders to collide).
- On phones the same control becomes **five or six 54 px icon-over-label cells**, so every view is visible and in thumb reach.
- It keeps the grey-track look the owner approved on screen 48, made calmer (Apple segmented control, 3 px track, raised current segment).
- It meets HIG guidance (equal widths, noun labels, ≤ 7 segments wide, ~5 on iPhone, the « More » cell holding the rest) and the ui-ux-pro-max rules (44 px+, 8 px spacing, no horizontal scroll).

**Must-fix items before it ships** (from the `design-review` pass):

- **Should-fix: the split button.** The ⋮ menu holds Infrastructure Map, View Report, Simulate treatment plan action #1 and Copy link. Those are not *variants* of « Review findings », and a split button promises variants. Keep the split only if the owner accepts that meaning. Otherwise keep `Review findings` + a separate round ⋮ (the board shows both ideas: a split on desktop, the same tokens otherwise).
- **Should-fix: two segmented controls stacked.** The in-page sub-view control (« Findings 68 · Treatment plan 14 ») sits right under the bar. If both are grey tracks of the same size, nothing leads. Demote the sub-views to the **secondary tier**: 32 px, text-weight, transparent track (or underline). The campaign views stay the only grey track on the page.
- **Should-fix: phone counters.** The « 68 » badge sits on the icon and almost touches the « Findings » label at 320 px. Give it 2 px more offset. Keep labels at ≥ 11 px (12 px preferred).
- **Note:** « +17 possible » is hidden on phones. It stays in the screen-reader name and on the Attack paths page.
- **Note:** « Paths » is the phone label for « Attack paths ». Use the short label everywhere on phones only.

**Effort:** about 2 days for the bar. Then 1–2 days to retire the old `.cb*` layers (see `DESIGN-SYSTEM.md`, implementation warning) and apply the family rules below.

## 2nd: Board 01, breadcrumb title + underline tabs

**Why:**
- The best desktop **hierarchy** of all 20: the campaign is the page title, the views sit on one hairline, the action is right-aligned on the title row.
- It is the GitHub / Primer pattern security users already know.
- It has the lowest « borders » risk: exactly one line in the whole component.

**Why it is not first:** on phones the tabs scroll sideways. That is the exact behaviour the owner rejected on screen 54, and Primer itself says UnderlineNav should not scroll at 320 px.

**Fix if chosen:** below 560 px, replace the scrolling row with board 07's view button + sheet, or with board 02's full-width cells.

## 3rd: Board 07, phone view switcher + bottom sheet

**Why:**
- The most **ergonomic phone answer**: the current view is named in a 44 px button, and all 8 views (with counters and a check mark) are in a sheet of 52 px rows.
- No sideways scroll and no second navigation system.
- The desktop part is a quiet two-row header with ghost pills.

**Why third:** on phones, changing view takes one extra tap, and a sheet component has to be built (the review drawer can be reused).

**Best use:** as the **phone half of board 01**. « 01 + 07 » is a valid alternative winner if the owner prefers underline tabs to a grey track on desktop.

## Rejected but worth noting

- **14 (screen 48 done right):** the cheapest fix (~1 d), and the owner already liked it. But phones still scroll inside the track, and a pill-in-pill design stays fragile. Keep it as a fallback if the budget is one day.
- **06 / 15 (bottom bar, dock):** the best thumb ergonomics, but they add a second navigation system next to the global hamburger (HIG: a tab bar is for app sections).
- **17 (sidebar tree):** the cleanest long-term direction (Vercel 2025), but it is an app-shell project, not a bar fix.

## Applying the winner (02) to the rest of the family

One rule set, three tiers. That way the page always has exactly one leading control.

| Place | Today | With board 02 |
|---|---|---|
| **Campaign page** (`app-campaign-bar`, campaign.html) | pill capsule `.cb` | Picker (title weight) + `Review findings` (+ ⋮) on row 1. Grey track, equal segments with icon + label + counter on row 2. Phone: 5–6 icon cells. |
| **Finding page** (same bar, Findings current) | same `.cb` | Same component, Findings segment raised. The finding's own actions stay in the page body, not in the bar. |
| **Frameworks page** (Frameworks inside More) | « Frameworks ⌄ » pill | The More segment shows its current label (« Frameworks ⌄ ») raised, already supported by `moreCurrent()`. The page's `.dv-fw__tabs` become **tier 2**. |
| **Project Infrastructure / Overview tabs** (`.pj-tabs`) | bordered pill group | The same grey track with 2 equal segments, full width up to 360 px. It is the page's main navigation, so it is tier 1. |
| **In-page sub-views** (`.dlc-seg`, `.ov-seg`, `.fa-seg`: « Findings · Treatment plan ») | 40 px pills in a bordered capsule | **Tier 2:** 32 px text segments on a transparent track; current = ink text + 2 px underline or `--active` fill. Never a second grey track right under tier 1. |
| **Map toolbar** (`.kit-bar`, `.kit-seg`) | bordered capsule mixing toggles and buttons | **Tier 3 (tools):** mode toggles as a small grey track (32 px, icons only + tooltips). Plain actions as separate 32 px ghost icon buttons, outside the track. HIG: don't mix actions and selection in one segmented control. |

Shared implementation rules:
1. One component (`dm-seg` with `size: lg | md | sm`), links with `aria-current="page"`, and arrow-key roving between segments. Keep the existing More menu-button logic.
2. The track is `--nt-control` (light `#ededed`, dark `#1a1a1a`) with 3 px padding. The current segment is `--track-on` (white in light, `#3a3a3a` in dark) with a 1 px drop shadow and no border.
3. Equal widths at tier 1 when the labels fit. Below 980 px, drop the icons. Below 560 px, switch to icon-over-label cells. Below 330 px, move Reports into More.
4. Remove the old selectors from `openai-tabs.css`, `chatgpt-ui.css`, `no-rings.css`, `coherence.css` and `ux-fit.scss` §1–2 in the same change. Do not out-rank them.
5. No Space Mono anywhere in the family (the grade chip `.ux-grade` still uses it; switch it to Google Sans Flex 700 with tabular figures).
