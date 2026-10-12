---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

<!--
THIS IS THE PART YOU WRITE, and it is the last one: the frame itself.
Weeks 1 through 3 handed you a working SKILL.md and you filled the
files behind it; this week the frame ships as headings, and you write
what it says. The frontmatter above and the section headings below are
fixed (CONTRACT.md's layout rule); the instructions under each heading
are yours. Write instructions to the tool, in the imperative, the way
weeks 1-3's frames spoke to you: what to read, in what order, what to
refuse, what to emit. Your executor in the rotation is the test: a
frame gap they hit (cannot tell what the tool reads, or how a verdict
gets assembled) is a missing sentence here.

One section is not yours: the JSON schema in "Verdict and output" is
reproduced from CONTRACT.md verbatim and may not be altered. Your
words decide everything around it.
-->

## The question

<!-- State, in your words, the one question this tool answers and what
a PR package is: what artifacts it contains and what they are read
against. CONTRACT.md fixes the question; your frame has to say it so
the tool cannot wander into grading something else. -->

You are grading one PR package to answer a single question: is this
ready to submit? A package is a candidate pull request (its title,
description, commits, diff, and test evidence), read against the plan
it claims to implement and the issue that plan belongs to. Grade
exactly one package per run. Never answer a different question (not
"is this a good idea?", not "is the plan good?", not "is the code
elegant?"), and never answer from gut feel: you answer by executing
`procedure.md`, which applies `rubric.md` to evidence gathered per
`references/evidence-guide.md`.

## Inputs and modes

<!-- Define both modes. Live mode: name every input (your plan.md with
its deviation notes, your branch's diff, your draft PR title and
description, your test evidence, your issue), where each comes from,
and what a house-chain student reads instead. The branch's diff is
everything the branch changes relative to the repo's default branch:
`git diff main...HEAD` (three dots), run from the working copy, is
the command that produces it; name the source that concretely. Eval
mode: state that the bundle is the whole world, nothing is fetched,
and every check runs with the full verdict rule. -->

Run in exactly one of two modes.

- **Live mode**: the student's own submission, checked before it goes
  out. Inputs and where each comes from:
  - `plan.md`, including its deviation notes, from the student's
    working copy. A deviation recorded in the plan is part of the plan
    and is graded against; a deviation that exists only in the diff
    is not recorded.
  - The branch's diff: everything the branch changes relative to the
    repo's default branch. Produce it with `git diff main...HEAD`
    (three dots), run from the working copy.
  - The draft PR title and description, as the student supplies them
    (a file or pasted text). The commits come from `git log main..HEAD`.
  - The test evidence: the test output or run notes the student
    supplies or quotes in the description.
  - The issue: gather issue-side evidence from the real repo (the
    thread, the PR template, the stated contribution policy) via
    `gh`, the GitHub API, or the web; `references/evidence-guide.md`
    says where each signal lives.
  - A student on the house chain has no issue and plan of their own
    to read: read the house plan and the house repro pack instead.
    The same checks grade the same things there.
  Read the package the way a maintainer will read the PR: it is what
  the diff, the title, and the description contain and quote, not other
  files in the working directory.
- **Eval mode**: a package bundle (a markdown file, one of the frozen
  PRs in `eval/packages/`) is the whole world. Use ONLY the bundle
  text as evidence. Fetch nothing, run no `git` or `gh` commands, and
  read no other file for facts. Eval mode always grades a complete
  package: every check, full verdict rule.

If you cannot tell which mode you are in, ask once; do not guess.

## The scope seam (live mode only)

<!-- Tell the tool when to read scope.md, what to do with the rules it
finds there, what to refuse, and what to do when the scope's repo line
is an unfilled placeholder. State that eval mode ignores scope.md
entirely. CONTRACT.md names the required behavior; your frame has to
instruct it. -->

In live mode, read `scope.md` before anything else, before reading any
other input. It names the repo the student's PR must live in and the
house rules of that environment; apply those rules when reading
evidence. Refuse to grade a PR that lives outside the scoped repo.
If the `Repo:` line still carries an unfilled placeholder, stop
without grading and tell the student to fill the `Repo:` line in
`scope.md` with their section's Path Review repo. Never guess a scope.
In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

<!-- Tell the tool when to read voice-guide.md, which outgoing text it
gates (the PR title and description), how to report a broken rule, and
why it never changes the verdict on its own. State that eval mode
ignores it entirely. -->

In live mode, after the scope check, read `voice-guide.md`: the
student's own rules for how they write upstream. Hold the outgoing PR
text, the title and the description, against those rules. Report
each rule the draft breaks in the readable summary, quoting the rule
and the offending text. The voice guide never changes the verdict on
its own, because voice is personal; it moves the verdict only if
`rubric.md` has a check that reads it. In eval mode, ignore
`voice-guide.md` entirely: voice carries no gold labels, and the
universal communication-quality checks live in the rubric.

## Component reads

<!-- Tell the tool how the pieces connect: rubric.md defines the
checks and the verdict rule, references/evidence-guide.md maps where
each evidence family lives, procedure.md is executed as written. Say
what the tool does when the procedure is silent on a step (report the
gap, never improvise around it) and what it does when rubric.md or
procedure.md has no content (the refusal rule, stated as an
instruction). -->

Read these components and keep their roles separate:

- `rubric.md` defines the checks (each with its evidence, pass
  condition, and weight) and the verdict rule that combines check
  grades into `accept` or `reject`.
- `references/evidence-guide.md` maps where each evidence family
  lives in a PR package (and, live, on GitHub) and what good looks
  like there.
- `procedure.md` is the operating procedure: the read order, how each
  evidence family is gathered, how a check executes, and how grades
  become the verdict. Execute it exactly as written.

Where the procedure is silent on a step, do not invent one: note the
gap in your readable summary, because a procedure gap is feedback the
student needs. If `rubric.md` has no checks written, or `procedure.md`
has no steps written (comments and template text do not count), stop
and say so: this tool refuses to grade without a rubric AND a
procedure, by design. Never invent checks at runtime.

## Verdict and output

<!-- State the binary verdict space (accept means submit, reject means
hold) and instruct the tool to end its reply with the fenced JSON
block below, valid and last, with nothing after it. The schema is
CONTRACT.md's, verbatim; do not edit it. -->

The verdict is binary: `accept` (ready to submit) or `reject` (hold).
There is no third verdict, no "accept with reservations", and no
score; put reservations in the check evidence lines. Assemble the
verdict only as `rubric.md`'s verdict rule directs. Before the JSON
block you may show a short readable summary (a line per check, plus
voice-guide notes and procedure gaps in live mode). End your reply
with the fenced JSON block below, valid, last, with nothing after it.
Use the PR URL as `item` in live mode and the bundle id in eval mode.
Do not alter, extend, or reorder the schema.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

<!-- Write the standing rules the tool grades under: evidence first,
grade the thing not the polish, the rubric decides, the procedure
decides how, and how unclear grades are treated when the rubric's
verdict rule is silent. CONTRACT.md states each as a guarantee; your
frame has to make them instructions. -->

- Evidence first: never grade a check without naming the fact or
  quote that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: a terse complete PR can be ready
  and a beautiful confident one can be hiding drift. Read the diff,
  tests, and claims against the plan and the issue, never the
  formatting.
- The rubric decides, not you: if a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary if you want; the fix belongs in the rubric, not in the run.
- The procedure decides how, not you: follow `procedure.md` as
  written and report its gaps instead of papering over them.
- Treat `unclear` as the rubric's verdict rule directs. If the rule
  is silent, treat `unclear` as `fail`: a PR you cannot verify from
  the package is a PR that is not ready to submit.
