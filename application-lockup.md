---
name: McDermott Application Lockup & Naming
description: 'Use when placing an application''s identity in a UI — the McDermott symbol + divider + app name lockup, where it appears (sidebar header, top bar, login), how the name is set and wrapped, app naming conventions, and the UI-vs-marketing distinction. Load whenever an app needs a name or brand mark on screen.'
version: 1.0.0
---
# McDermott Application Lockup & Naming
Every AI app shares one identity system — the **McDermott symbol + a divider + the application name**. No bespoke per-app logos. The divider signals the app is a *function inside the master brand*, not a standalone product.

## The lockup
`[ ◯M ] │ Application Name` — three parts, left to right:
1. **Symbol** — the McDermott mark (see slot below).
2. **Divider** — a thin vertical rule. This is the master-brand cue; never omit it in UI.
3. **Name** — the application name in Georgia.

Spacing: symbol → `--space-3` → divider → `--space-3` → name. The whole lockup is one inline-flex unit, vertically centered.

## The symbol
The symbol is the single source asset, embedded inline so it travels with this file (no separate asset folder). It uses `fill="currentColor"`, so it takes the color of whatever surface it sits on — no per-theme files. This is the official McDermott vector; do not redraw, re-trace, or substitute.

```html
<svg class="lockup__symbol-svg" viewBox="0 0 171.84 171.84" xmlns="http://www.w3.org/2000/svg" fill="currentColor" role="img" aria-label="McDermott">
  <path d="M43,85.12l22.6,36.87h-22.6v-36.87ZM113.34,121.9h16.81V47.95h-16.81v73.95ZM42.17,47.95l47.09,76.79,8.38-20.04-34.79-56.75h-20.67ZM171.84,85.92c0,47.37-38.55,85.92-85.92,85.92S0,133.29,0,85.92,38.55,0,85.92,0s85.92,38.55,85.92,85.92ZM162.77,85.92c0-42.37-34.47-76.85-76.85-76.85S9.07,43.55,9.07,85.92s34.47,76.85,76.85,76.85,76.85-34.47,76.85-76.85Z"/>
</svg>
```

The path is square (`viewBox 0 0 171.84 171.84`) and carries no hardcoded width/height/color — the `.lockup__symbol` box sizes it and `currentColor` colors it. This single inline mark serves every surface and theme; there are no separate per-color asset files to manage. (Fixed-color exports — navy, blue, teal, white — exist in the brand library if ever needed outside the product, but UI always uses the `currentColor` version above.)

Sizes: **32px** in a sidebar header or top bar; **48–64px** on a login/splash screen. Never between the scale steps. Keep the symbol's circle intact — don't crop or recolor per app.

## The divider
1px wide, height matched to the lockup (min 24px), in the surface's foreground color at low opacity: `color-mix(in srgb, currentColor 22%, transparent)`. It relates to its surface automatically. Never a heavy or full-height rule.

## The name
- Font: `--font-mix` (Georgia) — web-safe, matches the brand.
- Size: 18px in headers (scale up proportionally on splash screens).
- Case: **Title Case** (see `ux-copy-and-microcopy.md`).
- Weight: regular.
- **Wraps to two lines maximum**, left-aligned, vertically centered to the symbol. If it can't fit two readable lines, the name is too long — tighten the wording, never shrink the type below readable.

## Color by surface
The lockup inherits `currentColor`; set `color` from the surface it sits on:
- **Navy sidebar** (navy in both themes): white — symbol, name, and divider derive from `--color-white`. This is the primary case.
- **Light surfaces** (top bar, light login): `--text-primary` — navy in light, white in dark.

Blue and teal symbol variants are reserved; the standard lockup is white-on-navy or navy/white-by-theme. Never place the white symbol on a light surface (invisible) or navy on navy.

## Placement — in priority order
1. **Sidebar header** (primary) — top of the sidebar, above the nav. This is the app's home for its identity. See `navigation-and-ia.md`.
2. **No sidebar?** Leading (left) position of the top bar / app header — distinct from the page-title slot, which names the current *page*, not the app. See `app-shell-and-headers.md`.
3. **Login / splash / empty first-run** — centered, at the larger symbol size.

The lockup is app identity; it appears once per surface. It does not repeat in the top bar when the sidebar already shows it.

## Responsive
- **Collapsed sidebar** (72px) or **≤360px**: show the **symbol only** — hide the divider and name. The mark alone carries identity at small sizes.
- The two-line name wrap is what lets longer, descriptive names fit; never let the name push the sidebar wider (`min-width: 0` on the lockup).

## Favicon — one family, one icon per app

**Amended (v1.9).** Every app's favicon is built from two parts: a **navy disc** (the brand constant, identical everywhere) and a **Phosphor Fill glyph naming the app's domain** (the variable, looked up in the registry below). The disc carries McDermott; the glyph makes the tab findable. Neither half is designed per app — the disc is fixed and the glyph comes from a table, so generation stays deterministic and reviewable.

**Deprecated (v1.9):** the symbol-only favicon shared by every app → use the disc + domain glyph. The old tag survives *only* as the fallback for an app whose domain is not yet registered (see Fallback).

### Why it changed
Chrome drops tab titles at roughly eight open tabs, so at portfolio scale the favicon is the last wayfinding signal standing — and a firm-wide identical mark makes every McDermott tab the same tab. The previous rule optimized for brand at exactly the density where people need to find things. The disc keeps the brand read; the glyph restores the find.

### The tag
Paste verbatim into `<head>`. Substitute **only** `GLYPH_PATH`; every other byte is fixed.

```html
<link rel="icon" type="image/svg+xml" href="data:image/svg+xml,%3Csvg%20xmlns%3D'http://www.w3.org/2000/svg'%20viewBox%3D'0%200%20256%20256'%3E%3Cstyle%3E.d{fill:%23000042}.g{fill:%23FFFFFF}@media(prefers-color-scheme:dark){.d{fill:%23FFFFFF}.g{fill:%23000042}}%3C/style%3E%3Ccircle%20class%3D'd'%20cx%3D'128'%20cy%3D'128'%20r%3D'128'/%3E%3Cg%20class%3D'g'%20transform%3D'translate(46.08,46.08)%20scale(0.64)'%3EGLYPH_PATH%3C/g%3E%3C/svg%3E">
```

`GLYPH_PATH` is the `<path>` element (or elements) lifted from that glyph's **Fill** SVG in `@phosphor-icons/core` — `assets/fill/<name>-fill.svg` — with its double quotes swapped for single quotes. Nothing else about the glyph is edited: no recolor, no redraw, no rescale.

Anatomy, for review. Never hand-tune these numbers:

| Part | Value | Why |
|---|---|---|
| Canvas | `viewBox='0 0 256 256'` | Phosphor's native grid, so glyph paths drop in unscaled |
| Disc | `circle cx=128 cy=128 r=128` | Full bleed — the filled circle is the family cue, and the loudest shape in a tab strip |
| Glyph inset | `translate(46.08,46.08) scale(0.64)` | 64 percent — about 10px of usable glyph inside a 16px favicon |
| Light chrome | navy disc `#000042`, white glyph | |
| Dark chrome | **white disc `#FFFFFF`, navy glyph** | Navy on Chrome's `#202124` strip is **1.21:1** — the circle dissolves and takes the family cue with it, leaving a bare floating glyph. Invert **both halves**, never just the glyph. |

### Weight — Fill, and only here
The glyph is **Phosphor Fill**. This is the single place in the system where Fill is legal, and it is legal because Regular does not survive the size: at ~10px a 2px monoline falls below one device pixel and greys out, and Bold's counters clog and fill in. Fill is the only weight whose silhouette reads at true size. The carve-out is scoped and documented in `iconography.md` — it does not license Fill anywhere a person can actually see 2px.

### Domain → glyph registry
Look up the **first word of the app name**. The naming convention (below) guarantees names lead with the domain, which is what makes the lookup mechanical rather than a judgment call.

| Domain | Phosphor Fill glyph | Also covers |
|---|---|---|
| **Deposition** | `ph-microphone-stage` | testimony capture, witness prep |
| **Transcript** | `ph-article` | hearing and trial transcripts |
| **Privilege** | `ph-shield-check` | privilege logs, redaction review |
| **Contract** | `ph-file-magnifying-glass` | clause search, agreement review |
| **Matter** | `ph-folder-open` | matter intake, matter management |
| **Expert** | `ph-certificate` | expert reports, credentialing |
| **Trial** | `ph-images-square` | exhibits, exhibit management |
| **Invoice** | `ph-receipt` | billing, rate review, e-billing |
| **Hearing** | `ph-gavel` | argument prep, courtroom events |
| **Conflict** | `ph-scales` | conflicts checks, clearance |
| **Patent** | `ph-lightbulb-filament` | prosecution, prior art |
| **Diligence** | `ph-list-checks` | deal checklists, closing sets |
| **Discovery** | `ph-magnifying-glass` | document review, eDiscovery |
| **Docket** | `ph-calendar-dots` | deadlines, court calendars |
| **Pitch** | `ph-presentation-chart` | BD materials, pitch decks |
| **Client** | `ph-buildings` | client profiles, relationship data |
| **Policy** | `ph-book-open-text` | internal policy, guidance |
| **Regulatory** | `ph-bank` | filings, agency submissions |
| **Lease** | `ph-house-line` | real estate, property records |
| **Correspondence** | `ph-envelope-simple` | email review, letters |
| **Design** | `ph-swatches` | the design system showcase, brand assets |

Starter registry — practice groups extend it as apps land.

### Rules
- **The registry is the source of truth.** Same domain, same glyph, every app, every generation, every repo. Never re-decide a domain that already has a row.
- **Unregistered domain:** pick the Fill glyph that most directly names the domain *object*, then **add the row in the same change**. Choosing without registering is the exact failure this table exists to prevent — it puts the decision in forty repos instead of one file.
- **Collisions are fine when they are true.** "Contract Clause Finder" and "Contract Renewal Tracker" share a glyph because they *are* siblings; the `<title>` separates them at any density where titles render. Never invent a near-miss glyph to force uniqueness.
- **Fallback:** an app shipping before its domain is registered uses the legacy symbol-only tag below. That tag is legal *only* without a registry row — an app whose domain is registered and still ships the old mark is a bug.
- **Scope is the tab.** This changes the favicon and nothing else. The lockup (Anatomy, above) is unchanged: the sidebar still shows the symbol + divider + name, and the domain glyph never appears in it.
- Chrome, Edge and Firefox render it. Safari versions that ignore SVG favicons fall back to their default — acceptable, and not a reason to add a PNG pipeline. (A deployment that genuinely needs PNG — PWA manifest, pinned tiles — exports the same disc + glyph at 32/180/512px, never a different drawing.)

### Fallback tag (unregistered domains only)
```html
<link rel="icon" type="image/svg+xml" href="data:image/svg+xml,%3Csvg%20xmlns%3D'http://www.w3.org/2000/svg'%20viewBox%3D'0%200%20171.84%20171.84'%3E%3Cstyle%3Epath{fill:%23000042}@media(prefers-color-scheme:dark){path{fill:%23FFFFFF}}%3C/style%3E%3Cpath%20d%3D'M43,85.12l22.6,36.87h-22.6v-36.87ZM113.34,121.9h16.81V47.95h-16.81v73.95ZM42.17,47.95l47.09,76.79,8.38-20.04-34.79-56.75h-20.67ZM171.84,85.92c0,47.37-38.55,85.92-85.92,85.92S0,133.29,0,85.92,38.55,0,85.92,0s85.92,38.55,85.92,85.92ZM162.77,85.92c0-42.37-34.47-76.85-76.85-76.85S9.07,43.55,9.07,85.92s34.47,76.85,76.85,76.85,76.85-34.47,76.85-76.85Z'/%3E%3C/svg%3E">
```

## Naming convention (interim — Brand to finalize)
Names are **descriptive, plain real words**, specific enough to stay distinct across hundreds of apps.
- **Lead with the domain/object, then the function:** "Deposition Summarizer", "Privilege Log Analyzer", "Contract Clause Finder".
- **Avoid bare function words** ("Indexer", "Analyzer", "Tracker") — they collide at scale.
- No invented or branded names, no acronyms, codenames, or version numbers.
- Title Case; ~2–4 words; must fit the two-line lockup.
- **UI vs marketing:** UI uses the full lockup; **Marketing/BD uses the name in text only — never the lockup.**

## Reference snippet
```html
<span class="lockup">
  <span class="lockup__symbol"><!-- inline McDermott symbol SVG, fill="currentColor" (see The symbol) --></span>
  <span class="lockup__divider" aria-hidden="true"></span>
  <span class="lockup__name">Deposition Summarizer</span>
</span>
```
```css
.lockup { display: inline-flex; align-items: center; gap: var(--space-3); min-width: 0; }
.lockup__symbol { width: 32px; height: 32px; flex-shrink: 0; }
.lockup__symbol svg { width: 100%; height: 100%; fill: currentColor; }
.lockup__divider { width: 1px; align-self: stretch; min-height: 24px; flex-shrink: 0;
  background: color-mix(in srgb, currentColor 22%, transparent); }
.lockup__name { font-family: var(--font-mix); font-size: 18px; line-height: 1.15;
  display: -webkit-box; -webkit-line-clamp: 2; -webkit-box-orient: vertical; overflow: hidden; }
/* Sidebar header: color: var(--color-white). Light surface: color: var(--text-primary). */
```

## Anti-patterns
- Subtext under the lockup — version numbers, taglines, environment tags. The lockup stands alone; version/about info lives in Settings → Support & about (see `settings-and-profile.md`)
- Omit the divider in UI (it's the master-brand cue)
- Bespoke or per-app logos · recolored or cropped symbols
- White symbol on a light surface / navy symbol on navy (invisible)
- Name in a font other than Georgia (`--font-mix`)
- Title Case dropped, or ALL-CAPS app names
- Names longer than two lines, or type shrunk to force a fit
- Bare generic function names ("Indexer") that collide at scale
- The lockup in marketing/BD materials (text-only there)
- Repeating the lockup in the top bar when the sidebar already shows it
- A missing favicon, or a bespoke/hand-drawn per-app one — the tab icon is generated from the disc + registry glyph, never designed (see Favicon)
- A Regular- or Bold-weight glyph inside the disc — Fill is the only weight that survives 16px
- A glyph chosen without adding its registry row, or a near-miss glyph invented to dodge a legitimate collision
- A navy disc left navy on dark chrome — the inversion flips both halves, not just the glyph
- The legacy symbol-only tag on an app whose domain **is** registered
