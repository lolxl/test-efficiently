# Write tests that earn their runtime

Read the [quality contract](quality-contract.md) first. This guide concerns the
choice and construction of tests, not a replacement for a repository's required
verification or release gates.

## Choose the observation before the machinery

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
when those affect the contract.

For a new test, a useful short design note is: behavior; relevant bad version;
observation; chosen layer; required mutable isolation; expected expensive work.
It need not become a new repository template or approval door.

## Remove repeated preparation without sharing live state

Separate immutable source, templates and Git objects from mutable HOME, config,
queues, locks, history and outputs. Reusing the former may be cheap and safe;
sharing the latter needs a specific lifecycle contract.

In a matrix test, identify which reset restores independence and which setup is
redundant. Preserve every row and assertion. Break the reset and confirm the
test detects cross-row contamination before accepting the optimization. Do not
call two rows equivalent merely because both use the CLI.

Use a real tracked input or generate it in the fixture. An input present only in
one author's ignored working files creates costly failures in copied trees and
CI. Likewise, a copied test tree may intentionally lack the original Git history;
a fixture that shells out to an unavailable base commit is not a portable oracle.

## Control time at the correct seam

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

## Spend verification where it changes the decision

Start with the named behavior and its failing control. Exercise affected callers,
copied/public contexts and generated inventory or implementation-map checks when
the change can affect them. Use the project's required complete run after the
candidate stabilizes, rather than consuming it as the first debugging command.

A reviewer suggesting hundreds of repetitions is offering a hypothesis. Identify
what the repetitions would establish, their cost and their applicability before
running them. Prefer a deterministic controlled reproduction when it addresses
the same uncertainty; do not substitute it for a real load/latency experiment.
