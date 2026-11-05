# Final quality checklist

Use once before reporting, at the scope relevant to the change. Project runners
and required verification/release gates remain authoritative; the case study
supplies historical evidence, not additional universal rules.

- **Behavior:** For a speed-only change, retain assertions, input coverage,
  failure/skip semantics and isolation, including refusal and cleanup paths. If
  deliberately retiring or consolidating tests, map removed cases/entries to preserved behavior and failure controls or
  an explicitly retired requirement; report the inventory delta. Fewer IDs alone
  establish neither equivalence nor a speedup.
- **Sensitivity:** Where a regression or guard is at risk, show the valid path
  passing and a relevant bad version failing for the intended reason. An earlier
  unrelated refusal, malformed fixture, missing import or no-op mutation is not
  that control. Do not manufacture mutations for editorial changes.
- **Execution:** Bind evidence to content, command, runtime, population and
  environment. For every speed-only change, including fixture, cache and runner
  changes, compare stable case identities and skip/outcome results across the
  affected population, not just totals or `OK`; separate fixture failures and
  subtest-only cases. Record omissions and expected skips. Cached verdicts do not prove fresh
  execution.
- **Boundary and status:** Name the measured scope. Complete timings include
  normal build/setup, required stages, reaping and cleanup. Capture terminal
  status; an interrupted job has no completed result. A successful replay does
  not erase a failure or establish its cause. A target is not a stop deadline.
- **Claim:** Separate observations, estimates and hypotheses; state sample size.
  Logical clocks do not measure real latency, local wins do not establish a suite
  win, cross-version runs are not paired causal experiments, and n=1 establishes
  no percentile or repeatability. Arithmetic bounds are not execution forecasts.
- **Cost and next action:** Report verification cost and unresolved uncertainty.
  Repeat or broaden only for changed content, new evidence, a named unresolved
  concern or project policy, within the user's run limits. Keep a frozen candidate
  for the required complete run; do not search silently for a favorable sample.

Boundary-specific mechanisms live in [test design](test-design.md#controls-for-risky-boundaries)
and the [suite procedure](suite-optimization.md), rather than being repeated here.
