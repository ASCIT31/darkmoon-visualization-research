# Terminal Explorer — architecture (OpenCode TUI plugin)

Code: `conf/plugins/darkmoon-explorer/` in DarkMoon Pro (`origin/dev @ d6bfc0d9`, internal) and in the public Community repository [`ASCIT31/Dark-Moon`](https://github.com/ASCIT31/Dark-Moon/tree/master/conf/plugins/darkmoon-explorer) (`origin/master @ 10eaab0`). `index.tsx`, `core.ts` and `core.test.ts` have **identical blob hashes in both editions** (VERIFIED); only the Python adapter and its tests differ. Internal reports: IR-63 to IR-67.

Conventions (status labels VERIFIED / OBSERVED / SIMULATED / HYPOTHESIS / SUPERSEDED / UNKNOWN, internal source identifiers `IR-nn`, `IW`, `FT`, `CE`, owner notes) are defined in the [repository README](../../README.md#conventions). Commits are short SHAs of **DarkMoon Pro (internal, private repository)** unless marked Community (public `ASCIT31/Dark-Moon`).

Sections moved for navigation: [keyboard navigation](../keyboard-navigation/README.md) (§4), [rendering, themes and proof vocabulary](../rendering/README.md) (§7–9), [iterations and tests](../iterations/README.md) (§11).

## 1. Problem and requirement

Owner (session C, 2026-11-18): turn the OpenCode console into a pentest cockpit — browse projects, campaigns, findings, reports and attack paths **"without leaving the console"**, read-only, with **zero model calls** while browsing, and available in Community. OBSERVED (CE).

## 2. Why OpenCode, and the fork resync

| Step | Evidence | Status |
|---|---|---|
| OpenCode adopted as the agent runtime/TUI | Community `b349b43` (2026-01-27) | VERIFIED (timeline §9) |
| Options compared: **A** official TUI plugin after resync (chosen); **B** patch the fork's TUI (fallback); **C** separate `dm-explore` binary (rejected: fails "without leaving the console"); **D** custom commands (rejected: each step costs an LLM turn) | `IR-63` | VERIFIED (report) |
| Fork at 1.3.0 (`77b3b467`, 2026-03-23) had **no TUI plugin API**: `@opencode-ai/plugin` exported only `.` and `./tool`; routes a closed union; OpenTUI 0.1.90. First `./tui` export in 1.4.0. | `IR-63`, `TUI-ARCHITECTURE.md:5-17` | VERIFIED (report) |
| Fork fast-forwarded to **v1.19.6** (`f4e363c8`), Pro PR #51; `OPENCODE_DISABLE_AUTOUPDATE=1` so upstream updates cannot replace the Darkmoon binary. Rule: no deprecated calls, so 2.x is "a rebuild, not a rewrite". | `IR-63`; `IR-64` | VERIFIED (report) |

## 3. Plugin API actually used

- `api.route.register([{ name: "darkmoon.explorer", render }])` (`index.tsx:2678`).
- `api.keymap.registerLayer` → command `darkmoon.explorer.open`, `slashName: "explore"` (so `/explore`), palette category "Darkmoon", binding `<leader>d` (`index.tsx:2679-2695`).
- `api.kv` (username only), `api.ui.dialog` (`DialogPrompt/DialogSelect/DialogAlert`, `setSize`), `api.ui.toast`, `api.theme.mode()`, plugin options. Slots: not used.
- Export `{ id: "darkmoon-explorer", tui }` (`index.tsx:2698`); loaded through `tui.json` written by `conf/apply-settings.sh:583`.
- **OpenTUI 0.4.5** as a host peer: `@opentui/solid` (`useKeyboard`, `useTerminalDimensions`), `@opentui/keymap/solid` (`useBindings`, ~30 `dmx.*` commands), `@opentui/core` (`RGBA`, `SyntaxStyle`), `solid-js`. JSX via `/** @jsxImportSource @opentui/solid */`.

VERIFIED (code, via the TUI survey). Residual risk: API churn at OpenCode 2.x (`IR-64`).

| File | Lines |
|---|---|
| `index.tsx` (UI) | 2,698 |
| `core.ts` (model, data sources, formatting) | 1,151 |
| `core.test.ts` | 557 |
| `darkmoon_explore.py` (Pro / Community) | 420 / 547 |
| Community `_vendor/` (redaction, keyblocks, `exec_core`) | ~8,050 |

No `package.json`; no dependency added. VERIFIED.

## 5. Data adapters (Community vs Pro)

| | Pro `ExplorerApi` (`core.ts:167-277`) | Community `FileSource` (`core.ts:526-560`) |
|---|---|---|
| Transport | `fetch` to `DARKMOON_API_URL` (default local API), `POST /auth/login` → in-memory Bearer; 15 s timeout; 401/403 drops the token | `execFile python3 -I -B darkmoon_explore.py`, 20 s timeout, 64 MB buffer |
| Source | Existing GET routes only (projects, campaigns, vulnerabilities, report, attack-paths, executive, treatment-plan, framework-assessment, profile, maintenance-policy, legend); **no endpoint added** | Local JSON store of the Community install; docstring: "No daemon, no cache file, no write, no network, no model call" |
| Redaction | Server-side (`«redacted:…»`) | Vendored `evidence_redaction`; **fail closed**: if it cannot load, evidence becomes `{"withheld": true}` and the report shows "Report withheld" |
| Path proof | Server provenance | Declared links only; "Community never proves" (Community PR #59) |

Shared: `CachedSource` 60 s in-memory cache. Data-location rationale (no database; JSON files + one Markdown per report; Community `DATA_DIR` not configurable) in `IR-63` §0.1. VERIFIED.

## 6. Security and privacy

| Measure | Evidence | Status |
|---|---|---|
| Zero model calls while browsing: OpenCode DB 148 messages / 7 sessions / 641 parts before and after journeys; L8 with all providers disabled: 0 sessions | `IR-64`; `IR-67` | VERIFIED (report) |
| Password masked (`•`), token in memory only, `kv.json` holds only the username; password found 0 times in logs, scrollback, captures | `IR-64` | VERIFIED (report) |
| Terminal-escape sanitisation `clean()` (`core.ts:285-294`): 7-bit OSC/CSI, 8-bit OSC/DCS/SOS/PM/APC, C0/C1 controls, bidi overrides; tested live with `ESC[2J`, `ESC]0;`, `ESC]52;` (clipboard) | code + `TEST-RESULTS.md:35` | VERIFIED |
| Community adapter: paths confined to data root, id regex, symlinks leaving root refused | `darkmoon_explore.py:1-30` (COM) | VERIFIED |
| No per-project ACL in the Pro API: the TUI shows what the account can read | `IR-64` | OBSERVED (residual risk) |

## 10. Inspirations vs actual reuse

| Reference | Taken (design idea) | Not taken, and why | Code reused? |
|---|---|---|---|
| Lazygit | titled panes, focused-pane accent, list + inspector, `tab` | box borders around every pane (replaced by surface steps) — later partly reintroduced as single-line borders (§7) | No |
| K9s | dense table, header row, single-key filters, keys in footer | colour per column (kept to the four-step ink ramp) | No |
| btop | horizontal bars, sparkline, figures with a sub-visual | braille graphs, per-core meters (no time series beyond discovery times) | No |
| Posting | section bar, quiet panels, document-like reading pane | collapsible sidebar tree (contents rail instead) | No |

Source: `IR-65` (VERIFIED). **Verification of non-reuse**: the plugin has no `package.json`; imports are only `@opencode-ai/plugin/tui`, `@opentui/*`, `solid-js`, `node:child_process` (`index.tsx:9-14`); "No dependency added" (`IR-63`). htop, gitui, Textual: 0 mentions. Conclusion: **inspiration only, no code reuse** — VERIFIED. The report's own verdict: "not yet at Lazygit / btop level" on logo rendering and Markdown tables (README:42).

## 12. Limitations (as recorded)

No images; Markdown tables styled by the host; branching attack paths listed path by path, not merged; no per-project ACL in the Pro API; truecolor needed; with `--network none` OpenCode stays on a blank screen (`IR-66`); Pro `darkmoon_explore.py` lacks plan/frameworks/profile/executive/legend (Pro uses the API instead); no render-time measurement (see [benchmarks](../../benchmarks/README.md)).
