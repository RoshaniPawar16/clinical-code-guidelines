# clinical-code-guidelines

AI coding assistants write code fast. In clinical and research environments, fast is not enough.

## The Problem

Ask an AI assistant to write a data pipeline and it will hardcode file paths, drop rows without logging why, return point estimates with no uncertainty, and skip audit logging unless told otherwise. It assumes the data is clean, the population is homogeneous, and the context is generic.

In a consumer app these are bad habits. In a clinical trial data management system or a tool used by clinicians to make patient decisions, they are protocol deviations and governance failures.

The tools have improved. The guidelines for using them in regulated environments have not kept up.

---

## What This Is

A `CLAUDE.md` file — and supporting examples — that gives AI coding assistants the context they are missing when working on clinical and research software.

Drop it in your project. Your AI assistant reads it automatically and applies it to everything it writes.

It covers five failure modes specific to clinical and research code:

| Principle | The failure it prevents |
|-----------|------------------------|
| Reproducibility First | Analyses that cannot be re-run, results that cannot be verified |
| Audit Everything | Silent mutations, untraceable transformations, inspection failures |
| Surgical Data Handling | Hardcoded paths, undocumented cleaning, schema changes that break trials |
| Communicate Uncertainty | Point estimates presented as clinical findings |
| Think Before Coding | Wrong assumptions that invalidate results, not just functionality |

---

## Why Now

In January 2026, Anthropic launched [Claude for Healthcare](https://www.anthropic.com/news/healthcare-life-sciences) — purpose-built AI for providers, payers, and researchers, with HIPAA-ready infrastructure and connections to PubMed, ICD-10, and CMS databases.

The guidelines for building on top of it responsibly in research and clinical contexts are not there. This repo is that gap.

---

## Install

New project:
```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/RoshaniPawar16/clinical-code-guidelines/main/CLAUDE.md
```

Existing project:
```bash
echo "" >> CLAUDE.md
curl https://raw.githubusercontent.com/RoshaniPawar16/clinical-code-guidelines/main/CLAUDE.md >> CLAUDE.md
```

Works with Claude Code, Cursor, Copilot, and any AI coding assistant that reads project context files.

---

## Background

These guidelines come from building:

- Clinical trial data management systems under NHS Scotland governance and MHRA inspection standards
- A clinical decision support tool used by fertility clinicians worldwide, built on registry data from 100,000+ treatment cycles
- Biomedical NLP research examining tokenizer bias across historical and domain-specific medical corpora

The failure modes documented here were observed directly.

---

## Relation to Karpathy Guidelines

[Andrej Karpathy's observations on LLM coding pitfalls](https://github.com/forrestchang/andrej-karpathy-skills) — and the CLAUDE.md derived from them — address the general case well.

This repo addresses what happens when the general case is not enough. Clinical and research environments have failure modes that do not exist in product engineering. The cost of a wrong assumption is not a bug report — it is a protocol deviation.

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
