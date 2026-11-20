# Terminal interfaces

Conventions: see the [repository README](../../README.md#conventions).

Research question: can an operator browse a whole pentest record (workspace, projects, campaigns, findings, reports, plan, frameworks, attack paths) **without leaving the agent console** and **without any model call**, in both the open-source and the commercial edition?

Answer recorded on 2026-11-20: yes, as an OpenCode TUI plugin (Solid + OpenTUI) after resyncing the OpenCode fork to 1.19.6. Zero model calls while browsing were verified by message counts; the Community edition reads local JSON files through a sandboxed Python adapter that fails closed on redaction.

| Topic | Where |
|---|---|
| Options compared (plugin API, patched fork, separate binary, custom commands) and plugin API used | [tui/architecture](../../tui/architecture/README.md) |
| Keyboard model (vim-style moves, single-key filters, `q` as the only exit) | [tui/keyboard-navigation](../../tui/keyboard-navigation/README.md) |
| Markdown, Unicode graphics, ASCII fallback, borders, themes, proof vocabulary | [tui/rendering](../../tui/rendering/README.md) |
| V1 → V2 → visual upgrade → border polish → Community port → executive views → 80-column tabs | [tui/iterations](../../tui/iterations/README.md) |

## Inspirations vs reuse

| Reference | Idea taken | Code reused |
|---|---|---|
| [Lazygit](https://github.com/jesseduffield/lazygit) | titled panes, focused-pane accent, list + inspector, `tab` between panes | No |
| [K9s](https://github.com/derailed/k9s) | dense tables, header row, single-key filters, keys in the footer | No |
| [btop](https://github.com/aristocratos/btop) | horizontal bars, sparkline, figure + sub-visual | No |
| [Posting](https://github.com/darrenburns/posting) | section bar, quiet panels, document-like reading pane | No |

Non-reuse is verified from the plugin imports (`@opencode-ai/plugin/tui`, `@opentui/*`, `solid-js`, `node:child_process` only) and the absence of a `package.json`.

Captures: [workspace](../../screenshots/current/community-tui-workspace-120x40-v1.png), [plan](../../screenshots/current/community-tui-plan-120x40-v1.png), [paths](../../screenshots/current/community-tui-paths-160x45-v1.png), [legend 80×24](../../screenshots/current/community-tui-legend-80x24-v1.png).
