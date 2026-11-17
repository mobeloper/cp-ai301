# Contribution: Add a synthetic successive-release fixture for composition tests
 #5295

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Eric Michel
**GitHub Username:** mobeloper
**Issue:** https://github.com/maziyarpanahi/openmed/issues/5295
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Wrote the issue-scout strategy and patched its search, ranking and rubric until a scan surfaced a real issue**

I wrote a search strategy of 15 queries across agentic-AI orgs and repos, with ranking filters and a rubric. The first scan graded nothing: the top 5 were unrelated repos, and I first blamed multi-owner `--owner` lists, but testing showed they filter correctly. The real cause was the example line inside the strategy's HTML comment, which began with `gh search issues` and ran as an unscoped query. Fixing that, then logging each query's command and count, adding an owner/repo allowlist filter, ranking `bug`-labelled issues ahead of `good first issue`, and clarifying two rubric checks (maintainer self-replies, maintainer issue-plus-PR batches) produced the first accepted candidates

**Unit 6: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I found this issue through my issue-scout, in two steps. The scout ran 61 `gh search issues` queries across the agentic-AI repos on my list, plus a second priority list I added (MONAI, nnUNet and openmed). In the scan on 2026-11-15, 6 of the 50 issues it graded were accepted, and I first picked openmed #5296 from those six and claimed it. A day later the maintainer answered my claim: another contributor had opened a PR (#5682) for exactly that scope while my request was waiting, and the maintainer offered me a sibling issue, #5295, "Add a synthetic successive-release fixture for composition tests". #5295 had shown up in my scan only as a sibling of #5296, so it had never been graded. I ran the scout on it directly on 2026-11-16 before claiming it. The verdict was accept, and the difficulty note said straightforward: the issue names every file to touch (`tests/fixtures/risk/successive_releases_v1.jsonl`, the `tests/fixtures/risk/README.md` paragraph and `tests/unit/risk/test_successive_release_fixture.py`), lists acceptance criteria and the exact `pytest` command, and says composition scoring is out of scope. The only preferred check it failed was has-repro, because a fixture task has no bug to reproduce. I did not choose against the scout, but its accept depends on rubric patches I made myself. An earlier scan rejected this kind of issue as "not a bug fix", and I then patched the rubric so a self-contained fixture or test gap with acceptance criteria counts, because that is the isolated fix I told it I wanted. So "consistent with my profile" is a fairer description of that accept than "independently confirmed".

It fits me because it is a small, self-contained Python fixture-and-test task in one directory that I can finish in about two weeks. It needs no GPU, runs on my MacBook, and is not a new feature. The repo is active: the owner committed on 2026-11-16, shipped v3.3.0 on 2026-11-13, and 13 outside PRs were merged in the last 90 days. The issue is open, has no assignee, no comments and no linked PR, and the maintainer offered to assign it to me. I hope to learn how a privacy-focused project builds synthetic test data (a keyed privacy-unit column, two releases of one population with different generalizations, k-anonymity per release), how a repo keeps fixtures, a README contract and tests in sync, and how to take a first PR from claim to merge under a maintainer's rules. My honest read of the scout is that it found a good issue but needed my hand checking. It accepted two openmed siblings (#5121 and #5097) that direct grading later rejected, because the owner opened 82 PRs on 2026-11-15 and 16 for roadmap issues filed on 2026-11-07, including ones labelled help wanted and good first issue. #5295 was filed in that same batch and passes `unclaimed` only through the exemption for issues that invite outside contributors, so the risk is real: #5296, its sibling, drew an outside PR within nine days. The scout also cannot see PRs by outsiders on sibling issues before grading, so it missed that #5296 was about to be taken. What made me take #5295 anyway is the maintainer's explicit offer to assign it, which the rubric has no way to read.

Claim comment: https://github.com/maziyarpanahi/openmed/issues/5295#issuecomment-6933573207 (posted 2026-11-16)

**Toolkit**

- Tool: issue-scout, with my search strategy, my rubric and my scope file. I ran a 61-query scan (500 results each, a quota of 100 issues per repo, priority repos first, batches of 10 graded until 5 were accepted), graded my six picks directly, and on 2026-11-16 graded #5295 directly using `gh` calls. In step 3 I patched the scope check: I replaced the strict `small-and-easy` row, which rejected an issue when its design was debated, with `scope-feasible`, which fails only when an issue is not one piece of work. I also added the difficulty note (straightforward, moderate, ambitious) under the checks table and logged the change in `scan-notes.md`.
- What it produced: scan 12 graded 50 issues and accepted 6. The direct grading of #5295 passed all 9 required checks, failed only the preferred has-repro, and returned accept with a straightforward note. The read-outs are at the end of `scan-output.md`.
- Difficulties: the scout missed the owner opening 82 PRs for same-day issues, and it cannot see outside PRs on sibling issues, so my first pick (#5296) was taken before I started. It also let feature issues through, could not see claim comments before grading, and a GitHub rate limit hit two queries at 500 results. Maintainers also appear as CONTRIBUTOR rather than Member, so `responds-to-issues` cannot rely on author association.
- What changed: I patched the rubric three ways: `is-a-bug-fix` accepts test and fixture gaps, `unclaimed` exempts issues that invite contributors, and `responds-to-issues` counts a maintainer merging an outside PR or a commit author replying. I wrote into `scan-notes.md` that the exemption should be dropped when the same owner opens PRs for several same-label issues, with a maintainer's explicit offer to assign as the override, and that sibling PRs must be searched by number. I would also add `type/feature` and `maintainer only` to the label filter.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
