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
**Pull Request:** https://github.com/maziyarpanahi/openmed/pull/5698

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

**Unit 6: Installed the upstream voice guard and wrote a repo-memory file for openmed**

I folded my three voice guides into one, added four upstream rules (claims, questions, pings, each with a wrong/right pair), and installed `check_voice.py` as a `PreToolUse` hook; a fresh session blocked `gh issue comment --help` with "no body the guard can read", and the guard log recorded it. I also wrote a 38-line `CLAUDE.local.md` for my openmed clone (CONTRIBUTING rules, the fixture and `kanon_report` subsystem, test commands, AI policy) and kept it out of the PR with `.git/info/exclude`. What fell short: my #5295 claim was posted before the guard was installed, so it never passed through it, and the hook only sees commands it can parse, so browser posts need the manual `--draft` check. Reading `AGENTS.md` for the memory file turned up a rule my tools would have broken, which forbids co-author trailers and tool names in commits and PRs, so I recorded it there and will leave the default attribution off in this repo.

**Unit 7: Ran the ship-gate on my real branch, patched two checks to follow the repo's AI rule, and opened PR #5698**

My gate manifest runs `ruff check`, `ruff format --check` and `pytest tests/unit/risk -q`, and the ship-gate then runs my pr-precheck on the branch, `plan.md`, `test_evidence.md` and `pr_draft.md`. It stopped three times before it passed on commit `8be36858a`: the full suite and type-check were not shown, `ai-disclosure` failed because the course asks for an AI statement that openmed's `AGENTS.md` forbids in PR text, and two ticked boxes contradicted my notes. I changed only the `ai-disclosure` check in my rubric and procedure, installed `uv` to run the repo's `make` targets in the locked environment, and later removed the matching AI rule from my voice guide, which blocked `gh pr create`. The gate then passed on all 8 checks and the PR opened as https://github.com/maziyarpanahi/openmed/pull/5698.

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

openmed can measure k-anonymity for one release of a table, but it has no small, known example of the same synthetic population released twice with different generalizations. Without one, the later work on composition across releases (intersecting two releases to isolate rows) has nothing to test against. Nothing is broken; a test fixture is missing.

**Expected Behavior**

`tests/fixtures/risk/` holds `successive_releases_v1.jsonl`: a `meta` row plus two releases of 20-40 synthetic rows each, sharing a keyed privacy-unit column. Running `kanon_report` on each release alone gives the k recorded in the fixture, the README documents the format and the expected intersection result, and the issue's pytest command passes.

**Current Behavior**

The fixture, its README paragraph and `tests/unit/risk/test_successive_release_fixture.py` do not exist, so the issue's pytest command stops with "file or directory not found". The existing suites (`test_regression_suites.py`, `test_kanon.py`) pass, 35 tests.

**Affected Components**

New: `tests/fixtures/risk/successive_releases_v1.jsonl` and `tests/unit/risk/test_successive_release_fixture.py`. Edited: `tests/fixtures/risk/README.md`. Read only: `openmed/risk/kanon.py` (`kanon_report`) and `openmed/risk/population.py` (privacy-unit wording). Composition scoring is out of scope.

**Toolkit**

- Tool: Claude Code with `gh` and file reads. I had it re-read issue #5295 (body, comments, linked PRs) and the risk code (`kanon.py`, `population.py`, `test_regression_suites.py`, the fixture README) so the four answers rest on the repo as it is today, then I checked them against my repro evidence.
- What it produced: the four answers above, plus a check that no PR or assignee exists for #5295 and a reading of how `population.py` names its privacy-unit column.
- Difficulties: the issue template is bug-shaped, so "Current Behavior" had to be read as "the files are missing", the same stretch I met in repro-check. The tool also reads fast, so I re-checked each file name against the issue text rather than trusting the summary.
- What changed: I replaced the open "privacy-unit column name" note in my Reproduction findings with what `population.py` actually does.

---

## Reproduction Process

Issue #5295 reports a gap, not a crash: `tests/fixtures/risk/` has QI uniqueness and negation-trap fixtures "but nothing with two releases". I reproduced the gap on current `master`.

**Environment Setup**

- macOS 26.5.1 (MacBook Pro), Python 3.13.9. `uv` is not installed on my machine, so I used the pip fallback from CONTRIBUTING.md.
- Repo: my fork mobeloper/openmed at `master`, commit `e8325586e1ce2da73e9e865f34bd0345bb64301a` (2026-11-16), identical to `upstream/master`. Package version openmed 3.3.0 (`openmed/__about__.py`). The issue's "Release: v4.3" footer is a roadmap milestone, not a version where the behavior was seen. The bug template's Model ID, PyTorch and Transformers fields do not apply to an offline fixture.
- Setup: `python3 -m venv .venv`, `.venv/bin/python -m pip install --upgrade pip`, `.venv/bin/python -m pip install -e ".[dev]"`. It installed with no errors, and `import openmed` reports 3.3.0.
- Challenges: none. The one difference from the documented flow is that `uv sync --frozen --extra dev` was unavailable, so I used the supported pip fallback.

**Steps to Reproduce**

From the repo root, on the commit above:

1. Check that the files the issue asks for are absent: `ls tests/fixtures/risk/successive_releases_v1.jsonl tests/unit/risk/test_successive_release_fixture.py`
2. Run the issue's validation command: `.venv/bin/python -m pytest tests/unit/risk/test_successive_release_fixture.py -q`
3. Search for any existing successive-release fixture: `grep -rIl "successive_release" . --exclude-dir=.venv --exclude-dir=.git` and `grep -n -i "successive\|intersect" tests/fixtures/risk/README.md`
4. Confirm the building blocks already work: run `kanon_report` on the closest existing fixture, and run `.venv/bin/python -m pytest tests/unit/risk/test_regression_suites.py tests/unit/risk/test_kanon.py -q`
5. Observed result: the fixture and test files are missing, so the issue's pytest command cannot run, while the existing k-anonymity code and suites work.

**Reproduction Evidence**

- **Commit showing reproduction:** none. A gap needs no code change to show it, and I have not pushed anything to my fork.
- **Screenshots/logs:**

  ```text
  $ ls tests/fixtures/risk/successive_releases_v1.jsonl tests/unit/risk/test_successive_release_fixture.py
  ls: tests/fixtures/risk/successive_releases_v1.jsonl: No such file or directory
  ls: tests/unit/risk/test_successive_release_fixture.py: No such file or directory

  $ .venv/bin/python -m pytest tests/unit/risk/test_successive_release_fixture.py -q
  ERROR: file or directory not found: tests/unit/risk/test_successive_release_fixture.py
  no tests ran in 0.48s

  $ .venv/bin/python -m pytest tests/unit/risk/test_regression_suites.py tests/unit/risk/test_kanon.py -q
  35 passed, 9 subtests passed in 11.96s

  kanon_report on quasi_identifier_uniqueness.jsonl (4 records; QIs age, city, visit_date):
  suite quasi_identifier_uniqueness records 4 k 2 class_count 2
  ```

- **My findings:** the issue's description matches the repo today. `tests/fixtures/risk/` holds only `dp_budget_scenarios.json`, `negation_traps.jsonl`, `quasi_identifier_uniqueness.jsonl` and `README.md`, none of them has a release field, no tracked file in the repo matches `successive_release`, and the README mentions only "release-gate fixtures", with no successive releases or intersection result. `kanon_report` measures one release at a time, so I do not expect to touch `openmed/risk/`: the missing pieces are the fixture, its README contract and the per-release test. I also checked how `openmed/risk/population.py` words "keyed privacy-unit": it takes a column name through `sample_privacy_unit` / `population_privacy_unit`, its tests use ID columns such as `sample_id`, and it rejects a privacy-unit column that is also a quasi-identifier. So my fixture's key column is an opaque ID column that stays out of every quasi-identifier list. No PR or assignee exists for #5295 as of 2026-11-17.

**Toolkit**

- Tool: repro-check, in live mode with my wild-scope file, rubric and voice guide. I ran it on my claim draft first (claim-only), then on the claim and `repro-draft.md` together as a full package.
- What it produced: the first run refused to grade, because the scope says it only grades issues my scout has graded and #5295 had never been graded. After I ran the scout on it, the claim-only run returned accept, and the full package returned accept with all 7 checks passing, with two suggested edits that I applied here: say why "v4.3" is not the affected version, and drop a mention of a local file a reader cannot see.
- Difficulties: the rubric's checks assume a bug. "Quotes the error message from the issue body" and "actual behaviour" fit a fixture gap awkwardly, so I had to read them as "quotes the gap" and "shows the files are missing". Its environment check also could not tell a roadmap release tag from an affected version.
- What changed: I ran the scout on #5295 before grading, as the scope requires, and added the environment note above. I would add a line to the rubric for gap and test-fixture issues saying what counts as expected and actual behaviour for them.

---

## Solution Approach

**Analysis**

The cause is a coverage gap, not a defect in code. `kanon_report` in `openmed/risk/kanon.py` takes one table, builds equivalence classes from the quasi-identifiers (`_equivalence_key`) and returns `k` (smallest class size) and `class_count`. It has no notion of a release, so it measures one release at a time, and my repro showed it works (35 tests pass). What is missing sits in test data: `tests/fixtures/risk/` has `quasi_identifier_uniqueness.jsonl` (one table, 4 records) and `negation_traps.jsonl`, and neither has a release field or a privacy-unit key. `tests/unit/risk/test_regression_suites.py` only loads those two files, and `tests/fixtures/risk/README.md` documents only their formats. So later composition work has no committed example where two releases are each safe alone but isolate rows when intersected.

**Proposed Solution**

Add exactly what issue #5295 asks for and change no library code.

- `tests/fixtures/risk/successive_releases_v1.jsonl`: a `meta` row (same shape as the existing fixtures, with the committed per-release k in `expected`) and two releases of one synthetic population, 20-40 rows each, plus a keyed privacy-unit column (`privacy_unit_id`) shared by both releases. The two releases generalize the quasi-identifiers differently. Every row is marked `synthetic: true` and holds invented values only: no real names, dates or places.
- `tests/unit/risk/test_successive_release_fixture.py` (about 40 lines): loads the fixture, checks every row is marked synthetic, runs `kanon_report` on each release alone with explicit `quasi_identifiers` (so the ID column stays out), and asserts the committed k for each.
- `tests/fixtures/risk/README.md`: a paragraph describing the format and the expected intersection result for later composition tests.

I am not changing `openmed/risk/` (composition scoring is out of scope per the issue), the existing fixtures, or the existing tests, and I am not adding anything the issue does not list: no extra test helpers, no CHANGELOG entry, no docs beyond the README paragraph.

**Implementation Plan**

1. Branch from the latest `master`: `test/issue-5295-successive-release-fixture`.
2. Design the population on paper: about 24 units, each with a `privacy_unit_id`. Release 1 and release 2 generalize the quasi-identifiers differently, so each release alone has k of at least 2 and the intersection leaves at least one unit alone in its class. Confirm the k values and the intersection with `kanon_report` and a throwaway script I do not commit.
3. Write the fixture with the per-release k in `meta.expected`.
4. Write the test file.
5. Add the README paragraph, including the expected intersection result.
6. Run `.venv/bin/python -m pytest tests/unit/risk/test_successive_release_fixture.py -q`, then my manifest checks (`ruff check`, `ruff format --check`, `pytest tests/unit/risk -q`) and `python scripts/release/check_repo_policy.py --commits origin/master HEAD`.
7. Commit with imperative `test:` and `docs:` messages, then open one PR with `Closes #5295`, following `.github/PULL_REQUEST_TEMPLATE.md` and listing the exact validation commands.

**Deviations from the plan (2026-11-17):** the scope did not change, and I made no change to the files or the approach. Three small differences, each recorded here so the diff holds no surprise:

- The test file is 54 lines, not "about 40". I wrote the file loader and the per-release k check as two readable test functions, one of them parametrized over both releases, so the file runs 3 test cases.
- The `meta` row carries two fields the plan did not list, `quasi_identifiers` and `privacy_unit_column`, so the test reads the column names from the fixture and does not hard-code them. It still has the `expected` block the plan called for.
- The README paragraph is 24 lines. It repeats the row format in the same bullet style as the existing suites and gives the expected intersection result with the three isolated unit IDs, which the plan said it would document.

**Toolkit**

- Tool: my plan-check skill (`rubric.md`, `procedure.md`, scope and voice guide), run in Claude Code on `plan.md` and `plan-comment.md` for issue #5295, after I checked the plan against the issue and the sibling PR #5682.
- What it produced: `accept`, with all five required checks (diagnosis, scope, changes, test, comms) passing, and a voice-guide note that the guide's AI-use rule would conflict with `AGENTS.md`.
- Difficulties: the rubric assumes a bug, so it read "cause" as "gap" for a missing fixture, and it found no posted repro comment, so it graded diagnosis from the repro facts quoted in the plan. The voice-guide conflict it flagged did reach the ship-gate later.
- What changed: I cut anything beyond the issue's three deliverables from the plan before the check. After the build I recorded three small deviations in `plan.md` and the story, and later resolved the conflict by patching `ai-disclosure` and the voice guide.

---

## Testing Strategy

The fix adds test data, so the proof is the issue's own acceptance checks, run on `master` before and on my branch after. The full output is in `test_evidence.md`.

1. **Reproduction steps, before and after.** On `master` (`e8325586e`), `ls` reports the fixture and test files missing, `.venv/bin/python -m pytest tests/unit/risk/test_successive_release_fixture.py -q` stops with "ERROR: file or directory not found", and the README has no match for "successive" or "intersect". On the branch (`8be36858a`) the files exist, the same pytest command gives `3 passed in 0.13s`, and the README has the new paragraph. The 35 existing tests in `test_regression_suites.py` and `test_kanon.py` pass both before and after.
2. **Committed values, measured.** `kanon_report` on each release alone gives k 3 (release 1, eight classes of 3) and k 2 (release 2, class sizes 3, 3, 3, 3, 3, 3, 2, 4), matching `meta.expected`. A throwaway join on `privacy_unit_id`, not committed, gives intersection k 1 with `pu-03`, `pu-04` and `pu-21` isolated, matching the README.
3. **The test fails when the data is wrong.** I moved one release-1 row into a one-row area and the k check failed with `assert 1 == 3`. With the fixture removed, all 3 test cases failed. I restored the file after both runs.
4. **The repo's checks, in the environment CONTRIBUTING asks for.** I installed `uv` and ran `uv sync --frozen --extra dev` into a separate `.venv-uv`, so the versions come from `uv.lock`. There, `make format` leaves all 3623 files unchanged, `make lint` passes (ruff and `check_effect_paths.py`), `make format-check` passes, and `make type-check` gives "Success: no issues found in 4 source files". `pytest tests/unit/risk -q` gives 543 passed and 308 subtests passed, and `check_repo_policy.py --commits origin/master HEAD` passes for 3 commits.
5. **The full suite, and what fails without my change.** `pytest tests/ -q` gives `6 failed, 35040 passed, 405 skipped` in 13m36s. The 6 failures are 2 tests outside `tests/unit/risk`: `test_malformed_iscii_preserves_clinical_text_and_identifier_spans` in `tests/fuzz/test_deidentify_fuzz.py` and the `[video]` case of `test_local_media_is_scrubbed_and_sent_only_to_loopback` in `tests/unit/structured/test_decision_server.py`. On `master` in the same environment the same 6 failed, so they predate my commits, and I left them alone because they are outside the issue.

**Toolkit**

- Tool: Claude Code, running the commands from my reproduction steps and the repo's own checks through the shell on `master` and then on my branch: `pytest`, `ruff`, `mypy` through the `make` targets with `uv`, and the repo's `check_repo_policy.py`, plus small throwaway scripts (not committed) for the `kanon_report` values and the intersection join.
- What it produced: `test_evidence.md`, with the before and after output of my reproduction steps, the measured k values, a failing run on altered data, the manifest checks, and the full suite and `make` targets in the locked environment.
- Difficulties: the reproduction "before" state is a gap, so the evidence is a missing file, and a repo-wide grep on `master` matched my own untracked local files. My first checks ran in a pip-installed `.venv` with newer packages than `uv.lock`, so they showed 17 full-suite failures and a mypy crash that were not real problems with the repo; the locked environment shows 6 failures, which also fail on `master`. While checking mypy I switched branches during a running test, which spoiled that run, so I stopped it and reran it alone.
- What changed: I installed `uv` and reran everything with `uv sync --frozen --extra dev`, as CONTRIBUTING asks, and used that output in `test_evidence.md` and the PR description. I would have used the locked environment from the start. I also kept the intersection check out of the committed test, because composition scoring is out of scope.

---

## Implementation Notes

<!-- 
A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included. 
-->

**2026-11-17**

Branched from `master` at `e8325586e` (same as `upstream/master`) as `test/issue-5295-successive-release-fixture`, which follows CONTRIBUTING's `<type>/issue-<number>-<slug>` rule. I designed the population on paper before writing any file: 24 privacy units, release 1 with ten-year age bands and a town, release 2 with five-year age bands and a district. I arranged the units so release 1 has k 3 (eight classes of 3), release 2 has k 2 (classes of 3, 3, 3, 3, 3, 3, 2 and 4), and joining the two isolates `pu-03`, `pu-04` and `pu-21`. A small script wrote the JSONL from that table. `kanon_report` and a throwaway join script, neither committed, gave the same k values and the same three isolated units as my paper design.

**2026-11-17**

Committed in three steps: the fixture, the test, then the README paragraph. The test has two functions, one checking that every row is synthetic and each release has 20-40 rows, and one parametrized over both releases that checks each release alone gives the committed k from `meta.expected`, passing explicit `quasi_identifiers` so `privacy_unit_id` stays out. To check that it fails without the fix, I removed the fixture, and all 3 test cases failed with a missing-file error; I then changed one release-1 row into a singleton class and got `assert 1 == 3`. Restored, the file passes. Checks on the branch: `ruff check` and `ruff format --check` pass, `pytest tests/unit/risk -q` gives 543 passed, and `check_repo_policy.py` passes for the 3 commits. The diff is 3 files and 127 added lines, and nothing under `openmed/risk/` changed. One small difference from the plan: the test file is 54 lines, not the "about 40" I wrote, because I kept the loader and the parametrized k check readable. It stays inside the plan's scope, so I did not change `plan.md`.

**2026-11-17**

Ran the ship-gate on the branch, and it stopped three times before it passed. First, `repo-checks` failed because CONTRIBUTING asks for `pytest tests/ -q` and `make type-check` and I had shown only the risk tests. Second, `ai-disclosure` failed because the course asks for an AI-use statement, while the repo's `AGENTS.md` forbids tool names, co-author trailers and generation footers in PR text. I decided the repo rule outranks the course ask, so I changed only that check in my pr-precheck `rubric.md` and `procedure.md`, as the course allows: it now also passes when `AGENTS.md` forbids attribution and the description quotes that rule in one line with no attribution. Third, `template-complete` failed because I had ticked two boxes my own notes contradicted ("tests pass" beside the failing suite, and "ran make" beside "I did not run make"), so I unticked both and explained them.

**2026-11-17**

Installed `uv` with Homebrew and built the locked environment in `.venv-uv`, so my working `.venv` stayed untouched. `make format`, `make lint`, `make format-check` and `make type-check` all pass and `make format` changed no files. The full suite gives 6 failures in 2 unrelated tests, the same 6 on `master`, and I did not fix them. The ship-gate then passed on all 8 checks. I did not change the fixture, the test, the README, or my scope in this step.

**Toolkit**

- Tool: Claude Code, with `git`, `uv` and the repo's `make` targets, `pytest`, `ruff` and `mypy`, my ship-gate and pr-precheck, and small throwaway scripts (not committed). I used it to design the population from a table, generate the JSONL from it, write the test and the README paragraph, make the commits, run the checks and draft the PR description, then I read the diff.
- What it produced: the fixture (49 lines), the test (54 lines), the README paragraph (24 lines), three commits with `test:` and `docs:` messages, `test_evidence.md`, `pr_draft.md`, a patched `ai-disclosure` check, a passing ship-gate run, and four dated entries in this log.
- Difficulties: the first edit to this story failed because the file had not been read in that session, and a later command changed the file under me, so I re-read it before editing. The design needed one correction before it worked: my first layout gave release 2 no class of size 2, so I regrouped two release-1 classes into one district. The repo's rule against tool names and co-author trailers meant I left the default commit attribution off, which the policy script confirmed, and it conflicted with the course's AI-disclosure check.
- What changed: I wrote the k values and the isolated unit IDs into `meta.expected` and the README, so the test and the docs share one set of numbers. I recorded three small deviations from my plan under the Implementation Plan (test length, two extra `meta` fields, README length), and I changed neither `plan.md` nor the scope. I patched `ai-disclosure` in my rubric and procedure, and switched to the locked `uv` environment.

---

## Pull Request

**PR Link:** https://github.com/maziyarpanahi/openmed/pull/5698

**PR Description:** Title: `test: add successive-release fixture and per-release kanon_report k test`, from `mobeloper:test/issue-5295-successive-release-fixture` into `master`, with `Fixes #5295`. The PR adds a synthetic fixture with two releases of one population of 24 privacy units, generalized differently (ten-year age bands and a town, then five-year age bands and a district), and a test that runs `kanon_report` on each release alone. Release 1 has k 3 and release 2 has k 2, so neither isolates a unit alone, but joining them on `privacy_unit_id` isolates `pu-03`, `pu-04` and `pu-21` (k 1). The fixture's `meta.expected` and a paragraph in `tests/fixtures/risk/README.md` record those values for later composition tests. The diff is 3 files and 127 added lines in three commits, and nothing under `openmed/risk/` changes. The description pastes the before and after output (the files and test missing on `master`, then `3 passed`, and 543 passed in `tests/unit/risk`), the `make format`, `lint`, `format-check` and `type-check` results, and a failing run on altered data (`assert 1 == 3`). Notes for Reviewers disclose that the full suite shows 6 failures in 2 unrelated tests that fail the same way on `master`, that composition scoring is left out on purpose, and the three small differences from my plan. The description has no AI-use statement; it quotes the `AGENTS.md` rule that forbids assistant and tool attribution in PR text.

**Maintainer Feedback:**

- 2026-11-17: none yet.

**Status:** Awaiting review

**Toolkit**

- Tool: the ship-gate (`check_gate.py`) with my gate manifest, which runs `ruff check`, `ruff format --check` and `pytest tests/unit/risk -q` and then my pr-precheck on the branch, `plan.md`, `test_evidence.md` and `pr_draft.md`; my voice guard hook, which checked the description when `gh pr create` ran; and Claude Code, which pushed the branch and ran `gh pr create` with `--body-file pr_draft.md`. I read the PR on GitHub afterwards to check the base repo, the branches, the title and the body.
- What it produced: `ship-gate: PASS` with all three manifest checks and all 8 pr-precheck checks passing, on commit `8be36858a`, and PR #5698 on `maziyarpanahi/openmed`, which targets `master` and has `Fixes #5295` and no attribution footer.
- Difficulties: the gate stopped three times before passing (full suite and type-check not shown, `ai-disclosure`, and two ticked boxes my notes contradicted), and the first PR attempt was stopped by the voice guard, whose rule asks for an AI-use statement that `AGENTS.md` forbids in PR text. A second attempt run from the wrong folder was stopped by the gate as "on main", and the first nested run needed an approval for `gh pr create`, so I ran it again with only that command allowed.
- What changed: I patched `ai-disclosure` in my pr-precheck rubric and procedure to accept a quoted repo rule in place of a statement, and removed the AI-use rule from `voice-guide.md`, then copied it into the three `tools/` folders so the copies match. Both changes follow `AGENTS.md`, so the course's AI disclosure lives in this story and not in the PR. I would read the repo's agent rules before writing the voice guide.

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
