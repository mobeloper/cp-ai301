# Contribution: Add a synthetic free-text quasi-identifier fixture set
 #5296

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Eric Michel
**GitHub Username:** mobeloper
**Issue:** https://github.com/maziyarpanahi/openmed/issues/5296
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

I found this issue by running my issue-scout on my own search strategy: 61 `gh search issues` queries across the agentic-AI repos on my list, plus a second priority list that I added (MONAI, nnUNet and openmed). In the scan I ran on 2026-11-15 that came to 12,318 unique issues, and 9,146 were left after my ranking filters. The scout graded 50 of them against my rubric and accepted 6. openmed #5296, "Add a synthetic free-text quasi-identifier fixture set", is one of those six and the one I wanted most. The scout's verdict was accept, and its difficulty note said straightforward: the issue names the files to touch (`tests/fixtures/risk/text_quasi_identifiers_v1.jsonl`, its README and a new unit test), gives acceptance criteria and a `pytest` command, and says the fixture and test are the whole job. I did not choose against the scout, but it only accepted this issue after I changed it. An earlier scan rejected #5296 as "not a bug fix", and I then patched the rubric so that a self-contained test or fixture gap with acceptance criteria counts, because that is the kind of isolated fix I told it I wanted. I also patched how it reads a maintainer-filed issue and how it checks that maintainers respond. The patches came from what I asked for, so a better word for the accept is "consistent with my profile", not "independently confirmed".

It fits me because it is a small, self-contained Python test and fixture task in one directory, so I can finish it in about two weeks. It needs no GPU, runs on my MacBook, and is not a new feature. The repo is active: the owner committed on 2026-11-15, shipped a release on 2026-11-13, and merged 25 of 28 outside PRs in the last 90 days. I hope to learn how a privacy-focused project builds synthetic test data (PII spans, offsets, canonical labels), how a repo keeps fixtures and tests in sync, and how to take a first PR through a maintainer's rules from claim to merge. My honest read of the scout is that it found a good issue but made two mistakes I only caught by checking by hand. It accepted two sibling openmed issues (#5121 and #5097) that my direct grading later rejected, because the owner had opened 82 PRs on 2026-11-15 and 16 for roadmap issues filed the same day, including ones labelled help wanted and good first issue. That is a real risk for #5296 too, because no PR exists for it yet. The scout also let features through its label filter, and it cannot see claim comments or open PRs before grading. I would change the rubric so the owner-filed exemption stops applying when the same owner has opened PRs for several issues with the same label, and I would add `type/feature` and `maintainer only` to the label filter.

Claim comment: https://github.com/maziyarpanahi/openmed/issues/5296#issuecomment-6910821189 (posted 2026-11-16)

**Toolkit**

- Tool: issue-scout, with my search strategy, my rubric and my scope file. I ran a full scan of 61 queries (500 results each, a quota of 100 issues per repo, priority repos first, batches of 10 graded until 5 were accepted), then graded my six accepted picks directly. In step 3 I patched the scope check: I replaced the strict `small-and-easy` row, which rejected an issue if design was debated, with `scope-feasible`, which fails only when an issue is not one piece of work. I also added the difficulty note (straightforward, moderate, ambitious) under the checks table, and logged the change in `scan-notes.md`.
- What it produced: scan 12 graded 50 issues and accepted 6, with openmed #5296 marked accept and straightforward; the direct grading then kept it as an accept and reversed two sibling openmed issues. The full read-out is in `scan-output.md`.
- Difficulties: the scout missed an owner opening 82 PRs for issues filed the same day. It also let feature issues through, could not see claim comments before grading, and a GitHub rate limit hit two queries at 500 results. Maintainers also appeared as CONTRIBUTOR instead of Member, so `responds-to-issues` could not rely on author association.
- What changed: I patched the rubric three ways: `is-a-bug-fix` accepts test and fixture gaps, `unclaimed` exempts issues that invite contributors, and `responds-to-issues` counts a maintainer merging an outside PR or a commit author replying. I raised the limits, added the priority lists and the batch rule, and wrote the owner-PR finding and a proposed rubric fix into `scan-notes.md`. I would still tighten the exemption and the label filter.

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
