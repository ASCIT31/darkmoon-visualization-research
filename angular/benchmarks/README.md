# Angular client — benchmarks and reference products

Conventions: see the [repository README](../../README.md#conventions). All numeric measures and the explicit « not measured » list are in [benchmarks/](../../benchmarks/README.md); this page summarises the Angular-specific ones and the **design benchmarks** (reference products studied).

## Measures (Angular)

| Measure | Value | Label |
|---|---|---|
| Production initial bundle (raw / estimated transfer) | 1.23–1.26 MB / 255–264 kB (three production build logs, Nov 2026) | VERIFIED |
| Lazy routes | « initial JS 2.32 MB → 643 kB » (`8c73481d`, commit subject only) | OBSERVED |
| Component style budget | 20/40 kB → 48/96 kB; `map-rail.scss` 72.36 kB still over the 48 kB warning | VERIFIED |
| Dashboard CLS | one shift of 0.1065 at 1440×900 → fixed by reserved height (`8f6fdf4a`) | VERIFIED (commit body) |
| Unit tests | 718 → 788 → 835 → 844 | VERIFIED / OBSERVED |
| Route crawl | 33 routes × 2 themes × 2 viewports = 132 loads, 0 page errors on valid routes (IR-12) | VERIFIED (report) |
| Bundle size in CI | not tracked | VERIFIED |

## Design benchmarks (reference products)

| When | References | Used for | Outcome |
|---|---|---|---|
| Sep 2026 | Grafana, Splunk, JetBrains dashboards | integrations / infra map context | context only |
| 29–30 Oct | ~60 award-winning web projects; one agency style as « inspiration no. 1 » | « innovative dashboard » direction | refused |
| 30 Oct | ~15 fintech-style references | style layers | rejected as « too banking » |
| 30–31 Oct | ~95 design lots each citing a « site of the day » | night-hunt design lab | « no look kept » |
| 1 Nov | public design-system material of a consumer fintech app (26 sources) | typography and journey principles | adopted in the Focus kit |
| 4 Nov | 30-tool survey; prototypes Atlas / Signal / Story | map and frameworks views | Atlas kept |
| 20 Nov | GitHub Primer, Linear, Vercel, Stripe, Sentry, Snyk, Grafana, Material 3, Apple HIG, WAI-ARIA APG | campaign bar | 20 boards → Board 13 |

Source of the table: [UI reform history §4](../evolution/README.md#4-dashboard-benchmarks).
