# Homepage Redesign — Design Spec
**Date:** 2026-03-19
**Scope:** `index.html` and `styles.css` only (home page)

---

## Goal

Make the Kingsway Plumbing & Drainage homepage more visually stunning — on par with premium trade service sites like deltadrainage.ca. Key changes: white nav, full-bleed photo hero, premium feel throughout.

---

## Design Direction: "B — White Nav + Full-Bleed Photo Hero"
Approved by user on 2026-03-19.

---

## CSS Variables

In `:root`, add:
```css
--charcoal: #111827;
```

---

## Nav

```css
.nav {
  background: #fff;
  border-bottom: 3px solid var(--red);
  box-shadow: 0 2px 12px rgba(0,0,0,0.08);
  color: #1a1a1a;
}
.nav__brand { color: #1a1a1a; }
.nav__links a { color: #444; }
.nav__links a:hover { color: var(--red); opacity: 1; } /* override old opacity rule */
.nav__links a.active { color: var(--red); text-decoration: underline; text-underline-offset: 4px; }
.nav__phone {
  background: var(--red);
  color: #fff;
  padding: 9px 18px;
  border-radius: 5px;
  font-weight: 800;
  font-size: 0.875rem;
}
.nav__phone:hover { opacity: 0.88; }
```

**Mobile hamburger** — change span colour:
```css
/* @media (max-width: 640px) */
.nav-hamburger span { background: #1a1a1a; }
```

**Mobile open menu** — update four border declarations (was `rgba(255,255,255,*)`, now dark-on-white):
```css
/* @media (max-width: 640px) */
.nav__links         { border-top: 1px solid rgba(0,0,0,0.08); }
.nav__links a       { border-bottom: 1px solid rgba(0,0,0,0.06); color: #1a1a1a; }
.nav--open .nav__phone {
  background: transparent;
  color: var(--red);
  border-top: 1px solid rgba(0,0,0,0.08);
  padding: 13px 4px;
}
```

---

## Hero

### HTML change

In `<section class="hero">`:
1. **Remove** `<div class="hero__image">...</div>` entirely.
2. Replace `<div class="hero__text container" style="padding-left: 24px;">` with `<div class="hero__content">`. Keep all inner children unchanged.

Resulting structure:
```html
<section class="hero">
  <div class="hero__content">
    <p class="label">We Clean, Restore & Protect</p>
    <h1>Kingsway<br>Plumbing &<br>Drainage</h1>
    <p>Greater Vancouver's trusted drainage specialists.<br>Call today for a free estimate.</p>
    <a href="tel:6043626621" class="btn btn--red">Get a Free Estimate &rarr; 604-362-6621</a>
  </div>
</section>
```

### CSS change

Remove the existing `.hero__text`, `.hero__text .label`, `.hero__text h1`, `.hero__text p`, and `.hero__image` / `.hero__image img` rules.

Replace `.hero` and add new rules:
```css
.hero {
  position: relative;
  min-height: 82vh;
  display: flex;
  align-items: center;
  background: url('images/van-street.jpeg') center 40% / cover no-repeat;
}
.hero::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(100deg, rgba(0,0,0,0.80) 38%, rgba(0,0,0,0.35) 65%, rgba(0,0,0,0.08) 100%);
}
.hero__content {
  position: relative;
  z-index: 1;
  padding: 80px 32px;
  max-width: 1100px;
  margin: 0 auto;
  width: 100%;
}
.hero__content .label { color: var(--red); margin-bottom: 12px; }
.hero__content h1     { color: #fff; margin-bottom: 16px; max-width: 560px; }
.hero__content > p    { color: rgba(255,255,255,0.82); font-size: 1.1rem; margin-bottom: 32px; max-width: 480px; }
.btn--red { box-shadow: 0 4px 20px rgba(231,76,60,0.45); }
```

**Responsive** — update the `@media (max-width: 900px)` block (remove the old `hero { grid-template-columns: 1fr }` line and `hero__text` / `hero__image` overrides; add):
```css
@media (max-width: 900px) {
  .hero { min-height: 60vh; }
  .hero__content { padding: 64px 24px; }
}
@media (max-width: 640px) {
  .hero { min-height: 50vh; }
  .hero__content { padding: 48px 24px; }
}
```

---

## Service Strip

The `.service-strip` gap trick used `background: var(--border)` to colour the 2px gap between grid cells. With the new dark background, change it to `var(--charcoal)` — the 2px charcoal gap between charcoal cells is intentional (near-invisible hairline, consistent with the dark theme).

```css
.service-strip { background: var(--charcoal); }
.service-strip__item {
  background: var(--charcoal);
  border-left: none;             /* remove red left border */
  border-right: 1px solid rgba(255,255,255,0.07);
}
.service-strip__item:last-child { border-right: none; }
.service-strip__item h3 { color: #fff; }
.service-strip__item p  { color: rgba(255,255,255,0.55); }
```

---

## Why Us Section

Update `.why-us` and `.why-us p`:
```css
.why-us { text-align: left; }  /* was: center */
.why-us h2 { margin-bottom: 16px; }
.why-us p { max-width: 640px; margin-bottom: 32px; color: var(--muted); font-size: 1.05rem; }
/* margin: 0 auto removed; margin-bottom: 32px retained */
```

**HTML addition** — add after `<a href="services.html" class="btn btn--outline">View Our Services &rarr;</a>`:
```html
<div class="stats">
  <div class="stat">
    <div class="stat__num">500+</div>
    <div class="stat__label">Jobs Completed</div>
  </div>
  <div class="stat">
    <div class="stat__num">10+</div>
    <div class="stat__label">Years Experience</div>
  </div>
  <div class="stat">
    <div class="stat__num">100%</div>
    <div class="stat__label">Licensed & Insured</div>
  </div>
</div>
```

The stats block is a sibling of `.btn--outline` inside `.container` — it spans full container width (not constrained by the 640px text rule above it).

**CSS:**
```css
.stats {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 2px;
  margin-top: 48px;
  background: var(--red);
  border-radius: var(--radius);
  overflow: hidden;
}
.stat { background: #c0392b; padding: 28px 24px; text-align: center; color: #fff; }
.stat:nth-child(2) { background: var(--red); }
.stat__num   { font-size: 2.4rem; font-weight: 900; line-height: 1; margin-bottom: 6px; }
.stat__label { font-size: 0.78rem; font-weight: 700; opacity: 0.85; text-transform: uppercase; letter-spacing: 0.08em; }

@media (max-width: 640px) {
  .stats { grid-template-columns: 1fr; }
}
```

> **Note:** Numbers (500+, 10+, 100%) are placeholders — client to confirm before launch.

---

## Services Grid

```css
.services-grid { grid-template-columns: repeat(3, 1fr); } /* replace auto-fit */

.service-card {
  background: #fff;
  border-left: none;
  border-top: 3px solid var(--red);
  box-shadow: 0 2px 12px rgba(0,0,0,0.06);
}

@media (max-width: 900px) { .services-grid { grid-template-columns: repeat(2, 1fr); } }
@media (max-width: 560px) { .services-grid { grid-template-columns: 1fr; } }
```

**6th CTA card** — add as last child in `<div class="services-grid">`:
```html
<div class="service-card service-card--cta">
  <h3>Ready to book?</h3>
  <p>Call us today for a free estimate on any job, big or small.</p>
  <a href="tel:6043626621">604-362-6621 &rarr;</a>
</div>
```

```css
.service-card--cta {
  background: var(--red);
  border-top-color: #c0392b;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
}
.service-card--cta h3 { color: #fff; margin-bottom: 10px; }
.service-card--cta p  { color: rgba(255,255,255,0.85); font-size: 0.95rem; }
.service-card--cta a  { color: #fff; font-weight: 800; font-size: 0.9rem; margin-top: 16px; text-decoration: underline; text-underline-offset: 3px; }
```

---

## Photo Grid ("From the Job Site")

### HTML

Replace the existing `<div class="gallery-grid gallery-grid--4col">` block (currently 8 items) with a 7-item block. `job-09.jpeg` is intentionally removed from the home page; it remains in `gallery.html`.

```html
<div class="gallery-grid gallery-grid--home">
  <div class="gallery-grid__item ghp-1"><img src="images/van-street.jpeg" alt="Kingsway van"></div>
  <div class="gallery-grid__item ghp-2"><img src="images/job-07.jpeg" alt="Drainage work"></div>
  <div class="gallery-grid__item ghp-3"><img src="images/job-13.jpeg" alt="Drainage work"></div>
  <div class="gallery-grid__item ghp-4"><img src="images/job-05.jpeg" alt="Drainage work"></div>
  <div class="gallery-grid__item ghp-5"><img src="images/job-08.jpeg" alt="Drainage work"></div>
  <div class="gallery-grid__item ghp-6"><img src="images/van-site.jpeg" alt="Kingsway on site"></div>
  <div class="gallery-grid__item ghp-7"><img src="images/job-11.jpeg" alt="Drainage work"></div>
</div>
```

### CSS

```css
.gallery-grid--home {
  grid-template-columns: repeat(12, 1fr);
  grid-template-rows: 260px 200px;
}
.gallery-grid--home .gallery-grid__item { aspect-ratio: unset; }
.ghp-1 { grid-column: span 5; }
.ghp-2 { grid-column: span 4; }
.ghp-3, .ghp-4, .ghp-5, .ghp-6, .ghp-7 { grid-column: span 3; }

@media (max-width: 900px) {
  .gallery-grid--home {
    grid-template-columns: repeat(2, 1fr);
    grid-template-rows: unset;
  }
  .gallery-grid--home .gallery-grid__item { aspect-ratio: 4/3; }
  .ghp-1,.ghp-2,.ghp-3,.ghp-4,.ghp-5,.ghp-6,.ghp-7 { grid-column: span 1; }
}
```

The existing `gallery-grid--4col` CSS rule is now unused on the home page. It can be left or removed — no impact.

---

## CTA Banner

No changes.

---

## Footer

```css
.footer { background: var(--charcoal); }
```

**Phone link** — already has `style="color:var(--red);"` inline in HTML. Leave as-is. Do not add a class.

**Email link** — add class `footer__email` to the `<a href="mailto:...">` anchor in the HTML:
```html
<a href="mailto:info@kingswayplumbing.ca" class="footer__email">info@kingswayplumbing.ca</a>
```
```css
.footer__email { color: rgba(255,255,255,0.7); }
```

---

## What Does NOT Change

- CTA banner (stays red — intentional contrast anchor)
- All other pages (`services.html`, `gallery.html`, `about.html`, `contact.html`)
- All copy, phone number, email address
- Inner page `.page-header` (still red — only `index.html` hero changes)

---

## Files Changed

- `index.html`: hero wrapper, `.hero__image` removed, stats block, 6th service card, photo grid markup, `footer__email` class
- `styles.css`: nav, hero, service strip, stats, service cards, photo grid, footer, `--charcoal` variable, responsive updates

---

## Out of Scope

- No new pages, no backend changes, no JS beyond mobile nav colour fix
- Stats numbers are placeholders — client to confirm before launch
