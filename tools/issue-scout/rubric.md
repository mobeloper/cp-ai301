# Rubric: is this a good first issue?

Goal: a small, easy bug fix (not a new feature) that one person can finish in two weeks or less, in an active repo where maintainers answer questions.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-alive | the last 5 default-branch commit dates in repo facts | at least 2 of the last 5 commits are within 90 days of the capture date (today in live mode), and the repo is not archived | required |
| responds-to-issues | repo-level evidence: maintainer first-response sample in repo facts (recent issues opened by someone else, and who replied); this thread's author_association is a bonus, never the only source | in the repo, at least one reply within 90 days to an issue opened by someone else, from a human Owner, Member or Collaborator, **or from a login that authored a default-branch commit in the last 90 days** (many maintainers show as CONTRIBUTOR or NONE because their org membership is private; bot accounts do not count either way). Alternatively, a maintainer (same definition) merging or reviewing a pull request opened by someone else in the last 90 days counts as a reply, for repos where outside contributors open PRs directly instead of issues. A maintainer replying only on their own issues or PRs does not count. This is a property of the repo, not of this thread: a brand-new issue with 0 comments passes if the repo sample shows maintainers answering others, and fails if the sample shows none | required |
| unclaimed | assignees, linked PRs, and the Comments section | issue state is 'open', no assignee, no open linked PR, fewer than 2 closed linked PRs, and no comment claiming it ("I'll take this", "working on this") without a maintainer answer or a reply older than 30 days; fails if the issue was opened by a maintainer (Owner, Member, Collaborator) who has opened a fix PR within the last 7 days for a sibling issue from the same batch, or has a same-day pattern of filing an issue and its PR together, unless the issue invites outside contributors (a 'good first issue' or 'help wanted' label, or a 'for first-time contributors' or similar section), in which case the maintainer-opened clause does not apply (the other conditions still do) | required |
| is-a-bug-fix | issue title, body, and labels | the issue reports behavior that is wrong (error, crash, incorrect output, regression), or asks for one self-contained test, test fixture, or input/schema validation addition with stated acceptance criteria (a test gap), including when a maintainer filed it as a roadmap item for contributors; it is not a feature request, enhancement, refactor, question, discussion, or tracking/umbrella issue | required |
| scope-feasible | Issue body: the work it asks for | The issue describes one identifiable piece of work someone could start: a behavior, a gap, or a task you can point at. It fails only when there is no single piece of work in it: a codebase-wide checklist, an "improve X everywhere", or a container issue tracking many sub-tasks. An open design edge, an "etc.", or several files involved never fails this check; that raises the difficulty note instead. | required |
| no-special-hardware | issue body, labels, title, and any environment section | the fix and its test can be done on an ordinary laptop: no GPU, TPU, or other accelerator, no multi-node cluster, no specific device, and no paid cloud account. Fails when the body, labels or environment section name such hardware (CUDA, ROCm, H100/H200, TPU, TensorRT, multi-GPU, etc.) as needed to reproduce or verify | required |
| not-docs-only | labels, title, and body | the fix is in code or tests; type annotations, error-message changes, and added tests count as code. A docs-labelled issue passes only if it also requires a test or code change; documentation-only tickets (labels like 'documentation', 'docs', 'topic: documentation', or docstring-only, README-only or typo-only fixes) fail | required |
| macos-compatible | issue body, labels, title, and any environment section | the fix and its test can be done on macOS (the contributor's MacBook Pro). Fails when the issue is Windows-only or tied to another OS or platform (Linux-only, Android, iOS, WSL, PowerShell) as needed to reproduce or verify; passes when it also reproduces on macOS or names no platform | required |
| ai-contribution-allowed | contribution policy line in repo facts, plus any AI policy file | no outright ban on AI-assisted contributions; conditions such as disclosure, human review, or writing the PR description in your own words are fine; silence passes | required |
| good-to-start | labels (case-insensitive) | contains a beginner or help label: 'good first issue', 'good-first-issue', '🌱 Good first issue', 'Easy', 'easyfix', 'easy-fix', 'Difficulty: Easy', 'first-contribution', 'help wanted', or 'help-wanted' | preferred |
| has-repro | issue body | includes steps to reproduce, a stack trace, or a failing snippet | preferred |
| shipped-recently | latest release in repo facts | a release within the last 90 days | preferred |
| has-contributing-guide | contribution policy line in repo facts | repo has a CONTRIBUTING.md | preferred |
| merges-newcomer-prs | repo-level: closed PRs of the last 90 days (`gh search prs --repo R --state closed --closed ">DATE"`), keeping authors with association NONE, FIRST_TIMER, FIRST_TIME_CONTRIBUTOR or CONTRIBUTOR | at least 3 of those closed PRs were merged (not just closed) | preferred |

## Difficulty note

With each accepted candidate, report a one-line difficulty note in the read-out summary:

- **straightforward**: the issue names where the change lives and the change is bounded.
- **moderate**: the location is yours to find, or several files are involved.
- **ambitious**: an open design edge, or an unfamiliar subsystem; name the edge, because claiming this issue means raising it with the maintainer early.

The note is information for the chooser, never a verdict input: it does not appear in the fenced JSON block, it never changes accept or reject, and in eval mode it is skipped entirely.

## Verdict rule

Accept if every required check passes; preferred checks never change the verdict, they rank accepted issues; unclear counts as fail and must reject.
