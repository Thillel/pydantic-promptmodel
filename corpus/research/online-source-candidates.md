# Online prompt-source candidates

Research date: 2026-07-21

## Purpose

This is a provenance and selection ledger for prompt examples found in official
documentation and first-party repositories. It does not freeze any prompt and does
not reproduce complete external prompts.

External material is classified as one of:

- **direct structural candidate** — contains a concrete prompt whose structure is
  useful enough to inform a source draft;
- **negative or diagnostic candidate** — intentionally demonstrates a prompt with
  conflicts or failure modes;
- **guidance** — recommends prompt elements but does not provide a sufficiently rich
  prompt to use as a corpus source.

When an external structure is selected, we will create an original prompt for this
project, record the influence here and in the manifest, and review the new prompt on
its own merits. We will not silently copy a vendor example and call it ours.

## Best matches

### 1. OpenAI: Realtime model prompting guide

Source: [Using realtime models — OpenAI](https://developers.openai.com/api/docs/guides/realtime-models-prompting)

Classification: **direct structural candidate**

This is the closest match to the long, agentic Markdown requested for the corpus. The
guide explicitly recommends short labeled sections and supplies several substantial
prompt fragments. Its most useful complete pattern is a customer-support conversation
flow with:

```text
Conversation Flow
├── 1. Greeting
│   ├── goal prose
│   ├── response-rule list
│   └── exit condition
├── 2. Discover
├── 3. Verify
├── 4. Diagnose
├── 5. Resolve
└── 6. Confirm/Close
```

The same guide separately covers role and objective, personality and tone, language,
reasoning, message channels, preambles, verbosity, tools, unclear audio, entity
capture, long-context behavior, and escalation. It also demonstrates:

- H1 and H2 heading hierarchy;
- repeated phase sections with an identical internal schema;
- unordered action lists;
- inline labels for goal and exit criteria;
- example dialogue as block quotes;
- tables for tool-risk policy;
- fenced JSON and text examples;
- positive and negative examples;
- conditionally applicable sections rather than one mandatory universal skeleton.

Why it matters here: this is direct official evidence that a useful agent prompt may
combine document hierarchy, repeated semantic records, block syntax, and ordering.
It is also a concrete warning against treating every named concept as a peer field.

Corpus use: `md-011` is an original incident-response prompt using the repeated
`phase -> goal/actions/exit` structure. It does not duplicate the NorthLoop example
or its wording.

### 2. OpenAI: general prompt-engineering guide

Source: [Message formatting with Markdown and XML — OpenAI](https://developers.openai.com/api/docs/guides/prompt-engineering#message-formatting-with-markdown-and-xml)

Classification: **direct structural candidate and guidance**

OpenAI explicitly recommends Markdown headings and lists for prompt boundaries and
hierarchy, and XML tags for delimiting supporting content and attaching metadata. The
guide describes a common developer-message order:

1. identity;
2. instructions;
3. examples;
4. context.

Its concrete code-generation example is a hybrid rather than pure Markdown: H1
sections hold prose and bullet rules, while example user/assistant turns are enclosed
in XML tags. This is small, but it is highly relevant because it comes from the exact
Markdown/XML boundary the project intends to model.

Why it matters here: it supports keeping hybrid prompts in the corpus rather than
pretending all examples will be pure Markdown or pure XML. It also makes section
ordering part of the source design.

Corpus use: `md-014` is an original release-triage prompt whose Markdown headings
define the outer document and whose few-shot records use live XML boundaries. Its
fenced XML output template remains literal code, creating both kinds of XML use in
one reviewed source.

### 3. GitHub: complete Copilot code-review instructions

Source: [Using custom instructions to improve Copilot code review — GitHub](https://docs.github.com/en/copilot/tutorials/customize-code-review)

Classification: **direct structural candidate**

GitHub provides a complete multi-section Markdown instruction example for code review.
Its main document separates purpose, security, performance, code quality, review
style, and testing. The guide also demonstrates path-specific instruction files that
combine YAML front matter with Markdown headings, bullet rules, and fenced code
showing avoid/prefer pairs.

Why it matters here:

- It is a real agent-programming artifact rather than a one-shot chat prompt.
- Its sections mix prose, rules, and code examples naturally.
- The path-specific examples raise an important scope question: is YAML front matter
  part of the prompt representation, renderer metadata, or outside this library?
- A complete file may be assembled from repository-wide and path-specific instruction
  layers rather than one standalone prompt.

Corpus use: `md-012` is an original repository-change prompt with H1/H2/H3 sections,
code fences, and explicit avoid/prefer patterns. YAML front matter is excluded from
the rendered source and recorded as a deliberate scope decision rather than assumed
renderer metadata.

### 4. Google: agentic system-instruction template

Source: [Prompt design strategies — Gemini API](https://ai.google.dev/gemini-api/docs/prompting-strategies#agentic-workflows)

Classification: **direct structural candidate**

Google describes this as a researcher-evaluated instruction for agentic benchmarks.
The example is long and dominated by deeply nested ordered rules rather than Markdown
headings. Its major concerns include:

- logical dependencies and precedence;
- action risk;
- hypothesis exploration;
- adaptation after new observations;
- available information sources;
- precision and grounding;
- completeness;
- persistence and retry behavior;
- delaying action until checks are complete.

Why it matters here: it challenges a section-centric design. A correct Markdown prompt
can express most of its structure through hierarchical list numbering and indentation,
with almost no headings. The eventual model cannot treat lists as leaf arrays of
strings if list items contain subordinate rules.

Corpus use: `md-013` is an original production-migration policy centered on a deeply
nested ordered list. It supplies the intended challenge to a heading-centric design.

## Useful contrasts

### 5. OpenAI: GPT-5.1 event-planning agent and metaprompt

Source: [How to metaprompt effectively — OpenAI Cookbook](https://developers.openai.com/cookbook/examples/gpt-5/gpt-5-1_prompting_guide#how-to-metaprompt-effectively)

Classification: **negative or diagnostic candidate**

The guide presents a long event-planning system prompt organized with uppercase
plain-text labels and bullet lists. It then explains observed failures caused by
conflicting instructions about tool use, autonomy, clarification, verbosity, tone,
and response structure. Two further prompts diagnose the conflicts and request a
surgical revision.

This is useful for three reasons:

1. Uppercase lines visually resemble headings but are ordinary paragraph content in
   Markdown. A renderer must not promote them implicitly.
2. A prompt can be well organized syntactically while still being semantically
   inconsistent.
3. The original prompt, diagnostic prompt, and revised prompt form a versioned family,
   not interchangeable renderings of one prompt.

Corpus use: the first draft of `md-015` tested this convention in an original
temporary-access review prompt. Structural review found that a long nested policy was
not a natural heading-free document, so the retained prompt now uses explicit
headings. A shorter genuinely flat use remains a possible future candidate.

### 6. Anthropic: complex prompts from scratch

Source: [Chapter 9 — Anthropic prompt-engineering course](https://github.com/anthropics/courses/blob/master/prompt_engineering_interactive_tutorial/Anthropic%201P/09_Complex_Prompts_from_Scratch.ipynb)

Classification: **direct structural candidate and guidance**

Anthropic's tutorial builds long prompts from separately named elements. The career
coach and legal examples cover task context, tone, detailed rules, examples, input
data, the immediate request, analysis guidance, output formatting, and assistant
prefill. The lesson stresses that some element ordering matters and other ordering is
flexible.

The assembled prompts are mostly prose and XML-delimited data rather than heading-rich
Markdown. This contrast is valuable: the source code gives semantic elements names,
but the concatenated prompt does not automatically render those names as visible
headings.

Why it matters here: it is almost a direct demonstration of the mistake this project
must avoid. A variable named `TASK_CONTEXT` does not prove that the final prompt has a
`Task Context` heading. Source-code composition and rendered prompt structure are
different layers.

Corpus use: `xml-003` is an original long-document synthesis prompt with attributed
documents, multiple examples, conflict handling, and a grounded fallback. It records
the actual XML tree rather than any source-code variable layout.

### 7. Anthropic: current prompting best practices

Source: [Prompting best practices — Anthropic](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)

Classification: **guidance**

The current guide recommends clear, sequential instructions when order matters,
several relevant and varied examples, descriptive XML tags for complex prompts, a
system role, and deliberate placement of long input documents and queries. It also
recommends nesting document elements when the data has natural hierarchy.

Why it matters here: it validates several planned coverage dimensions, especially
multiple examples, nested context records, and the need to keep variable input
distinguishable from instruction text. It does not provide one long Markdown prompt
that should be copied into the corpus.

Corpus use: these recommendations informed review of the multi-example `md-014` and
the nested long-context `xml-003`. They remain guidance for the later translation
stage rather than source text.

### 8. Anthropic: Claude Code project instructions

Source: [Claude Code best practices — Anthropic](https://code.claude.com/docs/en/best-practices#write-an-effective-claudemd)

Classification: **guidance and small structural candidate**

Anthropic recommends keeping `CLAUDE.md` short, human-readable, broadly applicable,
reviewed, and pruned. Its example uses Code Style and Workflow headings with bullet
rules, and it demonstrates importing more focused instruction files by path.

Why it matters here: a realistic agent prompt may be a composition of multiple
Markdown files. Exact rendering then depends on whether imports are literal text,
references, or an external preprocessing feature. The project should not smuggle
import semantics into ordinary links or strings.

Corpus use: `md-012` adopts the natural repository-instruction form but deliberately
does not add import syntax. Composed prompt sources remain a later scope decision.

## Candidate ranking for this corpus

| Rank | Source pattern | Fit | Main structural value | Main caution |
| ---: | --- | --- | --- | --- |
| 1 | OpenAI Realtime conversation flow | Excellent | Long heading hierarchy with repeated phases and exit criteria | Voice-specific details should not be copied merely for variety. |
| 2 | GitHub complete code-review instructions | Excellent | Natural agent-programming Markdown with prose, lists, and code | YAML front matter may be outside renderer scope. |
| 3 | Google agentic system instruction | Excellent contrast | Deep ordered-list hierarchy with few headings | It is not itself a heading-rich Markdown document. |
| 4 | OpenAI Markdown/XML developer message | High | Official hybrid Markdown/XML pattern | The supplied example is short. |
| 5 | Anthropic complex-prompt tutorial | High contrast | Named semantic elements versus actual rendered text | The tutorial is older and its source variables are not document structure. |
| 6 | OpenAI event-planner metaprompt example | Diagnostic | Long conflicting prompt plus a repair workflow | The original is deliberately not optimal. |
| 7 | Anthropic current best practices | Review guidance | Examples, XML hierarchy, long-context placement | No single long Markdown source to adopt. |
| 8 | Anthropic `CLAUDE.md` guidance | Scope guidance | Multi-file instruction composition | Imports require semantics beyond standalone Markdown. |

## Corpus selection outcome

The research-driven additions are now present:

1. `md-011`: repeated incident-response phases with heterogeneous internal blocks;
2. `md-012`: long repository instructions with avoid/prefer code examples;
3. `md-013`: hierarchy carried by a deeply nested ordered list;
4. `md-014`: Markdown outer structure with XML-delimited few-shot examples;
5. `md-015`: a reviewed counterexample in which uppercase plain-text labels were
   replaced because they concealed real hierarchy; and
6. `xml-003`: attributed long documents, multiple examples, and grounded fallback.

Coverage review selected two additional originals rather than seeking external text:

- `md-016` exercises literal Markdown, nested fences, conditional blocks, and exact
  output order.
- `xml-004` contrasts strict and order-independent collections while carrying a
  literal Markdown template in CDATA.

These sources are structurally inspired by the documented patterns, not adaptations
of vendor wording or scenarios. They remain drafts pending source-quality review.

## Research conclusions

- Long production prompts are often heterogeneous: headings, paragraphs, lists,
  tables, examples, XML tags, and code blocks coexist.
- Repeated agent phases frequently have meaningful internal order, not just a mapping
  of names to values.
- Absolute heading depth matters. A document title shifts the visible level of every
  child section.
- Labels written in uppercase or bold are not Markdown headings.
- Nested list items can be structural containers with child rules, so `list[str]` is
  not always adequate.
- Prompt source code, prompt metadata, and rendered prompt text are distinct layers.
- Official guidance consistently favors evaluation and iteration; vendor examples
  should be treated as candidates to review, not immutable truth merely because they
  are official.
