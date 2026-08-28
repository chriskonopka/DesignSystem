---
name: McDermott Iconography
description: 'Use when adding, sizing, replacing, or designing icons; when working with Phosphor icons; when pairing icons with text; when handling icon accessibility; when animating icons; or when deciding between icon weights and styles.'
version: 1.0.0
---
# McDermott Iconography
The icon system is intentionally narrow. One source, one weight, one style. Consistency beats variety.
## Source & weight
- **Source:** Phosphor Icons **only** (`@phosphor-icons/react` in product code, web font in static prototypes only). Phosphor is the **sole licensed icon source** — never introduce another set (Font Awesome, Material, Heroicons, Lucide, custom icon fonts). Need a glyph Phosphor lacks? Build it on the 24×24 grid in Phosphor's style; don't import a library.
- **Weight:** Regular only. 2px stroke. Monoline. No fill. **One exception: the favicon** — see the carve-out below.
- **Style:** Outline, single color, rounded caps and corners.

Duotone, bold, light, and thin variants are forbidden outright. Filled is forbidden everywhere except the favicon (below). The system depends on the regular weight to read consistently across surfaces.

### Favicon carve-out (v1.9) — Fill, at 16px, in the tab only
The favicon glyph is **Phosphor Fill**. It is the single legal use of Fill in the system, and the scope is exactly that: the `<link rel="icon">` tag, nothing else.

Why the constitution bends here — Regular does not survive the size. Inside a 16px disc the glyph gets about 10px of usable width; at that scale a 2px monoline falls below one device pixel and greys out, and Bold's counters clog and fill in. Fill is the only weight whose silhouette reads at true size. Every other surface in the system renders icons at 16px or larger *with room around them*, where Regular reads correctly — so the carve-out buys legibility in the one place it is genuinely unavailable, and nowhere else.

The carve-out does **not** license Fill in buttons, badges, status rows, empty states, or anywhere a person can see 2px. A filled glyph outside the favicon tag is still the anti-pattern it always was. Canonical spec, including the exact tag and the domain glyph registry: `application-lockup.md` (Favicon).
## Sizing scale
| Size | Token / px | Use case |
|---|---|---|
| Small | 16px | Inline with body text, status badges, tight UI |
| Default | 20px | Buttons, form inputs, list items |
| Medium | 24px | Toolbar, top bar, default standalone |
| Large | 32px | Card decoration, feature highlights |
| XL | 48px | Empty states, hero illustrations |
| Max | 64px | Splash, onboarding only |

Never use sizes between these steps. Round to the nearest valid size.
## Color
- **Light theme:** `var(--icon-default)` → navy (`#000042`)
- **Dark theme:** `var(--icon-default)` → teal (`#00E2C1`)
- **On pale backgrounds** (theme-stable surfaces): always navy, never `--icon-default`. See the McDermott theme-stable foreground rule.
- **Interactive icons** inherit `var(--accent-interactive)` on hover (blue light / teal dark).
- **Disabled icons:** `opacity: 0.4`.

Never use color outside the McDermott palette.
## Pairing icons with text
- **Inline (within a sentence):** match the cap height of the surrounding text. 16px icon for 16pt text.
- **Adjacent (button, label):** align to the cap height of the text, not the baseline. Use `var(--space-2)` (8px) gap.
- **Above text (cards, tiles):** larger icon (24–48px), centered above text, `var(--space-3)` gap.
- Use `display: inline-flex` on icon wrappers so icons don't pick up text-decoration or unwanted spacing.
## Decorative vs meaningful
This is the most important accessibility distinction.
- **Decorative** (visual flourish, redundant with adjacent text): `aria-hidden="true"`. Screen readers skip them.
- **Meaningful** (conveys information not in adjacent text — e.g., icon-only button): needs an accessible label via `aria-label` or visually-hidden text.

```html
<!-- Decorative (text already says "Save") -->
<button class="btn btn-primary"><i class="ph ph-floppy-disk" aria-hidden="true"></i> Save</button>

<!-- Meaningful (icon-only button) -->
<button class="btn btn-icon" aria-label="Settings"><i class="ph ph-gear"></i></button>
```
## Icon-only buttons
- Square hit area: 36×36 minimum desktop, 44×44 on touch.
- Always have `aria-label` or `aria-labelledby`.
- Pair with a tooltip on desktop hover (delay 500ms).
- Never use icon-only for destructive actions without confirmation.
## Animation
- **Subtle motion only.** Rotation for "loading" / "refreshing", scale on hover (max 1.05), color transition on state change.
- **Duration:** `--duration-fast` (100ms) for state changes, `--duration-base` (200ms) for hover scales, `800ms` infinite for spinners.
- **Easing:** `--ease-standard` for state, `linear` for spinners.
- **Loading icon:** `ph-circle-notch` rotating linearly.
- Always wrap in `@media (prefers-reduced-motion: reduce)` and disable all but state-change transitions.
## Custom icons (rare exception)
Phosphor covers ~99% of needs. If you need a custom icon:
1. Confirm Phosphor doesn't have an equivalent (search by concept, not just name).
2. Build it on a 24×24 grid with 2px stroke, rounded caps/corners, monoline.
3. Export as inline SVG, not raster.
4. Document it in a custom-icons file and never duplicate it.
## Sourcing icons
Phosphor reference: https://phosphoricons.com/

Search by concept ("download", "user", "calendar"). The "regular" weight tab is the only valid one for UI; the "fill" tab is used solely to source a favicon glyph.
## McDermott-specific
- The McDermott rule "no `<i>` tags for icons" applies to **production React code** — use Phosphor React components there.
- Static HTML / prototypes / showcase pages may use Phosphor's web font (`<i class="ph ph-name">`) for convenience. Note this in any prototype.
- Filled Phosphor variants (e.g., `ph-fill ph-heart`) are forbidden everywhere in UI. The favicon is the one exception, and it is not UI — see the Favicon carve-out above.

## Mobile considerations
- **Icon-only buttons**: visual icon may be 18–24px, but the *tap target* must be ≥44×44 on mobile (≥24×24 minimum per WCAG 2.5.8). Pad the click area with transparent space if the visual is smaller.
- **Touch target spacing**: ≥8px between adjacent icon buttons to prevent miss-taps.
- **Active/pressed state on touch**: provide a clear pressed style since hover doesn't exist. A 0.6 opacity dim or a quick scale (0.96) signals tap.
- **Icons inline with text**: 16px on phones (matching smaller body text), 18–20px on desktop. Align to cap height of adjacent text.
- **Decorative icons in dense mobile layouts**: drop them entirely if space is tight. A line of text without an icon is better than a cramped icon competing for room.

See `responsive-and-mobile.md` for the universal checklist.

## Anti-patterns — Never Do This
- Mix Phosphor weights (regular + bold + thin in the same UI)
- Use filled (`ph-fill`) icons in UI — the favicon glyph is the one carve-out, and it is not UI
- Use a non-Phosphor icon library inline (Material Icons, Heroicons, etc.) without justification
- Skip `aria-hidden` on decorative icons (screen readers announce noise)
- Skip `aria-label` on icon-only buttons (screen reader users have nothing to read)
- Use color outside the McDermott palette for icons
- Use icons at sizes outside the scale (17px, 22px, 28px)
- Animate icons constantly (looks like a broken loading state)
- Use icons as the sole indicator of state (always pair with text or label)
- Use raster images (PNG, JPG) where an icon belongs
- Tint icons with hardcoded hex (always use `var(--icon-default)` or a token)
- Use a downward triangle for "more" — use Phosphor `caret-down` instead
