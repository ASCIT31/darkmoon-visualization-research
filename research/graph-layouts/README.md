# Graph layouts for infrastructure and attack paths

Conventions: see the [repository README](../../README.md#conventions).

DarkMoon draws the same campaign graph (assets, services, findings, candidate paths) with four layout families. They coexist in the Pro client on `origin/dev @ d6bfc0d9` (internal); none was evaluated against the others with users (see [§Open questions](#open-questions)).

| Family | Implementation | Reading direction | Strength | Weakness / status |
|---|---|---|---|---|
| Hierarchical tree | `@antv/g6` v5, `antv-dagre` top-down | root → leaves | familiar | with incoherent data, disconnected islands and every finding on the root; legacy component kept |
| **Layered path graph** | ELK Layered (vendored `elkjs` bundle, loaded on demand) | entry left → critical asset right (top → bottom for long paths) | deterministic, proven solid / hypothesis dashed, choke points double-ringed (×N paths) | ELK cost on phones was a decision topic; robustness fix `3e45fd55` |
| **Atlas 2D map** | custom canvas, nested boundaries (Internet / Edge / Application / Data), orthogonal relations, floating toolbar | zones by exposure | whole-page map, semantic zoom with 12 px label level-of-detail, fit-to-chain | ancestor `f26e0c3b` never merged; reimplemented in the Focus-kit line |
| **Orbital 3D map** | three.js + WebXR, render on demand | rings = server exposure zone (data 0, app 1, edge/internet/tooling 2) | PC / tablet / headset; guided story only for a proven path | severity hexes differ from app tokens; no frame-rate measure |
| Terminal chain | OpenTUI, vertical node cards | top → bottom | works at 80 columns, ASCII fallback | branches listed path by path, not merged |

Theory behind the families: the layered approach is the Sugiyama framework (layer assignment, crossing reduction, coordinate assignment) as implemented by the Eclipse Layout Kernel; the orbital and Atlas views apply focus+context ideas (degree of interest, semantic zoom); the guided story applies Shneiderman's « overview first, zoom and filter, then details on demand ». See [references](../../references/README.md).

Captures (real UI, synthetic demo data): [Atlas map](../../screenshots/current/pro-infra-map-2d-dark-1440-v2.png), [attack-paths tab](../../screenshots/current/pro-attack-paths-dark-1440-v2.png), [orbital 3D](../../screenshots/current/pro-orbital-3d-dark-1440-v2.png), [terminal chain](../../screenshots/current/community-tui-paths-160x45-v1.png).

Detailed study of the orbital map (problem, prototype lab, integration, honesty hardening, evaluation): [orbital-graph.md](orbital-graph.md). Ordering and edge-merge rules of the layered graph, master–detail patterns and path representation: [data-visualization](../data-visualization/README.md#2-graph-families-studied).

## Open questions

1. No measured comparison of Atlas vs ELK vs orbital for the same task (time to locate the most exposed asset, error rate). UNKNOWN.
2. No layout-time measurement for ELK on phones; no frame-time measurement for the orbital map. UNKNOWN.
3. Whether merging branching paths into one DAG helps triage is untested (the terminal lists paths one by one; the ELK graph draws shared assets once).
