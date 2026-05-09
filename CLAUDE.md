# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Single-file static landing page for Anneannemin Ekmekleri (Nilgün Canpolat's home bakery). No build step, no package manager, no framework — everything lives in one HTML file.

**Canonical file:** `Anneannemin Ekmekleri.html`  
**Assets:** `assets/logo.png`, `assets/logo-full.jpg`

To preview: open the HTML file directly in a browser (`open "Anneannemin Ekmekleri.html"`).

## Architecture

All CSS is in `<style>`, all JS is in `<script>` at the bottom — no external files except CDN dependencies.

**CDN dependencies (no local copies):**
- Google Fonts: Playfair Display, Montserrat, Tangerine
- `https://unpkg.com/lucide@latest/dist/umd/lucide.min.js` — call `lucide.createIcons()` after any dynamic icon insertion

**Section order in DOM:**
`NAV → HERO → GÜVEN ŞERİDİ → EKMEK GRİDİ (#menu) → HİKAYE (#hikaye) → SAKLAMA REHBERİ (#saklama) → SİPARİŞ (#siparis) → FOOTER`

## Design tokens (`:root`)

| Token | Value | Role |
|---|---|---|
| `--ae-cream` | `#FAF7F2` | Page background |
| `--ae-cream-2` | `#F4EFE6` | Subtle alternate bg |
| `--ae-bark` | `#4A3728` | Primary text / dark sections |
| `--ae-bark-soft` | `#6B523F` | Secondary text |
| `--ae-gold` | `#D4A373` | Primary accent (runtime-swappable) |
| `--ae-gold-dark` | `#B8834F` | Hover state for gold |
| `--ae-gold-soft` | `#E8C9A6` | Muted gold (hero pills, footer sig) |
| `--ae-sage` | `#8B9467` | Secondary labels / eyebrows |
| `--ae-linen` | `#E6DED3` | Borders, storage section bg |
| `--fd` | Playfair Display | Headings |
| `--fb` | Montserrat | Body / UI |
| `--fs` | Tangerine | Cursive signature |

## JS behaviours

- **Nav scroll:** adds `.scrolled` class at `scrollY > 60` — triggers frosted cream background + text color swap to bark
- **Mobile nav:** `toggleNav()` / `closeNav()` toggle `.open` on `#nav-mob`
- **Hero video fallback:** if `hero-loop.webm` errors, hides `<video>` — the `.hero-bg` background-image shows instead
- **Card CTAs:** `.card-cta[data-product]` buttons open WhatsApp with pre-filled message (`https://wa.me/905321452218`)
- **IBAN copy:** `copyIban()` uses `navigator.clipboard` with textarea fallback; calls `showToast()` on success
- **Scroll reveal:** `.rv` elements start `opacity:0; transform:translateY(22px)` — IntersectionObserver adds `.in` when visible. Stagger delays via `.rv-d1` / `.rv-d2` / `.rv-d3` (90ms / 180ms / 270ms)
- **Tweaks panel:** hidden panel (`#tw-panel`) for live accent color swap and compact mode. Listens for `postMessage` from parent frame (`__activate_edit_mode` / `__deactivate_edit_mode`). `setAccent()` writes to CSS custom properties at runtime

## Placeholder data to replace

- **Hero video:** `hero-loop.webm` doesn't exist yet — `.hero-bg` Unsplash image is the fallback
- **Product images:** all three cards use Unsplash URLs
- **Story photo:** Unsplash placeholder; replace with `src="nilgun.jpg"` when available
- **IBAN:** `TR45 0001 0023 6000 7890 1234 56` is test data
- **WhatsApp number:** `905321452218` — confirm this is current before going live
- **Instagram handle:** `anneannemin.ekmekleri` — confirm before going live

## Adding a new product card

Copy an existing `<article class="card rv">` block inside `<div class="cards">`. Add stagger class (`rv-d1`, `rv-d2`, etc.) to the article. Set `data-product` on the `.card-cta` button — this becomes the WhatsApp pre-fill text.
