# McDermott Design System — Claude Design Library

The files in this folder set up the McDermott Design System inside Claude Design. Upload them once at project setup and Claude Design will reference them on every generation.

## Setup (one-time)

In Claude Design's project setup screen:

1. Click **Create new design system**
2. Under **Link code from your computer** (or "Add assets"), upload **all 26 design-system files** (everything in this folder except this README)
3. **Company name and blurb:**
   > McDermott Design System — Enterprise-grade navy + pale + accent visual system. Saturated colors are targeted spice, not default tools.
4. **Any other notes** — paste the short brief below

Leave the **GitHub**, **.fig**, and **fonts/logos** fields empty — there's no repo or Figma file, the type is Arial + Georgia (brand-approved, referenced not embedded), and the McDermott mark ships as an inline SVG inside the spec files, so there are no separate assets to upload.

That's it. The design system is now assigned to the project and applies to every generation.

## Short brief for "Any other notes" (or to paste into Tweaks)

```
McDermott Design System non-negotiables:

1. Stepper: circles on a continuous 1px track. NEVER bordered rectangles around each step. Four states (not-started / current / completed / error), all same size, same baseline.

2. Match the nav to the app's depth; never default to a sidebar. Deep, multi-section apps → side nav. Shallow apps (≤~7 sections) → nav links live inline in the top bar, no sidebar. Single-purpose apps (one form / wizard / focused tool) → no global nav at all; the top bar is just chrome (page title + primary action + account). Never add a sidebar "for consistency." When a sidebar does exist, it's app navigation only (e.g. Summary, Reference, Admin) — NEVER mirror the canvas stepper in it.

3. Top bar at narrow viewports: hamburger (only if the app has global nav to collapse) + current page name + ONE primary action + theme/account icons. Never wrap text word-by-word. Move computed totals out of the top bar — to the sidebar matter card, or a sticky summary in the content area.

4. Below 1024px: when there's a sidebar, it slides in as a drawer from the left with scrim. Never stacks above content, never disappears entirely.

5. Canvas content is centered, never pinned left. A width-capped region (reading/forms, max-width ~1200px) gets margin-inline: auto so it sits centered in the canvas with balanced gutters — not flush-left with empty space on the right. Data-dense surfaces (tables, dashboards) go full-width with symmetric gutters instead. "Centered" = the content column is centered, not text-aligned center.

6. Buttons never wrap (white-space: nowrap). Card content reflows like a standalone form — segmented controls wrap or become selects on mobile.

7. Navy + pale + accent. var(--color-teal) direct ONLY for the dark sidebar's active state and categorical chart series. Everything else interactive uses var(--accent-interactive) — blue in light mode, teal in dark.

8. Text on pale backgrounds and alert fills is ALWAYS navy, never var(--text-primary).

9. Border radius is 2px or 999px. Nothing in between.

10. Canvas content adapts to its CONTAINER, not the viewport. A docked panel (e.g. the AI assistant) narrows the canvas at desktop screen widths where no media query fires — key canvas-scoped collapses to container width (@container): anchor navs convert to horizontal scrolling rows, setting/label rows wrap instead of clipping.

11. Requirements decide WHAT; the system decides HOW. The specs are conditional patterns, not a feature checklist — never add an AI assistant, notification center, news feed, or ⌘K palette the app's requirements didn't call for. Absent features leave no gap; the session cluster compacts.
```

## Per-generation workflow

The per-screen process is driven by the **POC-planner Design brief** (`03-CLAUDE-DESIGN-poc-prototype-brief.md`, produced by the POC-planner skill) — see the Starter Guide for the full flow. In short:

- Paste the **Design brief** once — *not* the requirements doc, which the brief replaces. Claude Design builds the first screen and waits.
- Steer through the remaining screens one at a time, in order. The brief owns the stop-and-go.
- Fix drift by steering in plain words. For recurring drift, paste the short brief above into the **Tweaks panel**.

There is no separate post-build tweak file to attach — that step has been retired.

## What's in this folder

### Core (load on every generation)

| File | Covers |
|---|---|
| `_core-requirements.md` | The constitution — tokens, critical rules, lockup, brand voice, and the "requirements decide what, the system decides how" principle |
| `mws-design-system-showcase.html` | Live demo of every component in light and dark mode |
| `extrapolating-the-system.md` | Pre-flight compliance gates for anything the specs don't cover |
| `governance.md` | Decision log and version changelog — why the rules are what they are |

### Layout & navigation

| File | Covers |
|---|---|
| `app-shell-and-headers.md` | Top bar minimalism, the session cluster (sparkle · bell · theme · avatar), centered content region |
| `navigation-and-ia.md` | Choosing the nav pattern by app depth (side nav / top-bar / none), sidebar contents, collapse rail, what should NOT be in nav |
| `application-lockup.md` | Application lockup (symbol + divider + name), placement, and app naming convention |
| `disclosure-surfaces.md` | Modals, sheets, popovers, accordions — and the AI assistant panel's three homes (docked / detached / popped out, one transcript across all) |
| `responsive-and-mobile.md` | Mobile rules down to 320px, plus canvas-width (container-query) responsiveness |

### Components & patterns

| File | Covers |
|---|---|
| `forms-and-input.md` | Fields, toggles, segmented controls, validation, save patterns |
| `steppers-and-wizards.md` | Stepper spec + anti-patterns |
| `data-visualization.md` | Charts + the data-table spec (filters, sort, pagination, scroll, density, row detail) |
| `iconography.md` | Phosphor icons — sizes, weights, accessible naming |
| `loading-empty-and-error-states.md` | Skeletons, spinners, empty states, error recovery |
| `notifications-and-feedback.md` | Toasts, banners, badges, the notification center |
| `search-and-command-palette.md` | Search field, typeahead, ⌘K palette |
| `settings-and-profile.md` | One Settings destination, account menu, canonical categories, danger zone |
| `authentication-and-session.md` | Login, SSO-first, session expiry |
| `accessibility.md` | WCAG 2.2 AA floor — focus, contrast, keyboard, ARIA |
| `ux-copy-and-microcopy.md` | Voice, error-message formula, banned words, capitalization |

### AI surfaces

| File | Covers |
|---|---|
| `ai-prompting-affordances.md` | Empty states, suggested prompts, input affordances |
| `ai-streaming-and-perceived-latency.md` | Streaming, thinking indicators, Stop-in-Send-position |
| `ai-trust-and-provenance.md` | The Generated marker, citations, provenance |
| `ai-feedback-and-correction.md` | Feedback controls, memory inspection, correction flows |
| `ai-tool-use-and-agency.md` | Tool-call visibility, agentic actions, confirmation |
| `ai-uncertainty-and-errors.md` | Uncertainty language, AI error states |

AI-surface specs apply **only when the app's requirements include an AI feature** — see the constitution's "Requirements decide what" rule.
