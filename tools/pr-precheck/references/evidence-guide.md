# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->

## Plan fidelity (harness category: silent-drift)

<!-- Where the plan states its scope, boundary, and deviation notes,
and where the diff shows what actually changed. What it means for a
diff to match the plan, for an honest deviation to re-tie a mismatch,
and what silent drift looks like in each direction (more than the
plan, or less with no note). The description's fidelity claims read
against the diff live here too: a description claiming more or less
than the diff delivers is silent drift, not a comms problem. -->

**Where it lives**

- *Eval bundle:* the plan is in `## Plan context`: the files it names
  (`Files:`), the boundary (`Scope, one bounded change` and
  `Not in scope`), and the planned steps. The change itself is
  `### Diff` under `## Candidate PR`; the files it touches are the
  `--- a/...` headers. The claims to test are in `### Description`
  (Summary text, any "implements the plan exactly" line, any
  checklist) and `### Commits`. A deviation note, if present, is inside
  the plan context or the description.
- *My own PR:* the plan is `./unit-3/plan.md`: `## Files I will touch`,
  `## Scope` (In scope / Not in scope), `## Approach`, and
  `## Deviations`, which is where a recorded change from the plan
  lives. The change is the output of `git diff main...HEAD` from the
  working copy (commits: `git log main..HEAD`). The claims are in
  `./unit-3/pr_draft.md`: Summary, Changes bullets, and Notes for
  Reviewers.

**What good looks like**

- Every file in the diff's headers is a file the plan names, or is
  named in a deviation note. Every planned file and step shows up in
  the diff, or its omission is disclosed. Example from calib-03: the
  plan names `statement.js`, tests under `tests/format/js/if/`, and a
  changelog, but the diff also rewrites `print/property.js`, which the
  plan never mentions and `Not in scope` rules out (any other statement
  type). That is silent drift, and the description's "no changes beyond
  it" contradicts the diff.
- Silent drift runs both ways: more than the plan (an extra file, a new
  option, a rewrite) or less (a planned step missing) with no deviation
  note or disclosure. An honest deviation re-ties the mismatch: a
  change recorded in `plan.md` under `## Deviations`, or disclosed in
  the description, is not drift. A deviation that exists only in the
  diff is.
- Each description claim is true of the diff, and every behavior change
  in the diff is described. The description's fidelity claims count
  here: "implements the plan exactly" is a claim checked against the
  file list, not a comms nicety.
- A small gap between plan wording and diff (a branch name, a few lines
  of difference in size) is not drift when the behavior and the files
  match. In my PR, the branch rename recorded under Deviations is
  honest and passes.

## Test evidence (harness category: not-tested)

<!-- Where the PR shows its proof: the test-evidence section's
before/after against the plan's test plan and the reproduction's own
steps, and the outcome of the repo's own checks or suite. What
decisive looks like (an observable behavior named, the expected-after
stated, the checks' outcome visible) next to "tests pass". -->

**Where it lives**

- *Eval bundle:* the PR's proof is `### Test evidence` under
  `## Candidate PR`. It is read against the plan's `Test plan:` and the
  `Repro evidence` in `## Plan context` (the repro steps and the
  Expected/Actual). The repo's own checks to look for are named in the
  repo-facts block's `pull requests:` line (the template's checklist
  and test asks). Test files added show up in `### Diff`.
- *My own PR:* the proof is `./unit-3/test_evidence.md` (the repro
  re-run before and after, plus the output of `make test-unit`,
  `make test-integration`, `make lint`, `make typecheck`), echoed in
  the Testing section of `./unit-3/pr_draft.md`. It is read against
  `## Test plan` in `./unit-3/plan.md` and the repro steps it quotes,
  and against the checks listed in the repo's
  `.github/PULL_REQUEST_TEMPLATE.md`. The new test is in the branch's
  diff (`tests/unit/test_security.py`).

**What good looks like**

- Decisive evidence shows an observable result that would differ
  without the fix: a before and an after output for the repro, or a new
  test that fails on the old code and passes on the new. It names the
  behavior and states the expected value (calib-03: the issue's input
  run before and after, with the after matching the expected output byte
  for byte).
- The evidence exercises the path the diff changes. Output from code the
  diff does not touch proves nothing. In calib-03 the repro runs the
  if-else printer the diff edits, so it counts.
- Every check the template or docs ask for appears with its result
  pasted, or its absence is explained (for example,
  `make test-integration` printing "no tests ran", pasted as is). A
  failing check reported with the reason still counts as evidence; a
  hidden one does not. An unticked CI box before the PR is open is not a
  gap. A ticked box the evidence contradicts is.
- Not decisive: "tested locally", "tests pass", "works now", an after
  with no before, or a suite run with nothing named. Record the quote
  and mark it as no observable result.
- Read each check's output as the repo prints it. `make test-unit`
  summaries such as "377 passed, 52 xfailed" are a pass: the xfailed
  tests are the repo's seeded bugs, not failures, and only a `failed`,
  `error`, or `XPASS(strict)` line is one. `make test-integration`
  printing "no tests ran" and ending `make: *** [test-integration]
  Error 5` is the expected result for a repo with no integration tests
  (a non-zero exit there is not a hidden failure). `make lint` ("All
  checks passed!") and `make typecheck` ("Success: no issues found")
  pass on those lines. A check whose exit line is blank is read from
  its own output lines.
- Read the seeded-bug marker box against the evidence and the diff
  together. A `grep -rn "issue #N:" tests` that finds nothing does not
  show whether a marker was removed (the marker may be keyed by another
  id, such as H-05). The proof that a marker was removed is the diff
  deleting the `@pytest.mark.xfail` lines; if the evidence says nothing
  was deleted while the description ticks the box, the box is
  contradicted.
- Count the repro itself as evidence only when the before and after
  runs of the plan's repro steps are shown. A full-suite pass alone does
  not show the new test fails on unfixed code.
- Check that `test_evidence.md` is real captured output. A file that
  holds some other tool's summary, not the repro and the checks, is
  not test evidence for this PR.

## Diff quality (harness category: unreviewable)

<!-- Where the change itself lives: the unified diff and the commit
list. What a reviewable change looks like (the fix visible, nothing
unrelated riding along) and the debris tells: debug leftovers, dead
code, commented-out blocks, formatting churn, drive-by edits. -->

**Where it lives**

- *Eval bundle:* `### Diff` under `## Candidate PR` (read hunk by
  hunk, using the `--- a/` and `@@` lines to see which file and area
  each hunk belongs to) and `### Commits` for what each commit says it
  does.
- *My own PR:* the output of `git diff main...HEAD` (three dots, from
  the working copy) and `git diff main...HEAD --stat` for the file list,
  plus `git log main..HEAD`. Only committed changes count. `plan.md`,
  `comment.md`, `pr_draft.md`, and `test_evidence.md` must not appear in
  the diff.

**What good looks like**

- Every hunk serves the planned change, and the fix is visible at a
  glance. For my PR that means `core/security.py` (the `try`/`except`,
  the import, the docstring line) and `tests/unit/test_security.py`
  (the removed `xfail` marker and the one new test), and nothing else.
- Debris tells, each of which fails the diff whether or not the
  description mentions it: debug prints or leftover logging; commented-
  out or dead code; formatting or whitespace churn on lines the fix does
  not need; a rename, cleanup, or refactor with no link to the plan
  (calib-03: the `property.js` hunk rewrites a function the issue does
  not mention, burying the real one-hunk fix); a stray file.
- A hunk that rewrites working lines into a different but equivalent
  form is churn, not part of the fix, even when it looks tidier.
- Commit count and message style are not evidence of quality; read the
  hunks, not the log's shape.

## Standards and comms (harness category: standards-wall)

<!-- Where the repo states its asks (the repo-facts block's PR
template sections, contributing instructions, and stated policy,
including AI-use disclosure) and where the PR honors them: the
stated sections filled with real content, the disclosure present,
explicit maintainer direction in the thread engaged. What compliant
looks like next to boilerplate or a visibly ignored ask. (Whether
the description's claims match the diff is plan fidelity, above.) -->

**Where it lives**

- *Eval bundle:* the repo's asks are in `## Repo facts`: the
  `pull requests:` line (template sections, checklist, changelog or
  docs entries it requires, and whether PRs that skip it may be closed)
  and the `contribution policy` line (any AI-use rule and what it
  covers). Maintainer direction is in `## Thread highlights`. The PR's
  answers are `### Title` and `### Description` (sections, issue link,
  checklist, any AI statement), and required entries must appear in
  `### Diff` (for example a changelog file).
- *My own PR:* the asks are in the repo's
  `.github/PULL_REQUEST_TEMPLATE.md` (Summary, Issue, Changes, Testing,
  Screenshots / Demo, Notes for Reviewers), `docs/CONTRIBUTING.md`
  (including "Working on a seeded bug" and the AI policy), and the
  issue thread. The answers are `./unit-3/pr_draft.md`: the title on
  line one, each section, the `Closes #` line, the ticked boxes, and
  Notes for Reviewers, where the AI-use disclosure goes because the
  template has no section for it. The branch name (a type prefix, the
  issue number, a short description) is a `git branch` read.

**What good looks like**

- Every template section has real content: Summary says what and why,
  Issue is `Closes #72` naming the plan's issue, Changes has one bullet
  per change in the diff, Testing ticks only true boxes and pastes
  results, Screenshots / Demo has an image or "N/A" with a reason, and
  Notes for Reviewers is filled. A heading left empty, a placeholder
  comment, or a section deleted is a visibly ignored ask.
- Every other stated ask is met in the package: if the repo requires a
  changelog entry, a docs update, or a checklist, the diff or the
  description shows it (calib-03 adds its changelog file and answers
  the checklist, so it meets them; a PR whose template names a required
  entry that is absent from the diff does not).
- AI disclosure is present in the author's own words and says how AI
  was used on this change, wherever the policy or the course requires
  it. A one-word "AI-assisted" or a boilerplate line is not disclosure.
  Treat every package as AI-assisted.
- Maintainer direction in the thread is engaged, not skipped, and a PR
  does not duplicate someone else's claimed effort.
- Compliant means the asks are answered with content. Boilerplate that
  only reuses the template's own words does not count, and a polished
  description does not cover a missing ask.
