# Rubric: is this a good first issue?

Goal: a small, easy bug fix (not a new feature) that one person can finish in two weeks or less, in an active repo where maintainers answer questions.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-alive | the last 5 default-branch commit dates in repo facts | at least 2 of the last 5 commits are within 90 days of the capture date (today in live mode), and the repo is not archived | required |
| responds-to-issues | maintainer first-response sample in repo facts, and author_association (Owner, Member, Collaborator) on comments in this thread | at least one reply within 90 days from an Owner, Member or Collaborator to an issue opened by someone else (a maintainer replying only on their own issues does not count) | required |
| unclaimed | assignees, linked PRs, and the Comments section | issue state is 'open', no assignee, no open linked PR, fewer than 2 closed linked PRs, and no comment claiming it ("I'll take this", "working on this") without a maintainer answer or a reply older than 30 days; fails if the issue was opened by a maintainer (Owner, Member, Collaborator) who has opened a fix PR within the last 7 days for a sibling issue from the same batch, or has a same-day pattern of filing an issue and its PR together | required |
| is-a-bug-fix | issue title, body, and labels | the issue reports behavior that is wrong (error, crash, incorrect output, regression), and is not a feature request, enhancement, refactor, question, discussion, or tracking/umbrella issue | required |
| small-and-easy | issue body and thread; any maintainer comment on scope | one bounded fix that plausibly touches a few files and fits in two weeks; fails if a maintainer says it needs core or design changes, the design is still debated, or earlier attempts were abandoned | required |
| not-docs-only | labels, title, and body | the fix is in code or tests; labels like 'documentation', 'docs', or 'topic: documentation' and typo-only fixes fail | required |
| ai-contribution-allowed | contribution policy line in repo facts, plus any AI policy file | no outright ban on AI-assisted contributions; conditions such as disclosure or human review are fine; silence passes | required |
| good-to-start | labels | contains a 'good first issue' or 'help wanted' label | preferred |
| has-repro | issue body | includes steps to reproduce, a stack trace, or a failing snippet | preferred |
| shipped-recently | latest release in repo facts | a release within the last 90 days | preferred |
| has-contributing-guide | contribution policy line in repo facts | repo has a CONTRIBUTING.md | preferred |

## Verdict rule

Accept if every required check passes; preferred checks never change the verdict, they rank accepted issues; unclear counts as fail and must reject.
