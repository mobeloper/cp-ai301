plan-check: issue #5295
https://github.com/maziyarpanahi/openmed/issues/5295 

Evidence read (per procedure.md):
- Issue: the title is "Add a synthetic successive-release fixture for composition tests". The task is a fixture, a README paragraph and a per-release test, and composition scoring is out of scope.
- Thread: the only comment is your claim, posted 2026-11-17. There is no assignee, no maintainer direction beyond the issue body, and no open PR mentioning #5295.
- Repo policy: CONTRIBUTING.md, the PR template and MAINTAINERS.md contain no rule requiring contributors to disclose AI use. AGENTS.md forbids tool names and attribution in repo metadata.

Checks
- diagnosis (pass): the plan names the cause as a coverage gap. kanon_report measures one table at a time, and no fixture under tests/fixtures/risk/ has a release field or a privacy-unit key. It cites _equivalence_key, the two existing fixtures, test_regression_suites.py and "35 tests pass", which matches the Reproduction section and the issue title.
- scope (pass): it names the new fixture, the new test and the README. It lists what it won't touch (openmed/risk/, the existing fixtures and tests, CHANGELOG, extra helpers) and gives the reason.
- changes (pass): each file has a stated purpose, and the two added files are exactly what the issue lists. The work is about 24 fixture rows, about 40 lines of test and one README paragraph, far under 500 lines.
- test (pass): it adds tests/unit/risk/test_successive_release_fixture.py, which runs kanon_report per release and asserts the committed k. On unfixed master that command stops with "file or directory not found".
- comms (pass): the comment follows the issue's boundary ("fixture and per-release test only"). It duplicates no claimed effort, and no disclosure is required for a comment here.

Voice-guide notes: the draft breaks no rule: no timeline, no praise, and the cause is stated only as far as the repro shows. One conflict: the guide's PR-description rule says to state how you used AI, while AGENTS.md forbids tool names. You've chosen to leave it out, so expect that rule, and possibly the ship-gate's pr-precheck, to flag the PR description later.

Result: accept. All five required checks pass.
- diagnosis: the cause is a coverage gap. kanon_report measures one table at a time, and no risk fixture has a release field or a privacy-unit key.
- scope: it names the fixture, the test and the README, and excludes openmed/risk/, the existing fixtures and tests, and CHANGELOG.
- changes: each file has a stated purpose, and the two new files are the issue's own deliverables. The work is well under 500 lines.
- test: it adds tests/unit/risk/test_successive_release_fixture.py, which runs kanon_report per release and asserts the committed k. On master that command stops with "file or directory not found".
- comms: the comment follows the issue's boundary. No competing PR or maintainer direction exists, and the repo requires no contributor AI disclosure.

Procedure gaps:
- No posted repro comment: I graded diagnosis from the repro facts the plan itself cites.
- Bug-shaped rubric: it assumes a bug, so I read "cause" as "gap".


Procedure gaps:
- No posted repro comment: your repro is in the story and repro-draft.md, not on the issue. I graded diagnosis from the repro facts the plan itself cites.
- Bug-shaped rubric: it assumes a bug, so "cause" was read as "gap".

{
  "item": "https://github.com/maziyarpanahi/openmed/issues/5295",
  "checks": [
    {"name": "diagnosis", "grade": "pass", "evidence": "Cause: kanon_report measures one table and no risk fixture has a release field or privacy-unit key; cites _equivalence_key, existing fixtures and 35 passing tests"},
    {"name": "scope", "grade": "pass", "evidence": "Names the fixture, test and README; excludes openmed/risk/, existing fixtures/tests, CHANGELOG; reason: add exactly what issue #5295 asks for"},
    {"name": "changes", "grade": "pass", "evidence": "Three files each with a stated purpose, two new files are the issue's listed deliverables; about 24 rows, about 40 test lines, one paragraph, under 500"},
    {"name": "test", "grade": "pass", "evidence": "Adds tests/unit/risk/test_successive_release_fixture.py running kanon_report per release and asserting committed k; fails on master (file not found)"},
    {"name": "comms", "grade": "pass", "evidence": "Comment follows the issue's boundary (fixture and per-release test only); no competing PR or assignee; repo docs require no contributor AI disclosure"}
  ],
  "verdict": "accept"
}