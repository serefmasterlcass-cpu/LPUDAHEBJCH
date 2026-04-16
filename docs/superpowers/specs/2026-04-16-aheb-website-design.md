# AHEB Website Design Spec
**Date:** 2026-04-16  
**Project:** Ang Huling El Bimbo (AHEB) — Musical Theatre Website  
**Output:** Single `index.html` file in `/home/seref/AHEB/AHEB WEBSITE]/`

---

## Overview

A single-page cinematic scroll website for *Ang Huling El Bimbo*, a musical theatre production by Grade 12 HUMSS students of LPU Davao Junior College. The site serves as an information hub and ticket pre-order portal for the show.

---

## Event Details

| Field | Value |
|---|---|
| Show Name | Ang Huling El Bimbo (AHEB) |
| Date | May 8, 2026 |
| Showtimes | 5:30 PM and 9:00 PM |
| Venue | Lyceum of the Philippines University – Davao |
| Produced by | Grade 12 HUMSS, LPU Davao Junior College |
| Pre-order link | https://docs.google.com/forms/d/e/1FAIpQLSdaMbFQRliTzhKOOuyQMsnJkjWaXBnyfqlir1jmQwziyClBZg/viewform?usp=sharing&ouid=107343474237936976300 |
| Facebook (Main) | https://www.facebook.com/LPUHumssAssoc |
| Facebook (Head) | https://www.facebook.com/LPUDavao |

---

## Design Direction

**Style:** Warm & Nostalgic  
**Palette:** Deep ambers (`#0d0500`, `#1a0800`, `#3d1a00`) + gold accents (`#FFD700`) + warm cream body text (`#c8a96e`, `#e8d5b0`)  
**Typography:** Georgia/serif for display headings; system sans-serif for labels, nav, body detail text  
**Heading tracking:** `-0.02em` on large titles  
**Body line-height:** `1.75`  
**Texture:** Subtle SVG noise grain overlay on hero  
**Gradients:** Layered radial gradients; no flat fills  
**Shadows:** Color-tinted, low-opacity layered shadows  
**Animations:** `transform` and `opacity` only — no `transition-all`  
**Interactive states:** Every clickable element has hover, focus-visible, and active states

---

## Layout: Cinematic Scroll

Single `index.html`, all styles inline, Tailwind CSS via CDN, mobile-first responsive.

Fixed transparent navigation bar at the top (blurs on scroll).

Sections in order:

### 1. Navigation Bar (fixed)
- Logo: "AHEB" in gold, uppercase, wide tracking
- Links: About · Cast · Tickets · Venue
- CTA button: "Pre-Order" — gold pill button
- Background: `rgba(13,5,0,0.85)` with `backdrop-filter: blur(12px)`
- Bottom border: `1px solid rgba(255,215,0,0.12)`

### 2. Hero Section
- Full viewport height (`min-h-screen`)
- Background: layered radial gradients in amber/brown + SVG grain noise overlay
- Content: Production tag line → Giant serif title "ANG HULING EL BIMBO" → date + venue subtitle → two CTAs ("Pre-Order Tickets" primary gold pill, "Learn More" ghost pill)
- The official poster image (`Officicial poster.jpg`) is displayed as a right-aligned floating image (roughly 40% width) beside the hero text, with a subtle gradient overlay (`bg-gradient-to-t from-black/60`) and slight drop shadow for depth

### 3. Countdown Timer
- Full-width band between Hero and About
- Dark background with subtle gold border top and bottom
- Displays days / hours / minutes / seconds counting down to May 8, 2026 00:00:00
- Live JavaScript countdown, updates every second
- Section label: "Opening Night In"

### 4. About the Show
- Two-column layout: text left, poster image right
- Text: heading "A Story of Love, Resilience & Discovery" + body paragraph about the HUMSS production
- Viewer advisory callout box (amber left-border): "Contains mature themes, strobe lights, and loud sounds. Not suitable for very young or sensitive audiences."
- Poster: `Officicial poster.jpg`

### 5. Cast & Characters
- Heading + subtitle
- Grid of cast cards (responsive: 4-col desktop, 2-col tablet, 1-col mobile)
- Uses character profile images from `Images/Character Profiles/PAVILION POSTER.zip` — extract and display each character
- Each card: portrait image + character name + role label
- If zip contents are unavailable, use styled placeholder cards

### 6. Tickets & Pre-Order
- Two-column layout: ticket tiers left, floor plan right
- **Ticket tiers:**
  - Gold Tier — ₱250 — 300 seats (highlighted with gold border)
  - Silver Tier — ₱200 — 400 seats
  - Bronze Tier — Coming Soon (muted/disabled appearance)
- **Showtime pills:** 5:30 PM (arrive 4:00 PM, entry closes 6:00 PM) and 9:00 PM (arrive 8:30 PM, entry closes 9:30 PM)
- **Payment notes:** GCash only · Non-refundable · Free seating (first-come, first-served) · Confirmation via email/phone/Facebook
- **CTA button:** "Pre-Order via Google Form →" links to the Google Form URL
- **Floor plan:** `Images/Floor Plan.png` displayed on the right with stage/tier labels visible

### 7. Venue & How to Get There
- Two-column: details left, Google Maps embed right
- Venue name: Lyceum of the Philippines University – Davao
- Map: embedded Google Maps iframe searching for "Lyceum of the Philippines University Davao" — use a standard Google Maps embed URL pointing to this location
- Show date + showtimes repeated for easy reference

### 8. Footer / Social
- Centered layout
- Heading: "Follow Us for Updates"
- Two Facebook link buttons: "LPU HUMSS Assoc" and "LPU Davao"
- Final CTA: "Pre-Order Tickets Now" gold pill
- Copyright line: "© 2026 Ang Huling El Bimbo · Grade 12 HUMSS · LPU Davao Junior College"
- LPU Davao Junior College logo (`Images/Logos/JC Logo.png`) displayed in footer

---

## Assets

| File | Usage |
|---|---|
| `Officicial poster.jpg` | Hero background/visual + About section |
| `Images/Logos/JC Logo.png` | Footer |
| `Images/Floor Plan.png` | Tickets section |
| `Images/Character Profiles/PAVILION POSTER.zip` | Cast section (extract images) |

All assets referenced by relative path from `index.html` in the project root.

---

## Technical Constraints

- Single `index.html` file, all styles inline (per CLAUDE.md)
- Tailwind CSS via CDN: `<script src="https://cdn.tailwindcss.com"></script>`
- Mobile-first responsive
- No `transition-all` — only animate `transform` and `opacity`
- Serve and screenshot via `node serve.mjs` on `http://localhost:3000`
- Screenshots saved to `./temporary screenshots/`
- Do at least 2 screenshot comparison rounds against mockup before declaring done

---

## Out of Scope

- Backend / form handling (pre-order links to external Google Form)
- User accounts or ticketing system
- Multiple pages
- CMS or editable content
