# Procedure: how this skill grades a plan package

Grade one package by running the four stages below in order. Use only
the evidence the package contains (eval mode: the bundle text, nothing
fetched or run; live mode: the student's drafts, their posted repro
comment, and the issue thread). The rubric has four checks, all
required: `diagnosis`, `scope`, `changes`, `test`. If this procedure is
silent on something, say so in the summary instead of inventing a step.

## 1. Read order

1. **Issue first** (`## Issue`; live: the issue page). Note the issue
   title (the exact behavior it names) and the task being asked for.
   The diagnosis is judged against the title, so read it before the
   plan.
2. **Repro evidence second** (`## Repro evidence`; live: the student's
   posted repro comment, or for the house issue the repro pack quoted
   in the drafts). Note the environment, each step's result, the
   control run, any error or debug output, and Expected/Actual. This
   is the ground truth the diagnosis must agree with.
3. **Candidate plan third**, in this order: Diagnosis, Scope, Changes
   (the Files and Approach sections), then Test plan. Note what each
   section claims.
4. **Candidate plan comment last.** No check grades it; read it only
   for consistency with the plan and, in live mode, for the voice-guide
   notes `SKILL.md` asks for. It never changes the verdict.

## 2. Evidence gathering

Record one quote or fact per item before grading anything. If an item
is not in the package, record "absent"; do not fetch, run, or invent it.

- **Issue:** the title and the task in hand.
- **Repro evidence:** the observed behavior and the facts that pin it
  down. Do not devise or run new test cases (the package is the whole
  world); instead note whether the repro evidence leaves the cause
  open, and read the diagnosis against what it does show.
- **Diagnosis:** the plan's stated cause, and the evidence it cites for
  it. Record whether the cause agrees with the repro facts and with
  the issue title.
- **Scope:** every file the plan says it will change, the statement of
  what it will not touch, and the reason for the changes.
- **Changes:** per file, the stated reason for modifying it; every file
  added or deleted and the justification given; the size of the work
  (from the Approach: "rewrite", new modules, number of files),
  estimated against the 500-line limit.
- **Test:** where the plan adds a test (the test case and the file it
  goes in).

Then compare the plan with the issue and the repro evidence for
consistency and alignment.

## 3. Check execution

Run the checks in this order, each against its own gathered evidence.
Grade `pass` only when every element below is met and you can quote the
fact that shows it. A missing required element is `fail`; the
`unclear` grade is only for a package that is ambiguous in a way you
cannot resolve from it (for example, a file named in a way that could
match two files). A check may be graded from its evidence notes
without re-reading the whole package.

1. **diagnosis** passes if:
   - the plan states what causes the bug;
   - it gives evidence supporting that cause;
   - the cause does not contradict the repro evidence and is relevant
     to the issue title.

   Fail if the cause or the evidence is missing.
2. **scope** passes if:
   - the plan names each file it will change;
   - it states what it will not touch;
   - it gives the reasoning for what the changes are for.

   Fail if any is missing. (The not-touching statement is required, as
   in the rubric.)
3. **changes** passes if:
   - each file to be modified is named with why it is modified;
   - every added or deleted file has a stated justification;
   - the change is minimal (under 500 lines).

   Fail if any is missing, an add/delete is unjustified, or the change
   is 500 lines or more.
4. **test** passes if the plan adds a test (a new or extended test
   case, naming where it goes). Fail if it has no test, or only says
   to run existing tests or check manually.

Judge the outcome, not the polish: a short plan that meets a condition
passes, and a long confident one that does not, fails. One check's
result never changes another's.

## 4. Verdict assembly

1. Apply the rubric's verdict rule: `accept` if all four checks are
   `pass`; otherwise `reject`. Preferred checks do not exist and never
   change the verdict.
2. `unclear` counts as `fail`, so any `fail` or `unclear` rejects.
3. In the summary, quote the deciding evidence for the first failing
   check in execution order; if all pass, quote the diagnosis evidence.
4. Finish with the JSON block `SKILL.md` requires (`item`, the four
   checks named exactly as in the rubric, each with a one-line
   `evidence`, and `verdict`) as the last thing in the output.
5. List any procedure gaps you hit.
