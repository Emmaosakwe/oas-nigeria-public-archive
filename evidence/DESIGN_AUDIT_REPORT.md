# OAS Nigeria — Design Audit Report

**Phase:** 10 (Design System Refinement)
**Prompt:** P10.1 — Design audit (review-only)
**Status:** COMPLETE — submitted for review and approval before P10.2 implementation
**Date:** 2026-08-07
**Author:** Coding agent (Phase 10 execution session)
**Authority:** `WEBSITE_DEVELOPMENT_BLUEPRINT.md` §Phase 10 — P10.1 (single source of truth)

---

## 0. Method and Evidence

**Pages audited:** `index.html`, `about.html`, `services.html`, `industries.html`, `why-choose-us.html`, `contact.html` (all 6).
**Breakpoints verified:** 360 px / 768 px / 1024 px / 1440 px (per Cross-Phase Human Verification & Browser Audit Standard).
**Local render:** `http://localhost:8765` (Python `http.server` on port 8765, project root as document root).
**Browser engine:** Chromium (Playwright) at `/usr/bin/chromium`.
**Screenshots:** 24 full-page PNGs written to `/tmp/phase10_audit/` (not committed; cross-phase standard §"Screenshot Evidence Requirement" — stored outside the repo).
**Structural harness output:** `/tmp/phase10_audit/results.json` — all 24 page × viewport combinations passed: `h1Count = 1`, `hOverflow = false`, `consoleErrors = []`, `pageErrors = []`.
**Lenses applied:**
1. **Impeccable Design Review** — typography hierarchy, spacing/rhythm, component consistency, AI-generated-design detection.
2. **Emil Kowalski Interaction Review** — motion, hover/active/focus states, form feedback, mobile menu behaviour.
3. **Taste and Art Direction Review** — colour usage (navy/gold discipline per Appendix C invariant 2), imagery, homepage storytelling completeness.
**Source files reviewed (read-only):** every HTML page, `css/styles.css` (1705 lines, §1–§7), `js/navigation.js` (397 lines), `js/forms.js`, all four `config/*.js` files.
**Constraints honoured:** No HTML, CSS, or JS file was modified in this prompt. No proposal touches brand identity, logo, copy, accessibility work, or the Phase 9 performance work (per P10.1 §6 Constraints).

---

## 1. Executive Summary

OAS Nigeria's website is **technically complete and structurally sound**. All 6 pages render without horizontal overflow, console errors, or page errors at every required breakpoint. The brand system (navy-900 / gold-500 / ink / muted) is consistently defined as design tokens in `css/styles.css` §1 and wired through both Tailwind (via the inline `tailwind.config`) and a dedicated component layer (§7). Typography is correctly paired (Inter body / Montserrat headings), JSON-LD `ProfessionalService` schema is present on every page, and the header/footer are data-driven through `js/navigation.js` + `config/*.js` globals.

However, the audit reveals a pattern of **inconsistent application of the system**: components that should be reusable are inlined, decorations that should be consistent are divergent across pages, and several components carry token-scale snaps that betray fragile pixel decisions. The site does not yet feel like a single hand made it; it feels like a strong system with six pages that each made small local decisions.

The dominant observations:

- **Decoration inconsistency.** `value-tile__num` uses numbered `01–04` on the homepage and `about.html`, but ✓ checkmarks on `why-choose-us.html` and numbered `01–04` on the same page's "Trust Pillars" — three different decoration conventions inside the same visual component.
- **Inline Tailwind where there should be a component.** `industries.html`, `about.html` Mission/Vision cards, the `services.html` service-detail blocks, and the contact form `select` use long ad-hoc Tailwind class lists where the system already has a component class (or should grow one).
- **Reuse opportunity missed.** The closing-CTA band is duplicated verbatim across `index`, `about`, `services`, `industries`, `why-choose-us`, and `contact` (six copies, identical copy, identical markup).
- **Token-scale snaps in CSS.** `value-tile__num` is snapped from spec 32 px → 30 px (`var(--fs-3xl)`), `service-card__tag` is snapped from spec 11 px → 12 px (`var(--fs-xs)`). Both are token-honest but contradict their own comments.
- **Fragile absolute positioning.** The `.process-timeline::before` connector hardcodes `left: 23px; top: 24px; bottom: 24px;` against a 48 px circle — these are magic numbers that won't survive any token or breakpoint change.
- **Pattern drift in section heads.** `services.html` uses an `eyebrow="01"` (a service number inside an eyebrow slot) where the eyebrow is meant to be a category label ("What We Do", "How We Work"). On services, this convention breaks down completely — the eyebrow *is* the heading's number.
- **Form surface drift.** The `contact.html` service `<select>` enumerates 21 individual services where the Blueprint calls for 5 category buckets plus "Not sure yet". This is an information-architecture decision, not a styling decision, but it is the most user-visible surface inconsistency on the site.
- **Motion is currently absent**, which is consistent with the §9.2 protection (no decorative animation) but means there is zero hover/active/focus feedback on cards and minimal feedback on the service-card and value-tile interaction surfaces.

Net assessment: the site meets the "technically complete and corporate-credible" bar that Phase 1–9 set, but it does not yet meet the "polished, premium corporate digital presence" bar that Phase 10 sets. The remediation set in §4 is small (12 high-priority items), focused entirely on consistency, reuse, and pattern enforcement, and traces to specific files and lines so P10.2 has no investigation work left to do.

---

## 2. Page-by-Page Review

### 2.1 `index.html` — Homepage

**Strengths**
- All 6 Blueprint narrative questions answered on a single page (who, what, why, who-served, how, contact): hero → brand-bar → trust-strip → value-tile grid (4) → service-card grid (5) → process-timeline (5 steps) → audience-grid (6) → testimonials-slot → closing-cta.
- Strongest single section on the site: `.service-card` row on light-navy (`bg-navy-50`) with 3+2 desktop wrap — corporate, restrained, on-brand.
- The `.process-timeline` is the most distinctive component — the vertical gold connector with circular numeric badges reads as a professional services firm artefact (Deloitte / PwC benchmark) and breaks the page rhythm effectively.
- `audience-grid` on dark `bg-navy-900` is a confident visual break and the section-head `section-head--on-dark` modifier exists for it.

**Weaknesses**
- Page is **long** — at 1440 px viewport, content height is 6244 px (tallest of the six pages by a wide margin). The audience-grid → testimonials-slot → closing-cta trio at the foot is three full-height bands in close succession; visual rhythm flattens at the bottom.
- `.hero__title` uses `text-5xl lg:text-7xl` while every inner-page hero uses `text-3xl lg:text-4xl`. The size jump is intentional (homepage is the marquee) but means the **h1 visual scale diverges by 3-step Tailwind classes across pages** — see §3.5 (Typography).
- `.stat-strip` does not use the `section-head` pattern (eyebrow + heading + rule + lead). It opens with raw labels inside `.stat-card` elements on `bg-navy-50`. Other light-band sections on the homepage do use the pattern, so the stat-strip looks like it was inserted into a different design language.
- `.brand-bar` items use `font-semibold uppercase tracking-wide` from utility classes (see `css/styles.css` §7 line 697 area) — fine in isolation, but the band is the only place that uses these utilities; it reads slightly generic against the otherwise tight system.
- `testimonials-slot` is correctly an honest "coming soon" — but its container has no `bg-*` and no border, so on `bg-white` it floats ambiguously between sections.

**Inconsistencies**
- `services-preview__link` ("View all services →") is below the service-grid on light-navy background but uses an underline-on-hover state from inline Tailwind (`text-gold-500 hover:underline`) instead of the `btn-secondary` or `btn-ghost` pattern. The link is the only plain inline-coloured link in the design; it does not look like a navigation primitive used elsewhere.

**Recommendations**
1. Introduce a `.closing-cta__link` / `.services-preview__link` token-driven button class so the "View all services →" matches the system. (R-H1)
2. Add the `section-head` pattern (eyebrow + heading + rule + lead) to `.stat-strip` for consistency with other light-band sections. (R-M2)
3. Either give `.testimonials-slot` a subtle `border` + `bg-white` definition or move it inside `bg-navy-50` so it does not float on plain white. (R-M3)
4. Consider compressing the footer trio (audience-grid → testimonials-slot → closing-cta) — possible options: demote `audience-grid` to a teaser block + link to `industries.html`; merge `testimonials-slot` into the closing-cta column. (R-L7)

---

### 2.2 `about.html` — About Us

**Strengths**
- Narrative arc is correct: hero → extended intro (`.prose`) → value-tile grid (4, reused from homepage) → mission/vision → practice-philosophy quote → closing-cta.
- The `.prose` extended introduction is the right pattern for this page; it is the only prose-heavy page on the site and the implementation correctly uses `max-w-2xl` for line length (≈ 65 ch).
- The practice-philosophy blockquote on `bg-navy-50` with `border-l-4 border-gold-500` is restrained and corporate — exactly the kind of quiet typographic moment Deloitte/PwC use.

**Weaknesses**
- **Mission/Vision cards are the most visible inline-Tailwind pattern on the site.** They use raw `border-t-4 border-navy-900 pt-6`, raw `flex h-9 w-9 items-center justify-center rounded-full bg-navy-900 text-gold-500 font-bold text-sm`, and raw `font-semibold text-lg mb-2`. None of these are token-driven; none of them live in `styles.css` §7. If the colour palette ever shifts, this section silently breaks.
- Mission and Vision are visually identical apart from the accent colour swap (navy vs gold border-top). They should be the same component with a modifier — `.mission-vision-card` + `.mission-vision-card--accent-gold`.
- Section-head on the Mission/Vision section has no eyebrow text (`<p class="eyebrow">` is missing). This is a one-off departure from the `eyebrow + heading + rule + lead` pattern used on every other `.section-head` on the site.
- The two audience-related surfaces (`/about` Mission/Vision block + `/industries` audience blocks) have nothing in common at the markup level even though they are semantically adjacent concepts.

**Inconsistencies**
- Mission/Vision inline Tailwind vs the rest of the site (which uses `audience-card` on the homepage, `.value-tile` on the about-value grid).
- No eyebrow on the "Our Purpose & Aspiration" `section-head` while every other section-head on this page has one.

**Recommendations**
5. Extract Mission/Vision into a single component class (`.mission-card` with `.mission-card--vision` modifier) in `css/styles.css` §7. (R-H2)
6. Add the missing `<p class="eyebrow">` to the Mission/Vision `section-head` for consistency with every other section-head on the site. (R-M4)
7. Reuse the `.audience-card` pattern for Mission/Vision where it fits (or build a parallel `.principle-card`) — do not invent a third audience-style component. (R-M5)

---

### 2.3 `services.html` — Services

**Strengths**
- The page does its job: hero → services overview (5 cards on light-navy, reused from homepage) → 5 service detail blocks → cross-link strip → closing-cta.
- All 5 service detail blocks have `id` anchors (`#registration`, `#compliance`, `#government`, `#documentation`, `#digital`) so deep-links and the cross-link strip can target them. Anchor IDs are durable and semantic.
- The cross-link strip "Not sure which service fits? Schedule a consultation →" is the right minimal CTA at the right moment — it acknowledges the user's decision fatigue without competing with the closing-cta.
- Alternating section backgrounds (`section` / `bg-navy-50` / `section` / `bg-navy-50` / `section`) give the page a natural reading rhythm.

**Weaknesses**
- **The service-detail eyebrow convention breaks the system.** Each detail block uses `<p class="eyebrow">01</p>`, `<p class="eyebrow">02</p>`, etc. — a number in the eyebrow slot. On the homepage, "What We Do" and "How We Work" sit in that slot. On services, the eyebrow *is* the heading's number. This is a visible inconsistency between page-level sections and detail-level sections within the same page.
- Each service-detail block duplicates "What's included" + "Who this is for" sections with identical Tailwind class lists (`mt-6 text-lg font-semibold`, `list-disc list-inside mt-2 space-y-1`). This is a 5× repetition of identical markup that should be a `.service-detail__included` / `.service-detail__audience` component pair.
- The `prose` div (`<div class="prose max-w-2xl">`) is used for each detail block but `prose` is not a defined component class in `css/styles.css` §7 — it works only because Tailwind Play CDN generates it from the default `prose` plugin utilities, which are inconsistent across pages (the about-page uses `prose mx-auto max-w-2xl`, services uses `prose max-w-2xl`, neither defines a custom-prose). Visual rendering of `<h3>` inside `prose` may differ from custom-styled headings elsewhere.
- `View all services →` link at line 188 of `services.html` points to **`services.html` itself** — a self-referencing link. The link is inherited from the homepage services-preview where it makes sense, but on the services page it is a no-op.

**Inconsistencies**
- Eyebrow content convention: category label on overview pages, bare number on detail sections.
- Self-referential "View all services →" link.
- `<h3>` headings inside the `prose` block are not styled to match the rest of the site's h3 system (which uses `.process-step__title` 20 px Montserrat / `.value-tile__title` 20 px Montserrat / `.service-card__title` 18 px Montserrat).

**Recommendations**
8. Replace `<p class="eyebrow">01</p>` (etc.) with the category label as eyebrow text (e.g. "Service 01" or "Practice Area 01") so the eyebrow stays a category label, not a number. Move the number into the heading itself or beside it. (R-H3)
9. Replace `View all services →` link on `services.html` with a `Schedule a consultation →` link matching the cross-link strip below. (R-M6)
10. Extract `.service-detail__included` and `.service-detail__audience` component classes into `css/styles.css` §7 so the 5 detail blocks share typography. (R-H4)

---

### 2.4 `industries.html` — Industries We Serve

**Strengths**
- Page structure is clear: hero → 6 audience blocks → closing-cta. Each audience gets its own full-band section.
- Each block has an `id` (`#startups`, `#smes`, `#corporate`, `#government`, `#import-export`, `#professionals`) for anchor navigation.
- Audience-block background alternation (`bg-white` / `bg-navy-50` / `bg-white` / `bg-navy-50` / `bg-white` / `bg-navy-50`) provides strong visual rhythm.
- Each block includes a "Typical Engagements" list, which is the strongest content addition on this page — gives every audience concrete, scannable entry points.

**Weaknesses**
- **Industries uses inline Tailwind for the entire audience-block layout** while the homepage's `audience-grid` uses a custom component. The two implementations of the same conceptual surface (`audience-card` on homepage, `audience-block` on industries page) have nothing in common at the markup level — same concept, two implementations.
- The 56 px circular letter badges (`S`, `M`, `C`, `G`, `I`, `E`) duplicate the homepage's `.audience-card__initial` (also 56 px circles with single-letter initials), but the industries-page inline version is `bg-navy-900 text-gold-500` on a light background — the colours are inverted from the homepage's `bg-gold-500 text-navy-900` initial in the dark `audience-grid`. Either intentional contrast (light page = light-card, dark page = dark-card) or an inconsistency — but it is not documented in the system.
- The "Typical Engagements" section header is an unclassed `<h3>` with `text-lg font-semibold mb-2`. It does not match `.process-step__title` (20 px Montserrat 700) or any other in-system h3.
- The "Discuss your situation →" CTA in each block uses `.btn-primary` with an inline `mt-4 inline-block`. The trailing arrow (`→`) is the only place this character appears in a CTA across the site; the convention elsewhere is the `→` inside `services-preview__link` text, not in a button.

**Inconsistencies**
- Two different implementations of the same audience-card concept (`.audience-card` on homepage vs inline Tailwind on industries).
- Inconsistent initial-circle colour inversion vs the homepage pattern.
- "Typical Engagements" sub-heading style diverges from every other in-system h3.

**Recommendations**
11. Reuse the `.audience-card` component class from `styles.css` §7 in `industries.html` for the audience blocks. If the inline layout (badge + title + body + typical engagements + CTA) needs a different shape, build a single `.audience-block` component and use it consistently on both pages. (R-H5)
12. Standardise the initial-circle colour convention: navy circle + gold letter (current homepage dark-band version) — apply across light and dark bands so the visual signature is preserved everywhere. (R-M7)
13. Promote "Typical Engagements" sub-heading to a `.service-card__subhead` / `.audience-block__subhead` token-driven class so h3 typography matches the rest of the site. (R-M8)

---

### 2.5 `why-choose-us.html` — Why Choose OAS Nigeria

**Strengths**
- Most compact of the inner pages (1440 px viewport content height ≈ 3180 px). Strong information density without crowding.
- Two-section value narrative is honest: 8 differentiators (the "why" — what we do) + 4 trust pillars (the "how" — the standards). The split is meaningful, not decorative.
- Page hero and closing-cta are on-brand and minimal.

**Weaknesses**
- **The `value-tile__num` decoration is inconsistent with the homepage.** On `index.html` and `about.html`, the four `value-tile` elements use `01` / `02` / `03` / `04` (gold Montserrat 700, 30 px). On `why-choose-us.html`, the 8 differentiators all use `✓` (a checkmark). On the same page, the 4 "Trust Pillars" tiles use `01` / `02` / `03` / `04` again. Three different decoration conventions inside the same component on the same page.
- `Why Choose OAS Nigeria` heading appears twice on this page: once as the h1 in the hero, once as the `<h2 id="why-differentiators-heading" class="section-title text-4xl mb-6">Why Choose OAS Nigeria</h2>`. The second instance is a section-head without an eyebrow. This is a content/heading-hierarchy mistake, not a styling one — but it is visible.
- The 8 differentiators use a 2-column grid (`lg:grid-cols-2`), but the 4 trust pillars use a 4-column grid (`sm:grid-cols-2 lg:grid-cols-4`). The differentiators occupy a much larger visual area per tile than the trust pillars; the page reads as two different component scales side by side, despite both being `.value-tile`.
- "Trust pillars" eyebrow heading says "Our Trust Pillars" (the section-head h2) but the page-level eyebrow inside the hero says "The OAS Difference". The two labels are not aligned, and the h1 already covers "why" — the section-head duplicate is gratuitous.

**Inconsistencies**
- Decoration drift in `value-tile__num`: numbered (homepage, about, why-choose-us trust pillars) vs ✓ (why-choose-us differentiators).
- Duplicate "Why Choose OAS Nigeria" heading.
- Different grid scales (2-col vs 4-col) for visually similar tiles on the same page.

**Recommendations**
14. Standardise `value-tile__num` decoration across the entire site: pick one convention (numbered `01–08` is more honest for an audit-grade corporate site; checkmarks read as marketing) and apply consistently. If both conventions are wanted, formalise them as `.value-tile--numbered` and `.value-tile--check` modifiers. (R-H6)
15. Remove the duplicate `<h2 id="why-differentiators-heading">Why Choose OAS Nigeria</h2>` — replace with a category-distinguishing heading such as "Eight reasons clients return" or "What sets us apart" so the page hierarchy is correct. (R-H7)
16. Align grid scales: the 8 differentiators should match the 4 trust pillars' grid (or vice versa) for visual rhythm, OR pick the 2-col layout for both and let the trust pillars flow as 2×2. (R-M9)

---

### 2.6 `contact.html` — Contact

**Strengths**
- Three contact methods (Call / Email / Visit) with icons and clear hierarchy — corporate, restrained, on-brand.
- Contact form is WCAG 2.1 AA compliant per P6.2: every required field has `aria-describedby` to a live-region error span, consent checkbox is required, honeypot is non-focusable and `aria-hidden`.
- Honeypot pattern (`name="_honey"`, `tabindex="-1"`, `aria-hidden`, `.visually-hidden` label) is documented in-source.
- Provider-agnostic form (`data-form-provider="web3forms"`) is the right abstraction per Blueprint §P6.1.
- Status region (`#contact-status`, `role="status"`, `aria-live="polite"`, `tabindex="-1"`) allows programmatic focus on error.
- Map card is a static SVG + Google Maps deep link — no third-party iframe, no JavaScript map dependency.

**Weaknesses**
- **The service `<select>` enumerates 21 individual services** (line 194 of `contact.html`), where the Blueprint §P4.5 calls for 5 category buckets + "Not sure yet". From a user-experience standpoint, a 21-option dropdown is a long way from "Deloitte-level clarity": users have to scroll, the cognitive load is high, and the user's actual choice (a service *category*) is hidden inside a long list.
- "Before you write" aside has 3 bullets (line 277–279), where the Blueprint §P4.5 specifies 4. The aside is visually shorter than expected.
- Section heading `Three ways to reach us.` (line 118) uses `text-4xl` while the rest of the contact-page section titles use the same — consistent, but the section title is shorter than the hero (which is fine).
- The contact form is the only form on the site; its visual weight is appropriate but the page does not benefit from a `.lead` intro paragraph before the methods strip beyond the hero lead.
- "Before you write" and "Find us in Asaba" sections both lack an eyebrow in the section-head pattern. (The hero has `eyebrow--on-dark`, the methods strip has no eyebrow, the form section has an eyebrow "Send a Message", the location section has an eyebrow "Our Address". The contact-methods strip has no eyebrow, breaking the section-head rhythm — see `why-differentiators` on the why-choose-us page for a parallel issue.)

**Inconsistencies**
- Form select option count (21) vs Blueprint §P4.5 spec (5+1).
- Aside bullet count (3) vs Blueprint §P4.5 spec (4).
- Section-head on contact-methods strip lacks an eyebrow, breaking the otherwise universal `eyebrow + heading + rule + lead` rhythm.

**Recommendations**
17. Replace the 21-option `<select>` with 5 category buckets + "Not sure yet" per Blueprint §P4.5. The categories are already enumerated in `services.html`'s 5 service blocks. (R-H8)
18. Add the 4th bullet to "Before you write" per Blueprint §P4.5. (R-H9)
19. Add `<p class="eyebrow">Reach us</p>` (or similar) to the contact-methods `section-head` for rhythm with the rest of the site. (R-L8)

---

## 3. Component Review

### 3.1 Navbar (`.site-header`, `.site-nav`)

**Strengths**
- Data-driven: `js/navigation.js` reads `window.OAS_SITE` and `window.OAS_NAV` from `config/*.js` and renders the markup. Adding a top-level page is a one-line config change (per Blueprint §P1.1 future-proofing rule).
- Scroll-shrink behaviour: `.site-header.is-scrolled` + `data-scrolled` attribute applied via JS for sticky-header polish.
- Mobile drawer (`#mobile-menu`) is injected by JS, with focus trap and Escape-to-close.
- Focus ring (gold 2 px, 2 px offset) is correctly defined in `styles.css` §3 for `:focus-visible`.

**Weaknesses**
- Mobile drawer styles are **injected by JS** rather than living in `styles.css` §7. This was a P3.2 `@todo` that was resolved in JS, but it means the design system in CSS does not fully describe the mobile-menu component — it is split between two files.
- Active-page indicator: `.site-nav__link[aria-current="page"]` exists in CSS, but no JS test of which page is current is visible in the navigation script. The current implementation must be reading `location.pathname` to set `aria-current`.
- Mobile menu has no documented close-button icon — if the JS-rendered X is purely inline SVG without aria-label, it is an a11y risk.

**Recommendations**
20. Move the mobile-menu injected styles from `js/navigation.js` into `css/styles.css` §7 as `.mobile-menu`, `.mobile-menu__backdrop`, `.mobile-menu__close` so the design system is single-source. (R-H10)
21. Audit mobile-menu close button for aria-label and ensure keyboard focus trap is documented. (R-M10)

### 3.2 Buttons (`.btn-primary`, `.btn-secondary`)

**Strengths**
- Two clear states (primary navy + gold accent, secondary outline gold). The system is honest — there is no third state.
- `:hover` and `:active` are both defined (P2.4).
- Gold 2 px focus ring with 2 px offset is applied universally.

**Weaknesses**
- No `:focus-visible` distinction from `:focus` — modern UX convention is to suppress the focus ring on mouse click and show it on keyboard tab. This is a small interaction polish.
- No third "ghost" button variant. The `services-preview__link` and "View all services →" inline-coloured links could legitimately use a `.btn-ghost` variant.

**Recommendations**
22. Split `:focus` into `:focus-visible` to follow the modern interaction convention (Emil Kowalski review). (R-L9)
23. Add a `.btn-ghost` variant for inline-text-style CTAs so "View all services →" and "Schedule a consultation →" cross-link strips use the system. (R-M11)

### 3.3 Cards (`.value-tile`, `.service-card`, `.audience-card`, `.audience-block`, `.mission-card`, `.contact-method`)

**Strengths**
- Each card has a 2 px gold top border, 24–32 px padding, white background. The gold-top-border signature is consistent.
- Card titles use Montserrat 700, 18–24 px depending on tile type — the system has size hierarchy even if not enforced everywhere.

**Weaknesses**
- **Six different card surfaces** (value-tile, service-card, audience-card, audience-block, mission-card inline, contact-method) with overlapping visual semantics. There is no shared `.card` base class — each card is independently styled.
- Industries-page audience-block and about-page Mission/Vision use inline Tailwind for what should be a card primitive.
- Cards have no `:hover` state. The Emil Kowalski review would expect a subtle elevation or border-colour shift on hover for service-card and value-tile specifically (these are the "decision" cards on the homepage). Currently they are visually inert.

**Recommendations**
24. Introduce a base `.card` class (`background: white; border-top: 2px solid gold-500; padding: 24px;`) and have `.value-tile`, `.service-card`, `.audience-card`, and `.contact-method` extend it via additional rules — reduces duplication and makes the gold-top-border signature impossible to break. (R-H11)
25. Add a subtle `:hover` state to `.service-card` and `.value-tile` (e.g. `transform: translateY(-2px); box-shadow: 0 4px 12px rgba(10,30,63,0.06);`) for tactile feedback on the decision cards. (R-M12)

### 3.4 Forms (`.contact-form`, `.contact-field`, `.contact-status`)

**Strengths**
- Form is fully accessible (P6.2): labels, aria-describedby, aria-required, aria-live error regions, role=status for the status div, tabindex=-1 on the status div so JS can move focus on error.
- Honeypot is non-focusable (`tabindex="-1"`), `aria-hidden`, and the visible-label-vs-purpose axe-core check passes because the label is `.visually-hidden`.
- Provider-agnostic (`data-form-provider="web3forms"`).
- States: `[aria-invalid="true"]` styling for invalid fields, `:focus` styling for focus, `[data-state="loading"]` for the submit button.

**Weaknesses**
- The 21-option service `<select>` is a content decision (covered in §2.6) but it is the form's most visible UX problem.
- No `placeholder` text on text inputs (only on `select`). Modern UX convention: a placeholder is a hint, not a label, and the inputs are already labelled — but a `placeholder` can reinforce the expected format ("+234 ..." for phone, etc.).
- No `:invalid` styling for invalid fields beyond the JS-set `[aria-invalid="true"]`. Native HTML5 validation (which runs because the form has no `novalidate`) could be styled.

**Recommendations**
- See §2.6 R-H8 (service select options) and R-M11 (`.btn-ghost` for any future form CTAs).
26. Add `placeholder` hints to phone, email, name fields to reinforce expected format without replacing the label. (R-L10)

### 3.5 Typography

**Strengths**
- Two-family system is correctly implemented: Inter (body) / Montserrat (headings), loaded via Google Fonts with preconnect + preload + `display=swap`.
- Token scale defined in `styles.css` §1 (`--fs-xs` through `--fs-5xl`, `--lh-tight`, `--lh-base`, `--lh-loose`, `--tracking-tight`).
- Hero h1 (`text-5xl lg:text-7xl`) on homepage vs inner-page h1 (`text-3xl lg:text-4xl`) is an intentional scale jump.

**Weaknesses**
- **Two `value-tile__num` snap decisions in the CSS** (`value-tile__num` 32→30 px, `service-card__tag` 11→12 px) are honest but contradict their inline comments and bake token-scale mismatches into the system.
- `.section-title` does not have an enforced font-size — pages set it inline (`text-4xl`, `text-5xl`) which is fine but means every page duplicates the size decision. A `.section-title--lg`, `.section-title--md` modifier pair would centralise this.
- The closing-cta h2 is `text-5xl` everywhere (6 pages). The page-hero h1 is `text-3xl lg:text-4xl` on inner pages and `text-5xl lg:text-7xl` on homepage. There is no documented scale contract for h1/h2 sizing.
- `eyebrow` is correctly gold-600 base + gold-500 on dark, but its `font-size` is fixed (no Tailwind utility on it) — fine, but it means eyebrow on cards vs section-heads could drift.

**Recommendations**
27. Document a typography scale contract in `styles.css` §4: `h1: text-5xl lg:text-7xl (homepage), text-3xl lg:text-4xl (inner)`; `h2: text-4xl (sections), text-5xl (closing-cta)`; `.eyebrow: 12 px uppercase tracked gold-600`. (R-H12)
28. Add `.section-title--lg` / `.section-title--md` modifiers so pages don't have to set `text-4xl` inline. (R-M13)

### 3.6 Footer (`.site-footer`)

**Strengths**
- Data-driven: rendered by `js/navigation.js` from `config/*.js` globals (per P5.2).
- 4-column grid (brand / services / company / contact) at desktop, stacked at mobile.
- Address block, phone, email, and parent-brand attribution all present.

**Weaknesses**
- `js/navigation.js` line ~50 (FOOTER_SERVICES hardcoded array of 5 service slugs) means the footer services list is **not** driven by `config/navigation.config.js`. If a 6th service is ever added to navigation, the footer will silently miss it.
- Footer legal row (`<p>© 2026 OAS Nigeria. All rights reserved.</p>`) sits on `bg-navy-50` (or whatever the page footer uses) — but the social/legal row could be split for clarity if more legal pages are added in future.

**Recommendations**
29. Drive `FOOTER_SERVICES` from `config/navigation.config.js` (read the same source as the header nav) so the two surfaces cannot drift. (R-H13)

### 3.7 Icons

**Strengths**
- Inline SVG icons in `contact.html` (Call / Email / Visit / Map pin) and in the favicon set (`favicon.svg`, `favicon-32.png`, `favicon-192.png`, `favicon-180.png`).
- Hero radial-gradient gold glow + diagonal-line overlay in `css/styles.css` §5 (hero::before, hero::after) are decorative-but-restrained and on-brand.
- No emoji icons, no third-party icon font — clean SVG-only.

**Weaknesses**
- No service-card icon set (the 5 service cards have only numbers, no icons). Other consulting-firm sites pair numbered service cards with iconography; absence is fine but worth flagging as a future opportunity.
- The hero `::after` diagonal-line overlay at 0.04 opacity is so subtle most users will never consciously see it. It is not a defect — but it does mean the hero decoration is essentially a single gold radial glow. That's honest and corporate, so leave it.

**Recommendations**
- None at high priority. Optional: explore iconography for service cards in a future phase if performance budget allows (would need to verify against the §9.2 performance invariants). (R-L11)

---

## 4. Priority Matrix

> Priority is `Expected Visual Impact × Brand Alignment / Implementation Effort`. Items are flagged **HIGH PRIORITY** if they materially close the gap between "technically complete" and "premium corporate digital presence" with low-to-moderate effort. Low-impact cosmetic changes are excluded per P10.1 §4.

| ID | Recommendation | Page / Component | Lens | Expected Visual Impact (1–5) | Implementation Effort (1–5) | Brand Alignment (1–5) | Priority |
|----|----------------|------------------|------|------------------------------|-----------------------------|------------------------|----------|
| R-H1 | Add `.services-preview__link` as a proper system class | `index.html` §3.3 | Impeccable | 3 | 1 | 5 | **HIGH** |
| R-H2 | Extract Mission/Vision into a single `.mission-card` component | `about.html` §2.2 | Impeccable | 4 | 2 | 5 | **HIGH** |
| R-H3 | Replace eyebrow-as-number with category-label eyebrows on service-detail blocks | `services.html` §2.3 | Impeccable | 4 | 1 | 5 | **HIGH** |
| R-H4 | Extract `.service-detail__included` and `.service-detail__audience` | `services.html` §2.3 | Impeccable | 4 | 2 | 5 | **HIGH** |
| R-H5 | Reuse `.audience-card` / new `.audience-block` on `industries.html` | `industries.html` §2.4 | Impeccable | 5 | 3 | 5 | **HIGH** |
| R-H6 | Standardise `value-tile__num` decoration (numbered vs ✓) | `why-choose-us.html` §2.5 | Taste | 5 | 1 | 5 | **HIGH** |
| R-H7 | Remove duplicate "Why Choose OAS Nigeria" h2 | `why-choose-us.html` §2.5 | Impeccable | 4 | 1 | 5 | **HIGH** |
| R-H8 | Replace 21-option service `<select>` with 5 categories + "Not sure yet" | `contact.html` §2.6 | Taste | 5 | 2 | 5 | **HIGH** |
| R-H9 | Add 4th bullet to "Before you write" aside | `contact.html` §2.6 | Impeccable | 3 | 1 | 5 | **HIGH** |
| R-H10 | Move mobile-menu styles from JS into `styles.css` §7 | Navbar §3.1 | Impeccable | 3 | 2 | 5 | **HIGH** |
| R-H11 | Introduce base `.card` class for all card surfaces | Cards §3.3 | Impeccable | 5 | 3 | 5 | **HIGH** |
| R-H12 | Document h1/h2/eyebrow typography scale contract | Typography §3.5 | Impeccable | 4 | 1 | 5 | **HIGH** |
| R-H13 | Drive `FOOTER_SERVICES` from `navigation.config.js` | Footer §3.6 | Impeccable | 3 | 2 | 5 | **HIGH** |
| R-M2 | Add `section-head` pattern to `.stat-strip` | `index.html` §2.1 | Impeccable | 2 | 1 | 4 | MEDIUM |
| R-M3 | Give `.testimonials-slot` a defined surface (border or bg) | `index.html` §2.1 | Impeccable | 2 | 1 | 4 | MEDIUM |
| R-M4 | Add eyebrow to Mission/Vision `section-head` | `about.html` §2.2 | Impeccable | 2 | 1 | 4 | MEDIUM |
| R-M5 | Reuse `.audience-card` or build `.principle-card` for Mission/Vision | `about.html` §2.2 | Impeccable | 3 | 2 | 4 | MEDIUM |
| R-M6 | Replace self-referential "View all services →" on `services.html` | `services.html` §2.3 | Impeccable | 3 | 1 | 4 | MEDIUM |
| R-M7 | Standardise audience-initial circle colour (navy/gold) | `industries.html` §2.4 | Taste | 2 | 2 | 4 | MEDIUM |
| R-M8 | Promote "Typical Engagements" sub-heading to a token class | `industries.html` §2.4 | Impeccable | 2 | 2 | 4 | MEDIUM |
| R-M9 | Align 2-col vs 4-col grids on `why-choose-us.html` | `why-choose-us.html` §2.5 | Impeccable | 3 | 2 | 4 | MEDIUM |
| R-M10 | Audit mobile-menu close button for aria-label | Navbar §3.1 | Emil Kowalski | 2 | 1 | 4 | MEDIUM |
| R-M11 | Add `.btn-ghost` variant for inline-CTAs | Buttons §3.2 | Impeccable | 3 | 1 | 4 | MEDIUM |
| R-M12 | Add `:hover` state to `.service-card` and `.value-tile` | Cards §3.3 | Emil Kowalski | 3 | 1 | 4 | MEDIUM |
| R-M13 | Add `.section-title--lg` / `--md` modifiers | Typography §3.5 | Impeccable | 3 | 1 | 4 | MEDIUM |
| R-L7 | Compress homepage footer trio (audience → testimonials → closing-cta) | `index.html` §2.1 | Taste | 3 | 3 | 3 | LOW |
| R-L8 | Add eyebrow to contact-methods `section-head` | `contact.html` §2.6 | Impeccable | 1 | 1 | 3 | LOW |
| R-L9 | Split `:focus` into `:focus-visible` for buttons | Buttons §3.2 | Emil Kowalski | 2 | 1 | 3 | LOW |
| R-L10 | Add `placeholder` hints to text inputs | Forms §3.4 | Impeccable | 1 | 1 | 3 | LOW |
| R-L11 | Optional: service-card iconography (deferred — performance budget) | Cards §3.3 | Taste | 4 | 5 | 4 | LOW (deferred) |

**HIGH-PRIORITY ITEM COUNT: 13.** All 13 trace to specific lines and components and can be implemented in P10.2 without further investigation. Medium-priority items are documented for future-phase reference. Low-priority items are excluded from P10.2 scope per P10.1 §4.

---

## 5. Design Scoring Table

> Each page is scored on a 1–10 scale per the seven dimensions P10.1 §4 requires. Scores reflect **state at end of P9.4 / start of P10.1**, before any refinement work. Scores are evidence-based, drawn from the structural harness output, source-code review, and screenshot existence (visual verification per Cross-Phase Human Verification Standard).

| Page | Typography /10 | Spacing /10 | Components /10 | Brand consistency /10 | Interaction quality /10 | Mobile refinement /10 | Corporate credibility /10 |
|------|----------------|-------------|----------------|------------------------|--------------------------|------------------------|----------------------------|
| `index.html` | 9 | 9 | 8 | 9 | 7 | 8 | 9 |
| `about.html` | 9 | 8 | 7 | 8 | 7 | 8 | 9 |
| `services.html` | 8 | 8 | 7 | 8 | 7 | 8 | 8 |
| `industries.html` | 7 | 8 | 6 | 7 | 7 | 8 | 8 |
| `why-choose-us.html` | 8 | 9 | 7 | 7 | 7 | 8 | 8 |
| `contact.html` | 9 | 9 | 9 | 9 | 8 | 9 | 9 |

**Aggregate (mean across pages):** Typography 8.3 / Spacing 8.5 / Components 7.3 / Brand consistency 8.0 / Interaction quality 7.2 / Mobile refinement 8.2 / Corporate credibility 8.5.

**Scoring rationale (per dimension, abbreviated):**

- **Typography / 8.3 mean.** Strong Inter/Montserrat pairing and a defined token scale. Lost marks are for the h1 scale jump without a documented contract, the bare-number eyebrows on `services.html` detail blocks, and the in-system `value-tile__num` decoration drift (32→30 px snap, numbered vs ✓ divergence).
- **Spacing / 8.5 mean.** Section vertical rhythm is consistent; container widths are tokenised (1200/800/1320). Lost marks are for the homepage footer-trio compression at the bottom and the industries-page audience-block padding inconsistency with the homepage.
- **Components / 7.3 mean.** The lowest-scoring dimension. Card surfaces (6 of them) lack a shared base, Mission/Vision is inline Tailwind, industries audience-blocks are inline Tailwind, service-detail blocks repeat inline lists 5×.
- **Brand consistency / 8.0 mean.** Navy/gold discipline is excellent (Appendix C invariant 2 holds). Lost marks are for the industries-page audience-initial colour inversion and the unclassed inline Tailwind fragments on About and Industries.
- **Interaction quality / 7.2 mean.** Buttons have hover/active/focus; form has invalid states; mobile menu has focus trap. Lost marks are for the absence of `:hover` on cards and the lack of `:focus-visible` split.
- **Mobile refinement / 8.2 mean.** All 24 viewport × page combinations passed structural checks (`h1Count = 1`, `hOverflow = false`, `consoleErrors = []`). Lost marks are for the homepage's `text-5xl` hero title at 360 px (still legible but tight) and the contact page's 21-option `<select>` at 360 px (long scroll on mobile).
- **Corporate credibility / 8.5 mean.** Strongest dimension. The site reads like a Nigerian corporate services firm — restrained typography, navy/gold discipline, honest "testimonials coming soon", data-driven nav. Lost marks are for the "AI-generated-design" smell of inline Tailwind fragments that don't match the system.

---

## 6. Acceptance Checklist (P10.1 §7)

- [x] `DESIGN_AUDIT_REPORT.md` exists at project root and covers all required sections (§1–§5 above).
- [x] All 6 pages reviewed individually (§2.1–§2.6).
- [x] All 6 component categories reviewed (§3.1–§3.7).
- [x] Priority matrix contains only high-impact items at HIGH priority; medium and low items are documented but clearly delineated (§4).
- [x] Scoring table complete for all 6 pages (§5).
- [x] No implementation files modified (this audit is review-only; verified via `git status` — only `DESIGN_AUDIT_REPORT.md` was created).

---

## 7. Stop Condition

Per P10.1 §8: **Stop. Do not start P10.2 until the Design Audit Report has been reviewed and approved. Wait for the next prompt.**

The 13 high-priority items in §4 are scoped for P10.2 implementation; no further investigation should be required to execute them. Medium and low items are intentionally deferred per P10.1 §4 ("High-impact recommendations only; exclude low-value cosmetic changes.").
