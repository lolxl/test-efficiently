# Write tests that earn their runtime

## Short path for an isolated change

1. Name the wrong behavior and the smallest assertion that distinguishes it
   from correct behavior. Use the cheapest existing test layer that observes it.
2. Add or repair the test with only the mutable state it needs. For a regression,
   check the relevant bad version fails for the intended reason when feasible.
3. Run the named test and affected callers required by the project. Report the
   behavior checked and the result. Isolated edits without the risky boundaries
   or runner changes below can end on this short path.
   Other work applies the relevant [final checklist](quality-contract.md) items
   after its controls. Speed and measurement work also uses the
   [suite procedure](suite-optimization.md).

For any mock, preserve the unrelated runtime mechanisms the test still needs.
Global patches of process helpers, environment or random state need the same
isolation care as shared clock patches; prefer a seam owned by the behavior.

For example, a pure parser regression can be one input/expected-output row and
its focused test command. If it changes a clock binding, launches a child, shares
mutable state, caches preparation or checks a refusal/cleanup guard, continue to
the relevant controls below even if only one test changes. Runner discovery,
placement, sharding or concurrency changes also use the relevant inventory,
fixture and ownership controls via the
[correctness-only runner path](suite-optimization.md#correctness-only-runner-changes)
when there is no performance goal.

## Investigate intermittent failures

Keep the failed run's report, terminal status, content/command, execution context
and observed load; do not replace it with a later pass. Reproduce the original
context/load within the authorized resource budget, or state which conditions
could not be recreated. Form a specific hypothesis and use a control that can
distinguish it from alternatives before claiming a cause. A quiet replay passing
neither erases the failure nor proves load caused it. Follow the relevant clock,
fixture, process or guard controls below once the affected boundary is identified.
An isolated flaky test does not require a full-suite inventory unless runner
selection, population or suite interaction is part of the uncertainty.
On a shared host, use the project's coordination/admission mechanism for probes,
load replays and builds too; verify actual activity and ownership before starting.

## Controls for risky boundaries

Read only the sections relevant to the boundary being changed. These controls
supplement the short path and the project's required verification gates.

### Choose the observation before the machinery

Name a concrete wrong behavior and the smallest observation that distinguishes
it from the correct behavior. Then choose the layer:

| Question | Usually useful evidence | Expensive work to justify separately |
|---|---|---|
| Does an input select the right decision? | A table over a pure decision seam, including nearby refusals | A new process and repository for every equivalent row |
| Does a state transition preserve its invariants? | A small isolated stateful fixture and before/after assertions | Rebuilding unrelated application state |
| Does a boundary preserve arguments, environment, ownership or errors? | A real boundary invocation with a recorder and relevant failure paths | Repeating every pure decision through that boundary |
| Does cancellation reap owned work? | A controlled live child, injected interruption and evidence before fixture teardown | A timer that lets the child die independently |
| Is real latency within a budget? | A real clock, specified load and an explicit timing budget | Replacing elapsed time with a logical clock |

A policy table and a boundary test complement each other. Move rows to a cheaper
layer only after mapping what the real invocation previously covered. Include
different configuration sources, fallback behavior and process/lock/signal paths
when those affect the contract. A mock must leave unrelated runtime mechanisms
needed by the test intact. For a refusal guard, check that valid work still
runs, the named forbidden action is refused, and disabling that guard makes the
relevant test fail. An unrelated earlier refusal cannot prove guard sensitivity.

For existing tests whose layer or continued value is in question, use the
[case-by-case review and worksheet](test-review.md). It maps the behavior,
real boundaries and side effects before choosing what to retain or move.

For a new test, a useful short design note is: behavior; relevant bad version;
observation; chosen layer; required mutable isolation; expected expensive work.
It need not become a new repository template or approval door.

### Remove repeated preparation without sharing live state

Separate immutable source, templates and Git objects from mutable HOME, config,
queues, locks, history and outputs. Reusing the former may be cheap and safe;
sharing the latter needs a specific lifecycle contract. If caching preparation,
use the invalidation controls in [suite step 4](suite-optimization.md#4-apply-the-controls-for-the-mechanism-being-changed)
without loading the whole optimization procedure for an isolated fixture repair.

In a matrix test, identify which reset restores independence and which setup is
redundant. Preserve every row and assertion. Break the reset and confirm the
test detects cross-row contamination before accepting the optimization. Do not
call two rows equivalent merely because both use the CLI.

Use a real tracked input or generate it in the fixture. An input present only in
one author's ignored working files creates costly failures in copied trees and
CI. Likewise, a copied test tree may intentionally lack the original Git history;
a fixture that shells out to an unavailable base commit is not a portable oracle.

### Keep subprocess fixtures inside their actual execution boundary

A fresh directory is not necessarily a permitted directory. Allocate through the
runner's supported temporary-root interface, and give each invocation its own
mutable state. If children must stay beneath a declared prefix, put their temp
and fake-home directories beneath that prefix too. Avoid a literal host temp
path that happens to work in a developer's shell but is denied in a copied or
restricted context.

Exercise the real caller when checking environment propagation. A unit test of
an environment-builder dictionary does not establish what a nested process
actually receives. Cover absent or invalid configuration through an injectable
fallback location when available; do not grant access to a host directory just
to make that fixture pass. A repaired fixture still needs to reach the intended
assertion, rather than succeed through a new capability skip or earlier refusal.

Give process fixtures only the external behavior they need. For example, a
report-export test that happens to query a notification service can use a local
recorder for that service. Retain a real service-boundary check separately when
that behavior is part of the requirement. Keep the executable lookup environment
usable: replacing all of PATH with an empty directory can break the test driver
before it reaches the call being examined. Unreadable inherited PATH entries
are a fixture condition to handle explicitly, not a reason to open the real home.

### Test cancellation and cleanup as observable behavior

Arrange a controlled child that stays alive until the tested cancellation, then
inspect owned work before fixture teardown. A child with its own short lifetime,
or teardown that kills every leftover, can make broken cancellation look correct.
Where process identity can be reused, bind observations to a birth identity as
well as a process number. A matching name or process group is not ownership by
itself. Sampling can miss short-lived work; a failed sample is unknown, not proof
that a child exited.

Use failure injection at the actual cleanup boundary, with controls suited to
the platform and the contract:

| Fixture condition | Observation the test should protect |
|---|---|
| Normal owned scratch tree | The operation runs and its scratch is released. |
| A nested directory resists removal | Cleanup either safely releases it or reports failure and the retained locations; an unrelated sibling stays untouched. |
| Both the operation and cleanup fail | The primary failure remains available, the cleanup error is separately recorded, and the result is not presented as a clean success. |
| A destination is replaced or points elsewhere | The stated ownership/link policy holds; do not infer containment merely from an open directory handle. |

Use real permission behavior where it matters and the platform supports it; a
mocked removal exception alone cannot prove a filesystem guarantee. Keep any
threat-model limit explicit. If a test cannot exercise a capability in its current
context, record the skip and identify the context that can exercise it. A green
restricted run does not establish that its skipped host operations worked.

For repeated-run cleanup checks, compare retained paths against the invocation's
owned-root record and a before-run inventory. A new directory name, timestamp or
empty process list does not identify its creator. Report unattributed residue as
such, and do not make a test pass by deleting directories selected only by age or
a familiar name pattern.

### Control time at the correct seam

Patch the clock binding owned by the behavior under test, or supply a clock
through an existing fixture seam. Patching `time.sleep` on the shared standard
library module can also change subprocess timeout waits and unrelated code.
Enumerate every actual clock consumer: repairing one imported binding can leave
another consumer unobserved.

Keep a positive control showing an unrelated real timed wait still works.
If product waits are forbidden by the test, make a deliberate product wait fail
even when the product catches the exception. For a polling contract, preserve
requested intervals, ordering, deadline decisions and worker state; merely
advancing a fake clock until the expected result appears proves little.

Keep a real-clock check where real elapsed behavior is the requirement. Increasing
a timeout, dropping a timing assertion, or skipping a case needs its own changed
requirement; it is not a fixture diagnosis.

### Spend verification where it changes the decision

Start with the named behavior and its failing control. Exercise affected callers,
portable-copy contexts and generated inventory or implementation-map checks when
the change can affect them. Use the project's required complete run after the
candidate stabilizes, rather than consuming it as the first debugging command.

A reviewer suggesting hundreds of repetitions is offering a hypothesis. Identify
what the repetitions would establish, their cost and their applicability before
running them. Prefer a deterministic controlled reproduction when it addresses
the same uncertainty; do not substitute it for a real load/latency experiment.
