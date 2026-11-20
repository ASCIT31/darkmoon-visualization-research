# Screenshots

Curated, privacy-checked captures: **46 real captures** (36 Angular, 10 terminal) across `current/`, `historical/`, `comparisons/` — plus 20 **mock-up** boards kept separately in [`experiments/alternatives/campaign-bar-boards/`](../experiments/alternatives/campaign-bar-boards/README.md). Every folder has a `metadata.json` (product, repository, commit, version, date, data source, viewport, theme, description, status `real` | `mock-up`, crop and privacy notes).

- **Data**: synthetic demo data — renamed copies of lab campaigns against the deliberately vulnerable DVGA and DVWA applications, with overlays labelled « SYNTHETIC DEMO DATA » (application profile, treatment statuses, one retest). IPs are tokenised (`IP_PRIVATE_001`, `URL_001`); secrets are redacted by the product. No client data.
- **Capture**: Playwright full-page screenshots of the real Angular UI against a local read-only API (non-GET calls aborted); terminal captures are ANSI dumps of the real OpenCode TUI rendered to PNG.
- **Compression**: 256-colour palette PNG (Pillow median-cut, no dither). Some tall pages were cropped to the part that was reviewed; crops are noted in `metadata.json`.
- **Naming**: `product-feature-theme-viewport-version.png` (terminal: `edition-tui-feature-COLSxROWS-version.png`).

## Versions

| Version | DarkMoon Pro commit (internal) | Date | State | Commit attribution |
|---|---|---|---|---|
| v1 | `b15d9b13` | 2026-11-20 | before Executive Security V2 (earlier demo seed) | VERIFIED (capture metadata) |
| v2 | `0973ee99` | 2026-11-20 | Executive Security V2 after cross-review | VERIFIED (capture metadata) |
| v3 | `e555d33d` | 2026-11-20 | before owner review pass 1 | INFERRED (capture timestamps) |
| v4 | `9fec5e75` | 2026-11-20 | after owner pass 1 (pill edges, plan table fits, finding rail, grade chip) | INFERRED |
| v5 | `98e6dd61` | 2026-11-20 | before owner pass 2 | INFERRED |
| v6 | `c33803d1` | 2026-11-20 | after pass 2 (Space Mono retired `d2af641b`, plan status pills); capsule bar | INFERRED |
| v7 | `17a74ea1` | 2026-11-20 | Board 13 header card | INFERRED |
| v8 | `42453ec3` | 2026-11-20 | UI polish (PR #57) | INFERRED |
| v9 | `03b8fc10` (merge `d6bfc0d9`) | 2026-11-20 | final production state | VERIFIED (release record) |
| TUI v1 | before `17f9f506` | 2026-11-20 | Terminal Explorer, executive views (lot L8), before the 80-column tab change | INFERRED |

## Folders

- [`current/`](current/) — latest state of each view (v9 where captured, otherwise v2) and the terminal.
- [`historical/`](historical/) — v1 baseline used in the before/after pairs.
- [`comparisons/`](comparisons/) — intermediate pairs (owner passes, Board 13, polish).

The before/after table is in the [repository README](../README.md#4-beforeafter-gallery).

## Not published, by rule

Commercial UI-kit captures (private licence); orbital-lab and Focus-kit captures (private lab addresses); material-v3 and Eclipse-redesign captures (unreviewed local data); dashboard/home captures and the not-rated DVWA terminal captures (a Docker-bridge private address is visible); raw `.ans`/`.html` terminal dumps (PNG only).
