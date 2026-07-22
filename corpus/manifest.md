# Corpus manifest

## Target composition

The initial target of 20 source prompts is complete: 16 are Markdown-first and 4 are
XML-first, giving the intended 80/20 split. The first working batch contains 10
prompts. The second batch adds 10 prompts selected to fill structural gaps and extend
the agentic-programming coverage without adding superficial variants.

## First working batch

| ID | Source purpose | Distinct source structures | Status | Review focus |
| --- | --- | --- | --- | --- |
| `md-001` | Concise summarization | H1 siblings, paragraph bodies, H2 subsection, unordered list, placeholder | `source-reviewed` | Independent structural review passed. |
| `md-002` | Policy-bound customer reply | H1–H3 hierarchy, emphasis, nested unordered lists, ordered procedure | `source-reviewed` | Independent structural review passed. |
| `md-003` | Few-shot classification | Prose before first heading, repeated H2 sections, block quotes, strong labels | `source-reviewed` | Independent structural review passed. |
| `md-004` | Structured data extraction | H1/H2 sections, fenced JSON, inline code, ordered rules | `source-reviewed` | Independent structural review passed. |
| `md-005` | Answer evaluation | Markdown table, ordered list, nested unordered lists, bold terms | `source-reviewed` | Independent structural review passed. |
| `md-006` | Code-review workflow | Semantically unchecked task list, fenced patch, inline code, H2/H3 sections | `source-reviewed` | Structural re-review passed after removing the redundant thematic break. |
| `md-007` | Multi-source research brief | Exact generated heading contract, symmetric source records, links, nested lists, fenced source data | `source-reviewed` | Structural re-review passed with valid links and opaque data boundaries. |
| `md-008` | Dialogue rewriting | Quoted conversation, ordered constraints, nested list item with multiple paragraphs | `source-reviewed` | Independent structural review passed. |
| `xml-001` | Support-ticket routing | Document-order precedence, nested containers, optional populated slots, repeated labels | `source-reviewed` | Structural re-review passed with one precedence authority. |
| `xml-002` | Branched tool workflow | Strict outer workflow, explicit success/escalation branch, repeated steps, mixed tool references | `source-reviewed` | Structural re-review passed with exclusive branch scope. |

## Second working batch: selected sources

| ID | Source purpose | Distinct source structures | Status | Review focus |
| --- | --- | --- | --- | --- |
| `md-009` | Autonomous action gate | System/context/role H1 siblings, repeated scenarios, uniform H3 criteria sections, section-body notes | `source-reviewed` | Independent structural review passed. |
| `md-010` | Agent delegation decision | Document title, H2 system/context/role, repeated H3 examples, uniform H4 criteria sections, fenced output template | `source-reviewed` | Independent structural review passed. |
| `md-011` | Incident-response agent | Long H1/H2 agent prompt, repeated six-phase flow, inline strong labels, action lists, exit criteria, block-quoted updates | `source-reviewed` | Independent structural review passed. |
| `md-012` | Repository change agent | Document title, H2/H3 instructions, ordered workflow, avoid/prefer Python fences, completion contract | `source-reviewed` | Independent structural review passed. |
| `md-013` | Production migration policy | One H1 followed by four-level nested ordered rules, continuation paragraphs, no subsections | `source-reviewed` | Independent structural review passed. |
| `md-014` | Release triage | Markdown shell, one uninterrupted raw XML example block, ordered precedence, fenced input and XML output shape | `source-reviewed` | Structural re-review passed with one well-formed raw XML block. |
| `md-015` | Production access review | H1–H3 hierarchy, repeated scenario sections, definition-style list, exact ordered output contract | `source-reviewed` | Structural re-review passed after exposing the real hierarchy. |
| `md-016` | Canonical release-note generation | Exact ordered and conditional output grammar, missing-data branches, nested fences, repeatable template slots | `source-reviewed` | Third structural gate passed after making every output branch total. |
| `xml-003` | Grounded three-document synthesis | Repeated attributed documents and examples, ordered conflict policy, typed conditional output presence | `source-reviewed` | Structural re-review passed with an explicit three-document invariant. |
| `xml-004` | Publication readiness | Strict workflow versus order-independent checks, first-match rule precedence, preserved CDATA Markdown | `source-reviewed` | Structural re-review passed with effective precedence and CDATA boundaries. |

## Coverage represented

| Structure or purpose | Examples |
| --- | --- |
| Paragraph directly beneath a heading | `md-001`, `md-002`, `md-004`, `md-006`, `md-007`, `md-009`, `md-010`, `md-011`, `md-012`, `md-013`, `md-014`, `md-015`, `md-016` |
| Nested heading levels | `md-001`, `md-002`, `md-003`, `md-004`, `md-006`, `md-007`, `md-009`, `md-010`, `md-011`, `md-012`, `md-014`, `md-015`, `md-016` |
| Unordered list | `md-001`, `md-002`, `md-004`, `md-007`, `md-009`, `md-010`, `md-011`, `md-012`, `md-014`, `md-015`, `md-016` |
| Ordered list | `md-002`, `md-004`, `md-005`, `md-008`, `md-012`, `md-013`, `md-014`, `md-015`, `md-016` |
| Nested list | `md-002`, `md-005`, `md-007`, `md-008`, `md-013`, `md-016` |
| Task list | `md-006` |
| Block quote | `md-003`, `md-008`, `md-011` |
| Fenced code | `md-004`, `md-006`, `md-007`, `md-010`, `md-012`, `md-014`, `md-016` |
| Inline code | `md-004`, `md-006`, `md-009`, `md-010`, `md-011`, `md-012`, `md-013`, `md-014`, `md-015`, `md-016` |
| Emphasis or strong emphasis | `md-002`, `md-003`, `md-005`, `md-007`, `md-011`, `md-015` |
| Table | `md-005` |
| Link | `md-007` |
| Prose before first heading | `md-003` |
| Multiple paragraphs in one list item | `md-008` |
| Repeated semantic sections | `md-003`, `md-009`, `md-010`, `md-011`, `md-014`, `md-015` |
| XML attributes | `xml-001`, `xml-002`, `xml-003`, `xml-004` |
| Repeated XML elements | `xml-001`, `xml-002`, `xml-003`, `xml-004` |
| Optional XML element | `xml-001`, `xml-002`, `xml-004` |
| Empty XML element with attribute-only meaning | `xml-001`, `xml-002` |
| XML mixed content | `xml-002` |
| XML CDATA containing Markdown | `xml-004` |
| XML strict versus order-independent containers | `xml-004` |
| Repeated attributed XML documents | `xml-003` |
| Explicit XML workflow branch | `xml-002` |
| Summarization | `md-001` |
| Classification | `md-003`, `xml-001` |
| Extraction | `md-004` |
| Evaluation | `md-005` |
| Transformation | `md-008` |
| Planning or tool use | `md-006`, `xml-002` |
| Policy constraints | `md-002` |
| Few-shot learning | `md-003` |
| Multi-input synthesis | `md-007` |
| Agentic decision policy | `md-009`, `md-010` |
| Repeated scenario schema | `md-009`, `md-010` |
| H4 headings | `md-010` |
| Document title with introductory body | `md-010` |
| Long conversation or execution flow | `md-011` |
| Inline labels within repeated sections | `md-011` |
| Repeated exit criteria and examples | `md-011` |
| Repository-wide agent instructions | `md-012` |
| Avoid/prefer code examples | `md-012` |
| Deep ordered-list hierarchy | `md-013` |
| Raw XML embedded in Markdown | `md-014` |
| Definition-like term/value list | `md-015` |
| Literal Markdown and escaping | `md-016` |
| Heading-like text inside code fences | `md-016` |
| Canonical generated-block order | `md-016` |
| Long-context grounded synthesis | `xml-003` |
| XML evaluation workflow | `xml-004` |

## Second-batch gap resolution

The second batch addresses the retained structural gaps as follows:

- definition-like term/value structure: `md-015`;
- literal Markdown delimiters and escaping: `md-016`;
- repeated sections containing heterogeneous block sequences: `md-011`;
- generation where output order is crucial: `md-016`;
- XML CDATA and escaped literal content: `xml-004`;
- semantically optional versus strict element order: `xml-004`;
- a structure that challenges heading-centric assumptions: `md-013`.

Two syntax categories are deliberately unresolved after structural review:

- The long access policy was not a natural heading-free document, so `md-015` now
  uses real headings. A future heading-free example must be short and genuinely flat.
- The thematic break in `md-006` was only decorative and was removed. The corpus will
  leave thematic-break coverage empty until a prompt has a real semantic use for one.

The two selected agentic prompts deliberately express similar repeated decision
structures at different absolute heading depths. `md-009` has top-level semantic
sections; `md-010` has a document title whose body contains all later sections. This
is useful corpus variation, not permission for a future model to ignore heading
levels.

Online source research that informed the second-batch selections is recorded in
[`research/online-source-candidates.md`](research/online-source-candidates.md). The
strongest new patterns are an OpenAI-style repeated conversation flow, a GitHub-style
repository instruction document, a Google-style deeply nested ordered policy, and a
hybrid Markdown/XML developer message. Those patterns informed the original drafts
`md-011` through `md-015`; source wording and scenarios remain original.

## Review record

All 20 source prompts passed independent structure-only review on 2026-07-22 and are
marked `source-reviewed`. Eleven Markdown sources passed their first gate. Five
Markdown and four XML sources were revised and freshly re-reviewed; `md-016` required
one additional correction and third gate. No translation or Pydantic design was used
to justify a source structure.
