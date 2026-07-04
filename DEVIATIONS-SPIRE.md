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

## Hero background swapped to Albert's chosen image (autonomous session, 2026-07-04)

Albert supplied a real hero background image and directed that the prior CSS/SVG dark hero be retired. This run started from HEAD (cd963c6, the "aurora color field" hero, which was still the committed index.html) and rebuilt the hero from that known baseline. The dark ink-to-steel field, the CSS aurora glow layers, the two-rings inline SVG motif, the instrument grid, and the film-grain noise overlay were all removed from the hero. The hero is now a full-bleed light image. (Note: an earlier draft of this section documented a prior attempt with different byte sizes and slightly different scrim/button choices; the numbers and treatment below reflect the artifacts and code actually shipped in this commit.)

### Image pipeline
- Source: spire_background.png (2752x1536, 5,910,340 bytes) from Albert's Downloads. Copied into the repo (never hotlinked).
- Resized to 1920x1072 (LANCZOS). Primary deliverable: spire-hero-bg.webp, quality 86, method 6, 109,024 bytes (well under the ~400KB target). PNG fallback: spire-hero-bg.png, 256-color quantized (adaptive octree, Floyd-Steinberg dither), 419,941 bytes; the fallback is only served to the rare browser without WebP support, so the quantization is acceptable. A straight 1920-wide truecolor PNG was 2,373,113 bytes, hence the quantized fallback.
- Delivery: a picture element inside .hero-bg (source webp + img png fallback), mirroring the existing meet-ariel picture pattern. The img (.hero-bg-img) uses object-fit: cover with object-position for focal control, plus loading=eager and fetchpriority=high since it is above the fold.

### Hero treatment
- Background: full-bleed cover image. object-position 50% 50% at desktop; biased left (32% at <=1024, 24% at <=640) so the calm left zone stays under the text block and the text stays legible as the viewport narrows (the ring motif on the right may crop out on mobile, as expected).
- Text bleeds directly on the image, no panel behind it: H1 in ink (--ink), the em accent word "Platforms." in --accent (#b8470b, was #ec8134), the H1 sub-line in --steel, the subhead in ink at 0.78 (rgba(10,10,15,0.78)), eyebrow in --accent per its existing style.
- Legibility aid: a single soft LIGHT scrim on .hero-inner::before, a paper-white radial gradient (rgba(245,243,238) 0.6 -> 0.38 -> 0, feathered, anchored at 18% from the left) behind the text block only; it fades to nothing on the right so the ring color stays visible. Never a dark panel; text opacity untouched. Contrast measured well above AAA even at the scrim edges, so this is insurance, not a fix.
- Buttons restyled for the light field: primary solid ink (--ink) with paper text; the two ghost buttons are transparent with an ink border and ink text (hover inverts to ink fill + paper text). All three sit on one row at 1280+, wrapping allowed at mobile (MEET ARIEL wraps to a second row at 375).
- Nav: untouched. It already carries its own translucent paper background (rgba(245,243,238,0.92)) with ink logo/links, so it stays legible over the light image without change.

### Hard fences (verified byte-identical against HEAD, git diff + Python region compare)
- Demo-number CTA comment block (PENDING LIVE-FIRE GO 2026-07): not in diff; block confirmed byte-identical to HEAD; present in file.
- talkToAriel() script (PENDING GO-SPIRE + WEB-CALL DECISION 2026-07): not in diff; byte-identical to HEAD; marker present.
- All head content (analytics, verification metas, SEO/OG/Twitter, all four JSON-LD blocks, canonical): not in diff; sampled regions byte-identical.
- Formspree form (action mvgkrwre, method, field names name/email/message, submit JS): not in diff; byte-identical.
- All body copy, all non-hero sections, nav and footer structure: unchanged. The full diff is hero-only (CSS hero block + the hero markup picture swap + two one-line responsive object-position rules).

### QA gates (all pass; headless Chrome rendering the local file)
- Screenshots at 1440 / 1280 / 768 / 375: the image renders as a light colorful background at every width; H1, subhead, and all three buttons legible; ring motif clearly visible on the right at 1440 and 1280; on 768/375 the left/center light zone sits under the text with the ring partially cropping, as intended.
- Pixel sampling of the rendered 1440 hero, 8 spread points: 8/8 LIGHT (luminance L=211-242 on 0-255), 4/8 in the visible warm-orange range (center tunnel and the three right-ring points). No dark/black/navy region anywhere. Automatic-FAIL condition (predominantly dark hero) not triggered.
- Contrast against actual sampled background regions in the text zone (darkest bg sampled L=239): H1 ink 17.3:1, subhead sub-line steel 13.7:1, hero-sub dark-grey 10.0:1. All far above WCAG AAA (7:1). No scrim or position change needed.
- Overflow: not captured as a numeric scrollWidth this run (Chrome --dump-dom returned no output through the shell in this environment). Guaranteed instead by construction: body carries overflow-x: hidden (line 130, unchanged), which makes horizontal overflow non-scrollable so body width equals viewport width; the 375 render confirms the image fills 0-375px edge to edge with no gutter and no dark box.
- Dash sweep: zero em-dashes (U+2014) and zero en-dashes (U+2013) in index.html and this log (verified in Python; the shell grep -P is locale-broken here).
