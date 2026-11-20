# Bibliography

Verification column: **verified** = checked on 2026-11-20 (DOI resolved and metadata matched through Crossref, or page fetched and title matched). Nothing below is cited from memory without a check; items that could not be checked are marked **unverified**.

## Visualization and HCI

| Ref | Reference | Used for | Verification |
|---|---|---|---|
| [Shneiderman 1996] | B. Shneiderman. « The eyes have it: a task by data type taxonomy for information visualizations ». *Proceedings 1996 IEEE Symposium on Visual Languages*, pp. 336–343. doi:[10.1109/VL.1996.545307](https://doi.org/10.1109/VL.1996.545307) | « overview first, zoom and filter, then details on demand »; guided map, progressive disclosure | verified (Crossref; pages from secondary sources) |
| [Munzner 2014] | T. Munzner. *Visualization Analysis and Design*. A K Peters / CRC Press, 2014. doi:[10.1201/b17511](https://doi.org/10.1201/b17511) | marks and channels; encoding proof level by line style rather than colour | verified (Crossref) |
| [Nielsen 2006] | J. Nielsen. « Progressive Disclosure ». Nielsen Norman Group, 3 Dec 2006. <https://www.nngroup.com/articles/progressive-disclosure/> | one main question per view; details on demand | verified (page and date) |
| [Nielsen heuristics] | Nielsen Norman Group. « 10 Usability Heuristics for User Interface Design ». <https://www.nngroup.com/articles/ten-usability-heuristics/> | review vocabulary | verified (page) |
| [Furnas 1986] | G. W. Furnas. « Generalized fisheye views ». *CHI '86* (ACM SIGCHI Bulletin 17(4)), pp. 16–23. doi:[10.1145/22339.22342](https://doi.org/10.1145/22339.22342) | focus+context, degree of interest, semantic zoom | verified (Crossref) |

## Graph drawing

| Ref | Reference | Used for | Verification |
|---|---|---|---|
| [Sugiyama 1981] | K. Sugiyama, S. Tagawa, M. Toda. « Methods for Visual Understanding of Hierarchical System Structures ». *IEEE Trans. Systems, Man, and Cybernetics* 11(2):109–125, 1981. doi:[10.1109/TSMC.1981.4308636](https://doi.org/10.1109/TSMC.1981.4308636) | layered drawing framework behind ELK Layered | verified (Crossref) |
| [ELK] | Eclipse Layout Kernel, ELK Layered algorithm reference. <https://eclipse.dev/elk/reference/algorithms/org-eclipse-elk-layered.html> (states it is based on Sugiyama, Tagawa and Toda 1981) | ap-graph layout | verified (page) |
| [elkjs] | kieler/elkjs — ELK's layout algorithms for JavaScript. <https://github.com/kieler/elkjs> | vendored bundle | verified (page) |
| [G6] | AntV G6 graph visualization engine. <https://g6.antv.antgroup.com/> | legacy infrastructure graph | verified (page) |
| [three.js] | three.js. <https://threejs.org/> | orbital map | verified (page) |
| [WebXR] | W3C. *WebXR Device API*. <https://www.w3.org/TR/webxr/> | headset mode | verified (page) |

## Design systems and platform guidelines

| Ref | Reference | Used for | Verification |
|---|---|---|---|
| [M3 tabs] | Material Design 3 — Tabs. <https://m3.material.io/components/tabs/overview> | fixed vs scrollable tabs; Board 13 | verified (page) |
| [HIG segmented] | Apple Human Interface Guidelines — Segmented controls. <https://developer.apple.com/design/human-interface-guidelines/segmented-controls> | equal widths, ~5 segments on phones | verified (page) |
| [HIG tab bars] | Apple Human Interface Guidelines — Tab bars. <https://developer.apple.com/design/human-interface-guidelines/tab-bars> | why a bottom bar would be a second navigation system | verified (page) |
| [Primer] | GitHub Primer — UnderlineNav. <https://primer.style/product/components/underline-nav/> | overflow into « More », no horizontal scroll at 320 px | verified (page) |
| [Priority+] | B. Frost. « Revisiting the Priority+ Pattern ». <https://bradfrost.com/blog/post/revisiting-the-priority-pattern/> | overflow pattern | verified (page) |
| [Linear 2024] | Linear. « A calmer interface for a product in motion » (design refresh). <https://linear.app/now/behind-the-latest-design-refresh> | separate location / view bars | verified (page) |
| [Vercel 2025] | Vercel changelog. « New dashboard navigation available ». <https://vercel.com/changelog/new-dashboard-navigation-available> | sidebar + mobile floating bar | verified (page) |
| [Grafana 2023] | Grafana Labs. « Grafana panel titles: why we changed from center to left-aligned ». <https://grafana.com/blog/2023/11/08/grafana-panel-titles-why-we-changed-from-center-to-left-aligned> | header element placement | verified (page) |
| [Angular signals] | Angular. « Signals ». <https://angular.dev/guide/signals> | state model | verified (page) |

Further references used only inside the campaign-bar study (Material Android docs, SAP Fiori, Flutter, Tabler, CSS-Tricks, Sentry, Snyk) are listed with their URLs in [VEILLE.md](../experiments/alternatives/campaign-bar-study/VEILLE.md); they were collected on 2026-11-20 by the study and were **not re-verified** for this repository.

## Accessibility

| Ref | Reference | Verification |
|---|---|---|
| [WCAG 2.2] | W3C. *Web Content Accessibility Guidelines (WCAG) 2.2*. <https://www.w3.org/TR/WCAG22/> | verified (page) |
| [Reflow] | W3C. *Understanding SC 1.4.10: Reflow*. <https://www.w3.org/WAI/WCAG22/Understanding/reflow.html> | verified (page) |
| [APG tabs] | W3C WAI-ARIA Authoring Practices — Tabs pattern. <https://www.w3.org/WAI/ARIA/apg/patterns/tabs/> | verified (page) |

## Terminal interfaces

| Ref | Reference | Verification |
|---|---|---|
| [Lazygit] | <https://github.com/jesseduffield/lazygit> — simple terminal UI for git commands | verified (page) |
| [K9s] | <https://github.com/derailed/k9s> — Kubernetes CLI | verified (page) |
| [btop] | <https://github.com/aristocratos/btop> — resource monitor | verified (page) |
| [Posting] | <https://github.com/darrenburns/posting> — API client in the terminal | verified (page) |
| [OpenCode] | <https://github.com/anomalyco/opencode> (formerly `sst/opencode`, which redirects there) | verified (page and redirect) |
| [OpenTUI] | <https://github.com/anomalyco/opentui> (formerly `sst/opentui`) | verified (page and redirect) |

## Unverified or not cited

- Wiz and CrowdStrike console layouts: no public layout documentation was found; no claim is made about them.
- Award-site and fintech references used during the October exploration are not listed: they were moods for rejected directions, and the raw crawls were not preserved.

## Internal sources

Internal identifiers (`IR-nn`, `IW`, `FT`, `CE`, owner notes) are defined in the [README conventions](../README.md#conventions). They are not public.
