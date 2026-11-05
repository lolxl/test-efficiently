# test-efficiently

A portable skill for writing useful tests and speeding up existing suites
without weakening behavior, coverage or evidence. Start with [SKILL.md](SKILL.md):
it routes small test edits directly to a short design path and sends risky
boundaries or performance work to the relevant controls.

## Install or use directly

Copy this folder as `test-efficiently`, including `SKILL.md`, `references/`,
`data/` and `LICENSE`, into one chosen destination. Include the optional
`agents/` folder to retain Codex UI metadata:

| Assistant | This project | All projects for this user |
|---|---|---|
| Codex | `<project>/.agents/skills/test-efficiently/` | `~/.agents/skills/test-efficiently/` |
| Claude Code | `<project>/.claude/skills/test-efficiently/` | `~/.claude/skills/test-efficiently/` |

Use a project destination for team-local instructions or the user destination
for personal reuse. If that folder already exists, review it before replacing
it. In Codex, invoke `$test-efficiently`; in Claude Code, invoke
`/test-efficiently`. Both also select relevant skills automatically.
See the official [Codex skill documentation](https://learn.chatgpt.com/docs/build-skills)
and [Claude Code skill documentation](https://code.claude.com/docs/en/skills)
for discovery and scope details.

The supplied `agents/openai.yaml` is optional Codex UI metadata: its display
name, short description and starter prompt help present and invoke the skill.
It is not an agent implementation or a requirement for other assistants.
You can also give an assistant [SKILL.md](SKILL.md) directly and make the linked
files available; no harness integration, scripts or service connection is required.

## Evidence and license

The [anonymous case study](references/case-study.md) and
[sanitized measurements](data/benchmark-runs.json) are self-contained examples.
The originating codebase is unpublished. Only authorized aggregate measurements
are provided: repository, issue, revision and internal test/entry identifiers
are omitted. Raw traces, source code and private records are not included.
Estimates are separate from observations, including a failed run and a green
full run that still exceeded its target. Each full configuration has one sample.

This skill and the supplied anonymous data are available under [MIT](LICENSE).
