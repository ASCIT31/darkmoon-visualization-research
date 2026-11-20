# Angular architecture — DarkMoon Pro client (`client-app/`)

Read from code with `git show origin/dev:…` (no checkout, no build). Reference: **Pro `origin/dev` @ `d6bfc0d9`** (2026-11-20, merge of PR #62), which is the production branch. All statements are **VERIFIED** from code unless labelled otherwise.

Conventions (status labels VERIFIED / OBSERVED / SIMULATED / HYPOTHESIS / SUPERSEDED / UNKNOWN, internal source identifiers `IR-nn`, `IW`, `FT`, `CE`, owner notes) are defined in the [repository README](../../README.md#conventions). Commits are short SHAs of **DarkMoon Pro (internal, private repository)** unless marked Community (public `ASCIT31/Dark-Moon`).

> Caution for reproducers: the local `dev` ref in the working clone was stale (`87d37f22`, 2026-11-05, 1,173 commits behind `origin/dev`) and contains none of the attack-path, Atlas, frameworks, executive or header-tabs code described below.

## 1. History

| Item | Value |
|---|---|
| First commit | `14a52fc4` 2026-03-31 "feat(front-app): initialize Angular client application" |
| Last commit (snapshot) | `03b8fc10` 2026-11-20 "fix(client): theme catch-all rules no longer erase charter borders" |
| Commits touching `client-app` on `origin/dev` | 829 |
| Monorepo | Backend (`mcp/`, FastAPI mounted at `/api/v1` + FastMCP) and frontend in the same repository |

## 2. Stack

| Concern | Choice |
|---|---|
| Framework | **Angular ^21.0.0**, standalone components, `@angular/build:application`; TypeScript ~5.9.2; RxJS ~7.8; pnpm 10.32.1 |
| Styling | **Tailwind CSS v4.2** (`@tailwindcss/postcss`) + 28 global stylesheets (see §6); no Angular Material |
| Icons | `@ng-icons/core` + `@ng-icons/lucide`; Phosphor-as-Lucide shim `_common/icons/phosphor-as-lucide.ts` |
| 3D | `three` ^0.186.1 (orbital graph only; OrbitControls; `renderer.xr.enabled`) |
| 2D graphs | `@antv/g6` ^5.1.0 (legacy `infra-graph` only); **ELK vendored** in `public/vendor/elk/elk.bundled.js`, loaded on demand by `_common/components/ap-graph/elk.ts` (not an npm dependency); no d3 |
| Documents | `marked` ^18, `mermaid` ^11.14 (`securityLevel:'strict'`), `pdfmake` ^0.3.7 |
| Declared but not imported anywhere in `src/` | `chart.js`, `jspdf`, `html2canvas`, `angular-oauth2-oidc` (whether they reach the bundle: UNKNOWN, nothing built) |
| Fonts | Google Sans Flex + Google Sans Code (`src/styles/fonts/*.woff2`), Poppins; Space Mono only in the PDF report |
| Tests | Vitest ^4 via `@angular/build:unit-test` (jsdom; `@vitest/browser-playwright` available); Playwright ^1.65 for e2e |

Build budgets (`angular.json:78-80` on `origin/dev`): initial 3 MB warning / 5 MB error; `anyComponentStyle` 48 kB / 96 kB (was 20/40 kB; raised in `8cebd509`, 2026-11-08, because `map-rail.scss` 69 kB and `report-rail` 46 kB broke the prod build). `inlineCritical: false`, font optimisation off.

## 3. Routing (`src/app/app.routes.ts`, `_common/sitemap.ts`)

```
/login                         public
'' (authGuard)
├── change-password            eager
├── ''           Dashboard     eager
├── history                    eager
├── projects                   eager
├── project/:id                eager   (?tab=infrastructure|overview)
├── project/:id/profile        lazy
├── project/:id/maintenance    lazy
├── campaign/:id               eager   (?tab=map|paths|findings|fixes|overview, &action=, &path=)
├── campaign/:id/frameworks    lazy
├── campaign/:id/replay        lazy
├── new-campaign               lazy
├── scheduler                  lazy
├── report/:id                 lazy
├── vulnerability/:id          lazy
├── infra/:id                  lazy   (3D orbital + ELK ap-graph)
├── pull-request/:id           lazy
└── **                         NotFound
```

- `authGuard` (`_common/guards/auth.guard.ts`) redirects to `/login` and forces `/change-password` when `mustChangePassword` is set.
- Lazy loading (`8c73481d`, 2026-10-29) keeps three.js, marked/mermaid/pdf-export, replay and frameworks out of the initial bundle (comment in routes file). Commit message: "initial JS 2.32 MB → 643 kB" (see [benchmarks](../../benchmarks/README.md); not reproduced from a log).
- Providers (`app.config.ts`): `HttpClient` with `authInterceptor`, `provideAppInitializer(() => AuthService.init())`, ng-icons. No routing modules, no other guards.

## 4. Component inventory

File naming drops the `.component` suffix (e.g. `dashboard/dashboard.ts`). 70 non-spec files carry `@Component` (73 decorators); 33 `@Injectable`; **116 `*.spec.ts`**.

| Area | Main files |
|---|---|
| Shell | `app.ts`/`app.html` (rail), `_common/views-nav.ts` (campaign views), `_common/components/command-palette/` (⌘K), `shortcuts/`, `services/rail-compact/` |
| Dashboard | `dashboard/dashboard.ts`, `widgets.ts`, `selection-bar.ts`, `zip.ts` (bulk report export) |
| Projects | `projects/`, `project/project.ts`, `score-strip.ts`, `sunburst/`, `evolution/`, `profile/` (editor + card), `maintenance/`, `target/target-tiles.ts`, `target-ports.ts` |
| Campaign | `campaign/campaign.ts` (36 KB), **`campaign-bar.ts`** (160 l., `app-campaign-bar`; l.17 "board 13 « Header card with fixed, equal-width tabs »"), `campaign-header.ts`, `verdict.ts`, `declared-paths.ts`, `findings-table.ts`, `filters.ts`, `plan/treatment-plan.ts`, `correction/`, `overview/overview-widgets.ts`, `overview-logic.ts`, `zone-map-logic.ts` |
| Header tabs | No component: global stylesheet `_common/components/header-tabs/header-tabs.scss` (listed in `angular.json` styles), classes `.hdt*` |
| Run / history | `new-campaign/`, `scheduler/`, `history/`, `replay/replay.ts`, `replay/agent-trace.ts` |
| Findings | `vulnerability/vulnerability.ts` (32 KB), `retest/retest-panel.ts`, `finding-markdown.ts`, `shell-highlight.ts`; `_common/components/proof-wells/`, `proof-level/`, `privacy-flags/`; `pull-request/` |
| Attack paths | `infra/attack-path-overlay.ts` (106 l.), `infra/declare-path/`, `infra/rail/map-rail.ts` (46 KB), `path-detail.ts`, `rail-model.ts`; models `attack-paths.ts`, `operator-paths.ts` |
| Graphs | `_common/components/ap-graph/` (ELK, 186 l.), `_common/components/orbital-graph/` (805 l. + `orbital-graph.data.ts`, `render-scheduler.ts`, `asset-insights.ts`), `campaign/map/campaign-map.ts`, `campaign/map/atlas/atlas-canvas.ts` (442 l.), `atlas-model.ts`, `atlas-session.ts`, `infra/atlas.logic.ts`, legacy `_common/components/infra-graph/` (G6) |
| Report | `report/report.ts`, `report-doc.ts`, `report-rail.ts`, `plan-cut.ts`; `_common/services/pdf-export/pdf-export.ts` (383 l.) + `pdf-model.ts`, `pdf-parts.ts`, `pdf-markdown.ts`, `report-figures.ts`; brand assets `public/report-brand/` |
| Frameworks | `frameworks/frameworks.ts` (515 l.), `frameworks/assessment/fw-assessment.ts`, `_common/utils/frameworks.logic.ts`, `frameworks.matrix.ts` — tabs ISO 27001, GDPR, NIS2, AI Act (when sent by server), ATT&CK, EBIOS RM, ATLAS, IEC 62443 |
| Executive | `_common/components/exec-block/` (`exec-block.ts`, `exec-grade.ts`, `exec-view.ts`), `_common/models/executive.ts`, `_common/legend/exec-legend.ts` |
| Admin / settings | No settings route. `change-password/`, profile and maintenance editors (admin edit, read-only otherwise), `credentials-modal`, `scope-editor`, `tag-input` |

## 5. Services and state

- **API layer**: `_common/services/api/*` on a base class `api-service-base.ts` (HttpClient, `/api` proxied by `src/proxy.conf.json`). Clients: auth, attack-paths, campaigns, credentials, dashboard, executive, frameworks, health, operator-paths, projects, pull-requests, retest, run, scheduler, targets, vulnerabilities.
- **Stores**: `ProjectsService`, `CampaignsService`, `TargetsService`, `VulnerabilitiesService` on top of the API clients; plus `campaign-run`, `live-refresh`, `theme`, `rail-compact`, `system`, `pdf-export`.
- **State model: Angular signals only** — ~129 `signal(` and ~477 `computed(` in non-spec code; zero `Subject`/`BehaviorSubject`; no NgRx. RxJS appears only through HttpClient.
- **Real time**: SSE via native `EventSource` (`api-run.ts openStream(runId)` → `/…/{runId}/stream`), consumed by `campaign-run.ts`; `live-refresh.ts` polls stores (15 s idle / 5 s while a run is active per `IR-67`) and refetches on tab visibility. No WebSocket.
- **Auth**: HttpOnly `dm_session` cookie; writes send `X-CSRF-Token` (double submit with `dm_csrf`); in-memory bearer only as fallback; nothing in localStorage (legacy keys purged). Earlier (Sep 2026) the app used a JWT in localStorage (OBSERVED, owner notes) — **SUPERSEDED** by the cookie/CSRF model (focus-skin LOT 2, Nov 2026, OBSERVED).
- **i18n**: none; English strings in templates (`index.html lang="en"`). VERIFIED (`IR-67` finding 1 and code).

## 6. Design-system layer

- `src/styles.scss` (454 l.) + 27 more global stylesheets in `angular.json`, including `src/focus/tokens.css` (437 l., ~406 `--fx-*` properties, "GENERATED — Focus kit tokens", sha256-locked copy from `d5a9182d`), `tokens-light-system.css`, `styles/focus-theme.css`, `apple-colours.css`, `nothing-theme.css`, `trade-republic.css`, `chatgpt-ui.css`, `chatgpt-rail.css`, `openai-tabs.css`, `diagram-tokens.css`, `a11y-fixes.css`, `coherence.css`, `no-rings.css`, `forms-login-dna.css`, `ux-fit.scss`, `header-tabs.scss`.
- **Architectural debt** (OBSERVED in `experiments/alternatives/campaign-bar-study/DESIGN-SYSTEM.md`): the campaign bar was styled by five stacked layers using `!important` and specificity chains of up to 24 `:not(#id)`; Board 13 deleted the `.cb*` rules from 11 stylesheets instead of out-ranking them (`17a74ea1`, VERIFIED commit body). A theme catch-all `* { border-color: transparent !important }` had to be scoped with `:where()` (`03b8fc10`, VERIFIED). The stylesheet names record the successive reference products used as style sources.
- Theme service (`services/theme/theme.ts`): follows `prefers-color-scheme` until the user chooses; choice in localStorage; `/infra` always dark.

## 7. Honesty labels in the UI layer

| Label | Where rendered |
|---|---|
| "observed attack path(s)" / evidence-backed | `infra/attack-path-overlay.ts:35,61,83,95`, `campaign/verdict.ts`, `infra/rail/path-detail.ts`, `_common/legend/exec-legend.ts` |
| "(hypothesis, not proven)" | `vulnerability/vulnerability.html:222` |
| Proven / Likely (not proven) / Interrupted groups | `infra/rail/map-rail.html`, `campaign/map/campaign-map.html:76`, `infra/infra.html`, `campaign/correction/correction.html` |
| "Declared by operator · not proven" | `infra/declare-path/declare-path-sheet.html:8`, `campaign/declared-paths.ts:35`, `proof-level/proof-level.ts:6` |
| `declared_by_agent` | model only (`_common/models/attack-paths.ts:42,46`), shown as "Possible" |
| "Exposure path · not proven" | `orbital-graph.ts` l.103–107 |
| "Synthetic lab · deterministic simulated provider" | demo campaign labelling (IR-12, VERIFIED in report) |

Word parity with the server is enforced by tests `mcp/tests/test_exec_front_word_parity.py` and `test_exec_legend_front_parity.py`, and fixtures are regenerated from the real API (`client-app/scripts/regen-exec-fixtures.py`, `test_front_fixtures_current.py`). OBSERVED (IR-67 §8.1); file existence not re-checked.

## 8. Security-relevant rendering

- Report Markdown: `new Marked({gfm:true})` with custom renderers (Mermaid placeholders with nonce, code escaped, images inert), then `neutraliseMedia()` strips fetching elements/attributes, then `[innerHTML]` (Angular sanitizer). No DOMPurify, no `bypassSecurityTrust`. VERIFIED (`IR-63` §0.3, file:line on an earlier `origin/dev`).
- XSS specs: `stream-viewer.xss`, `report.xss`, `orbital-graph.xss`, `finding-markdown` … 116/116 passing in IR-12. VERIFIED (report).
- ap-graph renders via Angular templates, no `innerHTML` (header comment). VERIFIED.

## 9. Tests

| Suite | Count (latest evidence) | Source | Status |
|---|---|---|---|
| Unit spec files | 116 on `origin/dev` | code | VERIFIED |
| Unit tests | 835 passed / 115 files (exec-v2 pass B); 844/844 at release decision | `IR-67` §8.2; `IR-67`; build log `IR-67` | VERIFIED (log) for 835 |
| e2e (Playwright via `node:test`) | `functional` 21, `integration` 15, `keyboard` 15, `write` 18 test cases (grep count); CI runs none of them; `npm run e2e` runs `functional` only | code | VERIFIED (approximate counts) |
| CI | `.github/workflows/tests.yml` job "ng test (dashboard)" (`pnpm exec ng test --no-watch`); no bundle-size, Lighthouse or Playwright step | code | VERIFIED |

## 10. Architecture diagram (textual)

```
Browser ──► Angular shell (rail, ⌘K, theme)
              │ signals stores ◄── live-refresh (poll) / EventSource SSE (runs)
              │ API clients (HttpClient + authInterceptor: cookie + CSRF)
              ▼
           /api/v1  FastAPI (mcp/api)  ── reads JSON store (DATA_DIR)
                                       ── attack-path engine, exec_core, redaction
           Views: Dashboard · Projects · Campaign[Map(Atlas 2D) | Paths | Findings | Plan | Overview | Reports]
                  · /infra (orbital three.js/WebXR + ELK ap-graph) · Frameworks · Report/PDF · Replay
```
