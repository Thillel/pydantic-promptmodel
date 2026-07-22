# Prompt corpus

This directory contains the design corpus for `pydantic-promptmodel`. Read
[`docs/project-plan.md`](../docs/project-plan.md) before advancing an example to a
new state.

The source-collection milestone is complete. Files under `sources/markdown/` and
`sources/xml/` are reviewed source prompts, but they are not canonical prompt pairs
until their counterpart translations have also been reviewed and frozen.

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

## Template materialization contract

The placeholder syntax marks typed content slots; it is not raw text-replacement
syntax. Materialization must parse the source template first and then replace the
corresponding content node or attribute value. Inserted values remain data and must
not be reparsed as Markdown, XML, or prompt instructions.

- Markdown inline slots accept single-line values and are escaped for their inline
  context. Multiline or markup-bearing values belong in explicit block containers.
- Values inserted into Markdown code blocks become literal code-block content. The
  serializer must choose a fence character and length that cannot be closed by the
  inserted value.
- XML character-data and attribute slots are escaped for their exact XML context.
  Placeholder values never inject raw XML fragments.
- An XML element marked `optional="true"` is omitted when its value is absent. A
  present self-closing or empty element therefore means an explicitly supplied empty
  value, not an absent value.
- Sibling order is always preserved. An `order` attribute may declare that order
  semantically significant; child `position` or `priority` attributes must not repeat
  the same authority unless they are stable identifiers with separately defined
  behavior.
- Character data in XML sources is exact. Ordinary prose leaves should remain on one
  physical line when no line break is intended. `xml:space="preserve"` marks payloads
  whose whitespace is deliberately significant.
- CDATA versus escaped character data is meaningful source syntax in this corpus and
  must survive source-format round trips even though a generic XML infoset may erase
  that lexical distinction.

## Status vocabulary

- `source-draft`
- `source-reviewed`
- `translated-draft`
- `pair-frozen`
- `model-draft`
- `model-approved`

All 20 current sources are `source-reviewed`. No source has been translated yet.
