# Failures, rejections and reverts

Conventions: see the [repository README](../../README.md#conventions). Owner quotes are translated from French (OBSERVED, owner notes); reverts are VERIFIED git objects. Captures of these directions are **not published** (lab addresses or unreviewed local data); see the [chronology](../../angular/evolution/before-after-chronology.md#publication-status-of-capture-sets).

| Date | Attempt | Outcome | Lesson |
|---|---|---|---|
| 2026-09-26 | Rebuild of the orbital graph « in its own way » | « That is not it at all » → lab engine reused | improve the liked artefact, do not rebuild it |
| 2026-10-29 | « Le Brief » hybrid dashboard (plain-language brief, XXL figures, Guided/Expert switch, moon/orbit in three places) | « very very ugly »; internal AI judges 4.5/10 twice | AI judges are not a substitute for the owner |
| 2026-10-29/30 | **Invented cards** (neon activity chart, severity bars, « Last campaign », « Top risky targets ») | « I forbid you to invent cards, modernise what exists » → revert `73bfc2c7`. Amended 20 Nov: well-made cards allowed, no duplicates or filler | add information, never filler |
| 2026-10-30 | Full-screen 3D dashboard (variant « Orbit »), editorial prototype, variants Instrument / Moonlight | « that's not a dashboard » | 3D is an accent (infra map, path focus) |
| 2026-10-30 | ~15 fintech style layers | « abject, cancel »; « too banking, not cyber enough » | domain tone matters |
| 2026-10-30 | **Liquid glass**, severity emblems, ambient field | reverted `6cf7b611`, `cd53a340`, `30d1860a`, `f7c1c63b` | effects without a job are removed |
| 2026-10-30 | Pixel-matched node-editor flow graph as default map | « absolutely ABJECT as an experience … no smooth transitions » | a reference is a mood for an experience, not an image to trace |
| 2026-10-31 | A new home-grown « visual engine » design system | « nothing of the kit in there » | the chosen kit is the system |
| 2026-10-31 | ~95 night-hunt design lots | « no look kept » (archived under a tag) | volume of options did not converge |
| 2026-11-03 | Focus-kit **skins** (several variants, deep black, card views) | rejected; pivot to the strict kit on real data | skins hide, not solve |
| 2026-11-04 | Invented sidebar / project tree / findings rail / matrix variants | « horrible », « nothing to do with the kit » | measure the reference, then reproduce |
| 2026-11-04 | Shell restructure, route blur cross-fade, summary strip, darker grey ladder | reverted `9fca57a3`, `1094ede8`, `58604f4d`, `b8397de5` | |
| 2026-11-05 | Card border / edge / grain / glass / gap experiments; chat-app-style findings rail; portfolio-site finding layout | 17 reverts incl. `3551998c`, `378a67fe`, `231d9d40`, `2e40cd0c` | |
| 2026-11-06 | Underline-tab campaign bar | capsule restored the same day (`053718cd`) | the problem was the capsule's layout, solved later by Board 13 |
| 2026-11-18 | Wording change on agent-declared link labels | reverted `cbc4df04` (gain not demonstrated, n = 1) | do not change vocabulary without evidence |
| 2026-11-20 | **Space Mono** display face | retired site-wide (`d2af641b`, « obsolete »); kept only in the PDF report — [before](../../screenshots/comparisons/pro-campaign-overview-dark-1440-v5.png) / [after](../../screenshots/comparisons/pro-campaign-overview-dark-1440-v6.png) | brand fonts belong to documents, not to dense UI |
| 2026-11-20 | **Board 02** (the study's first choice) | owner chose **Board 13** | a ranked study informs, the owner decides |
| 2026-11-20 | « Treatment overview » card | removed; counts merged into one line | no duplicate figures |

Branch census (VERIFIED): about 70 UI branches and ~120 worktrees were created between late October and mid-November 2026; the `fe/*` exploratory branches (6) were never merged. Details: [UI reform history §17–18](../../angular/evolution/README.md#17-rejected-attempts-and-owner-rejections-consolidated).
