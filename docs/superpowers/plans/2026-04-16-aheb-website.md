# AHEB Website Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single `index.html` cinematic-scroll website for the *Ang Huling El Bimbo* musical theatre production at LPU Davao, serving as an information hub and pre-order portal.

**Architecture:** Single self-contained `index.html` with all styles inline. No build step. Tailwind CSS via CDN for utilities; custom CSS for brand-specific styles. A lightweight `serve.mjs` Node.js static server for local development. All images referenced by relative path.

**Tech Stack:** HTML5, CSS3 (inline + Tailwind CDN), vanilla JavaScript (countdown timer), Node.js (serve.mjs dev server only)

---

## File Map

| File | Action | Purpose |
|---|---|---|
| `index.html` | Create | The entire website — all sections, styles, and JS in one file |
| `serve.mjs` | Create | Node.js static file server for local dev at http://localhost:3000 |
| `Images/Character Profiles/extracted/` | Already extracted | 8× V1 character card PNGs used in Cast section |

**Asset paths** (all relative from `index.html` at project root):
- `Officicial poster.jpg` — hero + about sections
- `Images/Logos/JC Logo.png` — footer
- `Images/Floor Plan.png` — tickets section
- `Images/Character Profiles/extracted/V1.png` through `V1 (8).png` — cast section

**Cast roster** (V1 card series — use these 8 for the cast grid):
| File | Character | Actor |
|---|---|---|
| `V1.png` | Joy Manawari | Jullana Bernal |
| `V1 (2).png` | Emman Azarcon | Ace Librero |
| `V1 (3).png` | Hector Samala | Marvin Quilantang |
| `V1 (4).png` | Anthony "AJ" Cruz | Zane Gonzales |
| `V1 (5).png` | Arturo Banlaoi | Simone Estipona |
| `V1 (6).png` | Tiya Dely | Yzane Espedillon |
| `V1 (7).png` | Ligaya | Andriea Estrella |
| `V1 (8).png` | Andrei Antonio | Michael Bardos |

---

## Task 1: Create the local dev server

**Files:**
- Create: `serve.mjs`

- [ ] **Step 1: Create serve.mjs**

```javascript
// serve.mjs — static file server for local dev
import { createServer } from 'http';
import { readFile } from 'fs/promises';
import { extname, join } from 'path';
import { fileURLToPath } from 'url';
import { dirname } from 'path';

const __dirname = dirname(fileURLToPath(import.meta.url));
const PORT = 3000;

const MIME = {
  '.html': 'text/html',
  '.css': 'text/css',
  '.js': 'application/javascript',
  '.png': 'image/png',
  '.jpg': 'image/jpeg',
  '.jpeg': 'image/jpeg',
  '.svg': 'image/svg+xml',
  '.ico': 'image/x-icon',
  '.woff2': 'font/woff2',
};

createServer(async (req, res) => {
  let urlPath = req.url === '/' ? '/index.html' : req.url;
  // strip query strings
  urlPath = urlPath.split('?')[0];
  const filePath = join(__dirname, decodeURIComponent(urlPath));
  try {
    const data = await readFile(filePath);
    const ext = extname(filePath).toLowerCase();
    res.writeHead(200, { 'Content-Type': MIME[ext] || 'application/octet-stream' });
    res.end(data);
  } catch {
    res.writeHead(404);
    res.end('Not found');
  }
}).listen(PORT, () => {
  console.log(`Serving at http://localhost:${PORT}`);
});
```

- [ ] **Step 2: Verify server starts**

```bash
cd "/home/seref/AHEB/AHEB WEBSITE]"
node serve.mjs &
curl -s -o /dev/null -w "%{http_code}" http://localhost:3000/
```
Expected output: `404` (no index.html yet — that's fine, server is running)

---

## Task 2: HTML skeleton, global styles, and fixed navigation

**Files:**
- Create: `index.html`

- [ ] **Step 1: Create index.html with skeleton, global CSS, and nav**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Ang Huling El Bimbo — May 8, 2026</title>
  <meta name="description" content="Ang Huling El Bimbo — A HUMSS Musical Production by LPU Davao Junior College. May 8, 2026. Pre-order tickets now.">
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    /* ── Reset & base ─────────────────────────────── */
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    html { scroll-behavior: smooth; }
    body {
      background: #0d0500;
      color: #e8d5b0;
      font-family: Georgia, 'Times New Roman', serif;
      line-height: 1.75;
    }

    /* ── Typography ───────────────────────────────── */
    .display-title {
      font-family: Georgia, serif;
      font-size: clamp(52px, 9vw, 96px);
      line-height: 0.92;
      letter-spacing: -0.025em;
      color: #FFD700;
    }
    .section-heading {
      font-family: Georgia, serif;
      font-size: clamp(28px, 4vw, 42px);
      color: #FFD700;
      line-height: 1.15;
      letter-spacing: -0.015em;
    }
    .section-label {
      font-family: system-ui, sans-serif;
      font-size: 10px;
      letter-spacing: 0.28em;
      text-transform: uppercase;
      color: rgba(255,215,0,0.5);
    }
    .body-text {
      font-family: system-ui, sans-serif;
      font-size: 15px;
      line-height: 1.75;
      color: #c8a96e;
      max-width: 600px;
    }
    .gold-divider {
      width: 52px;
      height: 2px;
      background: linear-gradient(90deg, #FFD700, transparent);
      margin: 18px 0 30px;
    }

    /* ── Buttons ──────────────────────────────────── */
    .btn-primary {
      display: inline-block;
      background: #FFD700;
      color: #1a0800;
      font-family: system-ui, sans-serif;
      font-size: 11px;
      font-weight: 700;
      letter-spacing: 0.13em;
      text-transform: uppercase;
      padding: 14px 32px;
      border-radius: 30px;
      text-decoration: none;
      transition: transform 0.18s cubic-bezier(0.34,1.56,0.64,1), opacity 0.18s ease;
    }
    .btn-primary:hover { transform: translateY(-2px); opacity: 0.92; }
    .btn-primary:active { transform: translateY(0); }
    .btn-primary:focus-visible { outline: 2px solid #FFD700; outline-offset: 3px; }

    .btn-ghost {
      display: inline-block;
      border: 1px solid rgba(255,215,0,0.4);
      color: rgba(255,215,0,0.85);
      font-family: system-ui, sans-serif;
      font-size: 11px;
      letter-spacing: 0.11em;
      text-transform: uppercase;
      padding: 14px 28px;
      border-radius: 30px;
      text-decoration: none;
      transition: transform 0.18s cubic-bezier(0.34,1.56,0.64,1), border-color 0.18s ease;
    }
    .btn-ghost:hover { transform: translateY(-2px); border-color: rgba(255,215,0,0.7); }
    .btn-ghost:active { transform: translateY(0); }
    .btn-ghost:focus-visible { outline: 2px solid rgba(255,215,0,0.5); outline-offset: 3px; }

    /* ── Sections ─────────────────────────────────── */
    .site-section {
      padding: 100px 0;
      border-top: 1px solid rgba(255,215,0,0.08);
    }
    .container {
      max-width: 1100px;
      margin: 0 auto;
      padding: 0 40px;
    }
    @media (max-width: 640px) {
      .container { padding: 0 20px; }
      .site-section { padding: 72px 0; }
    }

    /* ── Grain texture ────────────────────────────── */
    .grain::after {
      content: '';
      position: absolute;
      inset: 0;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.75' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.035'/%3E%3C/svg%3E");
      pointer-events: none;
      z-index: 1;
    }
  </style>
</head>
<body>

  <!-- ══ NAVIGATION ══════════════════════════════════════════ -->
  <nav id="site-nav" style="
    position: fixed; top: 0; left: 0; right: 0; z-index: 100;
    background: rgba(13,5,0,0.88);
    backdrop-filter: blur(14px);
    -webkit-backdrop-filter: blur(14px);
    border-bottom: 1px solid rgba(255,215,0,0.1);
    padding: 14px 40px;
    display: flex; justify-content: space-between; align-items: center;
    transition: background 0.3s ease;
  ">
    <a href="#hero" style="
      color: #FFD700; font-size: 13px; font-weight: 700;
      font-family: system-ui, sans-serif;
      letter-spacing: 0.22em; text-transform: uppercase; text-decoration: none;
    ">AHEB</a>
    <div style="display: flex; gap: 28px; align-items: center;">
      <a href="#about" style="color: rgba(232,213,176,0.65); font-family: system-ui, sans-serif; font-size: 11px; letter-spacing: 0.1em; text-transform: uppercase; text-decoration: none; transition: color 0.2s ease;" onmouseover="this.style.color='rgba(255,215,0,0.9)'" onmouseout="this.style.color='rgba(232,213,176,0.65)'">About</a>
      <a href="#cast" style="color: rgba(232,213,176,0.65); font-family: system-ui, sans-serif; font-size: 11px; letter-spacing: 0.1em; text-transform: uppercase; text-decoration: none; transition: color 0.2s ease;" onmouseover="this.style.color='rgba(255,215,0,0.9)'" onmouseout="this.style.color='rgba(232,213,176,0.65)'">Cast</a>
      <a href="#tickets" style="color: rgba(232,213,176,0.65); font-family: system-ui, sans-serif; font-size: 11px; letter-spacing: 0.1em; text-transform: uppercase; text-decoration: none; transition: color 0.2s ease;" onmouseover="this.style.color='rgba(255,215,0,0.9)'" onmouseout="this.style.color='rgba(232,213,176,0.65)'">Tickets</a>
      <a href="#venue" style="color: rgba(232,213,176,0.65); font-family: system-ui, sans-serif; font-size: 11px; letter-spacing: 0.1em; text-transform: uppercase; text-decoration: none; transition: color 0.2s ease;" onmouseover="this.style.color='rgba(255,215,0,0.9)'" onmouseout="this.style.color='rgba(232,213,176,0.65)'">Venue</a>
      <a href="#tickets" class="btn-primary" style="padding: 9px 22px; font-size: 10px;">Pre-Order</a>
    </div>
  </nav>

  <!-- sections will be inserted here in subsequent tasks -->

  <script>
    // scripts will be added in subsequent tasks
  </script>
</body>
</html>
```

- [ ] **Step 2: Verify in browser**

```bash
# server should still be running from Task 1; if not:
node serve.mjs &
```
Open http://localhost:3000 — expect: dark page, gold "AHEB" nav fixed at top, nav links visible.

- [ ] **Step 3: Commit**

```bash
cd "/home/seref/AHEB/AHEB WEBSITE]"
git add index.html serve.mjs
git commit -m "feat: add HTML skeleton, global styles, and fixed navigation"
```

---

## Task 3: Hero section

**Files:**
- Modify: `index.html` — add hero section HTML inside `<body>` after the `<nav>` block; add hero CSS inside `<style>`

- [ ] **Step 1: Add hero CSS inside the `<style>` block** (before the closing `</style>` tag)

```css
    /* ── Hero ─────────────────────────────────────── */
    #hero {
      min-height: 100vh;
      display: flex;
      align-items: center;
      position: relative;
      overflow: hidden;
      background:
        radial-gradient(ellipse at 65% 35%, rgba(190,100,0,0.38) 0%, transparent 55%),
        radial-gradient(ellipse at 15% 75%, rgba(120,50,0,0.28) 0%, transparent 50%),
        #0d0500;
      padding-top: 80px;
    }
    .hero-poster {
      position: absolute;
      right: 0;
      top: 0;
      bottom: 0;
      width: 45%;
      object-fit: cover;
      object-position: center top;
      mask-image: linear-gradient(to left, rgba(0,0,0,0.7) 40%, transparent 100%);
      -webkit-mask-image: linear-gradient(to left, rgba(0,0,0,0.7) 40%, transparent 100%);
      opacity: 0.55;
    }
    .hero-content { position: relative; z-index: 2; }
    .hero-meta {
      font-family: system-ui, sans-serif;
      font-size: 10px;
      letter-spacing: 0.28em;
      text-transform: uppercase;
      color: rgba(255,215,0,0.55);
      margin-bottom: 22px;
    }
    .hero-subtitle {
      font-family: system-ui, sans-serif;
      font-size: 14px;
      letter-spacing: 0.07em;
      color: rgba(200,169,110,0.8);
      margin-top: 22px;
      margin-bottom: 36px;
    }
    .hero-btns { display: flex; gap: 14px; flex-wrap: wrap; }
    @media (max-width: 768px) {
      .hero-poster { width: 100%; opacity: 0.18; }
    }
```

- [ ] **Step 2: Add hero HTML** — insert between `</nav>` and the closing `<script>` tag

```html
  <!-- ══ HERO ════════════════════════════════════════════════ -->
  <section id="hero" class="grain">
    <img
      src="Officicial poster.jpg"
      alt="Ang Huling El Bimbo official poster"
      class="hero-poster"
    >
    <div class="container">
      <div class="hero-content">
        <p class="hero-meta">A HUMSS Production &nbsp;·&nbsp; LPU Davao Junior College</p>
        <h1 class="display-title">ANG<br>HULING<br>EL BIMBO</h1>
        <p class="hero-subtitle">May 8, 2026 &nbsp;·&nbsp; Lyceum of the Philippines – Davao</p>
        <div class="hero-btns">
          <a
            href="https://docs.google.com/forms/d/e/1FAIpQLSdaMbFQRliTzhKOOuyQMsnJkjWaXBnyfqlir1jmQwziyClBZg/viewform?usp=sharing&ouid=107343474237936976300"
            class="btn-primary"
            target="_blank" rel="noopener"
          >Pre-Order Tickets</a>
          <a href="#about" class="btn-ghost">Learn More</a>
        </div>
      </div>
    </div>
  </section>
```

- [ ] **Step 3: Verify in browser**

Open http://localhost:3000 — expect: full-screen dark hero, giant gold "ANG HULING EL BIMBO" title, poster fading in from the right, two CTA buttons, nav sitting on top.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add hero section with poster and CTAs"
```

---

## Task 4: Countdown timer section

**Files:**
- Modify: `index.html` — add countdown HTML after `</section>` (hero), add countdown CSS to `<style>`, add countdown JS inside `<script>`

- [ ] **Step 1: Add countdown CSS** (inside `<style>`, after hero styles)

```css
    /* ── Countdown ────────────────────────────────── */
    #countdown {
      padding: 72px 0;
      background: linear-gradient(180deg, #0d0500 0%, #180900 50%, #0d0500 100%);
      border-top: 1px solid rgba(255,215,0,0.1);
      border-bottom: 1px solid rgba(255,215,0,0.1);
      text-align: center;
    }
    .countdown-grid {
      display: flex;
      justify-content: center;
      align-items: center;
      gap: clamp(16px, 4vw, 48px);
      margin-top: 28px;
      flex-wrap: wrap;
    }
    .countdown-unit { text-align: center; min-width: 70px; }
    .countdown-number {
      font-family: Georgia, serif;
      font-size: clamp(44px, 8vw, 72px);
      font-weight: 700;
      color: #FFD700;
      line-height: 1;
      display: block;
    }
    .countdown-lbl {
      font-family: system-ui, sans-serif;
      font-size: 9px;
      letter-spacing: 0.22em;
      text-transform: uppercase;
      color: rgba(255,215,0,0.4);
      margin-top: 8px;
      display: block;
    }
    .countdown-sep {
      font-family: Georgia, serif;
      font-size: clamp(36px, 6vw, 56px);
      color: rgba(255,215,0,0.18);
      line-height: 1;
      padding-bottom: 18px;
      align-self: flex-end;
    }
    @media (max-width: 480px) {
      .countdown-sep { display: none; }
    }
```

- [ ] **Step 2: Add countdown HTML** — insert after the closing `</section>` of the hero

```html
  <!-- ══ COUNTDOWN ══════════════════════════════════════════ -->
  <div id="countdown">
    <div class="container">
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

- [ ] **Step 3: Add countdown JS** — replace the empty `<script>` at the bottom of the file with:

```html
  <script>
    // ── Countdown timer ──────────────────────────────────────
    (function () {
      const SHOW_DATE = new Date('2026-05-08T00:00:00+08:00').getTime();
      const pad = n => String(n).padStart(2, '0');
      function tick() {
        const now = Date.now();
        const diff = SHOW_DATE - now;
        if (diff <= 0) {
          document.getElementById('cd-days').textContent = '00';
          document.getElementById('cd-hours').textContent = '00';
          document.getElementById('cd-minutes').textContent = '00';
          document.getElementById('cd-seconds').textContent = '00';
          return;
        }
        const days    = Math.floor(diff / 86400000);
        const hours   = Math.floor((diff % 86400000) / 3600000);
        const minutes = Math.floor((diff % 3600000) / 60000);
        const seconds = Math.floor((diff % 60000) / 1000);
        document.getElementById('cd-days').textContent    = pad(days);
        document.getElementById('cd-hours').textContent   = pad(hours);
        document.getElementById('cd-minutes').textContent = pad(minutes);
        document.getElementById('cd-seconds').textContent = pad(seconds);
      }
      tick();
      setInterval(tick, 1000);
    })();
  </script>
```

- [ ] **Step 4: Verify in browser**

Open http://localhost:3000 — scroll past hero. Expect: dark countdown band, 4 gold numbers ticking down in real time.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add live countdown timer to May 8 opening night"
```

---

## Task 5: About the Show section

**Files:**
- Modify: `index.html` — add about HTML after countdown `</div>`, add about CSS to `<style>`

- [ ] **Step 1: Add about CSS** (inside `<style>`, after countdown styles)

```css
    /* ── About ────────────────────────────────────── */
    .about-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 64px;
      align-items: center;
      margin-top: 8px;
    }
    .about-poster {
      width: 100%;
      border-radius: 10px;
      box-shadow:
        0 20px 60px rgba(0,0,0,0.6),
        0 4px 16px rgba(201,122,0,0.18);
      display: block;
    }
    .advisory {
      margin-top: 28px;
      padding: 14px 18px;
      background: rgba(220,90,0,0.07);
      border-left: 3px solid rgba(220,100,0,0.45);
      border-radius: 0 6px 6px 0;
    }
    .advisory p {
      font-family: system-ui, sans-serif;
      font-size: 12px;
      line-height: 1.65;
      color: rgba(220,160,100,0.85);
      max-width: none;
    }
    @media (max-width: 768px) {
      .about-grid {
        grid-template-columns: 1fr;
        gap: 36px;
      }
      .about-grid .poster-col { order: -1; }
    }
```

- [ ] **Step 2: Add about HTML** — insert after the countdown `</div>` block

```html
  <!-- ══ ABOUT ══════════════════════════════════════════════ -->
  <section id="about" class="site-section">
    <div class="container">
      <p class="section-label">About the Show</p>
      <div class="gold-divider"></div>
      <div class="about-grid">
        <div>
          <h2 class="section-heading">A Story of Love,<br>Resilience &amp; Discovery</h2>
          <p class="body-text" style="margin-top: 20px;">
            <em>Ang Huling El Bimbo</em> is a musical theatre play adapted and performed by the Grade 12 HUMSS students of LPU Davao. Through engaging musical numbers and dynamic performances, the production explores the experiences and emotions of young individuals navigating life's challenges — themes of love, resilience, and self-discovery that resonate across generations.
          </p>
          <p class="body-text" style="margin-top: 14px;">
            Presented by the Lyceum of the Philippines University Davao Junior College, this production marks a landmark achievement of student artistry and collaboration.
          </p>
          <div class="advisory">
            <p>⚠️ <strong style="color: rgba(255,140,60,0.95);">Viewer Advisory:</strong> Contains mature themes, strobe lights, and loud sounds. Not suitable for very young or sensitive audiences.</p>
          </div>
        </div>
        <div class="poster-col">
          <img
            src="Officicial poster.jpg"
            alt="Official Ang Huling El Bimbo poster"
            class="about-poster"
          >
        </div>
      </div>
    </div>
  </section>
```

- [ ] **Step 3: Verify in browser**

Open http://localhost:3000 — scroll to About. Expect: two-column layout, body text left, poster right, advisory callout with amber left border.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add About the Show section with poster and viewer advisory"
```

---

## Task 6: Cast & Characters section

**Files:**
- Modify: `index.html` — add cast HTML after about section, add cast CSS to `<style>`

- [ ] **Step 1: Add cast CSS** (inside `<style>`, after about styles)

```css
    /* ── Cast ─────────────────────────────────────── */
    .cast-grid {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 18px;
      margin-top: 36px;
    }
    .cast-card {
      background: rgba(255,255,255,0.025);
      border: 1px solid rgba(255,215,0,0.1);
      border-radius: 10px;
      overflow: hidden;
      transition: transform 0.22s cubic-bezier(0.34,1.56,0.64,1),
                  box-shadow 0.22s ease,
                  border-color 0.22s ease;
    }
    .cast-card:hover {
      transform: translateY(-5px);
      box-shadow: 0 14px 40px rgba(0,0,0,0.5), 0 2px 8px rgba(201,122,0,0.15);
      border-color: rgba(255,215,0,0.3);
    }
    .cast-card img {
      width: 100%;
      aspect-ratio: 3 / 4;
      object-fit: cover;
      object-position: top center;
      display: block;
    }
    .cast-body {
      padding: 12px 14px 14px;
    }
    .cast-character {
      font-family: Georgia, serif;
      font-size: 15px;
      color: #FFD700;
      line-height: 1.2;
    }
    .cast-actor {
      font-family: system-ui, sans-serif;
      font-size: 10px;
      letter-spacing: 0.1em;
      text-transform: uppercase;
      color: rgba(200,169,110,0.5);
      margin-top: 4px;
    }
    @media (max-width: 900px) {
      .cast-grid { grid-template-columns: repeat(3, 1fr); }
    }
    @media (max-width: 640px) {
      .cast-grid { grid-template-columns: repeat(2, 1fr); gap: 12px; }
    }
```

- [ ] **Step 2: Add cast HTML** — insert after the closing `</section>` of the about section

```html
  <!-- ══ CAST ═══════════════════════════════════════════════ -->
  <section id="cast" class="site-section">
    <div class="container">
      <p class="section-label">The Cast</p>
      <div class="gold-divider"></div>
      <h2 class="section-heading">Meet the Performers</h2>
      <p class="body-text" style="margin-top: 16px;">Grade 12 HUMSS students bringing the story to life through music, acting, and dance.</p>
      <div class="cast-grid">
        <div class="cast-card">
          <img src="Images/Character Profiles/extracted/V1.png" alt="Joy Manawari — Jullana Bernal" loading="lazy">
          <div class="cast-body">
            <div class="cast-character">Joy Manawari</div>
            <div class="cast-actor">Jullana Bernal</div>
          </div>
        </div>
        <div class="cast-card">
          <img src="Images/Character Profiles/extracted/V1 (2).png" alt="Emman Azarcon — Ace Librero" loading="lazy">
          <div class="cast-body">
            <div class="cast-character">Emman Azarcon</div>
            <div class="cast-actor">Ace Librero</div>
          </div>
        </div>
        <div class="cast-card">
          <img src="Images/Character Profiles/extracted/V1 (3).png" alt="Hector Samala — Marvin Quilantang" loading="lazy">
          <div class="cast-body">
            <div class="cast-character">Hector Samala</div>
            <div class="cast-actor">Marvin Quilantang</div>
          </div>
        </div>
        <div class="cast-card">
          <img src="Images/Character Profiles/extracted/V1 (4).png" alt="Anthony AJ Cruz — Zane Gonzales" loading="lazy">
          <div class="cast-body">
            <div class="cast-character">Anthony "AJ" Cruz</div>
            <div class="cast-actor">Zane Gonzales</div>
          </div>
        </div>
        <div class="cast-card">
          <img src="Images/Character Profiles/extracted/V1 (5).png" alt="Arturo Banlaoi — Simone Estipona" loading="lazy">
          <div class="cast-body">
            <div class="cast-character">Arturo Banlaoi</div>
            <div class="cast-actor">Simone Estipona</div>
          </div>
        </div>
        <div class="cast-card">
          <img src="Images/Character Profiles/extracted/V1 (6).png" alt="Tiya Dely — Yzane Espedillon" loading="lazy">
          <div class="cast-body">
            <div class="cast-character">Tiya Dely</div>
            <div class="cast-actor">Yzane Espedillon</div>
          </div>
        </div>
        <div class="cast-card">
          <img src="Images/Character Profiles/extracted/V1 (7).png" alt="Ligaya — Andriea Estrella" loading="lazy">
          <div class="cast-body">
            <div class="cast-character">Ligaya</div>
            <div class="cast-actor">Andriea Estrella</div>
          </div>
        </div>
        <div class="cast-card">
          <img src="Images/Character Profiles/extracted/V1 (8).png" alt="Andrei Antonio — Michael Bardos" loading="lazy">
          <div class="cast-body">
            <div class="cast-character">Andrei Antonio</div>
            <div class="cast-actor">Michael Bardos</div>
          </div>
        </div>
      </div>
    </div>
  </section>
```

- [ ] **Step 3: Verify in browser**

Open http://localhost:3000 — scroll to Cast. Expect: 4-column grid of character portrait cards with character name and actor name below each.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add Cast section with 8 character portrait cards"
```

---

## Task 7: Tickets & Pre-Order section

**Files:**
- Modify: `index.html` — add tickets HTML after cast section, add tickets CSS to `<style>`

- [ ] **Step 1: Add tickets CSS** (inside `<style>`, after cast styles)

```css
    /* ── Tickets ──────────────────────────────────── */
    .tickets-layout {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 56px;
      align-items: start;
      margin-top: 8px;
    }
    .tier-stack { display: flex; flex-direction: column; gap: 14px; }
    .tier-card {
      border: 1px solid rgba(255,215,0,0.18);
      border-radius: 10px;
      padding: 20px 24px;
      background: rgba(255,215,0,0.03);
      position: relative;
    }
    .tier-card.gold {
      border-color: rgba(255,215,0,0.5);
      background: rgba(255,215,0,0.07);
      box-shadow: 0 0 24px rgba(255,215,0,0.06);
    }
    .tier-card.muted { opacity: 0.45; }
    .tier-badge {
      font-family: system-ui, sans-serif;
      font-size: 9px;
      letter-spacing: 0.22em;
      text-transform: uppercase;
      color: rgba(255,215,0,0.5);
      margin-bottom: 6px;
    }
    .tier-price {
      font-family: Georgia, serif;
      font-size: 38px;
      color: #FFD700;
      line-height: 1;
    }
    .tier-seats {
      font-family: system-ui, sans-serif;
      font-size: 11px;
      color: rgba(200,169,110,0.55);
      margin-top: 5px;
    }
    .payment-note {
      font-family: system-ui, sans-serif;
      font-size: 12px;
      color: rgba(200,169,110,0.65);
      line-height: 1.7;
      margin-top: 22px;
    }
    .payment-note strong { color: rgba(255,215,0,0.8); }
    .showtime-row {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
      margin-top: 22px;
    }
    .showtime-pill {
      background: rgba(255,255,255,0.03);
      border: 1px solid rgba(255,215,0,0.14);
      border-radius: 8px;
      padding: 14px;
      text-align: center;
    }
    .showtime-time {
      font-family: Georgia, serif;
      font-size: 20px;
      color: #FFD700;
      display: block;
    }
    .showtime-arrive {
      font-family: system-ui, sans-serif;
      font-size: 10px;
      color: rgba(200,169,110,0.5);
      margin-top: 5px;
      line-height: 1.5;
      display: block;
    }
    .floor-plan-box {
      background: rgba(255,255,255,0.025);
      border: 1px solid rgba(255,215,0,0.1);
      border-radius: 10px;
      padding: 20px;
    }
    .floor-plan-box img {
      width: 100%;
      border-radius: 6px;
      display: block;
    }
    @media (max-width: 768px) {
      .tickets-layout { grid-template-columns: 1fr; gap: 40px; }
      .showtime-row { grid-template-columns: 1fr 1fr; }
    }
```

- [ ] **Step 2: Add tickets HTML** — insert after the closing `</section>` of the cast section

```html
  <!-- ══ TICKETS ════════════════════════════════════════════ -->
  <section id="tickets" class="site-section">
    <div class="container">
      <p class="section-label">Get Your Seats</p>
      <div class="gold-divider"></div>
      <h2 class="section-heading">Tickets &amp; Pre-Order</h2>
      <div class="tickets-layout" style="margin-top: 36px;">

        <!-- Left: tiers, showtimes, payment info, CTA -->
        <div>
          <div class="tier-stack">
            <div class="tier-card gold">
              <div class="tier-badge">⭐ Gold Tier</div>
              <div class="tier-price">₱250</div>
              <div class="tier-seats">300 seats available</div>
            </div>
            <div class="tier-card">
              <div class="tier-badge">Silver Tier</div>
              <div class="tier-price">₱200</div>
              <div class="tier-seats">400 seats available</div>
            </div>
            <div class="tier-card muted">
              <div class="tier-badge">Bronze Tier</div>
              <div class="tier-price" style="font-size: 20px; padding-top: 6px;">Coming Soon</div>
            </div>
          </div>

          <div class="showtime-row">
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
            <strong>GCash only</strong> &nbsp;·&nbsp; Tickets are <strong>non-refundable</strong><br>
            Free seating — first come, first served<br>
            Confirmation sent via email, phone, or Facebook
          </div>

          <div style="margin-top: 32px;">
            <a
              href="https://docs.google.com/forms/d/e/1FAIpQLSdaMbFQRliTzhKOOuyQMsnJkjWaXBnyfqlir1jmQwziyClBZg/viewform?usp=sharing&ouid=107343474237936976300"
              class="btn-primary"
              target="_blank" rel="noopener"
            >Pre-Order via Google Form &rarr;</a>
          </div>
        </div>

        <!-- Right: floor plan -->
        <div class="floor-plan-box">
          <p class="section-label" style="margin-bottom: 14px;">Venue Layout</p>
          <img src="Images/Floor Plan.png" alt="Venue floor plan showing Gold Tier (300 seats) and Silver Tier (400 seats)">
        </div>

      </div>
    </div>
  </section>
```

- [ ] **Step 3: Verify in browser**

Open http://localhost:3000 — scroll to Tickets. Expect: two-column layout, Gold/Silver/Bronze tier cards on left (Gold highlighted), two showtime pills, floor plan image on right.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add Tickets section with tiers, showtimes, floor plan, and pre-order CTA"
```

---

## Task 8: Venue section

**Files:**
- Modify: `index.html` — add venue HTML after tickets section, add venue CSS to `<style>`

- [ ] **Step 1: Add venue CSS** (inside `<style>`, after tickets styles)

```css
    /* ── Venue ────────────────────────────────────── */
    .venue-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 56px;
      align-items: start;
      margin-top: 8px;
    }
    .venue-tag {
      display: inline-block;
      font-family: system-ui, sans-serif;
      font-size: 10px;
      letter-spacing: 0.14em;
      text-transform: uppercase;
      background: rgba(255,215,0,0.08);
      border: 1px solid rgba(255,215,0,0.2);
      color: rgba(255,215,0,0.75);
      padding: 5px 14px;
      border-radius: 20px;
      margin-bottom: 20px;
    }
    .map-embed {
      width: 100%;
      aspect-ratio: 4 / 3;
      border-radius: 10px;
      border: 1px solid rgba(255,215,0,0.1);
      display: block;
      filter: grayscale(30%) brightness(0.85) sepia(20%);
    }
    @media (max-width: 768px) {
      .venue-grid { grid-template-columns: 1fr; gap: 36px; }
    }
```

- [ ] **Step 2: Add venue HTML** — insert after the closing `</section>` of the tickets section

```html
  <!-- ══ VENUE ══════════════════════════════════════════════ -->
  <section id="venue" class="site-section">
    <div class="container">
      <p class="section-label">Where to Find Us</p>
      <div class="gold-divider"></div>
      <div class="venue-grid" style="margin-top: 8px;">
        <div>
          <span class="venue-tag">📍 Venue</span>
          <h2 class="section-heading">Lyceum of the Philippines<br>University – Davao</h2>
          <p class="body-text" style="margin-top: 20px;">
            Located in Davao City, Philippines. Accessible via major roads with parking available on campus.
          </p>
          <p style="font-family: system-ui, sans-serif; font-size: 13px; color: rgba(200,169,110,0.55); margin-top: 22px; line-height: 1.75;">
            May 8, 2026<br>
            Showtimes: <strong style="color: rgba(255,215,0,0.7);">5:30 PM</strong> &amp; <strong style="color: rgba(255,215,0,0.7);">9:00 PM</strong>
          </p>
          <div style="margin-top: 28px;">
            <a
              href="https://www.google.com/maps/search/Lyceum+of+the+Philippines+University+Davao"
              class="btn-ghost"
              target="_blank" rel="noopener"
              style="font-size: 11px;"
            >Open in Google Maps &rarr;</a>
          </div>
        </div>
        <div>
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

Open http://localhost:3000 — scroll to Venue. Expect: two-column layout, venue details left, embedded Google Map right (may show blank if iframe blocked — that's expected in some browsers; the "Open in Google Maps" button is the reliable fallback).

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add Venue section with map embed and directions link"
```

---

## Task 9: Footer and Social section

**Files:**
- Modify: `index.html` — add footer HTML after venue section, add footer CSS to `<style>`

- [ ] **Step 1: Add footer CSS** (inside `<style>`, after venue styles)

```css
    /* ── Footer ───────────────────────────────────── */
    #site-footer {
      border-top: 1px solid rgba(255,215,0,0.1);
      background: #0d0500;
      padding: 88px 0 44px;
      text-align: center;
    }
    .social-links {
      display: flex;
      gap: 16px;
      justify-content: center;
      flex-wrap: wrap;
      margin-top: 28px;
    }
    .social-btn {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      border: 1px solid rgba(255,215,0,0.28);
      color: rgba(255,215,0,0.82);
      font-family: system-ui, sans-serif;
      font-size: 11px;
      letter-spacing: 0.1em;
      text-transform: uppercase;
      padding: 12px 24px;
      border-radius: 30px;
      text-decoration: none;
      transition: transform 0.18s cubic-bezier(0.34,1.56,0.64,1),
                  border-color 0.18s ease,
                  background 0.18s ease;
    }
    .social-btn:hover {
      transform: translateY(-2px);
      border-color: rgba(255,215,0,0.6);
      background: rgba(255,215,0,0.06);
    }
    .social-btn:active { transform: translateY(0); }
    .social-btn:focus-visible { outline: 2px solid rgba(255,215,0,0.5); outline-offset: 3px; }
    .footer-logo {
      width: 72px;
      height: 72px;
      object-fit: contain;
      margin: 0 auto 28px;
      display: block;
      opacity: 0.7;
    }
    .footer-bottom {
      margin-top: 56px;
      font-family: system-ui, sans-serif;
      font-size: 10px;
      letter-spacing: 0.12em;
      color: rgba(200,169,110,0.28);
      text-transform: uppercase;
    }
```

- [ ] **Step 2: Add footer HTML** — insert after the closing `</section>` of the venue section, before the `<script>` tag

```html
  <!-- ══ FOOTER / SOCIAL ════════════════════════════════════ -->
  <footer id="site-footer">
    <div class="container">
      <img src="Images/Logos/JC Logo.png" alt="LPU Davao Junior College logo" class="footer-logo">
      <p class="section-label">Stay Connected</p>
      <h2 class="section-heading" style="font-size: clamp(22px, 3vw, 32px); margin-top: 10px;">Follow Us for Updates</h2>
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
      <div style="margin-top: 44px;">
        <a
          href="https://docs.google.com/forms/d/e/1FAIpQLSdaMbFQRliTzhKOOuyQMsnJkjWaXBnyfqlir1jmQwziyClBZg/viewform?usp=sharing&ouid=107343474237936976300"
          class="btn-primary"
          target="_blank" rel="noopener"
        >Pre-Order Tickets Now</a>
      </div>
      <div class="footer-bottom">
        &copy; 2026 Ang Huling El Bimbo &nbsp;·&nbsp; Grade 12 HUMSS &nbsp;·&nbsp; LPU Davao Junior College
      </div>
    </div>
  </footer>
```

- [ ] **Step 3: Verify in browser**

Open http://localhost:3000 — scroll to footer. Expect: LPU logo, "Follow Us for Updates" heading, two Facebook buttons, final pre-order CTA, copyright line.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add Footer with social links, LPU logo, and final pre-order CTA"
```

---

## Task 10: Mobile responsive polish and final verification

**Files:**
- Modify: `index.html` — add mobile nav toggle + additional responsive CSS; visual comparison pass

- [ ] **Step 1: Add mobile nav CSS and hamburger** — add to `<style>` block (after footer styles)

```css
    /* ── Mobile nav ───────────────────────────────── */
    .nav-links-desktop { display: flex; gap: 28px; align-items: center; }
    .nav-hamburger {
      display: none;
      background: none;
      border: 1px solid rgba(255,215,0,0.3);
      border-radius: 6px;
      padding: 7px 10px;
      cursor: pointer;
      color: #FFD700;
    }
    .nav-hamburger:focus-visible { outline: 2px solid rgba(255,215,0,0.5); outline-offset: 2px; }
    .mobile-menu {
      display: none;
      position: fixed;
      top: 57px; left: 0; right: 0;
      background: rgba(13,5,0,0.97);
      backdrop-filter: blur(14px);
      border-bottom: 1px solid rgba(255,215,0,0.1);
      padding: 20px 24px 24px;
      z-index: 99;
      flex-direction: column;
      gap: 4px;
    }
    .mobile-menu.open { display: flex; }
    .mobile-menu a {
      color: rgba(232,213,176,0.75);
      font-family: system-ui, sans-serif;
      font-size: 14px;
      letter-spacing: 0.08em;
      text-transform: uppercase;
      text-decoration: none;
      padding: 12px 0;
      border-bottom: 1px solid rgba(255,215,0,0.06);
      transition: color 0.2s;
    }
    .mobile-menu a:last-child { border-bottom: none; }
    .mobile-menu a:hover { color: #FFD700; }
    .mobile-menu .btn-primary { margin-top: 12px; text-align: center; }
    @media (max-width: 640px) {
      .nav-links-desktop { display: none; }
      .nav-hamburger { display: block; }
      #site-nav { padding: 14px 20px; }
    }
```

- [ ] **Step 2: Add hamburger button to nav and mobile menu** — replace the existing `<nav>` block with this updated version

```html
  <nav id="site-nav" style="
    position: fixed; top: 0; left: 0; right: 0; z-index: 100;
    background: rgba(13,5,0,0.88);
    backdrop-filter: blur(14px);
    -webkit-backdrop-filter: blur(14px);
    border-bottom: 1px solid rgba(255,215,0,0.1);
    padding: 14px 40px;
    display: flex; justify-content: space-between; align-items: center;
    transition: background 0.3s ease;
  ">
    <a href="#hero" style="
      color: #FFD700; font-size: 13px; font-weight: 700;
      font-family: system-ui, sans-serif;
      letter-spacing: 0.22em; text-transform: uppercase; text-decoration: none;
    ">AHEB</a>
    <div class="nav-links-desktop">
      <a href="#about" style="color: rgba(232,213,176,0.65); font-family: system-ui, sans-serif; font-size: 11px; letter-spacing: 0.1em; text-transform: uppercase; text-decoration: none; transition: color 0.2s ease;" onmouseover="this.style.color='rgba(255,215,0,0.9)'" onmouseout="this.style.color='rgba(232,213,176,0.65)'">About</a>
      <a href="#cast" style="color: rgba(232,213,176,0.65); font-family: system-ui, sans-serif; font-size: 11px; letter-spacing: 0.1em; text-transform: uppercase; text-decoration: none; transition: color 0.2s ease;" onmouseover="this.style.color='rgba(255,215,0,0.9)'" onmouseout="this.style.color='rgba(232,213,176,0.65)'">Cast</a>
      <a href="#tickets" style="color: rgba(232,213,176,0.65); font-family: system-ui, sans-serif; font-size: 11px; letter-spacing: 0.1em; text-transform: uppercase; text-decoration: none; transition: color 0.2s ease;" onmouseover="this.style.color='rgba(255,215,0,0.9)'" onmouseout="this.style.color='rgba(232,213,176,0.65)'">Tickets</a>
      <a href="#venue" style="color: rgba(232,213,176,0.65); font-family: system-ui, sans-serif; font-size: 11px; letter-spacing: 0.1em; text-transform: uppercase; text-decoration: none; transition: color 0.2s ease;" onmouseover="this.style.color='rgba(255,215,0,0.9)'" onmouseout="this.style.color='rgba(232,213,176,0.65)'">Venue</a>
      <a href="#tickets" class="btn-primary" style="padding: 9px 22px; font-size: 10px;">Pre-Order</a>
    </div>
    <button class="nav-hamburger" id="hamburger-btn" aria-label="Open navigation menu" aria-expanded="false">
      <svg width="18" height="14" viewBox="0 0 18 14" fill="none" aria-hidden="true">
        <rect y="0" width="18" height="2" rx="1" fill="currentColor"/>
        <rect y="6" width="18" height="2" rx="1" fill="currentColor"/>
        <rect y="12" width="18" height="2" rx="1" fill="currentColor"/>
      </svg>
    </button>
  </nav>

  <!-- Mobile menu (toggled by JS) -->
  <div class="mobile-menu" id="mobile-menu" role="dialog" aria-label="Navigation menu">
    <a href="#about" onclick="closeMobileMenu()">About</a>
    <a href="#cast" onclick="closeMobileMenu()">Cast</a>
    <a href="#tickets" onclick="closeMobileMenu()">Tickets</a>
    <a href="#venue" onclick="closeMobileMenu()">Venue</a>
    <a href="https://docs.google.com/forms/d/e/1FAIpQLSdaMbFQRliTzhKOOuyQMsnJkjWaXBnyfqlir1jmQwziyClBZg/viewform?usp=sharing&ouid=107343474237936976300" class="btn-primary" target="_blank" rel="noopener" onclick="closeMobileMenu()">Pre-Order Tickets</a>
  </div>
```

- [ ] **Step 3: Add mobile menu JS** — add inside the `<script>` block, after the countdown code

```javascript
    // ── Mobile nav toggle ────────────────────────────────────
    const hamburger = document.getElementById('hamburger-btn');
    const mobileMenu = document.getElementById('mobile-menu');
    function closeMobileMenu() {
      mobileMenu.classList.remove('open');
      hamburger.setAttribute('aria-expanded', 'false');
    }
    hamburger.addEventListener('click', () => {
      const isOpen = mobileMenu.classList.toggle('open');
      hamburger.setAttribute('aria-expanded', String(isOpen));
    });
    document.addEventListener('click', (e) => {
      if (!mobileMenu.contains(e.target) && !hamburger.contains(e.target)) {
        closeMobileMenu();
      }
    });
```

- [ ] **Step 4: Visual verification — desktop**

Open http://localhost:3000. Scroll through all sections and verify:
- [ ] Hero: full-screen, title visible, poster visible on right, both buttons work
- [ ] Countdown: live numbers updating every second
- [ ] About: two-column, poster loads, advisory callout visible
- [ ] Cast: 4-column grid, all 8 character photos load with correct names
- [ ] Tickets: tier cards (Gold highlighted), showtime pills, floor plan image loads, pre-order button opens Google Form in new tab
- [ ] Venue: map embed loads, "Open in Google Maps" link works
- [ ] Footer: LPU logo loads, both Facebook buttons open correct pages in new tab, pre-order button works

- [ ] **Step 5: Visual verification — mobile**

In Chrome DevTools (F12), set device to iPhone 14 (390px). Verify:
- [ ] Hamburger menu appears, desktop links hidden
- [ ] Hamburger opens/closes mobile menu
- [ ] Hero text legible, poster fades to background only
- [ ] About section stacks vertically (poster above text)
- [ ] Cast grid shows 2 columns
- [ ] Tickets stack vertically (tiers above floor plan)
- [ ] Venue stacks vertically (map below text)
- [ ] No horizontal scroll

- [ ] **Step 6: Final commit**

```bash
git add index.html
git commit -m "feat: add mobile responsive nav with hamburger menu — AHEB website complete"
```
