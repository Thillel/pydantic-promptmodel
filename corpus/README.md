# Prompt corpus

This directory contains the design corpus for `pydantic-promptmodel`. Read
[`docs/project-plan.md`](../docs/project-plan.md) before advancing an example to a
new state.

The current milestone is source collection. Files under `sources/markdown/` and
`sources/xml/` are intentionally **drafts**, not canonical prompt pairs. They may be
rewritten or removed during source review.

Do not add format translations to `pairs/` until the corresponding source is marked
`source-reviewed` in `manifest.md`. Do not add Pydantic representations to `models/`
until all selected pairs have been frozen.

## File conventions

- `md-NNN-*` identifies a Markdown-first example.
- `xml-NNN-*` identifies an XML-first example.
- Placeholders use `{{ snake_case }}` in source text unless an example is explicitly
  testing another convention.
- Source files contain only the prompt. Review notes and coverage information belong
  in `manifest.md` or a later pair decision log.
- A source file's formatting is meaningful. Formatters must not be run blindly.

## Status vocabulary

- `source-draft`
- `source-reviewed`
- `translated-draft`
- `pair-frozen`
- `model-draft`
- `model-approved`

All current sources begin at `source-draft`.
