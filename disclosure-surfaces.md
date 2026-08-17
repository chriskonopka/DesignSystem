---
name: McDermott Disclosure Surfaces
description: 'Use when deciding between or building modals, dialogs, side sheets, drawers, bottom sheets, popovers, dropdowns, tooltips, accordions, inline expansions, or full-page routes. Use when revealing additional content, secondary actions, or contextual UI.'
version: 1.0.0
---
# McDermott Disclosure Surfaces
Decisions about *how to reveal* additional content or actions. Pairs with `McDermott Navigation & IA` for "where things live."
## Decision matrix
Pick the surface from the **relationship between the content and what the user is doing**.
| Relationship | Surface |
|---|---|
| Must act before continuing | **Modal** — blocking, focus-trapped |
| Related to the main view; user might reference both | **Side sheet** (desktop) / **bottom sheet** (mobile) — non-blocking |
| Brief, contextual, anchored to a trigger | **Popover / dropdown** |
| Part of the page's content flow | **Inline expansion / accordion** |
| Substantial standalone task; should be back-button-able | **Full page or new route** |
## Modal
Use only when the user **must respond before continuing** (destructive confirmation, required input, session timeout).
- Centered. Max-width 560px for forms, 480px for confirmations.
- Width on narrow viewports: `width: min(90vw, 480px)` so the modal scales down without ever filling the entire screen.
- Scrim: `rgba(0, 0, 0, 0.5)` with `backdrop-filter: blur(4px)` where supported. No fallback — never block content behind a missing-blur scrim.
- Focus is trapped inside the modal; first focusable element receives focus on open.
- Close on Escape, on scrim click (unless confirming destructive action), and via explicit close button.
- A destructive confirm's action button is the same **quiet destructive variant** used everywhere (pale-orange rest fill, 3px error spine; deep-red + white on hover) — the modal's named consequence carries the weight, not a resting saturated fill (`_core-requirements.md`, Destructive actions).
- Restore focus to the triggering element on close.
- Animation: `transform: scale(0.96) → scale(1)` + opacity fade, `--duration-slow` `--ease-emphasis`.
- **Never nest modals.** If a flow needs more than one, redesign as a multi-step modal or full page.
- **Modal action row must `flex-wrap: wrap`.** Two McDermott buttons (140px min-width each + gap) won't fit a typical 256px modal-content area at narrow widths. Without `flex-wrap`, the second button gets cut off the right edge of the modal.
## Side sheet (desktop) / Bottom sheet (mobile)
Use for **secondary content related to the main view** — filter panels, item detail next to a list, contextual settings. User can reference both.
- Side sheet: 360–480px wide, slides in from the right. Slides over content; does not push.
- Bottom sheet: takes 60–90% of viewport height, slides up from bottom. Drag handle at top.
- Non-blocking: outside content remains visible and partially dismissible (no focus trap by default).
- Dismiss on outside click, Escape, swipe-down (bottom sheet), and explicit close.
- Animation: slide + opacity, `--duration-slow` `--ease-emphasis`.
## Assistant panel — docked canvas column, detachable float
The AI chat panel has two homes, and the difference is the point.
- **Docked (default): part of the canvas, never an overlay.** The panel joins the app grid as a full-height right column — `min(400px, 40vw)` wide, sticky — and the content area **reflows to make room: a push, like the sidebar's icon rail** (`navigation-and-ia.md`). No scrim, no shadow over content; everything on the canvas responds to the reclaimed width, and the user works and chats side by side.
- **Detached: floats above the canvas.** An icon-only control in the panel header (`arrows-out-simple`) pops the panel into a floating card — `min(380px, viewport)` wide, ~540px tall, 1px `--border-light`, 2px radius, `--shadow-lg` — draggable by its header and clamped to the viewport. The same control re-docks (`arrows-in-simple`). Transcript, input, and streaming state survive dock/detach — it's the same surface, repositioned.
- **Popped out (optional third tier): a separate browser window.** The detached float is a DOM element and **cannot leave the browser window** — that's a platform constraint. For multi-monitor workflows, a second header control (`arrow-square-out`) opens the assistant in a real window via `window.open` (~420×640, resizable), carrying the transcript. **The transcript syncs both ways — it is one thread, not a copy.** Messages sent in the popped window append to the same conversation as they happen, and when the window closes, the returning in-app panel shows everything said while popped out (scrolled to the latest turn). A pop-out that carries the transcript out but drops it on the way back reads as data loss. Production apps sync live between windows (BroadcastChannel or a shared store); at absolute minimum, merge the popped window's turns into the thread on close. While popped out, **the in-app panel yields**: it closes, and the sparkle trigger focuses the popped window instead of opening a second assistant — one assistant, one instance, wherever it lives. Closing the popped window returns the panel in its previous mode. Progressive enhancement: Document Picture-in-Picture (Chromium) for an always-on-top window; fall back to `window.open`. Pop-out only ever happens on a user gesture — never automatically — and like detaching, it's desktop-only.

Entry point: a `sparkle` icon button in the top-bar session cluster — slot 1 of the fixed cluster order (see *The session cluster* in `app-shell-and-headers.md`); the same button toggles it closed. That sparkle is the assistant's **only** launcher. One assistant panel per app — it never stacks or duplicates.
- **Z-order (detached):** above page chrome and popovers, below modals, scrims, and toasts. A modal opened while the panel is up must block it like everything else. (Docked, the panel is in-flow — z-order doesn't apply.)
- **State persists per user:** open/closed and docked/detached restore on return. Restoring an open panel never steals focus.
- **Escape** closes the panel only when no blocking surface (modal, drawer, sheet) is open; focus returns to the trigger.
- **Behavior inside the panel** is owned by the AI specs: streaming, input lock, Stop-in-Send-position, thinking dots per `ai-streaming-and-perceived-latency.md`; empty state and suggested chips per `ai-prompting-affordances.md`; feedback per `ai-feedback-and-correction.md`; the Generated marker per `ai-trust-and-provenance.md`.
- **Mobile (<1024px):** the canvas is too narrow to split — the docked panel falls back to a full-height overlay sheet, and **below 640px it takes over the entire screen**: a partial overlay at phone widths can't be referenced beside anyway, so it only obscures. The close control in the sheet header is the way back — always visible, never scrolled away. Detaching is desktop-only, so hide the detach control below 1024px.

## Popover / dropdown
Use for **brief contextual choices anchored to a trigger** — action menus, date pickers, "more options."
- Anchored to and visually connected to the trigger (no scrim).
- Width: content-driven, max 320px.
- **Viewport-aware max-width** to prevent off-screen extension on narrow phones: `max-width: calc(100vw - var(--space-5) * 2)`. A min-width of 220px is fine on desktop but must be capped at narrow widths.
- Auto-position: prefer below + aligned to start; flip to above if no room.
- Close on outside click, Escape, selection (for menus), trigger toggle.
- Animation: opacity fade only, `--duration-fast` `--ease-standard`.
- Returns focus to trigger on close.
- **Destructive menu items** (the menu-scale member of the destructive family): always **last**, separated by a 1px `--border-light` rule, label + icon in `--text-error` (the semantic text token — never a raw red or an alert fill), error-tinted hover (`color-mix` of `--color-error` at ~10%). Invoking one never acts directly — it opens the destructive confirm modal.
## Inline expansion / accordion
Use when content is **part of the page's flow** and shouldn't displace context — FAQs, expandable table rows, "show more" sections.
- Trigger uses Phosphor `caret-down` rotating 180° on expand.
- Animate height + opacity, `--duration-base` `--ease-standard`.
- Multiple accordion items may be open simultaneously (don't auto-collapse siblings unless space is constrained).
## Full page / new route
Use for **substantial standalone tasks** that should be back-button-able — editing a complex record, multi-step workflows, anything you'd want to share via URL.
- The browser back button must do the right thing.
- Provide an explicit close/cancel that returns the user where they came from.
## McDermott-specific treatments
- Scrim color: `rgba(0, 0, 66, 0.5)` (navy-tinted, not pure black) on light theme; `rgba(0, 0, 0, 0.6)` on dark theme.
- All surfaces use `var(--bg-surface)` background, `var(--radius)` (2px) corners.
- Border above scrim: 1px `var(--border-light)`.
- Headers in disclosure surfaces use `--font-mix` (Georgia), 24pt, sentence case.
## Anti-patterns — Never Do This
- Nest modals inside modals
- Use a modal where a side sheet would do (over-blocks the user)
- Use a popover for tasks needing real estate (use a sheet or page)
- Open a full page when a popover would do (over-disrupts context)
- Use `border-radius` > 2px on disclosure surfaces (violates McDermott radius rule)
- Skip focus trap on a modal
- Skip focus restore on close (leaves keyboard users stranded)
- Auto-dismiss a modal that confirms a destructive action
- Use scrim blur on browsers that don't support it without testing the fallback (often results in blocked unreadable content)
- Animate height with `transition: all` (jank — animate `height` and `opacity` explicitly)
- Action button row inside a modal/sheet/permission with `display: flex` but no `flex-wrap: wrap` — at narrow viewports the second 140px-min-width button gets cut off
- Popover with a `min-width` but no `max-width` cap — extends off-screen at narrow viewports
- Fork the assistant transcript across surfaces — a message sent in the popped-out window that never appears back in the in-app thread is data loss, not a display quirk
