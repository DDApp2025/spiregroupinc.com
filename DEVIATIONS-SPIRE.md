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

## Hero chip primary/secondary style swap (autonomous session, 2026-07-04)

index.html only, visual swap of two hero CTA chips via their shared classes. No text, href, size, position, or order change.
- Mechanism: shared classes scoped to `.hero-actions` - `.btn-primary` (filled: `background: var(--ink)`, `color: var(--paper)`) and `.btn-ghost` (outlined: transparent background, `color: var(--ink)`, border). Swapped which class each chip carries; no CSS rules changed.
- "Meet Ariel": `btn-ghost` -> `btn-primary` (now filled black, white text). href `ariel.html#walkthrough` and its `onclick="talkToAriel()"` unchanged.
- "Start a Project": `btn-primary` -> `btn-ghost` (now outlined, dark text). href `contact.html` unchanged.
- "See the Platform" (`btn-ghost`, `case-study.html`): unchanged. Order unchanged.

## Hero paragraph rewrite (autonomous session, 2026-07-04)

index.html only, hero-sub paragraph text replaced. No H1, subhead, chip, graphic, or CSS change.
- Before: "Spire Group Inc. is a full-stack studio that designs, builds, and operates SaaS platforms and AI systems across industries. Our flagship deployment, Aesthetics To Go, is a fully autonomous operating system that proves what we build in production. We don't just ship software. We build intelligent platforms and run them." (~62 words)
- After: "Spire Group designs, builds, and operates SaaS and AI platforms across industries. Ariel, our AI receptionist, answers your calls and books appointments around the clock. We proved it running our own platform, Aesthetics To Go. We build intelligent systems and run them." (42 words)
- Content checks: zero em/en dashes (and zero entity dashes); "first product" absent; "Aesthetics To Go" spelled exactly; no forbidden terms.
- Fit re-verified (Playwright, reduced-motion; screenshots in scratchpad/heroshots3): chips above the fold at 1366x768 (575), 1440x900 (589), 1536x864 (594); paragraph renders 3 lines at desktop; horizontal overflow 0 at 375 and 768. Layout unchanged from the prior pass (still 3 lines at the 90ch measure), so no other adjustment needed.

## Call-button GO-LIVE: demo-number CTA activated, talkToAriel dials the live line (2026-07-04)

Albert's decision (GO): the site's "talk to Ariel" experience is a phone call to the live
demo line, shipped now. The in-browser web-call version is deferred to post-GO and was NOT
built. index.html only. Live demo line: +1 (702) 707-4919 (tel:+17027074919).

### Marker dispositions (both homepage GO markers resolved)
- **PENDING LIVE-FIRE GO 2026-07** (secondary demo-number CTA, #ariel section): RESOLVED.
  Uncommented and set to the live number. It is now the sole call button: visible text
  "Call Ariel Now: (702) 707-4919", aria-label "Call Ariel, the AI receptionist, at
  (702) 707-4919", href="tel:+17027074919" with onclick="talkToAriel(); return false;" as the
  JS dial path (the tel: href is the no-JS fallback, same number). The placeholder
  tel:+10000000000 is gone. Marker removed.
- **PENDING GO-SPIRE + WEB-CALL DECISION 2026-07** (talkToAriel() in the page script):
  RESOLVED. The web-call decision is made: phone call now, in-browser web-call deferred (not
  built). talkToAriel() now runs window.location.href='tel:+17027074919'. The PENDING marker
  and the web-call stub comment are removed and replaced with a note recording the decision.

No PENDING GO marker remains in index.html.

### Label rule (every dialer says so) and the two decouplings it forced
talkToAriel() was previously the shared click handler for the hero "Meet Ariel" chip and the
SCHEDULE A DEMO hotspot. Making it dial would have turned both into dialers. To satisfy the
label rule ("no button that dials may carry a label implying anything else") and to keep the
hotspot scheduling, both were decoupled from talkToAriel():
- **Hero "Meet Ariel" chip** (hero-actions): per Albert's explicit choice, kept as "Meet
  Ariel" navigation, not a call button. Its onclick="talkToAriel()" was removed and its href
  set to ariel.html (matching the header "Meet Ariel" button's destination). It navigates,
  never dials; label unchanged.
- **SCHEDULE A DEMO hotspot** (over the baked-in image region): its onclick="talkToAriel()"
  was removed; it now routes purely via href="ariel.html#walkthrough" (schedule), never dials,
  per the instruction that the region says SCHEDULE so it must schedule. Its aria-label was
  corrected from "Talk to Ariel" to "Schedule a demo walkthrough" so the accessible label
  matches the behavior.

After the change there is exactly ONE tel: link on the page (the Call Ariel Now button), and
it carries the required visible text and aria-label. Verified by DOM sweep at all three widths.

### Untouched (still gated)
- The Ariel-page walkthrough form's Formspree action (SWAP TO ARIEL WEBHOOK AT GO 2026-07) was
  NOT touched; that swap stays gated on GO-SPIRE. This change is index.html-only; ariel.html
  was not edited.
- Head content, JSON-LD, nav/footer, the homepage #contact Formspree form, and all other body
  copy: unchanged (the diff is the four edits only).

### QA (Playwright, bundled Chromium 1228, local http server; screenshots in scratchpad)
- Horizontal overflow: 0 at 1280, 768, and 375 (scrollWidth == innerWidth == body width each).
- Call Ariel Now button: rendered and visible, fully within the viewport (right edge
  1153/569/328 px at 1280/768/375), text/href/aria-label exactly as specified.
- Hero "Meet Ariel": href ariel.html, no onclick, not a tel. Hotspot: href
  ariel.html#walkthrough, aria "Schedule a demo walkthrough", not a tel. Exactly one tel: link.
- Dash sweep: zero em-dashes (U+2014), zero en-dashes (U+2013), zero &mdash;/&ndash; entities
  in index.html (Python scan; grep -P is locale-broken here).
- Forbidden-term gate: clean. Only "store" appears, in the pre-existing "Document store"
  database phrase (kept per the body-copy fence). No marketplace/med spa/medspa/DoorDash/
  DivaDash/DD-Bank/retired-people matches; no new forbidden term introduced.

### Related (other repo)
- The spire-sales Retell voice speed was lowered 0.95 -> 0.90 the same day; that change and its
  verification are logged in atg-agent DEVIATIONS.md D28 (Retell config only, no service
  deploy). Not part of this repo's diff.

## Call Ariel Now chip: desktop-readable phone number (autonomous session, 2026-07-04)

index.html only, additive. The "Call Ariel Now: (702) 707-4919" chip is a tel: link (`href="tel:+17027074919"`, plus `onclick="talkToAriel()"` which sets `window.location='tel:+17027074919'`). On mobile it dials; on desktop the tel: handler shows a "pick an app" dialog and the number looks broken. The chip is kept live and unchanged.
- Fix (additive, chip untouched): added a small selectable readout line directly under the actions row so desktop visitors can read/copy the number and dial manually.
  - Markup added after `.ariel-actions`: `<p class="ariel-call-note">On a computer? Dial <span class="ariel-phone">(702) 707-4919</span> from your phone.</p>`
  - CSS added: `.ariel-call-note` (mono, small, muted) and `.ariel-phone` (accent, bold, `white-space: nowrap`, `user-select: all` / `-webkit-user-select: all` so a single click selects the whole number to copy). The number is real text in a `<span>` (not an image, not a link), so it never triggers the desktop tel: dialog.
- Before: chip only; the number lived only inside the tel: anchor, awkward to select on desktop (clicking fires the OS dialog).
- After: chip unchanged (still tel:+17027074919 for mobile) + a plain, selectable (702) 707-4919 readout beneath it with "On a computer? ... from your phone." microcopy.
- Verify: chip href still `tel:+17027074919`; `.ariel-phone` is a `<span>` with computed `user-select: all` and selectable text captured as "(702) 707-4919"; zero horizontal overflow at 375, 768, 1366. Number, chip position, and live status unchanged. Zero em/en dashes.

## Contact section rebuilt from scratch: unified three-channel section (autonomous session, 2026-07-06)

This SUPERSEDES AND CANCELS all prior CTA redesign/amendment work, including the "Call Ariel
Now chip: desktop-readable phone number" entry above and the earlier call-button GO-LIVE CTA
cluster. The old cluster is deleted; a single unified contact section replaces it.

### index.html
- DELETED the CTA cluster in the #ariel section entirely: the "Book a walkthrough" button, the
  "Call Ariel Now: (702) 707-4919" tel: chip, and the "On a computer? Dial ... from your phone."
  helper line. Their CSS (.ariel-actions, .ariel-call-note, .ariel-phone) was removed.
- BUILT one unified contact section in their place (still inside .ariel-body, above the feature
  pills), on the existing brand system (paper field, hairline borders, sharp corners, mono
  labels, accent kickers):
  - HEADER: "Reach us any way you like. Ariel answers - that's the demo."
  - SUBLINE: "Ariel is our AI receptionist, and she's the product. Whichever way you contact us,
    she'll answer, take care of you, and set up a call with our team."
  - THREE EQUAL CHANNEL CARDS (grid, same visual weight):
    1. CALL - mobile (pointer: coarse) shows a button "Call (702) 707-4919" firing
       tel:+17027074919; desktop (pointer: fine) shows the number large with a Copy button and an
       inline "Copied" confirmation, NO tel: link. Both variants via CSS pointer-media queries
       (default = desktop readout; @media (pointer: coarse) swaps to the tel button). Caption
       "Ariel answers now, 24/7." aria-labels: call button "Call Ariel, our AI receptionist, at
       (702) 707-4919"; copy button "Copy the phone number (702) 707-4919".
    2. SEND A MESSAGE - scrolls to the existing homepage message form (href="#contact"); the form
       (id homepageForm, action https://formspree.io/f/mvgkrwre, fields name/email/message) was
       NOT touched. Caption "Ariel replies by email within minutes, day or night."
    3. EMAIL - mailto:info@spiregroupinc.com, address shown in full. Caption "Reaches our team
       directly." (deliberate: the inbox is human-answered; Ariel is not claimed here).
- JS: the now-dead talkToAriel() (the old chip's dialer) was removed; a copyPhone() clipboard
  helper (with an execCommand fallback and the "Copied" toggle) added. JS is used only for the
  desktop clipboard copy; the mobile call is a native tel: link.
- Hotspot aria-label over the baked-in image region aligned "Schedule a demo walkthrough" ->
  "Schedule a demo" (drops the visitor-facing "walkthrough"); its href ariel.html#walkthrough
  (an internal anchor to the ariel.html form section) is unchanged.

### ariel.html consistency sweep (each change reported)
- nav-cta (was `Book a Walkthrough` -> `#walkthrough`) -> label "Get in Touch" (href unchanged).
- hero button (was `Book a walkthrough`) -> "Get in Touch" (href #walkthrough unchanged).
- form section eyebrow "Book a walkthrough" -> "Send us a message" (the form-heading reframe).
- form submit button "Book my walkthrough" -> "Send message".
- form JS: success message dropped "to schedule your walkthrough" (now "Request received. We will
  be in touch within one business day."); the two failure-path button resets "Book my
  walkthrough" -> "Send message".
- LEFT UNCHANGED (per fences): the form action (formspree mvgkrwre) and all field names, the
  #walkthrough section id and its CSS selectors (internal anchor), the "SWAP TO ARIEL WEBHOOK AT
  GO" comment, and body/SEO prose (h1 "someone else booked", section-lead "walk you through",
  the "how it works"/pilot prose, FAQ "Book a walkthrough to begin", and all meta/OG/JSON-LD
  describing the product). Body prose is allowed to keep its wording per the brief.

### Rules honored
- No "walkthrough" or "book" in any visitor-facing LABEL on either page (buttons, headings, aria,
  JS button text). aria-labels state mechanism + number/address. CSS pointer-media for the call
  variants; JS only for clipboard. Internal/system uses (form action, field names, section id,
  comments, SEO/meta, product prose) untouched. There is no _subject on either form (nothing to
  preserve there); form actions are unchanged.

### QA (rendered, Playwright 1.61.1 driving cached Chromium 1228, local http server)
- DESKTOP 1280 (fine pointer): shows ONLY the desktop call variant (number + Copy), zero visible
  tel: links in the card; Copy writes "(702) 707-4919" to the clipboard and shows the inline
  "Copied" confirmation; three equal cards side-by-side (180px each); Send -> #contact; Email ->
  mailto; homepage form action still mvgkrwre; horizontal overflow 0.
- DESKTOP 768 (mouse): desktop call variant only; cards stack (below the 820px breakpoint);
  overflow 0.
- MOBILE 375 (coarse pointer, touch): shows ONLY the mobile call variant; the button href is
  tel:+17027074919 (fires the dialer on a real device); cards stacked; overflow 0.
- Dash sweep: zero em-dashes and zero en-dashes in index.html and ariel.html (Python scan).
- Forbidden-term gate: no marketplace/med spa/medspa/DoorDash/DivaDash/DD-Bank/retired-people
  matches introduced. (The pre-existing "Document store" database phrase is out of this diff.)

### Deploy
- Commit by explicit path (index.html, ariel.html, DEVIATIONS-SPIRE.md); push to main; GitHub
  Pages serves it. Verified live after push.

## GO-SPIRE Phase 5 completion: forms swapped from Formspree to the Ariel intake webhook (autonomous session, 2026-07-06)

The deferred GO-SPIRE Phase 5 form swap. Both site forms now post to Ariel's intake webhook, so
the contact section's promise ("Ariel replies by email within minutes") is true.

### Both forms repointed
- Homepage #contact form and the ariel.html walkthrough form: action changed from
  https://formspree.io/f/mvgkrwre to the token-gated intake URL
  https://agent.aestheticstogo.com/intake/spire-sales/<secret> (secret injected from the
  atg-agent .env, never printed; it is a semi-public endpoint token, like a Formspree form id).
- The "SWAP TO ARIEL WEBHOOK AT GO" marker comment on the ariel form was removed.
- Submit JS: both forms switched from FormData (multipart) to a JSON body, because the intake
  endpoint parses application/json only (express.json). Fields sent: { name, email, message }.
- Field-fold (ariel form): the endpoint has no structured fields for business_name, business_type
  (+ business_type_other), or phone, so the submit JS folds them losslessly into the message body
  ("Business: X · Type: Y · Phone: Z", preceded by "How they handle calls today: ..."). The
  homepage form maps 1:1 (name/email/message). Form field names and ids are otherwise unchanged.

### Enabling atg-agent change (cross-repo; logged in atg-agent commit 0dd01d0, task-def 27)
The intake endpoint had no CORS, so a browser cross-origin POST from the site would have been
blocked by the preflight. Added to the /intake router:
- CORS ALLOWLIST (exactly, no wildcard): https://spiregroupinc.com and https://www.spiregroupinc.com;
  methods POST + OPTIONS; allow-header Content-Type; 204 preflight answer. Verified live: allowed
  origins get Access-Control-Allow-Origin; other origins get none.
- NEW-INQUIRY founder notification: a first-touch web-form lead via /intake sends a "New website
  inquiry" email (name, email, message excerpt, lead + conversation id) to the tenant notification
  address (info@spiregroupinc.com), so swapping off Formspree (which notified on every submission)
  does not reduce Albert's visibility. First turn only; scoped to the intake path (the formspree
  path keeps Formspree's own notification). Booking/escalation alerts unchanged.

### Live tests (from info@spiregroupinc.com, per the in-domain test-sender rule)
- ARIEL FORM (booking request, folded fields): POST returned {ok, replied:true, form_submission}.
  The caller message carried Business/Type/Phone (verified), and Ariel's reply read them back
  ("book straight onto your calendar, so those after-hours calls at Bright Smiles Dental ...") and
  offered walkthrough times. The new-inquiry founder notification fired (first touch). A threaded
  follow-up ("the first time works") booked a real appointment (event:booked, appointment created);
  the booking-completion alert is DEFERRED by the existing spire enrich-then-alert design until the
  business type is captured, so immediate founder visibility comes from the new-inquiry notification.
- HOMEPAGE FORM (generic inquiry): POST returned {ok, replied:true, form_submission}. Ariel answered
  the question ("Spire Group builds and runs me, Ariel ...") and the new-inquiry notification fired.
- No Brevo send failures in CloudWatch during the tests (AI replies and notifications dispatched).
  Test leads and the test appointment were deleted afterward (leads 0, appointments 0). Actual
  inbox arrival at info@spiregroupinc.com is Albert's to eyeball; dispatch and backend records were
  verified here.

### QA
- Dash sweep: zero em/en dashes in index.html and ariel.html (the folded separator is a middot
  U+00B7, not a dash). Forbidden-term gate clean. Both forms verified to use JSON (no FormData) and
  point to the intake URL; no formspree.io left in either form.

## Hero layout: text right, Ariel messaging panel left (autonomous session, 2026-07-06)

Albert's standing spec, finally implemented. index.html hero only.
- Kept the full-bleed background image and the magazine type scale. Restructured .hero-inner into
  a two-column CSS grid: the text block (.hero-copy: eyebrow, H1 "SaaS & AI Platforms.", subline,
  paragraph, three CTAs - all copy unchanged) is placed in the RIGHT column (grid-column: 2,
  justify-self: end, capped width) with a right-feathered light scrim for legibility; a framed
  Ariel messaging panel (.hero-panel) is placed in the LEFT column (grid-column: 1).
- The H1 stays FIRST in the DOM (accessibility / reading order); the panel is placed left purely
  via CSS grid-column, so source order is text-then-panel and mobile stacks text-first naturally.
- Panel is real HTML text (never baked into an image): mono kicker "Meet Ariel", heading
  "The 24/7 AI receptionist by Spire Group", three proof lines ("Answers instantly, 24/7." /
  "Books while the caller is on the line." / "Hands off to a human when needed."), and
  "Call her now: (702) 707-4919" - a tel: link on coarse pointers, plain text on fine pointers
  (same pointer-media pattern as the contact section). Brand-styled: hairline ink border, sharp
  corners, semi-opaque paper field so it reads over the image.
- HONESTY GATE: the panel carries exactly those lines. No "never misses a call", no "2 rings", no
  unproven claims.
- Mobile (<=1024px, incl. 375): grid collapses to one column, explicit column placement reset,
  so it stacks TEXT first, panel second.

QA (Playwright, cached Chromium, rendered at 1280 / 768 / 375; screenshots in scratchpad/pwqa):
- 1280 (fine pointer): text block center in the right half (623-1200), MEET ARIEL panel in the
  left half (80-572), panel entirely left of the text, fine (plain-text) call variant shown, zero
  horizontal overflow.
- 768 (mouse): stacked, text first, no overflow.
- 375 (touch, coarse pointer): stacked text-first then panel, the coarse tel:+17027074919 call
  variant shown, no overflow.
- Dash sweep clean; forbidden gate clean. The contact section, both forms (Phase 5 intake action),
  and all markers are untouched (git diff is hero-only).

## Hero: two-column layout CANCELLED, reverted to single-column left text minus one sentence and the CTAs (autonomous session, 2026-07-06)

REVERSAL of the entry directly above. Albert reviewed the two-column hero (text right, MEET ARIEL
panel left) live and rejected it. That spec is CANCELLED PERMANENTLY. No future session may
reintroduce the two-column hero, the .hero-panel / .hero-copy / .hero-proof / .hero-call-* CSS, or
the left-side Ariel messaging panel. The hero is the ORIGINAL single-column layout.

What was done, index.html hero only:
- Reverted the hero markup AND its CSS exactly to the pre-two-column parent state (commit 083bfca,
  the parent of the two-column commit 1c5d838). This restored the single .hero-inner block with the
  left-anchored text over the full-bleed background and the original left-feathered .hero-inner::before
  scrim; it removed every two-column artifact (grid .hero-inner, .hero-copy, .hero-panel and its
  kicker/heading/proof/call rules, the pointer-media call variants, and the mobile grid resets).
- Then applied EXACTLY TWO surgical removals to the restored hero:
  1. Removed the final sentence of the hero paragraph: "We build intelligent systems and run them."
     The .hero-sub paragraph now ends at "... running our own platform, Aesthetics To Go."
     The H1 subline "We build intelligent systems, and operate them." (comma + "operate") STAYS.
  2. Removed the three CTA buttons (Start a Project / See the Platform / Meet Ariel) and their now
     empty container, the .hero-actions div. (The orphaned .hero-actions CSS rules were left in
     place, as they are harmless dead style and outside the two authorized removals.)
- Nothing else on the page changed: kicker, H1 "SaaS & AI Platforms.", subline, remaining paragraph
  text, left placement, background, and spacing are the original. The #ariel section, the contact
  section, and both Phase 5 intake forms are untouched (git diff HEAD is hero-region-only).

QA (Playwright, cached Chromium, rendered at 1280 / 768 / 375; screenshots revert-<w>.png in
scratchpad/pwqa): all 27 assertions PASS. At every width the hero text starts in the LEFT half
(h1.left 80 / 40 / 24), .hero-panel and .hero-copy are absent from the DOM, .hero-actions and the
hero .btn-ghost/.btn-primary buttons are absent, the removed sentence is gone, the kept subline is
present, and there is zero horizontal overflow. Whole-file dash sweep: zero em/en dashes. Forbidden
gate clean on changed lines (the one "store" hit is the pre-existing "document store" database term
in the untouched tech-stack section).
