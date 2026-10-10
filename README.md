# OAS Nigeria Website — Public Archive

This repository is a curated, evidence-backed public archive of the OAS Nigeria website project. It reconstructs the development history from the earliest verifiable milestone through the current state, using only information that is safe for public release or has been redacted/summarized appropriately.

## What is OAS Nigeria?

OAS Nigeria (legal name: O And Sons Nigeria Enterprises; parent brand: Eloson Group) is a Nigerian corporate services firm based at 10 Anwai Road, Asaba, Delta State. The firm specializes in business registration, corporate compliance certification, government-contract documentation support, general business documentation/office services, and web development.

## Website Scope

The website is a static, multi-page public marketing site — not a web application — consisting of six public pages:
- `index.html` (Homepage)
- `about.html` (About Us)
- `services.html` (Services)
- `industries.html` (Industries We Serve)
- `why-choose-us.html` (Why Choose OAS Nigeria)
- `contact.html` (Contact)

The site features:
- Data-driven navigation and footer (configured via `config/` JavaScript files)
- A provider-agnostic contact form (designed to swap backends without UI changes)
- Progressive enhancement (functional without JavaScript, with enhanced behavior when JS is available)
- Responsive design across four breakpoints (360px, 768px, 1024px, 1440px)
- Accessibility features targeting WCAG 2.1 AA (skip link, visible focus rings, semantic landmarks, labelled form fields, ARIA error handling)
- Search engine optimization (unique titles, meta descriptions, Open Graph/Twitter Card tags, JSON-LD `ProfessionalService` schema, `robots.txt`, `sitemap.xml`)
- Performance optimizations (limited by the no-build-step constraint; see Performance Evidence)

## Project Status

As of the latest verification (commit `96b7ab6`), the website's development work is recorded as complete to the extent supported by the evidence, while **production deployment remains pending** awaiting acquisition of a production domain.

All functional and visual requirements are implemented and verified. Known limitations include:
- Lighthouse Performance score of 84 (mobile emulation), below the documented target of ≥90 but explicitly accepted as a known limitation.
- No production domain currently assigned.

## My Role and Responsibilities

Based on the available evidence, my role involved:
- Directing AI-assisted implementation workflow (coding agent) to execute milestones strictly in order.
- Verifying each milestone against requirements before proceeding.
- Managing version control with meaningful Git checkpoints and preserving recovery work (e.g., regression recoveries).
- Organizing validation through browser audits, Lighthouse metrics, and accessibility/performance checklists.
- Documenting decisions, incidents, and recovery actions in project reports.
- Ensuring architectural decisions anticipated future modules (client portal, AI assistant, compliance dashboard, etc.) without restructuring.

Claims about my role are supported only by evidence such as commit messages, milestone documents, and verification reports.

## AI-Assisted Implementation and Workflow Management

The project followed a specification-driven milestone approach:
- The `WEBSITE_DEVELOPMENT_BLUEPRINT.md` served as the source of truth, organized into phases and numbered prompts (P1.1 through P10.2).
- Each prompt instructed the coding agent to complete exactly one milestone and then stop.
- Workflow rules included:
  - Execute prompts strictly in order; do not start the next until the current is verified complete.
  - Never modify the work of a previous point unless explicitly allowed.
  - Never introduce new dependencies, frameworks, or tools unless explicitly instructed.
  - Treat the OAS Nigeria Company Profile PDF (July 2026 edition) as the canonical source of copy.
  - Accessibility, performance, and responsiveness are non-negotiable gates per milestone.
- After each prompt, I waited for the next instruction (whether from a human or automated checkpoint).
- Git history preserves meaningful checkpoints and recovery work (e.g., commits `2706aa4` recovering from a P9.2 regression).
- Validation was performed through:
  - Lighthouse runs (performance, accessibility, best practices, SEO)
  - Accessibility hardening checklists (P6.2)
  - SEO implementation (P8.1)
  - Performance certification (P9.4)
  - Design audit (P10.1)
  - Final verification (P11)

## How Milestones, Git Checkpoints, Tests, and Reviews Were Handled

- **Milestones:** Each numbered prompt (e.g., P5.1: Header component) corresponds to a specific functional or technical milestone.
- **Git Checkpoints:** Meaningful commits were made at the completion of each milestone (visible in the Git history with prefixes like P1.1, P2.3, etc.). Recovery work is also documented (e.g., `2706aa4`: Recover OAS Nigeria website from P9.2 regression).
- **Tests:** Validation included:
  - Automated: Lighthouse metrics (scores and audits)
  - Manual: Browser audits at multiple breakpoints, keyboard navigation, screen‑reader walks, form testing
  - Checklists: Phase‑specific validation checklists (e.g., p94_validation_checklist.txt)
- **Reviews:** 
  - Design audit (P10.1) conducted against a high‑craft bar.
  - Final verification (P11) confirmed readiness for deployment.
  - Recovery work was documented and verified (e.g., P9.4 Performance Restoration Report).

## What Was Achieved, Measured, Remains Incomplete, and Why

**Achieved:**
- A static six‑page website with data‑driven navigation, provider‑agnostic contact form, and progressive enhancement.
- Implementation of accessibility primitives (skip link, focus ring, reduced motion, sr‑only).
- SEO foundation (unique titles, meta descriptions, Open Graph/Twitter Card tags, JSON‑LD schema, `robots.txt`, `sitemap.xml`).
- Responsive design across required breakpoints.
- Performance optimizations within the no‑build‑step constraint (e.g., warm‑only Google Fonts preload in P9.4.2).

**Measured:**
- Lighthouse scores (per run, stored as JSON reports in `evidence/lighthouse/`):
  - Performance: varies by run (e.g., 55 in August P10.2 base/final, 84 in September run)
  - Accessibility: consistently ≥96
  - Best Practices: 100
  - SEO: 100
- Specific audits (e.g., colour‑contrast findings in the September run).
- File sizes, asset optimization, and implementation checklists.

**Remains Incomplete:**
- **Production deployment:** Pending the acquisition of a production domain by the client.
- **Performance target:** The documented goal of Lighthouse Performance ≥90 (or interim ≥85) was not met; the final verified score is 84.
- **Future modules:** The architecture is designed to accommodate client portal, AI assistant, compliance renewal dashboard, appointment booking, knowledge base/blog, user accounts, and online payments, but these are not implemented in this public archive.

**Why:**
- Deployment is blocked by external factors (domain procurement) rather than technical readiness.
- The performance shortfall is accepted as a known limitation after exhaustive optimization attempts within the project constraints (no build step, no framework removal).
- Future modules are out of scope for the initial public marketing website; the site was deliberately architected to allow later extension without restructuring.

## How to Navigate the Evidence and Project Chronology

- **README.md** – This file: project overview and status.
- **PROJECT_TIMELINE.md** – Chronological summary of milestones, commits, and key events.
- **MILESTONE_REGISTER.md** – Detailed register of every identifiable milestone, sub‑phase, implementation, validation, recovery, and certification.
- **ARCHITECTURE_AND_DECISIONS.md** – Architectural overview, design system, and key decisions.
- **AI_WORKFLOW.md** – Description of the AI‑assisted implementation workflow and version control practices.
- **TESTING_AND_VALIDATION.md** – Overview of testing methods, validation checklists, and reports.
- **PERFORMANCE_EVIDENCE.md** – Performance measurements by run, with source references and known limitations.
- **INCIDENTS_AND_RECOVERY.md** – Documented incidents, regressions, recovery steps, and outstanding limitations.
- **EVIDENCE_INDEX.md** – Index of all evidence files included in the `evidence/` directory, with source references and SHA‑256 checksums where appropriate.
- **KNOWN_LIMITATIONS.md** – Summary of known limitations and unresolved questions.
- **MANIFEST.md** – Inventory of files in the public archive with their publication‑risk classification.
- **evidence/** – Reviewed, approved, safe‑to‑publish evidence only (e.g., Lighthouse reports, non‑sensitive logs).
- **data/** – Machine‑readable milestone or evidence indexes (if useful).

## Notes on Evidence

- This archive is **documentation‑first**. It does not copy the website source tree, production assets, the company‑profile PDF, or the original Git object database by default.
- Where evidence is included, it has been reviewed for publication safety. Sensitive information (such as the Web3Forms access key) has been redacted, summarized, or excluded pending human review.
- Original commit hashes are retained exactly in the evidence index; they are not implied to be public documentation commits.
- Where a public document paraphrases an internal blueprint or report, the source file and relevant section are identified when practical.
- The archive does not pretend to be a mirror of the original repository; it is a curated documentary record.

## Deployment Status

**Production deployment is awaiting the production domain.** The website is not live or deployed unless evidence establishes that it is. No claim is made that all accessibility requirements pass unless the evidence supports that condition.

---
*This archive was generated from the source repository at `/home/xavier/oas-nigeria-website` and is intended for public review by technical recruiters, engineering teams, and reviewers evaluating AI‑assisted development and workflow‑building skills.*
