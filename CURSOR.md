# CURSOR.md

The file at `.cursor/rules/clinical-guidelines.mdc` is `CLAUDE.md` in Cursor's
rule format. The content is identical — only the wrapper differs.

## Using it in a new project

```bash
mkdir -p .cursor/rules
curl -o .cursor/rules/clinical-guidelines.mdc \
  https://raw.githubusercontent.com/RoshaniPawar16/clinical-code-guidelines/main/.cursor/rules/clinical-guidelines.mdc
```

If your project already has a `CLAUDE.md`, you can use both — they do not conflict.

## When Cursor applies it

`alwaysApply` is set to false. Cursor activates the rule when the open file
matches one of these globs: `**/*.py`, `**/*.sql`, `**/*.ipynb`, or files
prefixed with `data_`, `pipeline`, `clinical`, `trial`, or `research`.
It does not apply to every file automatically.
