# DarkMoon Visualization Research

Research record of how **DarkMoon** shows offensive-security results to people: the Pro Angular dashboard, attack-path graphs (2D layered, 2D Atlas map, 3D orbital/WebXR), the executive views, and the **Terminal Explorer** that runs inside the OpenCode console in both the Community and Pro editions.

This is a research repository, not product code. It documents what was tried between March and November 2026, what was kept, what was rejected and why, with real before/after captures and the measures that actually exist.

| | |
|---|---|
| Authors | ASC (ASC-IT) — DarkMoon team |
| Version | 1.0.0 (2026-11-20) |
| Product (public) | [DarkMoon Community](https://github.com/ASCIT31/Dark-Moon) — Terminal Explorer at [`conf/plugins/darkmoon-explorer`](https://github.com/ASCIT31/Dark-Moon/tree/master/conf/plugins/darkmoon-explorer) |
| Pro UI demo | [demo.dark-moon.org](https://demo.dark-moon.org) (the Pro Angular UI) |
| Documentation | [docs.dark-moon.org](https://docs.dark-moon.org) |
| Sister repository | [darkmoon-attack-paths-research](https://github.com/ASCIT31/darkmoon-attack-paths-research) (attack-path engine, evidence and causality) |
| Licence | [GNU AGPL-3.0](LICENSE) for the text and original material of this repository; third-party works keep their licences ([NOTICE.md](NOTICE.md)) |

> The DarkMoon Pro source code is private. Pro facts are cited as « DarkMoon Pro, internal » with a short commit SHA so that they can be audited internally; no private repository is linked.

---

## Contents

1. [Research goals](#1-research-goals)
2. [Redesign chronology](#2-redesign-chronology)
3. [Visualization architecture](#3-visualization-architecture)
4. [Before/after gallery](#4-beforeafter-gallery)
5. [Attack-path visualization research](#5-attack-path-visualization-research)
6. [Terminal interface research](#6-terminal-interface-research)
7. [Technical comparisons](#7-technical-comparisons)
8. [Benchmarks](#8-benchmarks)
9. [UX results](#9-ux-results)
10. [Limitations](#10-limitations)
11. [Bibliography](#11-bibliography)
12. [Repository map](#12-repository-map) · [Conventions](#conventions) · [Articles](#articles) · [How to cite](#how-to-cite)

---

## 1. Research goals

1. **Show exposure without overstating it.** A pentest campaign produces findings, assets and candidate attack paths of very different certainty. The interface must never present a declared or inferred path as proven, and never show two contradictory figures for the same notion on one screen.
2. **Serve two readers with one data model.** Operators need the evidence, the map and the paths; executives need a rating, a treatment plan and framework relevance. The server computes every figure; the interfaces only display it.
3. **Work everywhere the operator works:** desktop, tablet, phone (no sideways scroll at 320 px), a VR headset browser for the 3D map, and an 80-column terminal.
4. **Stay inside the brand and an accessible baseline:** state never encoded by colour alone, 44 px touch targets, keyboard paths, light and dark themes.

The research questions that remain open (no user study yet, no measured comparison of the graph families) are listed in [§10](#10-limitations).

## 2. Redesign chronology

| Period | Step | Outcome |
|---|---|---|
| Mar → Sep 2026 | Functional Angular 21 dashboard, G6/dagre infrastructure graph | Graph incoherent because of **data**, not styling (findings piled on one node) |
| 25–27 Sep | Server-side finding attachment; three.js/WebXR orbital lab → **Guided** variant ported | Coherent map; BFS « attack paths » later relabelled **exposure paths** |
| 29–31 Oct | « Eclipse » redesign, attack-path PoCs, ~95 « night hunt » design lots, fintech layers, liquid glass, full-screen 3D, invented cards | **Almost all rejected**; rule « modernise, never add blocks » |
| 1–9 Nov | Focus kit on real data only; ELK path graph; Atlas 2D map; operator-declared paths; proven-only drawing | Merged through 33 per-lot branches; many visual experiments reverted |
| 16–18 Nov | Adversarial UI review; release with agent-declared paths labelled, never proven | PR merged 18 Nov |
| 18–20 Nov | Terminal Explorer (OpenCode TUI plugin), Community and Pro | 48/48 plugin tests; shipped in both editions |
| 19–20 Nov | Executive Security V2 (Cyber Score, Prioritized Treatment Plan, frameworks), owner review passes, Space Mono retired, **Board 13** campaign header | Production 20 Nov |

Full history with commits and statuses: [angular/evolution](angular/evolution/README.md) · step-by-step [before/after chronology](angular/evolution/before-after-chronology.md) · [UX decision records](decisions/README.md).

## 3. Visualization architecture

```
Browser ──► Angular 21 shell (rail, ⌘K, theme)       Terminal ──► OpenCode TUI ──► Explorer plugin (Solid + OpenTUI)
              │ signals stores ◄── poll / SSE                            │ Pro: read-only REST API (in-memory token)
              │ HttpClient + cookie + CSRF                               │ Community: local JSON store via python3 -I
              ▼                                                          ▼
           /api/v1 (server) ── attack-path engine, exec_core, redaction ── same words, same figures
           Views: Dashboard · Projects · Campaign [Map (Atlas 2D) | Attack paths | Findings | Plan | Overview | Reports]
                  · /infra (orbital three.js/WebXR + ELK layered graph) · Frameworks · Report / PDF · Replay
```

- **Server computes, UI displays** (UXDR-14): scores, rankings, path kinds and legend words come from the server; word parity is tested.
- **Three spatial views** coexist: ELK layered path graph (`ap-graph`), Atlas 2D nested-boundary map, orbital 3D map (render on demand, WebXR when supported); the legacy G6 graph remains in the code base.
- **One proof vocabulary** across 2D map, paths rail, 3D legend, report and terminal: Evidence-Backed (solid + shield), Possible (dashed + dashed circle), Treatment in progress (dotted + wrench).

Details: [angular/architecture](angular/architecture/README.md) · [research/graph-layouts](research/graph-layouts/README.md) · [tui/architecture](tui/architecture/README.md).

## 4. Before/after gallery

All images are **real UI captures** on **synthetic demo data** (renamed copies of lab campaigns against the deliberately vulnerable DVGA/DVWA applications, plus overlays labelled « SYNTHETIC DEMO DATA »; IPs tokenised) unless marked mock-up. Versions `v1`–`v9` are defined in [screenshots/README.md](screenshots/README.md); each folder has a `metadata.json`.

| Change | Before | After | What changed |
|---|---|---|---|
| Executive Security V2 — campaign overview | [v1](screenshots/historical/pro-campaign-overview-dark-1440-v1.png) | [v9](screenshots/current/pro-campaign-overview-dark-1440-v9.png) | Cyber Score block (grade + one coverage sentence + « what holds the grade »); « 7 fixes tied » tile replaced by « 14 actions, 4 tied at the top »; capsule bar replaced by Board 13 |
| Treatment plan — per finding → per weakness class | [v1](screenshots/historical/pro-treatment-plan-dark-1440-v1.png) | [v9](screenshots/current/pro-treatment-plan-dark-1440-v9.png) | 66 per-finding « fixes » became 14 actions (asset × weakness class) with status, verification, attack paths and controls |
| Attack-paths tab — proof labelling | [v1](screenshots/historical/pro-attack-paths-dark-1440-v1.png) | [v2](screenshots/current/pro-attack-paths-dark-1440-v2.png) | « Observed » group and choke-point styling → « Possible / Inferred / Likely » groups, dashed path, line « a reading of the inventory, not a demonstrated attack » (seed also differs, see metadata) |
| Atlas 2D map | [v1](screenshots/historical/pro-infra-map-2d-dark-1440-v1.png) | [v2](screenshots/current/pro-infra-map-2d-dark-1440-v2.png) | Rail regrouped by proof class; bottom toolbar gains « Exposure path » |
| Orbital 3D map | [v1](screenshots/historical/pro-orbital-3d-dark-1440-v1.png) | [v2](screenshots/current/pro-orbital-3d-dark-1440-v2.png) | « Trace the attack path » only with a proven path; otherwise the server's honesty note and « Walk the exposure path » |
| Frameworks | [v1](screenshots/historical/pro-frameworks-light-1440-v1.png) | [v2](screenshots/current/pro-frameworks-light-1440-v2.png) | Technical assessment cards (ISO 27001, GDPR, NIS2, AI Act): four states, counts only, « not an audit » |
| Owner pass 1 — treatment-plan table | [v3](screenshots/comparisons/pro-treatment-plan-light-1440-v3.png) | [v4](screenshots/comparisons/pro-treatment-plan-light-1440-v4.png) | Table fits its container, status pills on one line, bar keeps a fixed order |
| Owner pass 1 — finding page rail | [v3](screenshots/comparisons/pro-finding-dark-1440-v3.png) | [v4](screenshots/comparisons/pro-finding-dark-1440-v4.png) | Right rail gets room, charter edges, label column |
| Owner pass 2 — Space Mono retired | [v5](screenshots/comparisons/pro-campaign-overview-dark-1440-v5.png) | [v6](screenshots/comparisons/pro-campaign-overview-dark-1440-v6.png) | Uppercase mono chips → Google Sans chips with icons; status pill |
| Board 13 — desktop | [v6](screenshots/comparisons/pro-campaign-bar-light-1440-v6.png) | [v7](screenshots/comparisons/pro-campaign-bar-light-1440-v7.png) | Capsule pills (two wrapping rows) → one header card, equal-width icon-over-label tabs |
| Board 13 — phone 390 | [v6](screenshots/comparisons/pro-campaign-bar-dark-390-v6.png) | [v7](screenshots/comparisons/pro-campaign-bar-dark-390-v7.png) | Views hidden by sideways scroll → five visible cells with counters as badges |
| Board 13 — phone 320 | [v6](screenshots/comparisons/pro-campaign-bar-light-320-v6.png) | [v7](screenshots/comparisons/pro-campaign-bar-light-320-v7.png) | Same at 320 px, no horizontal scroll |
| Polish (PR #57) — project tabs | [v6](screenshots/comparisons/pro-project-tabs-light-1440-v6.png) | [v8](screenshots/comparisons/pro-project-tabs-light-1440-v8.png) | Bordered pill group → bare tabs with indicator |
| Terminal: Community vs Pro (same campaign) | [Community](screenshots/current/community-tui-paths-160x45-v1.png) | [Pro](screenshots/current/pro-tui-paths-120x40-v1.png) | Community never proves a path (0 evidence-backed); Pro shows the 2 server-backed paths |

Current state, other views: [findings](screenshots/current/pro-campaign-findings-light-1440-v9.png) · [finding](screenshots/current/pro-finding-dark-1440-v9.png) · [project overview](screenshots/current/pro-project-overview-light-1440-v9.png) · [plan drawer](screenshots/current/pro-plan-drawer-dark-1440-v2.png) · [legend](screenshots/current/pro-attack-paths-legend-dark-1440-v2.png) · [TUI workspace](screenshots/current/community-tui-workspace-120x40-v1.png) · [TUI legend 80×24](screenshots/current/community-tui-legend-80x24-v1.png).
Design study (**mock-ups**, not the product): [20 campaign-bar boards](experiments/alternatives/campaign-bar-boards/README.md).

## 5. Attack-path visualization research

- **Proof classes are visual classes.** Server provenance (observed, derived, declared, inferred, agent-declared) is collapsed into **Evidence-Backed** (observed, or derived only when every claim is checked) versus **Possible**; operator-declared paths are a separate class that is never counted (UXDR-11, UXDR-12, UXDR-13).
- **Exposure is not attack.** Without a proven path the 3D map offers an « exposure path » with a fixed honesty line; the legacy BFS « attack » mode was renamed (UXDR-05).
- **No ranking without proof.** Path-based prioritisation is refused with fewer than two proven paths sharing a step; the UI says so instead of showing an empty ranking.
- **Layout families** studied: G6/dagre tree, ELK layered graph (entry left → critical asset right, proven solid / hypothesis dashed, choke points double-ringed), Atlas nested boundaries with semantic zoom, three.js orbital rings by exposure zone, terminal vertical node-card chains.

Details: [research/data-visualization](research/data-visualization/README.md) · [research/graph-layouts](research/graph-layouts/README.md). Engine-side research (evidence, causality, path engine): [sister repository](https://github.com/ASCIT31/darkmoon-attack-paths-research).

## 6. Terminal interface research

The Terminal Explorer turns the OpenCode console into a read-only cockpit: projects, campaigns, findings, reports, plan, frameworks and attack paths, **with zero model calls while browsing** (verified by message counts before/after journeys). It required resyncing the OpenCode fork from 1.3.0 (no TUI plugin API) to 1.19.6. Layout adapts at 80 / 100 / 150 columns; ASCII fallback; terminal-escape sanitisation; Community adapter fails closed on redaction. Inspirations (Lazygit, K9s, btop, Posting) were design ideas only — **no code reused** (verified by imports).

Details: [tui/architecture](tui/architecture/README.md) · [keyboard navigation](tui/keyboard-navigation/README.md) · [rendering](tui/rendering/README.md) · [iterations](tui/iterations/README.md) · [research/terminal-interfaces](research/terminal-interfaces/README.md).

## 7. Technical comparisons

| Concern | Options compared | Retained | Why |
|---|---|---|---|
| Infra map renderer | G6/dagre restyle vs fix the data first | Fix the data (server attachment) | 38/40 findings had no node id on one audited campaign: no renderer fixes that |
| Path graph | Quarn-style flow graph (pixel-matched) vs ELK layered | ELK layered | Flow graph rejected as « no motion, no depth »; ELK gives deterministic left→right reading |
| 3D | Full-screen 3D dashboard vs 3D as accent | Accent only (infra map / path focus) | « That's not a dashboard » |
| Design system | Invented components, skins, M3 Expressive, commercial kit | Measured copy of the Focus kit (sha256-locked tokens) | Invented variants rejected; measured copies accepted |
| Campaign bar | 20 boards; study ranked 02 > 01 > 07 | **Board 13** (owner choice) | Equal-width cells, all views visible on phones, one hairline |
| Terminal UI | Official plugin API / patch fork / separate binary / custom commands | Official plugin API after fork resync | Binary leaves the console; commands cost an LLM turn per step |
| Community data access | API vs local files | Local JSON via sandboxed `python3 -I` | No daemon, no network, no model call; fail-closed redaction |

## 8. Benchmarks

Only measures found in build logs, test logs, commit bodies, reports or code are reported ([benchmarks/README.md](benchmarks/README.md)). Highlights:

- Production initial bundle **1.23–1.26 MB raw / ~255–264 kB transfer** (three build logs); lazy routes reported « 2.32 MB → 643 kB » (commit message only).
- Dashboard **CLS 0.1065 → fixed** by reserving height (Playwright layout-shift observer).
- Angular unit tests **718 → 835 → 844**; Terminal Explorer plugin tests **15 → 48**.
- Terminal Explorer: **+44 MB RSS**, **0 model calls** while browsing, ~1.5–1.8 s per Community executive fetch.

**Not measured** (qualitative only): Lighthouse on the Angular client, frame rate / GPU of the orbital map, ELK layout time, TUI render time, any user study (task time, error rate, SUS, comprehension of proof labels), bundle-size trend in CI.

## 9. UX results

Qualitative unless stated; sources are owner reviews and AI adversarial critics, **not users**.

- The decisive constraints were **epistemic**, not aesthetic: contradictory figures (« 0 proven » vs « 5 proven » in the PDF; « 7 fixes tied » vs 67 actions) were treated as defects.
- **Measured reproduction beat invention**: every invented component variant was rejected; measured copies of the reference kit were accepted.
- **Progressive disclosure** settled into one main question per view, details on click, collapsed regulatory line, first 10 actions then « Show all ».
- **Phones**: the campaign bar no longer hides views behind sideways scroll (Board 13); a 320 px reflow check was added after the frameworks page overflowed (1032 px at 390).
- **Terminal**: an 80-column layout, a shared legend and identical words to the dashboard.
- Rejections are first-class results: see [experiments/failures](experiments/failures/README.md).

## 10. Limitations

- **No user study.** Comprehension of « Evidence-Backed vs Possible » is a hypothesis.
- **No measured comparison** of ELK vs Atlas vs orbital for the same task; no frame-rate or layout-time measurement.
- **Single-owner design authority**: decisions reflect one product owner plus AI critics; owner quotes are translated from French.
- **Demo data**: all captures use one synthetic demo dataset; v1 used an earlier seed, so v1→v2 differences are partly data differences.
- **Board 13** was implemented under time pressure; the post-deploy owner review is not recorded.
- Known inconsistencies: orbital-map severity colours differ from app tokens; corroborated `derived` paths can be counted differently by the legend and the rail (0 on demo data).
- Pro source is private: Pro claims can only be audited internally from the cited SHAs.

## 11. Bibliography

Full list with verification status: [references/README.md](references/README.md). Core works: Shneiderman 1996 (visual information-seeking mantra); Munzner 2014; Nielsen 2006 (progressive disclosure); Furnas 1986 (fisheye views, focus+context); Sugiyama, Tagawa & Toda 1981 and the Eclipse Layout Kernel (layered graph drawing); Material Design 3; Apple Human Interface Guidelines; WAI-ARIA APG; WCAG 2.2; Lazygit, K9s, btop, Posting.

## 12. Repository map

```
research/        data-visualization · graph-layouts · human-computer-interaction · terminal-interfaces · accessibility
angular/         architecture · evolution (history + before/after chronology) · benchmarks
tui/             architecture · iterations · keyboard-navigation · rendering
experiments/     successes · failures · alternatives (campaign-bar study + 20 mock-up boards)
screenshots/     historical · current · comparisons (each with metadata.json)
benchmarks/      measures that exist + explicit « not measured » list
decisions/       UX decision records UXDR-01 … UXDR-21
references/      bibliography and internal-source identifiers
```

## Conventions

**Status labels**: VERIFIED (checked in a primary artifact: git object, code, report, build log), OBSERVED (stated in a report or conversation, not re-checked), SIMULATED (synthetic data or deterministic harness), HYPOTHESIS, SUPERSEDED, UNKNOWN.

**Internal source identifiers** (sources that are not published; cited so that statements stay auditable inside ASC-IT):

| Identifier | Meaning |
|---|---|
| `IR-nn` | Internal engineering or review report number *nn* (e.g. IR-12 adversarial UI review, 2026-11-16; IR-63–IR-66 Terminal Explorer; IR-67 Executive Security V2) |
| `IW`, `IW-audit` | Internal design workspace (Sep–Oct 2026); `IW-audit` = infrastructure-map audit, 2026-09-25 |
| `FT` | Internal forensic timeline of the research programme |
| `CE` | Internal conversation evidence (owner sessions) |
| owner notes | Product-owner review notes (secondary source, used for the owner's words) |

**Commits**: short SHAs. Unless marked Community, they belong to DarkMoon Pro (internal, private). Community commits refer to the public [`ASCIT31/Dark-Moon`](https://github.com/ASCIT31/Dark-Moon).

**Data**: no client data appears anywhere. Captures use synthetic demo data; IP addresses are tokenised (`IP_PRIVATE_001`, `URL_001`); credential values are redacted by the product (`«redacted:…»`). The visible default credentials of the DVGA training application are public documentation of that application.

## Articles

Coming soon.

## How to cite

See [CITATION.cff](CITATION.cff). Suggested text: *ASC (ASC-IT) — DarkMoon team (2026). DarkMoon Visualization Research (version 1.0.0). https://github.com/ASCIT31/darkmoon-visualization-research*

Contributions: [CONTRIBUTING.md](CONTRIBUTING.md) · Changes: [CHANGELOG.md](CHANGELOG.md).
