# UX decision records (UXDR)

ADR-style records of the UI/visualisation decisions that shaped DarkMoon Pro and the Terminal Explorer. "Decider" = owner unless stated; owner quotes are translated from French and come from owner review notes or conversations (OBSERVED), the implementation evidence is primary (VERIFIED where marked).

Conventions (status labels VERIFIED / OBSERVED / SIMULATED / HYPOTHESIS / SUPERSEDED / UNKNOWN, internal source identifiers `IR-nn`, `IW`, `FT`, `CE`, owner notes) are defined in the [repository README](../README.md#conventions). Commits are short SHAs of **DarkMoon Pro (internal, private repository)** unless marked Community (public `ASCIT31/Dark-Moon`).

Format: **Context · Decision · Alternatives rejected · Consequences · Source · Status**.

---

### UXDR-01 — Fix the infrastructure-map data before the renderer (2026-09-25/26)
- **Context**: all findings piled on one node; random links (G6/dagre).
- **Decision**: server-side deterministic finding attachment + BFS attack paths; agents told to attach every finding.
- **Rejected**: restyling the G6 graph alone.
- **Consequences**: coherent maps; still heuristic attachment (exposure, not proof).
- **Source**: `IW-audit` §3 (VERIFIED); `0f143981` (VERIFIED).
- **Status**: VERIFIED; heuristic BFS paths later SUPERSEDED as "exposure paths" (UXDR-05).

### UXDR-02 — Orbital three.js/WebXR map, Guided variant only, lab engine reused (2026-09-26)
- **Context**: need PC/tablet/VR ergonomics; lab with variants Minimal/Guided/Spatial.
- **Decision**: port the lab engine, keep only Guided, English, charter colours, VR button gated by `isSessionSupported`.
- **Rejected**: from-scratch rebuild ("not at all that"); variant switcher; off-charter cyan.
- **Source**: `IW`; `0f143981`; owner notes
- **Status**: VERIFIED (code) / OBSERVED (owner words).

### UXDR-03 — Real = observed only; demo isolated; ranking gated (2026-10-29)
- **Context**: infra/path view showed inferred or demo links as if real; no real campaign could support "fix X interrupts N/M paths".
- **Decision**: observed-path adapter, read-only analysis service, display gate on ranking.
- **Source**: `6ab37775`, `4d0786f3`, `01f04104` (VERIFIED); `FT` §A.
- **Status**: VERIFIED.

### UXDR-04 — "Modernise, never add blocks" (2026-10-29/30) — later relaxed
- **Context**: a round added an activity chart, severity bars, "Last campaign", "Top risky targets".
- **Decision**: "I forbid you to invent cards"; each view keeps exactly its existing blocks and order, restyled with kit components.
- **Consequences**: revert to reference blocks (`73bfc2c7`, VERIFIED).
- **Amendment 2026-11-20**: "you may add cards if they are well made" — no duplicates/filler (Security posture, Application profile kept; Treatment overview removed).
- **Source**: owner notes; `IR-67` §8.1.
- **Status**: OBSERVED; original rule SUPERSEDED in part.

### UXDR-05 — Exposure path is never called an attack path (2026-10-29 → 11-06)
- **Decision**: legacy orbital "attack" mode → "exposure path (inventory, not a proven attack chain)"; walk carries "Exposure path · not proven" and a fixed honesty line; guided story only for server-proven paths.
- **Source**: `009f2a8b`, `09fb2fc0` (VERIFIED); `orbital-graph.ts` l.56–63, 103–107 (VERIFIED).
- **Status**: VERIFIED.

### UXDR-06 — Classic dashboard, not futurism; 3D only as accent (2026-10-30)
- **Context**: Brief prototype, editorial prototype, full-screen 3D world, variants A/B/C.
- **Decision**: "a classic dashboard like Eclipse but pretty"; 3D limited to the infra map / path focus.
- **Rejected**: full-screen 3D ("that's not a dashboard"), Le Brief, fintech style layers, liquid glass, Petrillo emblems.
- **Source**: owner notes; reverts `6cf7b611`, `30d1860a` (VERIFIED).
- **Status**: OBSERVED / VERIFIED (reverts).

### UXDR-07 — ui-ux-pro-max arbitrates design options (2026-10-29)
- **Decision**: the skill decides open options, citing its rule; precedence owner > charter/kit > skill.
- **Consequences**: `uxpm/DECISIONS.md` on `fe/*` branches; skill's palette/Fira proposals discarded for the charter.
- **Source**: owner notes; `49d4932a` (VERIFIED commit).
- **Status**: OBSERVED.

### UXDR-08 — Measure the reference; never invent intermediate styles (2026-11-04/05)
- **Context**: invented sidebar/tree/rail/matrix variants and a Discord adaptation rejected ("abject").
- **Decision**: locate the matching kit screen, measure (computed styles), reproduce, show side-by-side board; confirm scope before building when a reference app is named.
- **Source**: owner notes; reverts `378a67fe`, `231d9d40`, `3551998c` (VERIFIED).
- **Status**: OBSERVED / VERIFIED.

### UXDR-09 — Focus kit on real data; skins rejected (2026-11-01 → 11-03)
- **Decision**: strict Focus kit implementation (`feat/focus-kit-real`) with only real backend fields, real navigation, zero fictional data; LOT 3 skins abandoned.
- **Source**: owner notes; `befd5441`, tag `focus-kit-real-stylish-v1` (VERIFIED).
- **Status**: VERIFIED (integration via `feat/dev-focus-kit` merged into `origin/dev`).

### UXDR-10 — No Figma iOS kit; public HIG only (2026-11-04)
- **Decision**: Apple Figma kit licence forbids web use/extraction → not used.
- **Source**: `d38decde` (VERIFIED).
- **Status**: VERIFIED.

### UXDR-11 — Operator-declared paths are a separate, never-counted class (2026-11-04)
- **Decision**: admin-only, audited, labelled "Declared by operator · not proven", never counted with observed paths, the plan or the report; thin dashed lines.
- **Source**: `506f390e`, `c6fb1438`; `declare-path-sheet.html:8`; `orbital-graph.ts` l.147–148 (VERIFIED).
- **Status**: VERIFIED.

### UXDR-12 — Agent-declared links shown immediately, never proven (Arch-D, 2026-11-17/18)
- **Decision**: `enabled_by` declarations displayed as "declared by agent" (UI: Possible); server adds "observed flow" badge only for links in its own traces.
- **Rejected**: Architecture C (token cost +1.02–1.75 %); wording change `e44ceba8` reverted (`cbc4df04`).
- **Source**: `FT` §7 items 11–13 (VERIFIED refs).
- **Status**: VERIFIED.

### UXDR-13 — One legend vocabulary across 2D, rail, 3D, report, TUI (2026-11-19)
- **Decision**: Evidence-Backed (solid + shield) / Possible (dashed + dashed circle) / Treatment in progress (dotted + wrench); evidence status as words; words parity-tested with the server.
- **Source**: `f3283be0`; `IR-67` §3.9; TUI L8 rename (`IR-67`).
- **Status**: VERIFIED (commit) / OBSERVED (tests).

### UXDR-14 — Server computes, UI displays (exec-v2, 2026-11-19)
- **Decision**: every number from an exec_core document; new fields optional (older servers degrade gracefully); fixtures regenerated from the real API.
- **Source**: `IR-67` header, §8.1.
- **Status**: OBSERVED.

### UXDR-15 — Space Mono retired from the product UI (2026-10-31 → 11-20)
- **Context**: charter PDF uses Space Mono titles; kit uses Google Sans Flex.
- **Decision**: titles in Google Sans (31 Oct); progressive removal (`fd8d6d92`, `42ab2afc`, `79b3de87`); **site-wide retirement** "obsolete" (`d2af641b`). PDF report keeps charter fonts.
- **Source**: commits (VERIFIED); owner notes
- **Status**: VERIFIED.

### UXDR-16 — Campaign bar = Board 13 header card (2026-11-20)
- **Context**: capsule bar defects (overlapping borders, button pushed down, empty space, phone views hidden by sideways scroll); research (`VEILLE.md`) + 20 boards.
- **Decision**: owner chose **Board 13** (one card: picker + actions on top, equal-width icon-over-label cells with a 3 px indicator; phones keep 5 cells, Reports into More, counters as badges), applied to the whole bar family.
- **Rejected**: Board 02 (study's first choice, grey-track segmented), Board 01 (underline tabs scroll on phone), Board 07, Board 14 (cheap fallback), bottom bar/dock (second nav system), sidebar tree (app-shell project).
- **Consequences**: `.cb` rules deleted from 11 stylesheets, not out-ranked; hairline protected from theme catch-all (`03b8fc10`); "implemented under time pressure; captures to be reviewed after deploy".
- **Source**: `experiments/alternatives/campaign-bar-study/RECOMMENDATION.md`; `17a74ea1` (VERIFIED); `IR-67`; MEM (owner choice).
- **Status**: VERIFIED (implementation) / OBSERVED (choice rationale) / UNKNOWN (post-deploy owner review).

### UXDR-17 — Terminal Explorer as an OpenCode TUI plugin after fork resync (2026-11-18)
- **Decision**: option A (official plugin API) after resync 1.3.0 → 1.19.6.
- **Rejected**: separate binary (leaves the console), custom commands (LLM turn per step); patch the fork (fallback only).
- **Source**: `IR-63`; OpenCode fork `f4e363c8`; Pro PR #51 (VERIFIED).
- **Status**: VERIFIED.

### UXDR-18 — Community explorer reads local files, fails closed on redaction (2026-11-18/19)
- **Decision**: stdlib Python adapter over the JSON store; vendored redaction; evidence withheld if redaction cannot load; Community never proves paths.
- **Source**: `darkmoon_explore.py` (COM); `IR-64`; Community PR #59 (VERIFIED).
- **Status**: VERIFIED.

### UXDR-19 — Root `esc` stays in the explorer (2026-11-18)
- **Context**: capture incident sent keystrokes to the chat ($0.09 model answer).
- **Decision**: toast "Press q to return to the console"; `q` is the only exit.
- **Source**: `index.tsx:387`; `IR-65` (VERIFIED).
- **Status**: VERIFIED.

### UXDR-20 — Pane borders reinstated in the TUI (2026-11-19)
- **Decision**: single-line borders with inline titles + ASCII border fallback, reversing V2's "no box frames".
- **Source**: `edecb5a9`, `ddfd523c`; `IR-64` (SUPERSEDED rule).
- **Status**: VERIFIED.

### UXDR-21 — PDF report = A4 portrait corporate (2026-11-08)
- **Decision**: charter = identity only (colours, fonts, logo, cover), never the slide format; white cover, greyscale-safe severity markers.
- **Source**: `4bb6c6e7`, `d6c6f570` (VERIFIED); owner notes
- **Status**: VERIFIED.
