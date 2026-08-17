---
name: McDermott Design System Governance
description: 'Use when changing the design system itself — adding or amending a companion spec, promoting an app-local pattern into the system, deprecating a rule, bumping the version, or deciding whether something belongs in the system at all. Load before editing any spec file.'
version: 1.0.0
---
# McDermott Design System Governance
How the system grows without becoming folklore. `extrapolating-the-system.md` tells you how to build *within* the system; this file tells you how the system itself changes.

## Regenerating the showcase — the fidelity contract
The showcase is not just a demo: the design-fidelity pipeline matches components by their **`mws-` BEM classnames** (`_core-requirements.md`, Component class naming). Any regeneration or large-scale edit must preserve that inventory. Before shipping a showcase change, diff `grep -o 'mws-[a-zA-Z0-9_-]*' | sort -u` old vs new — every removed name is a broken test until proven intentional and communicated to engineering. The v1.4→v1.5 rebuild dropped all 65 without notice; that's the failure this rule exists to prevent.

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
### v1.6
- **`mws-` BEM classnames restored** (showcase, `_core-requirements.md`, `governance.md`): the v1.4→v1.5 showcase rebuild (Aug 4) silently dropped the `mws-` BEM prefix from all 65 system-component classes (`.mws-btn--primary` → `.btn-primary`, etc.), breaking engineering's design-fidelity matching. All 65 restored 1:1 (plus `mws-btn--compact`, new this version); rendering verified pixel-equivalent before/after across 19 component fingerprints. The convention is now codified as a constitutional rule with an anti-pattern, and the "diff the `mws-` inventory before shipping a showcase change" gate is added to governance so a rebuild can't drop it silently again.
- **Pop-out transcript is one thread — synced both ways** (`disclosure-surfaces.md`): messages sent in the popped-out assistant window merge into the in-app thread as they happen; closing the window returns the panel with everything said while popped out, scrolled to the latest turn. Forking the transcript across surfaces is an anti-pattern — a pop-out that carries the conversation out but drops it on the way back reads as data loss.
- **Destructive buttons: solid resting red retired — pale rest, committing hover, pale-context inversion** (`_core-requirements.md`, `settings-and-profile.md`, `disclosure-surfaces.md`): all destructive buttons — entry points *and* the modal confirm — rest as `--color-pale-orange` fill + navy text + 3px `--color-error` left spine; on hover the fill deepens to the new `--color-error-deep` (`#C22A2A`) with white text (5.7:1 — white fails AA on the brand red at 3.6:1, which also rules out white-on-`--color-error` generally). Placement rule instead of a context variant: the button never sits *on* a pale fill — the danger zone's pale inline-alert wraps the intro text only, and the button sits beside it on the section surface (a briefly-shipped "surface-fill inversion" inside the banner read as an unsupported white button and was replaced by this layout fix). Rationale: the named-consequence modal does the safety work, so the red belongs to the moment of aim (hover), not to rest. `--color-error-deep` is an interaction fill, exempt from the alert-fills-pair-navy rule by definition. Decision evolved two-tier → quiet-everywhere → hover-commit → pale rest + placement rule, all within the same review; the `--btn-destructive-disabled-*` tokens are retired.
- **`--border-strong` token — non-text contrast fixed** (`_core-requirements.md`, `forms-and-input.md`, `accessibility.md`): audit of the dark theme found text contrast solid (7.9:1 secondary, 14.9:1 primary) but functional boundaries failing WCAG 1.4.11 — input borders and the toggle off-track sat at 1.3–2.1:1 in dark and 1.3–1.4:1 in light. New token `--border-strong` (`#85858F` light / `#6B6BD0` dark, ≥3:1 both themes) now bounds inputs/selects/textareas and fills the toggle off-track; `--border-light` stays decorative, `--border-button` stays on text-labeled buttons. Card-vs-page surface subtlety left as-is — no WCAG requirement, deliberate restraint.
- **Standard favicon, automated** (`application-lockup.md`, `_core-requirements.md`): every app ships one exact `<link>` tag — the official mark as a theme-aware inline SVG data URL (navy light / white dark via `prefers-color-scheme`). No per-app icons, no asset pipeline, nothing to forget; the tab identifies McDermott, the `<title>` identifies the app.
- **Text size defers to browser zoom** (`settings-and-profile.md`): like reduced motion, text scaling is owned by the browser/OS (and meets WCAG 200% resize); an in-app stepper appears only when a product offers text size as a real feature. Settings shows a quiet "Text size — use your browser's zoom" row so the deferral is visible, not an absence.
- **Requirements decide *what*, the system decides *how*** (`_core-requirements.md`, `extrapolating-the-system.md`): made explicit that specs are conditional patterns, not a feature checklist — a richly specced surface (assistant, notifications, palette) is never added to an app whose requirements didn't ask for it.
- **Assistant sheet takes the whole screen on phones** (`disclosure-surfaces.md`): below 640px the mobile overlay sheet is full-screen — a partial overlay at phone widths can't be referenced beside, so it only obscures; the close control stays visible in the sheet header as the way back.
- **Canvas-width responsiveness** (`responsive-and-mobile.md`, `settings-and-profile.md`): canvas content keys its adaptations to **container width**, not viewport media queries — a docked assistant panel narrows the canvas at desktop widths and must trigger the same collapse as a small screen (Settings anchor nav → horizontal row, rows wrap instead of clip). `container-type: inline-size` + `@container`, media queries kept as fallback.
- **Field feedback batch** (`navigation-and-ia.md`, `data-visualization.md`, `notifications-and-feedback.md`): (1) collapsed rail: dropdowns, expandable groups, and matter cards never render expanded inside the 72px rail — icon only, content as a flyout popover to the right. (2) Every data column is sortable (was: "only meaningful ones") — faint `caret-up-down` on inactive headers for discoverability; checkbox and actions columns exempt; priority/date columns sort semantically. (3) In-row progress/meter tracks never share the row-hover or selection tint — canon `--color-navy-gray-2` light / `--border-light` dark (the showcase's own bar track matched the hover tint and vanished on hover; fixed). (4) Labels & tags codified: same pill anatomy as status badges (pale fill or bordered-transparent) — gray-filled rectangular chips and default-button-styled labels are off-system.
- **Audit remediation batch** (`_core-requirements.md`, `forms-and-input.md`, `steppers-and-wizards.md`, `data-visualization.md`, `disclosure-surfaces.md`, showcase): (1) new semantic status-text tokens `--text-error`/`--text-success` — the only legal colored text; replaced all hardcoded status hex, including a destructive menu item that sat at 2:1 in dark mode. (2) Destructive menu items specced: last, separated, `--text-error` label+icon, always opens the confirm modal. (3) Stepper connector canon unified — 1px `--border-light` at rest, 2px `--accent-interactive` behind completed steps — across anatomy, state table, layout, tokens, and the pre-flight gate (previously three conflicting widths). (4) Checkbox/radio canon set to 24px (was 18px in the spec, 24px in the showcase; 24 matches the WCAG minimum target). (5) `--control-h-sm` (32px) and `.btn-compact` (28px) defined — `data-visualization.md`'s compact density referenced them but they didn't exist. (6) Settings page gained its missing Default behaviors category — 5 categories now legitimately earn the anchor nav. (7) Container-query adaptation extended beyond Settings to KPI tiles, bar-chart rows, and permission cards. (Toggle Space/Enter support was flagged by the audit but verification cleared it — a delegated document-level handler already provides it.)

### v1.5
- **New companions:** `settings-and-profile.md` (combined Settings destination, account menu, canonical categories, danger zone) · `authentication-and-session.md` (login, SSO-first, session expiry) · `search-and-command-palette.md` (search field, typeahead, ⌘K palette) · `governance.md` (this file).
- **Session cluster codified** (`app-shell-and-headers.md`): fixed order — assistant (sparkle) · notifications (bell) · theme toggle · account avatar. One launch point for app-level utilities; no top-bar settings gear.
- **Assistant panel** (`disclosure-surfaces.md`): docked = canvas column that pushes content; detached = floating draggable card; sparkle is the only launcher.
- **Sidebar collapse control** (`navigation-and-ia.md`): icon-only, in the sidebar header right of the lockup (was: labeled row at sidebar bottom — deprecated); sidebar bottom belongs to the pinned account control.
- **Lockup subtext banned** (`application-lockup.md`): no version numbers/taglines under the lockup; version info moved to Settings → Support & about (`app-shell-and-headers.md` table updated).
- **Toggle fully specced** (`forms-and-input.md`): anatomy, off-track = `--border-button` (was an internal-only gray in the showcase — fixed), disabled state, Space/Enter keyboard parity.
- **Segmented controls** (`forms-and-input.md`): optional icon+label (all-or-nothing per control); selected tint changed from `--color-pale-blue` to `color-mix(in srgb, var(--accent-interactive) 12%, transparent)` — theme-aware, calm in dark mode.
- **Reading-width content centers in the canvas** (`app-shell-and-headers.md`): capped columns get `margin-inline: auto` — leftover space splits evenly, never piling up on one side; re-centers when a docked panel narrows the canvas. Data-dense full-width surfaces unaffected.
- **Assistant pop-out tier** (`disclosure-surfaces.md`): third position — a real browser window via `window.open` that can leave the app window (multi-monitor); the in-app panel yields while popped out; user-gesture only; Document PiP as Chromium enhancement.
- **Font carve-out made explicit** (`_core-requirements.md`): rule 7 now states the `ui-monospace` functional-data exception (tabular numerals, IDs, code, shortcut hints) that the typography section and `data-visualization.md` have relied on since v1.4 — system-resident, no download, never for brand type. The garbled "system stacks only — unlicensed" anti-pattern parenthetical reworded. Conformance checks should allowlist `ui-monospace` in those four contexts and block it elsewhere.
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
