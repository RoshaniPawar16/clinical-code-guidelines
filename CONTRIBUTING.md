# CONTRIBUTING.md

Contributions are welcome. The bar is simple: every addition must trace to
a real failure mode observed in clinical or research software. Hypothetical
examples do not belong here.

---

## What belongs here

A new principle, example, or checklist item is worth adding if:
- It addresses a failure mode specific to clinical or research environments
- It cannot be covered by the existing five principles
- It comes from something you observed directly, not something you read about

## What does not belong here

- Generic coding advice that applies to any software project
- Failure modes already covered by the Karpathy guidelines
- Examples without clinical or research context

## How to contribute

1. Open an issue first. Describe the failure mode, where you observed it,
   and which file it belongs in.
2. One change per pull request. Do not bundle unrelated additions.
3. Follow the existing writing standard: plain language, no filler,
   real examples only.
4. If you are adding a code example to EXAMPLES.md, include both a before
   block and an after block. The before block must show what an AI assistant
   produces by default. Comments must explain clinical reasoning, not describe
   what the code does.

## Writing standard

No AI-generated phrasing. No padding. Every sentence must earn its place.
If removing it changes nothing, remove it.
