# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-alive| the last 5 default-branch commit dates in repo facts| at least 2 commits in last 90 days | required |
| responds-to-issues | maintainer reply times in last 5 issues | a reply within 90 days | preferred |
| shiped-recently | latest release is recent | a release in the last 90 days | preferred |
| good-to-start | labels | contains 'good first issue' label | preferred |
| unclaimed |  linked PRs | is empty or linked_prs state is closed and less than 2 and issue state is 'open' | required |
| fits-my-style |  contribution policy | contains CONTRIBUTING.md file | preferred |


## Verdict rule

Accept if every required check passes; preferred checks never change the verdict, they rank accepted issues; unclear counts as fail and must reject.