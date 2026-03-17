# CLAUDE.md — Barakah Investments Group

This file documents the codebase structure, conventions, and workflows for AI assistants working on this project.

---

## Project Overview

**Barakah Investments Group** is a single-page marketing website for a halal investment and business-building LLC based in the DMV area. The site showcases the founder, company mission, and portfolio of food/lifestyle ventures.

- **Stack:** Pure HTML5 + embedded CSS + embedded Vanilla JavaScript — no build tools, no dependencies, no backend
- **Single file:** All content, styles, and scripts live in `index.html`
- **Assets:** `ali_headshot_new.jpg` (founder headshot, 828×1792px)

---

## File Structure

```
barakah-investments/
├── index.html          # Entire website (HTML + CSS + JS, ~987 lines)
├── ali_headshot_new.jpg # Founder portrait image
└── CLAUDE.md           # This file
```

---

## Development Workflow

No build step required. To preview the site:

```bash
# Option 1: Open directly in browser
open index.html

# Option 2: Serve via any static HTTP server
python3 -m http.server 8080
npx serve .
```

There are no npm scripts, Makefile targets, linters, or test suites. Changes to `index.html` are immediately reflected on reload.

---

## Page Structure

The site is a single scrollable page with 8 sections, each with an `id` for anchor navigation:

| Section         | HTML `id`   | Description                                      |
|-----------------|-------------|--------------------------------------------------|
| Navigation      | *(fixed)*   | Sticky nav bar with logo, links, CTA button      |
| Hero            | `hero`      | Opening headline and island illustration (SVG)   |
| Mission         | `mission`   | Three pillars: Halal by Design, Excellence, Value |
| Portfolio       | `portfolio` | Four venture cards (Tacos, Scoop, Melt, Hibachi) |
| Philosophy      | *(none)*    | Quote block from founder                         |
| About           | `about`     | Founder bio, headshot, values list               |
| Contact         | `contact`   | Email CTA and halal investment note              |
| Footer          | *(none)*    | Logo, nav links, copyright                       |

Nav links point to: `#mission`, `#portfolio`, `#about`, `#contact`.

---

## Design System

### CSS Variables (defined in `:root`)

```css
--green-deep:   #1A3C2E   /* Primary brand green, headers, nav */
--green-mid:    #2B5E45   /* Hover states, accents */
--green-light:  #3D8B63   /* Lighter accents */
--green-pale:   #EAF4EE   /* Section backgrounds */
--gold:         #C9973A   /* Accent highlights, badges */
--gold-light:   #F0D08A   /* Lighter gold accents */
--cream:        #FAF8F3   /* Page background */
--warm-white:   #FFFFFF   /* Card backgrounds */
--text-dark:    #1C2B22   /* Primary body text */
--text-mid:     #4A5E53   /* Secondary text */
--text-light:   #8A9E92   /* Muted/caption text */
--radius:       14px      /* Standard border radius */
--shadow:       0 4px 24px rgba(26,60,46,0.10)   /* Elevated elements */
--shadow-card:  0 2px 12px rgba(26,60,46,0.08)   /* Cards */
```

Always use these variables rather than hardcoded color values.

### Typography

- **Headlines:** `Playfair Display` (serif) — weights 400, 600, 700; also italic 400
- **Body / UI:** `Inter` (sans-serif) — weights 300, 400, 500, 600
- Loaded via Google Fonts in the `<head>`

### Responsive Breakpoints

```css
@media (max-width: 900px) { /* Tablet: stack columns, reduce padding */ }
@media (max-width: 600px) { /* Mobile: full-width layout, smaller type */ }
```

---

## Animations & JavaScript

### Scroll Fade-In (IntersectionObserver)

Elements with class `fade-up` start invisible (`opacity: 0; transform: translateY(24px)`) and become visible when scrolled into view. The observer adds the `.visible` class at a 12% intersection threshold.

To animate a new element on scroll, add `class="fade-up"` to it. The CSS transition is defined in the `<style>` block.

### Active Nav Highlighting

A `scroll` event listener tracks which section is currently in view (offset −120px from top) and applies `color: #1A3C2E; font-weight: 600` to the matching nav link. This updates inline styles directly — no classes involved.

---

## Portfolio Venture Cards

Each `.biz-card` follows this structure:

```html
<div class="biz-card fade-up">
  <div class="biz-card-header biz-card-header-{name}">
    <span>{emoji}</span>
  </div>
  <div class="biz-card-body">
    <span class="biz-card-tag tag-active">Active</span>     <!-- or tag-coming -->
    <h3>{Venture Name}</h3>
    <p>{Description}</p>
  </div>
</div>
```

Badge variants:
- `tag-active` — green badge, for live businesses
- `tag-coming` — yellow/gold badge, for upcoming ventures

Each card header has a unique gradient class (`biz-card-header-tacos`, `-scoop`, `-melt`, `-hibachi`).

---

## Logo SVG

The Barakah tree logo is an inline SVG (`viewBox="68 80 104 135"`) used in both the nav and footer. It depicts a stylized tree with a brown trunk (`#7A5C10`) and gold/amber leaf ellipses (`#C9973A`, `#E4B96A`). Do not modify this SVG unless specifically asked — it is brand-critical.

---

## Contact

- **Email:** `ticali@gmail.com` — used in the contact CTA `<a href="mailto:...">` link
- No forms, no backend, no analytics scripts

---

## Key Conventions

1. **All code lives in `index.html`** — do not split into separate CSS/JS files unless explicitly asked.
2. **Use CSS variables** for all colors and shadows.
3. **Add `fade-up` class** to any new content blocks that should animate in on scroll.
4. **Section IDs** must match nav `href` anchors exactly (e.g., `id="portfolio"` ↔ `href="#portfolio"`).
5. **No external JS libraries** — keep the site dependency-free.
6. **Halal compliance** is central to the brand — maintain this framing in all copy changes.

---

## Git

- Remote: `http://local_proxy@127.0.0.1:36375/git/ticali/barakah-investments`
- Main branch: `main`
- Development branch convention: `claude/<description>-<SESSION_ID>`
