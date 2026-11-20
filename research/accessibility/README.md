# Accessibility

Conventions: see the [repository README](../../README.md#conventions). Contrast figures below are claims from commit messages, not re-measured for this repository.

| Measure | Evidence | Status |
|---|---|---|
| State never by colour alone: proof level = line style + word + icon (solid + shield / dashed + dashed circle / dotted + wrench) | `b8f91027`, `f3283be0` | VERIFIED (commits) |
| Severity colours only on data, never on navigation chrome; brand blues encode entity families, never risk | design tokens; [design-system recap](../../experiments/alternatives/campaign-bar-study/DESIGN-SYSTEM.md) | VERIFIED |
| Contrast fixes: Fixes list 1.07 → fixed (`bcc82859`); campaign-bar primary on light 1.12:1 → 3.5:1 shape (`1c6322f3`); frameworks text ≥ 7:1 (`ca798845`); 2 axe colour-contrast nodes on orbital labels (`02808ed4`) | commits | VERIFIED (claims) |
| 44 px targets at phone width (`b1d31ea9`); keyboard e2e with axe at 1440/390 (`8050fe6e`) | commits | VERIFIED |
| Campaign views are navigation (`<nav>` links + `aria-current="page"`), not an ARIA `tablist`, because each view has its own URL | [study §8](../../experiments/alternatives/campaign-bar-study/VEILLE.md) | VERIFIED (recommendation); implementation OBSERVED |
| Reflow: no horizontal page scroll at 390 / 320 px (WCAG 1.4.10) | IR-12 found `/frameworks` at 1032 px on a 390 px phone; later report: no overflow at 390 px on listed views | VERIFIED (reports); frameworks fix UNKNOWN |
| Orbital map: keyboard selection (arrow keys announce the asset in a live region), List view fallback, reduced motion, render on demand | IR-12 checks; `7c86bc05` | VERIFIED |
| Plan drawer: native modal `<dialog>`, focus trapped, Escape returns focus to the row | IR-67 | OBSERVED |
| PDF: greyscale-safe severity markers, white cover | `d6c6f570` | VERIFIED |
| Terminal: ASCII fallback (`DARKMOON_ASCII=1` or non-UTF-8 locale); terminal-escape sanitisation; truecolor needed for exact tokens | plugin code | VERIFIED |

Not done: screen-reader testing with users, an automated WCAG audit of every route in CI, Lighthouse accessibility scores.
