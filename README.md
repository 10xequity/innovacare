# Innovacare — website

Static marketing site for **Innovacare**, a global healthcare consulting firm working with health systems, biotech, and healthcare private equity.

**Version 1.0.0 · 2026-07-20**

No build step, no framework, no dependencies. Plain HTML, CSS, and vanilla JS — open a file or push the folder to any static host.

---

## Structure

```
innovacare/
├── index.html          Home (animated hero + all sections)
├── capabilities.html   Cost, interim/fractional leadership, optimization, new models
├── industries.html     Health systems · biotech · private equity
├── approach.html       The seven "In-" phases + engagement process
├── insights.html       Resource library (SEO abstracts)
├── faq.html            Deep FAQ + FAQPage structured data
├── about.html          Firm story, mission, track record
├── contact.html        Contact form + engagement start
├── 404.html            Not-found page (works on GitHub Pages)
├── robots.txt
├── sitemap.xml
├── css/
│   ├── tokens.css      Design tokens: color, type scale, spacing, motion
│   └── styles.css      All components and layout
├── js/
│   ├── hero.js         Canvas node-network hero (globe + cursor spotlight)
│   └── main.js         Nav, scroll reveals, FAQ accordion
└── assets/
    ├── favicon.svg
    └── og.png          Social share image (placeholder — replace with final art)
```

> `build.sh`, `gen1.sh`, `gen2.sh` are the generators used to stamp the shared
> nav/head/footer into each page. They are **not needed to run the site** and can
> be deleted; keep them if you'd like to regenerate pages after editing the shared
> chrome in `build.sh`.

## Run locally

Any static server works:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

Opening `index.html` directly also works; a server just makes relative paths behave exactly as in production.

## Deploy to GitHub Pages

1. Create a repo and push these files to the default branch.
2. Repo **Settings → Pages → Build and deployment → Source: Deploy from a branch**, branch `main`, folder `/ (root)`.
3. For the apex domain `www.innovacareusa.com`, add a `CNAME` file containing `www.innovacareusa.com` and point DNS at GitHub Pages per their docs.

## Wire up the contact form

`contact.html` currently uses a `mailto:` action, which opens the visitor's email client. For real inbox delivery, point the form at a handler:

- **Formspree** — replace the form `action` with `https://formspree.io/f/XXXX` and set `method="POST"`.
- **Basin / Getform / your endpoint** — same pattern.

Remove the `enctype="text/plain"` attribute when switching to a real handler.

## Design system (quick reference)

- **Color:** near-black `#070B14`, navy scale to `#24406B`, antique gold `#C6A15B` / bright `#E4C77E`, warm paper `#F3F1EB`. Gold is reserved for large text, lines, and UI; body copy stays off-white or ink for contrast.
- **Type:** Space Grotesk (display), IBM Plex Sans (body), IBM Plex Mono (labels/eyebrows). Loaded from Google Fonts.
- **Motion:** custom ease-out `cubic-bezier(0.23,1,0.32,1)`; UI transitions ≤ 220ms; hero uses a spring-eased cursor. All motion respects `prefers-reduced-motion` and the hero pauses when off-screen.
- Tokens live in `css/tokens.css` — change them there and the whole site follows.

## Content to replace before launch

The copy is written and ready, but a few things are intentionally left for you to supply with real, verifiable detail:

- **Outcome examples** (home + about) are drawn from the prior site's stated achievements and phrased qualitatively. Add real, client-approved metrics where you have them.
- **Insights** are SEO-ready abstracts. Expand each into a full article page when ready (the anchors and internal links already exist).
- **`assets/og.png`** is a generated placeholder — replace with final social art at 1200×630.
- **Contact email** is set to `innovacareusa@gmail.com` (carried over from the current site). Swap for a branded address when available.
- **`sameAs`** in the Organization schema (`index.html`) is empty — add LinkedIn / other profile URLs.

## Accessibility & SEO notes

- Semantic landmarks, `aria-current` on the active nav item, keyboard-operable menu and FAQ, visible focus states.
- Per-page `<title>`, meta description, canonical, and Open Graph tags; `FAQPage` and `Organization` JSON-LD; `sitemap.xml` and `robots.txt` included.
- Verify gold-on-navy contrast if you change the palette; current pairings target WCAG AA at the sizes used.
