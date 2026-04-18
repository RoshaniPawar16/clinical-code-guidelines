# clinical-code-guidelines

> AI coding assistants are exceptional at writing code fast. In clinical and research environments, fast is not enough.

## The Problem

When you ask an AI assistant to write a data pipeline, it will:

- Hardcode file paths and database credentials
- Process data silently without documenting transformations
- Return point estimates with no uncertainty
- Skip audit logging unless explicitly asked
- Assume the data is clean, the population is homogeneous, and the context is generic

In a consumer app, these are bad habits. In a clinical trial data management system, a research tool used by clinicians, or a biomedical NLP pipeline — they are protocol deviations, governance failures, and patient safety risks.

The tools have improved dramatically. The guidelines for using them in regulated, high-stakes environments have not kept up.

---

## What This Is

A single `CLAUDE.md` file — and supporting examples — that gives AI coding assistants the context they are missing when working on clinical and research software.

Drop it in your project. Your AI assistant reads it automatically and applies it to everything it writes.

Covers four failure modes specific to clinical and research code:

| Principle | The Failure It Prevents |
|-----------|------------------------|
| **Reproducibility First** | Analyses that cannot be re-run, results that cannot be verified |
| **Audit Everything** | Silent mutations, untraceable transformations, inspection failures |
| **Surgical Data Handling** | Hardcoded paths, undocumented cleaning, schema changes that break trials |
| **Communicate Uncertainty** | Point estimates presented as clinical findings |

Plus the Karpathy baseline — think before coding, simplicity first, surgical changes, goal-driven execution — applied specifically to research contexts.

---

## Why Now

In January 2026, Anthropic launched [Claude for Healthcare](https://www.anthropic.com/news/healthcare-life-sciences) — purpose-built AI for providers, payers, and researchers, with HIPAA-ready infrastructure and connections to PubMed, ICD-10, and CMS databases.

The engine is here. The guidelines for building *on top of it* responsibly are not.

This repo is that missing layer.

---

## Install

**Per project:**
```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/yourusername/clinical-code-guidelines/main/CLAUDE.md
```

**Existing project (append):**
```bash
echo "" >> CLAUDE.md
curl https://raw.githubusercontent.com/yourusername/clinical-code-guidelines/main/CLAUDE.md >> CLAUDE.md
```

Works with Claude Code, Cursor, Copilot, and any AI coding assistant that reads project context files.

---

## What It Covers

These guidelines were derived from real experience building:

- Clinical trial data management systems under NHS Scotland governance and MHRA inspection standards
- Clinical decision support tools used by fertility clinicians worldwide, built on registry data from 100,000+ treatment cycles
- Biomedical NLP research examining tokenizer bias across historical and domain-specific medical corpora

The failure modes documented here were observed directly, not hypothesised.

---

## How To Know It Is Working

- AI-generated pipelines seed random by default
- Every data transformation is documented inline, not just in comments
- Model outputs include uncertainty ranges, not just predictions
- Audit logging appears before business logic, not after
- Clarifying questions about data provenance and regulatory context come before implementation

---

## Customisation

These guidelines are designed to be merged with project-specific instructions. Add your own section:

```markdown
## Project-Specific Guidelines

- This system processes data under [governance framework]
- Patient identifiers are always stored as [format]
- All model outputs must be reviewed by [role] before clinical use
```

---

## Relation to Karpathy Guidelines

[Andrej Karpathy's observations](https://x.com/karpathy/status/2015883857489522876) on LLM coding pitfalls — and the [CLAUDE.md](https://github.com/forrestchang/andrej-karpathy-skills) derived from them — address the general case well.

This repo addresses what happens when the general case is not enough. Clinical and research environments have failure modes that do not exist in product engineering. The cost of a wrong assumption here is not a bug report — it is a protocol deviation.

---

## License

MIT
