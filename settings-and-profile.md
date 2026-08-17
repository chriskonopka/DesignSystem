---
name: McDermott Settings & Profile
description: 'Use when designing settings pages, preferences, account menus, user profiles, appearance and theme controls, session and sign-out flows, support/about sections, danger zones, or deciding where a per-user preference should live. Load whenever an app needs an account menu or any user-configurable option.'
version: 1.0.0
---
# McDermott Settings & Profile
One combined surface for who the user is and how the app behaves for them. Pairs with `navigation-and-ia.md` (the account control) and `app-shell-and-headers.md` (the session cluster).

## The one-surface principle
Profile and Settings are **one destination, named "Settings"**, with Profile & account as its first section. Every app uses the same entry point, the same category names, and the same order. An app has at most **one** Settings destination — a gear icon anywhere else deep-links into the relevant settings section, never opens a bespoke panel. Scattered per-feature settings panels are how consistency dies.

## Entry points
The **account control** opens the **account menu**; the account menu leads to Settings.
- **Sidebar apps:** the pinned account control at the sidebar bottom (avatar + name/role — required per `navigation-and-ia.md`).
- **No-sidebar apps:** the account avatar in the top-bar session cluster (per `app-shell-and-headers.md`). Never both.
- Sidebar apps may additionally list Settings under an "ACCOUNT" nav section (icon marker: `gear`) — optional, but the account menu is always present.

## The account menu
A standard popover (per `disclosure-surfaces.md`), anchored to the account control:
1. **Identity header** (not interactive): avatar 32px, name (Sans 14pt, 600-weight), email or role below (12pt, `--text-secondary`).
2. **Menu items**, 40px row height, Phosphor 20px icons: **Settings** (`gear`) · **Support** (`question`) — deep-links to the Support & about section.
3. **Sign out** (`sign-out`) last, separated by a 1px `--border-light` rule. Sign out is a normal menu item — never destructive-styled, never hidden in a submenu.

Popover behavior follows `disclosure-surfaces.md`: max 320px with the viewport-aware cap, opens above the control when anchored at the sidebar bottom, closes on outside click / Escape / selection, returns focus to the trigger. `aria-haspopup="menu"` + `aria-expanded` on the control.

## The settings surface — a full page route
Settings is a **substantial standalone task**: a back-button-able route (`/settings`), shareable, with sections addressable by anchor (`/settings#appearance`) so deep links from gear icons and "manage in Settings" links land on the right section.
- **Layout:** reading width (`max-width: min(100%, 1200px)` per `app-shell-and-headers.md`), single-column forms per `forms-and-input.md`.
- **Section navigation:** one scrolling page with anchored sections. With 5+ categories and ≥768px of *available canvas width*, add a left anchor nav (in-canvas, not the app sidebar) — icon leading markers, one marker per item, scroll-spy rules from `navigation-and-ia.md` apply (threshold ≥ scroll-padding + scroll-margin, immediate `setActive` on click). With ≤4 categories, section headers alone are enough. **Key the collapse to the container, not the viewport** (`responsive-and-mobile.md`): a docked assistant panel narrows the canvas at desktop widths, and the anchor nav must convert to the horizontal scrolling anchor row exactly as it does on a small screen — never sit as a fixed 210px column squeezing the content beside it.
- **Don't** paginate categories into separate routes unless the app is genuinely large — one page keeps settings searchable with Cmd/Ctrl+F and cheap to scan.
- Page title "Settings" is the canvas H1; the top bar never repeats it.

## Canonical categories — fixed order
Apps **omit** categories they don't need but **never reorder or rename** the ones they have. This is what makes every McDermott app feel like the same product.

| # | Category | Contents |
|---|---|---|
| 1 | **Profile & account** | Avatar, name, email, role. Org-managed fields render read-only (plain text per `forms-and-input.md`). Password / SSO note, active sessions, "Sign out of all devices". |
| 2 | **Default behaviors** | App-specific defaults (default matter, export format, landing page) **plus** the system-persisted preferences that otherwise have no home — table density, items-per-page, sidebar collapsed state — inspectable and resettable here. |
| 3 | **Appearance & accessibility** | Theme: Light / Dark / System segmented control, **mirroring** the top-bar toggle (the toggle stays — it's the fast path; this is the persistent record). Reduced motion follows the OS setting — state that here rather than duplicating it as an app toggle. Text size follows the same deferral: browser/OS zoom already owns it (and meets WCAG's 200% resize), so an in-app stepper is added **only if the product genuinely offers text size as a feature** (e.g., a reading-heavy surface); an app control layered on browser zoom fights it. When deferring, show a quiet Text size row stating the deferral — "Use your browser's zoom (Cmd/Ctrl +/−)" — exactly like the Reduced motion row, so the option reads as answered, not absent. |
| 4 | **Notifications** | Channel toggles (in-app, email) and per-event checkboxes, per `notifications-and-feedback.md`. |
| 5 | **AI & data** | Memory inspection — "What I remember about you" with per-fact edit/delete and "Forget everything" (per `ai-feedback-and-correction.md`). Connected context sources (per `ai-prompting-affordances.md`). Data retention and export. |
| 6 | **Support & about** | Version / environment label, documentation link, contact support (pre-populated with context per `loading-empty-and-error-states.md`), legal. |
| 7 | **Danger zone** | Always last. Delete / transfer / reset actions with named consequences. |

## Interaction rules
- **Toggles apply immediately, silently.** A toggle is a binary setting taking effect now (`forms-and-input.md`); the changed state IS the confirmation — no toast (`notifications-and-feedback.md`).
- **Grouped text fields save explicitly.** Profile-style field groups get the standard form footer: Save changes (primary, right), Cancel (secondary, left). Validate on blur + on save.
- **Theme and appearance changes apply instantly** across the app — never behind a Save button.
- **Scope is stated when ambiguous.** Helper text names the reach: "Applies to this app only" vs "Applies across your McDermott apps."
- **Destructive actions** use the modal with the specific consequence named in title and button (`ux-copy-and-microcopy.md`). Irreversible bulk deletion may add type-to-confirm.

## McDermott-specific
- **Avatar:** pill shape (`--radius-pill`), initials fallback on `--color-pale-blue` with navy initials (theme-stable rule). 32px in nav and menus, 64px in the Profile section. Photo avatars keep the same dimensions.
- **Section headers:** `--font-mix` (Georgia) 24pt, sentence case. Section separation: 1px `--border-light`, `--space-8` between major sections.
- **Setting rows:** label + helper left, control right; 1px `--border-light` between rows; row padding `--space-4` vertical. Rows are `flex-wrap: wrap` with `min-width: 0` on the label block and value — when the canvas is too narrow for label + control side by side, the control wraps below the label instead of clipping.
- **Danger zone:** the inline-alert error treatment (4px `--color-error` left border, `--color-pale-orange` fill, navy text) wraps the **intro text only** — the action button sits **beside it on the section surface**, never inside the pale block, so the pale-rest destructive button can't camouflage into a matching fill. Actions use the quiet destructive variant (pale-orange rest, deep-red + white on hover) — the only destructive button in the system; the confirm modal these actions open uses it too (`_core-requirements.md`, Destructive actions). No pale fill on the section itself — it's a routine surface, not a celebration.
- **Account menu:** `--bg-surface`, `--shadow-md`, 1px `--border-light`, 2px radius.
- **Segmented controls** in settings pair a 16px Phosphor icon with each label where a conventional glyph exists (theme: `sun` / `moon` / `monitor`; density: `rows` / `list-dashes`) — all segments or none, icons `aria-hidden`, never icon-only. Per `forms-and-input.md`.

## Mobile
- **Account menu:** popover on desktop; **bottom sheet** below 640px (thumb reach, per the mobile patterns in `ai-tool-use-and-agency.md` / `disclosure-surfaces.md`).
- **Settings page:** sections stack single-column; the left anchor nav becomes a horizontally scrolling anchor row (scrolls, never wraps, per the tab-row rule) or is dropped for ≤4 categories. The same conversion fires whenever the canvas narrows below ~768px for any reason — docked panel included, not just a mobile viewport.
- **Setting rows:** label left, toggle right, row height ≥44px. Long helper text wraps under the label, never beside it.
- **Danger zone buttons** stack vertically, full width.
- In the mobile drawer, the pinned account control behaves exactly as in the desktop sidebar.

## Anti-patterns
- Separate Profile and Settings destinations (one combined surface, always)
- Per-feature gear icons opening bespoke panels instead of deep-linking into Settings
- Settings in a modal or popover — it's a full page route
- The account control shown in both the top bar and the sidebar (pick one home per `app-shell-and-headers.md`)
- Sign out styled destructive, hidden, or missing from the account menu
- Toasting every toggle change (silent success is correct for settings)
- Reordering or renaming the canonical categories per app
- Theme control only in Settings — the top-bar toggle is the fast path; keep both, mirrored
- Danger zone anywhere but last, or styled as an ordinary section with no separation
- A persisted per-user preference (density, collapse state, items-per-page) with no inspectable home in Settings
- An in-app "reduce motion" toggle duplicating the OS preference
- Save buttons on instant-apply controls, or instant-apply on grouped text fields
