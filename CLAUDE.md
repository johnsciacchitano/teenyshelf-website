# Teeny App Family — Website Build Guide

You are building a product marketing website for a macOS app in the "Teeny" family. Every site in this family shares a consistent design system, structure, and tone — but each app gets its own personality in the middle sections. Follow this document exactly when building a new site.

---

## Design System

### Fonts

```
Fraunces 700        — brand name in body text (class `.brand-name`). Load locally via @font-face from fonts/Fraunces_72pt-Bold.ttf.
DM Serif Display    — display/heading font (var `--font-display`). Google Fonts.
DM Sans (400–700)   — body/UI font (var `--font-body`). Google Fonts.
```

Google Fonts import URL:
```
https://fonts.googleapis.com/css2?family=DM+Sans:ital,wght@0,400;0,500;0,600;0,700;1,400&family=DM+Serif+Display:ital@0;1&display=swap
```

Fraunces @font-face (include in `<style>`):
```css
@font-face {
    font-family: 'Fraunces';
    src: url('fonts/Fraunces_72pt-Bold.ttf') format('truetype');
    font-weight: 700;
    font-style: normal;
    font-display: swap;
}
```

CSS variables:
```css
--font-display: 'DM Serif Display', serif;
--font-body: 'DM Sans', sans-serif;
```

### Color Palette (CSS custom properties — use these exact values)

```css
:root {
    --orange: #FF6B35;
    --orange-hover: #E85A24;
    --orange-light: #FFF3EE;
    --orange-glow: rgba(255, 107, 53, 0.15);
    --text-primary: #1A1A2E;
    --text-secondary: #5A5A72;
    --text-muted: #8E8EA0;
    --bg-white: #FFFFFF;
    --bg-cream: #FAFAF8;
    --bg-light: #F5F5F3;
    --border: #E8E8E4;
    --border-light: #F0F0EC;
}
```

Orange is the universal accent color across all Teeny apps. It is used for: primary buttons, CTA button glows, hover states on outline buttons, `<em>` accent text in hero h1, and links.

Apps may extend the palette with their own semantic colors for app-specific UI elements (e.g. green/yellow/red threshold indicators). Define these as additional CSS custom properties and use them only in the middle content sections. The core palette above stays constant.

### Shadows

```css
--shadow-sm: 0 1px 3px rgba(0,0,0,0.04), 0 1px 2px rgba(0,0,0,0.06);
--shadow-md: 0 4px 16px rgba(0,0,0,0.06), 0 2px 4px rgba(0,0,0,0.04);
--shadow-lg: 0 12px 40px rgba(0,0,0,0.08), 0 4px 12px rgba(0,0,0,0.04);
--shadow-xl: 0 20px 60px rgba(0,0,0,0.10), 0 8px 20px rgba(0,0,0,0.06);
--shadow-screenshot: 0 16px 48px rgba(0,0,0,0.12), 0 6px 16px rgba(0,0,0,0.06);
```

### Border Radii

```css
--radius-sm: 8px;   /* buttons, inputs, small cards */
--radius-md: 12px;  /* cards, tool frames */
--radius-lg: 16px;  /* screenshots, mac window frame */
--radius-xl: 20px;  /* pricing card, modal */
```

### Layout

```css
--max-width: 1200px;
```

All `.section-inner` containers are centered with `max-width: var(--max-width); margin: 0 auto;`. Sections use `padding: 90px 40px;`. Responsive breakpoints are at 900px and 640px.

---

## Page Structure (Mandatory Sections)

Every Teeny app website follows this skeleton top-to-bottom. Sections marked **FIXED** must be reproduced exactly. Sections marked **FLEX** should adapt to the specific app.

### 1. NAV — FIXED

- Position: fixed, top, full-width
- Frosted glass: `rgba(255,255,255,0.85)` background + `backdrop-filter: blur(20px) saturate(180%)`
- Bottom border: `1px solid var(--border-light)`, gains `box-shadow: var(--shadow-sm)` on scroll via JS class `.scrolled`
- Height: 64px
- Left: Logo icon (app icon, 32×32, border-radius 8px) + brand name in `Fraunces 700, 20px`
- Right: "Download App" outline button + "Purchase License: $XX.XX" primary button
- The "Download App" button links to `#download` (which anchors to the pricing section). If the app is pre-launch and not yet available for download, it should instead trigger a download modal with email capture (class `.download-trigger`).
- The "Purchase License" button links to the Polar.sh checkout URL for that app

### 2. HERO — FIXED structure, FLEX content

- Padding: `140px 40px 80px` (accounts for fixed nav)
- Centered text block (max-width 720px)
- Radial gradient accent behind: `radial-gradient(circle, rgba(255,107,53,0.04) 0%, transparent 70%)` — 800×800px, centered
- Structure:
  1. `<h1>` — DM Serif Display, `clamp(42px, 6vw, 64px)`, weight 700. Use `<em>` for the orange italic accent phrase (e.g. "For Mac")
  2. `.hero-sub` — 20px, weight 500, `--text-secondary`. One or two punchy lines.
  3. `.hero-body` — 17px, `--text-muted`, max-width 540px. Brief description including brand name (use `.tt-brand` span for Fraunces), tool count, trial info, price.
  4. `.hero-buttons` — flex row, centered, gap 14px. "Download App" outline + "Purchase License" primary, both `.btn-large`.
  5. Hero media — screenshot image and/or video below buttons. Use `border-radius: var(--radius-lg)` and `box-shadow: var(--shadow-screenshot)`.

### 3. MIDDLE CONTENT — FLEX

This is where each app's unique story lives. Design sections that showcase the app's features using patterns from this toolkit:

**Available section patterns to mix and match:**

- **Category grid** — `grid-template-columns: repeat(4, 1fr)` (2 on tablet, 2 on mobile). Cards with icon (48×48 rounded square, orange-light bg, orange stroke SVG icon), title (15px 600), subtitle (13px muted).
- **Featured media grid** — 2-column or 3-column grid of screenshots/videos inside `.tool-card-frame` containers (rounded, shadowed) with figcaptions below.
- **Mac window frame** — Dark chrome (bg `#1E1E1E`, titlebar `#2A2A2A` with red/yellow/green dots) wrapping video or screenshot. Max-width 900px.
- **Full item list** — 3-column grid of categories, each with uppercase orange header (13px, 700, tracking 1.2px, orange underline) and plain list items below.
- **Alternating showcase rows** — Two-column flex rows (text + screenshot), alternating left/right via `.reverse` class. Text side uses DM Serif Display h3 (32px), body text 16px secondary. Image side gets rounded corners + shadow.
- **Feature card grid** — `grid-template-columns: repeat(3, 1fr)` (1fr on mobile). Cards with emoji icon (28px), bold h3 (16px), description (14px secondary). White bg, 1.5px border, 14px radius. Hover: border darkens, translateY(-2px).
- **Threshold / status cards** — Centered flex row of cards with colored dots/indicators and labels. Good for apps with state or level concepts.
- **Menu bar showcase strip** — Centered flex row of small screenshots (28px height) with colored tag pills below. Good for showing menu bar variations.
- **CTA break** — centered flex row of Download + Purchase buttons between sections.
- **Any other layout that fits** — the middle is flexible. Just use the design tokens above.

**Section backgrounds alternate between `var(--bg-white)` and `var(--bg-cream)`.**

### 4. FAQ — FIXED structure, FLEX content

- Background: `var(--bg-white)`
- Padding: `100px 40px`
- Centered header: DM Serif Display `<h2>`
- `.faq` container or `.faq-list`, max-width 640–720px, centered
- Each `.faq-item` has:
  - Bottom border `1px solid var(--border)`. First item also gets a top border.
  - `.faq-question`: full-width flex, 15–16px 600, toggles `.open` class on parent (via `onclick` or `addEventListener`)
  - A `+` toggle indicator that rotates 45° when `.open`. This can be implemented as either a CSS `::after` pseudo-element or an inline SVG — either approach is fine, just be consistent within a single site.
  - `.faq-answer`: `max-height: 0` → `max-height: 200–300px` on `.open`, with overflow hidden and transition
- Adapt FAQ content to the specific app. Always include these topics (reworded per app):
  - Price / one-time payment
  - Not a subscription
  - macOS version requirement
  - Internet connection needs
  - Data privacy
  - Bug reporting (email)
  - Support contact (email)
  - Feature requests (Google Form link if applicable)

### 5. PRICING — FIXED

- Background: `var(--bg-cream)`
- Centered header: "One price. Forever."
- `.pricing-card` / `.price-card`: max-width 380–420px, centered, white bg, 1.5px border, radius-xl (20px), shadow-md to shadow-lg, padding 44–48px 36–40px
- Optional: orange top accent bar via `::before` pseudo-element (3px tall, spanning 20%–80% width). Use when it suits the app's density; omit for simpler apps.
- Contains:
  1. App icon (48–56px, radius 12–13px)
  2. `.tier` / `.pricing-label` — "Lifetime License", bold, 17px (or uppercase 13px 700 tracking 1.5px)
  3. `.price` / `.pricing-amount` — DM Serif Display, 52–56px
  4. `.price-note` / `.pricing-note` — "One-time payment. No subscription. Ever.", 14–15px muted
  5. Full-width primary button linking to Polar.sh checkout: "Purchase License: $XX.XX"
  6. Full-width outline "Download App" button (links to `#download` or triggers modal if pre-launch)
  7. `.trial-note` / `.pricing-trial` — trial details, 13px muted

### 6. FOOTER — FIXED

- Border-top `1px solid var(--border)`, padding 32px 40px, white bg
- Three-part row: brand name (left, Fraunces 700 16px) | "Made for Mac" (center, 14px muted) | email + feature request link (right, 14px muted)
- Centered links row: Privacy Policy + Terms and Conditions (link to `privacy.html` and `terms.html`)
- Copyright line: `© 2026 [AppName]. All rights reserved.`

### 7. DOWNLOAD MODAL — CONDITIONAL (pre-launch only)

Include this section only when the app is not yet available for direct download. When the app is live, "Download App" buttons should link directly to `#download` or a `.dmg` URL instead.

- Full-screen overlay: `rgba(0,0,0,0.4)` + `backdrop-filter: blur(8px)`, z-index 200
- Fades in/out via `.active` class
- Modal box: white, radius-xl, shadow-xl, padding 48px 40px, max-width 440px
- Close button: top-right, 32×32 circle, `×` character
- Content: "Coming Soon" heading (DM Serif Display 24px 700), description paragraph, email input + "Notify Me" button in a flex row
- Success state: hidden by default, shows green checkmark circle + confirmation text
- Triggered by all elements with class `.download-trigger`
- Closes on: close button click, overlay click, Escape key

---

## Buttons

Three button variants, one size modifier:

```
.btn               — base: inline-flex, 600 weight, 15px, padding 10px 22px, radius-sm, transition 0.2s
.btn-primary       — orange bg, white text, orange box-shadow. Hover: darker, bigger shadow, translateY(-1px)
.btn-outline       — transparent bg, dark text, 1.5px border. Hover: orange border, orange text, orange-light bg
.btn-ghost         — transparent, orange text, smaller padding. Hover: underline
.btn-large         — padding 14px 32px, 16px, radius-md
```

Always use `<a>` tags for buttons, not `<button>`, except inside the modal form and FAQ toggles.

---

## Animations

- `.fade-up` class: starts at `opacity: 0; transform: translateY(24px)`. Add `.visible` to animate in (`opacity: 1; transform: translateY(0)`) with `transition: 0.6s ease`.
- IntersectionObserver in JS watches all `.fade-up` elements with `threshold: 0.1, rootMargin: '0px 0px -40px 0px'`.
- Card hovers: `translateY(-2px)` or `translateY(-3px)` with shadow upgrade.
- FAQ icon rotation: `transform: rotate(45deg)` on `.open`.

---

## JavaScript (include at bottom of body)

Two blocks of JS are required on every page, plus one conditional:

1. **Nav scroll shadow** — toggle `.scrolled` class on `<nav>` when `window.scrollY > 10`
2. **FAQ toggle** — add click listeners to `.faq-question` elements that toggle `.open` on the parent `.faq-item`
3. **Download modal** (pre-launch only) — open/close logic for `#downloadModal`, email form submission, success state swap. Omit when the app has a direct download link.

---

## Responsive Breakpoints

**At 900px:**
- Grid columns reduce (4→2, 3→1, etc.)
- Section/nav padding reduces to 24px
- Hero padding: `120px 24px 60px`

**At 640px:**
- Feature grids go single column
- Hero buttons stack vertically (max-width 300px each)
- Footer row stacks vertically
- Modal form stacks vertically
- Nav outline button hides; remaining button shrinks (13px, 8px 14px padding)

---

## Brand Name Styling

Every time the app's brand name appears in body text, wrap it in `<span class="brand-name">appname</span>`. This renders the name in Fraunces 700 to distinguish it from body copy. The CSS class name `.brand-name` stays the same across all Teeny apps for consistency.

The CSS rule:
```css
.brand-name {
    font-family: 'Fraunces', serif;
    font-weight: 700;
}
```

---

## File Structure

Each app site should be a single `index.html` file with all CSS in a `<style>` tag in `<head>` and all JS in a `<script>` tag before `</body>`. No external CSS or JS files besides Google Fonts.

Include in `<head>`:
- `<meta charset>`, `<meta name="viewport">`, `<title>`, `<meta name="description">`
- Open Graph tags: `og:title`, `og:description`, `og:image`, `og:url`
- Twitter Card: `<meta name="twitter:card" content="summary_large_image">`
- Favicon: `<link rel="icon" type="image/png" href="images/favicon.png">`

Supporting pages (`privacy.html`, `terms.html`) are separate files.

Images go in an `images/` subdirectory. Use `.webp` for screenshots and `.mp4` for video demos.

---

## Checklist Before Delivering

- [ ] Google Fonts imported (DM Serif Display, DM Sans) + Fraunces loaded via @font-face
- [ ] All CSS custom properties match the core palette above exactly
- [ ] OG and Twitter Card meta tags included
- [ ] Nav is fixed with frosted glass effect and scroll shadow JS
- [ ] Nav brand name uses `.brand-name` class (Fraunces 700)
- [ ] Hero uses DM Serif Display h1 with `<em>` orange accent
- [ ] Download buttons link to `#download` or trigger modal if pre-launch
- [ ] Purchase buttons link to the correct Polar.sh URL
- [ ] FAQ uses accordion toggle pattern with `+` indicator rotating 45° on open
- [ ] Pricing card has all required elements (icon, tier label, price, note, buttons, trial note)
- [ ] Footer has brand name, "Made for Mac", email, policy links, copyright
- [ ] Download modal included only if app is pre-launch (omit if direct download available)
- [ ] Fade-up animations with IntersectionObserver (optional but recommended)
- [ ] Responsive at 768px breakpoint minimum
- [ ] Brand name uses `.brand-name` span everywhere in body text
- [ ] Single-file HTML with inline CSS and JS
- [ ] Section backgrounds alternate white/cream
