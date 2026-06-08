# McDermott Design System — Claude Design Library

The files in this folder set up the McDermott Design System inside Claude Design. Upload them once at project setup and Claude Design will reference them on every generation.

## Setup (one-time)

In Claude Design's project setup screen:

1. Click **Create new design system**
2. Under **Link code from your computer** (or "Add assets"), upload **all 7 files in this folder**
3. **Company name and blurb:**
   > McDermott Design System — Enterprise-grade navy + pale + accent visual system. Saturated colors are targeted spice, not default tools.
4. **Any other notes** — paste the short brief below

That's it. The design system is now assigned to the project and applies to every generation.

## Short brief for "Any other notes" (or to paste into Tweaks)

```
McDermott Design System non-negotiables:

1. Stepper: circles on a continuous 1px track. NEVER bordered rectangles around each step. Four states (not-started / current / completed / error), all same size, same baseline.

2. Sidebar = app navigation only (Summary, Reference, Admin). NEVER duplicate the canvas stepper in the sidebar.

3. Top bar at narrow viewports: hamburger + current page name + ONE primary action + theme/account icons. Never wrap text word-by-word. Move computed totals to the sidebar matter card.

4. Below 1024px: sidebar slides in as a drawer from the left with scrim. Never stacks above content, never disappears entirely.

5. Buttons never wrap (white-space: nowrap). Card content reflows like a standalone form — segmented controls wrap or become selects on mobile.

6. Navy + pale + accent. var(--color-teal) direct ONLY for the dark sidebar's active state and categorical chart series. Everything else interactive uses var(--accent-interactive) — blue in light mode, teal in dark.

7. Text on pale backgrounds and alert fills is ALWAYS navy, never var(--text-primary).

8. Border radius is 2px or 999px. Nothing in between.
```

## Per-generation workflow

The per-screen process is driven by the **POC-planner Design brief** (`03-CLAUDE-DESIGN-poc-prototype-brief.md`, produced by the POC-planner skill) — see the Starter Guide for the full flow. In short:

- Paste the **Design brief** once — *not* the requirements doc, which the brief replaces. Claude Design builds the first screen and waits.
- Steer through the remaining screens one at a time, in order. The brief owns the stop-and-go.
- Fix drift by steering in plain words. For recurring drift, paste the short brief above into the **Tweaks panel**.

There is no separate post-build tweak file to attach — that step has been retired.

## What's in this folder

| File | Covers |
|---|---|
| `DESIGN.md` | Master visual spec (colors, type, spacing, components) |
| `mws-design-system-showcase.html` | Live demo of every component in light and dark mode |
| `responsive-and-mobile.md` | Mobile rules — every component must work down to 320px |
| `app-shell-and-headers.md` | Top bar minimalism + sidebar drawer pattern |
| `navigation-and-ia.md` | Sidebar contents + what should NOT be in nav |
| `steppers-and-wizards.md` | Stepper spec + anti-patterns |
| `extrapolating-the-system.md` | Pre-flight compliance checklist |
