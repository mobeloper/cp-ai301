# I built oss-agent, a human-gated, eval-backed pipeline for open source contributions, and used it to open openmed PR #5698

## Overview

oss-agent is a chain of Claude Code checkers I built to take a first open source contribution from issue to pull request without trusting the model unsupervised. The problem was that an AI agent will happily post confident work to strangers, so I wrote the rules that grade each stage (the rubrics, the search strategy, the voice guide, the ship manifest and the monitor playbook) and put a human gate between stages. I used it to open [openmed PR #5698](https://github.com/maziyarpanahi/openmed/pull/5698), which fixes issue #5295 with a synthetic fixture and a test. The PR is open today, passing 3 checks and waiting for its first review.

## What happened

### The pipeline

Each stage is a Claude Code skill with a rubric I wrote. The code is the harness, and the judgment is mine.

| Stage | Tool | Gate |
|---|---|---|
| Find an issue | issue-scout | ranks candidates, grades the top ones against my rubric and search strategy |
| Choose | issue-select | accept or reject against the rubric |
| Reproduce | repro-check | grades my claim and repro before I post |
| Plan | plan-check | grades the plan and plan comment |
| Submit | pr-precheck, then ship-gate | blocks `gh pr create` until the PR package passes |
| Speak | voice-guard | a `PreToolUse` hook that checks comments and PR text against my voice guide |
| Wait | pr-monitor | reads the PR and drafts one next move; never posts |

"Human-gated" is literal. The hooks stop commands, the monitor writes drafts to a file, and I post by hand. "Eval-backed" applies to the four graders from units 1 to 4, each run against 20 frozen, hand-labelled packages with a bar of 18 of 20. The later tools (scout, ship-gate, guard, monitor) are backed by logs on my real PR, not by an eval set.

### The graders failed first

plan-check scored 18/20 on its first run. I read the two misses anyway: `pkg-14` was an honest plan my rubric rejected for lacking exact file names and a new test, and `pkg-20` omitted an AI disclosure its repo required, which the rubric had no check for. I loosened the scope and test checks and added a required `comms` check read against the thread and the contribution policy. The run went to 20/20. issue-select went from 14/20 to 18/20 the same way, though it never cleared its category floor because it still misses its one `policy` case.

### Finding an issue

My first scan graded nothing. I blamed multi-owner `--owner` lists, but testing showed they filter correctly. The cause was the example line inside my strategy's HTML comment, which began with `gh search issues` and ran as an unscoped query. I removed it, logged each query's command and count, and added an owner/repo allowlist. A later scan of 61 queries graded 50 issues and accepted 6. I claimed #5296 from those six. The maintainer answered a day later: another contributor's PR #5682 already covered that scope, and they offered a sibling issue, #5295. The scout cannot see outside PRs on sibling issues, so I wrote into `scan-notes.md` that sibling PRs must be searched by number, and graded #5295 directly before claiming it.

### Building the contribution

openmed's `kanon_report` measures k-anonymity, and #5295 asked for a fixture for later composition tests. I designed 24 privacy units released twice: release 1 uses ten-year age bands and a town, release 2 uses five-year bands and a district. Each release alone looks safe, with k 3 and k 2. Joined on `privacy_unit_id`, they isolate `pu-03`, `pu-04` and `pu-21` (k 1). The test runs `kanon_report` on each release and compares it with the k committed in the fixture's `meta.expected`:

```python
assert report["k"] == meta["expected"]["k"][release]
```

To prove the test checks something, I altered one fixture row and ran it:

```
E       assert 1 == 3
FAILED tests/unit/risk/test_successive_release_fixture.py::test_each_release_alone_meets_its_committed_k[release_1]
1 failed, 2 passed in 0.13s
```

With the fixture file removed, all 3 cases failed with a missing-file error. Restored, `3 passed`, and `tests/unit/risk` gives `543 passed, 308 subtests passed`. The full suite has 6 failures in 2 unrelated tests that fail the same way on `master`, and I disclosed that in the PR.

### The gate stopped me, and I changed my own rule

My ship-gate stopped three times before it passed. The telling stop was `ai-disclosure`. My course rubric required an AI-use statement in the PR, but openmed's `AGENTS.md` forbids assistant, tool and vendor names, co-author trailers and generation footers in PR text. First gate run:

```
- ai-disclosure [fail]: Notes for Reviewers has no statement of how AI was used; the course requires disclosure in the author's own words.
```

My voice guard blocked `gh pr create` for the same reason (`guard-log.md`, 2026-11-17 21:00: `stop: rule broken: "In Notes for Reviewers I state in my own words how I used AI..."`). The repo's rule outranks mine. I changed only that check, so it accepts a quoted repo rule in place of a statement, and removed the matching rule from my voice guide. Second version, from `gate-log.md`:

```
- ai-disclosure [pass]: Repo-rule exception: Notes quotes the AGENTS.md rule ... and carries no AI attribution.
```

The other two stops were the full suite and type-check not shown, and two ticked boxes my notes contradicted ("New and existing unit tests pass locally" ticked beside `17 failed, 34943 passed`).

### What landed

Three things landed because of this work. First, my claim on #5296 got a maintainer reply that offered me #5295, which became the PR. Second, after the gate passed all 8 checks on commit `8be36858a`, the guard passed `gh pr create` (`2026-11-17 21:05 [hook] pass`) and PR #5698 opened. Third, the pr-monitor read the PR (0 comments, 3 checks passing) and drafted "No nudge yet. Wait", citing my playbook's 4-day rule. I wrote that rule from nine merged outside-contributor PRs (#5470–#5474, #5499, #5560, #5585, #5669, #5670), which all got a first response within about a day, so I posted nothing.

## Limitations and what's next

- The PR is unreviewed and unmerged, so I can't yet say how a maintainer will receive the diff, and nothing in this write-up shows a review comment.
- A fixture task has no bug to reproduce, so repro-check graded it low on its one preferred check.
- The scout is blind to sibling PRs and claim comments, and my accept of #5295 depends on a rubric patch I made myself, so "consistent with my profile" is a fairer description than "independently confirmed".
- The monitor counted quiet days from the last activity without saying whether it was mine, so I count them myself.
- Next I would give the scout an eval set like the graders have, search sibling PRs by number before claiming, and read a repo's `CONTRIBUTING.md` and `AGENTS.md` before writing the voice guide, because a gate that works against the repo is a bug I wrote.
