# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

<!-- [Link to the pull request you opened. It must be the pull request's own page on the Path
Review repo, not your fork's branch page.] -->


https://github.com/codepath/pathreview-ai301-fa26-s3/pull/91


**Branch**

<!-- [The name of the branch the pull request comes from, exactly as it appears in your fork.
The naming shape is a type prefix, then the issue number, then a short description. **The
issue number in the branch name must be the number of the issue the pull request fixes** —
a name carrying any other number does not satisfy this field.] -->

fix/72-hash-error


## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

<!-- [The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.] -->

1. 20/20 scored items (bar: 18/20: PASS), kept as `eval-run1.txt`.
2. 20/20 scored items (bar: 18/20: PASS), the final run, recorded in `eval-run.txt`.

`rubric.md`, `SKILL.md` and `procedure.md` have the same fingerprints in both runs. Only
`evidence-guide.md` changed between them, so the second run checked that the edit did not
move any verdict.

**Package analysis**

<!-- [Pick one scored package (`pkg-01` through `pkg-20` — the four `calib-` packages are never
scored). Name it by id, say what your rubric decided and what the gold label said, and
explain why your rubric read it that way.] -->


pkg-01 (pandas-dev/pandas#66657, standards-wall). My rubric decided reject. The gold label is reject, so we agree.

The fix itself is strong. The diff touches only tokenizer.c and test_c_parser_only.py, which are exactly the files the plan names. The test evidence shows a before and after on the plan's repro (99999, then 100000), a clean ASAN run, and the parser suite passing (2841 passed). That means diff-matches-plan, description-matches-diff, test-evidence and clean-diff all pass on their stated conditions.

The package fails on the repo's stated asks. The repo-facts block says pandas' PR template is a required checklist: "closes #xxxx with the issue number, tests added and passed, all code checks passed (pre-commit), type annotations added to new arguments, and an entry in the latest doc/source/whatsnew/vX.X.X.rst when fixing a bug". The candidate description has none of this. It has no Closes #66657, no checklist, and no pre-commit result. The diff contains no whatsnew file, which the repo requires for a bug fix. My template-complete check fails when "a required section is missing, empty, or a placeholder, the issue link is absent or names a different issue, ... or a required ask is ignored." All three of those happen here. repo-checks is also likely to fail, because pre-commit appears in the template's checks and is never shown.

My rubric reads it this way because the standards-wall checks grade whether the package answers the repo's asks, not how good the code is. A polished fix with decisive evidence doesn't cover a visibly ignored checklist. That is the "grade the thing, not the polish" rule in SKILL.md. The honest-outcome rule doesn't rescue it either. The missing items are silent: nothing in the description discloses them, so they aren't a disclosed shortfall.





**Check rationale**

<!-- [Quote one check from the `rubric.md` you uploaded to `tools/pr-precheck/`, exactly as it reads now.
Then say why it reads that way — what you revised to get there, or what you rejected in
favour of it.] -->

`repo-checks` (required), pass condition:

> Passes if every check the repo's template or docs ask for is shown with its result pasted, or its absence is explained (for example "no tests ran" pasted as is), and any failing check is reported with its reason rather than hidden. Fails if a stated check is never run or never shown, or a failure is hidden or contradicted by the evidence. The CI box left unticked pending the PR does not fail.

It reads this way because it grades the outcome, meaning whether each check the repo asks for
is visible with its result, and not how the evidence is formatted. Two clauses keep it from
failing PRs it should accept. First, an explained absence passes, and the example is
"no tests ran" pasted as is. That is the honest-outcome rule, and it matches my own PR #91,
where `make test-integration` collected 0 items and I pasted the output instead of ticking
the box. Second, the unticked CI box does not fail, because CI only runs after the PR is
open and no author can show it beforehand. I wrote the check against "a stated check is never
run or never shown" so a polished package that skips `pre-commit` or lint still fails, as
pkg-01 does.

**Trade-offs**

<!-- [Every check gives something up. Any one of these is a complete answer: a package whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.] -->

A case I accept `repo-checks` will miss: it only reads the package, so it cannot tell whether
a pasted result is real. A package that pastes plausible lint or typecheck output that was
never run, or that gives a thin reason for skipping a check ("explained" is the only bar), passes.
I accepted this because telling real output from invented output would need a second source
such as CI logs, which an eval package does not carry. The check judges whether the evidence is
visible and consistent with the diff, and does not try to verify it independently.

Nothing changed elsewhere between the two runs: both scored 20/20 with identical verdicts on
all 20 packages, and only the order of completion differed. The `rubric.md` fingerprint
(`1b70b4dfaf796555`) is the same in both, so the one edited file, `evidence-guide.md`, did not
move any package.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
