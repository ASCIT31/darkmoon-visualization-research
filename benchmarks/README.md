# Benchmarks — only measures that exist

Rule applied: a figure appears here only if it is found in a build/test log, a commit body/subject, a report, or code. Each row gives its source and label. Where no measurement exists, this is stated and the topic is **qualitative only**. Nothing was built or re-run while assembling this repository (no Docker, no builds); conventions in [UI reform history](../angular/evolution/README.md).

Conventions (status labels VERIFIED / OBSERVED / SIMULATED / HYPOTHESIS / SUPERSEDED / UNKNOWN, internal source identifiers `IR-nn`, `IW`, `FT`, `CE`, owner notes) are defined in the [repository README](../README.md#conventions). Commits are short SHAs of **DarkMoon Pro (internal, private repository)** unless marked Community (public `ASCIT31/Dark-Moon`).

## 1. Bundle size (Angular)

| Measure | Value | Source | Label |
|---|---|---|---|
| Initial JS before/after lazy routes | 2.32 MB → 643 kB | commit subject `8c73481d` (2026-10-29) | OBSERVED (no build log found; build configuration not stated) |
| Production build, initial total (raw / estimated transfer) | 1.23 MB / 255.02 kB | `IR-67` | VERIFIED (log) |
| Production build, initial total | 1.26 MB / 263.85 kB | `…/build-pro-788cece5.log:604` | VERIFIED |
| Production build, initial total | 1.26 MB / 263.79 kB | `…/build-pro-e43fba3a.log:480` | VERIFIED |
| Production build "initial total" in exec-v2 report | 1.25 MB | `IR-67` §5 | OBSERVED |
| Style budget warning | `map-rail.scss` 72.36 kB > 48 kB warning (by 24.36 kB) | `build-pro-b15d9b13.log:630` (same in the two other logs) | VERIFIED |
| Style budget history | 20/40 kB → 48/96 kB (map-rail 69 kB, report-rail 46 kB broke prod build) → error budget 72 kB for map-rail (`8b24184b`); 128 kB test-only (`d8c1f827`) | commit subjects | VERIFIED (subjects) |
| Dev/test (unoptimised) builds | 5.01 MB (`IR-53`), 5.99 MB (`IR-67`) | logs | VERIFIED — **not comparable** with production figures |
| Eclipse kit vendored bundle | 837 KB vs 1.04 MB (purged) | commit `f0904eb8` | OBSERVED |

Bundle size is **not tracked in CI** (no size step in `.github/workflows/`). VERIFIED.

## 2. Test counts

| Suite | Value | Source | Label |
|---|---|---|---|
| Angular spec files on `origin/dev` | 116 | code | VERIFIED |
| Angular unit, Arch-D closure | 99 files / 718 tests passed | `IR-53` | VERIFIED |
| Angular unit, exec-v2 pass A | 788 / 110 files (739 before) | `IR-67` §5 | OBSERVED |
| Angular unit, exec-v2 pass B | 835 / 115 files | `IR-67` | VERIFIED |
| Angular unit at release decision | 844/844 | `IR-67` | OBSERVED |
| Earlier milestones | 183/183 (focus-skin baseline), 246/246 (LOT 2), 412 + 23/23 e2e (Eclipse attack paths), 665 (AP V1 on Focus kit), 668 (e2e integration) | owner notes / CE | OBSERVED |
| IR-12 XSS-related specs | 12 files, 116/116 | `IR-12.md` | VERIFIED (report) |
| Playwright e2e (read-only demo stack) | functional 16/24, integration 3/15, keyboard 9/16 — identical to the pre-change build on the same data | `IR-67` §8.2 | OBSERVED; e2e not in CI |
| Terminal Explorer bun | 15 → 23 → 28 → 40 → 43 (224 expects) → 48 | TUI reports; `17f9f506` | VERIFIED (reports) |
| Terminal Explorer Python | Pro 7–8; Community 29 → 32 | TUI reports | VERIFIED (reports) |

## 3. Graph and data sizes

| Measure | Value | Source | Label |
|---|---|---|---|
| Orbital graph component size | 410 lines (2026-11-05) → 805 lines (`origin/dev`) | code | VERIFIED |
| ap-graph / atlas-canvas / pdf-export / frameworks | 186 / 442 / 383 / 515 lines | code | VERIFIED |
| Infra-map audit (one production campaign) | 17 nodes, 3 roots, 38/40 findings without `node_id` | `IW-audit` §3 | OBSERVED |
| Server attachment test (one lab campaign) | 57/57 findings attached, 0 duplicates; 6 assets lit, 5 BFS paths | owner notes | OBSERVED |
| Final demo capture | 68 findings, 5 assets, 2 evidence-backed + 17 possible paths, 14 plan actions | `screenshots/current/pro-campaign-overview-dark-1440-v9.png` | VERIFIED (capture), SIMULATED (synthetic copy of a lab campaign) |
| TUI large fixture | 1,200 findings → 24 pages of 50, only visible window rendered | `IR-64` | VERIFIED (report); SIMULATED data |
| UI code size (exec-v2 audit) | ~30.6 k lines TS/HTML/SCSS (specs excluded), 26 global stylesheets (~6.5 k lines), 99 spec files (~724 `it()`) at `92163a29` | `IR-67` header | OBSERVED |

## 4. Layout stability and runtime

| Measure | Value | Source | Label |
|---|---|---|---|
| Dashboard CLS | one shift of 0.1065 at 1440×900 after login ("Last campaigns" growing to ~784 px); observed > 0.1 26 times in a real campaign | commit body `8f6fdf4a` | VERIFIED (commit body; harness Playwright + layout-shift observer) |
| Report view CLS | 0.147 during a run (not reproduced); 0.013 on a completed campaign; infra/campaign 0.000 | same | VERIFIED (commit body) |
| Executive API timing (demo copy) | 0.03–0.2 s per campaign doc (1.8 s cold), < 0.1 s per project | `IR-67` §2 | OBSERVED |
| PDF export after lazy import | 519,852-byte file, no console error | `IR-12` | VERIFIED (report) |
| Route crawl | 33 routes × 2 themes × 2 viewports = 132 loads, 0 page errors on valid routes | `IR-12` | VERIFIED (report) |
| Orbital render loop | stops after 2,000 ms idle (design constant, not a measurement) | `render-scheduler.ts` | VERIFIED (code) |
| Board 13 hairline fix | 28 final captures pixel-identical with/without the change | `03b8fc10`; `IR-67` PR #62 | VERIFIED (commit body) |

## 5. TUI performance

| Measure | Value | Source | Label |
|---|---|---|---|
| Render time (ms) | **none recorded** | — | UNKNOWN |
| Memory | +44 MB RSS on ~700 MB baseline | `IR-64` | VERIFIED (report) |
| Image size | 353 MB, unchanged by the plugin | same | VERIFIED (report) |
| Community executive/plan/frameworks fetch | ~1.5–1.8 s each, after a 250 ms settle | `IR-67` | VERIFIED (report) |
| Network | one request per screen, 60 s cache | code + report | VERIFIED |
| Model calls while browsing | 0 (DB 148 messages before/after; 0 sessions with providers disabled) | `IR-64`; `IR-67` | VERIFIED (report) |
| Accidental model spend during capture | $0.09 | `IR-65` | VERIFIED |

## 6. Accessibility measures

Contrast ratios cited in commits (VERIFIED as commit claims, not re-measured): Fixes list 1.07 before fix (`bcc82859`); campaign-bar primary on light 1.12:1 → 3.5:1 (`1c6322f3`); frameworks text ≥ 7:1 (`ca798845`); 2 axe colour-contrast nodes fixed (`02808ed4`). Horizontal overflow: `/campaign/:id/frameworks` 1471 px at 1440 and 1032 px at 390 (`IR-12` #6, VERIFIED report).

## 7. Measures that do not exist (qualitative only)

- **Lighthouse**: no run found for the Angular client in reports, logs or commits. (Lighthouse/perf work exists for the marketing website, out of scope.) UNKNOWN.
- **Frame rate / GPU** for the orbital map or ELK layout time: none. UNKNOWN.
- **User studies** (task time, error rate, SUS, comprehension of proof labels): none. Owner reviews and AI adversarial critics only — qualitative.
- **Awwwards-style scores** "4.5/10" were internal LLM judges, not users or an external jury (OBSERVED, MEM). Not a measure of usability.
- **Bundle size trend** over time: only isolated points above.
