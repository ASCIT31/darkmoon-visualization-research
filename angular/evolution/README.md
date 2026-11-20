# UI reform history — DarkMoon Pro Angular client (Mar → Nov 2026)

Snapshot 2026-11-20. Conventions (status labels VERIFIED / OBSERVED / SIMULATED / HYPOTHESIS / SUPERSEDED / UNKNOWN, internal source identifiers `IR-nn`, `IW`, `FT`, `CE`, owner notes) are defined in the [repository README](../../README.md#conventions). Commits are short SHAs of **DarkMoon Pro (internal, private repository)** unless marked Community (public `ASCIT31/Dark-Moon`).

---

## 0. One-paragraph summary

The Pro client went from a functional Angular 21 dashboard (Mar–Apr 2026) with a G6/dagre infrastructure graph (Sep 2026) to a heavily iterated UI: an orbital three.js/WebXR graph (26 Sep), an "Eclipse" redesign with attack-path PoCs (29 Oct), a burst of rejected visual directions (30–31 Oct), the "Focus kit" port on real data (1–9 Nov), the release of the new UI with the Arch-D attack-path release (PR #46, 18 Nov), and the Executive Security V2 views plus the Board 13 campaign header (20 Nov). The dominant constraints that emerged were not aesthetic but epistemic: **never show two contradictory figures for one notion**, **never present a declared or inferred path as proven**, **never invent cards or data**. Roughly 70 UI branches and ~120 worktrees were created; most visual experiments were reverted on owner request (VERIFIED: `FT` §8, §12; branch census in §11 below).

---

## 1. Old UI (Mar → Sep 2026)

| Date | Ref | Event | Status |
|---|---|---|---|
| 2026-03-31 | Pro `14a52fc4` | "feat(front-app): initialize Angular client application" — first client-app commit. | VERIFIED |
| 2026-03-31 | Pro `aead7a0d` | Risk-level badge; Space Mono font added (origin of the charter mono that is removed in Nov). | VERIFIED |
| 2026-03-30 → 04-11 | Pro PRs #1–#19 | Back API + front app: data model (Campaign, Evidence, InfrastructureNode, Target, Vulnerability), dashboard/projects, vulnerability view (PR #18). | VERIFIED (`FT` §2) |
| 2026-06-12 | Pro PR #20 | Chatbot UI. | VERIFIED (timeline) |
| ≤ 2026-09-25 | Pro `client-app/src/app/_common/components/infra-graph/infra-graph.ts` | Infrastructure map = **`@antv/g6` v5**, layout `antv-dagre` (TB, nodesep 50, ranksep 70), 60×60 rect nodes, orthogonal polyline edges, emoji icons, severity stroke. | VERIFIED via `IW-audit` §2 (file:line audit, 2026-09-25); component still present on `origin/dev` |

## 2. Limits of the old UI

1. **Data, not styling, made the graph incoherent** (`IW-audit` §3, VERIFIED as a read-only audit):
   - *Root cause A*: findings were attached to nodes client-side by host matching; endpoints in `"METHOD url"` form failed to parse, every node had the same host, so all unmatched findings fell back to the root node. On one production campaign, 38 of 40 findings had an empty `node_id` (OBSERVED in the audit).
   - *Root cause B*: edges came only from `parent_node_id`, node ids were improvised per agent, producing three parent-less roots and duplicated subtrees.
2. **No attack-path concept** in the UI (audit §2: "no exploit chain / path to crown jewel concept anywhere").
3. **2D canvas only**: G6 renders to a 2D canvas, so "VR" meant opening a flat page in a headset browser (audit §5).
4. **Honesty gaps** found later by adversarial review (IR-12, 2026-11-16, VERIFIED report): a run exiting 0 without finishing was announced "Campaign completed"; replay of a stopped campaign ended on "Completed"; target tiles mixed target-wide and campaign facts ("High / 0 / Vulnerable"); unknown campaign id → infinite spinner.

## 3. Proposals (Sep → Oct 2026)

| Date | Proposal | Outcome | Status |
|---|---|---|---|
| 2026-09-25/26 | Orbital graph lab (`IW`, three.js 0.160, variants A Minimal / B Guided / C Spatial) plus seven static variants A–G (`IW`). | Guided variant kept, ported into Angular (§12). | VERIFIED (files) |
| 2026-09-26 | Server-side deterministic finding attachment + BFS attack paths (`_enrich_infrastructure`) to fix root causes A/B. | Shipped with `0f143981`. | VERIFIED commit; mechanism OBSERVED (owner notes) |
| 2026-10-29 | "Refonte Eclipse": phase 0–1 audit, attack-path PoC round 1, adversarial corrections, server chain data, hybrid direction with ui-ux-pro-max (`5f56c7a7`, `d53d86b2`, `82ffd75a`, `2671db84`, `49d4932a`). | Docs only on `fe/*`, never merged. | VERIFIED |
| 2026-10-29 | "Le Brief" hybrid (plain-language brief, XXL figures, Guided/Expert switch, moon/orbit in three places). | Rejected ("very ugly"; internal judges 4.5/10 ×2). | OBSERVED (owner notes) |
| 2026-10-29 eve | Stand-alone editorial HTML prototype, then 3D "world" dashboard, then variants A Orbit / B Instrument / C Moonlight. | All cancelled 30 Oct: "this is not a dashboard" for full-screen 3D. | OBSERVED (owner notes) |

## 4. Dashboard benchmarks

| When | Benchmark | Use | Status |
|---|---|---|---|
| 2026-09-25 | Grafana, Splunk, JetBrains real dashboards (`IW`). | Context for integrations/infra map. | VERIFIED (files exist) |
| 2026-10-29/30 | ~60 Awwwards projects ("Sites of the Year"), 23 visited; Outcrowd as owner's "inspiration no. 1". | Eclipse 3.0 "SOTY" direction refused; Outcrowd layer 3.1 tried then abandoned. | OBSERVED (owner notes; raw crawl lived in an ephemeral working directory — UNKNOWN whether preserved) |
| 2026-10-30 | ~15 fintech-style references (Finance, Solana ETF, Olymptrade, …). | Rejected as "too banking, not cyber enough"; state frozen at "v11". | OBSERVED (owner notes) |
| 2026-10-30/31 | "Night hunt" design lab: ~95 lots, each citing an Awwwards "Site of the Day"; lots 9 and 95 "no look kept". | Archived (tag `archive/night-hunt-2026-10-31` → `67713196`). | VERIFIED (93 commit subjects) |
| 2026-11-01 | Trade Republic design system (26 public sources) for "Focus-First". | Typography + journey principles adopted (Focus kit). | OBSERVED (owner notes) |
| 2026-11-04 | "Veille 30 outils" + prototypes Atlas / Signal / Story (73 patterns, 194 captures). | Atlas map and frameworks views. | OBSERVED (`FT` §11) |
| 2026-11-20 | Campaign-bar research: GitHub Primer, Linear, Vercel, Stripe, Sentry, Snyk, Grafana, Material 3, Apple HIG, WAI-ARIA APG (`experiments/alternatives/campaign-bar-study/VEILLE.md`). | Fed 20 boards and the Board 13 choice. | VERIFIED (report, with public URLs) |

## 5. UX research method

- **Skill-arbitrated decisions**: from 2026-10-29, the `ui-ux-pro-max` skill had to "decide" every design option, with precedence *owner's words > Darkmoon charter / kit > generic skill advice*; decisions traced in `docs/refonte-eclipse/uxpm/DECISIONS.md` (on `fe/*` branches). OBSERVED (owner notes).
- **Adversarial critics**: "Jobs/Ive" critic over captures of all screens, two rounds (FIXED/PARTIAL), and four judges (pentest expert, novice designer, a11y/perf, non-regression). OBSERVED (owner notes).
- **Measure the reference, don't invent**: computed-style comparison (Playwright `getComputedStyle`) between the kit contract and the app until zero deviation, then per-component pixel diff. OBSERVED (owner notes).
- **Real captures on demo data copies**, before/after at fixed viewports (1440×900, 834×1112, 390×844, 320×640; light/dark). VERIFIED (IR-67 `screenshots/*/capture-log.json`).

## 6. Design system

| Step | Ref | Status |
|---|---|---|
| Eclipse UI kit 2.7.2 loaded, `--ec-*` tokens exposed to Tailwind | Pro `6fb0a2e7` (2026-10-29) | VERIFIED (commit) |
| Eclipse 2.7.3 vendored "purged" (837 KB vs 1.04 MB) | Pro `f0904eb8` | VERIFIED (commit message) |
| Eclipse 3.1 Outcrowd / Lite kit / Material Experience (M3 Expressive) | `2f511af0`, `8f6e7f7d`, `5df0ff58` | VERIFIED; **not merged** (only on `fe/*`) |
| Focus kit L3 shell + tokens | `befd5441` (2026-11-01) | VERIFIED |
| Focus kit v1.0 copied verbatim with sha256 lock | `d5a9182d` (2026-11-04) | VERIFIED |
| Figma iOS/iPadOS kit **not used** (licence forbids web use/extraction); public HIG only | `d38decde` (2026-11-04) | VERIFIED |
| Current tokens: `src/focus/tokens.css` (~406 `--fx-*` custom properties: brand → semantic dark/light → system → motion) + 27 global stylesheets | `origin/dev` | VERIFIED (code read) |

Owner-chosen dark palette (Nov 4, focus-kit theme): page `#000`, cards `#111`, controls `#171717`, hover `#1C1C1C`; edges = fill colour; radii 28/22/14. Later superseded in part by the "nothing-theme" neutrals (`--nt-page #000`, `--nt-tile #1a1a1a`) and the "1 px .15 hairline everywhere" rule. OBSERVED (owner notes; `experiments/alternatives/campaign-bar-study/DESIGN-SYSTEM.md`) — the hex values above are SUPERSEDED by those in `DESIGN-SYSTEM.md`.

**Brand palette rule**: the six charter blues (`--fx-royal #0a2472` … `--fx-alice #dceaf4`) encode entity families, never risk; Alice is the only accent used in UI chrome. VERIFIED (`IR-67` key finding 7, against `src/focus/tokens.css`).

**Typography timeline** (VERIFIED commits unless stated):
- 2026-03-31 Space Mono added (`aead7a0d`).
- 2026-10-31 owner: titles in Google Sans only (OBSERVED, owner notes).
- 2026-11-04 `fd8d6d92` drops dev's global Space Mono uppercase headings; `42ab2afc` maps Tailwind sans/mono to Google Sans Flex / Google Sans Code and removes the Space Mono variable.
- 2026-11-06 `79b3de87` 3D map cards stop using Space Mono.
- **2026-11-20 `d2af641b` "retire the Space Mono display face site-wide"** (owner review, "screen 50": "obsolete"). Space Mono **remains in the PDF report** (`pdf-export.ts`, `pdf-markdown.ts`) and as an unused `@fontsource/space-mono` dependency. VERIFIED.

## 7. Navigation

- Shell: left rail (Dashboard, New campaign, Scheduler, History, Projects, "Your projects"), ⌘K command palette, keyboard help, theme toggle. VERIFIED (`IR-67` §1; `app.ts`).
- Campaign views: Map · Attack paths · Findings · Overview · Reports + More (Replay, Frameworks, simulate, 3D). Frameworks is reached from the report/More by design. VERIFIED (`_common/views-nav.ts`).
- Campaign bar history (VERIFIED commits): capsule bar → compact views 768–1439 px with scroll fade (`660abb89`, 11-05) → title row + underline tabs (`0ef66dac`, 11-06) → **capsule bar restored** the same day (`053718cd`) → **Board 13 header card** (`17a74ea1`, 11-20). See §14.
- Executive navigation chain added 2026-11-19: project → campaign score → plan drawer (action) → finding → framework control → map path; "every link targets an id the server sent". OBSERVED (`IR-67` §3.11).

## 8. Projects

Projects list → project (Infrastructure / Overview tabs) → target tiles. Exec-v2 added a Cyber Score column, project Overview (executive block, application profile) and `/project/:id/profile` editor (`62061de0`, 2026-11-19). IR-12 #4 fix: target tiles now read "on this target, all campaigns". VERIFIED (commit; capture `screenshots/current/pro-campaign-overview-dark-1440-v9.png` shows the wording).

## 9. Campaign history

History is fed by campaigns (owner decision 29 Oct, OBSERVED). Dashboard "Last campaigns": Table | Matrix over time; height reserved until the list arrives to remove a layout shift of 0.1065 measured with a layout-shift observer (`8f6fdf4a`, 2026-11-17). VERIFIED (commit body; see [benchmarks](../../benchmarks/README.md)). Replay (`replay/agent-trace.ts`) — IR-12 #3 showed it ended on "Completed" for a stopped campaign (VERIFIED in IR-12; fix status in current code UNKNOWN, not re-checked).

## 10. Findings

Findings table + finding page (`vulnerability/vulnerability.ts`, 32 KB) with evidence wells, proof level, privacy flags, retest panel; Treatment column/section added in exec-v2 (`cd905aa4`). Discord-style findings rail and "pda.archi" finding layout **reverted** (`231d9d40`, `2e40cd0c`, 11-05). VERIFIED.

## 11. Attack graphs (2D)

| Step | Ref | Status |
|---|---|---|
| Observed-path adapter, "real = observed only", gated ranking | `4d0786f3`, `6ab37775`, `01f04104` (10-29) | VERIFIED (`F2` timeline §7) |
| Legacy orbital attack mode relabelled "exposure path (inventory, not a proven attack chain)" | `009f2a8b` (10-29) | VERIFIED |
| Quarn-style flow graph, pixel-matched, as default infra map | `cbf1d678` (10-30) | VERIFIED; **rejected** ("abject … no smooth transitions") — OBSERVED (owner notes) |
| Atlas 2D layered scope map (zones, chains, cut overlay, semantic zoom, list fallback) | `f26e0c3b` (10-31, `fe/material-v3`) | VERIFIED; not merged as such; Atlas re-implemented in Focus-kit line (`campaign/map/atlas/atlas-canvas.ts`, 442 lines, on `origin/dev`) |
| ELK path graph + ELK infra map (Focus kit) | `7523f232`, `120c45bd` (11-03), ELK vendored `0d9541c1` | VERIFIED |
| Server proven paths drawn on the orbital map | `77b41f56` (11-03) | VERIFIED |
| Operator-declared paths, admin-only, "Declared by operator · not proven" | `506f390e`, `c6fb1438` (11-04); `infra/declare-path/declare-path-sheet.html:8` | VERIFIED |
| Accessible map legend (states, not colour alone) | `b8f91027` (11-11) | VERIFIED |
| One legend vocabulary (Evidence-Backed = solid + shield; Possible = dashed + dashed circle; Treatment in progress = dotted + wrench) across 2D map, paths rail, 3D legend | `f3283be0` (11-19) | VERIFIED (commit); semantics OBSERVED (`IR-67` §3.9) |

## 12. Orbital graph

See [orbital graph research](../../research/graph-layouts/orbital-graph.md). Key points: lab → Angular port (`0f143981`, 2026-09-26) with "Guided" variant only, English, charter colours; owner rejected a from-scratch rebuild ("not at all that") — the lab engine is the base (OBSERVED, MEM). Size 410 lines (Sep/Nov local) → 805 lines on `origin/dev`. VERIFIED.

## 13. Structured report

- Report page: Markdown via `marked` + custom renderers, Mermaid `securityLevel:'strict'`, `neutraliseMedia()`, no `bypassSecurityTrust`. VERIFIED (`IR-63` §0.3).
- PDF: pdfmake; "PDF export in the official charter with real UI diagrams" (`202c5893`) then "A4 corporate pentest report" (`4bb6c6e7`, 11-08) — owner rule: PDF = A4 portrait corporate, charter as identity only (OBSERVED, owner notes).
- Contradiction bug: PDF summary tile "0 proven" vs section "5 proven" caused by a regex scraping the annex in `pdf-export.ts summary()`; fixed (OBSERVED, CE; owner notes).
- Exec-v2: report body keeps first 10 actions, full plan moved to an appendix (`796fe8a0`). VERIFIED (commit list in `IR-67` §1).

## 14. Progressive disclosure

- Owner verdict 30 Oct: "there is an idea but it's a mess" → one main question per view, detail on click, short texts, honesty notes as tooltip/line (OBSERVED, owner notes).
- Implemented patterns (VERIFIED in `IR-67`): executive block shows one coverage sentence and "one collapsed regulatory line" (`<details>`); "How this plan is built" fold; first 10 actions then "Show all N"; "What do these mean?" legend text.
- **Board 13** (2026-11-20): the 20-board study recommended **Board 02** (grey-track segmented control) first, **Board 01** second, **Board 07** third; Board 13 ("fixed tabs in a card") was rated "heavy on desktop" (`RECOMMENDATION.md`). **The owner chose Board 13** (owner notes, "Owner choice 20/11"); implemented as `17a74ea1` "campaign header card with fixed equal-width tabs (board 13)", which deletes the old `.cb` rules from 11 stylesheets instead of out-ranking them. VERIFIED (commit body; `campaign-bar.ts:17`). Release decision notes it was "implemented under time pressure; captures to be reviewed by the owner after deploy" (`IR-67`). A follow-up `03b8fc10` stopped a theme catch-all (`* { border-color: transparent !important }`) from erasing its hairline.

## 15. Performance

See [benchmarks](../../benchmarks/README.md). Lazy routes (`8c73481d`, "initial JS 2.32 MB → 643 kB", commit message only); orbital render-on-demand + GPU cleanup (`7c86bc05`); CLS fix (`8f6fdf4a`); component-style budget raised 20/40 kB → 48/96 kB (`8cebd509`) because `map-rail.scss` broke the prod build; `map-rail.scss` still exceeds the 48 kB warning (72.36 kB) in production builds.

## 16. Angular fixes from adversarial review (IR-12, 2026-11-16)

| # | Severity | Issue | Status |
|---|---|---|---|
| 1 | BLOCKER | Authentication textarea re-ordered/re-split secrets while typing (comma split); spec baked the bug in. | VERIFIED in IR-12; fix status UNKNOWN (not re-checked) |
| 2 | HIGH | `campaign_unfinished` / `aborted` runs announced "Campaign completed". | INFERRED in IR-12 |
| 3–5 | MED | Replay chip, contradictory target tiles, infinite spinner on unknown campaign. | VERIFIED in IR-12; #4 fixed (wording visible in final captures) |
| 6 | MED | `/campaign/:id/frameworks` horizontal scroll (1471 px at 1440; 1032 px at 390). | VERIFIED in IR-12 |
| 7–10 | LOW | No stopped banner on report; `/infra/<unknown>` false empty state; global Escape listener; "1 spans". | VERIFIED/INFERRED in IR-12 |

Positive checks in the same review: PDF export after lazy import (519,852-byte file), deep links after lazy routes, 0 console errors on valid routes in both themes and viewports, XSS specs 116/116, demo campaign explicitly labelled "Synthetic lab · deterministic simulated provider". VERIFIED (report).

Earlier security fixes found during UI work: path traversal on `campaign_id` and stored XSS/CSP hotfixes (`ae884cc3`, `6614d823`, `d52fe904`, 11-04/05). VERIFIED (timeline §6).

---

## 17. Rejected attempts and owner rejections (consolidated)

| Date | Attempt | Rejection / outcome | Evidence | Status |
|---|---|---|---|---|
| 2026-09-26 | Rebuild of the orbital graph "in its own way" (route decomposition, satellites) | "That is not it at all" → lab engine reused | owner notes | OBSERVED |
| 2026-10-29 | "Le Brief" dashboard over the Eclipse kit | "very very ugly"; judges 4.5/10 | MEM | OBSERVED |
| 2026-10-29/30 | **Invented cards** (neon activity chart, severity bars, "Last campaign", "Top risky targets") | "I forbid you to invent cards, modernise what exists"; reverted to the 4310 blocks (`73bfc2c7`) | VERIFIED commit + owner notes | VERIFIED. **SUPERSEDED in part 2026-11-20**: "you may add cards if well made", still no duplicate/filler |
| 2026-10-30 | Full-screen 3D dashboard (variant A Orbit) | "that's not a dashboard"; 3D = accent/infra map only | MEM | OBSERVED |
| 2026-10-30 | ~15 fintech style layers | "abject, cancel"; "too banking" | MEM | OBSERVED |
| 2026-10-30 | Liquid glass, Petrillo severity emblems, ambient field | Reverted `6cf7b611`, `cd53a340`, `30d1860a`, `f7c1c63b` | git | VERIFIED |
| 2026-10-30 | Pixel-matched Quarn flow graph | "absolutely ABJECT as an experience" | MEM + `cbf1d678` | OBSERVED / VERIFIED commit |
| 2026-10-31 | "Darkmoon Visual Engine" new design system | "nothing of the Eclipse kit in there" | owner notes | OBSERVED |
| 2026-10-31 | Night hunt lots | "no look kept" (lots 9, 95) | git | VERIFIED |
| 2026-11-03 | Focus-kit **skins** (LOT 3: A, retouches, p4n, deep black, card views) | Rejected; pivot to strict Focus kit on real data (`feat/focus-kit-real`) | owner notes | OBSERVED |
| 2026-11-04 | Invented sidebar / project tree / findings rail / matrix variants | "horrible", "nothing to do with the kit"; only measured copies liked | owner notes | OBSERVED |
| 2026-11-04 | Kit shell restructure, route blur cross-fade, summary strip, darker grey ladder | Reverted `9fca57a3`, `1094ede8`, `58604f4d`, `b8397de5` | git | VERIFIED |
| 2026-11-05 | All card border/edge/grain/glass/gap experiments; Discord rail; pda.archi layout | Reverted (17 reverts incl. `3551998c`, `378a67fe`, `231d9d40`, `2e40cd0c`) | git; "c'est abjecte" (MEM) | VERIFIED |
| 2026-11-06 | Underline-tab campaign bar | Capsule restored same day (`053718cd`) | git | VERIFIED |
| 2026-11-18 | Wording change on `enabled_by` labels | Reverted `cbc4df04` (gain not demonstrated, n=1) | timeline §7 | VERIFIED |
| 2026-11-20 | Space Mono | Retired site-wide (`d2af641b`) | git | VERIFIED |
| 2026-11-20 | Board 02 (study's first choice) | Owner chose Board 13 | report + MEM + `17a74ea1` | VERIFIED (implementation) / OBSERVED (choice) |
| 2026-11-20 | "Treatment overview" card | Removed by owner decision; counts merged in one line | `IR-67` §8.1 | OBSERVED |

## 18. Branch census (VERIFIED, `git branch -a` against `origin/dev` @ `d6bfc0d9`)

| Family | Count | Window | Merged |
|---|---|---|---|
| `fe/*` (base-secu-attackpaths, cockpit-v4, dash-v5, flow-graph, material-v3, outcrowd) | 6 | 10-29..10-31 | none |
| `feat/focus-*` | 7 | 11-01..11-09 | focus-kit-ap-v1, focus-skin |
| `feat/fkr-*` | 4 | 11-01..11-03 | none |
| `feat/int-*` | 7 | 10-29..11-04 | int-d, int-e |
| `feat/fk-*` (per-lot) | 33 | 10-29..11-10 | all, via `feat/dev-focus-kit` |
| `feat/dev-focus-kit`, `feat/dev-visual-integrate` | 2 | 10-29..11-08 | yes |

`git worktree list`: 120 entries. Tags: `archive/night-hunt-2026-10-31`, `focus-kit-real-stylish-v1` (`9d25af34`), `focus-dashboard-ref-v1` (`de5f420d`). `fe/brief-dashboard` and `feat/eclipse-attack-paths` no longer exist in the working clone (UNKNOWN content beyond owner notes).

## 19. Open questions (UNKNOWN)

- Whether IR-12 findings #1, #2, #3, #5–#10 are fixed on `origin/dev` (not re-checked in this phase).
- Owner review of Board 13 captures after deploy (`IR-67` item 6): outcome not recorded in sources read.
- The Awwwards/Outcrowd raw crawls lived in an ephemeral working directory; preservation UNKNOWN.
