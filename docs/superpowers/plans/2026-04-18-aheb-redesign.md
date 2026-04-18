# AHEB Website Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Rewrite `index.html` as a polished editorial-theatrical single-page site with Playfair Display + Inter typography, warm gold palette, Hero-1 split layout, Cast-C leads-spotlight with clickable modal bios (via `<dialog>`), and Tickets-A classic split.

**Architecture:** Single `index.html` — all CSS inline in `<style>`, all JS inline in `<script>`, CAST data array inlined. No build step. Google Fonts via CDN `<link>`. Existing `serve.mjs` dev server untouched. Old `index.html` backed up as `index.html.bak` before work begins.

**Tech Stack:** HTML5, CSS custom properties, Vanilla JS (countdown, mobile nav, scroll-reveal, `<dialog>` modal), Google Fonts (Playfair Display + Inter).

---

## File Map

| File | Action | Purpose |
|---|---|---|
| `index.html` | **Rewrite** | Entire site — all sections, styles, JS, CAST data |
| `index.html.bak` | Create (backup) | Safety copy of the current build before changes |
| `SOURCES/Images/Logos/logo-lpud.png` | Already exists | Footer logo (LPU-D) |
| `SOURCES/Images/Logos/JC Logo.png` | Already exists | Nav logo (Junior College) |
| `SOURCES/Images/Officicial poster.jpg` | Already exists | Hero + about poster |
| `SOURCES/Images/Floor Plan.png` | Already exists | Tickets seating chart |
| `SOURCES/Images/Character Profiles/extracted/V1.png … V1 (8).png` | Already exists | Young + supporting cast portraits |
| `SOURCES/Images/Character Profiles/extracted/v2_ OLDER VERS.png … (4).png` | Already exists | Older cast portraits |
| `SOURCES/Images/Character Profiles/extracted/ima.png` | **Copy step** | Ms. Ima portrait (copied from `../../../Images/Updated Images for female partners/v2_ OLDER VERS.png`) |
| `SOURCES/Images/Character Profiles/extracted/mylene.png` | **Copy step** | Mylene portrait (copied from `../../../Images/Updated Images for female partners/v2_ OLDER VERS (2).png`) |
| `SOURCES/Images/Character Profiles/extracted/eleanor.png` | **Copy step** | Eleanor portrait (copied from `../../../Images/Updated Images for female partners/v2_ OLDER VERS (3).png`) |

**Design spec:** `docs/superpowers/specs/2026-04-18-aheb-redesign-design.md`

---

## Pre-Work: Copy Female Partner Images

**Files:**
- Create: `SOURCES/Images/Character Profiles/extracted/ima.png`
- Create: `SOURCES/Images/Character Profiles/extracted/mylene.png`
- Create: `SOURCES/Images/Character Profiles/extracted/eleanor.png`

- [ ] **Step 1: Copy images into SOURCES with readable names**

```bash
cp "../Images/Updated Images for female partners/v2_ OLDER VERS.png" \
   "SOURCES/Images/Character Profiles/extracted/ima.png"

cp "../Images/Updated Images for female partners/v2_ OLDER VERS (2).png" \
   "SOURCES/Images/Character Profiles/extracted/mylene.png"

cp "../Images/Updated Images for female partners/v2_ OLDER VERS (3).png" \
   "SOURCES/Images/Character Profiles/extracted/eleanor.png"
```

Run from the `AHEB WEBSITE]` directory. Verify the three files appear in `SOURCES/Images/Character Profiles/extracted/`.

- [ ] **Step 2: Commit**

```bash
git add "SOURCES/Images/Character Profiles/extracted/ima.png" \
        "SOURCES/Images/Character Profiles/extracted/mylene.png" \
        "SOURCES/Images/Character Profiles/extracted/eleanor.png"
git commit -m "chore: copy female partner portraits into SOURCES with readable names"
```

---

## Task 1: Back Up and Reset index.html

**Files:**
- Create: `index.html.bak`
- Modify: `index.html` (replace with new shell)

- [ ] **Step 1: Back up current index.html**

```bash
cp index.html index.html.bak
```

- [ ] **Step 2: Replace index.html with the new document shell**

Overwrite `index.html` with this full content (head + empty body — sections added in later tasks):

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Ang Huling El Bimbo — A HUMSS Musical · May 8, 2026</title>
  <meta name="description" content="Ang Huling El Bimbo — A musical theatre production by the Grade 12 HUMSS students of LPU Davao Junior College. May 8, 2026. Pre-order tickets now.">
  <!-- Open Graph -->
  <meta property="og:title" content="Ang Huling El Bimbo — A HUMSS Musical · May 8, 2026">
  <meta property="og:description" content="A musical theatre production by the Grade 12 HUMSS students of LPU Davao Junior College. May 8, 2026.">
  <meta property="og:image" content="SOURCES/Images/Officicial poster.jpg">
  <meta property="og:type" content="website">
  <!-- Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,500;0,600;0,700;1,400&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
  <style>
    /* ── Design tokens ──────────────────────────────── */
    :root {
      --bg-base:         #0d0500;
      --bg-elevated:     #1a0d05;
      --accent:          #F5C86B;
      --accent-soft:     rgba(245,200,107,0.55);
      --accent-dim:      rgba(245,200,107,0.15);
      --text-primary:    #f0d79b;
      --text-body:       rgba(200,169,110,0.78);
      --text-muted:      rgba(200,169,110,0.55);
      --silver:          #dde3ee;
      --silver-soft:     rgba(192,200,220,0.45);
      --warn-border:     rgba(220,100,0,0.5);
      --warn-bg:         rgba(220,90,0,0.07);

      --radius-sm:       6px;
      --radius-md:       12px;
      --radius-pill:     30px;

      --shadow-card:
        0 20px 50px rgba(0,0,0,0.55),
        0 4px 12px rgba(201,122,0,0.2),
        0 0 0 1px rgba(245,200,107,0.15);
      --shadow-card-hover:
        0 24px 60px rgba(0,0,0,0.65),
        0 6px 16px rgba(201,122,0,0.25),
        0 0 0 1px rgba(245,200,107,0.35);

      --ease-out-spring: cubic-bezier(0.22,1,0.36,1);
      --ease-hover:      cubic-bezier(0.34,1.56,0.64,1);
    }

    /* ── Reset ──────────────────────────────────────── */
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    html { scroll-behavior: smooth; }
    body {
      background: var(--bg-base);
      color: var(--text-primary);
      font-family: 'Inter', system-ui, sans-serif;
      line-height: 1.75;
      overflow-x: hidden;
    }

    /* ── Typography helpers ─────────────────────────── */
    .display-title {
      font-family: 'Playfair Display', Georgia, serif;
      font-size: clamp(56px, 10vw, 108px);
      font-weight: 500;
      line-height: 0.92;
      letter-spacing: -0.028em;
      color: var(--accent);
    }
    .section-heading {
      font-family: 'Playfair Display', Georgia, serif;
      font-size: clamp(28px, 4vw, 44px);
      font-weight: 600;
      line-height: 1.1;
      letter-spacing: -0.015em;
      color: var(--accent);
    }
    .section-label {
      font-size: 10px;
      font-weight: 500;
      letter-spacing: 0.28em;
      text-transform: uppercase;
      color: var(--accent-soft);
    }
    .body-text {
      font-size: 15px;
      line-height: 1.75;
      color: var(--text-body);
      max-width: 640px;
    }
    .gold-divider {
      width: 48px;
      height: 2px;
      background: linear-gradient(90deg, var(--accent) 0%, rgba(245,200,107,0.2) 70%, transparent 100%);
      margin: 10px 0 24px;
    }

    /* ── Buttons ──────────────────────────────────── */
    .btn-primary {
      display: inline-block;
      background: var(--accent);
      color: #1a0800;
      font-size: 11px;
      font-weight: 600;
      letter-spacing: 0.15em;
      text-transform: uppercase;
      padding: 14px 32px;
      border-radius: var(--radius-pill);
      text-decoration: none;
      border: none;
      cursor: pointer;
      position: relative;
      overflow: hidden;
      transition:
        transform 0.2s var(--ease-hover),
        box-shadow 0.2s ease;
    }
    .btn-primary:hover {
      transform: translateY(-3px);
      box-shadow: 0 8px 32px rgba(245,200,107,0.35), 0 2px 8px rgba(245,200,107,0.2);
    }
    .btn-primary:active  { transform: translateY(0); }
    .btn-primary:focus-visible {
      outline: 2px solid var(--accent);
      outline-offset: 3px;
    }

    .btn-ghost {
      display: inline-block;
      border: 1px solid rgba(245,200,107,0.45);
      color: rgba(245,200,107,0.88);
      font-size: 11px;
      letter-spacing: 0.13em;
      text-transform: uppercase;
      padding: 14px 28px;
      border-radius: var(--radius-pill);
      text-decoration: none;
      cursor: pointer;
      background: transparent;
      transition:
        transform 0.2s var(--ease-hover),
        border-color 0.2s ease,
        box-shadow 0.2s ease;
    }
    .btn-ghost:hover {
      transform: translateY(-3px);
      border-color: rgba(245,200,107,0.75);
      box-shadow: 0 4px 20px rgba(245,200,107,0.1);
    }
    .btn-ghost:active { transform: translateY(0); }
    .btn-ghost:focus-visible {
      outline: 2px solid rgba(245,200,107,0.5);
      outline-offset: 3px;
    }

    /* ── Layout helpers ─────────────────────────────── */
    .container {
      max-width: 1120px;
      margin: 0 auto;
      padding: 0 40px;
    }
    .site-section {
      padding: 110px 0;
      border-top: 1px solid rgba(245,200,107,0.07);
      position: relative;
    }

    /* ── Grain texture ──────────────────────────────── */
    .grain::after {
      content: '';
      position: absolute;
      inset: 0;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.75' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.04'/%3E%3C/svg%3E");
      pointer-events: none;
      z-index: 0;
    }

    /* ── Scroll-reveal ──────────────────────────────── */
    .reveal {
      opacity: 0;
      transform: translateY(28px);
      transition:
        opacity 0.7s var(--ease-out-spring),
        transform 0.7s var(--ease-out-spring);
    }
    .reveal.visible { opacity: 1; transform: translateY(0); }
    .reveal-delay-1 { transition-delay: 0.08s; }
    .reveal-delay-2 { transition-delay: 0.16s; }
    .reveal-delay-3 { transition-delay: 0.24s; }
    .reveal-delay-4 { transition-delay: 0.32s; }

    /* ── Reduced motion ─────────────────────────────── */
    @media (prefers-reduced-motion: reduce) {
      .reveal { opacity: 1; transform: none; transition: none; }
    }

    /* ── Mobile defaults ────────────────────────────── */
    @media (max-width: 640px) {
      .container  { padding: 0 20px; }
      .site-section { padding: 80px 0; }
    }
  </style>
</head>
<body>
  <!-- sections inserted by subsequent tasks -->
</body>
</html>
```

- [ ] **Step 3: Start dev server and verify blank page loads**

```bash
node serve.mjs &
```

Open `http://localhost:3000` — expect: dark `#0d0500` background, no errors in console.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: new index.html shell — tokens, fonts, reset, base styles"
```

---

## Task 2: Navigation

**Files:**
- Modify: `index.html` (add nav HTML inside `<body>`, add nav CSS inside `<style>`)

- [ ] **Step 1: Add nav CSS to the `<style>` block (before `</style>`)**

```css
/* ── Navigation ─────────────────────────────────── */
#site-nav {
  position: fixed;
  top: 0; left: 0; right: 0;
  z-index: 100;
  height: 64px;
  background: rgba(13,5,0,0.92);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  display: flex;
  align-items: center;
  transition: box-shadow 0.25s ease, background 0.25s ease;
}
.nav-inner {
  width: 100%;
  max-width: 1120px;
  margin: 0 auto;
  padding: 0 40px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 32px;
}
.nav-brand {
  display: flex;
  align-items: center;
  gap: 12px;
  text-decoration: none;
}
.nav-brand img {
  height: 36px;
  width: auto;
}
.nav-brand-text {
  font-family: 'Playfair Display', serif;
  font-size: 18px;
  font-weight: 600;
  color: var(--accent);
  letter-spacing: -0.01em;
}
.nav-links {
  display: flex;
  align-items: center;
  gap: 32px;
  list-style: none;
}
.nav-links a {
  font-size: 11px;
  font-weight: 500;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: var(--text-muted);
  text-decoration: none;
  transition: color 0.15s ease;
}
.nav-links a:hover { color: var(--accent); }
.nav-links a:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 4px;
  border-radius: 2px;
}
.nav-cta { margin-left: 8px; }

/* Hamburger */
#hamburger-btn {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
  padding: 8px;
  color: var(--accent);
}
#hamburger-btn:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 3px;
  border-radius: var(--radius-sm);
}
.hamburger-bar {
  display: block;
  width: 22px;
  height: 2px;
  background: currentColor;
  margin: 5px 0;
  transition: transform 0.2s ease, opacity 0.2s ease;
}

/* Mobile drawer */
#mobile-menu {
  display: none;
  position: fixed;
  top: 64px; left: 0; right: 0;
  background: rgba(13,5,0,0.98);
  border-top: 1px solid rgba(245,200,107,0.1);
  padding: 24px 20px 32px;
  z-index: 99;
  flex-direction: column;
  gap: 4px;
}
#mobile-menu.open { display: flex; }
#mobile-menu a {
  font-size: 13px;
  font-weight: 500;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--text-muted);
  text-decoration: none;
  padding: 12px 0;
  border-bottom: 1px solid rgba(245,200,107,0.07);
  transition: color 0.15s ease;
}
#mobile-menu a:hover { color: var(--accent); }
#mobile-menu .btn-primary { margin-top: 20px; text-align: center; display: block; }

@media (max-width: 768px) {
  .nav-links, .nav-cta { display: none; }
  #hamburger-btn { display: block; }
  .nav-inner { padding: 0 20px; }
}
```

- [ ] **Step 2: Add nav HTML as the first child of `<body>`**

```html
<!-- ══ NAV ════════════════════════════════════════════════ -->
<nav id="site-nav" role="navigation" aria-label="Main navigation">
  <div class="nav-inner">
    <a href="#hero" class="nav-brand" aria-label="AHEB — go to top">
      <img src="SOURCES/Images/Logos/JC Logo.png" alt="LPU Davao Junior College">
      <span class="nav-brand-text">AHEB</span>
    </a>
    <ul class="nav-links" role="list">
      <li><a href="#about">About</a></li>
      <li><a href="#cast">Cast</a></li>
      <li><a href="#tickets">Tickets</a></li>
      <li><a href="#venue">Venue</a></li>
    </ul>
    <a
      href="https://docs.google.com/forms/d/e/1FAIpQLSdaMbFQRliTzhKOOuyQMsnJkjWaXBnyfqlir1jmQwziyClBZg/viewform"
      class="btn-primary nav-cta"
      target="_blank" rel="noopener"
      style="font-size:10px; padding:10px 22px;"
    >Pre-Order</a>
    <button
      id="hamburger-btn"
      aria-expanded="false"
      aria-controls="mobile-menu"
      aria-label="Open navigation menu"
    >
      <span class="hamburger-bar"></span>
      <span class="hamburger-bar"></span>
      <span class="hamburger-bar"></span>
    </button>
  </div>
</nav>

<!-- Mobile nav drawer -->
<div id="mobile-menu" role="dialog" aria-label="Mobile navigation">
  <a href="#about"   onclick="closeMobileMenu()">About</a>
  <a href="#cast"    onclick="closeMobileMenu()">Cast</a>
  <a href="#tickets" onclick="closeMobileMenu()">Tickets</a>
  <a href="#venue"   onclick="closeMobileMenu()">Venue</a>
  <a
    href="https://docs.google.com/forms/d/e/1FAIpQLSdaMbFQRliTzhKOOuyQMsnJkjWaXBnyfqlir1jmQwziyClBZg/viewform"
    class="btn-primary"
    target="_blank" rel="noopener"
  >Pre-Order Tickets</a>
</div>
```

- [ ] **Step 3: Verify in browser at http://localhost:3000**

Expect: fixed dark nav at top with JC logo + "AHEB" wordmark, nav links on right, "Pre-Order" pill CTA. Resize to mobile — hamburger appears, links hide. Tap hamburger — drawer opens.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add sticky navigation with mobile hamburger drawer"
```

---

## Task 3: Hero Section

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add hero CSS inside `<style>`**

```css
/* ── Hero ───────────────────────────────────────── */
#hero {
  min-height: 100vh;
  display: flex;
  align-items: center;
  position: relative;
  overflow: hidden;
  padding-top: 64px; /* nav height */
}
.hero-bg {
  position: absolute;
  inset: 0;
  background:
    radial-gradient(ellipse at 68% 30%, rgba(200,100,0,0.42) 0%, transparent 50%),
    radial-gradient(ellipse at 15% 75%, rgba(130,50,0,0.3) 0%, transparent 50%),
    radial-gradient(ellipse at 50% 100%, rgba(80,20,0,0.5) 0%, transparent 60%),
    var(--bg-base);
}
.hero-poster {
  position: absolute;
  right: 0; top: 0; bottom: 0;
  width: 46%;
  object-fit: cover;
  object-position: center top;
  mask-image: linear-gradient(to left, rgba(0,0,0,0.65) 35%, transparent 100%);
  -webkit-mask-image: linear-gradient(to left, rgba(0,0,0,0.65) 35%, transparent 100%);
  opacity: 0.6;
}
.hero-content {
  position: relative;
  z-index: 2;
  max-width: 56%;
}
.hero-tagline {
  display: inline-block;
  font-size: 10px;
  font-weight: 500;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: var(--accent-soft);
  border: 1px solid rgba(245,200,107,0.2);
  padding: 5px 14px;
  border-radius: var(--radius-pill);
  margin-bottom: 28px;
  background: rgba(245,200,107,0.04);
}
.hero-subtitle {
  font-size: 14px;
  letter-spacing: 0.05em;
  color: var(--text-body);
  margin-top: 20px;
  margin-bottom: 36px;
  max-width: 480px;
}
.hero-btns { display: flex; gap: 14px; flex-wrap: wrap; }

@media (max-width: 768px) {
  .hero-poster { width: 100%; opacity: 0.12; }
  .hero-content { max-width: 100%; }
}
```

- [ ] **Step 2: Add hero HTML after the mobile-menu div**

```html
<!-- ══ HERO ═══════════════════════════════════════════════ -->
<section id="hero" class="grain">
  <div class="hero-bg" aria-hidden="true"></div>
  <img
    src="SOURCES/Images/Officicial poster.jpg"
    alt=""
    class="hero-poster"
    aria-hidden="true"
    loading="eager"
  >
  <div class="container">
    <div class="hero-content">
      <span class="hero-tagline reveal">May 8 · 2026 · LPU Davao</span>
      <h1 class="display-title reveal reveal-delay-1">Ang Huling<br>El Bimbo</h1>
      <p class="hero-subtitle reveal reveal-delay-2">
        A musical theatre production by the Grade 12 HUMSS students<br>
        of LPU Davao Junior College.
      </p>
      <div class="hero-btns reveal reveal-delay-3">
        <a
          href="https://docs.google.com/forms/d/e/1FAIpQLSdaMbFQRliTzhKOOuyQMsnJkjWaXBnyfqlir1jmQwziyClBZg/viewform"
          class="btn-primary"
          target="_blank" rel="noopener"
        >Pre-Order Tickets</a>
        <a href="#about" class="btn-ghost">About the Show</a>
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 3: Verify in browser**

Expect: full-height hero, dark warm background with amber glow, poster fading in from the right, large Playfair "Ang Huling / El Bimbo" in gold, subtitle, two buttons. On mobile: poster behind at low opacity.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add hero section — split layout, poster-right"
```

---

## Task 4: Countdown Band

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add countdown CSS**

```css
/* ── Countdown ──────────────────────────────────── */
#countdown {
  padding: 80px 0;
  background:
    linear-gradient(180deg, var(--bg-base) 0%, #160800 40%, #1a0900 60%, var(--bg-base) 100%);
  border-top: 1px solid rgba(245,200,107,0.12);
  border-bottom: 1px solid rgba(245,200,107,0.12);
  text-align: center;
  position: relative;
  overflow: hidden;
}
#countdown::before {
  content: '';
  position: absolute;
  top: 50%; left: 50%;
  transform: translate(-50%,-50%);
  width: 600px; height: 300px;
  background: radial-gradient(ellipse, rgba(245,200,107,0.05) 0%, transparent 70%);
  pointer-events: none;
}
.countdown-grid {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: clamp(16px,4vw,52px);
  margin-top: 24px;
  flex-wrap: wrap;
  position: relative;
}
.countdown-unit { text-align: center; min-width: 80px; }
.countdown-number {
  font-family: 'Playfair Display', serif;
  font-size: clamp(48px,9vw,80px);
  font-weight: 700;
  color: var(--accent);
  line-height: 1;
  display: block;
  text-shadow: 0 0 30px rgba(245,200,107,0.3);
}
.countdown-lbl {
  font-size: 9px;
  letter-spacing: 0.26em;
  text-transform: uppercase;
  color: rgba(245,200,107,0.38);
  margin-top: 10px;
  display: block;
}
.countdown-sep {
  font-family: 'Playfair Display', serif;
  font-size: clamp(36px,6vw,60px);
  color: rgba(245,200,107,0.15);
  line-height: 1;
  padding-bottom: 22px;
  align-self: flex-end;
}
@media (max-width: 480px) { .countdown-sep { display: none; } }
```

- [ ] **Step 2: Add countdown HTML after the hero section**

```html
<!-- ══ COUNTDOWN ══════════════════════════════════════════ -->
<div id="countdown">
  <div class="container" style="position:relative;">
    <p class="section-label">Opening Night In</p>
    <div class="countdown-grid" aria-live="polite" aria-label="Countdown to opening night">
      <div class="countdown-unit">
        <span class="countdown-number" id="cd-days">--</span>
        <span class="countdown-lbl">Days</span>
      </div>
      <span class="countdown-sep" aria-hidden="true">:</span>
      <div class="countdown-unit">
        <span class="countdown-number" id="cd-hours">--</span>
        <span class="countdown-lbl">Hours</span>
      </div>
      <span class="countdown-sep" aria-hidden="true">:</span>
      <div class="countdown-unit">
        <span class="countdown-number" id="cd-minutes">--</span>
        <span class="countdown-lbl">Minutes</span>
      </div>
      <span class="countdown-sep" aria-hidden="true">:</span>
      <div class="countdown-unit">
        <span class="countdown-number" id="cd-seconds">--</span>
        <span class="countdown-lbl">Seconds</span>
      </div>
    </div>
  </div>
</div>
```

- [ ] **Step 3: Add countdown JS (inside `<script>` at bottom of `<body>` — create the script block now)**

Add this as the first content of a new `<script>` tag just before `</body>`:

```javascript
// ── Countdown timer ──────────────────────────────────────
(function () {
  const SHOW_DATE = new Date('2026-05-08T00:00:00+08:00').getTime();
  const pad = n => String(n).padStart(2, '0');
  function tick() {
    const diff = SHOW_DATE - Date.now();
    if (diff <= 0) {
      ['cd-days','cd-hours','cd-minutes','cd-seconds']
        .forEach(id => { document.getElementById(id).textContent = '00'; });
      return;
    }
    document.getElementById('cd-days').textContent    = pad(Math.floor(diff / 86400000));
    document.getElementById('cd-hours').textContent   = pad(Math.floor((diff % 86400000) / 3600000));
    document.getElementById('cd-minutes').textContent = pad(Math.floor((diff % 3600000) / 60000));
    document.getElementById('cd-seconds').textContent = pad(Math.floor((diff % 60000) / 1000));
  }
  tick();
  setInterval(tick, 1000);
})();
```

- [ ] **Step 4: Verify in browser**

Expect: countdown band between hero and the rest of the page. Numbers tick every second. On mobile, colons hidden.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add countdown band with live JS timer"
```

---

## Task 5: About Section

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add about CSS**

```css
/* ── About ───────────────────────────────────────── */
.about-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 72px;
  align-items: center;
  margin-top: 8px;
}
.about-poster {
  width: 100%;
  border-radius: var(--radius-md);
  display: block;
  box-shadow: var(--shadow-card);
}
.advisory {
  margin-top: 28px;
  padding: 16px 20px;
  background: var(--warn-bg);
  border-left: 3px solid var(--warn-border);
  border-radius: 0 var(--radius-sm) var(--radius-sm) 0;
}
.advisory p {
  font-size: 12px;
  line-height: 1.65;
  color: rgba(220,160,100,0.85);
}
@media (max-width: 768px) {
  .about-grid { grid-template-columns: 1fr; gap: 40px; }
  .about-grid .poster-col { order: -1; }
}
```

- [ ] **Step 2: Add about HTML after the countdown div**

```html
<!-- ══ ABOUT ══════════════════════════════════════════════ -->
<section id="about" class="site-section">
  <div class="container">
    <p class="section-label reveal">About the Show</p>
    <div class="gold-divider reveal reveal-delay-1"></div>
    <div class="about-grid">
      <div class="reveal reveal-delay-1">
        <h2 class="section-heading">A Story of Love,<br>Resilience &amp; Discovery</h2>
        <p class="body-text" style="margin-top: 22px;">
          <em>Ang Huling El Bimbo</em> is a musical theatre play adapted and performed
          by the Grade 12 HUMSS students of LPU Davao. Through engaging musical numbers
          and dynamic performances, the production explores the experiences and emotions
          of young individuals navigating life's challenges — themes of love, resilience,
          and self-discovery that resonate across generations.
        </p>
        <p class="body-text" style="margin-top: 16px;">
          Presented by the Lyceum of the Philippines University Davao Junior College,
          this production marks a landmark achievement of student artistry and collaboration.
        </p>
        <div class="advisory" style="margin-top: 28px;">
          <p>
            <strong style="color: rgba(255,140,60,0.95);">Viewer Advisory:</strong>
            Contains mature themes, strobe lights, and loud sounds.
            Not suitable for very young or sensitive audiences.
          </p>
        </div>
      </div>
      <div class="poster-col reveal reveal-delay-2">
        <img
          src="SOURCES/Images/Officicial poster.jpg"
          alt="Official Ang Huling El Bimbo poster"
          class="about-poster"
          loading="lazy"
        >
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 3: Verify in browser**

Expect: two-column — copy left, poster right. Advisory callout with amber left border. On mobile, poster stacks above copy.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add about section"
```

---

## Task 6: Cast Data Array + Modal Infrastructure

**Files:**
- Modify: `index.html`

This task wires up the data and the modal `<dialog>` — the cast grid rendering comes in Task 7.

- [ ] **Step 1: Add modal CSS**

```css
/* ── Cast modal ─────────────────────────────────── */
#cast-modal {
  border: none;
  padding: 0;
  background: #1a0d05;
  border-radius: var(--radius-md);
  box-shadow: 0 40px 100px rgba(0,0,0,0.8), 0 0 0 1px rgba(245,200,107,0.2);
  max-width: min(720px, 95vw);
  width: 100%;
  max-height: 90vh;
  overflow-y: auto;
  color: var(--text-primary);
}
#cast-modal::backdrop {
  background: rgba(0,0,0,0.75);
  backdrop-filter: blur(4px);
  -webkit-backdrop-filter: blur(4px);
}
.modal-inner { position: relative; }
.modal-portrait {
  width: 100%;
  aspect-ratio: 4/3;
  object-fit: cover;
  object-position: top center;
  display: block;
}
.modal-body { padding: 28px 32px 36px; }
.modal-role {
  font-size: 10px;
  font-weight: 500;
  letter-spacing: 0.28em;
  text-transform: uppercase;
  color: var(--accent-soft);
  margin-bottom: 8px;
}
.modal-character-name {
  font-family: 'Playfair Display', serif;
  font-size: clamp(26px, 4vw, 36px);
  font-weight: 600;
  line-height: 1.1;
  letter-spacing: -0.015em;
  color: var(--accent);
}
.modal-actor-name {
  font-size: 11px;
  font-weight: 500;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: var(--text-muted);
  margin-top: 6px;
  margin-bottom: 24px;
}
.modal-divider {
  width: 40px;
  height: 2px;
  background: linear-gradient(90deg, var(--accent), transparent);
  margin-bottom: 24px;
}
.modal-bio-label {
  font-size: 9px;
  font-weight: 600;
  letter-spacing: 0.28em;
  text-transform: uppercase;
  color: var(--accent-soft);
  margin-bottom: 8px;
}
.modal-bio-text {
  font-size: 14px;
  line-height: 1.75;
  color: var(--text-body);
}
.modal-close {
  position: absolute;
  top: 14px; right: 14px;
  width: 44px; height: 44px;
  display: flex; align-items: center; justify-content: center;
  background: rgba(13,5,0,0.7);
  border: 1px solid rgba(245,200,107,0.2);
  border-radius: 50%;
  color: var(--accent);
  font-size: 18px;
  cursor: pointer;
  z-index: 10;
  transition: background 0.15s ease, border-color 0.15s ease;
}
.modal-close:hover { background: rgba(245,200,107,0.12); border-color: rgba(245,200,107,0.5); }
.modal-close:focus-visible { outline: 2px solid var(--accent); outline-offset: 3px; }
@media (max-width: 540px) {
  .modal-body { padding: 20px 20px 28px; }
}
```

- [ ] **Step 2: Add modal HTML just before `</body>`** (before the `<script>` tag)

```html
<!-- ══ CAST BIO MODAL ════════════════════════════════════ -->
<dialog id="cast-modal" aria-labelledby="modal-char-name">
  <div class="modal-inner">
    <button class="modal-close" id="modal-close-btn" aria-label="Close">&#x2715;</button>
    <img id="modal-portrait" class="modal-portrait" src="" alt="">
    <div class="modal-body">
      <p class="modal-role"    id="modal-role"></p>
      <h2 class="modal-character-name" id="modal-char-name"></h2>
      <p class="modal-actor-name"  id="modal-actor"></p>
      <div class="modal-divider"></div>
      <p class="modal-bio-label">Character</p>
      <p class="modal-bio-text" id="modal-char-bio"></p>
      <p class="modal-bio-label" style="margin-top:24px;">Performer</p>
      <p class="modal-bio-text" id="modal-actor-bio"></p>
    </div>
  </div>
</dialog>
```

- [ ] **Step 3: Add CAST data array and modal JS to the `<script>` block**

Add this after the countdown JS, still inside the same `<script>` tag:

```javascript
// ── CAST data ────────────────────────────────────────────
// CHARACTER BIO and ACTOR BIO fields: replace the placeholder strings
// with the real text from the character profiles txt file when available.
const CAST = [
  // ── Older leads (featured row) ──────────────────────────
  {
    id: 'older-hector',
    role: 'Older Hector Samala',
    actor: 'Dheb Pangandaman',
    strand: 'STEM · Reliability',
    group: 'lead',
    image: 'SOURCES/Images/Character Profiles/extracted/v2_ OLDER VERS.png',
    characterBio: '<!-- REPLACE: character bio from txt file -->',
    actorBio:     '<!-- REPLACE: actor bio from txt file -->',
  },
  {
    id: 'older-emman',
    role: 'Older Emman Azarcon',
    actor: 'Noel Vallejo',
    strand: 'STEM · Innovation',
    group: 'lead',
    image: 'SOURCES/Images/Character Profiles/extracted/v2_ OLDER VERS (2).png',
    characterBio: '<!-- REPLACE: character bio from txt file -->',
    actorBio:     '<!-- REPLACE: actor bio from txt file -->',
  },
  {
    id: 'older-anthony',
    role: 'Older Anthony Cruz Jr.',
    actor: 'Symon Castillo',
    strand: 'STEM · Efficiency',
    group: 'lead',
    image: 'SOURCES/Images/Character Profiles/extracted/v2_ OLDER VERS (3).png',
    characterBio: '<!-- REPLACE: character bio from txt file -->',
    actorBio:     '<!-- REPLACE: actor bio from txt file -->',
  },
  {
    id: 'older-joy',
    role: 'Older Joy Manawari',
    actor: 'Seven Obeso',
    strand: 'HUMSS · Inclusivity',
    group: 'lead',
    image: 'SOURCES/Images/Character Profiles/extracted/v2_ OLDER VERS (4).png',
    characterBio: '<!-- REPLACE: character bio from txt file -->',
    actorBio:     '<!-- REPLACE: actor bio from txt file -->',
  },
  // ── Young cast ─────────────────────────────────────────
  {
    id: 'younger-joy',
    role: 'Younger Joy Manawari',
    actor: 'Jullana Bernal',
    strand: 'HUMSS · Inclusivity',
    group: 'young',
    image: 'SOURCES/Images/Character Profiles/extracted/V1.png',
    characterBio: '<!-- REPLACE: character bio from txt file -->',
    actorBio:     '<!-- REPLACE: actor bio from txt file -->',
  },
  {
    id: 'younger-emman',
    role: 'Younger Emman Azarcon',
    actor: 'Ace Librero',
    strand: 'HUMSS · Inclusivity',
    group: 'young',
    image: 'SOURCES/Images/Character Profiles/extracted/V1 (2).png',
    characterBio: '<!-- REPLACE: character bio from txt file -->',
    actorBio:     '<!-- REPLACE: actor bio from txt file -->',
  },
  {
    id: 'younger-hector',
    role: 'Younger Hector Samala',
    actor: 'Marvin Quilantang',
    strand: 'STEM · Reliability',
    group: 'young',
    image: 'SOURCES/Images/Character Profiles/extracted/V1 (3).png',
    characterBio: '<!-- REPLACE: character bio from txt file -->',
    actorBio:     '<!-- REPLACE: actor bio from txt file -->',
  },
  {
    id: 'younger-anthony',
    role: 'Younger Anthony Cruz Jr.',
    actor: 'Zane Gonzales',
    strand: 'STEM · Efficiency',
    group: 'young',
    image: 'SOURCES/Images/Character Profiles/extracted/V1 (4).png',
    characterBio: '<!-- REPLACE: character bio from txt file -->',
    actorBio:     '<!-- REPLACE: actor bio from txt file -->',
  },
  // ── Supporting cast ────────────────────────────────────
  {
    id: 'arturo',
    role: 'Arturo Banlaoi',
    actor: 'Simone Estipona',
    strand: 'STEM · Reliability',
    group: 'supporting',
    image: 'SOURCES/Images/Character Profiles/extracted/V1 (5).png',
    characterBio: '<!-- REPLACE: character bio from txt file -->',
    actorBio:     '<!-- REPLACE: actor bio from txt file -->',
  },
  {
    id: 'tiya-dely',
    role: 'Tiya Dely',
    actor: 'Yzane Espedillon',
    strand: 'STEM · Innovation',
    group: 'supporting',
    image: 'SOURCES/Images/Character Profiles/extracted/V1 (6).png',
    characterBio: '<!-- REPLACE: character bio from txt file -->',
    actorBio:     '<!-- REPLACE: actor bio from txt file -->',
  },
  {
    id: 'ligaya',
    role: 'Ligaya',
    actor: 'Andreia Estrella',
    strand: 'ABM · Valor',
    group: 'supporting',
    image: 'SOURCES/Images/Character Profiles/extracted/V1 (7).png',
    characterBio: '<!-- REPLACE: character bio from txt file -->',
    actorBio:     '<!-- REPLACE: actor bio from txt file -->',
  },
  {
    id: 'andrei',
    role: 'Andrei Antonio',
    actor: 'Gio Bardos',
    strand: 'STEM · Efficiency',
    group: 'supporting',
    image: 'SOURCES/Images/Character Profiles/extracted/V1 (8).png',
    characterBio: '<!-- REPLACE: character bio from txt file -->',
    actorBio:     '<!-- REPLACE: actor bio from txt file -->',
  },
  {
    id: 'ms-ima',
    role: 'Ms. Ima',
    actor: 'Kaycee Victoria Camilotes',
    strand: '',
    group: 'supporting',
    image: 'SOURCES/Images/Character Profiles/extracted/ima.png',
    characterBio: '<!-- REPLACE: character bio from txt file -->',
    actorBio:     '<!-- REPLACE: actor bio from txt file -->',
  },
  {
    id: 'mylene',
    role: 'Mylene Azarcon',
    actor: 'Jzarylle Jaine Miras',
    strand: '',
    group: 'supporting',
    image: 'SOURCES/Images/Character Profiles/extracted/mylene.png',
    characterBio: '<!-- REPLACE: character bio from txt file -->',
    actorBio:     '<!-- REPLACE: actor bio from txt file -->',
  },
  {
    id: 'eleanor',
    role: 'Eleanor Cruz',
    actor: 'Mekylla Gubac',
    strand: '',
    group: 'supporting',
    image: 'SOURCES/Images/Character Profiles/extracted/eleanor.png',
    characterBio: '<!-- REPLACE: character bio from txt file -->',
    actorBio:     '<!-- REPLACE: actor bio from txt file -->',
  },
];

// ── Cast modal logic ─────────────────────────────────────
const castModal     = document.getElementById('cast-modal');
const modalClose    = document.getElementById('modal-close-btn');
let lastFocusedCard = null;

function openCastModal(id) {
  const member = CAST.find(c => c.id === id);
  if (!member) return;

  document.getElementById('modal-portrait').src = member.image;
  document.getElementById('modal-portrait').alt = `${member.role} — ${member.actor}`;
  document.getElementById('modal-role').textContent       = member.role;
  document.getElementById('modal-char-name').textContent  = member.role;
  document.getElementById('modal-actor').textContent      =
    member.strand ? `${member.actor} · ${member.strand}` : member.actor;
  document.getElementById('modal-char-bio').textContent   = member.characterBio;
  document.getElementById('modal-actor-bio').textContent  = member.actorBio;

  lastFocusedCard = document.activeElement;
  document.body.style.overflow = 'hidden';
  castModal.showModal();
  modalClose.focus();
}

function closeCastModal() {
  castModal.close();
  document.body.style.overflow = '';
  if (lastFocusedCard) lastFocusedCard.focus();
}

modalClose.addEventListener('click', closeCastModal);
castModal.addEventListener('click', e => {
  // close on backdrop click (click lands on <dialog> itself, not .modal-inner)
  if (e.target === castModal) closeCastModal();
});
castModal.addEventListener('cancel', e => {
  e.preventDefault();
  closeCastModal();
});
```

- [ ] **Step 4: Verify no console errors at http://localhost:3000**

Open DevTools console — expect zero errors. The dialog is hidden; no cast cards yet.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add cast data array and modal dialog infrastructure"
```

---

## Task 7: Cast Section HTML

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add cast CSS**

```css
/* ── Cast section ───────────────────────────────── */
#cast {
  background:
    radial-gradient(ellipse at 50% 0%, rgba(140,60,0,0.14) 0%, transparent 60%),
    var(--bg-base);
}

/* Lead cards (4 older protagonists) */
.leads-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
  margin-top: 32px;
}
.lead-card {
  background: rgba(255,255,255,0.025);
  border: 1px solid rgba(245,200,107,0.15);
  border-radius: var(--radius-md);
  overflow: hidden;
  cursor: pointer;
  text-align: left;
  width: 100%;
  padding: 0;
  transition:
    transform 0.25s var(--ease-out-spring),
    box-shadow 0.25s ease,
    border-color 0.25s ease;
}
.lead-card:hover {
  transform: translateY(-5px);
  box-shadow: var(--shadow-card-hover);
  border-color: rgba(245,200,107,0.4);
}
.lead-card:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 3px;
}
.lead-card img {
  width: 100%;
  aspect-ratio: 3 / 4;
  object-fit: cover;
  object-position: top center;
  display: block;
  transition: transform 0.4s var(--ease-out-spring);
}
.lead-card:hover img { transform: scale(1.03); }
.lead-body { padding: 14px 16px 18px; }
.lead-role {
  font-size: 9px;
  font-weight: 500;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: var(--accent-soft);
  margin-bottom: 4px;
}
.lead-name {
  font-family: 'Playfair Display', serif;
  font-size: 17px;
  color: var(--accent);
  line-height: 1.2;
}
.lead-actor {
  font-size: 9px;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--text-muted);
  margin-top: 5px;
}
.lead-teaser {
  font-size: 12px;
  line-height: 1.55;
  color: var(--text-body);
  margin-top: 8px;
  opacity: 0.8;
}

/* Supporting grid */
.supporting-heading {
  font-family: 'Playfair Display', serif;
  font-size: 22px;
  color: var(--accent);
  margin-top: 64px;
  margin-bottom: 8px;
}
.cast-grid {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 14px;
  margin-top: 24px;
}
.cast-group-label {
  grid-column: 1 / -1;
  font-size: 9px;
  font-weight: 500;
  letter-spacing: 0.28em;
  text-transform: uppercase;
  color: rgba(245,200,107,0.35);
  padding-bottom: 6px;
  border-bottom: 1px solid rgba(245,200,107,0.07);
  margin-top: 16px;
}
.cast-group-label:first-child { margin-top: 0; }
.cast-card {
  background: rgba(255,255,255,0.02);
  border: 1px solid rgba(245,200,107,0.1);
  border-radius: var(--radius-sm);
  overflow: hidden;
  cursor: pointer;
  text-align: left;
  width: 100%;
  padding: 0;
  transition:
    transform 0.22s var(--ease-out-spring),
    box-shadow 0.22s ease,
    border-color 0.22s ease;
}
.cast-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 12px 30px rgba(0,0,0,0.45), 0 0 0 1px rgba(245,200,107,0.3);
  border-color: rgba(245,200,107,0.3);
}
.cast-card:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 3px;
}
.cast-card img {
  width: 100%;
  aspect-ratio: 3 / 4;
  object-fit: cover;
  object-position: top center;
  display: block;
}
.cast-body { padding: 10px 12px 12px; }
.cast-character {
  font-family: 'Playfair Display', serif;
  font-size: 13px;
  color: var(--accent);
  line-height: 1.25;
}
.cast-actor {
  font-size: 8px;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--text-muted);
  margin-top: 4px;
}

/* Responsive */
@media (max-width: 1080px) {
  .cast-grid { grid-template-columns: repeat(4, 1fr); }
}
@media (max-width: 900px) {
  .leads-grid { grid-template-columns: repeat(2, 1fr); }
}
@media (max-width: 780px) {
  .cast-grid { grid-template-columns: repeat(3, 1fr); }
}
@media (max-width: 540px) {
  .leads-grid { grid-template-columns: 1fr; }
  .cast-grid  { grid-template-columns: repeat(2, 1fr); gap: 10px; }
}
```

- [ ] **Step 2: Add cast section HTML after the about section**

```html
<!-- ══ CAST ═══════════════════════════════════════════════ -->
<section id="cast" class="site-section">
  <div class="container">
    <p class="section-label reveal">The Cast</p>
    <div class="gold-divider reveal reveal-delay-1"></div>
    <h2 class="section-heading reveal reveal-delay-1">Meet the Performers</h2>
    <p class="body-text reveal reveal-delay-2" style="margin-top: 14px;">
      Grade 12 HUMSS brings the story to life through music, acting, and dance.
      Click any portrait to read their full character and performer bio.
    </p>

    <!-- Featured: 4 older-era leads -->
    <div class="leads-grid">
      <button class="lead-card reveal reveal-delay-1" data-cast-id="older-hector" onclick="openCastModal('older-hector')" type="button">
        <img src="SOURCES/Images/Character Profiles/extracted/v2_ OLDER VERS.png"
             alt="Older Hector Samala — Dheb Pangandaman" loading="lazy" decoding="async">
        <div class="lead-body">
          <p class="lead-role">Older Hector Samala</p>
          <p class="lead-name">Dheb<br>Pangandaman</p>
          <p class="lead-actor">STEM · Reliability</p>
        </div>
      </button>
      <button class="lead-card reveal reveal-delay-2" data-cast-id="older-emman" onclick="openCastModal('older-emman')" type="button">
        <img src="SOURCES/Images/Character Profiles/extracted/v2_ OLDER VERS (2).png"
             alt="Older Emman Azarcon — Noel Vallejo" loading="lazy" decoding="async">
        <div class="lead-body">
          <p class="lead-role">Older Emman Azarcon</p>
          <p class="lead-name">Noel<br>Vallejo</p>
          <p class="lead-actor">STEM · Innovation</p>
        </div>
      </button>
      <button class="lead-card reveal reveal-delay-3" data-cast-id="older-anthony" onclick="openCastModal('older-anthony')" type="button">
        <img src="SOURCES/Images/Character Profiles/extracted/v2_ OLDER VERS (3).png"
             alt="Older Anthony Cruz Jr. — Symon Castillo" loading="lazy" decoding="async">
        <div class="lead-body">
          <p class="lead-role">Older Anthony Cruz Jr.</p>
          <p class="lead-name">Symon<br>Castillo</p>
          <p class="lead-actor">STEM · Efficiency</p>
        </div>
      </button>
      <button class="lead-card reveal reveal-delay-4" data-cast-id="older-joy" onclick="openCastModal('older-joy')" type="button">
        <img src="SOURCES/Images/Character Profiles/extracted/v2_ OLDER VERS (4).png"
             alt="Older Joy Manawari — Seven Obeso" loading="lazy" decoding="async">
        <div class="lead-body">
          <p class="lead-role">Older Joy Manawari</p>
          <p class="lead-name">Seven<br>Obeso</p>
          <p class="lead-actor">HUMSS · Inclusivity</p>
        </div>
      </button>
    </div>

    <!-- Supporting cast grid -->
    <h3 class="supporting-heading reveal">Supporting Cast</h3>
    <div class="gold-divider reveal reveal-delay-1"></div>

    <div class="cast-grid">
      <div class="cast-group-label">Young Cast</div>

      <button class="cast-card reveal reveal-delay-1" data-cast-id="younger-joy" onclick="openCastModal('younger-joy')" type="button">
        <img src="SOURCES/Images/Character Profiles/extracted/V1.png" alt="Younger Joy Manawari — Jullana Bernal" loading="lazy" decoding="async">
        <div class="cast-body"><p class="cast-character">Younger Joy Manawari</p><p class="cast-actor">Jullana Bernal</p></div>
      </button>
      <button class="cast-card reveal reveal-delay-2" data-cast-id="younger-emman" onclick="openCastModal('younger-emman')" type="button">
        <img src="SOURCES/Images/Character Profiles/extracted/V1 (2).png" alt="Younger Emman Azarcon — Ace Librero" loading="lazy" decoding="async">
        <div class="cast-body"><p class="cast-character">Younger Emman Azarcon</p><p class="cast-actor">Ace Librero</p></div>
      </button>
      <button class="cast-card reveal reveal-delay-3" data-cast-id="younger-hector" onclick="openCastModal('younger-hector')" type="button">
        <img src="SOURCES/Images/Character Profiles/extracted/V1 (3).png" alt="Younger Hector Samala — Marvin Quilantang" loading="lazy" decoding="async">
        <div class="cast-body"><p class="cast-character">Younger Hector Samala</p><p class="cast-actor">Marvin Quilantang</p></div>
      </button>
      <button class="cast-card reveal reveal-delay-4" data-cast-id="younger-anthony" onclick="openCastModal('younger-anthony')" type="button">
        <img src="SOURCES/Images/Character Profiles/extracted/V1 (4).png" alt="Younger Anthony Cruz Jr. — Zane Gonzales" loading="lazy" decoding="async">
        <div class="cast-body"><p class="cast-character">Younger Anthony Cruz Jr.</p><p class="cast-actor">Zane Gonzales</p></div>
      </button>

      <div class="cast-group-label">Supporting</div>

      <button class="cast-card reveal reveal-delay-1" data-cast-id="arturo" onclick="openCastModal('arturo')" type="button">
        <img src="SOURCES/Images/Character Profiles/extracted/V1 (5).png" alt="Arturo Banlaoi — Simone Estipona" loading="lazy" decoding="async">
        <div class="cast-body"><p class="cast-character">Arturo Banlaoi</p><p class="cast-actor">Simone Estipona</p></div>
      </button>
      <button class="cast-card reveal reveal-delay-2" data-cast-id="tiya-dely" onclick="openCastModal('tiya-dely')" type="button">
        <img src="SOURCES/Images/Character Profiles/extracted/V1 (6).png" alt="Tiya Dely — Yzane Espedillon" loading="lazy" decoding="async">
        <div class="cast-body"><p class="cast-character">Tiya Dely</p><p class="cast-actor">Yzane Espedillon</p></div>
      </button>
      <button class="cast-card reveal reveal-delay-3" data-cast-id="ligaya" onclick="openCastModal('ligaya')" type="button">
        <img src="SOURCES/Images/Character Profiles/extracted/V1 (7).png" alt="Ligaya — Andreia Estrella" loading="lazy" decoding="async">
        <div class="cast-body"><p class="cast-character">Ligaya</p><p class="cast-actor">Andreia Estrella</p></div>
      </button>
      <button class="cast-card reveal reveal-delay-4" data-cast-id="andrei" onclick="openCastModal('andrei')" type="button">
        <img src="SOURCES/Images/Character Profiles/extracted/V1 (8).png" alt="Andrei Antonio — Gio Bardos" loading="lazy" decoding="async">
        <div class="cast-body"><p class="cast-character">Andrei Antonio</p><p class="cast-actor">Gio Bardos</p></div>
      </button>
      <button class="cast-card reveal reveal-delay-1" data-cast-id="ms-ima" onclick="openCastModal('ms-ima')" type="button">
        <img src="SOURCES/Images/Character Profiles/extracted/ima.png" alt="Ms. Ima — Kaycee Victoria Camilotes" loading="lazy" decoding="async">
        <div class="cast-body"><p class="cast-character">Ms. Ima</p><p class="cast-actor">Kaycee Victoria Camilotes</p></div>
      </button>
      <button class="cast-card reveal reveal-delay-2" data-cast-id="mylene" onclick="openCastModal('mylene')" type="button">
        <img src="SOURCES/Images/Character Profiles/extracted/mylene.png" alt="Mylene Azarcon — Jzarylle Jaine Miras" loading="lazy" decoding="async">
        <div class="cast-body"><p class="cast-character">Mylene Azarcon</p><p class="cast-actor">Jzarylle Jaine Miras</p></div>
      </button>
      <button class="cast-card reveal reveal-delay-3" data-cast-id="eleanor" onclick="openCastModal('eleanor')" type="button">
        <img src="SOURCES/Images/Character Profiles/extracted/eleanor.png" alt="Eleanor Cruz — Mekylla Gubac" loading="lazy" decoding="async">
        <div class="cast-body"><p class="cast-character">Eleanor Cruz</p><p class="cast-actor">Mekylla Gubac</p></div>
      </button>
    </div>
  </div>
</section>
```

- [ ] **Step 3: Verify in browser**

Expect: 4 lead cards in a row with large portraits + Playfair names; below: two-group supporting grid. Click any card → modal opens with portrait, role, name, actor (bio fields show placeholder text). Press Esc or click ✕ → modal closes, focus returns to the card.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add cast section — leads spotlight + supporting grid + bio modal"
```

---

## Task 8: Tickets Section

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add tickets CSS**

```css
/* ── Tickets ─────────────────────────────────────── */
.tickets-layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 60px;
  align-items: start;
  margin-top: 40px;
}
.tier-stack { display: flex; flex-direction: column; gap: 14px; }
.tier-card {
  border: 1px solid rgba(245,200,107,0.18);
  border-radius: var(--radius-md);
  padding: 20px 24px;
  background: rgba(245,200,107,0.03);
  position: relative;
  transition: border-color 0.2s ease;
}
.tier-card.gold {
  border-color: rgba(245,200,107,0.5);
  background: rgba(245,200,107,0.07);
  box-shadow: 0 0 32px rgba(245,200,107,0.06), inset 0 1px 0 rgba(245,200,107,0.15);
}
.tier-card.silver {
  border-color: var(--silver-soft);
  background: rgba(192,200,220,0.05);
}
.tier-card.bronze {
  border-color: rgba(200,150,90,0.2);
  background: rgba(200,150,90,0.03);
  opacity: 0.6;
}
.tier-badge {
  font-size: 9px;
  font-weight: 500;
  letter-spacing: 0.24em;
  text-transform: uppercase;
  color: var(--accent-soft);
  margin-bottom: 8px;
}
.tier-badge.silver-badge { color: rgba(192,210,240,0.6); }
.tier-badge.bronze-badge { color: rgba(200,150,90,0.7); }
.tier-price {
  font-family: 'Playfair Display', serif;
  font-size: 28px;
  font-weight: 600;
  color: var(--accent);
  line-height: 1;
}
.tier-price.silver-price { color: var(--silver); }
.tier-price.bronze-price { color: rgba(200,150,90,0.8); font-size: 16px; margin-top: 4px; }
.tier-seats {
  font-size: 11px;
  color: var(--text-muted);
  margin-top: 5px;
}
.showtime-row {
  display: flex;
  gap: 10px;
  margin-top: 24px;
}
.showtime-pill {
  flex: 1;
  border: 1px solid rgba(245,200,107,0.2);
  border-radius: var(--radius-md);
  padding: 14px 16px;
}
.showtime-time {
  font-family: 'Playfair Display', serif;
  font-size: 18px;
  color: var(--accent);
  display: block;
}
.showtime-arrive {
  display: block;
  font-size: 10px;
  line-height: 1.6;
  color: var(--text-muted);
  margin-top: 4px;
}
.payment-note {
  font-size: 12px;
  line-height: 1.75;
  color: var(--text-muted);
  margin-top: 18px;
  padding: 14px 18px;
  border: 1px solid rgba(245,200,107,0.1);
  border-radius: var(--radius-sm);
  background: rgba(245,200,107,0.02);
}
.floor-plan-box {
  border: 1px solid rgba(245,200,107,0.15);
  border-radius: var(--radius-md);
  overflow: hidden;
  background: rgba(0,0,0,0.25);
}
.floor-plan-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 14px 18px;
  border-bottom: 1px solid rgba(245,200,107,0.1);
}
.floor-plan-title {
  font-family: 'Playfair Display', serif;
  font-size: 14px;
  color: var(--accent);
}
.floor-plan-legend {
  display: flex;
  gap: 16px;
}
.legend-item {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 9px;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--text-muted);
}
.legend-dot {
  width: 8px; height: 8px;
  border-radius: 50%;
  display: inline-block;
}
.floor-plan-img-wrap img {
  width: 100%;
  display: block;
}
@media (max-width: 860px) {
  .tickets-layout { grid-template-columns: 1fr; gap: 40px; }
  .showtime-row { flex-direction: column; }
}
```

- [ ] **Step 2: Add tickets HTML after the cast section**

```html
<!-- ══ TICKETS ════════════════════════════════════════════ -->
<section id="tickets" class="site-section">
  <div class="container">
    <p class="section-label reveal">Get Your Seats</p>
    <div class="gold-divider reveal reveal-delay-1"></div>
    <h2 class="section-heading reveal reveal-delay-1">Tickets &amp; Pre-Order</h2>

    <div class="tickets-layout">
      <!-- Left: tiers + showtimes + policy + CTA -->
      <div class="reveal reveal-delay-2">
        <div class="tier-stack">
          <div class="tier-card gold">
            <div class="tier-badge">Gold Tier</div>
            <div class="tier-price">₱250</div>
            <div class="tier-seats">300 seats · closer to the stage</div>
          </div>
          <div class="tier-card silver">
            <div class="tier-badge silver-badge">Silver Tier</div>
            <div class="tier-price silver-price">₱200</div>
            <div class="tier-seats">400 seats · elevated view</div>
          </div>
          <div class="tier-card bronze">
            <div class="tier-badge bronze-badge">Bronze Tier</div>
            <div class="tier-price bronze-price">Coming soon</div>
          </div>
        </div>

        <div class="showtime-row" style="margin-top: 28px;">
          <div class="showtime-pill">
            <span class="showtime-time">5:30 PM</span>
            <span class="showtime-arrive">Arrive by 4:00 PM<br>Entry closes 6:00 PM</span>
          </div>
          <div class="showtime-pill">
            <span class="showtime-time">9:00 PM</span>
            <span class="showtime-arrive">Arrive by 8:30 PM<br>Entry closes 9:30 PM</span>
          </div>
        </div>

        <div class="payment-note">
          <strong style="color: var(--text-primary);">GCash only</strong> &nbsp;·&nbsp;
          Tickets are <strong style="color: var(--text-primary);">non-refundable</strong><br>
          Free seating — first come, first served<br>
          Confirmation sent via email, phone, or Facebook
        </div>

        <div style="margin-top: 32px;">
          <a
            href="https://docs.google.com/forms/d/e/1FAIpQLSdaMbFQRliTzhKOOuyQMsnJkjWaXBnyfqlir1jmQwziyClBZg/viewform"
            class="btn-primary"
            target="_blank" rel="noopener"
          >Pre-Order via Google Form &rarr;</a>
        </div>
      </div>

      <!-- Right: floor plan -->
      <div class="floor-plan-box reveal reveal-delay-3">
        <div class="floor-plan-header">
          <span class="floor-plan-title">Seating Chart</span>
          <div class="floor-plan-legend">
            <div class="legend-item">
              <span class="legend-dot" style="background: var(--accent);"></span>
              Gold — ₱250
            </div>
            <div class="legend-item">
              <span class="legend-dot" style="background: var(--silver);"></span>
              Silver — ₱200
            </div>
          </div>
        </div>
        <div class="floor-plan-img-wrap">
          <img src="SOURCES/Images/Floor Plan.png"
               alt="Venue floor plan — Gold tier (300 seats) and Silver tier (400 seats)"
               loading="lazy">
        </div>
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 3: Verify in browser**

Expect: two columns — tiers + showtimes + CTA left; floor plan card right. Gold tier has a warm gold glow; silver is cooler; bronze is dim/translucent. On mobile, columns stack.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add tickets section — tiers, showtimes, floor plan"
```

---

## Task 9: Venue Section

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add venue CSS**

```css
/* ── Venue ──────────────────────────────────────── */
.venue-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 64px;
  align-items: start;
  margin-top: 8px;
}
.venue-tag {
  display: inline-block;
  font-size: 9px;
  font-weight: 500;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: var(--accent-soft);
  border: 1px solid rgba(245,200,107,0.2);
  padding: 4px 12px;
  border-radius: var(--radius-pill);
  margin-bottom: 18px;
}
.map-embed {
  width: 100%;
  aspect-ratio: 4 / 3;
  border: 1px solid rgba(245,200,107,0.1);
  border-radius: var(--radius-md);
  display: block;
}
@media (max-width: 768px) {
  .venue-grid { grid-template-columns: 1fr; gap: 40px; }
}
```

- [ ] **Step 2: Add venue HTML after the tickets section**

```html
<!-- ══ VENUE ══════════════════════════════════════════════ -->
<section id="venue" class="site-section">
  <div class="container">
    <p class="section-label reveal">Where to Find Us</p>
    <div class="gold-divider reveal reveal-delay-1"></div>
    <div class="venue-grid">
      <div class="reveal reveal-delay-1">
        <span class="venue-tag">Venue</span>
        <h2 class="section-heading">Lyceum of the Philippines<br>University – Davao</h2>
        <p class="body-text" style="margin-top: 20px;">
          Located in Davao City, Philippines. Accessible via major roads
          with parking available on campus.
        </p>
        <p style="font-size: 13px; color: var(--text-muted); margin-top: 22px; line-height: 2;">
          May 8, 2026<br>
          Showtimes: <strong style="color: rgba(245,200,107,0.8);">5:30 PM</strong>
          &amp; <strong style="color: rgba(245,200,107,0.8);">9:00 PM</strong>
        </p>
        <div style="margin-top: 28px;">
          <a
            href="https://www.google.com/maps/search/Lyceum+of+the+Philippines+University+Davao"
            class="btn-ghost"
            target="_blank" rel="noopener"
          >Open in Google Maps &rarr;</a>
        </div>
      </div>
      <div class="reveal reveal-delay-2">
        <iframe
          class="map-embed"
          src="https://maps.google.com/maps?q=Lyceum+of+the+Philippines+University+Davao&output=embed"
          title="Lyceum of the Philippines University Davao location map"
          loading="lazy"
          allowfullscreen
        ></iframe>
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 3: Verify in browser**

Expect: two columns — venue info + ghost CTA left; map embed right with gold border. On mobile, stacks.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add venue section with map embed"
```

---

## Task 10: Footer

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add footer CSS**

```css
/* ── Footer ─────────────────────────────────────── */
#site-footer {
  padding: 80px 0 48px;
  border-top: 1px solid rgba(245,200,107,0.1);
  text-align: center;
  background:
    radial-gradient(ellipse at 50% 0%, rgba(140,60,0,0.1) 0%, transparent 60%),
    var(--bg-base);
}
.footer-logo {
  height: 64px;
  width: auto;
  display: block;
  margin: 0 auto 28px;
  opacity: 0.9;
}
.social-links {
  display: flex;
  justify-content: center;
  gap: 14px;
  margin-top: 28px;
  flex-wrap: wrap;
}
.social-btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  border: 1px solid rgba(245,200,107,0.35);
  color: rgba(245,200,107,0.85);
  font-size: 11px;
  font-weight: 500;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  padding: 10px 20px;
  border-radius: var(--radius-pill);
  text-decoration: none;
  transition:
    border-color 0.2s ease,
    background 0.2s ease,
    transform 0.2s var(--ease-hover);
}
.social-btn:hover {
  border-color: rgba(245,200,107,0.7);
  background: rgba(245,200,107,0.06);
  transform: translateY(-2px);
}
.social-btn:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 3px;
}
.footer-bottom {
  margin-top: 48px;
  font-size: 11px;
  letter-spacing: 0.05em;
  color: rgba(200,169,110,0.35);
}
```

- [ ] **Step 2: Add footer HTML after the venue section, before the modal `<dialog>`**

```html
<!-- ══ FOOTER ════════════════════════════════════════════ -->
<footer id="site-footer">
  <div class="container">
    <img
      src="SOURCES/Images/Logos/logo-lpud.png"
      alt="Lyceum of the Philippines University Davao"
      class="footer-logo"
    >
    <p class="section-label">Stay Connected</p>
    <h2 class="section-heading" style="font-size: clamp(22px,3vw,32px); margin-top: 12px;">
      Follow Us for Updates
    </h2>
    <div class="social-links">
      <a href="https://www.facebook.com/LPUHumssAssoc" class="social-btn" target="_blank" rel="noopener">
        <svg width="14" height="14" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><path d="M24 12.07C24 5.41 18.63 0 12 0S0 5.41 0 12.07C0 18.1 4.39 23.1 10.13 24v-8.44H7.08v-3.49h3.04V9.41c0-3.02 1.8-4.7 4.54-4.7 1.31 0 2.68.24 2.68.24v2.97h-1.51c-1.49 0-1.95.93-1.95 1.88v2.27h3.32l-.53 3.5h-2.79V24C19.61 23.1 24 18.1 24 12.07z"/></svg>
        LPU HUMSS Assoc
      </a>
      <a href="https://www.facebook.com/LPUDavao" class="social-btn" target="_blank" rel="noopener">
        <svg width="14" height="14" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><path d="M24 12.07C24 5.41 18.63 0 12 0S0 5.41 0 12.07C0 18.1 4.39 23.1 10.13 24v-8.44H7.08v-3.49h3.04V9.41c0-3.02 1.8-4.7 4.54-4.7 1.31 0 2.68.24 2.68.24v2.97h-1.51c-1.49 0-1.95.93-1.95 1.88v2.27h3.32l-.53 3.5h-2.79V24C19.61 23.1 24 18.1 24 12.07z"/></svg>
        LPU Davao
      </a>
    </div>
    <div style="margin-top: 40px;">
      <a
        href="https://docs.google.com/forms/d/e/1FAIpQLSdaMbFQRliTzhKOOuyQMsnJkjWaXBnyfqlir1jmQwziyClBZg/viewform"
        class="btn-primary"
        target="_blank" rel="noopener"
      >Pre-Order Tickets Now</a>
    </div>
    <p class="footer-bottom">
      &copy; 2026 Ang Huling El Bimbo &nbsp;&middot;&nbsp; Grade 12 HUMSS &nbsp;&middot;&nbsp; LPU Davao Junior College
    </p>
  </div>
</footer>
```

- [ ] **Step 3: Verify in browser**

Expect: centered footer with LPU-D logo, two Facebook social pills, final CTA, copyright line.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add footer with LPU-D logo, social links, final CTA"
```

---

## Task 11: Scroll Reveal + Nav JS Polish

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add remaining JS to the `<script>` block** (after the existing modal JS)

```javascript
// ── Mobile nav toggle ─────────────────────────────────────
const hamburger   = document.getElementById('hamburger-btn');
const mobileMenu  = document.getElementById('mobile-menu');

function closeMobileMenu() {
  mobileMenu.classList.remove('open');
  hamburger.setAttribute('aria-expanded', 'false');
}

hamburger.addEventListener('click', () => {
  const isOpen = mobileMenu.classList.toggle('open');
  hamburger.setAttribute('aria-expanded', String(isOpen));
});

document.addEventListener('click', e => {
  if (!mobileMenu.contains(e.target) && !hamburger.contains(e.target)) {
    closeMobileMenu();
  }
});

// ── Scroll reveal ──────────────────────────────────────────
(function () {
  const observer = new IntersectionObserver(
    entries => entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('visible');
        observer.unobserve(entry.target);
      }
    }),
    { threshold: 0.12, rootMargin: '0px 0px -40px 0px' }
  );
  document.querySelectorAll('.reveal').forEach(el => observer.observe(el));
})();

// ── Nav shadow on scroll ───────────────────────────────────
(function () {
  const nav = document.getElementById('site-nav');
  window.addEventListener('scroll', () => {
    if (window.scrollY > 40) {
      nav.style.boxShadow  = '0 4px 32px rgba(0,0,0,0.5)';
      nav.style.background = 'rgba(13,5,0,0.97)';
    } else {
      nav.style.boxShadow  = 'none';
      nav.style.background = 'rgba(13,5,0,0.92)';
    }
  }, { passive: true });
})();
```

- [ ] **Step 2: Verify in browser**

- Scroll down: nav darkens and gains a shadow at scroll > 40px.
- All `reveal` elements animate into view on scroll.
- Mobile: hamburger opens/closes the drawer; clicking a link or outside the drawer closes it.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add scroll reveal, nav shadow, mobile nav JS"
```

---

## Task 12: Populate Character Bios

> **Note:** This task requires the character profiles txt file from the user. If it has not been provided yet, skip this task and return to it when the file is available.

**Files:**
- Modify: `index.html` (update `characterBio` and `actorBio` fields in the `CAST` array)

- [ ] **Step 1: Receive/locate the txt file**

The user will provide (or has provided) a txt file with character and actor descriptions for all 15 cast members. Confirm its path before proceeding.

- [ ] **Step 2: Parse each bio and update the CAST array in `index.html`**

For each entry in `CAST`, replace the placeholder string:
```javascript
characterBio: '<!-- REPLACE: character bio from txt file -->',
actorBio:     '<!-- REPLACE: actor bio from txt file -->',
```
with the actual text from the file, e.g.:
```javascript
characterBio: 'Hector Samala is a man haunted by the memory of a love he never got to keep — a story he has carried in silence for decades.',
actorBio:     'Dheb Pangandaman is a Grade 12 STEM-Reliability student known for his deeply felt, understated performances.',
```

Use plain strings (no HTML). The modal renders them as `textContent`, so HTML entities are not needed.

- [ ] **Step 3: Verify in browser**

Click each of the 15 cast cards. Confirm the character bio and actor bio sections are populated with real text. No placeholder strings visible.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "content: add character and actor bios to all 15 cast entries"
```

---

## Task 13: Final Audit

**Files:**
- Modify: `index.html` (any remaining fixes)

- [ ] **Step 1: Check anti-generic checklist**

Review `MD FILES (CMUST READ)/VIbe Coded website must not know.md` Section 6 pre-ship checklist. If 5+ items apply, fix before shipping.

Quick self-check:
- No purple gradients ✓ (warm amber palette only)
- No sparkle emojis ✓ (advisory left-border only, no ⚠️ in HTML)
- No default Tailwind blues ✓ (no Tailwind at all)
- No semi-transparent low-contrast nav ✓ (solid dark with blur)
- No inconsistent radius ✓ (6/12/30 only)
- Social links go to real profiles ✓
- Footer copyright is correct ✓
- `transition-all` not used ✓

- [ ] **Step 2: Mobile responsiveness check**

Resize browser to 375px. Verify:
- Nav collapses to hamburger. Hamburger drawer opens and closes.
- Hero: poster behind copy at low opacity, copy readable.
- Countdown: numbers readable, colons hidden.
- About: single column, poster above.
- Leads grid: single column.
- Supporting cast grid: 2 columns.
- Tickets: single column.
- Venue: single column.
- Footer: centered, no overflow.

- [ ] **Step 3: Accessibility spot-check**

Tab through the page with keyboard only:
- Every interactive element (nav links, buttons, CTA links, cast cards, modal close) is reachable and has a visible focus ring.
- Open a cast card modal with Enter/Space. Tab inside: focus stays inside modal. Esc closes it. Focus returns to the card.

- [ ] **Step 4: Final commit**

```bash
git add index.html
git commit -m "chore: final audit — mobile, a11y, anti-generic checklist"
```

---

## Dependency Checklist Before Shipping

- [ ] `SOURCES/Images/Logos/logo-lpud.png` — exists ✓ (already confirmed)
- [ ] `SOURCES/Images/Character Profiles/extracted/ima.png` — copied in Pre-Work task
- [ ] `SOURCES/Images/Character Profiles/extracted/mylene.png` — copied in Pre-Work task
- [ ] `SOURCES/Images/Character Profiles/extracted/eleanor.png` — copied in Pre-Work task
- [ ] Character + actor bios populated in CAST array (Task 12)
