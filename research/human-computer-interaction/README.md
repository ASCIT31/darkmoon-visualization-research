# Human–computer interaction: method and findings

Conventions: see the [repository README](../../README.md#conventions).

## Method actually used

| Practice | Description | Status |
|---|---|---|
| Owner review rounds | The product owner reviewed captures of every screen and answered in short verdicts (« abject », « that's not a dashboard », « validated »). Quotes are translated from French. | OBSERVED (owner notes) |
| Skill-arbitrated options | From 2026-10-29 an AI design-review skill (`ui-ux-pro-max`) had to choose between open options and cite its rule; precedence: owner's words > brand charter / kit > generic advice. | OBSERVED |
| Adversarial critics | Capture-based critique rounds and four AI « judges » (pentest expert, novice designer, accessibility/performance, non-regression). Their scores (e.g. 4.5/10) are not usability measures. | OBSERVED |
| Measure the reference | Computed-style comparison (Playwright `getComputedStyle`) between a reference kit and the app until zero deviation, then per-component pixel diffs. | OBSERVED |
| Real captures on demo data | Fixed viewports 1440×900, 1280×800, 1024×768, 834×1112, 390×844, 320×640; light and dark; console and API errors logged per capture. | VERIFIED (capture logs) |
| Pattern research | Primer, Linear, Vercel, Stripe, Sentry, Snyk, Grafana, Material 3, Apple HIG, WAI-ARIA APG, for the campaign bar ([study](../../experiments/alternatives/campaign-bar-study/VEILLE.md)). | VERIFIED (public URLs) |

No user testing with external participants was done (no task-time, error-rate or SUS data).

## Findings (qualitative)

1. **Honesty constraints dominate.** The owner's strongest rejections concerned meaning, not style: two figures for one notion, a hypothesis drawn like a proof, invented cards or narratives. These became rules (UXDR-03, -05, -11, -13, -14).
2. **A reference is a mood for an experience, not an image to trace.** A pixel-matched node-editor graph was rejected for lacking motion and depth.
3. **Measured reproduction is accepted; invention is not.** Every invented intermediate component (sidebar trees, rails, matrices, skins) was rejected; measured copies of the kit were accepted (UXDR-08, -09).
4. **Progressive disclosure, concretely:** one main question per view; one coverage sentence and one collapsed regulatory line in the executive block; « How this plan is built » fold; first 10 actions then « Show all N »; legend behind « What do these mean? ».
5. **The three header jobs are different components** (context, views, action). Wrapping tab rows, overlapping borders and phone views hidden by sideways scroll were the measured complaints; Board 13 answers them with one card and equal-width cells.
6. **Rules can be amended.** « Never add a card » (29 Oct) became « you may add cards if they are well made, no duplicates or filler » (20 Nov).

Related: [decisions](../../decisions/README.md) · [experiments/failures](../../experiments/failures/README.md) · [accessibility](../accessibility/README.md).
