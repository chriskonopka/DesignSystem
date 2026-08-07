---
name: McDermott Authentication & Session
description: 'Use when designing login pages, sign-in flows, SSO buttons, password fields, MFA/one-time-code entry, session timeout and re-authentication, sign-out, locked accounts, or any signed-out surface. Load whenever an app needs a way in or a way out.'
version: 1.0.0
---
# McDermott Authentication & Session
The front door of every app. Users see it before anything else, so inconsistency here reads as "different product" instantly.

## The login page
A centered, single-purpose surface. No sidebar, no top bar, no navigation — nothing to do here but sign in.
- **Layout:** the lockup centered at 48–64px symbol size (per `application-lockup.md`), then a single card — `--bg-surface`, 1px `--border-light`, 2px radius, `--space-6` padding, `width: min(90vw, 400px)` — vertically centered on `--bg-page` (offset slightly above true center; optical center reads better).
- The page respects the theme like every other surface. Theme toggle is optional here; if present it sits alone at the top-right.
- One sentence of context maximum below the lockup ("Sign in to continue"). No marketing copy — this is UI, not a landing page.

## SSO first
McDermott apps authenticate through the firm's identity provider. The hierarchy is fixed:
1. **Primary button: "Continue with single sign-on"** — full-width primary button. Never a third-party vendor logo button; the firm's SSO is the product here.
2. **Email/password fallback** (only when the app genuinely supports it): separated by a 1px `--border-light` rule with a centered "or" in `--text-secondary`. Fields follow `forms-and-input.md` exactly.
3. Never social sign-in (Google/Microsoft-branded buttons) unless the platform team explicitly provisions it — and then it still uses McDermott button styles with a leading icon, not vendor brand buttons.

## Field rules (in addition to forms-and-input.md)
- Email: `type="email"`, `autocomplete="email"`, `inputmode="email"`.
- Password: `autocomplete="current-password"`, show-password toggle (`eye`/`eye-slash`). **Never disable paste** — it breaks password managers.
- New-password flows: `autocomplete="new-password"`, strength indicator is the one permitted real-time validation.
- One-time code: single input, `autocomplete="one-time-code"`, `inputmode="numeric"` — never six separate boxes (breaks paste and SMS autofill).
- Desktop may autofocus the first field; mobile must not (viewport jump).
- Submit: primary button, full width, loading state (spinner replaces label, dimensions retained) while authenticating.

## Errors — never confirm what exists
Auth errors follow what + why + how (`ux-copy-and-microcopy.md`) with one extra rule: **never reveal which part was wrong**.
- ✓ "That email and password didn't match. Try again or reset your password."
- ✗ "No account with that email" / "Wrong password" (account enumeration).
- Locked account: state the lockout plainly, name the recovery path ("Too many attempts. Try again in 15 minutes, or reset your password.").
- SSO failure: "Couldn't reach the sign-on service. Try again in a moment." — with a retry.
- Errors render as the standard inline alert above the fields (pale-orange fill, 4px `--color-error` border, navy text) — never a toast, never a browser alert.

## Session expiry & re-authentication
- **Warn before expiring:** at ~2 minutes remaining, a persistent inline banner (warning severity) with "Stay signed in" — never a modal yet, and never a countdown that causes anxiety-scrolling.
- **On expiry:** the session-timeout **modal** (per `disclosure-surfaces.md` — this is a legitimate blocking case): "Your session expired. Sign in again to continue." with a sign-in action.
- **Never discard work.** In-progress input is preserved through re-auth and restored after (`ai-uncertainty-and-errors.md` owns the principle: never discard the user's input). If preservation is impossible, say so *before* expiry, not after.
- Re-auth returns the user to where they were — never to the app home.

## Sign out
- Lives in the account menu, last item, separated by a rule (`settings-and-profile.md`). Normal menu styling — never destructive-styled.
- Signs out silently and lands on the login page with a quiet confirmation line ("You're signed out.") — not a toast on a page the user is leaving.
- "Sign out of all devices" lives in Settings → Profile & account, and *does* get a confirmation (it affects other sessions).

## Signed-out deep links
A signed-out user opening a deep link signs in and lands on **that link**, not the app home. Preserve the destination through the auth flow.

## McDermott-specific
- Card: `--bg-surface`, 1px `--border-light`, 2px radius. Lockup color follows surface rules (`application-lockup.md`).
- Buttons: standard primary/secondary variants — SSO is primary, fallback submit is primary within its section, "Forgot password" is a hyperlink, never a button.
- Splash/first-run screens share this layout with the lockup at 64px.

## Mobile
- Card goes full-width minus `--space-4` gutters below 480px; the page never scrolls horizontally.
- Input font-size ≥16px (iOS zoom), `enterkeyhint="go"` on the last field.
- Respect `env(safe-area-inset-*)` on full-screen auth layouts.
- SSO redirects return into the flow without losing the deep-link destination.

## Anti-patterns
- Error copy that confirms an account exists ("No account with that email")
- Vendor-branded SSO buttons or social-login rows McDermott didn't provision
- Disabling paste or autofill in credential fields
- Six-box one-time-code inputs
- Session expiry that destroys in-progress work, or re-auth that dumps the user at home
- Sign out styled destructive, or buried outside the account menu
- Marketing copy, imagery carousels, or testimonials on the login page
- A login card wider than 400px, or left-aligned login layouts
- Toasting auth errors (inline alert above the fields is the home)
- Countdown timers ticking per-second in the expiry warning
