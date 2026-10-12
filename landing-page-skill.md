---
name: creative-gsap-landing-page
description: Builds a complete, art-directed landing page for any business, product, SaaS, portfolio, event or personal brand, using vanilla HTML, Tailwind CSS v4, BEM and GSAP, with a design system derived from the project itself. Use this whenever someone asks for a landing page, marketing page, homepage or hero, or wants a "creative", "animated", "award-style" or "scroll-driven" page, or mentions GSAP, ScrollTrigger or scroll animation, even if they never name this skill.
---

# Creative GSAP Landing Page

Build one complete landing page that feels designed by a person, not stamped out of a template. It gets its own concept, its own design system, and a motion language built with GSAP.

This works with any LLM or coding agent. Paste it into a chat, or save it as `SKILL.md`. Fill in the brief below, or leave it empty and let the agent work the project out from the context it already has.

---

## Project brief

Fill in what you know. Anything left blank, the agent works out from the project.

- **Name:**
- **What it is and what it offers:**
- **Audience:**
- **Main goal of the page** (call, booking, enquiry, sign-up, purchase, waitlist, download):
- **Tone and personality** (calm, bold, playful, premium, technical):
- **Brand assets** (logo, colors, fonts, photos, copy):
- **Sections wanted** (blank = the agent decides from the goal):
- **References to study** (URLs):
- **Designs to avoid repeating** (links, screenshots, descriptions):
- **Language, region, currency:**
- **Anything else:**

### If the brief is empty or only partly filled

1. Look for context in the project first: README, `package.json`, `AGENTS.md` / `CLAUDE.md`, content or data files, existing pages, brand assets, and earlier messages in this conversation.
2. Fill the gaps with sensible assumptions and list them in one short block before building.
3. Ask a question only if the business cannot be identified at all. Otherwise carry on without waiting for approval.
4. Anything you invent is placeholder content. Mark it clearly as sample content.

---

## Stack and non-negotiables

**Technology**

- Vanilla HTML, CSS and JavaScript (ES modules). No framework and no unnecessary dependencies.
- Tailwind CSS v4, configured CSS-first: `@import "tailwindcss";` plus an `@theme` block. No `tailwind.config.js` and no v3 `@tailwind` directives. If the project already has a Tailwind setup, use it. Otherwise add the smallest build step that works (the Tailwind CLI or `@tailwindcss/vite`). The browser CDN build is for prototypes only.
- GSAP loaded from a CDN and pinned to an exact version, never `@latest`. Look up the current stable version, confirm each script URL actually loads before relying on it, and register every plugin you use. All GSAP plugins are free, including the former Club ones (SplitText, MorphSVG, DrawSVG, ScrollSmoother, Inertia and the rest). Check gsap.com if unsure.
- The page must stay readable and usable if GSAP fails to load or JavaScript is off. Hide initial animation states only after GSAP is confirmed (for example by adding a `has-gsap` class to `<html>`), never in plain CSS.

**Structure and naming**

- Semantic HTML: landmarks, one `<h1>`, a logical heading order, real `<button>` and `<a>` elements.
- BEM for every component: `block__element--modifier`, kebab-case. No ids or deep selectors for styling.
- BEM classes are the styling hooks. Write them in `@layer components` using theme tokens (`var(--color-ink)`, and `@apply` where it reads clearly). If styles are split across several CSS files, add `@reference` to each file that uses `@apply`. Tailwind utilities in markup are for tiny one-offs, never long class strings.
- JavaScript finds elements through `data-*` attributes (`data-reveal`, `data-parallax`), not BEM classes, so styling and behavior can change independently.
- Business data (name, phone, email, address, links, form endpoint) lives in one config file or object, never scattered through the markup. Assume the content will become dynamic later.

**Suggested file layout** (adapt to the project's existing structure)

```
index.html
src/css/main.css        tailwind import, @theme tokens, base, components
src/js/config.js        business data
src/js/main.js          boot, feature detection, plugin registration
src/js/motion/*.js      one small module per interaction
src/js/ui/*.js          nav, accordion, form
assets/img/             responsive images and a credits file
DESIGN.md               short record of tokens and the motion language
```

---

## Design system

Derive the design system from this project's brand, audience and concept. Do not reuse one from a previous project.

Define tokens in `@theme`. Use `@theme static` for tokens that JavaScript reads, because Tailwind v4 only outputs variables that are used.

- **Color:** neutrals, surface and ink, one or two accents, and semantic roles (`--color-surface`, `--color-ink`, `--color-ink-muted`, `--color-accent`, `--color-on-accent`). Record which foreground/background pairs pass AA.
- **Typography:** font stack, a fluid size scale with `clamp()`, line heights, tracking.
- **Spacing:** a scale, container widths, section rhythm.
- **Shape and depth:** radii, shadows, border colors.
- **Motion:** durations, easings, stagger amounts.
- **Layers:** a small z-index scale (header, menu, overlay).
- **Responsive rules:** breakpoints and how layouts change at each.

Illustrative shape only. Choose real values for the project:

```css
@import "tailwindcss";

@theme static {
  --color-surface: #ffffff;
  --color-ink: #111214;
  --font-sans: "Inter", system-ui, sans-serif;
  --text-display: clamp(3rem, 2rem + 5vw, 7rem);
  --radius-panel: 2rem;
  --ease-out-expo: cubic-bezier(0.16, 1, 0.3, 1);
  --duration-base: 0.7s;
}
```

Every section uses these tokens. No stray hex, px or ms values inside components. Summarise the tokens and the motion language in a short `DESIGN.md`.

---

## Workflow

### 1. Inspect

Read the project structure, existing tokens, components, sections and build process before editing. Keep any required sections and template functionality unless told otherwise.

Check which tools are already installed and use them. Do not just mention them.

- **GSAP official skills** (gsap-core, gsap-timeline, gsap-scrolltrigger, gsap-plugins, gsap-utils, gsap-performance): `npx skills add https://github.com/greensock/gsap-skills`
- **Inspo MCP**, an archive of real production sites with captures, reference components and a DESIGN.md per site: `npx -y inspo-mcp install`
- **A landing-page skill** (optional): https://skills.rest/skill/landing-page-mvp
- **A real browser tool** (Playwright, Chrome DevTools or any browser MCP) for research and QA.

If something is missing, install it using its documented setup when the environment allows. If it is unavailable, say so and continue with what exists. Never claim to have installed or used something unless you verified it.

### 2. Research

- Use Inspo to find layouts and interactions that suit this kind of project.
- Open real reference pages in a browser and watch how they scroll, transition, respond to hover and reflow on mobile. A screenshot cannot show how an interaction works.
- Look at composition, typography, pacing, image placement and transitions. Translate the ideas into something original. Never copy a page.
- Check gsap.com docs and demos for exact APIs when needed.

Keep research focused, then build.

### 3. Choose a direction

Explore several distinct concepts privately. Pick the strongest and write a short direction note: the big idea, palette, type, image treatment, section rhythm and signature motion. Then build without waiting for approval.

If earlier designs are listed in the brief, differ from them structurally (composition, image treatment, section rhythm, bento arrangements, palette, motion choreography). New colors and copy alone do not count as a new design.

### 4. Build

Tokens first, then base styles, then components, then sections, then motion. Add motion last so the page works without it.

### 5. Test and fix

Follow the Browser QA section. Fix what you find and test again.

### 6. Report

Follow the Final report section.

---

## Creative direction

**One idea.** The page expresses a single concept rooted in this business or product, not a generic "modern SaaS" look. Style comes from the brief and the audience. If nothing else is specified, default to clean, light and spacious, with rounded containers, selective pill and circle shapes, large confident type and strong imagery.

**One system, varied sections.** Every page uses the same tokens, type, radii and motion language. Each major section gets its own composition. Pick from:

- A full-bleed hero with integrated content panels
- An asymmetric image and text arrangement
- A varied bento grid
- A bounded stack of photo-led panels
- A sticky visual beside scrolling content
- A wide image reveal
- An editorial feature or service overview
- A horizontal gallery
- An expandable question list
- A prominent conversion block

Do not repeat image-left/text-right all the way down the page. Balance bento grids with other compositions so the page is not a wall of cards. Avoid large unexplained empty areas, but keep room to breathe. Build components that cope with different content lengths and image proportions.

**Typography**

- Every piece of text is at least 15px at every breakpoint: paragraphs, labels, buttons, navigation, captions, helper text. Main paragraphs should be larger (roughly 17 to 20px).
- Large, fluid headings with comfortable line height and normal or slightly open tracking. No tight compression. Keep heading styles consistent across comparable sections.

**House defaults** (change only if the brief says so)

- No eyebrow labels or small uppercase text above headings.
- No decorative numbers, numbered cards, numbered steps or section counters. Numbers that are real content are fine.
- Separators only between list or accordion items. Nothing above the first or below the last.

**Imagery**

- Prefer real photography of the real thing: people, the product in use, the work, the place. Use illustration for subtle backgrounds, decoration, or ideas a photo cannot explain.
- Choose images with clear usage rights (the client's own, Unsplash, Pexels, Pixabay, Wikimedia Commons). Record source and license in a credits file. Avoid watermarks, unrelated branding and repetitive generic stock.
- If no suitable photo is available, use a clearly marked placeholder rather than pretending.
- Responsive images: `srcset` and `sizes`, AVIF or WebP where possible, explicit `width` and `height` (or `aspect-ratio`) to prevent layout shift, `loading="lazy"` below the fold, `fetchpriority="high"` on the hero image. Useful alt text for meaningful images, `alt=""` for decorative ones.

**Content honesty**

- Never invent ratings, reviews, testimonials, awards, licenses, certifications, guarantees, client logos, statistics, years in business or availability. Sample content is labeled as sample.
- Write copy that is natural, specific and useful. No filler marketing language.

---

## Motion language and the GSAP toolbox

Motion is a core part of the design. Build a motion language: a small set of signature interactions, supported by quieter micro-interactions. The aim is a page full of delightful details with enough restraint to feel designed.

**How to use the toolbox**

- Draw from several categories below. As a rough target: one signature hero moment, three to five scroll-driven set pieces, a handful of micro-interactions, and one decorative effect such as gooey blobs or a morph.
- Do not default to fade-up everywhere. Do not repeat the same effect in neighboring sections. Not every section needs to move.
- Use easings and durations from the tokens so the page moves with one voice.
- Animate `transform` and `opacity` (`autoAlpha`). Settled content stays sharp.
- Avoid distracting loops. Pause anything continuous when it is offscreen.
- Keep native scrolling intact. No scroll hijacking.

**Entrances and text**

- **Staggered reveals:** `stagger` with `from: "start" | "center" | "edges" | "random"`, or grid stagger across cards and tiles.
- **Masked line reveals:** SplitText splits a heading into lines, and each line slides up from inside an overflow mask.
- **Character and word cascades:** SplitText chars or words with staggered transforms for hero headlines.
- **Scroll-filled text:** words move from muted to full ink as you scroll (scrubbed).
- **Scramble and decode:** ScrambleTextPlugin turns random characters into the final word. Good for labels, buttons and headline accents.
- **Typewriter:** TextPlugin, used sparingly.
- **Rolling text hover:** a duplicate label slides up on hover for buttons and nav links.
- **Variable-font animation:** tween `font-variation-settings` (weight or width) on hover or scroll when the font supports it.
- **Number counters:** tween numbers with `snap`. Only for real figures.

**Scroll (ScrollTrigger)**

- **Scrub:** ties animation progress to scroll position.
- **Pinned storytelling:** a section holds while steps play inside it.
- **Sticky stacked cards:** cards stack up, and the previous one scales and dims.
- **Sticky visual with scrolling text:** the visual changes as each text block enters (cross-fade, clip-path or Flip).
- **Horizontal scroll sections:** vertical scroll drives a horizontal track. Use `containerAnimation` for triggers nested inside it.
- **Parallax layers:** layers move at different speeds inside their section. ScrollSmoother's `data-speed` and `data-lag` are optional. Native scroll stays the default.
- **Wide image expand:** a card grows to full-bleed as it reaches the center, using clip-path or scale. Pair a mask reveal with a counter-scaled inner image for a premium feel.
- **Curtain reveal:** a section slides away to reveal the one beneath.
- **Section theme shifts:** background and ink tokens tween as sections enter.
- **Scroll progress and scroll-spy:** progress bar, active nav link tied to the section in view.
- **Velocity-reactive effects:** marquee speed, skew or blur driven by `self.getVelocity()`.
- **Image-sequence or video scrubbing:** canvas frames or video `currentTime` tied to scroll.
- **Batch reveals:** `ScrollTrigger.batch()` for lists and grids entering in groups.
- **Snap:** gentle section snapping. Use sparingly and test with keyboard and touch.

**Interaction**

- **Magnetic buttons:** pointer proximity pulls the element (`gsap.quickTo`), and it springs back with an elastic ease.
- **Custom cursor:** a follower built with `quickTo` that grows or shows a label on hover. Fine pointers only.
- **3D tilt cards:** pointer-driven `rotationX` and `rotationY` with perspective and a moving highlight.
- **Cursor spotlight:** a radial gradient follows the pointer through a CSS variable updated with `quickSetter`.
- **Mouse parallax:** hero layers shift at different depths.
- **Hover image reveal:** list rows reveal a floating image that follows the cursor.
- **Image trail:** images spawn along the cursor path and fade out.
- **Button fills:** a circle expands from the point of entry, with an arrow swap.
- **Draggable carousel with inertia:** Draggable plus InertiaPlugin for throw, snap and bounds.
- **Intent-based navigation:** Observer for wheel, touch and pointer gestures and swipe sliders.
- **Accordions:** animate `height: "auto"`, icon rotation and a content stagger, with correct ARIA.
- **Flip transitions:** Flip animates layout changes such as filter and tab switching, a card expanding into a detail view, or shared-element moves.

**Shapes, SVG and decoration**

- **Gooey and metaball blobs:** an SVG filter (`feGaussianBlur` plus `feColorMatrix`) on a decorative layer, so blobs merge as they move.
- **Shape morphing:** MorphSVGPlugin between blobs, icons and shapes.
- **Line drawing:** DrawSVGPlugin for underlines, doodles, routes and scroll-driven strokes.
- **Motion paths:** MotionPathPlugin sends elements along a curve (orbits, trails, scroll-driven journeys).
- **Clip-path transitions:** circle, inset or polygon wipes between images or sections.
- **Displacement hover:** `feDisplacementMap` distorts an image on hover, animating `scale` or `baseFrequency`.
- **Directional motion blur:** a brief `filter: blur()` or SVG `feGaussianBlur` with `stdDeviation="x 0"` during image and card transitions, scaled by velocity and cleared when settled.
- **Physics:** Physics2DPlugin or PhysicsPropsPlugin for confetti, falling objects and gravity pops.
- **Marquees and loops:** infinite `xPercent` loops that pause on hover and offscreen, optionally draggable.

**Easing and control**

- **Custom eases:** CustomEase for brand-specific curves, CustomBounce and CustomWiggle for squash and shake, EasePack for RoughEase, SlowMo and ExpoScaleEase.
- **Timelines:** labels, the position parameter, nested timelines, `timeScale`, and reversible hover timelines.
- **Utilities:** `gsap.utils` (`mapRange`, `clamp`, `wrap`, `snap`, `interpolate`, `random`, `pipe`), `gsap.quickTo`, `gsap.quickSetter`, `gsap.ticker`.
- **`gsap.matchMedia()` and `gsap.context()`:** responsive and reduced-motion conditions with automatic cleanup.
- **GSDevTools:** a timeline scrubber for development only. Remove before shipping.

**Effect rules**

- Gooey filters go on decorative shapes or a dedicated visual layer only. Text and controls sit above them, unfiltered.
- Motion blur is brief and used on a few suitable transitions. Clear the `filter` property when the animation settles so no compositing layer is left behind. Body text and controls always stay readable.
- Any intro or preloader is short and never blocks content.
- Keep decorative effects lightweight. Check performance on a mid-range device profile.

---

## Scroll containment (required)

Every stacked, sticky or pinned animation must stay inside its own section for its whole lifecycle.

- Prefer `position: sticky` inside an explicitly sized parent. It is the most reliable option. If GSAP `pin` is necessary, verify pin spacing, start and end positions, the containing block and layer order. Ancestors with `transform`, `filter` or `overflow: hidden` break sticky and pinning. Use `overflow: clip` where you need to clip without breaking sticky.
- Animated cards must never appear over neighboring sections, cover headings, or float after their section ends. A high `z-index` is not a containment fix.
- Account for: sticky header height (`scroll-margin-top`, trigger offsets), image and font loading, variable content length, resizing and orientation changes, reloading partway down the page, fast scrolling in both directions, and short or narrow screens.
- Refresh measurements at the right moments: after `document.fonts.ready`, after images load, and on resize. Use `invalidateOnRefresh: true` for function-based values.
- Create each ScrollTrigger once. No duplicates, no stale state. Build scroll animations inside `gsap.matchMedia()` so triggers are killed and inline styles are reverted when conditions change.
- Provide a normal-flow layout when the viewport cannot comfortably support the animation (narrow, short, or reduced motion) and when GSAP does not load. Content stays visible in every case.

---

## Accessibility and performance

**Accessibility**

- Landmarks (`header`, `nav`, `main`, `footer`), one `<h1>`, a logical heading order, descriptive link text, buttons for actions and links for navigation, labeled form controls, and a skip link.
- Everything works with a keyboard. Focus is clearly visible (`:focus-visible`). Touch targets are at least 44 by 44 CSS pixels where possible.
- Menus and accordions expose state with `aria-expanded` and `aria-controls`. Escape closes the menu.
- WCAG AA contrast: 4.5:1 for normal text, 3:1 for large text and UI components. Check the actual combinations, including muted text, buttons, hover and focus states, and text over images (use a scrim and measure it). Use https://webaim.org/resources/contrastchecker/ when unsure.
- Respect `prefers-reduced-motion` through `gsap.matchMedia()`. Reduced motion means a complete, readable page with no scrubbing, blur, parallax, animated stacking or marquees. Simple fades or nothing.
- Gate hover effects behind `(hover: hover) and (pointer: fine)`.

**Performance**

- Animate transforms and opacity. Do not animate layout properties (`width`, `height`, `top`, `left`) except deliberate cases like an accordion or Flip.
- Limit large blur filters. Keep gooey layers small. Use `will-change` only temporarily.
- Load only the GSAP plugins you actually use, and defer scripts.
- Preload the hero image and key font, use `font-display: swap`, reserve space for every image, and lazy-load the rest.
- Clean up with `gsap.context()` and `matchMedia` reverts so nothing leaks.

---

## Functionality

- Navigation, mobile menu, anchor links, accordions and every control must work. The mobile menu manages focus, closes on Escape, and exposes its state. Anchor links account for the sticky header.
- Forms have visible labels, native constraint validation and helpful error messages. Form destinations are configurable.
- If there is no backend or endpoint, say clearly that the form is in demo mode. Never show a success message when nothing was sent.
- Phone numbers, emails, addresses and links come from the config file.
- Repeat the main call to action at sensible points so it is never far away.

---

## Browser QA

Test in a real browser. A successful build or a screenshot is not proof.

**Viewports:** wide desktop (1920×1080), desktop (1440×900), tablet (1024×768 and 768×1024), narrow mobile (360×740 and 390×844), and short screens (1280×600 and phone landscape 844×390).

**For every scroll animation, check each stage:** before it starts, entry, middle, exit, after it ends, reverse scrolling, resizing while active, reloading while active, and fast scrolling in both directions. Confirm every animated viewport and card stays within its own section and that neighboring sections remain unobstructed.

**Also verify**

- No horizontal overflow and no clipped content
- Every visible text node computes to at least 15px (run a script over computed styles)
- Heading wrapping and spacing, and text contrast including hover, focus and text over images
- Images load, crop well and hold their space, with long and short content and different image proportions
- Keyboard navigation and visible focus through the menu, accordions and forms
- `prefers-reduced-motion` emulated: the page is complete and readable
- JavaScript disabled, and the GSAP CDN blocked: the page is still readable and usable
- Menus, links, anchors, and forms (valid, invalid and demo behavior)
- Console errors, failed requests and layout shift
- Scrolling feels smooth, and filters do not cause long frames

Capture representative screenshots, fix every issue and test again. Report only what you actually tested.

---

## Final report

Keep it brief and honest.

- The working implementation and where it lives
- The creative direction in a few lines
- The interactions added and which GSAP features or plugins power each one
- Which tools and resources were used, and which were unavailable
- What was actually tested (viewports, scenarios, tools)
- Remaining limitations, assumptions, placeholder content and image credits
