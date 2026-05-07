# Website Analysis & Improvement Offer

We analyzed the APA website (anthropartyargentina.com) to find issues and improvements. Below is everything we found, organized from critical to polish.

## The Offer to Organizers

The idea is: **you offer free web improvements in exchange for getting to know the organizers/staff**. You're not asking for anything — just "hey, I noticed some things on the site, here's a fix, no charge, just want to help and make some friends before the con."

### Sample message to send:

> "Holaa! Somos Ivan y Ale, vamos a APA desde Paraguay y estamos re emocionados. Trabajamos con desarrollo web y analizamos la página por si les sirve feedback. Encontramos varios temas (imágenes muy pesadas que ralentizan el sitio, algunos bugs técnicos, contenido faltante como FAQ/horarios). Les dejamos una lista organizada de todo. Si quieren les podemos dar una mano arreglando algunas cosas — sin costo, solo queremos ayudar y conocer gente antes del evento <3"

---

## CRITICAL ISSUES (fix these first)

### 1. Images are way too heavy — site loads slow

Three PNGs are massive and kill mobile performance:

| Image | Size | Problem |
|---|---|---|
| `honors_artwork.png` | **8.3 MB** | Takes 10+ seconds on mobile data |
| `landing_illustration_banner.png` | **7.4 MB** | Extreme |
| `about_hero_photo.jpg` | **4.8 MB** | Very large |
| `furcamp_shirt_design.png` | **2.5 MB** | Large |

**Fix**: Convert to WebP at 80% quality. Total payload drops from ~24MB to ~3MB. First paint goes from 10s to ~2s.

### 2. Leaking internal build URL to search engines

The booking and about pages show:
```html
<link rel="canonical" href="http://sveltekit-prerender/booking"/>
```

`http://sveltekit-prerender/` is the internal dev server — it's leaking to production and confuses Google.

**Fix**: Set `SVELTEKIT_PRERENDER_ORIGIN` env var to `https://anthropartyargentina.com` in the build config.

### 3. Reglamento page has wrong title tag

Both the homepage and reglamento page show `<title>APA - Anthro Party Argentina</title>`. The SvelteKit per-page title override isn't firing for the reglamento route.

**Fix**: Set the `<svelte:head>` title to `Reglamento | APA` on the reglamento page.

### 4. Duplicate hotel room photo

"Deluxe Twin" and "Classic TWIN" use **the exact same photo**. Guests comparing rooms can't tell the difference.

**Fix**: Replace the Deluxe Twin image with the correct photo.

---

## CONTENT GAPS (pages that should exist)

### 5. No schedule / cronograma page

The #1 thing attendees look for. Every other con (ArFF, Confuror) has one. Even a "coming soon" with rough slot structure would help.

### 6. No FAQ page

The reglamento has all the info, but nobody reads a legal document for "what time does it start?" or "can I bring my fursuit?" or "is there parking?". A FAQ page would reduce support questions dramatically.

### 7. No ticket/pricing page

The only way to see ticket options is to go to `/login` and register first. No public page showing "General vs Sponsor vs Super Sponsor" tiers, what each includes, early bird pricing. This kills conversions — curious visitors can't see prices without committing.

### 8. No Dealers Den / artist application page

The reglamento has detailed rules but nowhere for artists to actually apply. ArFF has `/calls.html` for this.

### 9. No gallery / past event photos

First-time attendees want to see what the vibe is like. No photos from Furcamp Cordobés to build trust.

### 10. No privacy policy

The login system collects personal data. A privacy policy page is legally advisable in some jurisdictions.

---

## DESIGN/UX ISSUES

### 11. Navigation is too thin

Only 3 items (Home, Hotel, About Us). Typical con nav: Home, Tickets, Hotel, Schedule, Dealers Den, About, FAQ, Rules, Gallery.

### 12. Footer icons missing alt text

All social icons use `alt="redes sociales"` (identical and useless). Should be `alt="Twitter/X"`, `alt="Instagram"`, `alt="Telegram"`. The team avatars use `alt="Image 1"` through `alt="Image 5"` — broken accessibility.

### 13. No Bluesky link

Every Argentine furry con is on Bluesky now (ArFF is). APA only has Twitter/X, Instagram, and Telegram.

### 14. CTA buttons all go to login

Every "Inscribite" button goes to `/login`. Consider differentiating — "Learn More" for some sections, "Book Room" for hotel, etc.

### 15. Hotel booking is manual

Users have to WhatsApp/email the hotel with a promo code instead of a direct booking link. If possible, get a direct booking portal from Hotel Quinto Centenario.

### 16. URL paths aren't clean

`/aboutus` instead of `/about` or `/sobre-nosotros`. The reglamento is buried under `/legal/reglamento` with no link in main nav (only in footer).

---

## SEO/TECHNICAL

### 17. No sitemap.xml

`/sitemap.xml` returns HTML instead of actual XML. This hurts Google indexing.

### 18. Only 5 pages total

For a con 9 months away, that's thin. Target 12-15 pages minimum.

### 19. No language switcher

ArFF has Spanish/English toggle. APA is Spanish-only. International furries want English.

---

## TL;DR — What to Actually Send

**Quick wins (1-2 hours)** if you want to offer concrete help:
- Convert the 3 massive PNGs to WebP (80% quality) — saves ~15MB total
- Fix the canonical URL leak (1 line in build config)
- Fix the reglamento page title (1 line in Svelte component)
- Add alt text to footer icons (5 minutes)

**Medium effort (half day)** if you want to impress them:
- Create a FAQ page by extracting Q&A from the reglamento
- Add a tickets/pricing comparison table
- Set up a proper sitemap.xml

This is genuine value you can offer for free. It costs you nothing and makes them remember you.
