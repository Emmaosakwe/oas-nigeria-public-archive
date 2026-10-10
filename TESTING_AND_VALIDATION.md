# Testing and Validation

This document outlines the testing methods, validation checklists, and reports used to ensure the OAS Nigeria website met its requirements for accessibility, performance, SEO, and correctness.

## Testing Philosophy

Testing and validation were integrated into every milestone. No milestone was considered complete until it passed the required verification checks, which treated accessibility, performance, and responsiveness as non‑negotiable gates.

## Types of Testing

### 1. Automated Testing
- **Lighthouse**: Runs were performed to measure performance, accessibility, best practices, and SEO. Scores and individual audits were recorded as JSON reports.
- **Automated accessibility checking**: Tools like `axe-core` (via Lighthouse or direct CLI) were used to detect violations.
- **HTML validation**: Basic markup correctness was checked (e.g., via browser developer tools or online validators).

### 2. Manual Testing
- **Browser audits**: The site was loaded in Chromium (via Playwright or manual inspection) at four breakpoints: 360px, 768px, 1024px, and 1440px.
- **Keyboard navigation**: Tab order was verified to ensure a logical flow, visible focus rings, and no keyboard traps.
- **Screen‑reader walkthroughs**: Navigated with NVDA or VoiceOver to confirm that announcements were correct and that focus management worked (e.g., on form errors).
- **Form testing**: The contact form was tested with valid and invalid inputs, including the honeypot and consent checkbox.
- **Link and metadata verification**: Confirmed that titles, meta descriptions, Open Graph tags, and JSON‑LD schema were present and correct.

### 3. Checklist‑Based Validation
Each phase had specific validation checklists that outlined mandatory items. Examples:
- **P6.2 Accessibility Hardening Checklist**: Verified skip link, focus ring, ARIA labels, error linkage, etc.
- **P9.4 Validation Checklist**: Confirmed that the warm‑only Google Fonts preload did not introduce regressions and delivered a measured performance improvement.
- **Design Audit (P10.1)**: A comprehensive review of design consistency, component reuse, and interaction quality.

### 4. Reporting
After validation, reports were generated to summarize findings:
- **Performance Restoration Report (P9.4)**: Detailed the analysis of performance regressions and the selection of safe alternatives.
- **Design Audit Report (P10.1)**: Documented inconsistencies and provided a prioritized list of refinements.
- **Final Verification Report (P11)**: Summarized the results of the pre‑deployment verification gate.

## Key Validation Artifacts

### Lighthouse Reports
Stored in `evidence/lighthouse/`:
- `p9.1-host-lighthouse.json` – Early verification after P9.1.
- `p10.2-base-head-8767-mobile.json` – Baseline before P10.2 refinements.
- `p10.2-final-8766-mobile.json` – After P10.2 design refinements.
- *(Note: The September Lighthouse run that yielded a Performance score of 84 is referenced in the final verification but is not included in the repository; see Known Limitations.)*

### Validation Checklists
- `p94_validation_checklist.txt` – For P9.4.2 (warm‑only Google Fonts preload).
- *(Other checklists may be referenced in commit messages or reports but are not included in the repository.)*

### Reports
- `DESIGN_AUDIT_REPORT.md` – P10.1 design audit.
- `FINAL_VERIFICATION_REPORT.md` – P11 final verification.
- `Performance_Restoration_Report.md` – P9.4 performance hardening and regression recovery.
- *(Other reports such as interim state updates may be found in `PROJECT_STATE.md`.)*

## How Validation Gates Were Applied

### Example: P6.1 Contact Form Implementation
1. The coding agent implemented the provider‑agnostic contact form per the prompt.
2. After implementation, the coding agent waited for verification.
3. Validation included:
   - Manual inspection of the form markup and behavior.
   - Keyboard navigation to ensure proper flow, visible focus rings, and no keyboard traps.
   - Screen‑reader testing to confirm that error messages were announced.
   - Testing of the honeypot field and consent checkbox.
   - Verification that the form could switch providers by changing `config/contact.config.js`.
   - A checklist (not retained in the repository) that covered all mandatory validation items from Section 8 of the blueprint.
4. Only after all validation passed did the human operator provide the next prompt (P6.2).

### Example: P9.4 Performance Hardening & Regression Recovery
1. After the P9.2 regression was recovered, the coding agent analyzed what optimizations were removed and which could be safely restored.
2. The coding agent implemented the warm‑only Google Fonts preload (the only admissible optimization under P9.4 constraints).
3. Validation required:
   - Lighthouse runs to measure performance change (baseline vs. post).
   - Full Section 8/§9 validation to ensure no regressions in accessibility, best practices, or SEO.
   - Manual checks (desktop/mobile navigation, footer, forms, Web3Forms integration, keyboard navigation, focus states, skip links, responsive layouts, image loading, typography, icons, JSON‑LD presence, stylesheet and Tailwind CDN script retention).
4. The validation checklist (`p94_validation_checklist.txt`) confirms that all items passed.
5. The Performance Restoration Report documents the disposition table and justifications for each optimization.

## Sources

- `WEBSITE_DEVELOPMENT_BLUEPRINT.md` (especially §6 Validation and §8 Mandatory Validation)
- `PROJECT_TRANSITION_KIT_1_.md`
- Milestone‑specific logs: `p94_implementation_log.txt`, `p94_validation_checklist.txt`
- Reports: `DESIGN_AUDIT_REPORT.md`, `FINAL_VERIFICATION_REPORT.md`, `Performance_Restoration_Report.md`
- Lighthouse JSON files in `evidence/lighthouse/`
