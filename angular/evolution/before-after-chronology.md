# Before / after — illustrated chronology

Initial → prototype → iterations → current, one row per step. Each step gives **capture, commit, goal, problem, change, result, limitation**. Captures are real UI screenshots unless marked **MOCK-UP** (static board or stand-alone prototype, not the product). Only the captures published in this repository are linked; others are cited by their internal source identifier and are **not** published (see [screenshots/README.md](../../screenshots/README.md) for the selection and privacy rules).

Conventions (status labels VERIFIED / OBSERVED / SIMULATED / HYPOTHESIS / SUPERSEDED / UNKNOWN, internal source identifiers `IR-nn`, `IW`, `FT`, `CE`, owner notes) are defined in the [repository README](../../README.md#conventions). Commits are short SHAs of **DarkMoon Pro (internal, private repository)** unless marked Community (public `ASCIT31/Dark-Moon`).

## Publication status of capture sets

| Set | Published here? | Reason |
|---|---|---|
| Executive Security V2 captures (IR-67: before/after, owner passes, Board 13, final, TUI) | **Curated subset** in `screenshots/` | Synthetic demo data (renamed copies of lab campaigns against DVGA/DVWA); IPs tokenised; each published file was checked visually |
| IR-67 TUI captures showing a not-rated DVWA workspace | No | A Docker-bridge private address is visible |
| Dashboard / Home captures | No | The DVWA demo target is shown by a Docker-bridge private address |
| Orbital lab prototype and its captures (IW, Sep 2026) | No | Lab data embeds private lab addresses |
| Focus-kit captures and boards | No | Sidebars show lab project names containing private lab addresses |
| Material-v3 and Eclipse-redesign captures | No | Local backend data not reviewed |
| Commercial UI-kit screenshots | Never | Private licence, not for redistribution |
| Adversarial review shots (IR-12) | No | Not preserved |

---

## Step 0 — Initial UI (Mar → Sep 2026)

- **Capture**: `Pro origin/dev:docs/pics/{matrix,report,start-assesment,sub-agent}.png` (README illustrations; era UNKNOWN); website static re-creation of the infra graph — **MOCK-UP** (described in `IW-audit` §1, not a product capture).
- **Commits**: `14a52fc4` (2026-03-31), PRs #1–#19, `aead7a0d`.
- **Goal**: functional dashboard, projects, campaigns, vulnerability view, G6 infrastructure graph.
- **Problem**: incoherent graph (findings on one node, duplicated subtrees), no attack-path notion, 2D only.
- **Result / limitation**: VERIFIED via audit; no screenshot of the broken production graph is preserved in the sources read (UNKNOWN).

## Step 1 — Orbital prototype (2026-09-25/26)

- **Capture**: `IW` (interactive **MOCK-UP**/prototype, variants Minimal/Guided/Spatial), `IW` (**MOCK-UP** variants), iteration captures (desktop, flat, OSI-decomposed, VR preview). Not published (lab addresses).
- **Commit**: none (lab outside the repo).
- **Goal**: PC/tablet/VR infra map with exposure rings and paths.
- **Change**: three.js 0.160 lab with inline lab data.
- **Result**: Guided variant selected (OBSERVED).
- **Limitation**: lab data embeds a private address; variant letters undocumented.

## Step 2 — Orbital map in the product (2026-09-26/27)

- **Capture**: `IW` (real UI against a mock API serving one stored campaign — OBSERVED, owner notes); a VR preview capture on an unmerged branch. Not published (lab addresses).
- **Commits**: `0f143981`, `a5e2d546`.
- **Goal**: replace the G6 graph view with the guided orbital map; fix data at the server.
- **Problem**: rebuild "in its own way" rejected; lab colours off-charter.
- **Change**: lab engine ported, Guided only, English, charter colours, server-side finding attachment + BFS paths.
- **Result**: coherent map on lab data (57/57 attached, OBSERVED).
- **Limitation**: BFS "attack paths" were exposure heuristics, later relabelled (Step 4).

## Step 3 — Refonte Eclipse and rejected directions (2026-10-29 → 10-31)

- **Capture**: internal captures of the rejected directions (56 baseline shots, 14 routes × 320/390/1280/1440; M3 journey captures). Not published (local backend data not reviewed).
- **Commits**: `5f56c7a7`, `d53d86b2`, `82ffd75a`, `2671db84`, `49d4932a`; `2f511af0`, `58045c48`, `5c6a9257`, `cbf1d678`, `8f6e7f7d`; `5df0ff58`, `69f28909`, `729a87f3`, `f26e0c3b`; reverts `30d1860a`, `6cf7b611`, `cd53a340`, `f7c1c63b`; night hunt `b3b9505b` … tag `archive/night-hunt-2026-10-31`.
- **Goal**: an "innovative" dashboard + attack-path prioritisation.
- **Problem**: owner rejections (Le Brief, 3D world, fintech layers, liquid glass, flow graph, invented cards).
- **Change**: none of `fe/*` merged; rule "modernise, never add blocks" (`73bfc2c7`).
- **Result**: direction narrowed to "classic dashboard, 3D as accent".
- **Limitation**: ~95 night-hunt lots and Awwwards crawls produced no kept look; raw crawls in ephemeral storage.

## Step 4 — Focus kit on real data + honest maps (2026-11-01 → 11-09)

- **Capture**: internal captures (132 final, fidelity series, side-by-side kit vs app boards, partly **MOCK-UP** references). Not published (sidebar project names contain private lab addresses).
- **Commits**: `befd5441`, tag `focus-kit-real-stylish-v1` (`9d25af34`), `d5a9182d`, `7523f232`, `120c45bd`, `77b41f56`, `09fb2fc0`, `506f390e`, `c6fb1438`; reverts `dac5e2f8`, `9fca57a3`, `6cabe26d`, `58604f4d`, `1094ede8`, `b8397de5`, `3551998c`, `378a67fe`, `231d9d40`, `2e40cd0c`; frameworks `2eed7e4c`, `57498e2d`.
- **Goal**: mature kit, real fields only, proven-only path drawing, operator-declared paths, frameworks.
- **Problem**: invented component variants rejected; Discord rail/pda.archi rejected; style budget broke prod build.
- **Change**: verbatim kit copy with sha256 lock; ELK path graph; Atlas 2D; orbital "without invented narrative"; budget 48/96 kB (`8cebd509`).
- **Result**: merged via `feat/dev-focus-kit` / `feat/fk-*` (33 lots) — VERIFIED.
- **Limitation**: 27 global stylesheets with `!important` layers (debt surfaced in Step 7).

## Step 5 — Release with Arch-D attack paths (2026-11-16 → 11-18)

- **Capture**: IR-12 review shots (ephemeral, UNKNOWN); release captures in `IR-61` and `IR-62` (not opened in this phase — UNKNOWN content).
- **Commits**: lazy routes `8c73481d` (earlier), PDF lazy import `54666f85`, orbital arrow keys `0e0c7133`/`7c674f6c`, CLS fix `8f6fdf4a`; release `release/arch-d-20261118` → Pro PR #46.
- **Goal**: ship the new UI with agent-declared paths labelled, never proven.
- **Problem** (IR-12): auth textarea corrupting secrets, "Campaign completed" for unfinished runs, contradictory target tiles, frameworks overflow on phone.
- **Change**: release; IR-12 #4 wording fixed later (visible in Step 7 captures).
- **Result**: PR #46 merged 2026-11-18 (VERIFIED, timeline).
- **Limitation**: fix status of other IR-12 findings UNKNOWN.

## Step 6 — Terminal Explorer (2026-11-18 → 11-20)

- **Capture**: internal series IR-64 (V1, 16 screens; V2), IR-65 (visual iterations `vq-before` → `vq-it1..3`, border polish), IR-66 (Community port), IR-67 (36 PNG per edition). Published subset: `screenshots/current/*-tui-*.png`.
- **Commits**: OpenCode fork `f4e363c8`; Pro `bbc87314` (PR #53), `6a30b3a7`/`de065105`, `9c77b70e` (PR #54), `edecb5a9`, `ddfd523c`, `d225566a`, `17f9f506` (PR #60); Community PR #59, PR #62.
- **Goal**: read-only cockpit in the console, zero model calls, both editions.
- **Problem**: fork lacked TUI API; V1 bland, truncated ids, unused data, no ASCII fallback; root `esc` leaked keystrokes to the chat.
- **Change**: fork resync; workspace, sparkline/bars, ASCII fallback, single-line pane borders, 80-column tabs, shared legend words.
- **Result**: bun 15 → 48 tests; GO Pro; Community shipped via PR #59/#62 (VERIFIED).
- **Limitation**: no images, host-styled Markdown tables, branches not merged into a tree, no render-time measure.

## Step 7 — Executive Security V2 + Board 13 (2026-11-19 → 11-20) — current

- **Capture**: published in this repository: `screenshots/historical/*-v1.png` (before, `b15d9b13`), `screenshots/current/*` (after and final), `screenshots/comparisons/*` (owner passes, Board 13, polish). Current reference: [`pro-campaign-overview-dark-1440-v9.png`](../../screenshots/current/pro-campaign-overview-dark-1440-v9.png) — Board 13 header card with Map · Attack paths 2 (+17) · Findings 68 · Overview · Reports · More, Cyber Score "E 0/100 Rated", "No ranking by attack path". Board study **MOCK-UPS**: [`experiments/alternatives/campaign-bar-boards/`](../../experiments/alternatives/campaign-bar-boards/).
- **Commits**: `f31eb44a` … `5d607fba` (L7), `027f9c25`, `17fcaef3`, `a812fae2`, `0973ee99`; `d2af641b` (Space Mono retired); `17a74ea1` (Board 13); `03b8fc10`; PRs #56, #57, #62 → `origin/dev` `d6bfc0d9`.
- **Goal**: executive reading (score, treatment plan, frameworks) on top of operator views; fix campaign-bar defects (void, overlapping borders, wrapping, hidden views on phone).
- **Problem**: five stacked CSS layers on the bar; contradictory tiles ("7 fixes tied" vs 67 actions); Space Mono judged obsolete.
- **Change**: server-computed executive block, plan, legend parity; Board 13 replacing the capsule, old rules deleted; Space Mono removed.
- **Result**: 835–844 unit tests green, CI green, production deploy (VERIFIED, IR-67).
- **Limitation**: Board 13 "implemented under time pressure", owner review of captures pending (UNKNOWN); `map-rail.scss` style budget warning persists; path-kind counting divergence for corroborated `derived` paths (OBSERVED).

---

## Summary table

| Step | Date | Key capture (path) | Commit(s) | Real / mock-up |
|---|---|---|---|---|
| 0 | Mar–Sep | `Pro origin/dev:docs/pics/*` | `14a52fc4` | real (era UNKNOWN) |
| 1 | 09-25/26 | `IW` | — | **mock-up/prototype** |
| 2 | 09-26/27 | `IW` | `0f143981` | real UI, mock API |
| 3 | 10-29/31 | internal, not published | `5f56c7a7` … reverts | real UI, not merged |
| 4 | 11-01/09 | internal, not published | `befd5441` … | real UI (lab data) |
| 5 | 11-16/18 | (IR-12 shots, ephemeral) | PR #46 | real UI |
| 6 | 11-18/20 | `screenshots/current/*-tui-*` | PR #53/#54/#60, Community #59/#62 | real TUI |
| 7 | 11-19/20 | `screenshots/current/`, `screenshots/comparisons/` | `17a74ea1`, `d2af641b`, PR #56/#57/#62 | real UI, synthetic demo data |
