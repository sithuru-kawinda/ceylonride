# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

"CeylonRiderSL" — a single-page marketing/booking site for Sri Lanka travel and vehicle hire. The whole site is one self-contained file, `index.html` (~1600 lines): inline `<style>`, markup, and inline `<script>`. There is no build step, package manager, linter, or test suite. To develop, open `index.html` in a browser (or serve the directory with any static server). Fonts and icons load from CDNs (Google Fonts, Font Awesome), so a network connection is needed to see them correctly.

## Layout of `index.html`

- `<style>` (top of file): design tokens are CSS variables in `:root` (`--navy`, `--green`, `--gold`, `--radius-*`, `--shadow-*`, `--font-*`). Reuse these rather than hard-coding colors.
- Body sections, each preceded by an `<!-- ========== NAME ========== -->` comment: navbar → hero (`#home`, background-slide carousel) → quick booking bar (`#book-now`) → detailed booking form (`#book-ride`) → trust bar → vehicles (`#vehicles`) → destinations (`#destinations`) → about → how it works → testimonials → CTA → contact (`#contact`) → footer. Nav links and drawer links rely on these section ids.
- Images (vehicles, destination galleries, `logo.png`) are local files under `images/`; destination images follow a `name-N.png` pattern. `__pycache__/` is stray and can be ignored.
- Single `<script>` at the end: hero carousel, mobile drawer, destination lightbox, date-input constraints, back-to-top, and form handlers.

## Key behaviors to know

- **There is no backend.** All three forms (`barBookingForm`, `detailedBookingForm`, `quickContactForm`) validate client-side, build a text message, and open `https://wa.me/<WHATSAPP_NUMBER>?text=...` via an inline `window.open(...)` in each submit handler. (A helper `openWhatsApp(message)` is defined near the top of the script but is currently unused.) The business number is the `WHATSAPP_NUMBER` constant at the top of the script; change it there only.
- Destination cards (`.dest-card`) carry their lightbox content in `data-title`, `data-desc`, and `data-img`, `data-img2`, `data-img3`, `data-img4` attributes; the lightbox script reads these, so new destinations need those attributes rather than extra JS.
- Pickup/return date inputs are linked by `change` listeners (return date is constrained by the pickup date) — there are two pairs: `barPickupDate`/`barReturnDate` and `dPickupDate`/`dReturnDate`.
- The page mixes `const`/`let` and `var` styles in the script; match the style of the nearby code when editing.
