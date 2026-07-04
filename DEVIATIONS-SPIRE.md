# DEVIATIONS-SPIRE.md

Deviation and decision log for the spiregroupinc.com redesign + Ariel integration (autonomous session, 2026-07-02).

## Phase A recon (verdict)

- Repo confirmed: homepage hero-eyebrow contains "Clinical SaaS & AI Healthcare Platforms" (integration not yet run state).
- Integration state verdict: **INTEGRATION_ABSENT**. No "Meet Ariel" section, no Ariel nav item, no ariel.html page exist.
- Stack: static hand-authored HTML, one file per page. No build step.
- Deploy: GitHub Pages. CNAME = spiregroupinc.com. Remote = github.com/DDApp2025/spiregroupinc.com (branch main). Deploy = commit + push to main.
- Form mechanism: Formspree. Homepage form posts to https://formspree.io/f/mvgkrwre with a fetch() JSON handler. contact.html uses the same pattern.
- URL convention: flat `.html` files at repo root (e.g. what-we-build.html). Blog posts live under /blog/ and use root-relative `/page.html` nav links.
- Design tokens (from index.html :root): --ink #0a0a0f, --paper #f5f3ee, --cream #ede9e0, --steel #1c2333, --accent #b8470b, --accent2 #2a4a6b, --muted #3d3a34, --rule rgba(10,10,15,0.12), Helvetica stack for all three font vars.
- Hero (before): two-column grid. hero-left = eyebrow + h1 + hero-sub + hero-actions (2 buttons). hero-right = dark steel card (.platform-card + .stat-row). hero-left::after = vertical hairline on left column's right edge. hero-right::before = radial-gradient decoration on the dark card.
- Section order: hero, #what (What We Build, cards 01-07), #casestudy, #stack, #ip (Credentials), #about, #contact, footer.
- Nav (root pages): What We Build, Case Study, Stack, Credentials, About, Blog, Contact Us + "Start a Project" CTA. Uniform across root pages; blog pages use `/`-prefixed hrefs; deck pages use a separate minimal "Back to Site" nav.

## Decisions

- **Nav placement of Ariel**: inserted as a nav-links item immediately after "What We Build" (before "Case Study") on every root and blog page. Footer: "Ariel" link added to `.footer-links` before "Terms of Service".
- **Deck pages excluded from nav/footer Ariel injection**: deck/*.html use a deliberately minimal password-gated "Back to Site" nav and their own footer/no footer. Adding main-nav items there would break their design intent. They are an internal pitch deck, not part of the public marketing IA. Documented exclusion.
- **medspa removal scope**: per Part 2.1, "medspa"/"med spa"/"med-spa" removed from metadata and JSON-LD site-wide only (title, meta description/keywords, OG, Twitter, JSON-LD). Body-copy occurrences on blog posts and other pages are out of the stated scope and left intact; no new medspa term is introduced in any content authored/modified this session.
- **Hero entrance animations**: the existing fadeUp animation classes (.hero-eyebrow, h1, .hero-sub, .hero-actions) are retargeted to the relocated text block (now in the right column). New classes .hero-graphic (left) and its children get their own fadeUp timing. The retired .platform-card / .stat-row animation rules are removed with the card.
- **hero-left::after / hero-right::before**: both retired. The vertical hairline is reintroduced as a divider appropriate to the new layout (on the text column). The radial-gradient belonged to the dark card and is removed with it.

## Animations retargeting (Part 1.5 record)

- .hero-eyebrow, h1, .hero-sub, .hero-actions keep their fadeUp animations (now in the right column).
- The retired .platform-card and .stat-row fadeUp rules were removed; .hero-graphic (left column) now carries a fadeUp at 0.3s.
- hero-left::after retired; replaced by .hero-copy::before vertical hairline (desktop only, hidden on mobile).
- hero-right::before radial gradient retired with the dark card.

## Rendered visual QA (Playwright, bundled Chromium)

Ran C:\Users\quail\AppData\Local\Temp\claude\...\scratchpad\pwqa\qa.js. Edge (msedge channel) launched but handed off to an existing Edge instance and exited immediately (Windows single-instance behavior), so QA used Playwright's bundled Chromium instead. Screenshots are untracked in the scratchpad pwqa folder.

Results (home and ariel at 1280 / 768 / 375):
- Horizontal overflow: 0 px at all six page/width combinations (after fixing a pre-existing About-numbers overflow at 375 by reducing .about-num-item padding and .about-num font-size under max-width:640).
- Hero SVG geometry: zero text elements overlap the ring bbox; zero text elements fall outside the viewBox at all widths. Verified via getBBox() against the outer ring bbox.
- Column behavior: 1280 side-by-side (graphic left, copy right); 768 and 375 stacked with the text column above the graphic (text first).

Screenshot filenames (untracked): shot-home-1280.png, shot-home-768.png, shot-home-375.png, shot-ariel-1280.png, shot-ariel-768.png, shot-ariel-375.png.

## GO-day swap points (restated)

1. Homepage Ariel section: secondary demo-number CTA is built but wrapped in an HTML comment marked "PENDING LIVE-FIRE GO 2026-07". Uncomment and set the live number at go.
2. Ariel page walkthrough form: posts to the existing Formspree endpoint, marked with comment "SWAP TO ARIEL WEBHOOK AT GO 2026-07". Repoint to the Ariel webhook at go.

## medspa gate note

"Document store" remains in the homepage stack section (a database term, benign false positive on the "store" forbidden term, not the retail concept). No action needed.

## Homepage editorial redesign (autonomous session, 2026-07-04)

Design-only redesign of index.html (no copy rewrite). Direction: magazine editorial meets high-tech, on the existing brand system (paper/cream/ink/steel, accent #b8470b, accent2 #2a4a6b, hairline rules, sharp corners, Helvetica + mono labels).

### What changed
- **Hero rebuilt as a full-bleed dark spread.** The prior two-column paper hero (image left, copy right) was replaced by a single dark ink-to-steel field with layered CSS/SVG only: a gradient mesh (ink base, steel wash, two restrained accent glows), a fine instrument grid faded with a radial mask, a large two-rings motif rendered as an inline SVG cropped off the top-right (thin low-opacity strokes plus one thin accent arc as the single warm signal), and a very subtle inline-SVG film-grain overlay. Over it: oversized editorial type in cream/paper, eyebrow ruled kicker, H1 at display scale (clamp up to 7.5rem) with the em word "Platforms." in --accent italic, narrower subhead, and the three hero actions as one row of editorial buttons (solid cream primary + two cream-hairline ghosts). No new image files were added; the rings are inline SVG.
- **Magazine system applied to the body.** Oversized mono section folios 01-07 (ariel, what, casestudy, stack, ip, about, contact) set as faint page numerals; ruled kickers retained; alternating paper/cream/ink/steel section fields kept so the page reads as spreads.
- **#what** converted from bordered boxes to an editorial index grid: two columns, accent numerals, hairline row separators, and a vertical column rule. No box hover fills.
- **#casestudy** lead sentence elevated as an oversized italic pull quote with an accent left-rule (same words).
- **#about** metrics restyled as instrument readouts: accent LED indicator per cell, tabular numerals, mono letter-spaced gauge labels.
- **#contact** form restyled to editorial underline fields (transparent inputs, hairline bottom borders, accent focus). Formspree action (mvgkrwre), field names (name/email/message), form id, and the fetch submit JS are unchanged; only presentation and the button hover (moved from inline JS to CSS) changed.
- **Modern-tech texture**: scroll-reveal via IntersectionObserver (progressive enhancement gated on an html.js-anim class set before first paint, so no-JS and reduced-motion users see content immediately); refined nav-scrolled state on scroll (structure unchanged); hero entrance keyframes retained under prefers-reduced-motion: no-preference.

### Hero image relocation rationale
The Meet Ariel image (meet-ariel-hero.png/webp) was moved out of the hero and into the #ariel section as the editorial feature image, because the new hero is a type-and-graphic dark spread (no photographic content) and the image is a purpose-built "Meet Ariel" promo that belongs with the Ariel narrative. The hero-image-wrap and the click-to-talk hotspot link were kept intact around the img (the hotspot percentages are tied to the image geometry, so the wrap was preserved verbatim; only its nesting depth changed). A mono figure caption ("Fig. 01 ...") was added under the image; its wording is drawn from the existing approved #ariel body copy ("every call within two rings, 24 hours a day") to avoid introducing any new claim. loading="eager" and the explicit 1200x1200 dimensions were preserved.

### Hard fences (verified byte-identical against HEAD)
- Demo-number CTA comment block (PENDING LIVE-FIRE GO 2026-07): present, byte-identical.
- talkToAriel() script (PENDING GO-SPIRE + WEB-CALL DECISION 2026-07): present, byte-identical.
- All head content (analytics, verification metas, SEO, OG/Twitter, all four JSON-LD blocks, canonical): lines 1-104 diff clean; the four ld+json blocks compare equal.
- Contact form: Formspree action, method, id, and all three field names preserved; submit JS behavior unchanged.
- Nav and footer link structure and hrefs unchanged (restyle only).

### QA gates (all pass)
- Playwright (bundled Chromium 1228, driven via the ATG-site diag playwright package) at 1280 / 768 / 375: zero horizontal overflow at all three widths; hero H1 and subhead never overflow the viewport and do not collide illegibly with the background layers (legibility held via layer opacity, not text shadows); text-first stacking on mobile (hero copy above the fold, #ariel single-column with heading then figure then body); the relocated hotspot is the top element at its center point at all three widths (hit-tested with elementFromPoint).
- Dash sweep: zero em-dashes (U+2014) and zero en-dashes (U+2013), including CSS comments. Remaining non-ASCII are pre-existing and benign: one right-arrow in the announcement bar, middots, and copyright signs.
- Forbidden-term gate: clean. Only "store" appears, in the pre-existing "Document store" database phrase (kept per body-copy fence). No marketplace/med spa/medspa/DoorDash/DivaDash/retired-people/DD-Bank matches.
- Lighthouse-sane: image loading attributes and explicit dimensions preserved; the hero is CSS/SVG only (no new image, no layout-shift regression).
