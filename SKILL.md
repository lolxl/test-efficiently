---
name: test-efficiently
description: Design useful tests, repair costly fixtures, and speed up existing suites while preserving behavior and trustworthy evidence. Use when writing tests, investigating test cost or flakiness, or planning a suite-speed improvement; ordinary edits without a testing question do not need this workflow.
license: MIT
---

# Test Efficiently

This standalone skill folder, including its sanitized data, is covered by
[its MIT license](LICENSE). It does not relicense other repository content.

Improve the time to a trustworthy answer, including implementation, diagnosis,
review and validation cost. A shorter command is useful only when it answers
the same relevant question.

Read [the quality contract](references/quality-contract.md), then choose the
path that matches the request:

- **Write or repair tests:** [test design](references/test-design.md). Choose the
  cheapest layer that can observe the defect, preserve an appropriate real
  boundary check, and make fixtures sensitive to the failure they claim to catch.
- **Optimize an existing codebase:** [suite optimization](references/suite-optimization.md).
  Inventory and measure before changing setup, discovery, weights or concurrency.
- **Assess evidence or explain the method:** [an anonymized case study](references/case-study.md)
  and its [sanitized measurements](data/benchmark-runs.json). These are examples,
  not performance expectations for another repository.

Start from the user's actual time and correctness constraints. Record the
behavior at risk, the proposed test scope, the expensive boundary, and the
observation that would disprove the proposed improvement. Use existing project
runners, timing exports, inventory checks and mutation facilities where they
exist; adapt to their formats rather than building a second test framework.

Keep three questions separate:

1. Did the behavior remain correct, including refusal and cleanup paths?
2. Did the intended population actually run with the expected outcome semantics?
3. What elapsed-time claim does this experiment support?

Finish with the implemented change, the behavioral and population evidence,
the measured scope and sample size, total validation cost, and remaining limits.
Keep failed and interrupted attempts visible. Follow the project's existing
authorization, isolation and release rules; this skill grants no extra execution
or publication permission.
