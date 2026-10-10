# Incidents and Recovery

This document details known incidents, regressions, recovery steps, and outstanding limitations in the OAS Nigeria website project.

## Incident: P9.2 Regression

### Description
Commit `91699aa` (P9.2: Performance remediation – render‑blocking CSS preload, async Tailwind, FOUC prevention, stylesheet preload) introduced a combination of optimizations that caused a blank‑page regression when combined with the existing inline Tailwind config.

### Root Cause
- **Async Tailwind Play CDN**: The `<script src="https://cdn.tailwindcss.com" async>` was moved to *after* the inline `tailwind.config={…}` block.
- **Inline config script runs synchronously**: Before the `async` CDN executes → `window.tailwind` is undefined → `ReferenceError` → Tailwind never compiles utility classes → unstyled/blank page.
- **Combined with FOUC hiding** (`<style id="fouc-style">body{visibility:hidden}</style>` + deferred `…remove()`): Even when Tailwind fails, the remover still runs and reveals a broken page.
- **Result**: A blank page is surfaced because Tailwind never compiles, and the FOUC hider eventually reveals the broken state.

### Recovery
- **Commit `2706aa4`**: Recover OAS Nigeria website from P9.2 regression.
  - This commit reverted the optimizations from `91699aa` that caused the regression.
  - After recovery, the website returned to a working state (though without the P9.2 performance optimizations).
- **Documentation**: The recovery was noted in the commit message and in the project timeline.

### Lesson Learned
- The async Tailwind Play CDN optimization is **fundamentally unsafe** for this architecture because it creates a write‑before‑define race condition with the inline config that injects brand tokens.
- No safe async/defer variant exists while preserving the brand‑token inline config under the no‑build‑step invariant.
- The optimization was therefore **rejected permanently** for this phase.

## Incident: Contact Doctype Corruption

### Description
Commit `a536bb1` documents a recovery from a contact.html doctype corruption.

### Details
- The exact nature of the corruption is not specified in the commit message, but it was corrected to restore a valid HTML5 doctype.
- The commit also recorded the state after recovery.

### Recovery
- The doctype was restored to `<!DOCTYPE html>`.
- No further details are available in the repository.

## Performance Regression and Restoration (P9.4)

### Description
After the P9.2 regression was recovered, the website was missing the performance optimizations from P9.2. Phase 9.4 aimed to restore performance safely.

### Analysis
The `Performance_Restoration_Report.md` analyzed each optimization removed by the P9.2 regression and determined whether to restore, replace, or reject it based on safety and measurability.

### Disposition Table (Summary)
| # | Removed Optimization | Why It Failed | Unsafe by Nature or Unsafe Implementation? | Safer Alternative Exists? | Disposition |
|---|----------------------|---------------|--------------------------------------------|---------------------------|-------------|
| 1 | Async Tailwind Play CDN | Inline config runs before async CDN executes → `ReferenceError` → blank page | **Unsafe by nature** (write‑before‑define race) | No | **REJECT** |
| 2 | FOUC hiding (`visibility:hidden` + remover) | Content hidden pending a script; if script delayed/blocked, body stays hidden; even when Tailwind fails, remoter runs and reveals broken page | **Unsafe by nature** (and explicitly prohibited by §7) | N/A | **REJECT** |
| 3 | Local stylesheet async (onload‑swap) | Styling only applies when `onload` fires → load failure leaves page unstyled‑but‑visible; reintroduces JS‑event dependency for styling | **Unsafe implementation** (Condition B breaks FOUC) | Yes (warm‑alongside‑sync) but redundant with existing synchronous fetch → fails Condition A | **REJECT** |
| 4 | Google Fonts async (onload‑swap) | Font CSS applies only on `onload` → failed/slow fetch leaves text in fallback faces → visible font‑flash and §9.2‑adjacent typography risk; reintroduces JS dependency for styling | **Unsafe implementation** (Condition B breaks FOUC of brand typography) | Yes — warm‑alongside‑sync variant (failsafe) | **REPLACE** (warm‑only variant only; gated by P9.4.2 measured validation) |

### Recovery
- The only admissible optimization under Phase 9.4 constraints was the **warm‑only Google Fonts preload** (an additional `<link rel="preload" as="style" href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Montserrat:wght@500;600;700&display=swap">` kept alongside the existing synchronous stylesheet).
- This was implemented in P9.4.2 and validated to yield a measured performance improvement of +9.0 points on Lighthouse Performance (baseline 66.0 → post 75.0) without introducing regressions in functionality, accessibility, or visual appearance.
- The optimization was gated by P9.4.2 measured validation: if it could not evidence a Condition‑A improvement or triggered any §8 regression, the Execution Loop would auto‑revert it and it would become **REJECT**.

### Outcome
- The warm‑only Google Fonts preload was successfully implemented and verified.
- However, the final performance score remained below the target of ≥90, as documented in the final verification.

## Outstanding Limitations

### 1. Performance Target Not Met
- **Description**: The Lighthouse Performance score remains at 84 (mobile emulation), below the documented target of ≥90.
- **Status**: Accepted as a known limitation. See `PROJECT_STATE.md` and `Performance_Restoration_Report.md`.
- **Impact**: The site does not meet the performance goal set in the blueprint, but the shortfall is explicitly accepted after exhaustive optimization attempts within the project constraints.

### 2. Missing Production Domain
- **Description**: No production domain is currently assigned; all absolute URLs use the approved placeholder `https://www.oasnigeria.example/`.
- **Status**: Pending purchase/assignment by the client.
- **Impact**: The website cannot be deployed to a production URL until the client provides a domain.

### 3. Colour‑Contrast Findings (September Lighthouse Run)
- **Description**: The September Lighthouse run that yielded a Performance score of 84 includes colour‑contrast findings that require accurate documentation.
- **Status**: The specific colour‑contrast failures are not detailed in the available evidence.
- **Impact**: While the Accessibility score is 96, indicating minimal impact, the exact nature of the findings is unknown.
- **Project Stance**: This is noted as an unresolved question in the Known Limitations.

### 4. Inconsistent Performance Metrics
- **Description**: Different documents report varying performance scores for what appears to be similar states (e.g., 66→75 vs. 77 vs. 84 vs. 55).
- **Status**: The discrepancy register preserves both claims with their respective sources and explains the discrepancy as likely due to different runs, conditions, or missing metadata.
- **Impact**: Prevents a definitive performance trend analysis but highlights the importance of contextualizing measurements.

## Sources

- Commit messages: `91699aa`, `2706aa4`, `a536bb1`, `ed5cfbc`
- Reports: `Performance_Restoration_Report.md`, `FINAL_VERIFICATION_REPORT.md`
- Discrepancy register (see `E_discrepancy_register.txt` in the audit)
