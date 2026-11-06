# Optimize an existing suite

Use this preferred sequence for cost/performance work. Adapt the outputs to
existing runner reports and project notes; a table or a few task-response lines
may suffice. It does not require a new report format or test framework. For a
small isolated test edit, start with [test design](test-design.md#short-path-for-an-isolated-change).

## Correctness-only runner changes

For discovery, placement, sharding or concurrency changes without a performance
claim, record the content, command and execution context, then use step 2's
before-change inventory, the relevant step-4 controls, and step 5's post-change
ID/skip/outcome reconciliation, terminal-status and cleanup checks. Finish with
the applicable checklist items in step 6. This route needs no timing baseline
or step-3 performance experiment; the project's required verification gates
still apply.

## 1. Define the question and baseline boundary

**Input:** The requested time budget, correctness requirements and real runner.

**Output:** A baseline record naming content/dirty state, command/options,
runtime, CPU/resource budget, worker geometry (counts, overlap and nested pools),
terminal status and what elapsed time includes. Distinguish targeted, portable-copy,
tooling and complete-run scopes.
Use an isolated checkout. Include normal environment protections and competing
work; record cache/thermal conditions when relevant or label them uncontrolled.
On a shared host, use the existing coordination mechanism for probes and builds
as well as the main run. Verify actual activity and ownership: old directories
or editing-only processes alone do not prove an active build.

**Decision:** If the baseline is missing or incomparable, collect the smallest
measurement needed before selecting a speed change. Respect authorized run limits.

## 2. Inventory what actually executes

**Input:** Runner discovery, entry-point definitions and available reports.

**Output:** An entry/case map covering shell aggregators, standalone leaves,
portable copies, tooling, stable case IDs, implicit skips and fixture outcomes.
For example, record `wrapper -> leaf IDs; setup/environment; pass/skip/fail`.
A wrapper and similarly named leaf may differ in selection or error behavior.
Separate case time, fixture events, worker elapsed and unallocated time; do not
assign all unallocated time to fixtures.

**Decision:** If an entry or population cannot be accounted for, resolve that gap
before deduplicating, moving or excluding it. Matching totals alone are insufficient.
If the proposal moves, consolidates or retires tests, also make the relevant
[case-by-case coverage map](test-review.md#copyable-worksheet). An execution
inventory alone does not establish that their behavioral protection is redundant.

## 3. Select one binding cost and a falsifiable experiment

**Input:** The inventory and measured costs for build/setup, subprocesses,
polling/waits, assertions and cleanup.

**Output:** One bounded proposal with expected benefit, engineering/validation
cost and an observation that would reject it. Rank the wall-clock critical path
as well as cumulative CPU/process time; overlapping stage durations cannot simply
be added. Example: remove one duplicate launch only if the entry map proves the
same behavior still runs freshly through the remaining path.

**Decision:** Stop or choose another opportunity if the measured share is
negligible, paired results are mixed, the proposal changes the data path, or the
model cannot represent relevant fixture costs.
File size, a visible cache miss or a large percentage of a tiny subgroup is not
enough. Record a rejected idea without expanding the patch to chase a number.

## 4. Apply the controls for the mechanism being changed

**Input:** The selected proposal and its execution/lifecycle assumptions.

**Output:** The bounded change and relevant controls from this list; use only
those that apply, plus [test-design controls](test-design.md#controls-for-risky-boundaries)
for clocks, subprocess boundaries or guard sensitivity.

- **Duplicate launches:** Compare selection, inputs, environment, fixtures,
  failures/skips and standalone behavior. Prove the retained entry reaches the
  leaves; a filename exclusion can silently lose future tests.
- **Preparation/cache:** Measure preparation separately. Key immutable reusable
  input by real content/configuration; preserve each case's mutable state and
  reset contract. Use independent controls for stale, changed and foreign input.
- **Weights/shards:** Account for class/module fixture re-entry and ordering.
  Case-only cost can omit expensive setup; moving identical IDs can create setup
  or destroy fixture affinity. Retain conservative fallback for uncertain modules.
- **Concurrency/ownership:** Enumerate shared writers and nested pools; define
  resource limits and keep isolation active. Keep outer admission active too.
  Outer admission is the project's existing permission and resource check
  before launching work.
  Two overlapping six-worker stages require twelve declared slots; a later
  four-worker stage need not overlap them. Slots cannot split an indivisible test.
  Keep ownership through the last child and cleanup. Check owned processes are
  gone before fixture teardown, which could otherwise hide failed cancellation.

**Decision:** If equivalence, invalidation, isolation or ownership cannot be
shown, narrow or reject the change before a costly complete run.

## 5. Validate locally, then measure the frozen candidate

**Input:** The changed behavior, its negative controls and authorized run budget.

**Output:** Focused results, affected caller/alternate-context results and the
required complete report bound to unchanged candidate content. Reconcile added
or retired IDs and changed skips. Check terminal code independently of the
console summary: case identities/outcomes, fixture/subtest accounting, fresh
entry execution, required stages, guard observations and reaping. Resolve schema
assumptions against an actual producer report; do not write a permissive reader
around invented telemetry.

For a local comparison, interleave baseline and candidate runs and record the
pair count, the distribution, the measured boundary and the controlled
conditions. Apply the Claim item in the
[final checklist](quality-contract.md) to that sample.

**Decision:** A failed/interrupted run is evidence to investigate and retain.
If the complete run passes but exceeds the target, correctness passed and the
performance target was missed. Select further work from its measured critical
path; an unchanged retry needs a named reason.
If local paired results are mixed, return to step 3 or stop and record the
rejected proposal.

## 6. Report the result at the supported scope

**Input:** The retained baseline, candidate reports, costs and limitations.

**Output:** Apply the [final checklist](quality-contract.md) once before
reporting.

**Decision:** Accept the supported claim or state the remaining gap. An arithmetic
partition or average-load bound needs fixture lifecycle, setup, contention,
serial work and cleanup before it can become a credible forecast. Seek margin
for the actual requirement instead of treating a barely favorable estimate as a pass.

## If releasing an anonymous benchmark

Permission to release measurements does not publish the originating repository
or its identity. Verify actual visibility. Authenticated access does not show
that the repository is published. For an unpublished source, use anonymous candidate,
cohort and entry labels and omit repository/issue links, revision IDs, internal
names, private paths, accounts and host/process identifiers.

Keep permitted counts, timings, outcomes, geometry, sample size and limitations.
Disclose when original code, inventory identities or raw records are unavailable;
aggregate counts alone do not independently prove those private checks. Inspect
filenames, contents and all reachable publication history, not only the latest
working tree. Keep private source context and the published skill package in
separate deliverables.
