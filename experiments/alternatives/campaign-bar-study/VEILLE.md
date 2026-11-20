# Design research: context switcher + view tabs + primary action

> Part of the campaign-bar design study (2026-11-20), reproduced as written. « Screens NN » are numbered owner review screenshots (not published). Boards are **mock-ups**: [campaign-bar-boards](../campaign-bar-boards/README.md). The owner finally chose **Board 13**, not the study's first choice.

Scope: how strong products lay out « where am I (context) · which view · what do I do now (action) » on desktop and on phones. This feeds the 20 boards in `boards/` and `RECOMMENDATION.md`. Patterns are described in my own words; no proprietary assets were copied.

## 1. The three jobs are different components

The products that do this well never put all three jobs in one capsule. They split the header into separate parts:

| Job | Typical component | Examples |
|---|---|---|
| **Context** (project, campaign) | Breadcrumb or pop-up button, often styled as the page title | GitHub `owner / repo`, Vercel scope switcher, Linear location bar, Stripe object header |
| **Navigation** (views of the same object) | Underline tabs, segmented control, or a vertical list | GitHub UnderlineNav, Sentry issue tabs (Details / Activity …), Snyk project tabs (Overview / History / Settings) |
| **Action** (the next step) | One primary button, plus an overflow menu for the rest | Stripe « Create … », GitHub « Code ▾ », Linear header actions |

Linear's 2024 refresh says it plainly: separate location and view bars, header actions kept in predictable places, desktop tabs made smaller « so they don't compete with the main work area ». Softer borders and fewer separators were part of the same change. [Linear, Behind the latest design refresh](https://linear.app/now/behind-the-latest-design-refresh).

**What this means for us:** today the Darkmoon bar packs all three jobs into one bordered pill capsule. Different widths wrap it differently, which caused screens 36–39 (borders overlapping, button pushed down) and 45 (empty space inside the box).

## 2. Overflow: Priority+ beats wrapping

- **Priority+ / More menu.** Show as many items as fit and move the rest into « More ». The current item must always stay visible. [Brad Frost, Revisiting the Priority+ pattern](https://bradfrost.com/blog/post/revisiting-the-priority-pattern/) · [CSS-Tricks, priority navigation](https://css-tricks.com/?p=201311) · [Tabler nav overflow](https://docs.tabler.io/ui/components/nav-overflow).
- **GitHub Primer UnderlineNav** has the most precise published rules:
  1. On narrow widths it hides icons first, then moves items one by one into a « More » menu.
  2. It must **not scroll horizontally, even at 320 px**.
  3. The toggle is a button with `aria-expanded`.
  4. Counters load together so the bar doesn't shift.
  5. `aria-current="page"` marks the current view.

  Sources: [Primer UnderlineNav](https://primer.style/product/components/underline-nav/) · [accessibility](https://primer.style/product/components/underline-nav/accessibility) · [navigation patterns](https://primer.style/product/ui-patterns/navigation).
- **Wrapping is never the answer.** None of the references lets a tab row wrap onto a second or third line. Wrapping is exactly what produced the « random » layouts in screens 36–39.

## 3. Scrollable tabs with edge fades

- Material separates **fixed** tabs (all visible, equal width, for few, short labels) from **scrollable** tabs (for many or long labels; some stay off-screen until scrolled). Badges are an optional part of both. Material 3 scrollable tabs start-align by default. [Material tabs (Android docs)](https://github-redirect.dependabot.com/material-components/material-components-android/blob/master/docs/components/Tabs.md) · [SAP Fiori on M3 tabs](https://www.sap.com/design-system/fiori-design-android/v25-4/components/m3-standard-components/tabs/usage) · [Flutter tab alignment](https://docs.flutter.dev/release/breaking-changes/tab-alignment.md).
- When you scroll, you must signal it: an edge fade (mask), a partly visible next item, and the current tab scrolled into view. Primer adds scroll arrows on coarse pointers.
- **Trade-off:** scrolling hides views. That is exactly what the owner objected to on screen 54 (« you can't see everything »). It is acceptable for a secondary strip; it is weak for the main navigation of the page.

## 4. Segmented controls (Apple HIG)

From [Apple HIG, Segmented controls](https://developer.apple.com/design/human-interface-guidelines/segmented-controls):

- Segments are usually **equal width**.
- Keep to about **5–7 segments on wide screens and about 5 on iPhone**.
- Use nouns as labels, and **don't mix text and icons** in one control.
- On iOS a segmented control switches between **closely related sub-views**. Separate app sections belong in a tab bar.
- On macOS, a segmented control in a **toolbar** can switch views.
- Unlike loose buttons, a segmented control **keeps its grouping at any size**.

**Fit for us:** Map / Attack paths / Findings / Overview / Reports are all views of the same campaign, so they are « closely related ». Six segments (5 + More) is at the limit, which argues for keeping « More ».

## 5. Phones: bottom bars, sheets, docks

- **Tab bar (iOS) / Navigation bar (Material 3):** 3–5 destinations, docked at the bottom where the thumb reaches. Tabs stay visible even when their content is unavailable. A tab bar is for **app sections**, a toolbar is for actions on the current screen, and the two do not appear together. [HIG Tab bars](https://developer.apple.com/design/human-interface-guidelines/tab-bars.md) · [SAP Fiori iOS tab bar: « More » tab beyond 5](https://www.sap.com/design-system/fiori-design-ios/v25-4/components/navigation-and-search/tab-bar/usage) · [Android layout & nav patterns](https://developer.android.com/design/ui/mobile/guides/layout-and-content/layout-and-nav-patterns).
- **Vercel 2025:** horizontal tabs were replaced by a resizable sidebar. On mobile there is a **floating bottom bar for one-handed use**. Projects act as filters on the same page, and the mobile header shows only the current scope name. [Vercel changelog: new dashboard navigation](https://vercel.com/changelog/new-dashboard-navigation-available) · [rollout](https://vercel.com/changelog/dashboard-navigation-redesign-rollout) · [team/project switcher](https://vercel.com/changelog/improved-experience-for-moving-between-your-teams-and-projects).
- **Pop-up button + sheet** (HIG pop-up buttons / sheets; Material bottom sheets): one button shows the current choice, and a sheet lists every option in large rows. This is the safest way to show « everything » on a 320 px screen without scrolling sideways.
- **Darkmoon caveat:** the app already has a global hamburger menu on phones. A local bottom tab bar would be a second navigation system on the same screen (HIG risk). A sheet or full-width segmented cells avoid that.

## 6. Context header patterns

- **GitHub repo:** small `owner /` crumb, `repo` as a strong title, actions on the right, then UnderlineNav with counters. The whole header is a full-bleed band with one bottom border.
- **Stripe / Linear object header:** small eyebrow (parent), large title (object), a metadata line (status, dates), actions on the right, tabs below. When you scroll, the title shrinks into a sticky row (same idea as Apple's large title → inline title).
- **Vercel scope switcher / Linear location bar:** every breadcrumb level is a menu, so you switch context without leaving the view. The Linear changelog notes that right-clicking a breadcrumb offers copy actions. [Linear changelog](https://linear.app/changelog/page/8).
- **Sentry issue details:** tabs (Details, Activity …) under an issue header, with progressive disclosure. [Sentry issue details](https://docs.sentry.io/product/issues/issue-details) · [new issue details UI](https://sentry.io/changelog/new-issue-details-ui-now-available/).
- **Grafana:** moved panel titles from centred to left-aligned so each header element has its own space and a single interaction. [Grafana blog](https://grafana.com/blog/2023/11/08/grafana-panel-titles-why-we-changed-from-center-to-left-aligned).
- **Security consoles:** Snyk projects use Overview / History / Settings tabs under a project header. [Snyk project information](https://docs.snyk.io/snyk-platform-administration/snyk-projects/project-information). I found no public layout documentation for Wiz or CrowdStrike, so I make no claims about them.

## 7. Counters and badges

- Put counters **inside the tab, after the label**, in a neutral pill (Primer, Material badges). Never colour the counter by severity in the navigation: severity colours mean findings, not views.
- Only show a second number when it adds meaning. Our `2 (+17)` (proven vs possible paths) is legitimate. On a phone it can move to the screen-reader text and the Attack paths page.
- Load all counters at once to avoid layout shift (Primer `loadingCounters`).

## 8. Accessibility: tabs vs navigation

- **Each campaign view has its own URL, so it is navigation, not a tab panel.** Use `<nav aria-label="Campaign views">` with links and `aria-current="page"`. The APG `tablist` / `tab` / `tabpanel` pattern is for panels that switch in place without changing the URL. With `tablist`: arrow keys wrap, Home/End jump, and you choose automatic or manual activation. [WAI-ARIA APG Tabs](https://www.w3.org/WAI/ARIA/apg/patterns/tabs/).
- Arrow keys may still move between links (a roving tabindex is optional). The current Darkmoon component already does this, and its More menu is a correct menu-button pattern (↑ ↓ Home End Escape, focus moved into the menu on open).
- **Touch targets:** 44 × 44 px (HIG; WCAG 2.5.5), 40 px minimum, and 8 px between adjacent targets (ui-ux-pro-max `touch-target-size`, `touch-spacing`).
- **No horizontal page scroll at 320 px** (WCAG 1.4.10 Reflow; ui-ux-pro-max `horizontal-scroll`, severity High; Primer UnderlineNav).
- Show the current view by more than colour: underline, weight, filled segment or check mark.

## 9. Takeaways for Darkmoon

1. **Split the jobs.** Context reads like a title, views are one control, and the action sits beside the context or the views, never inside the same bordered capsule.
2. **Don't let tab rows wrap.** Use fixed segments that fit, or Priority+ into More.
3. **No empty space inside a bordered box.** Either the box hugs its content (board 14) or the control fills the row (boards 02, 11, 13).
4. **One edge, not many.** Use fills for segments and at most one hairline per component. Overlapping borders come from adjacent bordered pills.
5. **Phones: show everything without scrolling sideways.** Use full-width cells (02, 13), a view sheet (07) or, with care, a bottom bar (06, 15).
6. **Keep link semantics** (`nav` + `aria-current`), not `tablist`.
