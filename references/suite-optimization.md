# Optimize an existing suite

Read the [quality contract](quality-contract.md). The useful objective is a
shorter reliable feedback loop, including the time spent validating the change.

## Establish a comparable baseline

Use the real runner in an isolated checkout. Capture the pinned content and
dirty state, command/options, runtime and CPU budget, competing tests/builds,
normal environment protections, terminal status, and the full stage boundary.
Record cache and thermal conditions when they matter; label them uncontrolled
when they were not measured.

Inventory executable entry points and stable case IDs. Include shell aggregators,
standalone leaf tests, public/portable copies, tooling, implicit skips and fixture
outcomes. A wrapper and a leaf with similar names may change environment, setup,
selection or error behavior.

Use existing machine-readable runner reports where available. Case time,
fixture events, worker elapsed time and unallocated time answer different
questions. Do not silently assign all unallocated time to fixtures.

## Find the binding cost

Separate build/setup, required subprocess work, polling/waits, assertions and
cleanup. Rank wall-clock critical paths as well as cumulative CPU/process time.
For overlapping stages, adding all stage durations overstates wall time.

Choose one bounded opportunity and state why it is worth the engineering and
validation cost. Examples include removing a proved duplicate launch, reducing
redundant fixture preparation, or avoiding repeated parsing on a measured hot
path. A large source file, a visible cache miss, or an attractive percentage of
a tiny subgroup is not sufficient evidence.

Useful stop decisions include: a negligible measured share, mixed paired
results, a change that alters the data path, or fixture costs that the model
cannot represent. Record the rejected idea rather than enlarging the patch
until some number improves.

## Preserve execution semantics

Deduplication needs an entry-point/behavior matrix: selection, inputs,
environment, fixtures, failures/skips and standalone behavior. Prove the chosen
entry reaches the leaves; a filename exclusion can silently lose future tests.

For preparation caches, measure the preparation separately. Key reusable input
by its real content and configuration, and preserve per-case mutable isolation.
Keep independent negative controls for stale, changed and foreign input.

For timing weights or sharding, account for class/module fixture re-entry and
ordering. Case-only costs may exclude the most expensive setup. Changing
placement can create new setup or destroy affinity, even with identical IDs.
Retain conservative fallback behavior for unmeasured or uncertain modules.

For concurrency, enumerate shared writers and nested pools, define the CPU/resource
budget, and keep the normal outer admission and isolation. Two six-worker stages
that overlap need twelve declared slots; a later four-worker stage need not
overlap them. More slots do not split an indivisible slow test. Cancellation must
retain ownership through the last child and cleanup, not merely until the parent
process returns.

## Validate the proposed explanation

Run the smallest changed behavior first, with relevant negative controls. Then
exercise the affected scope and any alternate execution context. Reconcile
intentionally added/retired cases and changed skips explicitly. Freeze the
candidate for the required complete run.

Use interleaved paired measurements when claiming a causal local improvement.
State the number of pairs, distributions and measured boundary. When only one
complete run is authorized, report it as one observation; do not invent a causal
claim or silently repeat it until it passes a target.

Inspect the complete report and terminal code independently of a console summary:
population identity, observed results, fixture/subtest accounting, fresh entry
execution, required stages, guard observations and reaping. Resolve schema
assumptions against an actual producer report rather than writing a permissive
reader around invented telemetry.

Report correctness and performance separately. A fully green run above the target
is a successful correctness validation and a missed performance target. Use its
new critical path to select the next experiment instead of rerunning unchanged
content without a reason.

## Release an anonymous benchmark safely

Permission to release measurements does not make their originating repository
or identity public. Verify actual visibility; authenticated access is not proof
of public availability. For an unpublished source, use anonymous candidate,
cohort and entry labels and omit repository/issue links, revision IDs, internal
names, private paths, accounts and host/process identifiers.

Keep permitted counts, timings, outcomes, geometry, sample size and limitations.
Disclose when original code, inventory identities or raw records are unavailable;
aggregate counts alone do not independently prove those private checks. Inspect
filenames, contents and all reachable publication history, not only the latest
working tree. Keep private source context and the portable public skill in
separate deliverables.
