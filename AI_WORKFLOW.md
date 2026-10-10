# AI‑Assisted Workflow

This document describes how AI‑assisted implementation and workflow management were organized in the OAS Nigeria website project.

## Workflow Overview

The project followed a **specification‑driven milestone** approach, where a human operator (the user) directed a coding agent to implement each milestone in strict sequence. The coding agent acted as an pair‑programming partner, executing exactly what was specified in each prompt and then stopping for verification.

## Key Elements of the Workflow

### 1. Source of Truth
- **Primary**: `WEBSITE_DEVELOPMENT_BLUEPRINT.md` — a milestone‑by‑milestone build spec organized into 10 phases and 29 numbered prompts (P1.1 through P10.2).
- **Secondary**: `PROJECT_TRANSITION_KIT_1_.md` — a kit that wraps the blueprint in process discipline for safe multi‑agent handoff.
- **Canonical Copy Source**: The `OAS_Nigeria_Company_Profile.pdf` (July 2026 edition) provided all text copy, ensuring fidelity to the firm's official materials.

### 2. Milestone Execution Rules
The coding agent was bound by the following rules (as stated in the blueprint and transition kit):
1. **Execute prompts strictly in order.** Do not start Prompt P*X.Y* until P*X.(Y‑1)* is verified complete.
2. **After each prompt, stop and wait for the next instruction.** Do not start the next milestone on your own.
3. **Never modify the work of a previous point** unless the current point explicitly says you may.
4. **Never introduce new dependencies, frameworks, or tools** unless the current point explicitly says you may.
5. **Treat the OAS Nigeria Company Profile PDF as the canonical source of copy.** When copy is adapted for the web, it must remain substantively identical in meaning.
6. **If something is unclear, ask before coding.** Do not invent features, services, statistics, or claims that are not in the profile.
7. **Accessibility, performance, and responsiveness are non‑negotiable.** No milestone is complete if Lighthouse scores drop, keyboard navigation breaks, or mobile layout fails.

### 3. Verification Checkpoints
After each milestone, the coding agent waited for verification. Verification could include:
- **Automated checks**: Lighthouse runs (performance, accessibility, best practices, SEO).
- **Manual checks**: Browser audits at multiple breakpoints, keyboard navigation, screen‑reader walks, form testing.
- **Checklists**: Phase‑specific validation checklists (e.g., `p94_validation_checklist.txt` for P9.4.2).
- **Reports**: Detailed reports such as the Design Audit (P10.1) or Performance Restoration Report (P9.4).

Only after verification was complete would the human operator provide the next prompt.

### 4. Version Control Practices
- **Meaningful Commit Messages**: Each milestone completion was committed with a clear message prefix (e.g., `P5.1: feat: complete P5.1 header component ...`).
- **Recovery Work Preserved**: When regressions occurred (e.g., the P9.2 regression that blanked the page), recovery commits were made (e.g., `2706aa4: feat: recover OAS Nigeria website from P9.2 regression`) and documented.
- **Documentation Commits**: Updates to `PROJECT_STATE.md` and other documents were committed separately to keep the implementation history clean.
- **No Squashing or Rewriting**: The commit history preserves the exact sequence of milestones, recoveries, and decisions, allowing a transparent audit trail.

### 5. Handling of Regressions and Incidents
- When a milestone introduced a regression (e.g., P9.2's async Tailwind caused a blank page), the coding agent stopped and waited for instructions.
- Recovery work was treated as a separate milestone‑like effort, with its own commits and documentation.
- Incidents were logged in reports such as the `Performance_Restoration_Report.md`, which analyzed what went wrong, why, and how to safely restore performance.

### 6. AI‑Agent Responsibilities
The coding agent was responsible for:
- Translating milestone prompts into working code.
- Adhering strictly to the rules above.
- Stopping after each milestone and awaiting verification.
- Not inventing features, statistics, or claims outside the source material.
- Preserving accessibility, performance, and responsiveness as hard gates.
- Writing clear commit messages that reflect the work done.

### 7. Human Operator Responsibilities
The human operator was responsible for:
- Providing the next prompt only after verifying the current milestone.
- Reviewing verification evidence (Lighthouse reports, checklists, manual tests).
- Directing work when regressions occurred.
- Ensuring the project stayed within the defined scope and constraints.
- Keeping the canonical copy source (the Company Profile PDF) respected.

## Outcomes of the Workflow

- **Traceability**: Every line of code can be tied to a specific milestone prompt or recovery effort.
- **Accountability**: The Git history shows exactly what was implemented, when, and what verification passed.
- **Learning from Mistakes**: Regressions were documented and used to inform safer alternatives (e.g., the warm‑only Google Fonts preload replaced the unsafe onload‑swap pattern).
- **Consistency**: The config‑driven approach ensured that navigation and footer remained consistent across all pages.
- **Extensibility**: The architecture is ready for future modules without restructuring, as anticipated in the workflow's future‑proofing rule.

## Sources

- `WEBSITE_DEVELOPMENT_BLUEPRINT.md`
- `PROJECT_TRANSITION_KIT_1_.md`
- Commit messages (especially those with `feat:` and `docs:` prefixes)
- Milestone‑specific logs and reports (e.g., `p94_implementation_log.txt`, `Performance_Restoration_Report.md`)
