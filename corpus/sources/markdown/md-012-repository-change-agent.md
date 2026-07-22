# Repository change agent

These instructions govern changes to the Meridian Python service. Apply them whenever
you inspect, modify, test, or review this repository.

## Mission

Deliver the smallest complete change that satisfies the requested behavior while
preserving public interfaces, existing user work, and operational safety.

## Repository context

- Application code lives in `src/meridian/`.
- Tests mirror the application layout under `tests/`.
- Database migrations live in `migrations/` and are append-only after merge.
- Generated API documentation under `docs/api/` is produced from source annotations.
- The supported Python versions are 3.11 through 3.13.

## Working boundaries

### Files you may change

- Edit files directly required by the requested behavior.
- Add focused tests and small fixtures that demonstrate the behavior.
- Update human-authored documentation when a public contract changes.
- Create a migration only when the requested behavior changes persisted data.

### Files you must not change

- Do not edit generated API documentation by hand.
- Do not rewrite existing migrations.
- Do not update dependency locks unless dependency resolution is part of the request.
- Do not reformat unrelated files or remove unrelated worktree changes.

## Change workflow

1. Read the nearest implementation, tests, and public interface before editing.
2. State the behavior that must change and the behavior that must remain stable.
3. Identify the narrowest implementation boundary that owns the behavior.
4. Make the change and add a regression test that fails without it.
5. Run the smallest relevant test command.
6. Run the broader affected test group when shared code or a public interface changed.
7. Review the final diff for unrelated edits, generated files, and missing docs.

If a requested change conflicts with these instructions, explain the conflict and ask
for direction before modifying files.

## Python standards

### Public interfaces

- Add type annotations to public functions and methods.
- Preserve parameter names and return types unless the task explicitly changes the
  public contract.
- Use keyword-only parameters when adding independent optional behavior.
- Raise domain exceptions at package boundaries instead of leaking driver exceptions.

### Resource handling

Acquire external resources through context managers and release them on every exit
path.

Avoid:

```python
connection = pool.acquire()
result = connection.execute(query)
pool.release(connection)
return result
```

Prefer:

```python
with pool.connection() as connection:
    return connection.execute(query)
```

### Error translation

Translate low-level failures once, at the boundary where the application gains enough
context to name the failed operation.

Avoid:

```python
try:
    return repository.load(customer_id)
except Exception:
    raise RuntimeError("Something failed")
```

Prefer:

```python
try:
    return repository.load(customer_id)
except RecordNotFound as error:
    raise CustomerNotFound(customer_id) from error
```

### Asynchronous code

- Do not perform blocking file, network, or database operations on the event loop.
- Keep cancellation visible; do not convert `CancelledError` into an ordinary failure.
- Use structured concurrency when several child operations share one lifetime.
- Preserve input ordering only when the public contract promises it.

## Data migrations

A migration must be safe for a rolling deployment in which old and new application
versions run concurrently.

### Required sequence

1. Add backward-compatible storage first.
2. Deploy code that can read both old and new representations.
3. Backfill data with a restartable operation.
4. Switch writes only after the backfill is verified.
5. Remove obsolete storage in a later change.

### Migration review questions

- Can the migration resume after partial completion?
- Does it lock a high-traffic table for an unbounded period?
- Can the previous application version continue operating?
- Is rollback a schema rollback, an application rollback, or both?

## Tests

### Test selection

- Run one test module while iterating on isolated behavior.
- Run the package test group after modifying shared helpers.
- Run integration tests after changing persistence, serialization, or external-service
  boundaries.
- Do not report a test as passing unless its command completed successfully.

### Test quality

- Name tests after observable behavior, not implementation details.
- Include the failure case that motivated the change.
- Keep time, randomness, and external services under explicit control.
- Assert the public result or side effect instead of private call order unless call
  order is the contract.

## Security review

Treat a change as security-sensitive when it affects authentication, authorization,
tenant boundaries, secrets, deserialization, file paths, or command execution.

For a security-sensitive change:

- identify the trust boundary;
- show how untrusted input is validated;
- test at least one rejected input;
- avoid logging credentials or full user-provided payloads; and
- call out any residual risk in the final report.

## Completion report

When work is complete, report:

1. the behavior changed;
2. the principal files changed;
3. the validation commands and their results; and
4. any unresolved risk or follow-up.

Do not include a generic summary that merely repeats the task.
