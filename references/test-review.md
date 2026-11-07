# Review existing tests before moving or removing them

Use this guide when deciding whether existing tests should stay at their current
layer, move to a cheaper one, combine, or retire. Review the relevant subset,
not the whole suite by default. A few response lines or an existing work note can
hold the result; the worksheet below is optional, not a new report or approval gate.

This guide owns the case-by-case coverage decision. Use [test design](test-design.md)
for boundary controls, the [suite procedure](suite-optimization.md) for measured
cost changes, and the [final checklist](quality-contract.md) before reporting.

## Establish what the test protects now

Read the assertions and actual caller, not only the test name. Name the current
requirement and its source: a supported contract, project documentation or an
explicit requirement decision. Use change history when the expectation or path
has evolved. Current implementation alone does not prove a requirement has been
retired, and a slow or flaky test is not evidence that its protection is obsolete.
If the sources do not settle the intended behavior, record the gap before editing
the expectation or deleting the case.

Map inputs to observations, including both valid work and the forbidden outcome.
Separate these questions:

- Which input distinctions exercise different decisions or configuration sources?
- Which real invocation checks arguments, environment, errors or caller ordering?
- Which side effects must happen, and which must not happen before a refusal?
- Which observations depend on state, reset, child lifetime or a real clock?

An exit code alone may not answer them. A command can return the expected error
but already overwrite a file; another API can encode refusal in a result field
while returning a successful process status. Confirm the relevant branch was
reached rather than passing because of an earlier unrelated refusal.

## Choose a disposition from evidence

| Decision | Evidence needed |
|---|---|
| Keep at this layer | It observes a distinct real boundary or side effect, or the proposed replacement has an unresolved gap. |
| Move to a cheaper layer | The input decisions remain covered there, with the necessary real-boundary observations retained elsewhere. |
| Consolidate | The retained path covers the relevant inputs, observations and isolation; identify any reduced cross-component combinations explicitly. |
| Retire | The requirement is explicitly retired, or coverage is demonstrably replaced. Name that source or replacement and record the inventory delta. |
| Unresolved | Intent, overlap or sensitivity is not established; preserve the evidence and identify the missing check instead of forcing a deletion. |

Matching names, totals or final outputs do not establish equivalent coverage.
Check actual assertions, fallback/configuration sources, state and failure paths.
A parser table and a real caller test complement each other: moving decisions
into the table does not automatically replace the caller's side-effect checks.
There is no universal number of integration tests to keep; choose representatives
for the distinct boundary paths that remain and disclose combinations no longer
executed through the complete entry.

For a timing-sensitive test, distinguish the required latency boundary from
incidental preparation or unrelated pipeline work. Do not silently raise a
limit, move it to another boundary, or replace real elapsed behavior with a
logical clock. A different timing contract needs a stated requirement change
under the project's existing process.

## Copyable worksheet

Use one row per distinct input/behavior or protected observation, which may be
several rows for one test ID. Link to evidence actually inspected. Group repeated
rows only after explaining their equivalence.

| Original test / input | Current requirement + source | Wrong behavior + observable assertion | Real boundary / protected side effect | Existing or replacement evidence | Decision + destination layer | Failing control / remaining gap |
|---|---|---|---|---|---|---|
| Case or subcase ID; concrete input | Requirement and source location, or unknown | What incorrect behavior would make this red? | Calls, state changes, errors, ownership or time that a pure test cannot observe | Exact test/entry and assertions, or not found in the inspected scope | Keep, move, consolidate, retire or unresolved; name the retained path | Relevant bad version and expected failure, or evidence still needed |

Add a short mapping from removed/renamed IDs to replacement IDs or subcases and
any explicitly retired requirement. Count input rows, case IDs, subtests and
fixture outcomes separately. A smaller ID count is not by itself lost coverage,
and a matching count is not proof that coverage stayed intact.

If all rows produce the same value, use a distinct positive control where needed
to prove the intended source or operation is being observed. For example, a
configuration override equal to the default cannot prove the override was read.
Do not erase meaningful input distinctions just because their final values match.

## Worked example: a hypothetical report exporter

This is an invented design example, not a benchmark or a projection of private
code. Suppose its documented contract says: JSON and CSV are supported; a CLI
format overrides the configuration file; invalid input must be refused before
the destination is opened. The status of an old compatibility option is unstated.

| Existing case | Observation and gap | Proposed disposition | Relevant falsifier |
|---|---|---|---|
| Several supported/rejected format rows each start the whole CLI | Format selection is a decision; the CLI also wires parsing to the writer. | Move the decision rows to the parser seam; retain real entry coverage for writing and refusal ordering. | A relevant wrong format decision must fail the table; caller-order errors must fail the real entry check. |
| Unsupported format returns an error | Status alone cannot show whether an existing destination was already changed. | Keep a real refusal-path check and assert pre-existing destination bytes stay unchanged. | Opening/truncating the destination before rejecting the format must fail, even with the expected error status. |
| CLI format equals the file's configured format | The same output cannot establish which source won. | Use different valid formats and check the selected output through the actual configuration boundary. | Ignoring the CLI override must fail the distinct-value positive control. |
| Old compatibility-option test appears unused | No source establishes whether compatibility is still required. | Unresolved; find the requirement decision before retiring or rewriting it. | Evidence that compatibility remains supported rejects retirement. |

The parser tests do not prove filesystem safety, and the real entry check need
not repeat every pure format row. The cost and risk of the actual boundary,
not this example's row count, determine the retained set.

## Validate the proposed replacement

Use the relevant controls from [test design](test-design.md#controls-for-risky-boundaries).
For a meaningful behavior or guard change, show the valid path passing and a
relevant bad version failing for the intended reason. If a boundary moves,
check its actual caller and side effects; do not validate only an isolated mock.
An editorial worksheet change does not itself require artificial mutations.

Keep replacement sensitivity, required context coverage and the ID/skip/outcome
reconciliation explicit. Follow project-required complete validation after the
candidate stabilizes. Separate measured cost from estimates: removing a slow
case does not necessarily remove that duration from suite wall time because
other workers, fixture re-entry and serial stages may remain on the critical path.
This review does not authorize additional runs or make a speed claim by itself.
