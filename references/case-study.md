# Case study: a process-heavy test suite

Boundary: an anonymized improvement arc from an unpublished codebase. Only
methods and owner-authorized aggregate measurements are disclosed. Project,
issue, revision and internal entry identifiers are omitted. The
[measurements](../data/benchmark-runs.json) are supplied case-study evidence;
the private source and raw records are not part of this repository.

## Terms

Source context runs in a normal source checkout. Public/portable-copy context
runs in an exported or copied tree that may lack Git history or ignored files.
The JSON key `public` names this execution context. The originating repository's
published status is a separate question. Outer admission is the project's
existing permission and resource check before launching work.

## The improvement arc

1. **Separate cleanup from runtime evidence.** The initial review found a large,
   complicated codebase. File size and possible helper reduction did not prove
   that behavior could be retired. Verified obsolete/duplicate behavior was
   removed first; footprint reduction was not presented as a timing improvement.
2. **Make a fixture optimization preserve the question.** Four matrix tests kept
   their 192 cells, assertions and isolation controls while redundant per-cell
   preparation was removed. Three interleaved pairs, six runs, gave baseline and
   candidate medians of 104.244 and 50.687 seconds; median paired reduction was
   51.47%. Breaking reset produced real failures. This supported a local result,
   not a claim that the complete suite became twice as fast.
3. **Inventory every launch.** Reported historical aggregates identified a tooling
   entry taking 1,345.083 seconds. The sample count and statistic type are
   unavailable in the released material, and raw samples were not released.
   Wrapper/leaf review found duplicate aggregate work.
   A conservative pilot preserved standalone behavior while reducing that
   version's tooling inventory from 53 to 45 entries. These historical counts are
   distinct from the later 48-entry inventory; the reason for that version-to-version
   difference is not supplied. A roughly 56–57 second
   paired subgroup saving was not extrapolated to the complete suite.
4. **Profile before caching or rewriting.** A costly CLI test led to
   a bounded hot-path change. The text report calls that test a 19-subcase
   CLI test. That subcase count is narrative-only, text-report-only, and has no JSON field.
   Seven paired CLI measurements improved in every
   pair, with a 25.4% median paired reduction. That pair count and that percent
   match the JSON. Differential checks and a complete
   115.41-minute validation passed. That full run was not the paired benchmark.
   Loader caching accounted for only about 0.339% of seven profiled intervals.
   Those seven profiled intervals are narrative-only and text-report-only.
   The JSON field `pairs` is 7 for this comparison, and that field counts pairs.
   A path-hoist pilot had mixed results and was closed. Fewer Git calls also
   revealed a changed fixture data path: less work had not proved equivalent work.
5. **Include fixture lifecycle in weights.** A broad case-only scheduling model
   promising roughly 182 seconds was withdrawn when fixture re-entry risks
   surfaced. A conservative revision retained fallback behavior for uncertain
   modules. An integrated full first failed; repaired changes later passed in
   3,399.379 seconds. The 56.66-minute figure is narrative-only and
   text-report-only. It is 3,399.379 divided by 60, and it has no JSON field.
   That observation had no same-content control and did not isolate a causal
   scheduling benefit.
6. **Change geometry with ownership and isolation controls.** The later
   experiment overlapped complete source and public/portable-copy contexts with
   six workers each, reaped both, then executed tooling in isolated fragments
   under normal outer admission (the existing resource/permission check before
   launching work). Build isolation, environment restoration,
   provenance, refusal behavior and cancellation ownership had their own controls.
7. **Retain a red full run.** Candidate A ran all 9,005 IDs in each context and
   48 tooling entries in two fragments. Two source test cases failed; complete
   elapsed was approximately 33m58s. Quiet replays passed, which neither erased
   the failure nor proved that load was its sole cause.
8. **Investigate fixture assumptions and repair the oracle.** A fixture changed
   shared standard-library sleep and could perturb unrelated subprocess timeout
   waits. The repair isolated owned clock bindings and retained a real timeout
   control; a second clock consumer needed its own mutation-sensitive control. A drain fixture kept its state,
   polling and deadline assertions using a logical clock. This proved a logical
   contract, not real latency. The original failure's exact stack was not recovered,
   so the exact cause of those two case failures remains unproved.
9. **Measure the complete candidate.** Candidate B added nine cases and retained
   all earlier 9,005 IDs. Both contexts accounted for 9,014 IDs; all 48 tooling
   entries ran freshly in four fragments. Anchored cleanup completed.
   `guard_calls` in the JSON for this run is 0. The author reports no added
   skips among the earlier 9,005 IDs in both the source context and the
   public/portable-copy context. The JSON field `new_skips_among_previous_ids`
   stores that reported count, 0. The released aggregates omit the per-ID
   mapping, so a reader cannot recompute that 0 from those totals.
   The public/portable-copy case-level skip total rose from
   79 to 82. Given that reported retained-ID result, at least three of the nine
   added IDs are skipped in that context. The per-ID mapping and any retained
   skip-to-pass transitions are absent from the released aggregates. Complete
   conservative elapsed was
   **1,817.171 seconds: 30m17.171s, above the 30-minute target**. 1,817.171 and
   30m17.171s are rounded display values sourced from the JSON. This was one green
   observation, not a stable latency guarantee.

## Keep observations and estimates apart

| Observation | Case IDs per context | Tooling entries / fragments | Complete elapsed | Functional result | Under 30m |
|---|---:|---:|---:|---|---|
| Candidate A | 9,005 | 48 / 2 | Approximately 33m58s; exact clocks in JSON | Failed: two source cases | No |
| Candidate B | 9,014 | 48 / 4 | 30m17.171s | Passed | No |

Anonymous cohorts identify populations without exposing case identities.

Each full configuration has **n=1**. Both used normal builds, six workers per
concurrent first-stage context, twelve declared CPU slots, normal outer admission and
refusing/logging service-launch protection. Other host tests, probes and large
builds were held; another editing worker could be paused with no CPU use.
Cache and thermal conditions were not controlled as a paired experiment. These
cross-version observations do not isolate the cause of a whole-suite speedup.

Replaying older costs suggested an ideal four-way total near 29m53s, only
6.8 seconds below target. 6.8 seconds is a rounded display value sourced from
the JSON. This was arithmetic, not a four-way execution. Changed
first-stage time and higher tooling costs consumed that small margin.

In Candidate B, tooling entries totaled 1,021.632 seconds. The four-worker
average-load lower bound was 255.408 seconds. The observed tooling stage was
257.152 seconds. At those same costs, reassigning indivisible entries offers
less than two seconds, insufficient to recover the 17-second gap. 1,021.632,
255.408, 257.152, and the 17-second gap are rounded display values sourced
from the JSON. This is a one-sample bound, not a prediction under another
placement or environment.

Source workers differed in elapsed time, but case-only rebalancing had already
shown fixture and context risks. The next justified step was a bounded,
fixture-aware experiment, not an automatic weight rewrite or an unchanged retry.

## Transferable lessons

- Choose the smallest observation that distinguishes correct from incorrect work.
- Reuse immutable preparation only with proven mutable-state independence.
- Profile an expensive boundary before promoting a cache or broad rewrite.
- Prove wrapper/leaf equivalence before deduplicating launches.
- Include fixture lifecycle and the critical path in scheduling decisions.
- Keep local benchmarks, complete validation and causal claims separate.
- Preserve failed and interrupted observations with terminal and cleanup evidence.
- Seek realistic margin: green correctness can still miss the performance goal.

## Limits of the earlier summaries

The early inventory JSON block re-encodes the existing narrative. It is not
independent corroboration. The released material does not supply the sample count
or statistic type, whether a single observation, a maximum, a median, or another
summary, behind 1,345.083 seconds. It also omits raw observations and the reason
the historical inventory later differs from 45 to 48 entries. Unknown does not
mean no samples existed.

The 115.41-minute validation, roughly 0.339% profile share, withdrawn roughly
182-second estimate, and 3,399.379-second validation are narrative-only reported
historical figures. The 19-subcase count, the seven profiled intervals, and the
56.66-minute figure for 3,399.379 divided by 60 are narrative-only and
text-report-only, and they have no JSON field. No structured measurements or raw records are released
for those narrative figures. Preserve their observation and estimate distinctions
without treating these figures as independently verified evidence. The 25.4%
median paired reduction and the 7 pairs remain the JSON fields for that CLI
comparison. The earlier local paired aggregates in JSON are also reported
summaries, and the raw paired samples are not released.
Opaque entry labels permit arithmetic/partition inspection; they do not reveal
the original tools or make the unpublished suite reproducible.
