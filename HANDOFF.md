# Innovacare — Developer & Stakeholder Handoff

**Version 2.0.0 · 2026-07-22 · Theme: White / Teal / Gold**

This document is the single reference for maintaining, editing, and shipping the Innovacare site. It assumes basic HTML/CSS familiarity but no framework knowledge.

---

## 1. What this is

A static marketing website — 8 content pages plus a 404 — for a global healthcare consulting firm. No framework, no build pipeline, no runtime dependencies. Everything renders from plain files. That is a deliberate choice: it deploys anywhere, has no supply chain to patch, and will still open in ten years.

**Status:** feature-complete and validated (HTML parses, JS lint-clean, JSON-LD valid, zero broken internal links). **Not yet cleared for public launch** — see the pre-launch checklist (§11), especially client-name verification.

## 2. Stack & principles

| Layer | Choice | Notes |
|---|---|---|
| Markup | Static HTML5 | One file per page; shared chrome stamped by generators |
| Styling | Hand-written CSS, custom properties | `tokens.css` (variables) + `styles.css` (components) |
| Behavior | Vanilla JS (ES5-safe) | Two small files, no libraries |
| Hero visual | Canvas 2D | `hero.js`, dependency-free |
| Fonts | Google Fonts | Space Grotesk, IBM Plex Sans, IBM Plex Mono |
| Hosting target | GitHub Pages | Any static host works |

Principles: light-first theme with dark-teal "feature bands" for rhythm; motion is transform/opacity only and ≤220ms; everything degrades without JS (`<noscript>` reveal fallback); accessibility is built in, not bolted on.

## 3. Repo map

```
index.html capabilities.html industries.html approach.html
insights.html faq.html about.html contact.html 404.html
robots.txt sitemap.xml README.md HANDOFF.md
css/tokens.css   css/styles.css
js/hero.js        js/main.js
assets/favicon.svg assets/og.png assets/art-network.png assets/art-flow.png
build.sh gen1.sh gen2.sh   ← generators (optional; not runtime)
```

## 4. Design system

**Color tokens** (in `css/tokens.css`):

| Token | Value | Use |
|---|---|---|
| `--white` / `--surface` | `#FFFFFF` / `#F4F7F7` | page & alternating sections |
| `--tint` | `#E2F0EE` | pale teal sections, art ground |
| `--teal` | `#0FA79E` | brand color, graphics, fills |
| `--teal-700` | `#0B6E68` | teal **text/links** on white (AA) |
| `--teal-deep` / `--teal-deep-2` | `#08403D` / `#062E2C` | dark bands, footer, CTA |
| `--gold` / `--gold-bright` | `#C6A15B` / `#E4C77E` | CTAs, accents, hairlines |
| `--ink` | `#0A2E2C` | body + heading text |
| `--on-dark` / `--on-dark-dim` | `#EAF5F3` / `#9EC3BF` | text on dark-teal bands |

Rule of thumb: **teal = system color, gold = call-to-action + accent, ink = text.** Never use bright `--teal` or `--gold` for small body text on white (contrast) — use `--teal-700` or `--ink`.

**Type scale:** fluid `--step--1` … `--step-5` (clamp-based). Display = Space Grotesk 500; body = IBM Plex Sans; labels/eyebrows = IBM Plex Mono uppercase.

**Spacing:** `--space-2xs` … `--space-3xl`. **Radius:** `--radius` 6px, `--radius-lg` 14px. **Motion:** `--ease-out`, `--dur-ui` 220ms.

## 5. Component inventory (CSS class → what it is)

- `.nav` — fixed header; `data-scrolled` toggles the blurred state, `data-open` the mobile drawer.
- `.hero` + `.hero__canvas[data-network]` — animated landing hero (light).
- `.pagehead` + `.pagehead__canvas` — dark-teal interior page header with lighter network.
- `.section` / `.section--paper` / `.section--tint` / `.section--navy` — section backgrounds. **`.section--navy` renders dark teal**, not navy (name kept for stability).
- `.card` / `.card--link` — feature/pillar cards; `.card__num`, `.card__list`.
- `.phase` — numbered process rows (`.phase__k` label).
- `.statband` / `.stat` — big-number band.
- `.outcome` — two-column outcome rows.
- `.case` — engagement snapshot card (`.case__client`, `.case__result`, `.case__body .tag`).
- `.clients` / `.client-mark` — typographic client wordmarks.
- `.res` — resource/reference rows.
- `.faq` / `.faq__item[data-open]` — accordion.
- `.cta` — dark-teal call-to-action card.
- `.artframe` / `.artframe--wide` / `.artframe__cap` — image/art holders (16:10).
- `.reveal[data-d="1..4"]` — scroll-in animation with stagger.

## 6. Page map

| Page | Purpose | Notable |
|---|---|---|
| `index.html` | Home | Hero, thesis + art, **clients strip**, capabilities, industries, approach teaser, outcomes, **case snapshots**, model band, insights teaser, short FAQ, CTA. `Organization` JSON-LD. |
| `capabilities.html` | Four pillars in depth + services matrix | anchors `#cost #leadership #optimization #models` |
| `industries.html` | Health systems / biotech / PE | anchors `#systems #biotech #private-equity` |
| `approach.html` | Seven "In-" phases + engagement process + operating principles | |
| `insights.html` | 8 SEO abstracts + **external references** (CMS, Joint Commission, AHRQ, HFMA, WHO) | anchors per article |
| `faq.html` | 14 grouped Q&As | `FAQPage` JSON-LD |
| `about.html` | Story, mission, **Selected clients**, track record, brand | art panel |
| `contact.html` | Form + what-to-include | `mailto:` action (see §9) |
| `404.html` | Not found | works on GH Pages |

## 7. The hero animation

`hero.js` finds every `<canvas data-network>` and renders a rotating globe of nodes with a spring-eased cursor spotlight. Tune per canvas via attributes:

- `data-nodes="160"` — node count (hero 160, page headers ~90).
- `data-spin="0.00035"` — base rotation speed.
- `data-pulses="off"` — disables the travelling data dots (used on page headers).

Colors live at the top of the file: `TEAL` (nodes/links) and `GOLD` (spotlight/pulses/halo). It pauses when scrolled off-screen (IntersectionObserver) and renders a single static frame under `prefers-reduced-motion`.

## 8. Common edits

- **Palette change:** edit values in `css/tokens.css` only. Update the hero's `TEAL`/`GOLD` arrays in `js/hero.js` and `theme-color` meta if you change brand color.
- **Add a nav item:** edit `nav_html()` in `build.sh`, then `bash gen1.sh && bash gen2.sh`, and hand-edit the nav block in `index.html` (it's inline there).
- **Add an FAQ:** add a `.faq__item` in `gen2.sh`, and mirror it in the `FAQ_LD` JSON-LD block, then regenerate. Keep the two in sync.
- **Add an insight:** copy an `<article class="outcome">` block in `gen2.sh`, give it a unique `id`, regenerate.
- **Add/remove a client:** edit the `.clients` block in `index.html` and the one in `gen2.sh` (about), regenerate. See §11.
- **Swap art:** replace `assets/art-network.png` / `art-flow.png` (any 16:10 image) or point the `.artframe img src` elsewhere.
- **Real form:** in `contact.html`, change `action` to your handler URL, set `method="POST"`, remove `enctype="text/plain"`.

## 9. Generators

The shared header, nav, and footer live once in `build.sh` as shell functions. `gen1.sh` builds capabilities/industries/approach; `gen2.sh` builds insights/faq/about/contact/404. They inject `aria-current` on the active nav item automatically. **`index.html` is hand-maintained** (its hero is bespoke) — the generators do not touch it. After editing shared chrome: `bash gen1.sh && bash gen2.sh`. The generators are not needed to run or deploy the site.

## 10. Deployment

1. Push to the default branch of a GitHub repo.
2. Settings → Pages → Deploy from a branch → `main` / root.
3. Custom domain: add `CNAME` file with `www.innovacareusa.com`; set DNS per GitHub Pages docs. HTTPS is automatic once DNS resolves.

## 11. Pre-launch checklist

- [ ] **Verify all four client names are real, permissioned relationships** (Kaiser Permanente, Stanford, Medtech Global, Apricity Health). Remove any that are not. Highest risk: Kaiser, Stanford. Blocks launch.
- [ ] Point the contact form at a real handler; test a submission end-to-end.
- [ ] Replace `innovacareusa@gmail.com` with a branded address (search all files).
- [ ] Fill `sameAs` in `index.html` Organization schema (LinkedIn etc.).
- [ ] Replace generated art with final art/photography if desired.
- [ ] Confirm outcome/case wording is accurate; add approved figures if available.
- [ ] Set the real canonical domain if not `innovacareusa.com`.
- [ ] Run Lighthouse; validate schema at search.google.com/test/rich-results.
- [ ] Add analytics if wanted (none is included).

## 12. Accessibility & SEO

Semantic landmarks; `aria-current="page"` on active nav; keyboard-operable menu (Escape closes) and FAQ; visible focus rings; `prefers-reduced-motion` honored; no-JS `.reveal` fallback keeps content visible. Per-page title/description/canonical/OG, `Organization` + `FAQPage` JSON-LD, `sitemap.xml`, `robots.txt`. Contrast pairings target WCAG AA at the sizes used.

## 13. Browser support & performance

Modern evergreen browsers (Chrome, Edge, Firefox, Safari). Uses `backdrop-filter`, `aspect-ratio`, `clamp()`, `svh` — all broadly supported in 2025+. No images above the fold except fonts; hero is canvas. Total page weight is small; the two art PNGs (~280 KB each) are `loading="lazy"`.

## 14. Known risks / open items

- **Client-name accuracy** (see §11) — the one hard blocker.
- Contact form is `mailto:` until a handler is wired.
- `og.png` and the two art PNGs are generated placeholders; fine to ship, better to replace.
- Insights are abstracts, not full articles yet; anchors exist for expansion.

## 15. Version history

- **2.0.0 — 2026-07-22** — Rebrand to White/Teal/Gold; light-first restyle; teal/gold hero; Selected Clients (home + about); case snapshots; external references; generative art; handoff doc.
- **1.0.0 — 2026-07-20** — Initial build: navy/black/gold, 8 pages + 404, animated hero, full SEO scaffold.
