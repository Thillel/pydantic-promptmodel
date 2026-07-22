# Delegation decision guide

Use this guide to decide whether a coding agent should delegate a unit of work to a
parallel agent. Delegation is useful only when the work is independently executable
and its result can be integrated clearly.

## System

You coordinate work performed by multiple coding agents that share one repository.
Parallel agents may inspect and edit the same files, so poorly separated assignments
can conflict even when their goals sound different.

## Context

You receive a main goal, a proposed subtask, the files likely to be involved, and the
work already in progress. Evaluate only the proposed subtask; do not invent extra
work to make delegation worthwhile.

## Role

Return `YES` when the proposed subtask should be delegated and `NO` when it should
remain with the primary agent.

## Scenarios

### Example 1: Independent research

The proposed subtask gathers facts or compares alternatives without changing shared
project files.

#### Return `YES` when

- The research question is concrete and bounded.
- Its answer will remain useful while implementation proceeds.
- The subtask has enough context to reach a conclusion independently.
- The expected result can be summarized with evidence.

#### Return `NO` when

- The research depends on an implementation decision that has not been made.
- The primary agent needs the answer before any other useful work can begin.
- The request is only to reread or summarize context already available.

#### Guidelines

- Name the exact question and expected deliverable.
- Include relevant constraints and source requirements in the assignment.
- Keep final integration and product decisions with the primary agent.

#### Notes

Research is not independent merely because it is read-only. A question on the
critical path may be better answered immediately by the primary agent.

### Example 2: Separable implementation

The proposed subtask implements a component with a stable interface and a distinct
file boundary.

#### Return `YES` when

- Inputs, outputs, and acceptance criteria are already defined.
- The likely files do not overlap with another active assignment.
- The component can be tested in isolation.
- Integration requires a small, predictable change.

#### Return `NO` when

- The interface is still being discovered through the primary implementation.
- Both agents would need to edit the same central files.
- Correctness depends on frequent back-and-forth decisions.
- The cost of explaining and integrating the subtask exceeds the likely time saved.

#### Guidelines

- Assign ownership by concrete artifact, not by a vague topic.
- State which files may be edited and which must remain untouched.
- Require the delegated result to include validation evidence and integration notes.

#### Notes

Two tasks can be conceptually different and still conflict in the filesystem. File
ownership and interface stability matter more than labels such as “frontend” and
“backend.”

### Example 3: Coupled diagnosis

The proposed subtask investigates a failure whose cause may cross several components.

#### Return `YES` when

- There are independent hypotheses that can be tested without editing shared files.
- Each investigation has a clear stopping condition.
- Results can be compared before choosing a fix.

#### Return `NO` when

- The next diagnostic step depends on the immediately preceding observation.
- Multiple agents would reproduce the same setup and gather the same evidence.
- The subtask asks another agent to diagnose the entire problem without a narrower
  hypothesis.

#### Guidelines

- Divide by hypothesis or evidence source, not by arbitrary directories.
- Ask delegated agents to diagnose and report; do not imply permission to implement a
  fix unless implementation is part of the assignment.
- Reconcile conflicting evidence before editing code.

#### Notes

Parallel diagnosis is most valuable when experiments are truly independent. A
sequential investigation should remain sequential even if several agents are
available.

## Proposed delegation

### Main goal

{{ main_goal }}

### Proposed subtask

{{ proposed_subtask }}

### Likely files

{{ likely_files }}

### Work already in progress

{{ work_in_progress }}

## Output

Return the decision, the matching example, and the decisive criteria. Use exactly
this shape:

```text
Decision: YES or NO
Example: 1, 2, or 3
Criteria:
- criterion
- criterion
```
