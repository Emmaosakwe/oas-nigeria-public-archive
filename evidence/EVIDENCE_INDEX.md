# Evidence Index

This index lists all evidence files included in the `evidence/` directory, with their source references and SHA‑256 checksums where appropriate.

## Lighthouse Reports

- `evidence/lighthouse/p9.1-host-lighthouse.json`
  - Source: Original Lighthouse run from the OAS Nigeria website project.
  - SHA‑256: `6be919f6c05eede63bfd6a17d31524ee5a4c1e783387ca9874f430978e67d512`
  - Notes: Contains Performance, Accessibility, Best Practices, and SEO scores for a specific run.

- `evidence/lighthouse/p10.2-base-head-8767-mobile.json`
  - Source: Original Lighthouse run from the OAS Nigeria website project.
  - SHA‑256: `6881ac62d32d04d0290459c183bef1f88da7f3b3c701fb96ffee55cb0ec4a5d4`
  - Notes: Contains Performance, Accessibility, Best Practices, and SEO scores for a specific run.

- `evidence/lighthouse/p10.2-final-8766-mobile.json`
  - Source: Original Lighthouse run from the OAS Nigeria website project.
  - SHA‑256: `5951a87146bc273076abf1187932f99c1b3df53e4ffdb3f3ea117e53e417e437`
  - Notes: Contains Performance, Accessibility, Best Practices, and SEO scores for a specific run.

## Validation and Log Files

- `p94_implementation_log.txt`
  - Source: Copied from the OAS Nigeria website project repository.
  - SHA‑256: `6c2f0b5fc42d357b58687caabfdc95cf6e4cb5cf76bb6470e08ef90541255204`
  - Notes: Documents the implementation of P9.4.2 (warm‑only Google Fonts preload). References temporary Lighthouse reports (`/tmp/p94_baseline_index.json` and `/tmp/p94_post_index.json`) that are not available in the public archive.

- `p94_validation_checklist.txt`
  - Source: Copied from the OAS Nigeria website project repository.
  - SHA‑256: `bcda7f412e540dbb04317b770aecedf8fd9702a0011f82867692199f8519f693`
  - Notes: Validation checklist for P9.4.2; confirms that all Section 8/§9 validation items passed.

- `DESIGN_AUDIT_REPORT.md`
  - Source: Copied from the OAS Nigeria website project repository.
  - SHA‑256: `00f442af240011a6092fa8add7487a4fd1ab3d5a3bcbc4189189557d2a8f1b77`
  - Notes: P10.1 design audit report; identifies inconsistencies and provides a prioritized list of refinements.

- `FINAL_VERIFICATION_REPORT.md`
  - Source: Copied from the OAS Nigeria website project repository.
  - SHA‑256: `289efb422ce43c4261b2b9b3d26de9e1cae53e0e94daeb92ac1cce73e3324d41`
  - Notes: P11 final verification report; confirms readiness for Phase 11 deployment.

- `Performance_Restoration_Report.md`
  - Source: Copied from the OAS Nigeria website project repository.
  - SHA‑256: `f8fc90af9a74949b01f2a8b59ea2911d98d01f8326f1068713342befa0f0ab10`
  - Notes: P9.4 performance hardening and regression recovery report; analyzes removed optimizations and selects safe alternatives.
