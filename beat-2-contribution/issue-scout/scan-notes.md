# Scan notes

## 2026-10-18 scan (first run): superseded, diagnosis was wrong
- Top 5 were all non-reputable repos, nothing graded. The note blamed multi-owner `--owner` lists. That was wrong: they filter correctly.
- Real cause: the example line inside the HTML comment of search-strategy.md ("gh search issues --label "good first issue" --language python ...", no `--owner`) starts a line with `gh search issues`, so it was run as a 15th query. It returned unfiltered good-first-issue results (Jeffy123-zhu, SoroScan, ...).
- Fixes: example line is now indented and marked `[EXAMPLE ONLY, NOT A QUERY]`; added an owner/repo allowlist ranking filter; SKILL.md now logs command + count + top repos per query.

## 2026-10-18 scan (re-run)
- 14 real queries, 403 unique issues, 308 after ranking filters. Top 5 all from named orgs (pytorch/*).
- GitHub secondary rate limit (HTTP 403) hit when the 14 queries ran back to back; a few queries silently returned 0. Add a ~8s pause between queries (or check for errors) in the scan.
- Graded top 5: 0 accepted.
  - pytorch/examples#391: reject, maintainer-alive (last commit 2025-09-01), stale 2018 issue.
  - pytorch/pytorch#89197: reject, claim comments (2026-07, 2026-08 follow-up) and a 2026-10-04 comment says the bug is already fixed on nightly.
  - pytorch/executorch#10819: reject, is-a-bug-fix (feature/refactor checklist; mostly already done).
  - pytorch/ao#1920: reject, is-a-bug-fix and small-and-easy (research benchmarking).
  - pytorch/tensordict#3005: reject, responds-to-issues unclear (see below).
- Friction: ranking prefers `good first issue` first, but in pytorch that label is mostly on stale, claimed or non-bug issues. The #1-#4 slots were wasted.
- Near miss: tensordict#3005 is a real bug with repro, root cause and fix location in the body, no assignee, active repo, AGENTS.md welcomes AI. But the only maintainer comments in 90 days are the maintainer (peterdsharpe) replying on their own issues, and they fix their own issues same day (#2976 opened and closed in 1 day). Rubric's responds-to-issues does not say whether a self-reply counts.

## 2026-10-18 scan (re-run 3, 15 queries incl. scipy GFI)
- scipy GFI query returned 0. Global (all-owners) queries were removed by the student before this run, so they never ran.
- Top 5: same four pytorch rejects as before, plus tensordict#3043 (new). Graded #3043: reject (responds-to-issues unclear, small-and-easy unclear).
- tensordict inventory is maintainer-generated: peterdsharpe opens bug issues and a matching PR within minutes (#3005 already has an open fix PR). Looks unclaimed on paper but is not available in practice. Rubric gap: `unclaimed` only reads assignee/linked PRs/comments at grading time.
- Dropping `enhancement` from the label filter lets TypeSafeAI/modex `[CR-n][P1]` roadmap items into ranks 7+ (title starts with `[CR-`, so the feature-title filter misses them).

## 2026-10-18 scan (re-run 4, after rubric + ranking changes)
- Changes: rubric `responds-to-issues` (self-replies don't count) and `unclaimed` (maintainer batch issue+PR pattern fails); ranking now bug label first, then good first issue, then fewest comments; `[P1]/[P2]` no longer dropped from titles (only `Feature`/`[Feature`).
- Top 5: tensordict#3043 (reject, unclaimed), tensordict#3042 (reject, unclaimed), modelcontextprotocol/servers#5176 (reject, blocked on python-sdk release; waiting-on-sdk), langchain-ai/openwiki#1066 (ACCEPT), modelcontextprotocol/python-sdk#3729 (reject, asks whether inclusive boundary is intended; not yet confirmed as a bug).
- Accepted: langchain-ai/openwiki#1066. Bug in `rewriteLinks()` (src/visualize/client.ts): in-page links to non-ASCII filenames 404. Repro, 0 comments, no assignee, no linked PR, collaborator colifran replies to outsiders' issues (Oct 9, Oct 7, Sep 22...), commits Oct 17/13/9, release Oct 9. CONTRIBUTING has binding rules for AI agents (single-concern PRs), not a ban.
- Watch: other openwiki issues attract "I'd like to take this" claims within a day; claim upstream quickly if taking it.
