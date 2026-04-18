# AHEB Website Redesign — Design Spec

**Date:** 2026-04-18
**Project:** Ang Huling El Bimbo — HUMSS Musical Production site (LPU Davao, May 8 2026)
**Scope:** Visual redesign of the single-page `index.html` to a cleaner, more polished, editorial-theatrical design. Adds clickable cast bios sourced from `Character profiles/AngHulingBimbo_AllCastProfiles.txt`. Swaps in updated images for Ms. Ima, Mylene, and Eleanor.

---

## 1. Goals and Non-Goals

**Goals**
- Match the visual bar of a mature school-production / off-Broadway-program site. Avoid "AI-generated" red flags per `MD FILES (CMUST READ)/VIbe Coded website must not know.md`.
- Establish a clear, consistent design system (palette, typography, spacing, radius, shadow).
- Introduce a clickable cast-bio interaction using the character + actor descriptions already captured.
- Keep the site a single `index.html` on the existing `serve.mjs` dev server.

**Non-goals**
- No framework migration. No multi-page architecture.
- No new content beyond the provided bios.
- No automated tests (static marketing page).
- No CMS or backend. Content stays inline.

---

## 2. Foundation — Design System

### Palette
| Token | Hex | Use |
| --- | --- | --- |
| `--bg-base` | `#0d0500` | Page background, hero backdrop |
| `--bg-elevated` | `#1a0d05` | Card/panel fills |
| `--bg-stage-glow` | `rgba(200,100,0,0.35)` | Warm radial gradient layer |
| `--accent-gold` | `#F5C86B` | Primary accent (headings, CTAs, dividers) |
| `--accent-gold-soft` | `rgba(245,200,107,0.55)` | Labels, subtle borders |
| `--text-primary` | `#f0d79b` | Display headings in accent contexts |
| `--text-body` | `rgba(200,169,110,0.78)` | Body copy |
| `--text-muted` | `rgba(200,169,110,0.55)` | Meta, captions, small text |
| `--silver-tier` | `#dde3ee` | Silver-tier price/label |
| `--warn-tint` | `rgba(220,100,0,0.5)` | Viewer-advisory left border |

No Tailwind default blues/indigos. No purple. No neon glows.

### Typography
- **Display:** `Playfair Display` (Google Fonts), weights 400/500/600/700 + italic 400.
- **Body / UI:** `Inter` (Google Fonts), weights 400/500/600.
- **Type ramp:**
  - Hero title: `clamp(56px, 10vw, 108px)`, weight 500, tracking `-0.028em`, line-height 0.92.
  - Section heading: `clamp(28px, 4vw, 44px)`, weight 600, tracking `-0.015em`, line-height 1.1.
  - Sub-heading (e.g. "Leads", "Supporting Cast"): 22px Playfair.
  - Body: 14–15px Inter, line-height 1.75, max-width 600–640px.
  - Label (all-caps): 10–11px Inter, letter-spacing 0.28em, uppercase.
  - Meta/caption: 9–10px Inter, letter-spacing 0.2em, uppercase.

### Spacing and grid
- **8pt rhythm.** Allowed values: 4 / 8 / 12 / 16 / 20 / 24 / 32 / 40 / 48 / 64 / 80 / 110.
- **Container:** `max-width: 1120px`, horizontal padding 40px desktop / 20px mobile.
- **Section padding:** 110px vertical desktop / 80px mobile.

### Radius
- `6px` — small inline elements.
- `12px` — cards, panels, media.
- `30px` — pill buttons.
- No mixing outside these values.

### Elevation / shadows
Single reusable layered shadow:
```
box-shadow:
  0 20px 50px rgba(0,0,0,0.55),
  0 4px 12px rgba(201,122,0,0.2),
  0 0 0 1px rgba(245,200,107,0.15);
```
Hover variant: same with `0 0 0 1px rgba(245,200,107,0.35)`.

### Motion rules
- Animate `opacity` and `transform` only. Never `transition-all`.
- Easing: `cubic-bezier(0.22,1,0.36,1)` for entrances; 0.2s cubic-bezier for hovers.
- Scroll reveal: IntersectionObserver, 28px translateY + opacity fade.
- Drop the hero "breathing" and spotlight-sway animations. Drop the countdown colon-blink. Keep the system calm.

---

## 3. Information Architecture

Single-page, top-to-bottom:

1. **Navigation** (sticky)
2. **Hero** — split, poster-right
3. **Countdown** — compact band
4. **About** — two-column copy + poster
5. **Cast** — leads spotlight + supporting grid (clickable → modal bios)
6. **Tickets** — classic split (tiers/CTA left, floor plan right)
7. **Venue** — two-column copy + Google Maps iframe
8. **Footer** — LPU-D logo + socials + final CTA + copyright

---

## 4. Components

### 4.1 Navigation
- Sticky top, height 64px. `background: rgba(13,5,0,0.9)`; darkens + gains a subtle shadow when `window.scrollY > 40`.
- Left: `SOURCES/Images/Logos/JC Logo.png` (Junior College logo) + wordmark "AHEB" in Playfair 18px.
- Right links (Inter, 12px, 0.15em tracking, uppercase): About · Cast · Tickets · Venue. Plus a primary pill CTA "Pre-Order".
- Mobile (<768px): links collapse into a hamburger → full-height slide-in drawer. Focus returns to hamburger on close. Drawer closes on outside click or link click.

### 4.2 Hero (Hero 1 — Split, Poster-Right)
- Grid: left 55%, right 45%.
- Left stack (gap 12–24px): tagline pill ("May 8 · 2026 · LPU Davao"), Playfair title `Ang Huling / El Bimbo`, one-line Inter subtitle, row with two CTAs — `.btn-primary` "Pre-Order Tickets" and `.btn-ghost` "About the Show".
- Right: `<img src="SOURCES/Images/Officicial poster.jpg">` absolutely positioned, full-height, masked with `linear-gradient(to left, rgba(0,0,0,0.65) 35%, transparent 100%)` so it fades into the dark background. No sway or breathe animations.
- Backdrop: two static warm radial gradients for "stage lighting" + `grain` SVG noise overlay.
- Below 768px: poster drops to 15% opacity and sits behind the copy.

### 4.3 Countdown
- Centered band between hero and about, 80px vertical padding.
- Label "OPENING NIGHT IN".
- Four units: Days / Hours / Minutes / Seconds. Each: Playfair `clamp(48px, 9vw, 80px)` number, 9px Inter uppercase label.
- Static separator colons (no blink). One soft radial glow behind the numbers.
- Target: `2026-05-08T00:00:00+08:00`. When diff ≤ 0, render `00`.

### 4.4 About
- Two-column grid 1fr/1fr, gap 72px.
- Left column: "About the Show" label, gold divider, section heading ("A Story of Love, Resilience & Discovery"), 2 paragraphs of body copy, advisory callout.
- Advisory: `padding: 16px 20px; background: rgba(220,90,0,0.07); border-left: 3px solid var(--warn-tint); border-radius: 0 8px 8px 0;` — no emoji sirens.
- Right column: poster with the shared layered shadow + 12px radius.
- Stacks on ≤768px; poster moves above copy.

### 4.5 Cast (Leads Spotlight + Clickable Modal Bios)

#### 4.5.1 Featured leads (row of 4)
Cards for the four Older-era protagonists: **Older Hector Samala**, **Older Emman Azarcon**, **Older Anthony Cruz Jr.**, **Older Joy Manawari**. These carry the story's emotional arc and deserve the largest treatment.

Layout: CSS grid, 4 equal columns desktop → 2 tablet → 1 mobile. Each card:
- Full-width portrait, `aspect-ratio: 3/4`, `object-position: top center`.
- Body padding 14–18px.
  - Role label (10px Inter uppercase, 0.2em tracking, gold-soft).
  - Character name (Playfair 18–20px, `--accent-gold`).
  - Actor name (10px Inter uppercase, 0.14em tracking, muted).
  - Teaser line — the **first sentence** of the character bio from the txt file, no ellipsis (if a sentence is too long the card just wraps).
- Interaction: entire card is a `<button>`; hover lifts 4–6px with the shared shadow upgrade. Focus-visible ring `2px solid var(--accent-gold)` with 3px offset.

#### 4.5.2 Supporting cast grid (11 cards)
Cards for: the 4 young leads (Younger Joy, Emman, Hector, Anthony), Arturo, Tiya Dely, Ligaya, Andrei, **Ms. Ima**, **Mylene**, **Eleanor**. Grouped with small uppercase labels: "Young Cast" above the four young leads, "Supporting Cast" above everyone else.

Layout: grid `repeat(5, 1fr)` desktop → `repeat(4, 1fr)` → `repeat(3, 1fr)` → `repeat(2, 1fr)`. Gap 16px. Group labels span the full grid row with a hairline bottom border.

Each card:
- Portrait, `aspect-ratio: 3/4`.
- Character name (Playfair 13–14px, gold).
- Actor name (9px Inter uppercase, muted).
- Clickable (same `<button>` treatment).

#### 4.5.3 Bio modal
- Built on the native `<dialog>` element for accessibility.
- Open via JS: `dialog.showModal()` when a card is activated.
- Layout inside dialog (max-width 720px, max-height 90vh, scrollable):
  - Full-width portrait, `aspect-ratio: 3/4` on mobile, `3/4` with `max-height: 420px` on desktop.
  - Role label + character name (Playfair 28–32px).
  - Actor name + strand (10px Inter uppercase).
  - **Character bio** paragraph (Inter 15px).
  - **Actor bio** paragraph (Inter 15px) under a divider.
  - Close button top-right (24px × 24px, hit area 44×44, `aria-label="Close"`).
- Accessibility:
  - `aria-labelledby` on the dialog pointing at the character-name heading.
  - Focus moves to the close button on open.
  - Focus trap within the dialog while open (`<dialog>` handles most of this; add keyboard handler for `Esc` and click-outside backdrop).
  - Body gets `overflow: hidden` while open to lock page scroll.
- Content source: parse `Character profiles/AngHulingBimbo_AllCastProfiles.txt` into a JS data array at build-time (inline the array in the HTML file — the file is small, ~15 records).

**Data model** (inline JS const):
```js
const CAST = [
  {
    id: 'younger-joy',
    role: 'Younger Joy Manawari',
    actor: 'Jullana Bernal',
    strand: 'HUMSS · Inclusivity',
    group: 'young',
    image: 'SOURCES/Images/Character Profiles/extracted/V1.png',
    characterBio: '…',
    actorBio: '…',
  },
  // …15 entries total
];
```

#### 4.5.4 Image mapping
All paths relative to project root.

| Character | Image path |
| --- | --- |
| Younger Joy Manawari | `SOURCES/Images/Character Profiles/extracted/V1.png` |
| Younger Emman Azarcon | `SOURCES/Images/Character Profiles/extracted/V1 (2).png` |
| Younger Hector Samala | `SOURCES/Images/Character Profiles/extracted/V1 (3).png` |
| Younger Anthony Cruz Jr. | `SOURCES/Images/Character Profiles/extracted/V1 (4).png` |
| Older Hector Samala | `SOURCES/Images/Character Profiles/extracted/v2_ OLDER VERS.png` |
| Older Emman Azarcon | `SOURCES/Images/Character Profiles/extracted/v2_ OLDER VERS (2).png` |
| Older Anthony Cruz Jr. | `SOURCES/Images/Character Profiles/extracted/v2_ OLDER VERS (3).png` |
| Older Joy Manawari | `SOURCES/Images/Character Profiles/extracted/v2_ OLDER VERS (4).png` |
| Arturo Banlaoi | `SOURCES/Images/Character Profiles/extracted/V1 (5).png` |
| Tiya Dely | `SOURCES/Images/Character Profiles/extracted/V1 (6).png` |
| Ligaya | `SOURCES/Images/Character Profiles/extracted/V1 (7).png` |
| Andrei Antonio | `SOURCES/Images/Character Profiles/extracted/V1 (8).png` |
| **Ms. Ima** | `Images/Updated Images for female partners/v2_ OLDER VERS.png` |
| **Mylene Azarcon** | `Images/Updated Images for female partners/v2_ OLDER VERS (2).png` |
| **Eleanor Cruz** | `Images/Updated Images for female partners/v2_ OLDER VERS (3).png` |

The Ms. Ima / Mylene / Eleanor swap replaces the current placeholder files ("It looks like a freaking comedy show*.png").

### 4.6 Tickets (Option A — Classic Split)
- Two-column grid 1fr/1fr, gap 60px. Stacks on mobile.
- **Left column:**
  - Stack of tier cards:
    - **Gold:** `border-color: rgba(245,200,107,0.5)`, `background: rgba(245,200,107,0.07)`, tier label, price `₱250` in Playfair 22–28px, seat count "300 seats · closer to the stage".
    - **Silver:** cooler tint `rgba(192,200,220,*)` with `#dde3ee` price.
    - **Bronze:** smaller row, opacity ~0.5, label "Coming soon".
  - Row of two showtime pills: "5:30 PM · Arrive by 4:00 PM · Entry closes 6:00 PM" and "9:00 PM · Arrive by 8:30 PM · Entry closes 9:30 PM".
  - Policy note (Inter 12–13px, muted): "GCash only · Tickets non-refundable · Free seating, first come first served · Confirmation sent via email, phone, or Facebook".
  - Primary CTA "Pre-Order via Google Form →" linking to the existing Google Form URL.
- **Right column:** floor-plan card
  - Header row: "Seating Chart" title + legend dots (gold / silver).
  - `<img src="SOURCES/Images/Floor Plan.png">` inside a 12px-radius frame with the shared shadow.

### 4.7 Venue
- Two-column grid 1fr/1fr, gap 64px.
- Left: "Where to Find Us" label + divider + heading "Lyceum of the Philippines / University – Davao" + body copy + date/showtimes in small caps + `.btn-ghost` "Open in Google Maps →".
- Right: `<iframe>` embed (`https://maps.google.com/maps?q=Lyceum+of+the+Philippines+University+Davao&output=embed`) at 100% width, `aspect-ratio: 4/3`, 12px radius, 1px `rgba(245,200,107,0.1)` border.

### 4.8 Footer
- Center-aligned stack, 80–110px padding.
- **Logo:** `SOURCES/Images/Logos/LPU-D Logo.png` (height ~56px). **This file does not yet exist in the repo — user must drop it in before implementation.** Fallback while missing: render JC logo and log a console warning.
- "Stay Connected" label, "Follow Us for Updates" heading.
- Two social buttons (pill, ghost style, Facebook SVG icon):
  - `https://www.facebook.com/LPUHumssAssoc` — "LPU HUMSS Assoc"
  - `https://www.facebook.com/LPUDavao` — "LPU Davao"
- Final primary CTA "Pre-Order Tickets Now".
- Copyright line: `© 2026 Ang Huling El Bimbo · Grade 12 HUMSS · LPU Davao Junior College`.

---

## 5. Interaction Details

### 5.1 Cast modal lifecycle
1. User clicks a cast card (or activates with `Enter`/`Space`).
2. JS reads `data-cast-id` from the card, looks up the entry in the inline `CAST` array.
3. JS populates the template dialog (portrait, role, names, bios), calls `dialog.showModal()`, sets `body.style.overflow = 'hidden'`, focuses the close button.
4. On dismiss (`Esc`, close button, or backdrop click):
   - Call `dialog.close()`, restore `body.style.overflow`, return focus to the originating card.

### 5.2 Scroll reveal
- Keep the existing IntersectionObserver pattern. Threshold 0.12, rootMargin `0px 0px -40px 0px`.
- `.reveal-delay-1..4` classes for stagger.

### 5.3 Nav scroll shadow
- At `scrollY > 40`, add `box-shadow: 0 4px 32px rgba(0,0,0,0.5)` and darken nav bg. Passive scroll listener.

---

## 6. Accessibility

- All interactive elements have visible `:focus-visible` rings (gold, 2px, 3px offset).
- Cast cards are `<button type="button">` — not divs.
- Modal uses `<dialog>` with `aria-labelledby` and focus trap.
- Color contrast: body copy `rgba(200,169,110,0.78)` on `#0d0500` passes WCAG AA for normal text. Gold `#F5C86B` on the same background passes for 18px+ / bold.
- `prefers-reduced-motion: reduce` disables scroll-reveal translate and removes the countdown glow animation (opacity fade remains).
- `alt` text on every image (character name + actor name for cast photos, "Official Ang Huling El Bimbo poster" for the poster, descriptive alt for the floor plan).

---

## 7. Technical Notes

### File layout
- `index.html` — single file, inline `<style>`, inline `<script>` with the `CAST` data array.
- No Tailwind CDN required after redesign; raw CSS is shorter and avoids the utility-class sprawl. If Tailwind is kept, scope it to layout-only utilities (grid, flex) and avoid color utilities.
- Google Fonts: `<link>` preconnect + `Playfair+Display:ital,wght@0,400;0,500;0,600;0,700;1,400` + `Inter:wght@400;500;600`.

### Dependencies to provide before shipping
- `SOURCES/Images/Logos/LPU-D Logo.png` — new asset from user.
- Verified images in `Images/Updated Images for female partners/` are the correct portraits for Ms. Ima, Mylene, Eleanor in that order (`v2_ OLDER VERS.png` → Ms. Ima, `(2).png` → Mylene, `(3).png` → Eleanor).

### Meta / SEO
- `<title>`: `Ang Huling El Bimbo — A HUMSS Musical · May 8, 2026`
- `<meta name="description">`: existing description is fine.
- OpenGraph: `og:title`, `og:description`, `og:image` pointing at `SOURCES/Images/Officicial poster.jpg`, `og:type=website`.
- Favicon: derived from JC logo (32×32, 180×180 apple-touch).
- `lang="en"`.

### Dev workflow
- Runs on existing `serve.mjs` at `http://localhost:3000`.
- No build step, no package install required.

---

## 8. Anti-Generic Guardrails (Enforced)

Checked against `MD FILES (CMUST READ)/VIbe Coded website must not know.md` and `SOURCES/MD_/CLAUDE.md`:

- No default Tailwind blues/indigos — custom gold palette only.
- No purple gradients.
- No sparkle/emoji UI (existing advisory ⚠️ emoji is retained **only** in plain-text copy, not as a UI decoration — design element is the colored left border).
- No aggressive hover lifts on cards (subtle 4–6px max, no rotation, no glow-under-cursor).
- No semi-transparent blurry nav with low contrast — nav uses solid dark bg with subtle shadow.
- Footer copy verified correct, no "all right reversed".
- One consistent radius scale (6/12/30), one shared shadow, one type pair.
- Every social link goes to a real profile.
- No `transition-all`.
- Loading states: the site is static HTML + one async call (the Google Maps iframe), so no additional skeletons needed. Cast portraits use `loading="lazy"` and `decoding="async"`.

---

## 9. Open Items for User

1. Drop `LPU-D Logo.png` into `SOURCES/Images/Logos/`.
2. Confirm the three files in `Images/Updated Images for female partners/` map to Ms. Ima / Mylene / Eleanor in that order.
