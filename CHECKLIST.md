# CHECKLIST.md

Pre-commit checklist for clinical and research code. Answer every question
before pushing. If the answer to any question is no, fix it first.

---

## Reproducibility

- [ ] Every random process is seeded explicitly
- [ ] Library versions are logged alongside outputs
- [ ] The pipeline produces identical results on a different machine
- [ ] Data sources are named explicitly, not assumed

A 2024 review of 511 clinical ML papers found only 21% shared their code and only 44% reported performance variance — Ciobanu-Caraus et al., Acta Neurochirurgica, 2024.

## Audit Trail

- [ ] Every data mutation logs what changed, when, and why
- [ ] Audit logging is in place before any business logic runs
- [ ] An MHRA inspector could reconstruct the full record history from logs alone

Meeting FDA 21 CFR Part 11 requires audit capture at the database level — application-layer logging does not cover direct database access — Jiang & Cao, 2011.

## Data Handling

- [ ] No hardcoded file paths, patient identifiers, or credentials
- [ ] Data preprocessing is separate from modelling
- [ ] Every dropped or excluded record is documented with a reason
- [ ] Unexpected values in the data are surfaced, not silently handled

## Uncertainty

- [ ] Model outputs include confidence intervals, not point estimates alone
- [ ] Training population and known limitations are stated inline
- [ ] A clinician acting on this output without reading documentation would not make an unsafe decision

Four of the most widely cited medical ML models published since 2016 had no abstention mechanism and returned predictions regardless of confidence — Kompa et al., npj Digital Medicine, 2021.

## Assumptions

- [ ] All assumptions about the data are stated explicitly in the code
- [ ] Ambiguous requirements were clarified before implementation, not after
- [ ] Every changed line traces to a specific requirement
