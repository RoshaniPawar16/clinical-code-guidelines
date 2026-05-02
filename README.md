# clinical-code-guidelines

AI coding assistants write code fast. In clinical and research environments, fast is not enough.

## The Problem

Ask an AI assistant to write a data pipeline and it will hardcode file paths, drop rows without logging why, return point estimates with no uncertainty, and skip audit logging unless told otherwise. It assumes the data is clean, the population is homogeneous, and the context is generic.

In a consumer app these are bad habits. In a clinical trial data management system or a tool used by clinicians to make patient decisions, they are protocol deviations and governance failures.

The tools have improved. The guidelines for using them in regulated environments have not kept up.

---

## What This Is

A `CLAUDE.md` file — and supporting examples — that gives AI coding assistants the context they are missing when working on clinical and research software.

It covers five failure modes specific to clinical and research code:

| Principle | The failure it prevents |
|-----------|------------------------|
| Reproducibility First | Analyses that cannot be re-run, results that cannot be verified |
| Audit Everything | Silent mutations, untraceable transformations, inspection failures |
| Surgical Data Handling | Hardcoded paths, undocumented cleaning, schema changes that break trials |
| Communicate Uncertainty | Point estimates presented as clinical findings |
| Think Before Coding | Wrong assumptions that invalidate results, not just functionality |

In January 2026, Anthropic launched [Claude for Healthcare](https://www.anthropic.com/news/healthcare-life-sciences) — guidelines for using it responsibly in research and clinical code are what this repo provides.

---

## Install

**Option A: Claude Code Plugin (recommended)**
```
/plugin marketplace add RoshaniPawar16/clinical-code-guidelines
```

Then install:
```
/plugin install clinical-code-guidelines
```

This installs the guidelines globally across all your projects.

**Option B: Per-project**

New project:
```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/RoshaniPawar16/clinical-code-guidelines/main/CLAUDE.md
```

Existing project:
```bash
echo "" >> CLAUDE.md
curl https://raw.githubusercontent.com/RoshaniPawar16/clinical-code-guidelines/main/CLAUDE.md >> CLAUDE.md
```

Works with Claude Code and any AI coding assistant that reads project context files. For Cursor, see `.cursor/rules/clinical-guidelines.mdc`.

---

## Background

These guidelines come from building:

- Clinical trial data management systems under NHS Scotland governance and MHRA inspection standards
- A clinical decision support tool used by fertility clinicians worldwide, built on registry data from 100,000+ treatment cycles
- Biomedical NLP research examining tokenizer bias across historical and domain-specific medical corpora

The failure modes documented here were observed directly.

---

## What is in this repo

Each file has a specific job.

* CLAUDE.md — the guidelines themselves. Drop this in any project and your AI assistant applies them automatically.
* EXAMPLES.md — before and after Python code showing what AI generates by default and what it should generate instead.
* CHECKLIST.md — a pre-commit checklist. Answer every question before pushing. If any answer is no, fix it first.
* CONTRIBUTING.md — how to add a new failure mode. The bar is simple: it must trace to something observed directly, not hypothesised.
* REFERENCES.md — four peer-reviewed papers that support the principles. Each one is cited in the relevant section of CLAUDE.md.
* CURSOR.md — setup instructions for using the guidelines in Cursor.
* .cursor/rules/clinical-guidelines.mdc — the Cursor rule file. Applied automatically to clinical and research file patterns.
* .claude-plugin/ — the Claude Code plugin manifest. Enables global install via the plugin marketplace.
* skills/clinical-guidelines/SKILL.md — the skill definition used by the Claude Code plugin.

---

## How to know it is working

These guidelines are working if:
- every data transformation is documented and traceable
- model outputs include uncertainty, not just predictions
- pipelines reproduce identically across environments
- audit logs exist before the first line of business logic
- questions about data provenance and regulatory context come before implementation

---

## Relation to Karpathy Guidelines

Karpathy's post on January 26, 2026 described what he was seeing:

> "The models make wrong assumptions on your behalf and just run along
> with them without checking. They don't manage their confusion, don't
> seek clarifications, don't surface inconsistencies, don't present
> tradeoffs, don't push back when they should."

> "LLMs are exceptionally good at looping until they meet specific
> goals... Don't tell it what to do, give it success criteria and
> watch it go."

Those observations are about general coding. This repo is about what
happens when the code touches a patient record, a trial database, or
a clinical decision tool.

[Andrej Karpathy's observations on LLM coding pitfalls](https://x.com/karpathy/status/2015883857489522876) — and the CLAUDE.md derived from them — address the general case well.

---

## Customisation

Add project-specific rules below the general guidelines:

```markdown
## Project-Specific Guidelines

- This system processes data under [governance framework]
- Patient identifiers are always stored as [format]
- All model outputs must be reviewed by [role] before clinical use
```

---

## License

MIT
