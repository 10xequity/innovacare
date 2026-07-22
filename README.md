# Innovacare — website

Static marketing site for **Innovacare**, a global healthcare consulting firm working with health systems, biotech, and healthcare private equity.

**Version 2.0.0 · 2026-07-22 · Theme: White / Teal / Gold**

No build step, no framework, no dependencies. Plain HTML, CSS, and vanilla JS — open a file or push the folder to any static host.

---

## Structure

```
innovacare/
├── index.html          Home (animated hero, clients strip, case snapshots, all sections)
├── capabilities.html   Cost, interim/fractional leadership, optimization, new models
├── industries.html     Health systems · biotech · private equity
├── approach.html       The seven "In-" phases + engagement process
├── insights.html       Resource library + external References & further reading
├── faq.html            Deep FAQ + FAQPage structured data
├── about.html          Firm story, mission, Selected clients, track record
├── contact.html        Contact form + engagement start
├── 404.html            Not-found page (works on GitHub Pages)
├── robots.txt
├── sitemap.xml
├── css/
│   ├── tokens.css      Design tokens: color, type scale, spacing, motion
│   └── styles.css      All components and layout (light-first + dark-teal bands)
├── js/
│   ├── hero.js         Canvas node-network hero (globe + cursor spotlight)
│   └── main.js         Nav, scroll reveals, FAQ accordion
└── assets/
    ├── favicon.svg
    ├── og.png          Social share image (1200×630, generated)
    ├── art-network.png Generative teal/gold art — home thesis panel
    └── art-flow.png    Generative teal/gold art — about panel
```

> `build.sh`, `gen1.sh`, `gen2.sh` are the generators that stamp the shared
> nav/head/footer into each page. They are **not needed to run the site** and can
> be deleted. Keep them if you want to regenerate pages after editing shared
> chrome in `build.sh`, then run `bash gen1.sh && bash gen2.sh`.

## Run locally

```bash
python3 -m http.server 8000     # then open http://localhost:8000
```

## Deploy to GitHub Pages

1. Push these files to the default branch.
2. **Settings → Pages → Deploy from a branch**, branch `main`, folder `/ (root)`.
3. For `www.innovacareusa.com`, add a `CNAME` file containing `www.innovacareusa.com` and point DNS at GitHub Pages.

## Wire up the contact form

`contact.html` uses a `mailto:` action. For real inbox delivery, point the form at a handler (Formspree `https://formspree.io/f/XXXX` with `method="POST"`, or Basin/Getform/your own endpoint) and remove `enctype="text/plain"`.

## Design system (quick reference)

- **Color:** white `#FFFFFF` and soft surface `#F4F7F7`; brand teal `#0FA79E` (text-safe `#0B6E68`); dark bands `#08403D`/`#062E2C`; antique gold `#C6A15B` / bright `#E4C77E`. Teal is the system color; gold is reserved for CTAs, accents, and hairlines. Body text is ink `#0A2E2C`.
- **Sections:** default white; `.section--paper` soft surface; `.section--tint` pale teal; `.section--navy` is the **dark-teal feature band** (light text) — the class name is kept from v1 for markup stability but now renders teal, not navy.
- **Type:** Space Grotesk (display), IBM Plex Sans (body), IBM Plex Mono (labels). Google Fonts.
- **Motion:** ease-out `cubic-bezier(0.23,1,0.32,1)`, UI ≤ 220ms, spring-eased hero cursor. Respects `prefers-reduced-motion`; hero pauses off-screen.
- Change tokens in `css/tokens.css` and the whole site follows.

## ⚠️ Verify before publishing — client names

`index.html` and `about.html` list **Kaiser Permanente, Stanford, Medtech Global, and Apricity Health** as clients (typographic wordmarks, not the organizations' trademarked logos). HTML comments mark each block.

**Confirm every one is a real, permissioned client relationship before this site goes public.** Naming real organizations as clients without a genuine, permissioned relationship is a false-advertising and trademark risk — most acute for Kaiser Permanente and Stanford. Remove any name that isn't verified. Note this also softened the site's confidentiality language: engagement *details* are still described as confidential, but named clients are shown "with permission."

## Content to replace before launch

- **Outcome / case snapshots** (home + about) are phrased qualitatively from the prior site's stated achievements. Add real, client-approved specifics/figures where you have them.
- **Insights** are SEO-ready abstracts; expand each into full article pages (anchors/links already exist).
- **External references** (insights) link to CMS, The Joint Commission, AHRQ, HFMA, and WHO as standards/sources — not affiliations. Adjust as desired.
- **Contact email** is `innovacareusa@gmail.com` (carried from the current site). Swap for a branded address.
- **`sameAs`** in the Organization schema (`index.html`) is empty — add LinkedIn/other profile URLs.
- **Art** (`art-network.png`, `art-flow.png`) is generated abstract work. Replace with commissioned art or real photography if preferred; `.artframe` slots are sized 16:10.

## Accessibility & SEO

- Semantic landmarks, `aria-current` on active nav, keyboard-operable menu and FAQ, visible focus rings, no-JS reveal fallback.
- Per-page `<title>`, meta description, canonical, Open Graph; `FAQPage` + `Organization` JSON-LD; `sitemap.xml` + `robots.txt`.
- Color pairings target WCAG AA at the sizes used; re-check contrast if you change the palette.
