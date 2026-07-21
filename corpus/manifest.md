# Corpus manifest

## Target composition

The initial target is 20 source prompts: 16 Markdown-first and 4 XML-first. The first
working batch contains 10 prompts in the same 80/20 ratio. A second batch will be
selected after reviewing the first so it can fill genuine coverage gaps rather than
add superficial variety.

## First working batch

| ID | Source purpose | Distinct source structures | Status | Review focus |
| --- | --- | --- | --- | --- |
| `md-001` | Concise summarization | H1 siblings, paragraph bodies, H2 subsection, unordered list, placeholder | `source-draft` | Is the output format part of the task or its own section? |
| `md-002` | Policy-bound customer reply | H1–H3 hierarchy, emphasis, nested unordered lists, ordered procedure | `source-draft` | Check whether the deep hierarchy improves instruction scope. |
| `md-003` | Few-shot classification | Prose before first heading, repeated H2 sections, block quotes, strong labels | `source-draft` | Confirm repeated examples are easy to scan and unambiguous. |
| `md-004` | Structured data extraction | H1/H2 sections, fenced JSON, inline code, ordered rules | `source-draft` | Separate JSON illustration from literal output requirements. |
| `md-005` | Answer evaluation | Markdown table, ordered list, nested unordered list, bold terms | `source-draft` | Ensure table is genuinely the clearest rubric form. |
| `md-006` | Code-review workflow | Task list, fenced code, inline code, thematic break, H2/H3 sections | `source-draft` | Decide whether task boxes carry semantics or merely decoration. |
| `md-007` | Multi-source research brief | Links, named inputs, nested lists, heading siblings, explicit citations | `source-draft` | Make source priority and conflict handling precise. |
| `md-008` | Dialogue rewriting | Quoted conversation, ordered constraints, nested list item with multiple paragraphs | `source-draft` | Verify speaker boundaries survive copy/paste and translation. |
| `xml-001` | Support-ticket routing | Semantic attributes, nested containers, optional context, repeated labels | `source-draft` | Determine whether attributes contain information Markdown must expose. |
| `xml-002` | Ordered tool workflow | Ordered repeated steps, mixed content, empty optional element, escaped operators | `source-draft` | Check that mixed content is necessary and not accidental. |

## Coverage represented in the first batch

| Structure or purpose | Examples |
| --- | --- |
| Paragraph directly beneath a heading | `md-001`, `md-002`, `md-004`, `md-006`, `md-007` |
| Nested heading levels | `md-001`, `md-002`, `md-003`, `md-004`, `md-006`, `md-007`, `md-008` |
| Unordered list | `md-001`, `md-002`, `md-004`, `md-007` |
| Ordered list | `md-002`, `md-004`, `md-005`, `md-008` |
| Nested list | `md-002`, `md-005`, `md-007`, `md-008` |
| Task list | `md-006` |
| Block quote | `md-003`, `md-008` |
| Fenced code | `md-004`, `md-006` |
| Inline code | `md-004`, `md-006` |
| Emphasis or strong emphasis | `md-002`, `md-003`, `md-005`, `md-007` |
| Table | `md-005` |
| Thematic break | `md-006` |
| Link | `md-007` |
| Prose before first heading | `md-003` |
| Multiple paragraphs in one list item | `md-008` |
| Repeated semantic sections | `md-003` |
| XML attributes | `xml-001` |
| Repeated XML elements | `xml-001`, `xml-002` |
| Optional or empty XML element | `xml-001`, `xml-002` |
| XML mixed content | `xml-002` |
| Summarization | `md-001` |
| Classification | `md-003`, `xml-001` |
| Extraction | `md-004` |
| Evaluation | `md-005` |
| Transformation | `md-008` |
| Planning or tool use | `md-006`, `xml-002` |
| Policy constraints | `md-002` |
| Few-shot learning | `md-003` |
| Multi-input synthesis | `md-007` |

## Known gaps for the second batch

The second batch should be chosen after source review, with special attention to:

- an intentionally heading-free Markdown prompt;
- a document title followed by introductory content before its first section;
- a definition-like term/value structure;
- literal Markdown delimiters and escaping;
- a prompt with repeated sections containing different block sequences;
- a generation prompt where output order is crucial;
- XML CDATA or heavily escaped literal content;
- XML where element order is optional versus explicitly significant;
- at least one source whose natural structure challenges assumptions learned from the
  first batch.

## Review record

No source has been reviewed or frozen yet. Review should begin with each prompt's
clarity and source-format structure, without considering how easy it will be to map
to XML or Pydantic.
