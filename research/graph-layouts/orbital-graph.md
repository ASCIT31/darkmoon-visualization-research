# Orbital graph research — three.js / WebXR infrastructure map

Code references are on DarkMoon Pro `origin/dev @ d6bfc0d9` (internal) unless stated.

Conventions (status labels VERIFIED / OBSERVED / SIMULATED / HYPOTHESIS / SUPERSEDED / UNKNOWN, internal source identifiers `IR-nn`, `IW`, `FT`, `CE`, owner notes) are defined in the [repository README](../../README.md#conventions). Commits are short SHAs of **DarkMoon Pro (internal, private repository)** unless marked Community (public `ASCIT31/Dark-Moon`).

## 1. Research question

How should a pentest campaign's infrastructure (assets, services, findings, attack paths) be shown so that (a) an operator can locate exposure at a glance, (b) a path is never shown as more certain than the evidence allows, (c) the same view works on PC, tablet and a VR headset browser, and (d) it stays within the Darkmoon charter?

## 2. Starting point and problem (2026-09-25)

| Item | Finding | Status |
|---|---|---|
| Renderer | `@antv/g6` v5, `antv-dagre` top-down tree, 2D canvas (`infra-graph.ts`). | VERIFIED (`IW-audit` §2; component still in `origin/dev`) |
| Visual quality | Design language already matched the marketing mock (same severity palette, 60×60 rects, orth edges, grid); "real component looks worse **because of the DATA, not the styling**". | VERIFIED (audit) |
| Root cause A | Client-side host matching collapsed all findings on the root node (38/40 findings without `node_id` on one production campaign). | OBSERVED (audit measurement) |
| Root cause B | Edges only from `parent_node_id`; three id schemes; duplicated subtrees, three roots. | VERIFIED (audit, file:line) |
| VR | A 2D canvas opens in a headset browser but is not stereoscopic; true immersion needs a WebXR/three.js path. | VERIFIED (audit §5, reasoning) |

**Key lesson**: the first fix was **server-side data coherence**, not a new renderer. Changing the visualisation would not have fixed findings piling on one node.

## 3. Prototype phase (2026-09-25/26)

| Artifact | Content | Status |
|---|---|---|
| `IW` (408 lines, 78 KB) | three.js 0.160 lab with three UX variants in one page: **A Minimal, B Guided, C Spatial** (`role="tablist"` switcher, lines 114–117, 323–376). Embeds one lab campaign as inline JSON. | VERIFIED |
| `IW` | Seven static visual variants of infra map and attack-path view. | VERIFIED (files); which variant letter maps to which concept: UNKNOWN beyond file names |
| `IW` | Iteration captures of the orbital map (desktop, flat mode, OSI decomposition on tilt, attack overlay, VR preview, final). | VERIFIED (files, dated 2026-09-26); content described from file names only |
| `IW` (480 l.), `IW` (292 l.) | Earlier graph labs. | VERIFIED (files) |

Design ideas tested in the lab (OBSERVED, owner notes; lab code VERIFIED for variants): concentric **exposure rings** with graduations; **OSI decomposition** when tilting; MITRE-annotated paths; WebXR without vendor SDKs.

**Privacy flag**: the lab HTML and its captures embed a Juice Shop lab campaign whose node labels are a Docker-bridge private address. Lab data, not client data, but the address must be redacted before any publication.

## 4. Product integration (2026-09-26 → 27)

| Ref | Change | Status |
|---|---|---|
| Pro `0f143981` (2026-09-26) | "feat(infra): orbital WebXR infrastructure graph + deterministic finding attachment and attack paths". Component `app-orbital-graph`, `three` ^0.186. | VERIFIED |
| same | Server `_enrich_infrastructure()`: rebuilds `node.vulnerability_ids` (endpoint hint → data-store leak → node_id → host:port → app → target), `risk_level` = max severity, `attack_paths` = BFS from the internet-facing entry to each high/critical asset. Offline test: 57/57 findings attached, 0 duplicates; 6 assets lit, 5 paths. | OBSERVED (MEM) — figures from a test on one lab campaign |
| same | Agent prompts: `dashboard_push_finding` docstring "ATTACH EVERY FINDING"; `pentest.md` step C.1 asset attachment & coverage. | OBSERVED (MEM); prompt text VERIFY in COM/Pro `conf/agents/pentest.md` |
| Pro `a5e2d546` (2026-09-27) | Remediation PRs surfaced in the graph and vuln view; clarified VR message. | VERIFIED |

Owner constraints at integration (OBSERVED, MEM): keep **only the Guided variant**; everything in English; off-charter cyan of the lab removed; a rebuild "in its own way" was rejected ("that is not it at all"), so the lab engine and its bucketing were kept; VR control shows "Enter VR" only if `navigator.xr.isSessionSupported('immersive-vr')`, otherwise an explanation popover.

## 5. Honesty hardening (Oct → Nov 2026)

The largest change after integration was semantic, not visual.

| Date | Ref | Change | Status |
|---|---|---|---|
| 2026-10-29 | `009f2a8b` | Legacy "attack" mode renamed **exposure path** ("inventory, not a proven attack chain"). | VERIFIED |
| 2026-10-29 | `7c86bc05` | Full GPU/listener cleanup, render on demand, live reduced-motion. | VERIFIED |
| 2026-10-29 | `3c27a5df` | Merge: server finding attachment, honest exposure wording, a11y text view, leak fixes. | VERIFIED |
| 2026-11-03 | `77b41f56` | Draw the server's **proven** attack paths on the existing orbital map. | VERIFIED |
| 2026-11-04 | `09fb2fc0` | "orbital map without invented narrative (server zones, labels and proven path only)". | VERIFIED |
| 2026-11-06 | `79b3de87` | Intro/step cards drop Space Mono for the home-page face. | VERIFIED |
| 2026-11-06 | `a0a441a9` | Atlas chrome (tab bar, Resources tree) around the **unchanged** 3D map. | VERIFIED |
| 2026-11-19 | `f3283be0` | Shared legend vocabulary on the 3D legend (labels only). | VERIFIED |

Current code (VERIFIED, `orbital-graph.ts` on `origin/dev`, 805 lines; was 410 lines on 2026-11-05):

- Header comment (l.38–42): "Guided infrastructure map … Integration A2: nothing invented — rings and their labels [come from the server] … the guided story exists only for a server-proven path, and the scene renders on demand."
- Ring captions use server zone labels verbatim (`ZONE_LABELS` l.45–46: Internet, Edge, Application, Data, Tooling; `ringOfZone` l.48: data → 0, app → 1, others → 2).
- Intro card: with proven paths, "N proven attack path(s)" + "Trace the attack path"; without, the server's honesty note ("… This does not mean that no path exists.") + "Walk the exposure path →" (l.56–59).
- Every exposure step carries `EXPOSURE_LINE` = "Reachable assets in order of exposure — a reading of the inventory, not a demonstrated attack." and the eyebrow "Exposure path · not proven" (l.63, l.103–107), drawn dashed.
- Inputs: `provenStory`, `noPathNote`, `declaredOverlay` (operator-declared paths as "thin dashed lines, never the proven-path style", l.147–148), `declaredGaps`, `hiddenNodeIds`, fix-simulation overlay (interrupted scenarios dashed/dimmed).
- All dynamic text escaped via `esc()` (l.51–52); `orbital-graph.xss.spec.ts` exists.
- `render-scheduler.ts`: the three.js loop runs only while something moves (interaction, tween, damping, XR session) and stops after `idleMs = 2000` ms; paused when the List view hides the map. Pure class, unit-tested.

## 6. Design evaluation

| Criterion | Observation | Status |
|---|---|---|
| Ergonomics PC/tablet/VR | Owner insisted on all three (session A, 2026-09-25/26). WebXR session + controller ray picking present; requires HTTPS or localhost. | OBSERVED (CE); WebXR code VERIFIED on 2026-11-05 snapshot (line numbers not re-checked on `origin/dev`) |
| Keyboard | Click canvas then ArrowDown selects next asset and announces it in `#live`; arrows not captured outside the map or in List view; Escape clears. | VERIFIED (IR-12 "Checked and holding", Playwright scripts) |
| Accessibility | a11y text/List view fallback; legend "states, not colour alone" (`b8f91027`); light-theme label inks fixed for 2 axe contrast nodes (`02808ed4`). | VERIFIED (commits) |
| Rendering cost | Render on demand (idle = no animation loop). No frame-time or GPU measurement found. | VERIFIED (code) / UNKNOWN (numbers) |
| Charter | Severity hexes in `orbital-graph.ts` l.28 (`#ef4444`, `#f97316`, `#eab308`, `#22c55e`, …) are Tailwind-like and **differ from the app severity tokens** (`#EF1600`, `#FF9A3C`, `#F4FA6A`, `#50ADFD` per `experiments/alternatives/campaign-bar-study/DESIGN-SYSTEM.md`). | VERIFIED (both sources) — inconsistency, HYPOTHESIS that it is unintended |
| Low-end devices | Owner rule (website): degrade quality, never remove the 3D. Applies to the marketing site; no equivalent rule found for the app. | OBSERVED (owner notes) |
| "Is 3D a dashboard?" | Full-screen 3D proposed as the dashboard was rejected: 3D is an accent for the infra map / path focus only. | OBSERVED (MEM, 2026-10-30) |

## 7. Relationship with the 2D views

The orbital map is one of three spatial views on `origin/dev` (VERIFIED code): the legacy G6 `infra-graph`, the **Atlas** 2D canvas (`campaign/map/campaign-map.ts`, `campaign/map/atlas/atlas-canvas.ts`), and the **ap-graph** (ELK layered path graph, `_common/components/ap-graph/`). The `/infra/:id` page hosts both the ap-graph (`infra.html:74`) and the orbital graph (`infra.html:133`); `/infra` is always drawn on a dark scene (`theme.ts`). The website reuses the product's `orbital-graph.ts` with a guided tour after a "Cyber City 3D" attempt was reverted (OBSERVED, CE, session P 2026-10-20/21).

## 8. Limitations and open questions

- No quantitative usability study (task time, error rate) exists; all judgements are owner review + adversarial critic. UNKNOWN beyond that.
- No performance measurement (fps, GPU memory) of the orbital map found. UNKNOWN.
- Mapping of variant letters A–G in the internal design workspace to concepts is not documented. UNKNOWN.
- Severity palette divergence (§6). HYPOTHESIS: to be aligned to `--nt-*` tokens.
- Branching attack paths: the guided story tells one proven path; behaviour with many proven paths depends on `provenStory` (server-selected). Not evaluated on real data with ≥ 2 proven paths. UNKNOWN.
