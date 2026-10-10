# Known Limitations

This document summarizes the known limitations and unresolved questions in the OAS Nigeria website project, as of the latest verification.

## 1. Performance Target Not Met
- **Description**: The Lighthouse Performance score remains at 84 (mobile emulation), below the documented target of ≥90.
- **Evidence**: 
  - `FINAL_VERIFICATION_REPORT.md`: Performance score 84 (Mobile emulation, Moto G Power).
  - `PROJECT_STATE.md`: "Lighthouse Performance: Verified at 84 (Mobile emulation). Performance Target: The documented target of ≥90 was not met. Status of Shortfall: This shortfall is accepted and documented as a known limitation."
- **Status**: Accepted as a known limitation. The project team accepted the shortfall after exhaustive optimization attempts within the project constraints (no build step, no framework removal).
- **Impact**: The site does not meet the performance goal set in the blueprint, but the shortfall is explicitly accepted.

## 2. Missing Production Domain
- **Description**: No production domain is currently assigned; all absolute URLs use the approved placeholder `https://www.oasnigeria.example/`.
- **Evidence**:
  - `README.md`: "Until deployment, all absolute URLs use the approved placeholder `https://www.oasnigeria.example/` to simplify future domain replacement."
  - `FINAL_VERIFICATION_REPORT.md`: Under "Deployment Note": "Replace the approved placeholder domain (`https://www.oasnigeria.example/`) with the production domain during deployment."
- **Status**: Pending purchase/assignment by the client.
- **Impact**: The website cannot be deployed to a production URL until the client provides a domain.

## 3. Colour‑Contrast Findings (September Lighthouse Run)
- **Description**: The September Lighthouse run that yielded a Performance score of 84 includes colour‑contrast findings that require accurate documentation.
- **Evidence**: 
  - `FINAL_VERIFICATION_REPORT.md`: Mentions a separate September Lighthouse run reporting Performance 84, Accessibility 96, Best Practices 100, SEO 100, and notes that the September report includes colour-contrast findings that require accurate documentation.
  - The discrepancy register (internal) notes this as an undocumented item.
- **Status**: The specific colour‑contrast failures are not detailed in the available evidence.
- **Impact**: While the Accessibility score is 96, indicating minimal impact, the exact nature of the findings is unknown. This is an unresolved question requiring further clarification.

## 4. Inconsistent Performance Metrics
- **Description**: Different documents report varying performance scores for what appears to be similar states (e.g., 66→75 vs. 77 vs. 84 vs. 55).
- **Evidence**:
  - `p94_implementation_log.txt`: Baseline 66.0 → Post 75.0 (+9.0 points) on mobile simulated Lighthouse.
  - Commit `ed5cfbc`: "Performance score 77 (below Appendix C ≥90 target, shortfall accepted)".
  - `FINAL_VERIFICATION_REPORT.md`: Performance score 84.
  - `evidence/lighthouse/p10.2-base-head-8767-mobile.json` and `p10.2-final-8766-mobile.json`: Both show Performance 55.
- **Status**: The discrepancy register preserves both claims with their respective sources and explains the discrepancy as likely due to different runs, conditions, or missing metadata (e.g., Lighthouse version, throttling details).
- **Impact**: Prevents a definitive performance trend analysis but highlights the importance of contextualizing measurements.

## 5. Web3Forms Access Key Exposure
- **Description**: The Web3Forms access key is present in `config/contact.config.js`. While documented as a client-side public key, its suitability for public release requires verification.
- **Evidence**:
  - `config/contact.config.js`: Contains accessKey: "<REDACTED>",
  - The file's comment states: "accessKey - Web3Forms access key. This is a CLIENT-SIDE credential and is public by design (the <script> ships it to the browser either way); submission spam is controlled by the _honey honeypot + by Web3Forms' own spam filter, NOT by secrecy of this key."
- **Status**: Requires human review to confirm if this key is indeed intended to be public. If it is a secret key, it must be redacted or excluded from the public archive.
- **Impact**: If the key is not intended for public use, including it could pose a security risk (e.g., spam or unauthorized use). If it is public, it is safe to include.

## 6. Company Logo and Proprietary Assets
- **Description**: The company logo (`assets/images/logo-full.svg` and `assets/images/logo-mark.svg`) and the company‑profile PDF (`assets/documents/OAS_Nigeria_Company_Profile.pdf`) are potentially proprietary business materials.
- **Evidence**: 
  - The README states: "All website copy must be coherent with the **OAS Nigeria Company Profile** (PDF, July 2026 edition), located at `assets/documents/OAS_Nigeria_Company_Profile.pdf`."
  - The logo is used in the header of every page.
- **Status**: Requires human review to determine if publication is authorized. If not, these assets should be excluded from the public archive.
- **Impact**: Publishing proprietary materials without authorization could violate confidentiality or intellectual property rights.

## 7. Missing Raw Lighthouse Reports for P9.4
- **Description**: The P9.4 implementation log references temporary Lighthouse reports (`/tmp/p94_baseline_index.json` and `/tmp/p94_post_index.json`) that are not available in the repository.
- **Evidence**: 
  - `p94_implementation_log.txt`: "Evidence: Lighthouse JSON reports stored at `/tmp/p94_baseline_index.json` (pre) and `/tmp/p94_post_index.json` (post)."
- **Status**: Confirmed missing from the repository. These reports were likely temporary and not committed.
- **Impact**: Prevents independent verification of the claimed performance improvement in P9.4.2.

## 8. Blueprint vs. Transition Kit Phase/Prompt Count Discrepancy
- **Description**: The `WEBSITE_DEVELOPMENT_BLUEPRINT.md` states 11 phases and 34 numbered prompts (P1.1 through P11.5). The `PROJECT_TRANSITION_KIT_1_.md` states the blueprint has 10 phases and 29 numbered prompts (P1.1–P10.2).
- **Evidence**: 
  - `WEBSITE_DEVELOPMENT_BLUEPRINT.md`, line 12: "This document is a **milestone-by-milestone build spec**. It is organised into **11 phases** and **34 numbered prompts** (P1.1 through P11.5)."
  - `PROJECT_TRANSITION_KIT_1_.md`: Companion document description: "`OAS_Nigeria_Website_Blueprint.md` (10 phases, 29 numbered prompts, P1.1–P10.2)"
- **Status**: Unresolved contradiction between the two key documents.
- **Impact**: Creates uncertainty about the exact number of milestones planned versus executed. The actual commit history shows prompts up to P10.2, with Phase 11 being a deployment certification phase.

## 9. Phase 11 Implementation Status
- **Description**: Phase 11 is referenced in the certification commit (`96b7ab6: chore: certify final state for Phase 11 deployment`) and the final verification, but no implementation prompts for Phase 11 are found in the blueprint or commit history.
- **Evidence**: 
  - Commit `96b7ab6`: certify final state for Phase 11 deployment
  - Commit `81bc7cc`: docs: reconcile final project state and record awaiting deployment
  - No commits with P11.* prefix found.
- **Status**: Phase 11 appears to be a verification/deployment phase rather than an implementation phase with numbered prompts.
- **Impact**: If Phase 11 was intended to include implementation work, that work was not executed. However, it may be correct that Phase 11 is solely a verification gate.

## Sources
- Internal audit documents: `A_inventory.txt`, `B_chronology.txt`, `C_milestones.txt`, `D_evidence_map.txt`, `E_discrepancy_register.txt`, `F_publication_risk.txt`, `G_classification.txt`
- Project documents: `README.md`, `WEBSITE_DEVELOPMENT_BLUEPRINT.md`, `PROJECT_TRANSITION_KIT_1_.md`, `PROJECT_STATE.md`
- Milestone‑specific logs and reports: `p94_implementation_log.txt`, `p94_validation_checklist.txt`, `DESIGN_AUDIT_REPORT.md`, `FINAL_VERIFICATION_REPORT.md`, `Performance_Restoration_Report.md`
- Lighthouse reports in `evidence/lighthouse/`
