# The Wedding Design Company — static rebuild

A faithful static recreation of the **wdcindia.com** homepage. No build step, no
dependencies — open it through any static file server.

```bash
python3 -m http.server 8848
# → http://localhost:8848
```

It must be served over HTTP rather than opened as a `file://` URL: the hero
engine and the Design Studio carousel read layout metrics that the file
protocol restricts.

## Layout

```
index.html        homepage markup
styles.css        full stylesheet (design tokens live in :root)
script.js         hero scene engine, carousels, nav, scroll reveal, WhatsApp contact form
assets/
  changes/        hero background, baraat layer, logo, atelier + service art
  changes/OVERLAY/  the 7 hero hotspot SVG overlays
  design-studio/  18 gallery photographs
  oldassets/      about / contact illustrations
  fonts/          Cormorant Garamond + Manrope (self-hosted woff2)
  icon/           scroll-to-top arrow
```

## Page structure

Hero → About (`#wdc-section`) → Wedding Atelier (`#atelier-section`) → Services
(`#service-section`) → Venues (`#venues-section`) → Design Studio
(`#design-section`) → Contact (`#contact-section`).

### The hero

A 2200×1557 illustrated streetscape that pans continuously and loops seamlessly.
Three layers move at different rates: the panorama, a foreground overlay group,
and the baraat parade at `BARAAT_SPEED_MULT` (1.1×) for depth. It supports
auto-travel and drag-to-travel, and carries seven hotspots — newsstand, WDC
office, services, contact, airplane, atelier and gallery — each pairing an
invisible anchor with an SVG overlay that lights up on hover.

The tuning constants sit at the top of `script.js`: `SCENE_LOOP_OFFSET`,
`FOREGROUND_SHIFT_RATIO`, `HERO_ZOOM`, `BARAAT_SPEED_MULT`.

### Design Studio

`script.js` holds the slide list (around line 135) and drives a 3D carousel on
desktop and a swipeable one on mobile.

## Content changes from the live site

- **Media page removed, in full.** Gone: the `Media` nav link, the newsstand
  hero hotspot and its overlay layer, the 345KB `WDC_NEWSTAND_OVERLAY.svg`, and
  every stylesheet rule that served it — `.newsstand-hotspot`, the base
  `.scene-foreground__img`, `.section-news__*`, and the whole news-archive
  sheet (`.news-card*`, `.news-archive*`, `.news-year*`, `.featured-news-*`).
  The hero now carries six hotspots instead of seven; the script keys off the
  generic `.hero-link-hotspot` class, so it needed no change.
- **Contact and About illustrations** now point at the Meghavi Comferts
  artwork (`comfert.jpeg`, `grpahic_contact.jpeg`). The favicon link is
  commented out — the old WDC icon was removed and has no replacement yet.
- **Venues section added** (`#venues-section`): established 1997, with the Taj
  group since 1997 — Taj Lake Palace, Taj Fateh Prakash, Udai Bagh, Taj Lalit
  Bagh. Reachable from the nav slot the Media link vacated.
- **Entertainment lineup added** to the Entertainment & Experiences service
  block: saxophone for the gala dinner, Qawwali setup, Langa setup, DJ party,
  guitarist & singer with band.
- **`info@meghavicomferts.in`** added to the contact details.

Styles for the two new blocks are appended at the bottom of `styles.css` under a
marked heading, built from the existing `:root` tokens.

## Differences from the live site

1. **Homepage only.** Links to the other pages (`team`, `services`, `contact`,
   `destinations`) point at `https://www.wdcindia.com/...` so nothing
   dead-ends. In-page anchors stay local.
2. **Contact form.** Form submits directly to WhatsApp (+91 80059 12079). No backend or PHP server required.

`_old-react-scaffold/` holds the earlier React/Vite DOM-snapshot attempt, kept
for reference; nothing in the live build reads from it.

