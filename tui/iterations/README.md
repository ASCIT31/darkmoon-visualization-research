# Terminal Explorer — iterations and tests

Part of the Terminal Explorer research (DarkMoon Community and Pro, OpenCode TUI plugin, public code at [`ASCIT31/Dark-Moon` → `conf/plugins/darkmoon-explorer/`](https://github.com/ASCIT31/Dark-Moon/tree/master/conf/plugins/darkmoon-explorer)). Conventions: see the [repository README](../../README.md#conventions). Overview: [architecture](../architecture/README.md).

## 11. Iterations and tests

| Stage | Date / ref | Result | Status |
|---|---|---|---|
| V1 prototype | Pro `bbc87314`, PR #53 (2026-11-18) | 16 screens on real DVGA data; bun 15 tests | VERIFIED (reports) |
| V2 workspace + Community source | `6a30b3a7`, `de065105` | 34 captures; bun 23, Python 7; Pro GO, Community "GO for port / NO-GO for release today" (image not pinned to 1.19.6) | VERIFIED (`IR-64`) |
| Visual upgrade | `9c77b70e`, PR #54 | bun 28, Python 8; before/after `vq-before` → `vq-it1..3` | VERIFIED |
| Border polish | `edecb5a9`, `ddfd523c` | 8 `.ans` + 8 `.png` | VERIFIED |
| Community port | Community PR #59 (2026-11-19) | 18 PNG + 18 ANSI captures; GO to open PR, NO-GO for release until AGPL legal question | VERIFIED (`IR-66`) |
| L8 (exec-v2) | `d225566a` etc. | bun 40 → 43 (224 expects); Community Python 29 → 32; 36 PNG per edition; plan v2 fields, status words | VERIFIED (`IR-67`) |
| 80-column tabs + scrollable legend | Pro PR #60 / Community PR #62, `17f9f506` | bun 48/48 | VERIFIED |

**Incident** (`IR-65`, VERIFIED): during capture scripts, `esc` at the explorer root closed it and the next keystrokes reached the Pro chat: "6" (no answer, $0) and "5" (one model answer, **$0.09**). Fixed in product (root `esc` stays) and verified (3×esc + digit + enter → 0 new messages). Lesson: capture harnesses driving a real agent console must assert the active route before every keystroke.

