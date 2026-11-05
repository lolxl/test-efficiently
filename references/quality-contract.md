# Quality contract

Boundary: the reusable contract for this skill. Project-specific runners and
verification requirements remain authoritative; the case study owns historical
facts, not additional universal rules.

## Preserve the question being tested

For a speed-only change, preserve behavior, assertions, input coverage, outcome
semantics and isolation. Compare stable case identities and failure/skip results,
not only a total or the last `OK` line. A matching count can hide replacement
cases; an unchanged list can hide newly skipped cases.

Deliberate retirement or consolidation is a separate correctness decision. Map
the removed cases and entry points to preserved behavior and failure controls,
or to an explicitly retired requirement. Report the inventory delta. Fewer test
IDs alone prove neither equivalence nor a speedup.

Reuse immutable fixture input when appropriate; give each case its own mutable
state and reset the state that can affect the next case. Cache source data or
content-addressed preparation only with a demonstrated invalidation and isolation
contract. Cached verdicts do not establish fresh execution.

## Make a failure observable

A regression or safety test needs a relevant failing control. First make the
valid path pass; then reintroduce the defect or disable the specific guard and
observe the named test fail for the intended reason. Missing imports, malformed
fixtures, an earlier unrelated refusal, and a no-op mutation are not that evidence.
Use this where it changes confidence, rather than manufacturing mutations for
every editorial edit.

Keep policy decisions, state transitions, real subprocess behavior and
wall-clock latency distinct. A logical clock can prove a polling/state contract;
it does not measure real latency. A mock must leave the unrelated runtime
mechanisms needed by the test intact.

## Measure the run that actually happened

Bind evidence to content, command, runtime, input population and relevant
environment. Distinguish targeted, public/portable, tooling and complete runs.
Record omissions and expected skips. Track fixture failures and subtest-only
cases separately when the framework does so.

For a complete timing claim, include normal build/setup, required stages,
process reaping and cleanup. Record worker geometry and competing work. On a
shared host, use the existing resource coordination mechanism for test probes
and builds as well as the main runner. Static old directories and editing-only
processes are not proof of an active build; verify actual activity and ownership.

Record terminal status from the real job. An interrupted job has no completed
result. A quiet successful replay does not erase an earlier failure or prove its
cause. A target time is distinct from an operational stop deadline.

## Bound the claim

Label observations, estimates and hypotheses. State sample size and scope.
An isolated improvement does not establish an end-to-end improvement; a
cross-version green run is not a paired causal experiment. A one-run success
does not establish a percentile, a stable service level or repeatability.

An arithmetic partition or average-load bound is a sizing aid. Account for
fixture lifecycle, setup, contention, serial work and cleanup before treating
it as an execution forecast. Seek enough margin for the claim that matters.

Choose further verification to resolve a named uncertainty. Freeze the candidate
for the required complete run, and broaden or repeat only when changed content,
new evidence, an unresolved concern, or project policy justifies it. Respect the
user's existing limits on costly runs; do not turn an unfavorable sample into an
unannounced search for a favorable one.
