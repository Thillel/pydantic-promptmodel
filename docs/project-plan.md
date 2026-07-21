# Pydantic PromptModel design plan

## Purpose

This project will explore an extension of Pydantic's `BaseModel` that represents an
LLM prompt in a form that is natural to read and author. A prompt model should
eventually be renderable as Markdown or XML with a single operation.

The current stage is design research. Its deliverable is a reviewed collection of
three-way examples:

```text
frozen Markdown <-> frozen XML <-> proposed prompt model
```

This stage does **not** include implementing the renderer or committing to a model
API prematurely.

## The central constraint: syntax is part of the ground truth

The model must not merely contain the same words as the prompt. It must preserve the
prompt's document structure.

For example, this Markdown:

```markdown
# Task

Summarize the article.

## Requirements

- Be concise.
```

contains all of the following facts:

- `Task` is a level-one section.
- `Summarize the article.` is paragraph content directly inside that section.
- `Requirements` is a level-two subsection of `Task`.
- `Be concise.` is an unordered-list item inside `Requirements`.

A model with sibling `description` and `requirements` fields does not automatically
preserve those facts. It only works if `description` is given undocumented special
rendering behavior. We must not invent such behavior before the examples establish
that it is general, predictable, and readable.

Accordingly:

1. Correct prompts are designed before models.
2. Frozen prompts are never changed to make a proposed model easier to design.
3. Field names do not silently imply Markdown syntax unless the final design
   deliberately specifies and consistently demonstrates that convention.
4. A model proposal is acceptable only if there is a consistent mapping from the
   entire model back to the frozen prompt.

## Why Markdown leads the corpus

Markdown expresses hierarchy partly through headings and partly through the blocks
that follow them. That makes details such as heading depth, paragraph placement,
list type, nesting, and block order significant. XML makes much of its hierarchy
explicit through element nesting, so the structural problem is generally easier to
see there.

The source corpus will therefore use this approximate split:

- **80% Markdown-first:** design and freeze the correct Markdown, then derive XML.
- **20% XML-first:** design and freeze the correct XML, then derive the best
  Markdown representation.

The XML-first minority prevents the design from treating XML as a cosmetic
serialization of Markdown. It should expose structures such as attributes, repeated
elements, mixed content, and explicit containers that require real translation
decisions.

## What “reversible” means

The target is structural and content reversibility, with a canonical textual
rendering.

For every frozen pair, the eventual design must support these invariants:

```text
Markdown -> model -> Markdown = frozen canonical Markdown
XML      -> model -> XML      = frozen canonical XML
model    -> Markdown -> model = equivalent model
model    -> XML      -> model = equivalent model
```

Byte-for-byte preservation of arbitrary whitespace is not required unless we later
choose to represent formatting trivia. The frozen files define the canonical output,
including meaningful ordering, heading levels, list styles, code fences, and block
types. XML comparison may normalize insignificant formatting, but it must not ignore
element order, attributes, text, or hierarchy.

## Artifacts and states

Each example receives a stable identifier such as `md-001` or `xml-001`; the prefix
records its source format, not its eventual preferred rendering.

An example advances through these states:

1. `source-draft` — the source prompt exists but may still change.
2. `source-reviewed` — its content and source-format structure are judged optimal.
3. `translated-draft` — the counterpart format exists but is still under review.
4. `pair-frozen` — both representations are approved and may no longer be adjusted
   for model convenience.
5. `model-draft` — one or more model representations are being compared.
6. `model-approved` — the model is readable, consistent, and passes the round-trip
   criteria.

Repository layout:

```text
corpus/
├── manifest.md              # inventory, coverage, and current state
├── sources/
│   ├── markdown/            # Markdown-first source prompts
│   └── xml/                 # XML-first source prompts
├── pairs/                   # translations, only after source review
└── models/                  # model proposals, only after pairs freeze
```

`corpus/pairs/` and `corpus/models/` intentionally remain empty during initial
source collection.

## Phase 1: define corpus coverage

Before judging an abstraction, the corpus must be broad enough to break simplistic
ones. The manifest records which examples exercise each relevant structure.

Markdown coverage should include:

- documents with and without a document title;
- skipped and unskipped heading levels where appropriate;
- paragraphs directly under headings;
- sibling sections and nested subsections;
- ordered, unordered, nested, and task lists;
- multiple paragraphs within a single list item;
- block quotes and quoted conversations;
- fenced code blocks with language identifiers;
- inline code, emphasis, strong emphasis, and links;
- tables;
- thematic breaks;
- placeholders and literal delimiter-like text;
- repeated sections such as few-shot examples;
- content where ordering is semantically important;
- prose before the first heading, when genuinely useful.

XML coverage should include:

- nested and repeated elements;
- wrapper/container elements;
- attributes that carry real semantics;
- empty or optional elements;
- mixed text and child elements;
- content requiring escaping or CDATA decisions;
- ordered instructions or examples;
- structures whose best Markdown representation is not obvious.

Prompt-purpose coverage should include summarization, extraction, classification,
generation, transformation, evaluation, planning, tool use, safety or policy
constraints, few-shot learning, and multi-input synthesis.

### Phase 1 exit gate

- The intended corpus has an approximately 80/20 Markdown/XML source split.
- Every important syntax category has at least one planned example.
- No example exists solely to duplicate the structure of another.

## Phase 2: create source prompts

Create realistic prompts in their designated source formats. At this point, optimize
only the source prompt. Do not create the counterpart merely to maintain momentum;
doing so can cause translation concerns to distort the source.

Every source should be checked for:

- clarity and plausibility as an actual LLM prompt;
- correct and intentional document hierarchy;
- useful rather than decorative markup;
- unambiguous scope of instructions;
- consistent placeholder spelling;
- deliberate ordering of roles, inputs, constraints, examples, and output format;
- absence of artifacts added only to test syntax.

### Phase 2 exit gate

- Each prompt is considered optimal in its source format.
- Every markup construct is intentional and defensible.
- Reviewers would not rearrange the prompt merely for style.
- The manifest marks the example `source-reviewed`.

## Phase 3: translate each reviewed source

Only reviewed sources may be translated.

For Markdown-first examples, create XML that preserves meaning, hierarchy, order,
and distinctions between container structure and content. The XML does not have to
imitate Markdown mechanically; it should be the natural XML expression of the same
prompt.

For XML-first examples, create Markdown that is genuinely good Markdown rather than
a tag-by-tag transcription. Any XML distinction without a direct Markdown analogue
must be represented deliberately and documented.

Each translation receives a short decision log covering non-obvious choices, such
as:

- whether a paragraph became element text or a named child;
- whether an attribute became a label, prose, or another structure;
- how repeated XML elements became list items or repeated sections;
- whether a wrapper element has a visible Markdown counterpart;
- how heading depth was selected.

### Phase 3 exit gate

- The source and translation express the same prompt.
- Neither side contains information absent from the other.
- Ordering and hierarchy are preserved or have an explicit, repeatable mapping.
- Both files are natural in their own format.

## Phase 4: freeze the pairs

Review each Markdown/XML pair as an independent artifact before thinking about
Pydantic.

Freezing means:

- content and structure are approved;
- canonical formatting is established;
- later model experiments may not modify either prompt;
- a model that cannot represent the pair must be improved or rejected.

This is the boundary that prevents the examples from being shaped around a favored
implementation.

### Phase 4 exit gate

- Every example is marked `pair-frozen`.
- The complete corpus still satisfies the coverage matrix.
- Cross-format decisions use consistent rules or explicitly identify necessary
  exceptions.

## Phase 5: propose a model for every frozen pair

Only now should Pydantic representations be written. Start locally: find the most
natural representation of each individual pair before extracting a universal API.

For each proposal, record:

- the complete model class definitions;
- a populated example instance or template declaration;
- the rule that maps every field and value to Markdown;
- the rule that maps every field and value to XML;
- whether the representation preserves order and hierarchy;
- any reserved names, metadata, annotations, or implicit behavior;
- anything that is awkward or format-specific.

A pleasant-looking class is insufficient if its rendering depends on unexplained
special cases. Conversely, a perfectly lossless generic syntax tree may be too
mechanical to satisfy the goal of natural authoring. The corpus will reveal where
the right balance lies.

### Phase 5 exit gate

- Every frozen pair has at least one complete model representation.
- Each mapping is explicit and works for the whole corpus.
- Reserved names and conventions, if any, are few, documented, and consistent.
- The representation reads like a prompt rather than renderer configuration.

## Phase 6: compare and consolidate the model design

Compare the per-example models and identify recurring concepts only after seeing
all of them. Candidate abstractions might include sections, body blocks, inputs,
examples, or rendering metadata, but none is assumed in advance.

Evaluate candidate APIs against:

- human readability;
- ordinary Pydantic conventions;
- explicit preservation of structure;
- minimal hidden behavior;
- Markdown and XML parity;
- stable rendering;
- parsing and round-trip feasibility;
- support for extension without redesigning the core.

If no single abstraction handles the corpus naturally, document the failure instead
of weakening the examples. That result may indicate a layered design, a small set of
explicit primitives, or a distinction between semantic and presentation models.

## Phase 7: validate before implementation

Turn every frozen example into a conformance case. Before building the full library,
specify the expected renderings and verify that the proposed design can satisfy all
round trips without per-example hacks.

Only after the design passes this validation should implementation planning begin.

## Working rules

- Do not add model sketches while any source prompt remains under structural review.
- Do not translate an example before it is marked `source-reviewed`.
- Do not change a frozen pair to rescue a model proposal.
- Do not equate shared vocabulary with shared hierarchy.
- Do not treat field names such as `description`, `body`, or `requirements` as
  special unless that behavior becomes an explicit part of the approved design.
- Record uncertainty in the manifest instead of silently resolving it.
- Prefer a smaller, well-reviewed corpus over a large collection of shallow variants,
  while retaining enough diversity to challenge the design.

## Current milestone

The immediate milestone is source collection and review:

1. establish the initial coverage matrix;
2. draft Markdown-first and XML-first prompts;
3. audit the sources for usefulness and structural diversity;
4. revise them until they can be marked `source-reviewed`.

No XML counterparts for Markdown-first prompts, Markdown counterparts for XML-first
prompts, or Pydantic model representations should be created during this milestone.
