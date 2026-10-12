# Procedure: how this tool grades a PR package

<!--
THIS IS THE PART YOU WRITE (second week running for the procedure).
Week 3 you wrote these steps for a plan package; this week the graded
object is a PR package, and the read that matters most is a
side-by-side: the diff against the plan, the evidence against the test
plan, the description against both. Your week-3 procedure is the
pattern; do not paste it unchanged, because its read order was built
for a different object.

Your rotation is the design brief again, and this week friction routes
three ways: a stall on WHAT to decide is a rubric gap, a stall on
WHERE to look is a procedure gap (this file), and a stall on what the
tool even reads or outputs is a frame gap (your SKILL.md). A complete
procedure lets someone who has never seen a PR package before grade
one exactly the way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
plan's scope pair before opening the diff, and list the files the plan
names" is a step; "understand the change" is a wish.
-->

Grade one PR package by running the four stages below in order. Use
only the evidence the package contains (eval mode: the bundle text,
nothing fetched or run; live mode: the inputs `SKILL.md` names, with
`scope.md` read first). The rubric has eight checks, in this order:
`diff-matches-plan`, `description-matches-diff`, `test-evidence`,
`repo-checks`, `clean-diff`, `template-complete`, `ai-disclosure`
(all required) and `title-and-issue` (preferred). If this procedure is
silent on something, say so in the summary instead of inventing a step.

## 1. Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides where the plan sits in the order (before the diff? before the
description?) and says why the order matters for the side-by-side
checks that come later. -->

The plan and the issue are read before the diff and the description,
so that the diff is judged against what was promised and not the other
way round. The description is read last, because it is the claim the
other parts are tested against.

1. **Repo facts and issue first** (eval: `## Repo facts` and
   `## Issue`; live: the PR template `.github/PULL_REQUEST_TEMPLATE.md`,
   `docs/CONTRIBUTING.md`, and the issue page). Note the issue number
   and title, every section and checklist item the PR template asks
   for, any other stated ask (changelog or whatsnew entry, issue
   link), and the contribution policy line, especially any AI-use
   disclosure rule and what it covers.
2. **Thread highlights second** (eval: `## Thread highlights`; live:
   the issue thread). Note any maintainer direction the PR must
   honor.
3. **The plan third** (eval: `## Plan context`; live: `plan.md`).
   Note, in this order: the repro evidence and its steps with
   Expected/Actual, the files the plan names, the not-in-scope
   statement, each planned step, the test plan, and every deviation
   note (live: `## Deviations` in `plan.md`). Write the list of
   planned files and steps down now; the diff is graded against it.
4. **The diff fourth** (eval: `### Diff` under `## Candidate PR`;
   live: the output of `git diff main...HEAD`, with `git log main..HEAD`
   for the commits). Note every changed file and, per file, what each
   hunk does. Note anything that looks like debris (debug output,
   commented-out or dead code, whitespace-only changes, unrelated
   hunks).
5. **The test evidence fifth** (eval: `### Test evidence`; live:
   `test_evidence.md` and the Testing section of the draft). Note the
   before and after output, any new test, and the result of each
   repo check named in the template.
6. **The title and description last** (eval: `### Title` and
   `### Description`; live: `pr_draft.md`). Note the title, each
   template section and what is in it, the issue link, the Changes
   bullets, every fidelity claim ("matches the plan", "no other
   changes"), every ticked checkbox, and any AI-use statement. In live
   mode, hold the title and description against `voice-guide.md` and
   list the rules they break for the summary.

## 2. Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which file, diff,
or page per your evidence guide) to pull the fact from, and what to
record. The load-bearing gathers this week are pairings: diff files
against plan scope, claimed evidence against the plan's test plan and
the repo's checks, description claims against diff contents. A
complete procedure leaves no check whose evidence an executor would
have to hunt for. -->

Record one quote or fact per item before grading anything. If an item
is not in the package, record "absent"; do not fetch, run, or invent
it. Where `references/evidence-guide.md` says where an evidence family
lives, use that location.

- **Plan fidelity (diff against plan).** Build a two-column list:
  every file the diff changes, beside the plan's named files and
  boundary. For each changed file, mark it "planned", "covered by a
  deviation note", or "unplanned". For each planned step and planned
  file, mark it "in the diff" or "missing". Record any new behavior in
  the diff (a new option, a rewrite, a refactor) the plan scopes out
  or never mentions, quoting the hunk. Record any omission and whether
  a deviation note or the description discloses it.
- **Description against diff.** Make a list of every claim the
  description makes (each Summary sentence, each Changes bullet, each
  fidelity claim). Mark each "in the diff" or "not in the diff". Then
  walk the diff's behavior changes and mark each "described" or
  "undescribed".
- **Test evidence (evidence against test plan).** Record the plan's
  test plan and the repro steps with their Expected/Actual. Record what
  the test-evidence section shows: a before output, an after output,
  any new or extended test and the file it lives in. Record whether the
  evidence exercises the path the diff changes (name the changed
  function or file and the evidence line that reaches it). If the
  evidence is only a claim ("tested locally", "tests pass"), record the
  quote and mark it "no observable result".
- **Repo checks.** List each check the template or docs ask for
  (for example `make test-unit`, `make test-integration`, `make lint`,
  `make typecheck`). For each, record "result shown" with the quoted
  result, "absent", or "failed" with the reason given. Record any
  ticked box and whether the evidence supports it. Do not require CI
  to have run.
- **Diff quality.** For each hunk, record what it does and whether the
  plan covers it. List every debris tell found: debug prints or
  leftover logging, commented-out or dead code, formatting or
  whitespace churn on lines the fix does not need, unrelated hunks.
  Quote each.
- **Standards and comms.** Per template section, record "filled" with
  a short quote, "empty or placeholder", or "missing". Record the
  issue link as written and whether it names the plan's issue. Record
  any other stated ask and whether the package meets it (for example,
  a changelog entry stated as required, and whether the diff contains
  it).
- **AI disclosure.** Record the policy line's wording and what it
  covers. Record the description's AI-use statement verbatim (usually
  in Notes for Reviewers), or "absent". Note whether it says how AI
  was used or is only a boilerplate line.
- **Title and issue.** Record the title and compare it to the diff
  (what changed and where), and record the issue the PR names.

Then compare the description with the plan for consistency: every
difference the description states from the plan should match a
deviation note or the diff.

## 3. Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->

Run the checks in the order below, each against its own gathered
evidence. Grade `pass` only when every element of the rubric's pass
condition is met and you can quote the fact that shows it. A missing
required element is `fail`. Grade `unclear` only when the package is
ambiguous in a way you cannot resolve from it (for example, a hunk
that could belong to two planned files); where evidence is simply
absent, grade `fail` and write "absent" as the evidence. A check may be
graded from its evidence notes without re-reading the whole package.
Judge the outcome, not the polish: a terse PR that meets a condition
passes, and a long confident one that does not, fails. Apply the
honest-outcome rule: a shortfall or deviation that the description or
the plan's deviation notes disclose does not fail the check it
touches; a silent one does. One check's result never changes another's.

1. **diff-matches-plan.** Use the two-column list. Pass if every
   changed file and behavior change is planned or covered by a
   deviation note and nothing planned is missing without disclosure.
   Fail on any unplanned, undisclosed change, or any silently missing
   planned step.
2. **description-matches-diff.** Use the claim list. Pass if every
   claim is in the diff, every behavior change is described, and any
   fidelity claim is true. Fail on a claim the diff does not contain,
   an undescribed behavior change, or a fidelity claim the diff
   contradicts.
3. **test-evidence.** Pass if the evidence shows an observable result
   that would differ without the fix (before and after, or a new test
   that fails on the old code) AND exercises the changed path. Fail if
   it is only a claim, shows only the after, or exercises unchanged
   code.
4. **repo-checks.** Pass if every check the template or docs ask for
   is shown with its result (or its absence explained, such as
   "no tests ran" pasted as is) and no failure is hidden. Fail on a
   check never run or never shown, or a ticked box the evidence
   contradicts. An unticked CI box is not a fail.
5. **clean-diff.** Pass if every hunk serves the planned change. Fail
   on any debris tell or unrelated hunk, whether or not the
   description mentions it. Do not grade commit count or message
   style.
6. **template-complete.** Pass if every template section has real
   content, the issue link names the plan's issue (`Closes #N`), every
   ticked box is true, and every other stated ask is met. Fail on a
   missing, empty, or placeholder section, an absent or wrong issue
   link, or an ignored stated ask. If the repo has no template, use
   the rubric's no-template condition.
7. **ai-disclosure.** Pass if the description states how AI was used,
   in the author's own words, where the policy or the course requires
   it. Fail if the disclosure is absent or says nothing about how AI
   was used. Treat every package as AI-assisted.
8. **title-and-issue** (preferred). Pass if the title says what changed
   and where, in terms the diff bears out, and the PR names its issue.
   Fail otherwise. This grade is reported only.

## 4. Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check: the
check whose failure the verdict turned on. When more than one check
failed, your procedure picks which one gets quoted (first failing
required check in rubric order is a fine rule); nothing picks it for
you, so write the rule down. A complete procedure produces the same
verdict from the same grades, every time. -->

1. Apply the rubric's verdict rule: `accept` if all seven required
   checks are `pass`; otherwise `reject`. The preferred check
   `title-and-issue` never changes the verdict.
2. `unclear` counts as `fail`, so any `fail` or `unclear` among the
   required checks rejects.
3. In the summary, quote the deciding evidence for the first failing
   required check in rubric order (the execution order above). If all
   required checks pass, quote the `diff-matches-plan` evidence.
4. In live mode, add the voice-guide rules the title and description
   break to the summary. They never change the verdict. If a required
   check failed, say which file to fix (the diff, `plan.md`,
   `test_evidence.md`, or `pr_draft.md`).
5. Finish with the JSON block `SKILL.md` requires (`item`, the eight
   checks named exactly as in the rubric, each with a one-line
   `evidence`, and `verdict`) as the last thing in the output.
6. List any procedure gaps you hit in the summary before the JSON
   block.
