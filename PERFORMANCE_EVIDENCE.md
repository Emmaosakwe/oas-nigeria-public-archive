# Performance Evidence

This document presents the performance measurements collected during the OAS Nigeria website project, organized by run and environment. It includes source references, known limitations, and explanations of discrepancies.

## Measurement Philosophy

Every Lighthouse run is treated as an individual measurement. No raw reports are fabricated or reconstructed. Missing metadata is not inferred; missing raw reports are explicitly noted as unavailable.

## Available Lighthouse Reports

The following Lighthouse JSON reports are included in the `evidence/lighthouse/` directory:

### 1. P9.1 Host Lighthouse Report
- **File**: `evidence/lighthouse/p9.1-host-lighthouse.json`
- **Context**: Captured after P9.1 implementation (status: implementation complete but verification pending due to sandbox restrictions).
- **Available Data**: Includes Lighthouse version, requested URL, device/form‑factor, throttling method, and scores for Performance, Accessibility, Best Practices, and SEO.
- **Note**: The exact date and throttling details are embedded in the JSON.

### 2. P10.2 Base Report
- **File**: `evidence/lighthouse/p10.2-base-head-8767-mobile.json`
- **Context**: Captured before implementing P10.2 design refinements (baseline).
- **Available Data**: Same structure as above.

### 3. P10.2 Final Report
- **File**: `evidence/lighthouse/p10.2-final-8766-mobile.json`
- **Context**: Captured after implementing P10.2 design refinements (final).
- **Available Data**: Same structure as above.

## Reported Measurements

The following measurements have been referenced in project documents but are not all available as raw reports in the repository.

### P9.4 Implementation Log Measurements
- **Source**: `p94_implementation_log.txt`
- **Claim**: Baseline Performance 66.0 → Post‑implementation Performance 75.0 (+9.0 points) on mobile simulated Lighthouse.
- **Evidence Cited**: Lighthouse JSON reports stored at `/tmp/p94_baseline_index.json` (pre) and `/tmp/p94_post_index.json` (post).
- **Status**: The referenced raw reports are **not present** in the repository (they were temporary files in `/tmp`). Therefore, these measurements cannot be independently verified from the repository's evidence.

### P9.4 Certification Commit Measurement
- **Source**: Commit `ed5cfbc` (chore: complete P9.4 performance certification)
- **Claim**: Performance score 77 (below Appendix C ≥90 target, shortfall accepted).
- **Context**: This was the state after implementing the warm‑only Google Fonts preload in P9.4.
- **Status**: A specific Lighthouse report for this commit is not available in the repository. The claim is based on the commit message and related documentation.

### Final Verification Measurement
- **Source**: `FINAL_VERIFICATION_REPORT.md`
- **Claim**: Performance score 84 (Mobile emulation, Moto G Power).
- **Supporting Metrics**: FCP 2.9s, LCP 2.9s, TBT 20ms (good), CLS 0 (good).
- **Context**: This score matches the certified state from `Final_Performance_Certification.txt` (referenced but not in repository) and shows no regression from the prior certified state.
- **Status**: The raw Lighthouse report for this run is **not included** in the repository. However, the FINAL_VERIFICATION_REPORT.md is considered a reliable summary.

### P10.2 August Measurements
- **Source**: Filenames and context.
- **Claim**: Both the base and final P10.2 reports show Performance 55, Accessibility 96, Best Practices 100, SEO 100.
- **Status**: The raw reports are available in `evidence/lighthouse/` and can be independently verified.

### September Lighthouse Run (Referenced)
- **Source**: `FINAL_VERIFICATION_REPORT.md` and project discussions.
- **Claim**: A separate September Lighthouse run reported Performance 84, Accessibility 96, Best Practices 100, SEO 100.
- **Additional Detail**: The September report includes colour‑contrast findings that require accurate documentation.
- **Status**: The raw report for this run is **not included** in the repository. The FINAL_VERIFICATION_REPORT.md confirms that the Performance score of 84 matches the certified state and that no new performance regressions were detected.

## Known Limitations and Discrepancies

### 1. Inconsistent Performance Numbers
- **Description**: Different documents report varying performance scores for what appears to be the same or similar states.
- **Examples**:
  - P9.4 implementation log: 66.0 → 75.0 (+9.0)
  - P9.4 certification commit: 77
  - FINAL_VERIFICATION_REPORT.md: 84
  - P10.2 August base/final: 55
  - September run (referenced): 84
- **Explanation**: These numbers likely come from different runs at different times, under potentially different conditions (e.g., before and after specific optimizations, different throttling settings, or different Lighthouse versions). The project documentation does not always provide enough context (e.g., exact date, Lighthouse version, throttling details) to allow direct comparison.
- **Project Stance**: Where evidence conflicts, both claims are preserved with their respective sources, and the discrepancy is noted.

### 2. Missing Raw Reports for Key Claims
- **Description**: Some performance claims cite raw Lighthouse reports that are not available in the repository.
- **Examples**:
  - The P9.4 implementation log references `/tmp/p94_baseline_index.json` and `/tmp/p94_post_index.json`.
  - The final verification cites a Performance score of 84 but does not include the raw report.
  - The September run is referenced but not included.
- **Impact**: Prevents independent verification of the exact measurements and limits the ability to analyze metric breakdowns (e.g., FCP, LCP, TBT, CLS) for those specific claims.
- **Mitigation**: Where raw reports are missing, the project relies on verified summaries (e.g., FINAL_VERIFICATION_REPORT.md) and explicitly states when raw evidence is unavailable.

### 3. Colour‑Contrast Findings in September Run
- **Description**: The September Lighthouse run (Performance 84) includes colour‑contrast findings that require accurate documentation.
- **Status**: The specific colour‑contrast failures are not detailed in the available evidence. Since the Accessibility score remains 96, any failures are likely minor and do not significantly impact the overall score, but they should be disclosed for transparency.
- **Project Stance**: The discrepancy register notes this as an undocumented item requiring further clarification.

### 4. Performance Target Shortfall
- **Description**: The intended Performance target of ≥90 (or interim ≥85) was not achieved in the final state.
- **Evidence**: FINAL_VERIFICATION_REPORT.md: Performance score 84, which is below Appendix C target of ≥90 but explicitly accepted.
- **Project Stance**: This shortfall is accepted as a known limitation and documented in `PROJECT_STATE.md` and `Performance_Restoration_Report.md`.

## How to Interpret the Evidence

- **Prefer Raw Reports When Available**: The reports in `evidence/lighthouse/` are the most reliable source for performance measurements because they are original, unaltered outputs from Lighthouse.
- **Treat Summaries as Secondary**: Documents like `FINAL_VERIFICATION_REPORT.md` and `p94_implementation_log.txt` are valuable summaries but should be cross‑checked with raw reports where possible.
- **Note Missing Evidence**: When a raw report is cited but not available, treat the claim as having weaker evidential support and explicitly state the limitation.
- **Consider Context**: Performance scores can vary based on the tested URL, device emulation, network throttling, CPU throttling, and Lighthouse version. Always check the metadata embedded in the JSON reports.
- **Accept Known Limitations**: The project explicitly accepts the Performance target shortfall and documents it as a known limitation.

## Sources

- `evidence/lighthouse/` (raw JSON reports)
- `p94_implementation_log.txt`
- `FINAL_VERIFICATION_REPORT.md`
- `Performance_Restoration_Report.md`
- `PROJECT_STATE.md`
- Commit messages (e.g., `ed5cfbc`)
