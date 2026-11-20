# Terminal Explorer — rendering, themes and proof vocabulary

Part of the Terminal Explorer research (DarkMoon Community and Pro, OpenCode TUI plugin, public code at [`ASCIT31/Dark-Moon` → `conf/plugins/darkmoon-explorer/`](https://github.com/ASCIT31/Dark-Moon/tree/master/conf/plugins/darkmoon-explorer)). Conventions: see the [repository README](../../README.md#conventions). Overview: [architecture](../architecture/README.md).

## 7. Rendering

- **Markdown**: OpenTUI `<markdown>` with a `SyntaxStyle` built from the palette, `tableOptions={{style:"grid"}}`, `conceal` (`index.tsx:1996`, `:2652-2668`). Limit: tables and block quotes are drawn by the host component and "cannot be restyled from a plugin beyond colours" (`IR-65`). Mermaid shown as code (recommendation in `IR-63` §0.3). VERIFIED.
- **Unicode graphics** (`core.ts:753-768`): `■ █ ─ ▌ ● ○ │ ┆ ▼ ·`, sparkline `▁▂▃▄▅▆▇█` (finding discovery over the campaign window), horizontal bars (agents, severity), node cards `╭╮╰╯─│┬`; chain arrows `─▶` (evidence-backed) and `╌▶` (hypothesis) (`core.ts:451`).
- **ASCII fallback**: `DARKMOON_ASCII=1` or non-UTF-8 locale → `# = - > * o | : v .`, sparkline ` .:-=+*#`, boxes `+-|`. No ASCII-mode capture in L8 (limitation).
- **Borders ("pixel"/border polish)**: the phrase "pixel borders" does not appear in the reports; the shipped change is `edecb5a9` "single-line pane borders with inline titles" (OpenTUI `borderStyle="single"`, `┌─┐│└┘`, title in the top line; focused pane in Jordy, others `#3a3a3e` dark / `#c7c7cc` light; wide emoji replaced by a one-cell mark so borders align) and `ddfd523c` ASCII borders via `customBorderChars`. This **reverses** the V2 rule "No border is drawn between panes… no box frames" (`IR-64`), which is therefore SUPERSEDED. Captures: `IR-65` (120×40 and 80×24). VERIFIED.
- **Layouts**: wide ≥ 150 cols / split ≥ 100 / compact ≥ 80 / single (`index.tsx:173`); captured at 80×24, 120×40, 160×45.
- **No images**: the plugin API exposes no graphics protocol, so the brandmark is the text `DARKMOON`. VERIFIED (README:27).

## 8. Themes and Darkmoon charter

Dark/light follow `api.theme.mode()` (`index.tsx:170`). Palette (`core.ts:740-748`, VERIFIED): brand Alice `#dceaf4`, Jordy `#87bfff` (light `#2f6fb8`), Neon `#2667ff`, Chrysler `#3b26cc`, Royal `#0a2472`, Uranian `#add7f6`; dark ladder `#000000 / #0f0f0f / #1a1a1a / #242424`, inks `#ffffff / #d1d1d6 / #a1a1a6 / #808085`; severity `#ff453a / #ff8a1f / #ffd426 / #2ec5d3`. The V2 release states the dashboard's neutral ladder, inks and severity marks were "used verbatim", with Alice / Jordy / Uranian as the only accents (`IR-64` req. 3). Note: the TUI severity hexes differ from the Angular `--nt-*` severity tokens (`#EF1600 / #FF9A3C / #F4FA6A / #50ADFD`); HYPOTHESIS that "verbatim" refers to an earlier token set — UNKNOWN. Truecolor is required for exact tokens.

## 9. Proof vocabulary in the terminal

- `pathKind` (`core.ts:419-432`): `observed` = Evidence-Backed; `derived` only if the engine checked every claim; everything else = Possible Attack Path. `pathProof.why` (`core.ts:438`): "declared by the agent, not verified" / "inferred by the server, a hypothesis".
- Screen text (`index.tsx:1599`): "Only evidence the server recorded makes a path evidence-backed; declared and inferred paths stay possible."
- Earlier wording `PROVEN · corroborated` / `DECLARED · not verified` / `INFERRED · hypothesis` (`IR-64`) — SUPERSEDED in lot L8 to match the Angular legend (`IR-67`).

