# Evidence guide: where evidence lives in a plan package

A plan package (the eval harness calls them packages; they live in
`eval/packages/`) is one markdown file with these parts, in this order:

1. `## Repo facts`: stars, latest release, bug-report template,
   contribution policy.
2. `## Issue`: the title (a `###` line), opener, labels, and body.
3. `## Thread highlights`: maintainer and contributor comments, each
   with an author association (COLLABORATOR, NONE...). May say
   "(no comments)".
4. `## Repro evidence`: the accepted repro report: environment,
   numbered steps with results, a control run when there is one, and
   Expected/Actual.
5. `## Candidate plan`: the plan under review.
6. `## Candidate plan comment`: the comment the author would post.

**Plan headings are not standard.** Match sections by the role they
play, not the label. Packages seen so far:

| Role | `pkg-01` | `calib-03` | `calib-01` |
|---|---|---|---|
| Cause | `### Diagnosis` | `### Diagnosis` | `Cause:` paragraph |
| Scope | `### Scope` | `### Scope` | `In:` / `Out:` inside `Change:` |
| Files/changes | `### Files`, `### Approach` | `### Changes` (numbered) | `Change:` (names one file) |
| Test | `### Test plan` | `### Test plan` | `Test:` paragraph |

If a role has no matching section anywhere in the plan, that evidence is
absent and the check that needs it fails.

In live mode the same roles are in the student's `plan.md`; the repro
evidence is their posted repro comment on the issue, the thread is the
live issue thread, and the repo facts are the repo's `CONTRIBUTING.md`,
`.github/` templates, and releases.

## Diagnosis and grounding

- **Where it lives.**
  - *Eval Bundle Package:* the plan's cause statement (`### Diagnosis` or `Cause:`).
    Read it against, in this order: the `## Issue` title; the
    `## Repro evidence` steps, especially the control run and the
    Actual line; and `## Thread highlights`, in case the plan borrows
    its cause from a comment.
  - *Live:* the same plan section against the student's repro comment
    and the issue thread.
- **What good looks like.** The cause names a specific mechanism or code
  location, cites a repro fact that supports it, explains the behavior
  the issue title names, and contradicts nothing in the repro evidence.
  A cause taken from a thread comment is only as good as the repro
  evidence behind it: check that a repro step shows it.
- **Examples.**
  - `calib-01` passes: the cause (the post-push refresh misses the
    branch-commits view, so push-status flags stay stale) matches repro
    steps 3 and 4 (color stale until the view is re-entered) and the
    issue title (push does not refresh the color).
  - `calib-03` fails: the plan's cause is the pager's key-binding
    registration, taken from a thread comment ("As identified in this
    thread"). Repro step 3 runs with `--paging=never` (no pager at all)
    and still takes 25.8 s, and step 4 with color off takes 0.2 s: the
    repro points at syntax highlighting. The plan's own Scope even
    calls highlighting "unrelated". Cause contradicts the repro.
  - `pkg-01` fails: the plan calls the Python-version difference a "red
    herring", while the repro shows the same command passing on 3.13.5
    and failing on 3.11.15.

## Scope

- **Where it lives.**
  - *Package:* the plan's in-scope and out-of-scope statements
    (`### Scope`, or `In:` / `Out:`), plus the files named under
    `### Files` / `### Changes` / `Change:`. Compare the file list with
    the files the diagnosis implicates, and with maintainer comments in
    Thread highlights about what is wanted.
  - *Live:* the same in `plan.md`, plus the issue thread.
- **What good looks like.** Every file to be changed is named, an
  explicit not-touching statement excludes something real (something
  someone could plausibly be tempted to change), and a reason is given
  for what the changes are for. The Changes/Approach steps stay inside
  the Scope statement.
- **Examples.**
  - `calib-01` passes: names one file
    (`pkg/gui/controllers/sync_controller.go`), says Out: "any change to
    how push status is computed, or to other views' refresh behavior",
    and says why (add the commits context to the post-push refresh).
  - `calib-03`'s Scope statement is well formed (one file, highlighting
    pipeline excluded), but what it excludes is exactly what the repro
    implicates; that is a diagnosis failure, not a form failure. Grade
    the Scope check on its own condition and let `diagnosis` carry the
    failure.
  - `pkg-01`'s Scope says "rewrite the request-item tokenizer": a
    rewrite signals a larger change than the reproduced bug needs; read
    it together with the Changes check.

## Executability

(This family supplies the evidence for the rubric's `changes` check.)

- **Where it lives.**
  - *Package:* the files and steps part of the plan (`### Files` and
    `### Approach`, `### Changes`, or `Change:`). Look for: each file
    and why it is modified, any file added or deleted and why, and
    words that signal size ("rewrite", new modules, many files).
    Check named files and symbols against the Repo facts and repro
    evidence.
  - *Live:* the same in `plan.md`, with the named paths checkable in the
    repo.
- **What good looks like.** A stranger could start without asking the
  author: files or functions are named, each change is a concrete
  action ("call `generate_default_bindings()` before registering bat's
  custom bindings") rather than "improve handling", any added or deleted
  file has a stated justification, and the described work is small
  enough to stay under 500 lines changed. Estimate size from what the
  plan describes; a one-callback change or a few numbered steps is far
  under 500, a "rewrite" or new subsystem is not.
- **Examples.**
  - `calib-01`: one file, one concrete change (add the commits context
    to the push callback's refresh scope); small.
  - `calib-03`: three numbered concrete steps in one file plus a smoke
    test; executable and small, even though the diagnosis is wrong.

## Test plan

- **Where it lives.**
  - *Package:* the plan's test section (`### Test plan` or `Test:`) and
    any step in Changes that adds a test (`calib-03` change 3: "Add a
    pager-integration smoke test"). Read it against the repro evidence's
    steps and Expected/Actual.
  - *Live:* the same in `plan.md` against the student's repro comment.
- **What good looks like.** Per the rubric, the plan adds a test: a new
  or extended test case that names where it goes. Better still, it
  reuses a repro step and names an observable result (output or exit
  code) that would fail on the unfixed code. Re-running the repro by
  hand or "run existing tests" does not add a test.
- **Examples.**
  - `calib-03` passes: a smoke test asserting the default binding set
    is registered (Changes step 3), plus a manual shift+G check.
  - `calib-01` fails: its Test is only the manual repro steps and some
    manual checks; no test is added.
  - `pkg-01` passes: new tokenizer cases in `tests/test_cli.py`.

## Honesty

Not graded by the rubric; it never changes the verdict. Use it for
summary notes.

- **Where it lives.**
  - *Package:* risks, unknowns, or assumptions in the plan and the plan
    comment, and the confidence of the cause's wording compared with the
    repro evidence.
  - *Live:* the same in `plan.md` and the draft comment; a mid-build
    deviation is recorded in `plan.md` (what changed and why), not only
    in the diff.
- **What good looks like.** Claims are as strong as the evidence:
  anything not reproduced or checked is stated as an unknown. Example of
  the opposite: `calib-03`'s comment says "I traced this to..." when the
  cause was only taken from a thread comment.

## Comms

Not graded by the rubric; it never changes the verdict. Use it for
summary notes, and in live mode alongside the voice guide.

- **Where it lives.**
  - *Package:* `## Candidate plan comment` read against Thread
    highlights (maintainer signals, other people's claims or PRs) and
    the Repo facts' bug-report template and contribution policy
    (including any AI-use rule).
  - *Live:* the draft comment against the live thread, `CONTRIBUTING.md`,
    `.github/` templates, and any AI policy file.
- **What good looks like.** The comment engages the thread, matches the
  plan rather than overstating it, and respects stated policy.
  `calib-01`'s comment responds to CONTRIBUTING's review-bandwidth note
  ("keeping it minimal"). `calib-03`'s comment repeats a cause that
  another contributor already claims is fixed in PR #3836, without
  acknowledging that PR.
