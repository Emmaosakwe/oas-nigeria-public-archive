# Manifest

This manifest lists all files included in the public archive, along with their publication‑risk classification and notes on their inclusion.

## Root Directory

| File | Classification | Notes |
|------|----------------|-------|
| README.md | Safe to publish in full | This file: overview of the archive, project status, and navigation. |
| PROJECT_TIMELINE.md | Safe to publish in full | Chronological summary of milestones, commits, and key events. |
| MILESTONE_REGISTER.md | Safe to publish in full | Detailed register of every identifiable milestone, sub‑phase, implementation, validation, recovery, and certification. |
| ARCHITECTURE_AND_DECISIONS.md | Safe to publish in full | Architectural overview, design system, and key decisions. |
| AI_WORKFLOW.md | Safe to publish in full | Description of the AI‑assisted implementation workflow and version control practices. |
| TESTING_AND_VALIDATION.md | Safe to publish in full | Overview of testing methods, validation checklists, and reports. |
| PERFORMANCE_EVIDENCE.md | Safe to publish in full | Performance measurements by run, with source references and known limitations. |
| INCIDENTS_AND_RECOVERY.md | Safe to publish in full | Documented incidents, regressions, recovery steps, and outstanding limitations. |
| KNOWN_LIMITATIONS.md | Safe to publish in full | Summary of known limitations and unresolved questions. |
| MANIFEST.md | Safe to publish in full | This file: inventory of included files with classification. |
| EVIDENCE_INDEX.md | Safe to publish in full | Index of all evidence files in the `evidence/` directory, with source references and SHA‑256 checksums. |

## evidence/ Directory

| File | Classification | Notes |
|------|----------------|-------|
| evidence/lighthouse/p9.1-host-lighthouse.json | Safe to publish in full | Original Lighthouse run after P9.1 implementation. |
| evidence/lighthouse/p10.2-base-head-8767-mobile.json | Safe to publish in full | Baseline Lighthouse run before P10.2 design refinements. |
| evidence/lighthouse/p10.2-final-8766-mobile.json | Safe to publish in full | Lighthouse run after P10.2 design refinements. |
| evidence/p94_implementation_log.txt | Safe to publish in full | Documents the implementation of P9.4.2 (warm‑only Google Fonts preload). References temporary Lighthouse reports (`/tmp/p94_baseline_index.json` and `/tmp/p94_post_index.json`) that are not available in the public archive. |
| evidence/p94_validation_checklist.txt | Safe to publish in full | Validation checklist for P9.4.2; confirms that all Section 8/§9 validation items passed. |
| evidence/DESIGN_AUDIT_REPORT.md | Safe to publish in full | P10.1 design audit report; identifies inconsistencies and provides a prioritized list of refinements. |
| evidence/FINAL_VERIFICATION_REPORT.md | Safe to publish in full | P11 final verification report; confirms readiness for Phase 11 deployment. |
| evidence/Performance_Restoration_Report.md | Safe to publish in full | P9.4 performance hardening and regression recovery report; analyzes removed optimizations and selects safe alternatives. |

## Excluded Items

The following items were considered for inclusion but were excluded due to publication‑risk, privacy, or proprietary concerns. See the internal publication‑risk register (`F_publication_risk.txt` in the audit) for details.

- `config/contact.config.js`: Contains the Web3Forms access key; requires human review to confirm if it is intended for public use.
- `assets/documents/OAS_Nigeria_Company_Profile.pdf`: Potentially proprietary business material; requires human review for publication authorization.
- `assets/images/logo-full.svg` and `assets/images/logo-mark.svg`: Company logo; requires human review to determine if publication is authorized.
- Other assets (e.g., favicons, OG image) are considered safe to publish but are not included to keep the archive documentation‑first. They can be made available upon request if needed.
- The website source tree (HTML, CSS, JavaScript) is not copied by default, as the archive is documentation‑first. The implementation details are described in the documentation and evidence above.

## How to Use This Manifest

- Review the classification of each file to understand its publication‑risk status.
- The SHA‑256 checksums in `EVIDENCE_INDEX.md` can be used to verify the integrity of the evidence files.
- For excluded items, consult the internal audit (available upon request) for the detailed publication‑risk register and classification rationale.

