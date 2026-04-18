# CLAUDE.md

Guidelines for AI coding assistants working on clinical and research software. Merge with project-specific instructions as needed.

Tradeoff: these guidelines bias toward caution over speed. In regulated clinical environments, a wrong assumption is not a bug — it is a protocol deviation.

---

## 1. Reproducibility First

Seed every random process. No exceptions.

- Never rely on implicit data ordering. Sort deterministically or document why you cannot.
- Log library versions, data sources, and run parameters alongside outputs.
- Document transformations with reasoning, not just code.
- If a result cannot be reproduced from scratch on a different machine six months from now, it is not done.

In a tokenizer bias study across GPT-2, GPT-3.5, GPT-4, and Claude variants, a 15-40% degradation in compression efficiency on historical corpora was only detectable because every run was seeded and logged identically. Without that, the finding is noise.

---

## 2. Audit Everything

Log transaction identity across the full lifecycle of every operation, not just errors.

- Never mutate data silently. Every transformation must trace to a timestamp, a function, and a reason.
- Build audit trails before business logic, not after.
- If you are unsure whether a change needs logging, it does.

The test: could an MHRA inspector or Research Ethics Committee reconstruct exactly what happened to every record, and when?

A messaging service handling participant communications across concurrent live trials is only inspectable if transaction identity is logged start to finish — from send through to webhook response. That is not optional. It is what makes the trial defensible.

---

## 3. Surgical Data Handling

Never hardcode file paths, patient identifiers, trial IDs, or credentials.

- Separate data preprocessing from modelling. They are different concerns.
- Do not clean data without documenting every decision. In clinical data, what looks like noise may be a signal.
- Do not change schemas without understanding downstream dependencies. In multi-centre trials, one schema change can break data integrity across sites.
- If something unexpected appears in the data, stop and surface it. Do not work around it silently.

---

## 4. Communicate Uncertainty

A model output is not a clinical finding.

- Always surface confidence intervals, not point estimates alone.
- Document what the model was trained on and where it may not generalise.
- State limitations inline, not in a separate README.
- If a clinician could act on this output without reading the documentation and make an unsafe decision, the uncertainty is not communicated well enough.

An IVF success rate calculator used globally by fertility clinicians — built on registry data from over 100,000 treatment cycles — showed that the difference between a point estimate and a confidence interval changed how clinicians counselled patients. Technically correct is not the same as clinically useful.

---

## 5. Think Before Coding

Do not assume. Do not hide confusion. Surface tradeoffs.

- State assumptions explicitly. In research code, wrong assumptions invalidate results, not just functionality.
- If multiple interpretations exist, present them. Do not pick silently.
- If something is unclear about the data, the population, or the regulatory context, stop and ask.
- Push back when a simpler approach exists.

The cost of a wrong assumption in a consumer app is a bug report. The cost of a wrong assumption in a clinical trial data management system is a protocol deviation.

---

These guidelines are working if:
- every data transformation is documented and traceable
- model outputs include uncertainty, not just predictions
- pipelines reproduce identically across environments
- audit logs exist before the first line of business logic
- questions about data provenance and regulatory context come before implementation

---

Inspired by Andrej Karpathy's observations on LLM coding pitfalls and Anthropic's Claude for Healthcare. Merge with project-specific instructions as needed.
