# Source corpus audit

## Audit scope

This audit covers the complete 20-source working corpus. It evaluates whether the
files are useful, structurally distinct source prompts. It does not approve
translations or suggest model shapes.

Initial audit date: 2026-07-21

Corpus-completion audit date: 2026-07-22

## Mechanical checks

- The corpus contains 16 Markdown-first and 4 XML-first prompts: an exact 80/20
  split.
- All four XML documents are well-formed according to `xmllint`.
- Every source identifier and filename is unique.
- Placeholders consistently use `{{ snake_case }}`.
- Markdown heading levels observed in the corpus are H1 through H4.
- The claimed lists, block quotes, table, code fences, task list, link, pre-heading
  prose, and raw XML block are present in the source files. Thematic-break and long
  heading-free coverage were deliberately removed after structural review found their
  uses artificial.

## Structural reading

### `md-001`

The `Requirements` H2 is intentionally a child of the `Task` H1. The task's opening
paragraph is section-body content, not a peer subsection. `Role`, `Source article`,
and `Output` are H1 siblings of `Task`.

Review question: should the role mention the audience, or should audience become a
separate top-level concern? Decide based on prompt clarity, not future model fields.

### `md-002`

This is the batch's deepest Markdown hierarchy. `Damaged products` and `Unwanted
products` are H3 children of `Refund eligibility`; `Privacy` is its H2 sibling.
Nested lists express conditions within rules, while the ordered list expresses a
procedure.

Review question: confirm that “within 30 calendar days” has an unambiguous inclusive
boundary and that this domain detail does not distract from structural review.

### `md-003`

The opening instruction intentionally precedes the first heading. Examples are
repeated H2 sections rather than list items because each example combines a quoted
input with a separately labeled answer.

Review question: decide whether the inline bold labels communicate roles more clearly
than separate subheadings would. The current form is intentionally flatter.

### `md-004`

`Fields`, `Output shape`, and `Extraction rules` are H2 children of `Task`. The JSON
fence is an example of the required shape; the prose explicitly prevents the model
from emitting the fence. `Incident report` ends the task section as a new H1.

Review question: confirm that a schema-like illustration is preferable to a prose or
formal JSON Schema definition for this prompt.

### `md-005`

The rubric is a table because readers must compare criteria, weights, and standards
across rows. The ordered method contains nested unordered requirements. `Material`
contains three H2 input sections.

Review question: the required response specifies content order but not a concrete
serialization. Decide whether that flexibility is intentional or under-specified.

### `md-006`

The task list is a real review checklist, not a decorative bullet style. `Findings`
and `Review summary` are H3 children of `Reporting`. `Proposed patch` is the next H1
and therefore closes the instruction hierarchy without an extra separator.

First-review resolution: the thematic break was decorative and parsed as the final
block of `Review summary`, so it was removed. Fresh re-review confirmed the unchecked
task-list semantics and fenced patch boundary remain natural.

### `md-007`

`Required structure` uses H2 children to explain three exact H2 sections in a
generated H1-titled brief. `Sources` uses symmetric H2 records, each with one URL and
one fenced source body. The citation example uses a complete literal link, while the
three runtime URL values remain code-styled data slots in their source records.

First-review resolution: the generated heading contract, three URL slots, and opaque
source boundaries are now explicit. Fresh re-review confirmed that the source fences
are genuine input containers rather than output examples.

### `md-008`

The third ordered constraint intentionally contains two body paragraphs and a nested
unordered list. The conversation is one block quote with blank quoted lines between
turns. This makes the conversation a single quoted artifact rather than three
unrelated quotations.

Review question: decide whether the requested plain `Speaker: message` output should
instead preserve the input's Markdown quotation style. Either is valid, but the
transformation should be intentional.

### `xml-001`

Queue attributes encode identity only; queue document order is the sole precedence
authority. Repeated elements encode labels, and self-closing field elements describe
required output fields. Optional ticket inputs contain placeholders and are omitted
at materialization when absent under the corpus contract.

First-review resolution: duplicate priority attributes and the ambiguous empty input
were removed. Fresh re-review confirmed that `min-items="0"` makes the required labels
field compatible with the general queue's empty label collection.

### `xml-002`

The workflow has one initial step followed by an explicit success/escalation branch.
Branch-local steps prevent the answer path from running after escalation. Mixed
content places tool references inside instruction sentences; optional inputs use
populated slots that are omitted when absent.

First-review resolution: numbered order, unnecessary comparison escaping, and the
ambiguous empty input were removed. Fresh re-review confirmed that the explicit
branch is natural and keeps the answer and escalation paths exclusive.

## First-batch result

The first batch was sufficiently diverse to guide selection of the second batch. At
this point in the audit, every example still had a substantive review question to be
resolved on the source's own terms.

The second batch should now target the gaps in `manifest.md`, after which all 20
sources can undergo a consistent source-quality review.

## Second-batch audit

### `md-009`

`System`, `Context`, `Role`, `Scenarios`, `Current case`, and `Output` are H1 siblings.
Each scenario is an H2 child of `Scenarios`; each `Return YES when`, `Return NO when`,
`Guidelines`, and `Notes` section is an H3 child of its scenario. The explanatory
sentence immediately after a scenario heading is body content belonging directly to
that scenario, not another named field.

Review question: confirm that the three scenarios are mutually understandable when a
candidate action combines inspection and mutation. The output asks for the decisive
criterion, so the prompt must make precedence clear enough to choose one.

### `md-010`

This prompt has a document title at H1, with introductory body text before the first
H2. `System`, `Context`, `Role`, `Scenarios`, `Proposed delegation`, and `Output` are
therefore H2 siblings inside the titled document. Each example is H3, and its four
repeated decision sections are H4. The absolute heading depth is intentionally one
level deeper than the related structure in `md-009`.

Review question: confirm that “guide” is an appropriate document-level title rather
than an unnecessary wrapper. If it remains, a later representation must preserve it
and the resulting heading-depth shift.

These prompts are not frozen. Their repeated subsection names are useful evidence for
future design, but they do not establish reserved field names or renderer behavior.

### `md-011`

This source adapts the repeated conversation-flow pattern found in OpenAI's official
Realtime prompting guide to an original incident-response domain. `Role and
objective`, `Operating context`, `Communication style`, `Tools`, `Incident flow`,
`Escalation`, and `Session context` are H1 siblings. The six phases are H2 children of
`Incident flow`.

Inside every phase, `Goal`, `Actions`, `Exit when`, and `Example update` are strong
inline labels at the beginnings of ordinary blocks. They are deliberately **not** H3
subsections. `Actions` introduces a list, while `Goal` and `Exit when` remain paragraph
content and `Example update` introduces a block quote. This repeated but heterogeneous
block sequence is exactly the kind of distinction a future representation must retain.

Review question: determine whether allowing a return to an earlier phase is precise
enough when the production-change sequence remains gated. Also check that every exit
condition can be observed using the stated tools and session context.

### `md-012`

The H1 title owns an introductory paragraph, and H2 sections divide the repository
instructions into operational concerns. H3 subsections refine those concerns; the
`Avoid` and `Prefer` labels beneath resource handling and error translation are
ordinary paragraphs immediately followed by fenced Python examples, not additional
headings. YAML front matter is deliberately absent because this source describes the
rendered standalone prompt rather than repository file-selection metadata.

Review question: confirm that the repository conventions form one coherent prompt
rather than an overly broad collection of policies. Also confirm that excluding front
matter leaves no needed instruction out of the rendered text.

### `md-013`

Only the document title is a Markdown heading. The operational hierarchy lives in a
deep ordered list whose items contain subordinate ordered lists. The continuation
paragraph beneath rule 1.3 belongs to that list item even though it is separated by a
blank line. The four labeled input placeholders at the end are ordinary paragraphs,
not list items or sections.

Review question: verify every indentation level and ensure that cross-references can
identify a rule stably without inventing implicit headings. Also confirm that the
input labels are clearest as trailing paragraphs rather than another list level.

### `md-014`

Markdown headings define the outer prompt. `<examples>` is one uninterrupted raw block
whose source text forms one well-formed XML tree; a second XML parse can recover its
nested records. The release record is now fenced Markdown data, not live XML. The XML
under `Output` is also fenced, but it is a literal output template rather than input.

First-review resolution: blank lines that split the raw block were removed, and the
unsafe custom-tag input wrapper became a code-block slot. Fresh re-review confirmed
that the one remaining raw XML block is preserved byte-for-byte and earns its
second-pass parsing rule.

### `md-015`

`System` is the sole H1. Global concerns and scenarios are H2 children; `Yes when`,
`No when`, `Guidelines`, and `Notes` are repeated H3 children of each scenario. The
definition-like terms remain list items with strong inline labels. The output's three
required lines are represented by an ordered instruction list.

First-review resolution: the long document's visual labels hid real scope and would
have required reserved-word recovery, so they became headings. Fresh re-review
confirmed that the hierarchy exposes real structure without artificial subsections.

### `md-016`

The source itself uses H1 and H2 headings. The heading markers inside the
four-backtick `markdown` fence belong to the literal canonical template and are not
headings in the source document. Its nested triple-backtick `text` fence is likewise
template data, made possible by the longer outer fence. Missing name, date, purpose,
highlights, breaking-change status, migration status, verification, and references
now have explicit output branches. List and command patterns declare repeatable
cardinality rather than example cardinality.

First-review resolution: materialization now selects data fences from their contents,
and the prompt distinguishes absent evidence from established negative facts.
The second gate found two remaining incomplete branches; after those were corrected,
a third independent gate confirmed that every fallback agrees with the seven-block
grammar and the canonical template does not imply fixed list or command counts.

### `xml-003`

The XML hierarchy explicitly separates role, objective, instruction groups,
examples, documents, the question, and ordered output sections. Repeated `<rule>`,
`<factor>`, `<example>`, and `<document>` elements are sequences rather than uniquely
named fields. The packet now explicitly contains exactly three dated documents, and
every example uses three sample documents. Factor priority remains semantic ranking;
conflict/output order relies on document order plus one container declaration.
Conditional output uses a consistently typed `presence` attribute.

First-review resolution: fixed cardinality is now a deliberate task invariant rather
dates were removed. Fresh re-review confirmed that the exactly-three invariant is
consistent across role, instructions, examples, and runtime inputs.

### `xml-004`

The outer workflow is strictly ordered, while the quality checks explicitly allow any
evaluation order. Decision rules use cumulative predicates and first-match precedence,
so listed order can affect the result. The report template alone uses whitespace-
preserved CDATA for substantial literal Markdown; the draft is ordinary character
data. Escaped angle brackets and ampersands exercise required XML text handling.

First-review resolution: blocking metadata, numeric positions, placeholder-only CDATA,
and the empty style-guide ambiguity were removed. Fresh re-review confirmed that every
remaining ordering and CDATA choice carries independent meaning.

## Structural review result

The planned source mix is complete: 16 Markdown-first and 4 XML-first prompts. One
fresh reviewer examined each source independently. Eleven Markdown sources passed on
their first gate; five Markdown and four XML sources were revised and freshly
re-reviewed. `md-016` required one further correction and a third independent gate.

All 20 prompts are now `source-reviewed`. The review evaluated source hierarchy,
block attachment, ordering, markup naturalness, parser behavior, interpolation
boundaries, and lossless source-format hazards. It did not create translations or
infer Pydantic model shapes.
