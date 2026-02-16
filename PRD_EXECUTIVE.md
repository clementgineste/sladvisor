# PRD Executive Summary — SLAdvisor

Version: 1.0  
Date: 2026-02-16  
Source document: `PRD.md`  
Scope: Executive (condensed, decision-oriented)

## 1) Purpose

This document is the executive layer of the product plan.
It does **not** replace `PRD.md`; it summarizes it for fast alignment and decisions.

## 2) Problem and Opportunity

Teams often request high availability targets (e.g., 99.99%) without fully understanding the technical and operational implications.

SLAdvisor solves this by converting business constraints (criticality, budget, RTO/RPO, support) into:
- a concrete SLA tier recommendation,
- explicit architecture/operations implications,
- visible trade-offs between reliability, complexity, and cost.

## 3) Product Vision (12 months)

Evolve SLAdvisor from a useful calculator into a **decision-grade SLA advisory platform** while preserving the current strengths:
- static deployment,
- transparent logic,
- fast UX.

Target outcome:
- a guided wizard for fast onboarding,
- an interactive expert dashboard for deep analysis,
- robust exports/share flows usable in stakeholder meetings.

## 4) Strategic Objectives

1. Improve decision quality
- Reduce unrealistic SLA commitments by making constraints and implications explicit.

2. Increase trust in outputs
- Eliminate silent failures and strengthen quality, accessibility, and reliability.

3. Scale usability
- Support bilingual use cases and presentation-ready outputs.

4. Keep operating model lean
- Maintain low-complexity, static architecture and predictable maintenance cost.

## 5) Scope (What We Will Deliver)

## In Scope

- Dashboard v2: interactive SLA implications view (issue #10)
- Quality hardening: export, clipboard, URL validation, a11y improvements (issue #13 split)
- Test baseline and CI smoke checks (issue #7)
- Product decision log for strategic choices (issue #21)

## Out of Scope (this phase)

- Backend services
- Runtime monitoring platform features
- Full-blown architecture simulation engine across all cloud permutations

## 6) Roadmap Aligned to Milestones

## Milestone: `v2.0 - Major features`

- Dashboard MVP specification and interaction model (`#19`)
- Dashboard MVP implementation phase 1 (`#20`)
- Decision log and product arbitration (`#21`)
- i18n completion path remains tracked in existing backlog (`#8`)

## Milestone: `Infrastructure`

- PDF reliability and fail-safe state handling (`#15`)
- Clipboard reliability and truthful UX feedback (`#16`)
- URL parameter validation/normalization (`#17`)
- Accessibility semantic alignment (`#18`)
- Testing and CI baseline remains tracked in existing backlog (`#7`)

## Epic umbrella
- PRD execution epic (`#14`) aggregates the above work.

## 7) Key Decisions Required

These decisions are explicitly tracked in `#21`:

1. Default language policy
- Auto-detect browser
- English by default
- French by default

2. Dashboard v2 release depth
- Lean MVP
- Extended MVP (includes observability/error-budget depth)
- Full scope in first release

3. Test strategy level
- Unit only
- Unit + smoke CI
- Unit + smoke + key E2E

4. Dependency hardening strategy
- Minimal CSP
- SRI/pinning where possible
- Self-host third-party assets

## 8) Success Metrics

Product
- Share/export usage rates
- Session depth (number of meaningful parameter changes)
- Dashboard engagement after recommendation

Quality
- JS runtime error rate
- Copy/export failure handling correctness
- Coverage of critical scoring rules in tests

Adoption
- Returning users
- Session duration trend
- Ratio of feature requests vs blocking defects

## 9) Risks and Mitigation

Risk: dashboard complexity reduces clarity
- Mitigation: stage rollout via `#19` (spec) then `#20` (phase 1)

Risk: quality regressions during expansion
- Mitigation: execute infra hardening and test baseline in parallel (`#15-#18`, `#7`)

Risk: strategic drift and indecision
- Mitigation: formal decision log with rationale and owners (`#21`)

## 10) Governance and Traceability

This executive document is intentionally concise and traceable.

Traceability map:
- Epic: `#14`
- Quality hardening: `#15`, `#16`, `#17`, `#18`
- Dashboard v2: `#19`, `#20`
- Product decisions: `#21`
- Existing linked initiatives: `#10`, `#8`, `#7`, `#13`

---

## Important Clarification

This executive summary is **additive**, not destructive:
- `PRD.md` remains the full reference.
- `PRD_EXECUTIVE.md` is the fast decision layer.
- No feature is forced by this file alone; implementation follows issue-level decisions and prioritization.
