Assignment 4: Test and Submit
Before you open your PR, read it the way a maintainer will.

🛠️ The work, in order
Steps 1-7 build and test your skill, 8-12 get your pull request (PR) ready and open it, and 13 submits.

1. DONE! Install your skill: clone your Unit 4 materials (git clone https://github.com/codepath/ai301-unit4-starter.git, or Code, then Download ZIP on the repo page). Copy everything inside the clone's skill/ folder into ~/.claude/skills/pr-precheck/, so SKILL.md ends up at ~/.claude/skills/pr-precheck/SKILL.md (create the folders if they don't exist). Do all your editing in that installed copy from now on, so eval runs and live runs grade with the same files. Then, in the installed scope.md, replace the placeholder on the Repo: line with your Path Review repo.

Your Path Review repo:

codepath/pathreview-ai301-fa26-s3


2. DONE! Write SKILL.md: this week you write the skill's own instructions too. Read CONTRACT.md first. It's the part that stays fixed: the file layout, the two modes, the accept or reject verdict, and the JSON block every run ends with. Live mode checks your own PR, and eval mode grades a practice submission (one of the frozen PRs in the clone's eval/packages/ folder; the eval harness calls these files packages). Then, using your unit 3 SKILL.md (~/.claude/skills/plan-check/SKILL.md) as a pattern, write instructions to Claude under each heading of the new one. Leave the new file's JSON block exactly as it is. Make sure it says what the skill reads in each mode and when it refuses to grade. In live mode, that's the files the command in step 10 names, the diff from git diff main...HEAD, your issue's thread, and the repo's PR template and docs/CONTRIBUTING.md.

3. DONE! Write your rubric: start from your group's activity worksheet. Every no in its "Files" and "Claims" tables is a problem one of your checks should catch. Put the checks in rubric.md's table (Check, Evidence, Pass condition, Weight), with each Weight either required or preferred. Under it, write a verdict rule that says how the check grades add up to accept or reject, including what an unclear grade does. The practice submissions whose gold label is reject fail in four ways, and the comment in rubric.md explains each one: silent drift, not tested, unreviewable, and standards wall. Make sure your checks cover all four. Missed the activity? Start from your Unit 3 rubric and rewrite each check for a PR.


I filled in the Checks table and the Verdict rule in unit-4/skill/rubric.md. I kept the guideline comment untouched. I haven't run the harness, so none of it is tested against the eval packages yet.

Slides: I couldn't render the PDF pages because pdftoppm isn't installed, so I extracted the text of slides 12 and 13 instead. Slide 12 gives the anatomy: the title says what changed and where, the description says why to believe it, the commits show how it got here, the diff shows what moved, and the evidence shows how to see it work. Slide 13 is the six-section PR template. The checks below follow both, and I've added a short intro line mapping them.

┌──────────────────────────┬───────────────────┬───────────┐
│          Check           │  Failure family   │  Weight   │
├──────────────────────────┼───────────────────┼───────────┤
│ diff-matches-plan        │ silent drift      │ required  │
├──────────────────────────┼───────────────────┼───────────┤
│ description-matches-diff │ silent drift      │ required  │
├──────────────────────────┼───────────────────┼───────────┤
│ test-evidence            │ not tested        │ required  │
├──────────────────────────┼───────────────────┼───────────┤
│ repo-checks              │ not tested        │ required  │
├──────────────────────────┼───────────────────┼───────────┤
│ clean-diff               │ unreviewable      │ required  │
├──────────────────────────┼───────────────────┼───────────┤
│ template-complete        │ standards wall    │ required  │
├──────────────────────────┼───────────────────┼───────────┤
│ ai-disclosure            │ standards wall    │ required  │
├──────────────────────────┼───────────────────┼───────────┤
│ title-and-issue          │ none (title and   │ preferred │
│                          │ issue link)       │           │
└──────────────────────────┴───────────────────┴───────────┘

Verdict rule:
- accept if every required check passes.
- Any required fail means reject.
- unclear counts as fail.
- Preferred checks never change the verdict.
- I added an honest-outcome line: a shortfall that the description or plan.md discloses can still pass, and only a silent shortfall fails.

Pass conditions: they judge outcomes, not shape. For example, test-evidence asks for an observable before and after on the path the diff touches. clean-diff looks for debris and unrelated hunks, not commit count or message style.

I also split the repo's own checks (make test-unit, lint and the rest) into their own check, repo-checks. The Unit 4 instructions list those checks as a separate requirement, and keeping them out of test-evidence stops one bad grade from covering for the other.

ai-disclosure is required rather than preferred. The course always asks for it, and standards wall has only two eval packages, so missing it would be costly. 





4. DONE! Write your procedure: start from your Unit 3 procedure.md and rewrite each step for a PR, under the same four headings (Read order, Evidence gathering, Check execution, Verdict assembly). Most steps now compare two things side by side, such as the diff against the plan, the test evidence against the plan's test plan, and the description against the diff.


I rewrote unit-4/skill/procedure.md for PRs. It keeps all the guideline comments and uses the same four headings as your Unit 3 procedure. I haven't run it through the harness yet.

- Intro: it names the eight checks in rubric order and tells the tool to report any gap it hits instead of inventing a step.
- Read order: the repo facts and issue come first, then the thread, then the plan (including its deviation notes), then the diff, the test evidence, and the title and description last. The plan sits before the diff so the diff is judged against what was promised. The description comes last because it is the claim being tested.
- Evidence gathering: each family has a concrete move, and the three load-bearing ones are pairings:
  - Diff against plan: a two-column list marking each changed file as planned, covered by a deviation note, or unplanned, and each planned step as in the diff or missing.
  - Description against diff: a list of every claim in the description marked as in the diff or not, plus every behavior change in the diff marked as described or not.
  - Test evidence and repo checks: the plan's test plan and repro steps set against what the evidence shows, with each check the template asks for recorded as result shown, absent, or failed.

  Evidence is gathered separately for diff quality, template and standards asks, AI disclosure, and the title.
- Check execution: the eight checks run in rubric order, each with its pass and fail condition restated for the executor.
  - Absent evidence grades fail and is recorded as "absent".
  - unclear is reserved for genuinely ambiguous packages.
  - The honest-outcome rule is applied: disclosed shortfalls don't fail the check they touch.
- Verdict assembly:
  - accept only if all seven required checks pass, and unclear counts as fail.
  - The tool quotes the first failing required check in rubric order, or diff-matches-plan if everything passes.
  - In live mode it adds the voice-guide breaks and which file to fix.
  - The JSON block comes last, and procedure gaps are listed before it.





5. DONE! Write your evidence guide: open a practice submission, such as calib-03.md (they're all in the clone's eval/packages/ folder), next to references/evidence-guide.md, and fill in one heading at a time. Under each heading, write where that evidence lives in a practice submission, where it lives in my own PR (my ./unit-3/plan.md, your branch's diff, test_evidence.md from ./unit-3/test_evidence.md, and pr_draft.md from ./unit-3/pr_draft.md), and what good looks like.


I filled in all four headings in unit-4/skill/references/evidence-guide.md, using calib-03.md as the example package. The original comments and the file's top guideline block are untouched. Each heading now has Where it lives (in an eval bundle and in your own PR) and What good looks like. I haven't run the harness, so none of this is tested.

calib-03 fits well as the example. Its diff carries a property.js rewrite the plan never mentions, while the description claims "no changes beyond" the plan. That one package shows both silent drift and unreviewable debris.

- Plan fidelity (silent drift):
  - Eval bundle: the plan in ## Plan context against ### Diff and ### Description.
  - Your PR: the plan in unit-3/plan.md (Files, Scope, Approach, Deviations) against git diff main...HEAD and pr_draft.md.
  - Good: every changed file is planned or covered by a deviation note, drift is counted in both directions, and the "implements the plan exactly" claim is checked against the file list.
- Test evidence (not tested):
  - Eval bundle: ### Test evidence against the plan's test plan and repro.
  - Your PR: unit-3/test_evidence.md and the Testing section of pr_draft.md.
  - Good: an observable before and after on the path the diff changes, with the repo's own checks pasted. "Tests pass" doesn't count, and an unticked CI box is fine.
- Diff quality (unreviewable):
  - Where: the diff hunks, read file by file, from git diff main...HEAD --stat, and git log main..HEAD.
  - Good: only your two files, core/security.py and tests/unit/test_security.py. The debris tells are listed. Equivalent-but-rewritten lines count as churn.
- Standards and comms (standards wall):
  - Eval bundle: the repo-facts pull requests: and policy lines and ### Description.
  - Your PR: the TING.md, andpr_draft.md, including the AI disclosure in Notes for Reviewers.
  - Good: every section has real content, required entries appear in the diff, and the disclosure says how AI was        used.





6. DONE! Copy in your voice guide: run cp ~/.claude/skills/plan-check/voice-guide.md ~/.claude/skills/pr-precheck/voice-guide.md, which replaces the template with your guide. Add rules if a PR title and description need something your comments didn't.


I added four PR-specific rules to unit-4/skill/voice-guide.md, plus five new lines in "Things I never post". Your week-2 content and all the comment guidelines are unchanged. The unit-4 copy already had your Unit 3 guide pasted in, so I extended that rather than copying it over again.

Each new rule has a wrong and right pair, written in your existing format. I based the right-hand examples on your actual PR for issue #72.

┌────────────────┬────────────────────────────────────────┐
│      Rule      │                 Covers                 │
├────────────────┼────────────────────────────────────────┤
│ Title says     │ fix: return False from verify_password │
│ what changed   │  on malformed stored hash, not "Fix    │
│ and where      │ bug in security".                      │
├────────────────┼────────────────────────────────────────┤
│ Claim only     │ No "implements the plan exactly" or    │
│ what the diff  │ "no other changes" unless I've checked │
│ does           │  the diff. This is the silent-drift    │
│                │ tell.                                  │
├────────────────┼────────────────────────────────────────┤
│ Show test      │ Paste the before and after and each    │
│ output, not    │ check's result, and tick only the      │
│ test claims    │ boxes the output supports.             │
├────────────────┼────────────────────────────────────────┤
│ Say what I     │ Describe the AI use in my own words,   │
│ left out and   │ and name anything omitted or changed   │
│ what I used AI │ from the plan. A bare "AI-assisted"    │
│  for           │ doesn't count.                         │
└────────────────┴────────────────────────────────────────┘

The new "never post" lines cover:
- vague titles
- an unchecked "implements the plan exactly"
- "tested locally" with no output
- a bare AI note
- promised follow-up PRs, which extends your existing no-timeline rule

The voice guide is read only in live mode and never changes the verdict. My existing rules are comment-oriented, such as "name the version" and "no promised timelines". They still apply to PR text, so I left them as they were.







7. Run the harness: from the clone's eval/ folder, run python3 run_eval.py --skill ~/.claude/skills/pr-precheck --save-run eval-run.txt. This full run grades with your installed copy and saves the results to eval-run.txt in that folder. The table at the end compares each verdict with the gold labels (the staff answer key). Revise your files for the rows marked NO, then re-grade just those practice submissions with --only (the eval README in your Unit 4 materials shows how, and Retries below says what to add when you loosen a check). Keep going until 18 of the 20 agree and every category on the categories: line has at least one match, adding --save-run eval-run.txt to every full run. A partial run never overwrites the file, so it always holds your last full run, and that's the file you submit. The score itself sets no points, so if you run out of time, submit the one you have.


python3 run_eval.py --skill ~/.claude/skills/pr-precheck --save-run eval-run.txt
grading 20 package(s) with the tool at /Users/EricMichel/.claude/skills/pr-precheck, model sonnet, 5 worker(s)...
  pkg-03: reject
  pkg-05: accept
  pkg-01: reject
  pkg-02: accept
  pkg-04: reject
  pkg-06: reject
  pkg-09: reject
  pkg-08: accept
  pkg-07: reject
  pkg-10: reject
  pkg-12: reject
  pkg-13: accept
  pkg-14: reject
  pkg-15: reject
  pkg-11: accept
  pkg-16: accept
  pkg-17: reject
  pkg-18: reject
  pkg-19: accept
  pkg-20: reject

item    category        gold    verdict  agree  note
pkg-01  standards-wall  reject  reject   yes
pkg-02  clear-accept    accept  accept   yes
pkg-03  silent-drift    reject  reject   yes
pkg-04  not-tested      reject  reject   yes
pkg-05  clear-accept    accept  accept   yes
pkg-06  silent-drift    reject  reject   yes
pkg-07  not-tested      reject  reject   yes
pkg-08  clear-accept    accept  accept   yes
pkg-09  silent-drift    reject  reject   yes
pkg-10  not-tested      reject  reject   yes
pkg-11  clear-accept    accept  accept   yes
pkg-12  unrevi yes
pkg-13  clear-accept    accept  accept   yes
pkg-14  not-tested      reject  reject   yes
pkg-15  unreviewable    reject  reject   yes
pkg-16  clear- yes
pkg-17  silent-drift    reject  reject   yes
pkg-18  unrevi yes
pkg-19  clear-accept    accept  accept   yes
pkg-20  standa yes

categories: cl 4/4  silent-drift4/4  standards-wall 2/2  unreviewable 3/3
agreement: 20/20 scored items  (bar: 18/20: PASS)
run written to eval-run.txt



8. Test your change: no working change from unit 3? A TF gives you the house chain in class (a house issue, its repro pack and plan, and a branch with a working change), or ask in #dts-fa26-ai301-solution-planning on Slack. Use those wherever these steps say yours. From here on, work in the top folder of your fork's clone, on your unit 3 branch.

a. Run grep -rn "issue #41:" tests, with your issue number in place of 41. If it finds an @pytest.mark.xfail marker, your issue is one of Path Review's seeded bugs, and that marker makes the suite fail once your fix works. Delete each marker it finds, along with any suppression in pyproject.toml that names your issue (Path Review's docs/CONTRIBUTING.md explains why), and commit.

---
The grep found no issue #72: marker, and pyproject.toml has no mention of 72. I'll search more loosely and run the tests.

  Ran 2 shell commands

No xfail marker or pyproject.toml suppression refers to issue #72, so I deleted nothing and there is nothing to commit.

- Grep: grep -rn "issue #72:" tests returned nothing. The only #72 mention in tests is the docstring of test_verify_password_malformed_hash in tests/unit/test_security.py. That test is a regular test, not marked xfail.
- pyproject.toml: it has no suppression naming #72.
- Test suite: 377 passed and 52 xfailed, with no XPASS(strict) failures. The #72 fix and its test pass.
- Working tree: clean.

Issue #72 doesn't appear to be one of the seeded bugs that has a marker, so no cleanup is needed.

---




b. Run the four checks Path Review's PR template lists: make test-unit, make test-integration, make lint, and make typecheck. Path Review has no integration tests yet, so make test-integration stops with "no tests ran". That's expected, so paste it as is.

---

Results of the four checks (the exit: lines came out blank, so the status comes from each command's own output):

- make test-unit: 377 passed, 52 xfailed, 1 warning (the pydantic deprecation warning in core/config.py).
- make test-integration: no tests ran, as expected. The output, as is:
.venv/bin/pytest tests/integration -v -m integration
...
collecting ... collected 0 items

============================ no tests ran in 0.05s =============================
make: *** [test-integration] Error 5
- make lint: ruff check . reported "All checks passed!"
- make typecheck: mypy reported "Success: no issues found in 76 source files".

Apart from the expected integration result, everything is clean.


---

Re-run your unit 2 repro steps, on main for the before (or reuse your unit 2 output) and on your branch for the after.

---

No changes need it.

---


Paste all of that output into a new file, test_evidence.md, next to plan.md. A failing check reported honestly, with the reason, still counts as evidence; hiding one doesn't.





9. Write your PR description: in a new file, pr_draft.md, next to plan.md, put your PR title on the first line. Under it, paste Path Review's PR template (.github/PULL_REQUEST_TEMPLATE.md in your Path Review repo) and fill in every section:
Summary: what the change does and why.
Issue: your issue number after Closes #.
Changes: one bullet per change in your diff.
Testing: tick only the boxes that are true, and paste your before and after and your checks' results from test_evidence.md. Leave the CI box unticked, because CI only runs after you open the PR, and its result doesn't affect your grade.
Screenshots / Demo: add one if it helps a reviewer, or write "N/A" and why.
Notes for Reviewers: say in your own words how you used AI on this change. The course always asks for this disclosure, and the template has no section for it, so it goes here. Also note anything you changed from your plan or left out on purpose.


I filled in unit-3/pr_draft.md with a title on the first line, then the Path Review PR template with every section completed. I read plan.md and its Deviations section for the content, and pulled the template from the repo with gh. I only used facts from plan.md, and I haven't run any of the tests or checks myself.

- Title: fix: return False from verify_password on malformed stored hash
- Summary, Issue, Changes: Summary covers the cause and the fix. Issue is Closes #72. Changes has one bullet per change in the diff: the core/security.py catch, removing the xfail marker, and adding the new test.
- Testing:
  - I ticked lint, typecheck, new tests, and the xfail removal, since plan.md says those passed or were done.
  - I left make test-unit and make test-integration unticked because nothing in your files shows them run. I also left CI unticked, as the assignment says.
  - The before and after are written from your Deviations notes, and the 26 passed is for the security test file only.
- Screenshots / Demo: N/A, with the reason.
- Notes for Reviewers: the AI-use disclosure, the branch-name difference from the plan, and the two items you left out on purpose (the ValueError case and the lack of logging).







10. DONE! Check it with your skill: from the top folder of your fork's clone, on your branch, run claude "pr-precheck: grade my draft PR for issue https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72 plan in plan.md, the diff on this branch, title and description in pr_draft.md, test evidence in test_evidence.md". "The diff on this branch" is every committed change this branch makes compared with main, which git diff main...HEAD (three dots) shows you. A correct run reads your installed skill's files, prints a per-check summary, and ends with a fenced JSON block whose verdict is accept (ready) or reject (hold). If it stops with a scope message instead, your Repo: line still has the placeholder, so go back to step 1. If you see neither, the skill didn't run.



11. DONE! Fix what your skill finds: remove the debris it flags (debug lines, commented-out code, unrelated edits), fill the gaps in your evidence, and make the description match the diff. Commit each code fix, because step 10 reads only committed changes, but never git add plan.md, comment.md, pr_draft.md, or test_evidence.md. If a fix changes what your plan said, write what changed and why under ## Deviations in plan.md, because graders read your diff against that file. Re-run step 10 until the verdict is accept, or until every fail left is one your description discloses. Before you push, git diff main...HEAD --stat should list only the files your fix changes.


test_evidence.md fixes:

I rewrote test_evidence.md with output from this session. I didn't re-run the precheck.

- Before, on unfixed main: the H-05 test run with --runxfail fails with passlib.exc.UnknownHashError. With the branch's test file against the unfixed code, both the H-05 test and the new test_verify_password_malformed_hash fail.
- After, on the branch: both tests pass. make test-unit gives 377 passed, 52 xfailed. make lint and make typecheck pass. make test-integration finds no tests and exits with Error 5, which is expected for this repo.
- Seeded-bug marker: the old file's grep used issue #72: with a colon, but the marker's reason reads issue #72 (manifest H-05) with no colon, so it never matched. The file now shows the marker on main and the diff deleting it.

Still to do in pr_draft.md:
- Delete the leftover <!-- How did you verify your changes? --> comment.
- Optionally paste the before output into Testing, as the voice guide asks, instead of describing it.

Untracked files are pr_draft.md, test_evidence.md and skill-run-result.txt, which I didn't create. None is committed, so none appears in the diff.



pr_draft.md fixes:

Update(pr_draft.md)
Removed 1 line
  which asserts `verify_password("test-password", "not-a-valid-password-hash") is False`.

## Testing
<!-- How did you verify your changes? -->
- [ ] **CI is green on this PR (all five jobs)** — required for review
- [x] Unit tests pass (`make test-unit`)
- [ ] Integration tests pass (`make test-integration`)

⏺ Update(pr_draft.md)
Added 18 lines, removed 4 lines
- [x] If this fixes a seeded bug: removed its `@pytest.mark.xfail` marker (and any
      matching suppression in `pyproject.to

**Before (unfixed `main`):** running the H-05 test with the marker ignored,
`pytest tests/unit/test_security.py::TestSerong_hash_format --runxfail`,
raises `passlib.exc.UnknownHashError`. The new test also fails on the unfixed
code with the same error.
**Before (unfixed `main`):**
```
$ pytest tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format --runxfail
E           passlib.exc.UnknownHashError: hd
FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format
========================= 1 failed, 1 warning in 0.13s =========================
```
With this branch's test file on the unfixed `core/security.py`, the new test
fails the same way:
```
FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format
FAILED tests/unit/test_security.py::TestSecurity::test_verify_password_malformed_hash
================= 2 failed, 24 deselected, ===========
```

**Seeded-bug marker:** the diff deletes thet=True, ...)`
block whose reason reads `issue #72 (manifest H-05)`. `pyproject.toml` has no
suppression for #72.

**After (this branch):**
- `make test-unit`: 377 passed, 52 xfailed, 1 warning (the pydantic
  deprecation warning in `core/config.py`), with no `XPASS(strict)` failures.

⏺ Update(pr_draft.md)
Added 1 line
suppression for #72.

**After (this branch):**
- Both tests above pass: `2 passed, 24 dese`.
- `make test-unit`: 377 passed, 52 xfailed, 1 warning (the pydantic
  deprecation warning in `core/config.py`), with no `XPASS(strict)` failures.
  The fix's new test, `test_verify_password the

Re-run of pr-precheck, live mode. I re-used the scope, rubric, procedure and voice guide I read earlier. The diff is unchanged: 2 files, 1 commit. I re-graded against the new test_evidence.md and the edited pr_draft.md.



Verdict: accept (ready to submit). All seven required checks pass.

┌──────────────────────────┬───────┬──────────────────────────────────────────────────────────────────────────────────────────┐
│          Check           │ Grade │                                         Evidence                                         │
├──────────────────────────┼───────┼──────────────────────────────────────────────────────────────────────────────────────────┤
│ diff-matches-plan        │ pass  │ Only the two planned files changed. The catch, docstring line, marker removal and one    │
│                          │       │ new test are all there. The branch rename is recorded under Deviations.                  │
├──────────────────────────┼───────┼──────────────────────────────────────────────────────────────────────────────────────────┤
│ description-matches-diff │ pass  │ Every Changes bullet is in the diff, and "same two files… marker removed, one new test"  │
│                          │       │ is true.                                                                                 │
├──────────────────────────┼───────┼──────────────────────────────────────────────────────────────────────────────────────────┤
│                          │       │ test_evidence.md now has pasted before and after runs, and the new test failing on       │
│ test-evidence            │ pass  │ unfixed code. The before run shows UnknownHashError from verify_password, the function   │
│                          │       │ the diff changes.                                                                        │
├──────────────────────────┼───────┼──────────────────────────────────────────────────────────────────────────────────────────┤
│                          │       │ All four checks have pasted results. The seeded-bug box is now supported by git grep on  │
│ repo-checks              │ pass  │ main and the -    @pytest.mark.xfail( hunk in the diff. The integration run shows "no    │
│                          │       │ tests ran" with Error 5, which is expected, and CI is left unticked.                     │
├──────────────────────────┼───────┼──────────────────────────────────────────────────────────────────────────────────────────┤
│ clean-diff               │ pass  │ Every hunk serves the planned change. There's no debris, and none of the draft or        │
│                          │       │ evidence files is committed.                                                             │
├──────────────────────────┼───────┼──────────────────────────────────────────────────────────────────────────────────────────┤
│ template-complete        │ pass  │ All sections have content, the leftover template comment is gone, Closes #72 is correct, │
│                          │       │  and every ticked box is true.                                                           │
├──────────────────────────┼───────┼──────────────────────────────────────────────────────────────────────────────────────────┤
│ ai-disclosure            │ pass  │ The Notes for Reviewers state how Claude Code was used and that you ran the commands and │
│                          │       │  read the diff.                                                                          │
├──────────────────────────┼───────┼──────────────────────────────────────────────────────────────────────────────────────────┤
│ title-and-issue          │ pass  │ The title says what changed and where, and the PR names Closes #72.                      │
│ (preferred)              │       │                                                                                          │
└──────────────────────────┴───────┴──────────────────────────────────────────────────────────────────────────────────────────┘

Voice guide: no rules broken. The before and after output is now pasted, and the plan deviations and the left-out items are disclosed.



12. DONE! Open your PR: check your branch name first. It needs a type prefix (fix/, docs/, feat/, test/, refactor/, perf/, or chore/), the issue number, and a short description, like fix/1234-null-check. If it doesn't match, rename it with git branch -m <new-name>. Then push your branch to your fork (git push -u origin <your-branch-name>). On your fork's GitHub page, click Compare & pull request, or open Pull requests, then New pull request. Set the base repository to your Path Review repo and the base branch to main. Replace GitHub's pre-filled template with the title and description from pr_draft.md, then click Create pull request.


- Title: now "fix: return False from verify_password on malformed stored hash".
- Body: now the full pr_draft.md content, from ## Summary through Notes for Reviewers. That includes the pasted before and after output and the seeded-bug marker note.
- Check: the live body is 4608 characters and the source file is 4610. I think the 2-character gap is line-ending or trailing-whitespace handling, but I didn't diff them.

The PR's code is unchanged, still at bd57589. Once CI runs on #91, check that all five jobs are green. The first fork PR may need a maintainer to approve the workflow run.


Link to PR is here: 
https://github.com/codepath/pathreview-ai301-fa26-s3/pull/91 


CI is green on PR #91. All five required jobs passed:

┌──────────────────┬────────┬──────┐
│       Job        │ Result │ Time │
├──────────────────┼────────┼──────┤
│ lint             │ pass   │ 10s  │
├──────────────────┼────────┼──────┤
│ typecheck        │ pass   │ 1m2s │
├──────────────────┼────────┼──────┤
│ test-unit        │ pass   │ 1m6s │
├──────────────────┼────────┼──────┤
│ test-integration │ pass   │ 1m7s │
├──────────────────┼────────┼──────┤
│ frontend         │ pass   │ 22s  │
└──────────────────┴────────┴──────┘

- Merge state: CLEAN.
- Review decision: none yet.
- Integration job: test-integration passed in CI, even though it exits with Error 5 ("no tests ran") when run locally. CI evidently treats that case differently, so the PR text saying CI is unticked "because it only runs after the PR is open" is now out of date.


>>PR #91 now has the graded-accept title and description, and all five CI jobs pass.



13. Submit: in your course repo, upload everything in ~/.claude/skills/pr-precheck/ into tools/pr-precheck/, including the references folder itself, so evidence-guide.md stays inside it. Upload eval-run.txt (from the clone's eval/ folder) into beat-1-sandbox/unit-4/. Both use the same Add file, then Upload files flow as unit 3. If you added deviations in step 11, upload the new plan.md into beat-1-sandbox/unit-3/ too. Fill in beat-1-sandbox/unit-4/pull-request.md: the link to your PR's own page on Path Review (not your fork's branch page), your branch name, and the four eval fields. Then submit your course repo link on this tab.


What you submit
Your course repo link, through the course portal. It is the same link every week, with this unit's additions in place.

❗ IMPORTANT: Submit a link to your entire repo, not just a folder. (What does this mean?)
Three places in it carry this unit's work:

tools/pr-precheck/: every file from your installed copy. That's your SKILL.md, rubric.md, procedure.md, references/evidence-guide.md, and carried-over voice-guide.md, plus CONTRACT.md and scope.md (those two aren't graded, but include them so the folder arrives whole).
beat-1-sandbox/unit-4/pull-request.md: the fields listed in step 13. The four eval fields (Run history, Package analysis, Check rationale, Trade-offs) explain how your skill got to the run in eval-run.txt.
beat-1-sandbox/unit-4/eval-run.txt: the file your last full run wrote with --save-run (step 7). Its header records the model, your skill, and a fingerprint of each file in the run, so never hand-edit it. Your Run history field is where you account for the runs it took.
🎯 The points
This assignment is worth 50 points, twice the earlier units, because you build the whole skill and open your first PR. The full rubric is on the Grading Rubrics page under Course Info. In short: your skill's files carry 12 points, the eval run and write-up 20, and your PR 18. The 20 eval points split two ways:

6 for committing one complete run, written by the harness. A partial run, or none, earns 0.
14 across four separate fields in beat-1-sandbox/unit-4/pull-request.md, each scored on its own:
Run history (4): every run's score in order, ending with the score in your eval-run.txt.
Package analysis (4): one scored practice submission named by id (such as pkg-07), with both verdicts and your reasoning.
Check rationale (4): one check quoted as it now reads, matching the file you uploaded, and why it reads that way.
Trade-offs (2).
If your first full run agrees on all 20, that one run is a complete Run history. For Package analysis, the practice submission you pick doesn't have to be one your rubric got wrong. Name it, give your rubric's verdict and the gold label, and explain why your rubric decided that way.

Why 18 of 20 (a number to aim at, not a grade tier): 16 of the 20 scored practice submissions have clear signals a reasonable rubric should never miss, and 4 are genuinely arguable. The category floor (every category needs at least one match) matters most for the standards-wall category, which has only 2 practice submissions (PRs that skip something the repo's PR template or AI policy requires). A rubric that never checks the repo's stated asks will miss both, and getting the other 18 right can't make up for it. The 4 calibration practice submissions (calib-01 to calib-04) are never scored. If a category ends up with no matches, that's the best thing to write about in your eval fields, because it shows a kind of problem your rubric can't see.

The 18 PR points split six ways:

2 for opening it on your Path Review repo.
2 for naming its issue in the description.
2 for a branch name with a type prefix, the issue number, and a short description, like fix/1234-null-check.
4 for a description with every section filled in, including the AI-use disclosure.
4 for evidence in the description: your repro steps re-run with the before and after, plus the repo's own checks with their results.
4 for a faithful PR. The description matches the diff, every change is in your plan or its Deviations, and the diff has no debris.
An honestly disclosed shortfall keeps its faithfulness points. If your PR leaves something out, or your fix needed a change the plan didn't name, and the description and plan.md say so, you keep all 4. A diff that quietly does more or less than the plan gets 0. An unrelated change, like a cleanup the issue doesn't need, costs 2 even when you explain it. Your PR is graded as you opened it, so review comments, a merge, or a close afterward never change a score. The house chain is graded on the same criteria.