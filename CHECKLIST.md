# CHECKLIST.md

Pre-commit checklist for clinical and research code. Answer every question
before pushing. If the answer to any question is no, fix it first.

---

## Reproducibility

- [ ] Every random process is seeded explicitly
- [ ] Library versions are logged alongside outputs
- [ ] The pipeline produces identical results on a different machine
- [ ] Data sources are named explicitly, not assumed

## Audit Trail

- [ ] Every data mutation logs what changed, when, and why
- [ ] Audit logging is in place before any business logic runs
- [ ] An MHRA inspector could reconstruct the full record history from logs alone

## Data Handling

- [ ] No hardcoded file paths, patient identifiers, or credentials
- [ ] Data preprocessing is separate from modelling
- [ ] Every dropped or excluded record is documented with a reason
- [ ] Unexpected values in the data are surfaced, not silently handled

## Uncertainty

- [ ] Model outputs include confidence intervals, not point estimates alone
- [ ] Training population and known limitations are stated inline
- [ ] A clinician acting on this output without reading documentation would not make an unsafe decision

## Assumptions

- [ ] All assumptions about the data are stated explicitly in the code
- [ ] Ambiguous requirements were clarified before implementation, not after
- [ ] Every changed line traces to a specific requirement
