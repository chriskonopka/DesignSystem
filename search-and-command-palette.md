---
name: McDermott Search & Command Palette
description: 'Use when designing search fields, search results, typeahead, autocomplete, global search, the Cmd/Ctrl+K command palette, quick switchers, or keyboard-driven navigation. Load whenever users need to find content or jump somewhere by typing.'
version: 1.0.0
---
# McDermott Search & Command Palette
Two surfaces, two jobs. **Search** answers "find me content." The **command palette** answers "take me somewhere / do something." Don't conflate them — and never make the palette the only navigation (`navigation-and-ia.md`).

## Which one does this app need?
| Situation | Surface |
|---|---|
| Content-heavy app (documents, matters, records) | Search field, always visible |
| Deep IA + power users | Command palette (⌘K) as a supplement |
| Both | Search field visible; palette adds commands + navigation. Focusing the search field is not the palette — they stay distinct |
| Small app, shallow IA | Often neither — don't add search ceremony to five pages |

## The search field
- **Placement:** top bar, center or leading-right — never buried in a menu. It is **not** part of the session cluster (`app-shell-and-headers.md`); it sits left of the action area. Mobile: `magnifying-glass` icon in the top bar expands to a full-width field on tap.
- **Anatomy** (per `forms-and-input.md` search input): leading `magnifying-glass`, placeholder naming the scope ("Search matters…" — never bare "Search"), clear button when filled, Enter submits. Show the palette hint in the trailing slot when the app has one: `⌘K` in 12pt `--text-secondary`.
- **Typeahead popover:** opens after 2+ characters, max ~7 results, grouped by type when mixed (eyebrow-style group labels), highlighted match substring in `--accent-interactive`, footer row "View all N results" leading to the full results page. Standard popover behavior (`disclosure-surfaces.md`): Escape closes, outside click closes, focus returns.
- **Results page:** a full route (back-button-able, shareable). Standard table/list patterns apply; filtered-to-zero uses the subtle empty state (`loading-empty-and-error-states.md`), never the ceremonial one.
- Debounce requests (~200ms); never navigate on keystroke.

## The command palette (⌘K / Ctrl+K)
- **Trigger:** the keyboard shortcut, plus the search field's `⌘K` hint. No dedicated top-bar icon — the cluster is spoken for, and the palette is a keyboard surface.
- **Surface:** centered horizontally, top-anchored at ~15vh (not vertically centered — results grow downward), `width: min(640px, 90vw)`, `--bg-surface`, `--shadow-lg`, 1px `--border-light`, 2px radius, scrim behind, focus-trapped. Animation: `scale(0.98)→1` + fade, `--duration-base`.
- **Anatomy:** input first (no label, `magnifying-glass` leading, placeholder "Search or jump to…"), then grouped results: **Commands** (verb + noun, with shortcut hints right-aligned), **Pages/Sections**, **Recent**. Empty query shows Recent + top commands, never a blank panel.
- **Keyboard:** Up/Down move selection (wrapping), Enter activates, Escape closes, Tab does not leave the palette. Typing filters with fuzzy matching; matched substrings highlight in `--accent-interactive`.
- **Selection row:** 40px height, selected row gets the 3px left rail + `color-mix(in srgb, var(--accent-interactive) 12%, transparent)` tint (same selected treatment as segmented controls — `forms-and-input.md`).
- **No results:** "No matches for '{query}'" + one suggestion line — subtle, in-panel, no illustration.
- Destructive commands never execute directly from the palette — they open their normal confirmation surface.

## Accessibility
- Palette: `role="dialog"` containing a combobox pattern — input with `role="combobox"`, `aria-expanded`, `aria-controls` pointing at the listbox, `aria-activedescendant` tracking the selected option. Announce result counts politely ("6 results").
- Search typeahead: same combobox pattern anchored to the field.
- Focus returns to wherever it was when the palette closes (trigger element or canvas).
- Shortcut discoverability: the `⌘K` hint in the search field is the visible teacher; a "Keyboard shortcuts" entry in the palette itself lists the rest.

## Mobile
- Palette: full-screen sheet, not a floating panel (`ai-prompting-affordances.md` sets the precedent). Triggered from the expanded search field or a long-press affordance — never assume a hardware keyboard.
- Search field: expands from the top-bar icon to full width; collapses on clear + blur.
- Result rows ≥48px tap height; recent searches clearable individually.

## McDermott-specific
- Search field height `--control-h` (40px); palette input 48px, 16pt.
- Group labels: Sans 11pt ALL CAPS, 10% tracking, `--text-secondary`.
- Shortcut hints: 12pt `ui-monospace`, `--text-secondary`, 1px `--border-light` keycap border, 2px radius.
- Match highlight: `--accent-interactive` text, no background.

## Anti-patterns
- The palette as the only navigation, or search hidden inside a menu
- A bare "Search" placeholder that doesn't name the scope
- Navigating or executing on keystroke; auto-submitting typeahead
- Vertically-centered palette (results push it around) · palette without keyboard selection
- A dedicated top-bar palette icon crowding the session cluster
- Six results in six visual styles — group and align them
- Destructive commands executing straight from the palette
- No-results states with ceremony (illustrations, pale fills)
- Fuzzy matching so loose that unrelated results rank above exact matches
- Forgetting focus return on close (keyboard users stranded)
