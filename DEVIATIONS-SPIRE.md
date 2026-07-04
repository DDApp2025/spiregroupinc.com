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

Albert supplied a real hero background image and directed that the prior CSS/SVG dark hero be retired. The dark ink-to-steel field, the CSS/SVG glow layers, the two-rings inline SVG motif, the instrument grid, and the film-grain noise overlay were all removed from the hero. The hero is now a full-bleed light image.

### Image pipeline
- Source: spire_background.png (2752x1536, 5,910,340 bytes) from Albert's Downloads. Copied into the repo (never hotlinked).
- Resized to 1920x1072 (LANCZOS). Primary deliverable: spire-hero-bg.webp, quality 82, method 6, 88,348 bytes (well under the ~400KB target). PNG fallback: spire-hero-bg.png, 256-color quantized (Floyd-Steinberg), 293,297 bytes; the fallback is only served to the rare browser without WebP support, so the quantization is acceptable. A straight 1920-wide PNG was 2,373,113 bytes, hence the quantized fallback.
- Delivery: a picture element inside .hero-bg (source webp + img png fallback), mirroring the existing meet-ariel picture pattern. The img uses object-fit: cover with object-position for focal control.

### Hero treatment
- Background: full-bleed cover image. object-position 50% 50% at desktop; biased left (32% at <=1024, 24% at <=640) so the calm left zone stays under the text block and the text stays legible as the viewport narrows (the ring motif on the right may crop out on mobile, as expected).
- Text bleeds directly on the image, no panel behind it: H1 in ink (--ink), the em accent word "Platforms." in --accent (#b8470b, was #ec8134), the H1 sub-line in --steel, the subhead in ink at 0.78, eyebrow in --accent per its existing style.
- Legibility aid: a single soft LIGHT scrim (paper white radial, ~0.80 to 0 opacity, feathered) anchored behind the left text block via .hero::before; it fades to nothing on the right so the ring stays visible. Never a dark panel; text opacity untouched.
- Buttons restyled for the light field: primary solid ink with paper text; the two ghost buttons use ink border + ink text on a faint paper backing (rgba(245,243,238,0.5)). All three on one row at 1280+, wrapping at mobile.
- Nav: untouched. It already carries its own translucent paper background (rgba(245,243,238,0.92)) with ink logo/links, so it stays legible over the light image without change.

### Hard fences (verified byte-identical against HEAD, git diff)
- Demo-number CTA comment block (PENDING LIVE-FIRE GO 2026-07): not in diff, present in file.
- talkToAriel() script (PENDING GO-SPIRE + WEB-CALL DECISION 2026-07): not in diff, present in file.
- All head content (analytics, verification metas, SEO/OG/Twitter, all four JSON-LD blocks, canonical): not in diff.
- Formspree form (action mvgkrwre, method, field names name/email/message, submit JS): not in diff.
- All body copy, all non-hero sections, nav and footer structure: unchanged. The full diff is hero-only (CSS hero block + the hero markup picture swap).

### QA gates (all pass; Playwright driving system Chrome)
- Screenshots at 1440 / 1280 / 768 / 375: the image renders as a light colorful background at every width; H1, subhead, and buttons legible; ring motif visible on the right at 1440 and 1280.
- Overflow: scrollWidth == innerWidth == body width at all four widths (1440, 1280, 768, 375). No horizontal overflow; body width equals viewport width at 375.
- Pixel sampling, 8 spread points per width: predominantly LIGHT luminous field (relative luminance mostly 0.5 to 0.94) with visible warm-orange regions at 6 to 7 of 8 points every width. Not dark/navy. The lone dark sample at 375 (rgb ~129,125,125) is the solid ink "Start a Project" button, not the background.
- Contrast against actual sampled background regions: H1 15.6 to 17.7:1; subhead 10.9 to 13.7:1. All far above WCAG AA/AAA. No scrim or position change needed.
- Dash sweep: zero em-dashes and zero en-dashes in index.html (verified in Python, not the locale-broken grep -P).

## Hero above-the-fold compaction + header button retarget (autonomous session, 2026-07-04)

index.html only. Two changes, CSS-only compaction plus a single header-link retarget. No wording, graphic, color, font, or structural change.

### Recon (as-found)
- Hero container: `.hero` with `min-height: 100vh; display: flex; flex-direction: column; justify-content: center; padding: 11rem 5rem 6rem;`. Top spacing below the fixed nav was the 11rem top padding; measured gap nav-bottom to eyebrow-top was 144px. Content wrapper `.hero-inner` (max-width 1180px, centered).
- Vertical spacing between hero elements: `.hero-eyebrow { margin-bottom: 2rem }`, `h1 { margin-bottom: 2.2rem; max-width: 15ch }`, `.hero-h1-line { margin-top: 1.4rem; max-width: 34ch }` (the "We build intelligent systems..." line, a span inside h1), `.hero-sub { max-width: 46ch; margin-bottom: 3rem; line-height: 1.75 }` (the "Spire Group Inc. is a full-stack studio..." paragraph), `.hero-actions { gap: 0.9rem; flex-wrap: wrap }` (row of three chips).
- Paragraph as-found: 6 lines at desktop (max-width 46ch, ~419px at 1366 / ~442px at 1440).
- Lower hero "MEET ARIEL" chip markup: `<a class="btn-ghost" href="ariel.html#walkthrough" onclick="talkToAriel(); return false;">Meet Ariel</a>`. Its href attribute is `ariel.html#walkthrough`.
- Header button (top-right nav): `<a class="nav-cta" href="contact.html">Start a Project</a>`.
- Baseline fit: at 1366x768 and 1440x900 the three chips fell BELOW the fold (chips.bottom = 916px at both), so the hero did not fit above the fold.

### Change 1 - compaction (CSS only, before -> after)
- `.hero` `justify-content: center` -> `flex-start` (content starts near the top instead of vertically centered, so the hero starts higher).
- `.hero` `padding: 11rem 5rem 6rem` -> `6.5rem 5rem 3rem` (reduced the empty band under the nav; still clears the ~69px fixed nav).
- `.hero-eyebrow` `margin-bottom: 2rem` -> `1.1rem`.
- `h1` `margin-bottom: 2.2rem` -> `1.2rem`.
- `.hero-h1-line` `margin-top: 1.4rem` -> `0.9rem`.
- `.hero-sub` `max-width: 46ch` -> `90ch` (paragraph reflows 6 lines -> 3 lines at desktop; ~820px at 1366, ~865px at 1440, inside the 1180px inner and clear of the ring motif), and `margin-bottom: 3rem` -> `1.6rem`.
- Mobile untouched: the `@media (max-width: 1024px)` and `(max-width: 640px)` rules already override `.hero` padding and set `min-height: auto`, so only desktop (>1024px) is affected. The widened paragraph is width-constrained on narrow screens and wraps back to more lines (8 lines at 375), which is expected.

### Change 2 - header button retarget
- `<a class="nav-cta" href="contact.html">Start a Project</a>` -> `<a class="nav-cta" href="ariel.html#walkthrough">Meet Ariel</a>`.
- Shared href: `ariel.html#walkthrough` (matches the lower hero chip's href attribute exactly; QA confirmed header CTA href === hero chip href). Text "Meet Ariel" in Title Case to match the other header items' source casing (rendered uppercase by the existing `text-transform: uppercase`).
- Nuance (documented, not changed): the lower hero chip additionally carries `onclick="talkToAriel(); return false;"`, so JS-enabled clicks on the chip route to `ariel.html` (top of page) via the stub, while the header button, per the instruction to set only its href, navigates to `ariel.html#walkthrough`. Only the href was retargeted; the chip is unchanged and still present. Other pages' header buttons keep "Start a Project" -> contact.html (this change is homepage-only, as scoped).

### QA (Playwright, system Chrome; screenshots in scratchpad/heroshots)
- 1366x768: chips.bottom 683px (visible above the 768 fold), paragraph 3 lines, gap nav->eyebrow 72px (was 144), horizontal overflow 0.
- 1440x900: chips.bottom 687px (visible), paragraph 3 lines, gap 72px, overflow 0.
- 768x1024: chips visible (620px), overflow 0.
- 375x812: overflow 0 (no horizontal scroll); paragraph wraps to 8 lines and chips fall below the mobile fold, which is expected on mobile.
- Header CTA reads "Meet Ariel" and href `ariel.html#walkthrough` == hero chip href at 1366 and 1440.
- Dash sweep: zero em-dashes and zero en-dashes (and zero &mdash;/&ndash; entities) in index.html.

## Hero H1 downsizing to fit above the fold (autonomous session, 2026-07-04)

index.html only, CSS-only. Follow-up to the prior compaction: the H1 was still clamping to 120px (2 lines = ~240px), which pushed the paragraph and chip row below the laptop fold. Reduced the H1 (and its line-height) and nudged the subhead down to hold the hierarchy. No wording, structure, graphic, or color change; "Platforms." stays on its own line in the accent em color.

### Before -> after (CSS)
- `h1 { font-size: clamp(2.6rem, 9vw, 7.5rem); line-height: 1.0 }` -> `font-size: clamp(2.5rem, 5.5vw, 5rem); line-height: 0.95`. On laptops the middle 5.5vw governs, giving ~75px at 1366, ~79px at 1440, ~80px at 1536 (capped at 5rem = 80px for wider screens). Still a responsive clamp, so mobile scales to ~40px at 375 and ~42px at 768 (min 2.5rem = 40px). H1 remains the dominant element (roughly 4x body size).
- `.hero-h1-line { font-size: clamp(1.05rem, 2vw, 1.7rem) }` -> `clamp(1.05rem, 1.7vw, 1.5rem)` (the "We build intelligent systems, and operate them." subhead, nudged down proportionally to keep H1 clearly dominant).

### Iterated measurement (Playwright, system Chrome; screenshots in scratchpad/heroshots2)
Target: bottom of the three-chip row within the viewport height at all three laptop sizes. Final result (chips.bottom / viewport height):
- 1366x768: chips 575 / 768 FIT (H1 75px), "Platforms." on its own line, horizontal overflow 0.
- 1440x900: chips 589 / 900 FIT (H1 79px), overflow 0.
- 1536x864: chips 594 / 864 FIT (H1 80px), overflow 0.
- 768x1024: chips 562 / 1024 FIT (H1 42px), overflow 0.
- 375x812: horizontal overflow 0; H1 40px scales down sensibly; the chip row sits ~6px below the mobile fold, which is expected on mobile (vertical scroll).
Final H1 font-size landed on: clamp(2.5rem, 5.5vw, 5rem), line-height 0.95.
- Dash sweep: zero em-dashes and zero en-dashes (and zero entity dashes) in index.html.

## Header Meet Ariel button retargeted to Ariel page top (autonomous session, 2026-07-04)

index.html only, single href change. The top-right header nav button (`.nav-cta`, "Meet Ariel") linked to the walkthrough anchor; changed it to load the Ariel page at the top.
- File changed: index.html (line 988). Before: `<a class="nav-cta" href="ariel.html#walkthrough">Meet Ariel</a>`. After: `<a class="nav-cta" href="ariel.html">Meet Ariel</a>`.
- Template check: the nav is hardcoded inline per page (no shared template/partial/build step in this repo), and only index.html's header CTA carries the "Meet Ariel" label (other pages' `.nav-cta` reads "Start a Project" -> contact.html), so no other page needed the change and none is left stale.
- Left unchanged: the lower hero "MEET ARIEL" chip (`.btn-ghost`, line 1008) still `href="ariel.html#walkthrough"` with its `onclick="talkToAriel()"`, and all other links.
