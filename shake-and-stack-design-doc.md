# Shake & Stack — Website Design Document

**Type:** Multi-page marketing/informational website (no built-in checkout — links out to delivery platforms)
**Status:** Design doc — ready to hand to Claude Code for implementation

---

## 1. Project Overview

- **Business:** Shake & Stack — burger & milkshake restaurant
- **Primary goals of the site:**
  - Show off the menu, photos, and personality of the shop
  - Make it effortless to find hours/location and get in the door
  - Route all online orders out to existing DoorDash / Uber Eats listings (no custom ordering system)
  - Surface strong Google reviews to build trust with new visitors
  - Give visitors playful reasons to explore the whole site (scavenger hunt, community burger builder)
- **Tech approach:** Frontend as a static site (HTML/CSS/JS), free hosting (Netlify or Vercel), optional ~$12/yr custom domain, plus a lightweight backend (see §11) to support the scavenger hunt discount codes and the community burger builder
- **Build phases:**
  - **Phase 1 (v1 launch):** Core informational site — Home, Menu, About, Gallery, Contact, reviews carousel, order-online links, scavenger hunt
  - **Phase 2 (v2):** Community Burger Builder — bigger in scope (submissions, voting, moderation, monthly cycle), planned as a follow-on so the core site can ship sooner

---

## 2. Brand & Visual Identity

The exterior signage, interior decor, and food styling already give us a strong identity to build from — the site should feel like a natural extension of the physical space, not a generic template.

### Color Palette

| Role | Color | Notes |
|---|---|---|
| Primary — Brand Yellow | `#F5C518` | Pulled directly from exterior signage |
| Ink / Text | `#1A1A1A` | Matches the black paneling & awning |
| Accent — Burgundy | `#7A2331` | Matches the door pillar trim |
| Accent — Blossom Pink | `#F4C2C2` | Soft accent from the cherry blossom decor, used sparingly |
| Background — Cream | `#FFF8EC` | Warm off-white, avoids stark clinical white |

### Typography

- **Headings:** Bold, condensed display font (e.g. *Anton*, *Bebas Neue*, or *Archivo Black*) — echoes the thick block lettering on the exterior sign
- **Body:** Clean, readable sans-serif (e.g. *Inter* or *Poppins*) for menu text, descriptions, hours

### Iconography & Imagery

- Use hand-drawn/doodle-style icons (burger, milkshake, fries) as recurring accents — this style is already established on the tray liners and exterior panels, so it should carry through into the site (section dividers, bullet icons, loading states, etc.)
- Photography should stay bright, warm, and a little informal — closer to what's on the tray liner than a polished studio shoot
- Interior shots (string lights, cherry blossom wall, wood paneling) are great for the About/Gallery section to convey atmosphere, not just food

---

## 3. Site Architecture (Sitemap)

```
Home  |  Menu  |  About  |  Gallery  |  Contact
```

- **Home** (`index.html`) — first impression, highlights, CTAs
- **Menu** (`menu.html`) — full menu with categories
- **About** (`about.html`) — story + atmosphere
- **Gallery** (`gallery.html`) — exterior/interior/food photo grid
- **Contact** (`contact.html`) — map, hours, contact form

A persistent "Order Online" button lives in the header/nav on every page — it's the single most important conversion action on the site.

---

## 4. Page-by-Page Layout

### 4.1 Home
1. **Header/Nav** — logo, nav links, sticky "Order Online" button
2. **Hero** — full-width photo (exterior night shot works great — neon "OPEN" sign has real charm), overlay with name + tagline, two CTAs: `View Menu` / `Order Online`
3. **Quick info strip** — today's hours, address, phone (icon row)
4. **Featured menu items** — 3–4 cards with photos, pulled from Menu page *(placeholder images until owner-provided photos are available)*
5. **Burger variation carousel** — full rotating carousel cycling through every burger on the menu *(placeholder — pitch this to the owner for intentionally shot photos: consistent background/surface, consistent angle & lighting, name + price visible per shot; see §4.2)*
6. **Reviews carousel** — rotating 4–5★ reviews only (see §5)
7. **Order online section** — DoorDash / Uber Eats buttons with platform logos
8. **Footer** — address, hours, social links, small map, scavenger-hunt progress tracker (see §9)

### 4.2 Menu
- Categorized sections: Burgers, Shakes, Sides, Drinks
- Each item: name, short description, price, optional photo *(placeholder images until owner-provided photos are available)*
- **Burger variation carousel** anchors the top of the page — one consistently-shot photo per burger, cycling through all variations
- Sticky category jump-nav for fast scrolling on mobile
- Repeat Order Online CTAs at the bottom

### 4.3 About — *(placeholder page until owner provides content)*
- Short story/history of the shop
- Warm, personal tone
- A few interior photos woven into the text (cherry blossom wall, lighting) to convey the atmosphere, not just list facts

**Questions to bring to the owner for this page:**
- How'd the name "Shake & Stack" come about?
- Any family/personal story behind starting the shop?
- What do they want customers to know that they *can't* get from just looking at the menu?
- Anything about the space itself worth mentioning (the cherry blossom wall, the lanterns — is there a story there)?

### 4.4 Gallery — *(placeholder — open to whichever photos the owner wants displayed)*
- Grid layout: Exterior / Interior / Food, filterable by tab if desired
- Lightbox on click (a free JS lightbox library is fine — no backend needed)

### 4.5 Contact
- Embedded Google Map (free embed, no API key required)
- Address, phone, email
- Hours table
- Simple contact form (Formspree free tier works well since there's no backend)
- Social links

---

## 5. Reviews Carousel — Functional Spec

- **Source:** Manually curated (Google Places API caps results at 5, so full automation adds complexity for little benefit)
- **Filter:** Only 4★ and 5★ reviews included
- **Rotation:** Refreshed quarterly — someone edits a small `reviews.json` file and redeploys (~10 min task)
- **Display:** CSS/JS carousel, auto-rotating with manual arrows, 1 review visible at a time on mobile / 2-3 on desktop

Example data shape:
```json
[
  { "author": "Jane D.", "rating": 5, "text": "Best burger in the neighborhood.", "date": "2026-05" },
  { "author": "Mike T.", "rating": 4, "text": "Great shakes, friendly staff.", "date": "2026-04" }
]
```

---

## 6. Order Online Integration

- Buttons link out directly to the existing DoorDash and Uber Eats restaurant pages
- Use each platform's official logo/button style for instant recognition
- No custom cart or checkout — this keeps the site free to run and easy to maintain

---

## 7. Technical Recommendations (for Claude Code build)

- **Frontend stack:** Plain HTML/CSS/JS, or a lightweight static site tool (e.g. Astro) if you want components/reuse across pages
- **Backend:** Supabase (free-tier Postgres + auth + file storage) recommended over a hand-rolled server — covers both the scavenger hunt's code counter and the Community Burger Builder's submissions/votes/images with minimal backend code
- **Hosting:** Netlify or Vercel free tier (frontend), Supabase free tier (backend/DB)
- **Suggested folder structure:**

```
/
├── index.html
├── menu.html
├── about.html
├── gallery.html
├── contact.html
├── community.html          (Phase 2 — community burger builder)
├── css/
│   └── style.css
├── js/
│   ├── main.js              (nav, mobile menu)
│   ├── carousel.js          (reviews + burger variation carousels)
│   ├── lightbox.js          (gallery lightbox)
│   ├── scavenger-hunt.js    (sticker tracking + code claim)
│   └── burger-builder.js    (Phase 2 — build/vote/submit)
├── images/
│   ├── exterior/
│   ├── interior/
│   ├── food/
│   └── ingredients/         (Phase 2 — illustrated ingredient layers)
└── data/
    └── reviews.json
```

- Mobile-first, responsive layout (single-column stacking, hamburger nav under ~768px)
- Reviews stored in `data/reviews.json` for easy quarterly hand-edits

---

## 8. Scavenger Hunt — Functional Spec

- **Concept:** One hidden burger-sticker icon per subpage (Home, Menu, About, Gallery, Contact = 5 total). Finding all 5 unlocks an in-store discount code.
- **Discoverability:** Subtle wiggle/glow animation so it's findable but not frustrating; must be tap-friendly on mobile, not hover-only
- **Progress tracking:** Client-side (e.g. `localStorage`) tracks which stickers a visitor has found, shown as a small "3/5 found" tracker in the footer
- **Limited monthly codes:**
  - Only **20 codes given out per calendar month**
  - Backend (Supabase) holds a `discount_codes` table: `{ month, count, limit }`
  - Completing the hunt calls a backend function that **atomically increments the counter** (avoids race conditions if two people finish at the same moment) and returns the code only if `count <= limit`
  - If the month's codes are exhausted, show a friendly "This month's codes are claimed — new codes drop next month!" message instead
  - Counter naturally resets by querying on current month — no manual reset needed
- **Anti-abuse:** Tie a completion to something more durable than pure `localStorage` (e.g. a lightweight email capture, or a session token) so one visitor can't clear their browser and re-claim repeatedly, eating into the monthly pool

---

## 9. Community Burger Builder (Phase 2 Feature)

A bigger, standalone feature layered on after the core site ships — turns "build your burger" from a one-off quiz into an ongoing community platform.

### Concept
- Visitors build a burger from a fixed set of ingredients
- Submissions are public and upvotable by other visitors
- The top-voted burger each month becomes that month's **"Monthly Burger"** — displayed on a dedicated page and (pending owner buy-in) actually added to the real menu
- **Community Burgers page** — browse all submissions, filterable by month

### Visual Design Approach (solves the "how do I make this look good" problem)
Rather than compositing real food photos (messy, inconsistent), build burgers from **illustrated ingredient layers**:
- Commission a small, one-time set of flat, hand-drawn ingredient illustrations (bun top, bun bottom, patty, cheese, lettuce, tomato, onion, bacon, sauce, etc.) in the same doodle-icon style already used on the tray liner and signage
- Each ingredient is a transparent PNG/SVG at the same width/perspective
- The builder stacks selected layers live via CSS (absolute positioning) — consistent, on-brand results every time, no photography needed per submission
- This turns an ongoing design problem into a one-time illustration pass (~10–15 ingredient assets)

### Data Model (rough sketch)
- `burgers`: `id, creator_name (optional), ingredients[], created_at, month`
- `votes`: `id, burger_id, voter_identifier, created_at` — one vote per identifier per burger (session token or similar, same anti-abuse pattern as §9)
- `monthly_winner`: `month, burger_id` — computed/cached at month-end

### Things to Settle Before Building
- **Moderation:** user-submitted names/content need at least basic filtering or a report/flag mechanism before going live publicly
- **Owner buy-in on the prize:** "winner becomes a real menu item" is an operational commitment (kitchen sourcing/prep), not just a website feature — worth confirming with the owner early, separate from the tech build
- **Scope:** recommended as a v2 launch after the core site is live, given it's meaningfully bigger than the rest of the site (submissions, voting, moderation, monthly cycle)

---

## 10. Content/Asset Checklist

What's needed before build:
- [ ] Final menu items + prices
- [ ] Intentionally-shot burger photos for the burger variation carousel (consistent background, angle, lighting — pitch to owner)
- [ ] Short "our story" write-up for About page (see questions in §4.3)
- [ ] Any additional photos the owner wants displayed (Gallery)
- [ ] Hours of operation
- [ ] DoorDash / Uber Eats profile links
- [ ] 8–10 curated 4–5★ Google reviews
- [ ] Social media handles
- [ ] Phone number / email for contact form
- [ ] *(Phase 2)* Illustrated ingredient layer set for the Community Burger Builder

---

## 11. Next Steps

1. Review and adjust this doc — flag anything that doesn't feel right
2. Pitch the burger variation photos and About Us content to the owner
3. Once Phase 1 is finalized, hand this file directly to Claude Code in VS Code as the project brief to scaffold the actual site
4. Revisit §9 (Community Burger Builder) as a Phase 2 planning session once the core site is live
