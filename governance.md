---
name: McDermott Design System Governance
description: 'Use when changing the design system itself — adding or amending a companion spec, promoting an app-local pattern into the system, deprecating a rule, bumping the version, or deciding whether something belongs in the system at all. Load before editing any spec file.'
version: 1.0.0
---
# McDermott Design System Governance
How the system grows without becoming folklore. `extrapolating-the-system.md` tells you how to build *within* the system; this file tells you how the system itself changes.

## The three-artifact rule
A pattern is not "in the system" until all three exist:
1. **Spec** — a companion `.md` (or a section in an existing one) in house style: decision guidance, anatomy, tokens, mobile, anti-patterns.
2. **Index row** — the companion table in `_core-requirements.md`, so the pipeline loads it.
3. **Showcase demo** — a working example in `mws-design-system-showcase.html` (plus its nav link and skills-library card), so there's a live reference in both themes.

Spec without showcase drifts. Showcase without spec is folklore. Either alone is a bug.

## Promotion path — app-local → system
Analysts will invent patterns the spec doesn't cover; that's expected (`extrapolating-the-system.md`). A pattern earns promotion into the system when:
- **Two or more apps** need it (or one app needs it and every future app obviously will — settings, login, search);
- It **composes entirely from existing tokens** — a pattern that needs new colors, radii, or spacing values is a proposal to change the constitution, which is a bigger conversation;
- It **passes the pre-flight gates** in `extrapolating-the-system.md` and works 320px→1920px in both themes.

Until promoted, an app-local pattern stays app-local — don't copy an unspecced pattern between apps ("shadow standards" are how inconsistency compounds). Promote it instead.

## Amending an existing rule
- Fix the **canonical owner** file (each rule has one home; other files point to it). Update cross-references in the same change.
- If the amendment contradicts what shipped apps do, note the old treatment as deprecated (below) rather than silently rewriting history.
- **Re-audit the showcase** against the amended rule before the change is done — spec changes are where showcase drift starts.

## Deprecation
- Mark the old treatment in the owning spec: `**Deprecated (v1.x):** <old rule> → use <new rule>`. Keep the note for one minor version, then delete it.
- Anti-pattern lists are the enforcement teeth — when deprecating, add the old treatment there too.

## Versioning
- The version lives in **two places**: the `_core-requirements.md` header and the showcase's Settings → Support & about row. Bump both in the same change — nowhere else (never as sidebar subtext, per `application-lockup.md`).
- **Minor bump (1.x):** new companion file, new pattern, amended rule.
- **Patch-level edits** (typos, clarified wording, no behavioral change): no bump.
- Every minor bump gets a changelog entry below.

## Changelog
### v1.5
- **New companions:** `settings-and-profile.md` (combined Settings destination, account menu, canonical categories, danger zone) · `authentication-and-session.md` (login, SSO-first, session expiry) · `search-and-command-palette.md` (search field, typeahead, ⌘K palette) · `governance.md` (this file).
- **Session cluster codified** (`app-shell-and-headers.md`): fixed order — assistant (sparkle) · notifications (bell) · theme toggle · account avatar. One launch point for app-level utilities; no top-bar settings gear.
- **Assistant panel** (`disclosure-surfaces.md`): docked = canvas column that pushes content; detached = floating draggable card; sparkle is the only launcher.
- **Sidebar collapse control** (`navigation-and-ia.md`): icon-only, in the sidebar header right of the lockup (was: labeled row at sidebar bottom — deprecated); sidebar bottom belongs to the pinned account control.
- **Lockup subtext banned** (`application-lockup.md`): no version numbers/taglines under the lockup; version info moved to Settings → Support & about (`app-shell-and-headers.md` table updated).
- **Toggle fully specced** (`forms-and-input.md`): anatomy, off-track = `--border-button` (was an internal-only gray in the showcase — fixed), disabled state, Space/Enter keyboard parity.
- **Segmented controls** (`forms-and-input.md`): optional icon+label (all-or-nothing per control); selected tint changed from `--color-pale-blue` to `color-mix(in srgb, var(--accent-interactive) 12%, transparent)` — theme-aware, calm in dark mode.
- **Reading-width content centers in the canvas** (`app-shell-and-headers.md`): capped columns get `margin-inline: auto` — leftover space splits evenly, never piling up on one side; re-centers when a docked panel narrows the canvas. Data-dense full-width surfaces unaffected.
- **Assistant pop-out tier** (`disclosure-surfaces.md`): third position — a real browser window via `window.open` that can leave the app window (multi-monitor); the in-app panel yields while popped out; the transcript is one thread synced both ways — turns from the popped window are in the panel when it returns; user-gesture only; Document PiP as Chromium enhancement.
- **Font carve-out made explicit** (`_core-requirements.md`): rule 7 now states the `ui-monospace` functional-data exception (tabular numerals, IDs, code, shortcut hints) that the typography section and `data-visualization.md` have relied on since v1.4 — system-resident, no download, never for brand type. The garbled "system stacks only — unlicensed" anti-pattern parenthetical reworded. Conformance checks should allowlist `ui-monospace` in those four contexts and block it elsewhere.
- **Requirements decide *what*, the system decides *how*** (`_core-requirements.md`, `extrapolating-the-system.md`): made explicit that specs are conditional patterns, not a feature checklist — a richly specced surface (assistant, notifications, palette) is never added to an app whose requirements didn't ask for it.
- **Assistant sheet takes the whole screen on phones** (`disclosure-surfaces.md`): below 640px the mobile overlay sheet is full-screen — a partial overlay at phone widths can't be referenced beside, so it only obscures; the close control stays visible in the sheet header as the way back.
- **Canvas-width responsiveness** (`responsive-and-mobile.md`, `settings-and-profile.md`): canvas content keys its adaptations to **container width**, not viewport media queries — a docked assistant panel narrows the canvas at desktop widths and must trigger the same collapse as a small screen (Settings anchor nav → horizontal row, rows wrap instead of clip). `container-type: inline-size` + `@container`, media queries kept as fallback.
- **Overlay scrollbars codified** (`_core-requirements.md`): invisible at rest, `currentColor`-derived pill thumb revealed only while scrolling (`.is-scrolling` utility, ~900ms decay), transparent track, reserved gutter. Default browser chrome on themed surfaces is an anti-pattern; so is removing the scrollbar mechanism entirely (hidden-at-rest with reveal is the rule). `overflow: auto`, never `scroll`.

### v1.4 and earlier
Pre-governance. The constitution and 19 companions as of the v1.4 header.

## Anti-patterns
- Editing a spec without bumping the version or logging the change
- A new pattern shipped in an app and copied to a second app without promotion
- Spec changed, showcase not re-audited (drift)
- Two files owning the same rule (one canonical owner; the rest link)
- Deprecation by silent deletion — old treatments get a marked exit
- Version numbers surfacing anywhere except the constitution header and Settings → Support & about
