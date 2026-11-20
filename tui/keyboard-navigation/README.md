# Terminal Explorer — keyboard navigation

Part of the Terminal Explorer research (DarkMoon Community and Pro, OpenCode TUI plugin, public code at [`ASCIT31/Dark-Moon` → `conf/plugins/darkmoon-explorer/`](https://github.com/ASCIT31/Dark-Moon/tree/master/conf/plugins/darkmoon-explorer)). Conventions: see the [repository README](../../README.md#conventions). Overview: [architecture](../architecture/README.md).

## 4. Navigation model

- View kinds (`index.tsx:139-151`): login, workspace, projects, campaigns, campaign, findings, finding, report, paths, path, plan, frameworks, profile.
- 7 tabs: Work / Proj / Camp / Find / Plan / Paths / Docs; four-letter labels at 80–99 columns (`17f9f506`, Pro PR #60 / Community PR #62, 2026-11-20).
- Workspace → projects → campaign history → campaign → findings (filter/sort/search) → finding → evidence; report with contents toggle and chapter goto (`g`); attack paths → path walk → step → finding.
- `esc` at the root stays in the explorer with the toast "Press q to return to the console." (`index.tsx:387`) — introduced after an incident (§11).

Keyboard (`index.tsx:1088-1120`, VERIFIED): `j/k/↑/↓`, `pgup/pgdn/ctrl+u/d`, `enter/→/l` open, `esc/←/h/backspace` back, `q` console, `/` search, `n/N` matches, `f/s/t` filters (severity / proof / target), `o` sort, `c` clear, `tab` pane, `R` report, `P` paths, `T/X/A/L/W` plan / frameworks / profile / legend / maintenance, `g` chapter, `F` cited findings, `r` refresh, `?` keys, `1–7` tabs, `]`/`[` next/previous.

