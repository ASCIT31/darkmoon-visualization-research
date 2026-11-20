# Data-visualization research — DarkMoon security data

Code paths are under the Pro client `client-app/src/app/` on `origin/dev @ d6bfc0d9` (internal) unless stated. Captures are real UI screenshots unless marked **MOCK-UP** (hand-built board/prototype, not the product).

Conventions (status labels VERIFIED / OBSERVED / SIMULATED / HYPOTHESIS / SUPERSEDED / UNKNOWN, internal source identifiers `IR-nn`, `IW`, `FT`, `CE`, owner notes) are defined in the [repository README](../../README.md#conventions). Commits are short SHAs of **DarkMoon Pro (internal, private repository)** unless marked Community (public `ASCIT31/Dark-Moon`).

## 1. Governing principles (derived from the record)

| # | Principle | Origin | Status |
|---|---|---|---|
| P1 | **The server computes, the UI displays.** Every number on a view comes from a server document; the client never recomputes a score, a ranking or a path. | `IR-67` header; exec-v2 parity tests `test_exec_legend_front_parity.py`, `test_exec_front_word_parity.py` | VERIFIED (report + test names) |
| P2 | **Never two contradictory figures for one notion on a screen.** | Owner, session C 2026-11-03 (CE) | OBSERVED; violations found and fixed: PDF "0 proven" vs "5 proven" (regex bug), `verdict.ts` miscount on 8 surfaces (CE) |
| P3 | **Proof level is encoded by line style + word + icon, never by colour alone and never by severity colour.** Evidence-Backed = solid + shield; Possible = dashed + dashed circle; Treatment in progress = dotted + wrench. | `f3283be0`; `IR-67` §3.9 | VERIFIED (commit) |
| P4 | **Severity colours appear on data only, never on navigation chrome; brand blues encode entity families, never risk.** | `experiments/alternatives/campaign-bar-study/DESIGN-SYSTEM.md` ("Rule"); `src/focus/tokens.css` comment cited in `IR-67` | VERIFIED |
| P5 | **Nothing invented**: no fabricated assets, labels, narratives or cards; a guided story exists only for a server-proven path. | `09fb2fc0`; `orbital-graph.ts` l.38–42; `ap-graph-logic.ts` l.9 "nothing is invented" | VERIFIED |
| P6 | **One main question per view, details on demand.** | Owner 2026-10-30 (owner notes) | OBSERVED |

## 2. Graph families studied

### 2.1 Classic (hierarchical) graph — G6 + dagre
- `_common/components/infra-graph/infra-graph.ts` (`@antv/g6` v5, `antv-dagre` TB). Still in the code base.
- Strength: familiar tree. Weakness: with incoherent data (duplicate roots, missing `node_id`) dagre spreads disconnected "islands" and every finding falls on the root (`IW-audit` §2–3). VERIFIED (audit).
- Reference mock: website `InfraView()` was a hand-placed static SVG re-creation of this graph (audit §1) — **MOCK-UP**.

### 2.2 Oriented layered graph — ELK ("ap-graph")
- `_common/components/ap-graph/ap-graph.ts` (186 l.), `ap-graph-logic.ts`, `elk.ts` (ELK vendored as `public/vendor/elk/elk.bundled.js`, loaded on demand). VERIFIED.
- Design (header comment, VERIFIED): "Paths to <objective>": entry left → critical asset right (top → bottom for long paths); orthogonal edges with rounded corners (React Flow "smoothstep", radius 8); **proven links solid, hypotheses dashed**; MITRE technique chip on each link of the selected path; **double ring on critical asset and on choke points (×N = paths through)**; hovering an asset lights every path through it; rendered by Angular templates (no `innerHTML`).
- Ordering rule (`ap-graph-logic.ts` l.34): proven first, then criticality, then length (shorter = easier), then id.
- Edge merge rule (l.59): one link per (from → to); proven as soon as one proven path uses it.
- Origin: ported from the Focus-kit reference prototype `ui/ap-graph.ts` (`7523f232`, 2026-11-03). ELK cost on phones was an explicit decision topic (`76be717d`, decisions D53–D58). VERIFIED (commit subjects); decision content UNKNOWN (file on `feat/focus-kit-real`, not read).
- Robustness fix: "an ELK constructor present now wins over a pending script load" (`3e45fd55`, 2026-11-20). VERIFIED.

### 2.3 Orbital graph (3D, WebXR)
See [orbital graph research](../../research/graph-layouts/orbital-graph.md). Radial encoding: ring = server exposure zone (data 0, app 1, edge/internet/tooling 2). VERIFIED.

### 2.4 Infrastructure map (2D "Atlas")
- `campaign/map/campaign-map.ts` + `campaign/map/atlas/atlas-canvas.ts` (442 l.), `atlas-model.ts` (~40 KB), `atlas-session.ts`. VERIFIED.
- Owner reference (2026-11-07, quoted in the header comment): the AWS diagram style of a public diagramming product — nested boundaries, orthogonal relations, floating bottom toolbar (Overview / Standard / Detailed, zoom %, Fit / Reset, server paths, Report, Open in 3D, View in VR, "Updated Xd ago"); the 2D view takes the whole page with the campaign bar floating over the dotted canvas. VERIFIED (code comment).
- Earlier ancestor: "2D layered scope map (zones, chains, cut overlay, semantic zoom, list fallback)" `f26e0c3b` on `fe/material-v3` (not merged). VERIFIED.
- Semantic zoom with 12 px label level-of-detail and fit-to-chain on load (`0e7c6b11`, gate round 1). VERIFIED (commit subject).

### 2.5 Flow graph (rejected)
- Quarn-style node-editor graph, pixel-matched to a Dribbble shot (`cbf1d678`, `fe/flow-graph`). Rejected for lack of motion/depth ("abject … no smooth transitions or blur"); lesson recorded: "a reference is a mood for an experience, not a static image to trace". OBSERVED (owner notes). Never merged. VERIFIED.

## 3. Path representation

| Aspect | Current solution | Status |
|---|---|---|
| Proof classes | Server `provenance`: observed / derived / declared / inferred / agent-declared; UI collapses to **Evidence-Backed** (observed, or derived only when every claim checked) vs **Possible**. `infra/attack-path-overlay.ts:35,83,95`. | VERIFIED |
| Operator-declared | Separate class, thin dashed, "Declared by operator · not proven … Never counted with observed attack paths, the Prioritized Treatment Plan or the report" (`infra/declare-path/declare-path-sheet.html:8`). | VERIFIED |
| Agent-declared | `declared_by_agent` exists in the model (`_common/models/attack-paths.ts:42,46`) but no template renders that literal; shown as "Possible". | VERIFIED |
| Counts | "Paths: 2 evidence-backed · 17 possible"; in the campaign tab counter `Attack paths 2 (+17)`. | VERIFIED in capture `screenshots/current/pro-campaign-overview-dark-1440-v9.png` (synthetic demo copy) |
| Exposure walk | When no path is proven, the guided map offers an **exposure path** with the line "a reading of the inventory, not a demonstrated attack". | VERIFIED (`orbital-graph.ts` l.63) |
| Fix simulation | Interrupted scenarios dashed/dimmed with a badge; server simulation read-only (sha256 of data dir unchanged in independent review, 18/18 "resist"). | Code VERIFIED; review OBSERVED (owner notes) |
| Known inconsistency | `exec_core.legend.path_kind` reads every non-`observed` provenance as Possible while the rail treats corroborated `derived` as evidence-backed — counts can differ (0 on demo data). | OBSERVED (`IR-67` §7 item 1) |
| TUI | Vertical chain of node cards; branches listed path by path; `─▶` proven, `╌▶` hypothesis. | VERIFIED (see [TUI research](../../tui/architecture/README.md)) |

## 4. Master-detail

- **Map + rail**: `infra/rail/map-rail.ts` (46 KB) is the shared Resources rail of the 2D Map tab and of `/infra` (3D); owner decision 2026-11-07 to reuse "the rail of the finding page" instead of a menu table; asset/relation details open in the rail. VERIFIED (code comment l.39–42).
- **Finding page**: left source rail + body + "More info" sections (Treatment, paths with this finding). VERIFIED (IR-67 §3.7).
- **Plan drawer**: native modal `<dialog>`, focus trapped, Escape returns focus to the row; URL `?tab=fixes&action=<id>`. OBSERVED (IR-67 §3.6).
- **Frameworks**: framework tree in the left rail (`2eed7e4c`), counters C H M L I per node. VERIFIED.
- **Terminal**: list + inspector panes (Lazygit-inspired, see TUI doc).

## 5. Vulnerability visualisation

| View | Encoding | Capture (real UI, demo data) | Status |
|---|---|---|---|
| Campaign "By severity" | Donut with total in the centre + legend with counts (Critical 10, High 21, Medium 23, Low 13, Info 1) | `screenshots/current/pro-campaign-overview-dark-1440-v9.png` | VERIFIED (capture viewed) |
| "Findings by agent" | Horizontal bars with counts | same | VERIFIED |
| KPI tiles | Big number + one short line ("34 Exploited", "10 Critical", "2 findings On attack paths", "14 actions Treatment plan") | same | VERIFIED |
| Dashboard "Last campaigns" | Table | Matrix over time (stacked bars per target, SVG) | `IR-67 captures` | VERIFIED (files) |
| Frameworks matrix | ATT&CK / ISO Annex A / ATLAS coverage matrices (PurpleLab-inspired; AGPL tool, "rendering reproduced, no code", `328573cb`) | `IR-67 captures` | VERIFIED (commit) |
| Project sunburst / evolution | `project/sunburst/`, `project/evolution/` | not captured in sources read | VERIFIED (code exists) / UNKNOWN (usage) |

Charts are custom SVG/Angular; `chart.js` is declared in `package.json` but imported by no source file. VERIFIED.

## 6. Visual prioritisation

- **Server "Fix first" → Prioritized Treatment Plan**: one action per asset × weakness class; tile reads the plan ("14 actions … 4 tied at the top"), never the older `fix_first` value (R5 H1). OBSERVED (IR-67 §8.1).
- **No ranking without proof**: "No ranking by attack path. No Evidence-Backed Attack Path in this campaign: nothing to rank by paths." visible in the final capture. Engine refuses path ranking with fewer than 2 proven paths sharing a step. VERIFIED (capture) / OBSERVED (MEM, engine message).
- **Cyber Score**: grade letter + /100 or the word "Not rated" (never an empty number); grade letter coloured A green → E red, Not rated neutral (owner 20/11). VERIFIED (capture shows "E 0/100 Rated") / OBSERVED (rule). Methodology labelled "not scientifically validated" (timeline §8).
- **Severity colour discipline** (critic rules, 2026-10-30): red/orange = critical/high only, yellow = exploited, medium/low/info grey. OBSERVED (owner notes). **SUPERSEDED in part**: current tokens give medium yellow `#F4FA6A` and low blue `#50ADFD` (`DESIGN-SYSTEM.md`).

## 7. Accessibility

| Measure | Evidence | Status |
|---|---|---|
| State never by colour alone (shape/line + word) | `b8f91027`, `IR-67` §4 | VERIFIED |
| Contrast fixes: Fixes list 1.07 → fixed (`bcc82859`); campaign-bar primary light 1.12:1 → 3.5:1 shape (`1c6322f3`); frameworks "every text ≥ 7:1" (`ca798845`); 2 axe colour-contrast nodes on orbital labels (`02808ed4`) | commits | VERIFIED (claims in commit messages) |
| 44 px targets at phone width (`b1d31ea9`); keyboard e2e with axe at 1440/390 (`8050fe6e`) | commits | VERIFIED |
| Campaign views = `<nav>` links + `aria-current`, not `tablist` (VEILLE §8) | `VEILLE.md` | VERIFIED (recommendation); implementation in Board 13 OBSERVED (commit body: "ids, data-view and routes kept") |
| Reflow: no horizontal page scroll at 390/320 px | IR-12 #6 found `/frameworks` at 1032 px on phone; IR-67 §6 "no horizontal overflow at 390 px" on listed views | VERIFIED (both reports; frameworks fix status UNKNOWN) |
| Live regions, `aria-busy` on reserved boxes, sr-only captions | IR-67 §4 | OBSERVED |
| PDF: greyscale-safe severity markers, white cover | `d6c6f570` | VERIFIED |

## 8. Ergonomics and density

- **Density reference chosen by owner (20/11)**: "compact container with two rows of filled grey pills"; hates empty void and low-contrast pills; labels never wrap; KPI = big number + one short line. OBSERVED (MEM).
- **Campaign-bar study** (20 boards, `experiments/alternatives/campaign-bar-boards/`, **MOCK-UPS** rendered at 1440/834/390/320 light/dark): scored against owner complaints V (void), B (overlapping borders), W (wrapping), M (mobile visibility). Rules: split context / views / action; no wrapping tab rows; one edge per component; phones show all views without sideways scroll.
- **Layout stability**: reserved heights for async cards (CLS 0.1065 → fixed, `8f6fdf4a`); "nothing moves while loading" on campaign Overview (one-render gate). VERIFIED (commit) / OBSERVED (IR-67 §3.5).
- **Concentric radii and hairlines** drawn as inset box-shadow so they add no layout width. OBSERVED (`DESIGN-SYSTEM.md` Geometry).

## 9. Captures and prototypes

The published, privacy-checked captures are in [`screenshots/`](../../screenshots/README.md) (real UI on synthetic demo data) and [`experiments/alternatives/campaign-bar-boards/`](../../experiments/alternatives/campaign-bar-boards/) (**MOCK-UPS**). Earlier prototype and iteration captures (orbital lab, rejected directions, Focus-kit port) stay internal; the reasons are listed in the [before/after chronology](../../angular/evolution/before-after-chronology.md#publication-status-of-capture-sets). Commercial UI-kit screenshots are never published (private licence).

## 10. Open research questions

1. No user study (task completion, comprehension of "Evidence-Backed vs Possible") has been run. HYPOTHESIS: the line-style + word encoding is understood without training; untested.
2. No measured comparison of 2D Atlas vs ELK ap-graph vs orbital for the same task. UNKNOWN.
3. Severity palette divergence between orbital graph and app tokens ([orbital graph research](../../research/graph-layouts/orbital-graph.md) §6).
4. Branching attack paths are never merged into a DAG view in the TUI and only partially in ap-graph (shared assets drawn); whether a merged tree aids triage is untested.
