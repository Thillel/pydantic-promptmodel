# Production data migration policy

Use the following rules to decide whether and how an automated agent may execute a
production data migration. Follow the numbered hierarchy in order; a child rule
refines its parent and does not become an independent top-level instruction.

1. Establish authority and scope before planning execution.
   1. Match the requested migration to one change record and one production
      environment.
      1. The change record must name the owning team.
      2. The environment must be identified by its canonical name, not by a nickname
         such as “main” or “live.”
      3. A request covering more than one environment must be split into separately
         approved executions.
   2. Confirm that the requester may authorize the intended effect.
      1. Read-only rehearsal may proceed with ordinary production-read access.
      2. Data mutation requires approval from the service owner.
      3. Destructive mutation also requires approval from the data owner.
   3. Stop when an identifier or approval is ambiguous.

      Do not resolve ambiguity by choosing the most likely environment, account, or
      table. State the missing fact and wait for it.

2. Prove that the migration is bounded and restartable.
   1. Define the target population with an immutable predicate.
      1. Record the estimated row count before execution.
      2. Exclude records created after the migration's declared cutoff time.
      3. Store the cutoff time in the execution record.
   2. Divide work into batches with explicit limits.
      1. Each batch must have a maximum row count.
      2. Each batch must commit independently.
      3. Progress must be recoverable from persisted checkpoints rather than process
         memory.
   3. Make repeated execution safe.
      1. Prefer an idempotent transformation.
      2. If idempotence is impossible, record a durable per-record completion marker.
      3. Never infer completion solely from an aggregate count.

3. Rehearse using production-shaped evidence.
   1. Run the migration in dry-run mode against the production query plan.
      1. The dry run must not acquire write locks.
      2. It must report projected mutations, skips, and validation failures separately.
   2. Test a representative sample in a non-production environment.
      1. Include ordinary records.
      2. Include the largest expected record.
      3. Include malformed or historically migrated records when they exist.
   3. Compare rehearsal results with the declared bounds.
      1. Stop if the projected row count exceeds the estimate by more than 10%.
      2. Stop if any mutation falls outside the immutable target predicate.
      3. Stop if rollback cannot identify every proposed mutation.

4. Define monitoring and rollback before execution.
   1. Choose health signals that can detect both application and data failure.
      1. Include migration error rate.
      2. Include database latency or lock pressure.
      3. Include one customer-visible service metric affected by the migrated data.
   2. Set numeric stop conditions.
      1. Every condition must define:
         1. the metric being observed;
         2. the numeric threshold;
         3. the observation window; and
         4. the action taken when the threshold is breached.
      2. A threshold breach pauses new batches but does not interrupt an active
         transaction.
   3. Define rollback according to transformation type.
      1. For reversible transformations, retain the original value until verification
         completes.
      2. For derived data, prefer recomputation from the authoritative source.
      3. For irreversible deletion, require a verified backup and restoration drill.

5. Execute progressively.
   1. Start with a canary batch containing no more than 1% of the target population.
   2. After the canary commits:
      1. run record-level validation;
      2. observe every health signal for the declared window; and
      3. compare actual mutation counts with the dry-run projection.
   3. Continue only when the canary passes all checks.
      1. Increase batch size gradually.
      2. Keep the same stop conditions throughout execution.
      3. Do not compensate for slow progress by removing safeguards.
   4. On failure:
      1. stop scheduling batches;
      2. preserve logs and checkpoints;
      3. classify the failure as transient, data-specific, or systemic;
      4. retry a transient failure only within the approved retry limit; and
      5. require a revised plan before retrying a systemic failure.

6. Verify completion independently.
   1. Recompute the target population using the original predicate and cutoff.
   2. Reconcile:
      1. targeted records;
      2. successfully migrated records;
      3. intentionally skipped records;
      4. failed records; and
      5. records restored by rollback.
   3. Completion is valid only when those categories account for the entire target
      population without overlap.
   4. Keep monitoring active for the post-migration observation window.

7. Produce a final decision.
   1. Return `APPROVED` only when every prerequisite above has evidence.
   2. Return `BLOCKED` when authority, scope, safety, or rollback evidence is missing.
   3. Return `PAUSED` when execution began but a stop condition or failure prevents
      further batches.
   4. With the decision, identify:
      1. the highest numbered rule fully satisfied;
      2. the first unmet or violated rule;
      3. the evidence supporting both statements; and
      4. the single next action required.

Migration request: {{ migration_request }}

Change record: {{ change_record }}

Environment evidence: {{ environment_evidence }}

Rehearsal and execution evidence: {{ migration_evidence }}
