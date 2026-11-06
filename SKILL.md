---
name: test-efficiently
description: Write or repair useful tests, diagnose flakiness and costly fixtures, and speed up suites without weakening behavior or evidence. Isolated test edits start with references/test-design.md. Runner discovery, placement, sharding or concurrency changes use the relevant controls in references/suite-optimization.md even without a speed claim. Speed and measurement work also reads references/quality-contract.md. Ordinary edits without a testing question do not need this skill.
license: MIT
---

# Test Efficiently

Improve the time to a trustworthy answer, including implementation, diagnosis,
review and validation cost. A shorter command is useful only when it answers
the same relevant question. This standalone folder and its sanitized data are
covered by [MIT](LICENSE); that license does not cover other repository content.

## Choose the path by risk and cost

- **Intermittent or load-sensitive failure:** start with
  [flakiness diagnosis](references/test-design.md#investigate-intermittent-failures).
  Preserve the failed run and investigate its context before choosing a repair;
  a passing quiet replay does not establish a cause.
- **Small, isolated test additions or repairs with a known behavior:** read
  [test design](references/test-design.md), starting at the
  [short test-design path](references/test-design.md#short-path-for-an-isolated-change).
  A pure input/output assertion usually needs no suite inventory or benchmark.
- **General test writing across several behaviors:** apply the short path to
  each behavior and use the [layer-selection table](references/test-design.md#choose-the-observation-before-the-machinery).
  Scale verification to affected behavior and risk, rather than test count.
- **Review existing tests or decide which to move, combine or retire:** use
  the [case-by-case review](references/test-review.md), including its optional
  worksheet. Review only the relevant cases; ordinary isolated edits keep the
  short path above.
- **Clocks, child processes, shared mutable fixtures, caches, refusal or cleanup
  guards:** read the relevant [test-design controls](references/test-design.md#controls-for-risky-boundaries)
  even for a single test. Test count alone does not determine risk.
- **Runner discovery, placement, sharding or concurrency changes:** use the
  [correctness-only runner path](references/suite-optimization.md#correctness-only-runner-changes)
  when there is no performance claim. Speed or measurement work uses the full
  suite procedure below.
- **Test-cost investigation or suite-speed improvement:** read the
  [final checklist](references/quality-contract.md) and follow the numbered
  [suite-optimization procedure](references/suite-optimization.md). It produces
  an inventory, a bounded experiment and evidence appropriate to the claim.
- **Assess or explain benchmark evidence:** use the
  [anonymous case study](references/case-study.md) and
  [sanitized measurements](data/benchmark-runs.json). These are examples, not
  performance expectations for another repository. When preparing measurements
  for publication, also read the [final checklist](references/quality-contract.md)
  and the [anonymous-release guard](references/suite-optimization.md#if-releasing-an-anonymous-benchmark).

Use existing project runners, reports and note formats. A design note can live
in the task response or an existing work log; do not create a framework or new
file for every small change. Start from the user's actual time/correctness
constraints and the observation that would disprove the proposed improvement.

## Terms used in the references and data

- **Source context** runs in a normal source checkout. **Public/portable-copy
  context** runs in an exported or copied tree that may lack Git history or
  ignored files. The JSON key `public` names this execution context. The
  originating repository's published status is a separate question.
- **Outer admission** is the project's existing permission/resource check before
  launching work, such as reserving CPU slots or refusing service starts. Keep
  it active; this skill does not require a particular scheduler or guard tool.
- **Entry** is an executable suite/tooling launch; a **case ID** identifies a test.
  A **cohort** is the case population for one candidate. Anonymous labels retain
  these distinctions without revealing original names.

For non-isolated work, flakiness, risky boundaries, runner changes, and speed or
measurement work, apply the relevant [final checklist](references/quality-contract.md)
items once before reporting. Pure isolated edits report the behavior checked and
the result from the short path. Follow the project's authorization, isolation
and release rules.
This skill grants no extra execution or publication permission.
