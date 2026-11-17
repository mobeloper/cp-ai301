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

## 2026-11-06 rubric patch: scope informs instead of rejecting
- Replaced the `small-and-easy` check (one bounded fix, fails if design is debated or earlier attempts were abandoned) with `scope-feasible`, which only fails when the issue is not one piece of work (umbrella, codebase-wide checklist). Open design edges, "etc.", and multi-file changes now raise a difficulty note (straightforward / moderate / ambitious) instead of rejecting.
- Driver: real issues rarely have the tidy scope of the unit-1 sandbox; the strict check rejected most real candidates (e.g. #3729 and #1920 above).

## 2026-11-06 scan (21 queries, added repos batches 1-3)
- 21 queries, 585 unique after the leak guard, 472 after ranking filters. Q3, Q7, Q15, Q17, Q21 returned 0. Q1, Q2, Q16 hit the 100 cap, so results were truncated.
- Q2 (all-owners typescript bug) returned 99 `anthropics/claude-code` results of 100; the claude-code drop filter handles it, but it crowds out real typescript results. Consider `--repo`-scoping it or excluding that repo in the query.
- Top 5: OpenHands#19432 (reject, scope-feasible: fix already shipped in LiteLLM 1.109.0), litellm#50394 (reject, unclaimed: open PR #49926; responds-to-issues unclear), DeepSpeed#9024 (ACCEPT, ambitious), scikit-learn#35330 (ACCEPT, straightforward), langgraph#9620 (reject, responds-to-issues unclear).
- Chosen: scikit-learn#35330 (`SelectorMixin.inverse_transform` fails when no features are selected). Claimed upstream 2026-11-06.
- Friction: ranking is bug label, then good first issue, then fewest comments, so the top 15 were all issues filed the same day with 0 comments. Fewest-comments favours brand-new issues with no track record. A minimum-age filter (e.g. open at least 2 days) would help.
- Friction: hardware-heavy bugs (DeepSpeed tensor-parallel, vllm GPU) rank the same as pure-Python ones. Difficulty is only a note in the rubric, so the ranking cannot see it.
- Friction: `responds-to-issues` came out unclear for litellm and langgraph because their maintainer replies are mostly bots or I found none in a 100-comment sample. The rubric does not say whether bot COLLABORATOR replies (e.g. all-hands-bot) count.
- Friction: scikit-learn#35330 carries a "not yet ready for a PR" banner and a `Needs Triage` label, but the rubric passes it. Rubric gap: a maintainer-stated "not ready for PR" label is not checked.
- Friction: the AI-policy check is binary (ban vs conditions). scikit-learn, DeepSpeed and langgraph all set conditions (disclosure, human review, maintainer pre-approval), which are terms to follow, not rejections.

## 2026-11-06 patch: responds-to-issues is repo-level; min-age ranking filter
- Trigger: langgraph#9620 was rejected as `responds-to-issues` unclear; the student pointed out the issue was 4 hours old. The check is repo-level, not thread-level (a deeper repo sample still found no human maintainer replies to outsiders since 2025-06, so the reject stood), but the wording ("in this thread") invited the confusion.
- Rubric: `responds-to-issues` now states it is a repo property, human Owner/Member/Collaborator only (bots do not count), and that a 0-comment new issue passes or fails on the repo sample alone. Resolves the earlier bot-COLLABORATOR question (all-hands-bot).
- Strategy: added `createdAt` to every query's `--json` fields and a ranking filter dropping issues created less than 3 days ago. Reason: fewest-comments-first put same-day issues at the top of the top-N, where the thread says nothing about maintainer responsiveness.
- Not changed: no "young issues are exempt" rule; that would let through repos whose maintainers never answer outsiders.

## 2026-11-06 strategy: added general-purpose Python repos (batch 4)
- Added 2 queries (bug and good-first-issue, python) for: django, flask, fastapi, wagtail, scrapy, pandas, pytorch-lightning, matplotlib, black, pydantic, poetry, requests, rich, TheAlgorithms/Python, cve-bin-tool, home-assistant/core, locust, ansible. Skipped scikit-learn and keras (already in batch 3).
- Not yet run. These repos are general Python, not agentic AI, so they trace to "python" + "small bug fixes" + "active repo" in scope.md, not to the agentic-AI sentence.
- Two copies of search-strategy.md exist: the installed one (~/.claude/skills/issue-scout/) and tools/issue-scout/ in the repo. Only the installed one is read by the skill; the repo copy is stale.

## 2026-11-06 scan 2 (23 queries, batch 4 + 3-day age filter): 0 accepted of 5
- Top 5: TensorRT#4954 (maintainer same-day issue+PR), jax#42263 (optimization request, TPU), litellm#49688 (CVE question), openai/evals#1856 (stale repo, eval proposal), vllm#63793 (maintainer tracker, fix in humming repo). Full output in scan-output.md.
- Batch-4 repos did not reach the top 5: the first one ranks 14th (pandas#58299). Ordering is bug label, then fewest comments, then recency, so 0-comment issues from any repo beat batch-4 issues that have comments.
- Friction: hardware-heavy bugs and maintainer-filed issues rank first; the opener's authorAssociation is not in the search fields, so ranking cannot see it. Consider a rubric note on hardware and a ranking tie-break on repo size.
- The 3-day filter drops scikit-learn#35330, the previous best pick.

## 2026-11-06 strategy expansion (batch 5): large OSS orgs, wider languages, no special hardware
- scope.md fit profile: languages now python, javascript, typescript, css, scss, html, java, react, dockerfile (go/c++/rust still avoided); orgs pytorch, open-telemetry, apache, opensearch-project, backstage, kubernetes, openedx, hackforla; labels good first issue / help wanted / bug / easy; new line "avoid any issue that needs special hardware".
- rubric.md: new required check `no-special-hardware` (fix and test doable on an ordinary laptop; fails on GPU/TPU/accelerator/cluster/device/paid-cloud needs). Driver: 3 of 5 graded issues in scan 2 needed GPU/TPU hardware.
- search-strategy.md: +19 queries (42 total): 6 owner groups x 3 labels (good first issue, help wanted, bug) + one `easy` query. Findings from testing gh: repeated or comma-joined `--label` ANDs, so OR needs separate queries; comma-joined `--language` ORs. Added `--sort updated --order desc` so the 100 cap keeps the newest. `easy` returned 0 everywhere; kept because it is in the profile.
- Ranking: label priority is now (1) bug + friendly label (good first issue/easy/help wanted), (2) bug, (3) good first issue/easy, (4) help wanted; then fewest comments, then recency. Owner allowlist extended to the new orgs.
- Result: 1286 unique, 1014 after filters; 26 issues in tier 1. Graded top 5: 2 accepted (both openedx/openedx-platform: #39114, #39119), 3 rejected.
- Friction: kubernetes returned almost nothing (the language list drops its Go code); apache and backstage queries hit the 100 cap even with newest-first sorting; `[ENHANCEMENT]` issues labelled `bug` (opensearch dashboards-search-relevance#757) still reach the top 5 because ranking only reads labels and titles starting with Feature.

## 2026-11-06 strategy + rubric update for the beginner-friendly Python repo list (batch 6)
- Repo facts checked with gh: django/django has GitHub issues disabled (tickets are in Trac, "Easy pickings"), so it cannot be scanned or graded with gh; intel/cve-bin-tool was renamed ossf/cve-bin-tool (the old name made the leak guard drop every result, so batch 4 silently returned nothing for it) and is fixed; saltstack/salt is still the current name.
- Label vocabularies differ: good first issue (most), good-first-issue (mypy), 🌱 Good first issue and first-contribution (matplotlib), Easy (scikit-learn), easyfix (ansible), help wanted (zulip, sklearn, lightning, scrapy), help-wanted (salt, home-assistant, frappe); fastapi's "easy close" means easy to close, not easy to fix.
- search-strategy.md: +9 queries: bug and good-first-issue for the 9 new repos (httpx, mypy, hatch, salt, open library, wger, zulip, erpnext, frappe), and 7 label-variant queries over all 20 listed repos. Test counts: help wanted 57 (zulip 25, sklearn 17, lightning 14), help-wanted 5, good-first-issue 1, easyfix 1, 🌱 Good first issue 6, first-contribution 12; `Easy` and `easy-fix` returned 0 (kept `Easy`, dropped easy-fix and Difficulty: Easy). Ranking now compares labels case-insensitively with the beginner and help label lists above, and drops `easy close`.
- The kubernetes queries and the all-org `easy` query from batch 5 were no longer in the file when I edited it ; I did not restore them.
- rubric.md: `not-docs-only` now says type annotations, error-message changes and tests count as code while docstring/README/typo-only fail; `ai-contribution-allowed` counts "write the PR description in your own words" as a condition, not a ban; `good-to-start` accepts the label variants; new preferred check `merges-newcomer-prs` (at least 3 merged PRs from outside contributors among closed PRs in 90 days), with an evidence-guide row. Preferred, so no verdict changes.
- Not changed on purpose: many of the pasted repos tag documentation work as beginner-friendly; the rubric still rejects docs-only issues, as your profile asks for code bug fixes.

## 2026-11-06 scan 4 (47 queries, batch 6)
- Same top 5 as scan 3 (openedx #39114 and #39119 accepted; opensearch #616, apache fesod #272, opensearch #757 rejected), so grades were reused after re-checking live state. Details in scan-output.md.
- Friction: the listed beginner-friendly repos rank low (best-per-repo ranks: lightning 9, zulip 27, mypy 30, erpnext 39, frappe 46, pandas 122, sklearn 266). 788 "bug only" results sort by fewest comments, which favors a few large repos. Proposed strategy change: limit the top-N to at most 1 or 2 issues per repo.
- The new label-variant queries mostly returned 0 (Easy, easyfix, help-wanted, good-first-issue, 🌱 Good first issue).

## 2026-11-06 scan 5 (per-repo quota 20, top 10 graded): 2 accepted of 10
- Strategy: per-repo cap of 20 plus a no-label top-up for any listed repo below 20 (63 of 70 needed one); top-N raised from 5 to 10. Renamed repos fixed: explodinggradients/ragas -> vibrantlabsai/ragas, mendableai/firecrawl -> firecrawl/firecrawl (they had silently returned nothing, like intel/cve-bin-tool earlier).
- Coverage: only 31 of 70 listed repos reach 20 issues after the filters; 8 have 0 (tensorflow, flask, fastapi, adk-python, locust, three TypeSafeAI repos). The filters, not the search, are the limit.
- Result: openedx #39114 and #39119 accepted again; ranks 6-10 (maka #3328 and #3312, seatunnel #10172, lightning #18645, opensearch-java #611) all rejected, mostly on `unclaimed` (recent claim comments with no maintainer reply) or hardware/infrastructure. Near miss: apache/maka #3312 (unclear whether it needs a paid model account).
- Friction: the quota did not change the top 10, since ranking is applied after it; only a per-repo cap on the graded list would add more repos. Unlabelled top-up issues rank in tier 4 and never reach grading.
- Friction: gh prints "Invalid search query" for renamed repos on stderr; add an error check per query.

## 2026-11-06 scan 6 (round-robin ranking): 1 accepted of 10
- Strategy: the 3-per-repo cap was replaced by round-robin (sort by in-repo number first, then the existing order). Reused the scan 5 search results.
- Graded list: 10 repos instead of 8. New entries: otel python-contrib #1704 (reject, open PRs #5056 and #5062) and apache/beam #20983 (reject, needs an Apple Silicon Mac).
- Trade-off: openedx #39119 (accepted earlier) is now openedx's second-best issue and falls outside the graded list.
- Friction: tier-1 (bug plus a friendly label) still wins every round; none of the beginner-friendly repos from the pasted list reached the top 10. Open PRs mentioned only in comments and device-specific bugs are invisible to the pre-grading filters.

## 2026-11-06 scan 7 (4 reserved beginner slots + round-robin): 1 accepted of 10
- Strategy: 4 of 10 graded slots go to beginner-labelled issues without a bug label (round-robin over that group), 6 to the main round-robin. Reused the scan 5 search results.
- Beginner-only group: 103 issues in 39 repos. The reserved four (hackforla/website #8855, apache/magpie #1891, hackforla/peopledepot #731, hackforla/tdm-calculator #3399) were all rejected: none is a bug fix (a content edit, an eval-suite feature, a schema update, an icon swap).
- Friction: beginner label means easy, not bug. Fewest-comments ordering favors maintainer-filed 0-comment work items (hackforla). A rank filter on the title/labels for non-bugs (`feat(`, `role:`/`size:` work-item labels, "Update", "Dev:") could keep these out of the graded slots.

## 2026-11-06 scan 8 (non-bug title filter + reserved slots by repo diversity): 1 accepted of 10
- Strategy: new drop filter for titles starting with feat(, feat:, [feat, enhancement, [enhancement or the word "update"; reserved beginner slots are now one issue per repo, repos ordered by total result count (busiest first), no comment count. Reused the scan 5 search results.
- Reserved four: pytorch/pytorch #89197 (possibly already fixed on nightly), frappe/frappe #42122 (a feature request), hackforla/website #8850 (a content edit), matplotlib #32556 (claimed 2026-10-30; matplotlib closes AI-generated PRs for good first issues). All rejected.
- Friction: beginner-labelled issues on the biggest repos are often old or features; hackforla's content-edit work items still pass the title filter.

## 2026-11-15 scan 9 (priority repos first, fresh-issue ordering, macOS + isolated-fix scope): 0 accepted of 10
- Search: 58 strategy queries (24 priority-repo queries first) plus 13 priority top-ups; 2518 unique results, 2012 after the drop filters. All 17 priority repos reached the pool (adk-python only 3, browser-use 13). The graded 10 came from the priority pool only (sglang took two slots).
- Graded: sglang #27214 (reject: tracking list, needs GPU, open PR, many claims), dify #46447 (claim comment 2026-11-15, no maintainer reply), vllm #63465 (names an H200), paperclip #9153, openclaw #197381 (open PR #197811), langchain4j #7122 (a comment says it is fixed on main), fastmcp #6491, langgraph #9676 (claim comment 2026-11-12), sglang #37524 (tracking issue), browser-use #4803 (a question, answered).
- Near misses: paperclip #9153 (validation error message, bounded) and fastmcp #6491 (proxy elicitation bug, an open design edge). Both sunk only by responds-to-issues.
- Friction: responds-to-issues is not verifiable from author_association. Maintainers of these repos show as CONTRIBUTOR or NONE (org membership is private), and the replies are mostly bots (dosu, clawsweeper, github-actions). A reply sample checked against recent commit authors found no replies in any priority repo. Proposed rubric patch: also count a reply from a login that authored a default-branch commit in the last 90 days.
- Friction: claim comments without a maintainer answer are common in priority repos (dify, langgraph), and open PRs sit in cross-references. The pre-grading filters cannot see either; consider dropping results with `commentsCount` >= 1 whose newest comment is under 30 days old.
- Friction: tracking issues ("... bug tracking", "support new models") pass the label filters. Add a title drop for `tracking`, `roadmap` and `support new`.

## 2026-11-15 scan 10 (patched responds-to-issues, priority list B added): 1 accepted of 10
- Strategy: added Batch 10 (MONAI, nnUNet, openmed; 3 queries plus 3 top-ups) and reused scan 9's other results. 2756 unique results, 2096 after the drop filters. nnUNet's top-up returned 0 and MONAI's 16, so neither reached the graded list.
- Accepted: PrefectHQ/fastmcp #6491 (proxy elicitation bug, opened 2026-11-14). It passes responds-to-issues only through the patch: zzstoatzz, a default-branch committer, replied on #4920 (2026-08-31) and #5236 (2026-09-23). Difficulty: ambitious (protocol-level design edge). Ask to be assigned before coding, because fastmcp's bot closes PRs for unassigned issues.
- Still rejected: paperclip #9153 (the patch found no committer replies in a 10-issue sample), openmed #5296 (a fixture task opened by the owner, not a bug, and no replies to others) and #4814 (Android/Kotlin, a large migration), plus the scan 9 rejects (sglang #27214, dify #46447, vllm #63465, openclaw #197381, langchain4j #7122, langgraph #9676).
- Friction: openmed is a single-committer repo whose issues are mostly the owner's roadmap items, so responds-to-issues and unclaimed both struggle there. Owner-filed roadmap issues with contributor-ready acceptance criteria are wanted, not a problem (retracted 2026-11-15: do not drop sole-committer issues); the real gaps were is-a-bug-fix rejecting test and fixture tasks, and the unclaimed maintainer-opened clause. Both patched.

## 2026-11-15 scan 11 (full re-run, top 20 graded, patched rubric): 2 accepted of 20
- Accepted: openmed #5296 and fastmcp #6491. Full read-out is in scan-output.md.
- Friction: add `type/feature`, `maintainer only` and `workstream-rollup` to the label drop (hermes-agent #156302 and gemini-cli #21418 reached grading as features). Claim comments without a maintainer answer sank 6 of 20, and the pre-grading filters cannot see them.

## 2026-11-15 scan 12 (limit 500, quota 100, batches of 10 until 5 accepted): 6 accepted of 50
- Accepted: openmed #5296, #5121, #5097; chrome-devtools-mcp #3412; fastmcp #6398 and #6491. Stopped after batch 5 (cap is 5 batches). All graded issues came from the priority repos.
- Friction: add `type/feature`, `maintainer only`, `workstream-rollup` to the label drop. paperclip can never pass responds-to-issues (committers are its CONTRIBUTORs). Batches 2 and 3 repeated scan 11 rejects; a persistent reject list would save slots.
- Friction: GitHub's secondary rate limit hit Q11 and Q12 at --limit 500; the run script needs a retry-after-60s on HTTP 403.

## 2026-11-15 direct grading of 6 picks: 4 accepted, 2 reversed
- Correction to scan 12: openmed #5121 and #5097 are rejected on `unclaimed`. The owner opened 82 PRs on 2026-11-15/16, each closing a roadmap issue filed on 2026-11-07, including 19 help-wanted and 2 good-first-issue ones. Scan 12 only searched PRs by text and missed this.
- Friction: the maintainer-opened exemption for 'good first issue'/'help wanted' labels is too generous for openmed, where the owner PRs even labelled issues (#5422, #5448). Proposed rubric patch: drop the exemption when the same maintainer has opened PRs for 2 or more issues carrying the same invite label in the last 7 days. #5296 would then fail.

## 2026-11-16 direct grading of openmed #5295: accepted (straightforward)
- Accepted by the rubric as written. Full read-out is at the end of scan-output.md. Only has-repro failed (preferred, and a fixture task has no repro).
- Friction: `unclaimed` passes #5295 only through the maintainer-opened exemption, the same one the 2026-11-15 note proposes dropping. The rubric cannot see that the owner offered to assign it on #5682's thread, so the offer is the deciding fact, not a rubric signal. Keep the proposed patch, but let a maintainer's explicit offer to assign override it.
- Friction: scout ran from `gh api` calls by hand. Outsider PRs on sibling issues (#5682 by Mr-Neutr0n for #5296) never show up in the issue's timeline; only the owner's cross-reference does. Search PRs by text and by sibling number.
- Housekeeping: `~/.claude/skills/issue-scout/scan-output.md` does not exist, so the read-out went to this folder's `scan-output.md`, as in earlier entries.
