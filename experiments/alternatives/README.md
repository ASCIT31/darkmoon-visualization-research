# Alternatives studied

## Campaign bar (2026-11-20)

The campaign header had to carry three jobs — **context** (project, campaign), **views** (Map, Attack paths, Findings, Overview, Reports, More) and **action** (Review findings) — at 1440, 834, 390 and 320 px. Owner complaints used as the test: (V) empty space inside the box, (B) overlapping borders / button pushed down, (W) wrapping rows, (M) views not visible on phones.

- Research: [VEILLE.md](campaign-bar-study/VEILLE.md) (Primer, Linear, Vercel, Material 3, Apple HIG, WAI-ARIA APG, …)
- Tokens used by the boards: [DESIGN-SYSTEM.md](campaign-bar-study/DESIGN-SYSTEM.md)
- Scoring and top 3: [RECOMMENDATION.md](campaign-bar-study/RECOMMENDATION.md) — study ranking **02 > 01 > 07**
- The 20 boards (**mock-ups**): [campaign-bar-boards/](campaign-bar-boards/README.md)
- **Owner decision: Board 13**, implemented as `17a74ea1` and visible in [the final capture](../../screenshots/current/pro-campaign-overview-dark-1440-v9.png).

Why the study's first choice lost is recorded honestly: the study rated Board 13 « heavy on desktop »; the owner preferred it. No user test arbitrated between them. See [UXDR-16](../../decisions/README.md).

## Other alternatives (summarised elsewhere)

- Orbital lab variants Minimal / Guided / Spatial → Guided kept ([orbital-graph.md](../../research/graph-layouts/orbital-graph.md)).
- Terminal options A (plugin API) / B (patched fork) / C (separate binary) / D (custom commands) → A kept ([tui/architecture](../../tui/architecture/README.md)).
