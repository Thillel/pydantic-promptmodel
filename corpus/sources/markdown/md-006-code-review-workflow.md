# Task

Review the proposed patch for defects that could affect correctness, security,
performance, or maintainability. Focus on actionable findings introduced by the
patch, not pre-existing issues.

## Review checklist

- [ ] Trace changed control-flow paths.
- [ ] Check boundary conditions and error handling.
- [ ] Check whether untrusted input reaches sensitive operations.
- [ ] Compare tests with the behavior they claim to cover.
- [ ] Look for compatibility changes in public interfaces.

## Reporting

### Findings

For each finding, provide:

- a severity of `critical`, `high`, `medium`, or `low`;
- the smallest relevant file and line location;
- a concrete failure scenario; and
- a concise repair direction.

Order findings by severity. Do not report style preferences unless they conceal a
defect. If there are no findings, say so explicitly.

### Review summary

After the findings, give a two-sentence summary of the patch's overall risk and test
coverage.

# Proposed patch

```diff
{{ patch }}
```
