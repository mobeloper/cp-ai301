The ship-gate printed ship-gate: PASS and exited 0 on branch test/issue-5295-successive-release-fixture. It ran with your title and pr_draft.md, and the clone had no uncommitted tracked changes.

- Repo checks: all three of the manifest's checks passed: lint, format check, and the risk unit tests (pytest tests/unit/risk -q).
- pr-precheck: all 8 checks passed, including the preferred title-and-issue check. ai-disclosure passed through the exception I added to your rubric: the description quotes the AGENTS.md rule that bans AI and tool attribution in PR text and contains none.
- Gate output: it ran pr-precheck on its own and wrote a longer summary. That summary has the same grades as my earlier run.

The pass is good for 30 minutes. Opening the PR through Claude Code in that window lets it through without a re-run, but only if nothing in the clone changes first. That includes pr_draft.md, plan.md, test_evidence.md and the branch commits.