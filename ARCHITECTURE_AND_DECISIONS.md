# Architecture and Decisions

This document outlines the architectural approach, design system, and key decisions made during the OAS Nigeria website project.

## Architectural Overview

The website is built as a static HTML5 site with the following characteristics:
- **No build step**: The site relies on plain HTML5, Tailwind CSS via the Play CDN, and vanilla JavaScript. There is no npm, bundler, or framework.
- **Config‑driven, not hardcoded**: Brand values, navigation, and the contact‑form provider are centralized in `config/*.js` files and read at runtime via `window.OAS_*` globals. This avoids duplication across pages and simplifies future updates.
- **Component‑hook pattern without a framework**: Static pages carry empty `data-component="site-header"` and `data-component="site-footer"` placeholders. A small vanilla‑JS layer (`js/navigation.js`) injects the real markup at runtime, ensuring every current and future page gets a consistent header and footer.
- **Provider abstraction for external services**: The contact form's backend (e.g., Web3Forms) is selected by a single `provider` field in `config/contact.config.js` and dispatched to in `js/forms.js`. Swapping providers is a one‑file change, requiring no UI edits.
- **Progressive enhancement**: The page is meaningful without JavaScript (forms degrade to a clear "email us directly" fallback). JavaScript enhances the experience (e.g., dynamic header/footer, form validation, state management).

## Design System

The design system is defined in `css/styles.css` and includes:
- **Design tokens**: CSS custom properties for colors (`--color-*`), font sizes (`--fs-*`), line heights (`--lh-*`), spacing (`--space-*`), etc.
- **Typography**: Inter for body text, Montserrat for headings, loaded via Google Fonts with `preconnect`, `preload`, and `display=swap`.
- **Layout primitives**: Containers, grids, and utility classes derived from the token scale.
- **Utilities**: Tailwind utility classes extended to match the design tokens (colors, font families, tracking).
- **Component styles**: Reusable components such as `.value-tile`, `.service-card`, `.audience-card`, `.btn-primary`, `.btn-secondary`, etc., each with a 2 px gold top border, consistent padding, and defined typography.

## Key Decisions

### 1. Technology Stack
- **Chosen**: Static HTML5 + Tailwind CSS (Play CDN) + vanilla JavaScript.
- **Reason**: To meet the "no build step" constraint, keep the site cheap to extend, and avoid framework lock‑in. The Tailwind Play CDN allows utility‑first styling without a build process.
- **Alternative Considered**: A traditional build step with PostCSS or a framework like React was rejected because it would violate the no‑build‑step requirement and add complexity for a static marketing site.

### 2. Data‑Driven Navigation and Footer
- **Chosen**: Navigation and footer rendered from `config/navigation.config.js` via `js/navigation.js`.
- **Reason**: Adding a top‑level page requires only a one‑line change in the config file. This ensures consistency between header and footer and simplifies future growth (e.g., client portal, blog).
- **Alternative Considered**: Hardcoding navigation in each page was rejected because it would create maintenance overhead and inconsistency.

### 3. Provider‑Agnostic Contact Form
- **Chosen**: The contact form uses a `data-form-provider` attribute and a handler in `js/forms.js` that reads configuration from `config/contact.config.js`.
- **Reason**: Allows the backend to be swapped (e.g., from Web3Forms to Formspree or a custom endpoint) without touching the UI. The form validates progressively and degrades to a native HTML5 baseline when JavaScript is disabled.
- **Alternative Considered**: Hardcoding the Web3Forms endpoint or using a third‑party form service directly in the HTML was rejected because it would lock the site to a specific provider.

### 4. Font Loading Strategy
- **Chosen**: Load Inter and Montserrat via Google Fonts with `<link rel="preconnect">`, `<link rel="preload" as="style">`, and `display=swap`. Additionally, a warm‑only preload (additional `<link rel="preload" as="style">` alongside the existing stylesheet) was implemented in P9.4.2 to warm the font connection without causing FOUC.
- **Reason**: The `display=swap` policy prevents invisible text during font loading. The warm‑only preload is failsafe: if the preload fails, the synchronous stylesheet still applies the fonts.
- **Alternative Considered**: An `onload‑swap` pattern (replacing the synchronous stylesheet with a preload that switches on load) was rejected because it reintroduces a JavaScript dependency for styling and can cause a flash of unstyled text (FOUC) if the preload fails.

### 5. Accessibility Primitives
- **Chosen**: Skip link, visible focus ring (2 px gold‑500 outline), `prefers‑reduced‑motion` support, semantic landmarks (`header`, `nav`, `main`, `footer`), labelled form fields with `aria-describedby` error linkage, and a focus‑trapped mobile menu.
- **Reason**: These are baseline requirements for WCAG 2.1 AA and were treated as non‑negotiable gates per milestone.
- **Alternative Considered**: Omitting any of these would have failed accessibility validation and was not permitted.

### 6. SEO Foundation
- **Chosen**: Unique `<title>` and meta description per page, Open Graph and Twitter Card tags, JSON‑LD `ProfessionalService` schema on every page, `robots.txt` and `sitemap.xml`.
- **Reason**: To ensure discoverability, indexability, and rich sharing on social platforms. The schema provides structured data to search engines about the business.
- **Alternative Considered**: Omitting structured data or using generic metadata would have reduced the site's usefulness in search results and was not permitted per the blueprint.

### 7. Performance Optimization Within Constraints
- **Chosen**: Optimized images (SVG for logos, compressed JPG for photos), critical CSS inlining for the homepage hero (not implemented due to constraints), and font loading via `display=swap` with a warm‑only preload (P9.4.2).
- **Reason**: The no‑build‑step constraint prevented advanced techniques like code splitting, CSS minification via a build step, or async script loading with safe fallbacks. The warm‑only Google Fonts preload was the safest alternative that could be measured for improvement.
- **Alternative Considered**: Asynchronous loading of the Tailwind Play CDN or local stylesheet was rejected because it caused blank pages or FOUC under the project's architecture (see Performance Restoration Report).

### 8. Future‑Proofing
- **Chosen**: The architecture was designed to accommodate future modules without restructuring:
  - Navigation is config‑driven (new top‑level page = one config entry + one HTML file).
  - The `future` array in `navigation.config.js` reserves nav slots for Client Portal, AI Assistant, Compliance Dashboard, Appointment Booking, etc.
  - The `partials/` directory is reserved for future template reuse.
  - The contact‑form provider abstraction is a template for every future integration (e.g., payment gateway, user accounts).
- **Reason**: To avoid a rebuild when adding later features, saving development time and ensuring consistency.
- **Alternative Considered**: A monolithic structure that would require redesign for future modules was rejected because it would increase long‑term technical debt.

## Sources

- `WEBSITE_DEVELOPMENT_BLUEPRINT.md`
- `PROJECT_TRANSITION_KIT_1_.md`
- Implementation files: `css/styles.css`, `js/navigation.js`, `js/forms.js`, `config/*.js`
- Reports: `DESIGN_AUDIT_REPORT.md`, `Performance_Restoration_Report.md`
