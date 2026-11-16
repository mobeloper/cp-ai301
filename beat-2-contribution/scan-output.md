I ran the scan with all 21 queries and graded the top 5. Two candidates pass the rubric: scikit-learn #35330 and DeepSpeed #9024. I'd take scikit-learn #35330.

The three queries that returned nothing are the javascript one for the main owners, good-first-issue typescript for the main owners, and good-first-issue for scipy. The all-owners typescript query returned 99 anthropics/claude-code results out of 100; the leak guard dropped them. Q1, Q2 and Q16 hit the 100-result cap, so some matches in those were cut off.

That left 585 unique results, then 472 after the ranking filters.

Ranked read-out

Accepted

1. scikit-learn #35330: inverse_transform of feature selectors fails when no features are selected.
   - It came from Q19 (python bugs in the third batch of added repos). It ranked 4th for being labelled Bug, having 0 comments, and being updated today.
   - It has a copy-paste repro, and SelectKBest(k=0).inverse_transform raises a ValueError instead of returning zeros. The bug is in one place, SelectorMixin.
   - Maintainers (lorentzenchr, jeremiedbb, betatim) replied on other issues today. The latest release is 1.9.1 (Sep 11), and the repo has a contributing guide.
   - Difficulty: straightforward.
   - Caveat: the issue carries a banner saying "not yet ready for a PR" and a Needs Triage label. The rubric passes it, but you should ask for a triage OK in a comment before building. The AI policy requires disclosing AI use in the PR and reviewing every line, so it is conditions rather than a ban.
2. DeepSpeed #9024: AutoTP counts replicated GPTBigCode key/value rows once per tensor-parallel rank in the gradient norm.
   - It came from Q8 and ranked 3rd on the same signals.
   - It is unassigned with no linked PR, and collaborators stas00 and delock replied today. The latest release is Oct 29.
   - Difficulty: ambitious. It needs tensor-parallel and ZeRO knowledge, and likely multi-GPU hardware to verify. Raise the approach with a maintainer first.
   - The AI policy (in AGENTS.md) requires disclosure, human review and listing the test hardware used, so this one is conditions too.

Rejected

┌──────────────┬──────────┬─────────────────────────────────────────────────────────────────────────────────┐
│  Candidate   │ Query /  │                                     Sunk by                                     │
│              │   rank   │                                                                                 │
├──────────────┼──────────┼─────────────────────────────────────────────────────────────────────────────────┤
│ OpenHands    │ Q20,     │ scope-feasible: the issue says the fix is already in LiteLLM v1.109.0, so there │
│ #19432       │ rank 1   │  is no piece of work in this repo                                               │
├──────────────┼──────────┼─────────────────────────────────────────────────────────────────────────────────┤
│ litellm      │ Q1, rank │ unclaimed: open PR #49926 already fixes it. responds-to-issues was also         │
│ #50394       │  2       │ unclear, since I found no human maintainer replies                              │
├──────────────┼──────────┼─────────────────────────────────────────────────────────────────────────────────┤
│ langgraph    │ Q1, rank │ responds-to-issues unclear: I found no human maintainer replies. Their PR       │
│ #9620        │  5       │ template also requires a maintainer-approved issue with you assigned            │
└──────────────┴──────────┴─────────────────────────────────────────────────────────────────────────────────┘

Here is the grading as JSON, accepted first:

[
  {"item":"https://github.com/scikit-learn/scikit-learn/issues/35330","checks":[
    {"name":"maintainer-alive","grade":"pass","evidence":"last 5 commits all 2026-11-06, not archived"},
    {"name":"responds-to-issues","grade":"pass","evidence":"MEMBER replies on others' issues 2026-11-06 (lorentzenchr, jeremiedbb, betatim)"},
    {"name":"unclaimed","grade":"pass","evidence":"open, no assignee, 0 comments, no linked PR"},
    {"name":"is-a-bug-fix","grade":"pass","evidence":"inverse_transform raises ValueError instead of returning zeros"},
    {"name":"scope-feasible","grade":"pass","evidence":"one behavior in SelectorMixin.inverse_transform"},
    {"name":"not-docs-only","grade":"pass","evidence":"labels Bug, Needs Triage; code fix"},
    {"name":"ai-contribution-allowed","grade":"pass","evidence":"Automated Contributions Policy requires disclosure and human review, no ban"},
    {"name":"good-to-start","grade":"fail","evidence":"no good first issue or help wanted label"},
    {"name":"has-repro","grade":"pass","evidence":"SelectKBest(k=0) snippet"},
    {"name":"shipped-recently","grade":"pass","evidence":"1.9.1 released 2026-09-11"},
    {"name":"has-contributing-guide","grade":"pass","evidence":"CONTRIBUTING.md present"}],"verdict":"accept"},
  {"item":"https://github.com/deepspeedai/DeepSpeed/issues/9024","checks":[
    {"name":"maintainer-alive","grade":"pass","evidence":"commits 2026-11-05/06, not archived"},
    {"name":"responds-to-issues","grade":"pass","evidence":"COLLABORATOR stas00 and delock replied on #9025 and #9028 on 2026-11-06"},
    {"name":"unclaimed","grade":"pass","evidence":"no assignee, no linked PR, 0 comments; author is CONTRIBUTOR, not a maintainer"},
    {"name":"is-a-bug-fix","grade":"pass","evidence":"global grad norm counts replicated K/V rows once per TP rank"},
    {"name":"scope-feasible","grade":"pass","evidence":"one identifiable norm-accounting fix"},
    {"name":"not-docs-only","grade":"pass","evidence":"labels bug, training"},
    {"name":"ai-contribution-allowed","grade":"pass","evidence":"AGENTS.md demands disclosure and human review, no ban"},
    {"name":"good-to-start","grade":"fail","evidence":"no good first issue or help wanted label"},
    {"name":"has-repro","grade":"unclear","evidence":"body gives code references and analysis; I did not read it past the first 1,500 characters"},
    {"name":"shipped-recently","grade":"pass","evidence":"v0.19.9 released 2026-10-29"},
    {"name":"has-contributing-guide","grade":"pass","evidence":"CONTRIBUTING.md present"}],"verdict":"accept"},
  {"item":"https://github.com/OpenHands/OpenHands/issues/19432","checks":[
    {"name":"maintainer-alive","grade":"pass","evidence":"commits 2026-11-06"},
    {"name":"responds-to-issues","grade":"pass","evidence":"MEMBER neubig and malhotra5 commented 2026-11-06"},
    {"name":"unclaimed","grade":"pass","evidence":"no assignee, 0 comments, no linked PR"},
    {"name":"is-a-bug-fix","grade":"pass","evidence":"tool calls dropped"},
    {"name":"scope-feasible","grade":"fail","evidence":"issue says the fix is already in LiteLLM v1.109.0; no work to do in this repo"},
    {"name":"not-docs-only","grade":"pass","evidence":"bug label"},
    {"name":"ai-contribution-allowed","grade":"pass","evidence":"AGENTS.md only; no ban"}],"verdict":"reject"},
  {"item":"https://github.com/BerriAI/litellm/issues/50394","checks":[
    {"name":"maintainer-alive","grade":"pass","evidence":"commits 2026-11-06"},
    {"name":"responds-to-issues","grade":"unclear","evidence":"no human maintainer replies found in sampled comments"},
    {"name":"unclaimed","grade":"fail","evidence":"open PR #49926 fixes this issue"},
    {"name":"is-a-bug-fix","grade":"pass","evidence":"404 on Azure AI Foundry embeddings"},
    {"name":"scope-feasible","grade":"pass","evidence":"one route fix"},
    {"name":"not-docs-only","grade":"pass","evidence":"code fix"},
    {"name":"ai-contribution-allowed","grade":"pass","evidence":"no ban in CONTRIBUTING.md or AGENTS.md"}],"verdict":"reject"},
  {"item":"https://github.com/langchain-ai/langgraph/issues/9620","checks":[
    {"name":"maintainer-alive","grade":"pass","evidence":"commits 2026-11-05/06"},
    {"name":"responds-to-issues","grade":"unclear","evidence":"no human maintainer replies found in sampled comments"},
    {"name":"unclaimed","grade":"pass","evidence":"no assignee, 0 comments, no linked PR"},
    {"name":"is-a-bug-fix","grade":"pass","evidence":"handles retained after close, with repro"},
    {"name":"scope-feasible","grade":"pass","evidence":"one leak"},
    {"name":"not-docs-only","grade":"pass","evidence":"bug label"},
    {"name":"ai-contribution-allowed","grade":"pass","evidence":"AI-generated PR text may be ignored, not a ban"}],"verdict":"reject"}
]

Friction for scan-notes.md

- Ranking order is fewest comments first, and every top result was filed today with 0 comments. That favours brand-new issues over ones with a track record. A min-age filter would fix it.
- Bugs that need GPU or distributed hardware, like DeepSpeed, rank the same as pure-Python ones. Rubric difficulty is only a note, so consider a small-fix hint in the strategy.
- Three queries returned zero results, and three hit the 100-result cap.

---

# 2026-11-06 scan 2 (23 queries, with batch-4 general Python repos and the new 3-day min-age filter)

## Result: 0 accepted of 5 graded

No candidate passed; none of the top 5 is a good first issue. scikit-learn #35330 from the previous scan is still the best pick. The new 3-day filter drops it from this pool because it was filed on 2026-11-06.

## Search log

| Q | Result count | Notes |
|---|---|---|
| 1 | 100 | owners, python bugs. Hit the cap |
| 2 | 100 | owners, typescript bugs. Hit the cap; 99 were anthropics/claude-code, dropped by the filter |
| 3 | 0 | owners, javascript bugs |
| 4 | 39 | named repos, python bugs (AutoGPT 25, pydantic-ai 10, smolagents 4) |
| 5 | 1 | named repos, typescript bugs (vercel/ai) |
| 6 | 2 | owners, GFI python |
| 7 | 0 | owners, GFI typescript |
| 8 | 46 | batch 1 repos, python bugs (jax 30, DeepSpeed 15) |
| 9 | 5 | batch 1 repos, GFI python |
| 10 | 22 | dify bugs |
| 11 | 2 | microsoft/ai-agents-for-beginners |
| 12 | 7 | TypeSafeAI repos |
| 13 | 73 | pytorch org python bugs (tensordict 64) |
| 14 | 9 | pytorch org GFI |
| 15 | 0 | scipy GFI |
| 16 | 100 | batch 2 repos, python bugs (vllm 88, docling 10). Hit the cap |
| 17 | 0 | batch 2 repos, typescript bugs |
| 18 | 1 | batch 2 repos, GFI |
| 19 | 28 | batch 3 repos, python bugs (scikit-learn 13, transformers 11) |
| 20 | 52 | batch 3 repos, typescript bugs (all OpenHands) |
| 21 | 0 | batch 3 repos, GFI |
| 22 | 80 | NEW batch 4, python bugs (pandas 62, ansible 13, pytorch-lightning 5) |
| 23 | 1 | NEW batch 4, GFI (wagtail 1) |

666 unique results after the leak guard, 499 after the ranking filters (assignee, 90-day activity, repo allowlist, claude-code, **created at least 3 days ago**, at most 15 comments, feature titles, doc labels).

The first batch-4 result ranks 14th (pandas-dev/pandas #58299), so none of the new repos reached the top 5. Every top-5 slot is an issue with a bug label and 0 comments, ordered by most recent update.

## Ranked read-out (all rejected)

1. **pytorch/TensorRT #4954** (Q13; bug label, 0 comments, updated 2026-11-06): rejected on `unclaimed`. Opened by collaborator cehongwang at 09:06 on 2026-10-30, and the same collaborator's PR #4955 (a docs fix for it) was opened at 09:29 and merged the same day. That is the same-day issue-plus-PR pattern. It also bundles three separate problems and needs an NVIDIA GPU.
2. **jax-ml/jax #42263** (Q8; bug, 0 comments): rejected on `is-a-bug-fix`. It asks for in-place updates of a donated buffer when a Pallas kernel has remote DMAs, which is a missing optimization, not wrong output. The body also says the repro "has not yet been run". It needs a TPU and Pallas compiler knowledge.
3. **BerriAI/litellm #49688** (Q1; bug, 0 comments): rejected on `is-a-bug-fix` and `responds-to-issues`. It is a question about the fix status of two CVEs from a scanner report, and I found no human maintainer replies on others' issues in the sample.
4. **openai/evals #1856** (Q1; bug, 0 comments): rejected on `maintainer-alive`, `responds-to-issues` and `is-a-bug-fix`. The last 5 commits are 2026-04-14 and older, with 0 in the last 90 days, there is no release, and it is a proposal to add a legal-hallucination eval case, not a code bug.
5. **vllm-project/vllm #63793** (Q16; bug, 0 comments): rejected on `unclaimed` and `scope-feasible`. Maintainer mgoin opened it with a 4-item plan, and the fix is in the separate `humming` repo, where mgoin already has PR #177. It needs 8x H200.

## Friction

- The 3-day filter works, but the top 5 is still not usable. With "bug label first, then fewest comments, then most recently updated", every slot is a 0-comment issue ranked purely by recency. That favors maintainer-filed trackers, hardware bugs and off-topic reports.
- Hardware-heavy issues (TensorRT, jax Pallas on TPU, vllm on H200) fill 3 of 5 slots. The rubric only records difficulty as a note.
- Maintainer-opened issues (TensorRT, vllm) rank first because they have 0 comments and are well written. A ranking signal on the opener's `authorAssociation` would help, but the search results do not carry it.
- The `responds-to-issues` sample is expensive on very busy repos (litellm, vllm): thousands of comments since the cutoff, and several API calls timed out.
- The 3-day filter also removes scikit-learn #35330, the previous scan's best pick, which is only hours old.

## Graded results

```json
[
  {"item":"https://github.com/pytorch/TensorRT/issues/4954","checks":[
    {"name":"maintainer-alive","grade":"pass","evidence":"last 5 commits all 2026-11-06, not archived"},
    {"name":"responds-to-issues","grade":"pass","evidence":"COLLABORATORs micwill755 and narendasan replied on outsiders' issues #4967 and #4968 on 2026-11-06"},
    {"name":"unclaimed","grade":"fail","evidence":"issue opened 2026-10-30 09:06 by collaborator cehongwang; their PR #4955 opened 09:29 and merged the same day"},
    {"name":"is-a-bug-fix","grade":"pass","evidence":"non-contiguous mutated input silently misses the update"},
    {"name":"scope-feasible","grade":"pass","evidence":"three named problems, each with its own repro section"},
    {"name":"not-docs-only","grade":"pass","evidence":"labels bug, component: runtime, component: plugins"},
    {"name":"ai-contribution-allowed","grade":"pass","evidence":"CONTRIBUTING.md has no AI policy"}],"verdict":"reject"},
  {"item":"https://github.com/jax-ml/jax/issues/42263","checks":[
    {"name":"maintainer-alive","grade":"pass","evidence":"last 5 commits 2026-11-06"},
    {"name":"responds-to-issues","grade":"pass","evidence":"COLLABORATOR jakevdp replied on outsiders' issues #42254, #42377, #42318 on 2026-11-05/06"},
    {"name":"unclaimed","grade":"pass","evidence":"open, no assignee, 0 comments, no linked PR"},
    {"name":"is-a-bug-fix","grade":"fail","evidence":"asks for in-place updates when a kernel has remote DMAs ('blanket rule'), a missing optimization, not wrong output"},
    {"name":"scope-feasible","grade":"pass","evidence":"one behavior: avoid the two buffer copies"},
    {"name":"not-docs-only","grade":"pass","evidence":"code fix in Pallas lowering"},
    {"name":"ai-contribution-allowed","grade":"pass","evidence":"docs/contributing.md: coding assistance tools allowed if the CLA is met; you are responsible for the code"}],"verdict":"reject"},
  {"item":"https://github.com/BerriAI/litellm/issues/49688","checks":[
    {"name":"maintainer-alive","grade":"pass","evidence":"last 5 commits 2026-11-06"},
    {"name":"responds-to-issues","grade":"unclear","evidence":"no human maintainer replies on others' issues found in the sample"},
    {"name":"unclaimed","grade":"pass","evidence":"open, no assignee, 0 comments, no linked PR"},
    {"name":"is-a-bug-fix","grade":"fail","evidence":"title starts 'Question:' and asks the fix status of two CVEs"},
    {"name":"scope-feasible","grade":"fail","evidence":"asks questions and advisory updates; no single code task"},
    {"name":"not-docs-only","grade":"pass","evidence":"label bug"},
    {"name":"ai-contribution-allowed","grade":"pass","evidence":"AGENTS.md and CONTRIBUTING.md direct AI agents; no ban"}],"verdict":"reject"},
  {"item":"https://github.com/openai/evals/issues/1856","checks":[
    {"name":"maintainer-alive","grade":"fail","evidence":"last 5 commits 2026-04-14, 2026-04-06, 2025-11-03, 2024-12-18, 2024-09-30; 0 within 90 days"},
    {"name":"responds-to-issues","grade":"fail","evidence":"no human maintainer replies on others' issues since 2026-08-08"},
    {"name":"unclaimed","grade":"pass","evidence":"open, no assignee, 0 comments, no linked PR"},
    {"name":"is-a-bug-fix","grade":"fail","evidence":"title 'Accuracy Bug/Eval Proposal'; asks to add a legal-hallucination eval case"},
    {"name":"scope-feasible","grade":"pass","evidence":"one proposed JSON eval case"},
    {"name":"not-docs-only","grade":"pass","evidence":"label bug"},
    {"name":"ai-contribution-allowed","grade":"pass","evidence":"no policy file found"}],"verdict":"reject"},
  {"item":"https://github.com/vllm-project/vllm/issues/63793","checks":[
    {"name":"maintainer-alive","grade":"pass","evidence":"last 5 commits 2026-11-06"},
    {"name":"responds-to-issues","grade":"pass","evidence":"COLLABORATOR robertgshaw2-redhat replied on outsider hugoabonizio's issue #66009 on 2026-11-06"},
    {"name":"unclaimed","grade":"fail","evidence":"opened by MEMBER mgoin, who already has the fix PR vllm-project/humming#177 (referenced in the timeline)"},
    {"name":"is-a-bug-fix","grade":"pass","evidence":"AssertionError at startup at TP=4/8 on Hopper"},
    {"name":"scope-feasible","grade":"fail","evidence":"maintainer plan checklist with a Humming release and a requirements bump; the fix is in another repo"},
    {"name":"not-docs-only","grade":"pass","evidence":"labels bug, deepseek, quantization"},
    {"name":"ai-contribution-allowed","grade":"pass","evidence":"AGENTS.md: pure code-agent PRs not allowed; human review and AI disclosure required, so conditions, not a ban"}],"verdict":"reject"}
]
```


---

# 2026-11-06 scan 3 (42 queries: batch 5 large OSS orgs, wider languages, no-special-hardware rule)

Result: 2 accepted of 5 graded, both in openedx/openedx-platform.

Search: 1286 unique results, 1014 after the ranking filters, 26 in the top label tier (bug plus good first issue / easy / help wanted). New query counts: Q24 71, Q25 100, Q26 100, Q27 0, Q28 9, Q29 0, Q30 9, Q31 2, Q32 73, Q33 10, Q34 1, Q35 100, Q36 13, Q37 26, Q38 42, Q39 51, Q40 100, Q41 38, Q42 0 (`easy`).

Accepted (fit order)
1. openedx/openedx-platform #39114 (Q41; tier 1, 0 comments): subsection prerequisite at min_score 0 is shown as gated by Studio but the LMS does not enforce it. Repro table in the body, no assignee, no PR. Open edX's AI policy explicitly allows Claude with disclosure. Difficulty: moderate with an open design edge (create the milestone for completion-only requirements, or make Studio warn); raise it with a maintainer early. Needs a Tutor dev environment, no special hardware.
2. openedx/openedx-platform #39119 (Q41; tier 1, 1 comment): calculate_problem_responses_csv raises UsageKeyNotInBlockStructure with no way for the instructor to see it. Difficulty: ambitious, because three fix options are listed (validate and return 400, fall back to the modulestore, return a task id) and sibling issue #39209 covers the task id. The one comment is a proposed approach, not a claim.

Rejected
- opensearch-project/query-insights-dashboards #616 (Q33/Q35): unclaimed fails (claim comment 2026-11-05 asking to be assigned, no maintainer reply). Also responds-to-issues unclear: the only maintainer reply found was to a CI bot's issue.
- apache/fesod #272 (Q25): unclaimed fails (open PR #693 linked; member delei says they submitted a fix PR).
- opensearch-project/dashboards-search-relevance #757 (Q33): is-a-bug-fix and scope-feasible fail ("[ENHANCEMENT] Improve UI test coverage across Search Relevance modules" carries a bug label).

Note: shipped-recently and has-contributing-guide fail for openedx-platform (no GitHub releases since 2015, no CONTRIBUTING.md in the repo; contribution rules live in the org-level docs and AI_POLICY.md). Preferred checks never change the verdict.

```json
[
  {"item":"https://github.com/openedx/openedx-platform/issues/39114","checks":[
    {"name":"maintainer-alive","grade":"pass","evidence":"last 5 commits 2026-11-04 to 2026-11-06, not archived"},
    {"name":"responds-to-issues","grade":"pass","evidence":"MEMBERs kdmccormick and mariajgrimaldi replied on outsiders' issues #39029 and #37409 (2026-10-29 to 2026-11-03)"},
    {"name":"unclaimed","grade":"pass","evidence":"open, no assignee, 0 comments; linked items are issues in end-to-end-tests, not PRs; author is a CONTRIBUTOR"},
    {"name":"is-a-bug-fix","grade":"pass","evidence":"LMS does not gate a subsection Studio reports as gated"},
    {"name":"scope-feasible","grade":"pass","evidence":"one behavior: create the gating milestone for completion-only requirements"},
    {"name":"no-special-hardware","grade":"pass","evidence":"Django LMS/Studio logic, testable on a laptop"},
    {"name":"not-docs-only","grade":"pass","evidence":"labels bug, help wanted; code fix"},
    {"name":"ai-contribution-allowed","grade":"pass","evidence":"openedx AI_POLICY.md lists Anthropic Claude as allowed; disclosure required"},
    {"name":"good-to-start","grade":"pass","evidence":"help wanted label"},
    {"name":"has-repro","grade":"pass","evidence":"API calls and a min_score/min_completion table"},
    {"name":"shipped-recently","grade":"fail","evidence":"latest GitHub release 2015-07-17"},
    {"name":"has-contributing-guide","grade":"fail","evidence":"no CONTRIBUTING.md in the repo or .github"}],"verdict":"accept"},
  {"item":"https://github.com/openedx/openedx-platform/issues/39119","checks":[
    {"name":"maintainer-alive","grade":"pass","evidence":"last 5 commits 2026-11-04 to 2026-11-06, not archived"},
    {"name":"responds-to-issues","grade":"pass","evidence":"MEMBER replies on outsiders' issues #39029 and #37409"},
    {"name":"unclaimed","grade":"pass","evidence":"open, no assignee, no linked PR; the one comment (Swir, 2026-09-18) proposes an approach, not a claim"},
    {"name":"is-a-bug-fix","grade":"pass","evidence":"task raises UsageKeyNotInBlockStructure and the report never appears, with no error shown"},
    {"name":"scope-feasible","grade":"pass","evidence":"one failing report endpoint; three fix options are an open design edge, not a container issue"},
    {"name":"no-special-hardware","grade":"pass","evidence":"Django task and API logic"},
    {"name":"not-docs-only","grade":"pass","evidence":"labels bug, help wanted; code fix"},
    {"name":"ai-contribution-allowed","grade":"pass","evidence":"openedx AI_POLICY.md allows Claude with disclosure"},
    {"name":"good-to-start","grade":"pass","evidence":"help wanted label"},
    {"name":"has-repro","grade":"pass","evidence":"endpoint, timing condition and error named"},
    {"name":"shipped-recently","grade":"fail","evidence":"latest GitHub release 2015-07-17"},
    {"name":"has-contributing-guide","grade":"fail","evidence":"no CONTRIBUTING.md in the repo or .github"}],"verdict":"accept"},
  {"item":"https://github.com/opensearch-project/query-insights-dashboards/issues/616","checks":[
    {"name":"maintainer-alive","grade":"pass","evidence":"commits 2026-11-06, 11-03, 10-26"},
    {"name":"responds-to-issues","grade":"unclear","evidence":"only maintainer reply found was dzane17 on #605, opened by opensearch-ci-bot"},
    {"name":"unclaimed","grade":"fail","evidence":"ariktadas144 commented 2026-11-05 asking to be assigned with a plan; no maintainer reply"},
    {"name":"is-a-bug-fix","grade":"pass","evidence":"save failures are silent"},
    {"name":"scope-feasible","grade":"pass","evidence":"one UI flow"},
    {"name":"no-special-hardware","grade":"pass","evidence":"TypeScript UI"},
    {"name":"not-docs-only","grade":"pass","evidence":"code fix"},
    {"name":"ai-contribution-allowed","grade":"pass","evidence":"no policy found"}],"verdict":"reject"},
  {"item":"https://github.com/apache/fesod/issues/272","checks":[
    {"name":"maintainer-alive","grade":"pass","evidence":"commits 2026-11-05 and 11-06"},
    {"name":"responds-to-issues","grade":"pass","evidence":"MEMBER delei replied on #1290 and #1258 (2026-10-23 to 11-01)"},
    {"name":"unclaimed","grade":"fail","evidence":"open linked PR #693; MEMBER delei: 'I have already submitted a PR to try to fix it'"},
    {"name":"is-a-bug-fix","grade":"pass","evidence":"formulaValue returns incorrect results"},
    {"name":"scope-feasible","grade":"pass","evidence":"one formula behavior"},
    {"name":"no-special-hardware","grade":"pass","evidence":"Java library"},
    {"name":"not-docs-only","grade":"pass","evidence":"code fix"},
    {"name":"ai-contribution-allowed","grade":"pass","evidence":"AGENTS.md present, no ban"}],"verdict":"reject"},
  {"item":"https://github.com/opensearch-project/dashboards-search-relevance/issues/757","checks":[
    {"name":"maintainer-alive","grade":"pass","evidence":"commits 2026-11-06, 11-03, 11-02"},
    {"name":"responds-to-issues","grade":"pass","evidence":"MEMBER epugh replied on iprithv's issues #942 and #974"},
    {"name":"unclaimed","grade":"pass","evidence":"no assignee; the 2026-09-25 request to be assigned is stale (over 30 days, no maintainer answer); 1 closed linked PR"},
    {"name":"is-a-bug-fix","grade":"fail","evidence":"title '[ENHANCEMENT] Improve UI test coverage'; asks for tests, not a fix for wrong behavior"},
    {"name":"scope-feasible","grade":"fail","evidence":"'across Search Relevance modules': a codebase-wide improvement"},
    {"name":"no-special-hardware","grade":"pass","evidence":"UI tests"},
    {"name":"not-docs-only","grade":"pass","evidence":"tests"},
    {"name":"ai-contribution-allowed","grade":"pass","evidence":"no policy found"}],"verdict":"reject"}
]
```


---

# 2026-11-06 direct grading of 3 picks (rubric now includes no-special-hardware)

Command: issue-scout, direct mode, for langgraph #9620, openedx-platform #39114 and openedx-platform #39119. Each is graded independently; the query for all three is "direct".

## Ranked read-out

**Accepted** (fit order)
1. **openedx/openedx-platform #39114** (direct): a subsection prerequisite with min_score 0 is shown as gated by Studio but the LMS does not enforce it. A python backend bug with a repro table, 0 comments, no assignee, no PR, and a `help wanted` label. Maintainers (kdmccormick, mariajgrimaldi) replied on outsiders' issues in the last 2 weeks, and Open edX's AI policy allows Claude with disclosure.
   - Difficulty note: **moderate**. The issue does not name the file; the gating code lives under `lms/djangoapps/gating/` and `openedx/core/lib/gating/api.py`. The issue gives two acceptable outcomes (create the milestone for completion-only requirements, or have Studio warn), so confirm the choice with a maintainer before building.
2. **openedx/openedx-platform #39119**: `calculate_problem_responses_csv` raises `UsageKeyNotInBlockStructure` with no way for the instructor to see it. A python backend bug, no assignee or PR, `help wanted`. The one comment (2026-09-18) proposes an approach, not a claim.
   - Difficulty note: **ambitious**. The issue lists three fixes (validate and return 400, fall back to the modulestore, return a task id), and the task id part overlaps open sibling issue #39209. Claiming it means raising which fix to build with a maintainer first. The code touches `lms/djangoapps/grades/tasks.py` and the block-structure manager.

**Rejected**
- **langchain-ai/langgraph #9620**: `unclaimed` fails. The reporter wrote "I would like to work on a focused fix; could a maintainer assign this issue to me", with before and after output attached. The repo's automation also closes PRs from non-assigned authors, and no one gets assigned without a maintainer acting. Everything else passes (a human member, hntrl, replied to outsiders' issues #8614 and #7432 on 2026-10-06; the freshest 400-comment sample shows none, so this is a thin pass).

Preferred checks never change the verdict. For both openedx issues, `good-to-start` and `has-repro` pass, and `shipped-recently` and `has-contributing-guide` fail (no GitHub release since 2015, no CONTRIBUTING.md in the repo). For langgraph, `has-contributing-guide` now passes through an AGENTS.md file but there is no CONTRIBUTING.md, and `good-to-start` fails.

```json
[
  {"item":"https://github.com/openedx/openedx-platform/issues/39114","checks":[
    {"name":"maintainer-alive","grade":"pass","evidence":"last 5 commits 2026-11-04 to 2026-11-06, not archived"},
    {"name":"responds-to-issues","grade":"pass","evidence":"MEMBERs kdmccormick and mariajgrimaldi replied on outsiders' issues #39029 and #37409, 2026-10-29 to 2026-11-03"},
    {"name":"unclaimed","grade":"pass","evidence":"open, no assignee, 0 comments, no linked PR (linked items are end-to-end-tests issues)"},
    {"name":"is-a-bug-fix","grade":"pass","evidence":"LMS gated:false while Studio reports gated for min_score 0"},
    {"name":"scope-feasible","grade":"pass","evidence":"one behavior: milestone is only created when min_score >= 1"},
    {"name":"no-special-hardware","grade":"pass","evidence":"Django LMS/Studio logic, no accelerator or device needed"},
    {"name":"not-docs-only","grade":"pass","evidence":"labels bug, help wanted; code fix"},
    {"name":"ai-contribution-allowed","grade":"pass","evidence":"openedx AI_POLICY.md lists Anthropic Claude as allowed with disclosure"},
    {"name":"good-to-start","grade":"pass","evidence":"help wanted label"},
    {"name":"has-repro","grade":"pass","evidence":"API path and min_score/min_completion table"},
    {"name":"shipped-recently","grade":"fail","evidence":"latest GitHub release 2015-07-17"},
    {"name":"has-contributing-guide","grade":"fail","evidence":"no CONTRIBUTING.md in repo or .github"}],"verdict":"accept"},
  {"item":"https://github.com/openedx/openedx-platform/issues/39119","checks":[
    {"name":"maintainer-alive","grade":"pass","evidence":"last 5 commits 2026-11-04 to 2026-11-06, not archived"},
    {"name":"responds-to-issues","grade":"pass","evidence":"MEMBER replies on outsiders' issues #39029 and #37409"},
    {"name":"unclaimed","grade":"pass","evidence":"open, no assignee, no linked PR; Swir's 2026-09-18 comment proposes an approach, it does not claim the issue"},
    {"name":"is-a-bug-fix","grade":"pass","evidence":"task raises UsageKeyNotInBlockStructure and the report never appears"},
    {"name":"scope-feasible","grade":"pass","evidence":"one failing report endpoint; the fix options are a design edge, not a container"},
    {"name":"no-special-hardware","grade":"pass","evidence":"Django task and API logic"},
    {"name":"not-docs-only","grade":"pass","evidence":"labels bug, help wanted; code fix"},
    {"name":"ai-contribution-allowed","grade":"pass","evidence":"openedx AI_POLICY.md allows Claude with disclosure"},
    {"name":"good-to-start","grade":"pass","evidence":"help wanted label"},
    {"name":"has-repro","grade":"pass","evidence":"endpoint, timing condition and error named"},
    {"name":"shipped-recently","grade":"fail","evidence":"latest GitHub release 2015-07-17"},
    {"name":"has-contributing-guide","grade":"fail","evidence":"no CONTRIBUTING.md in repo or .github"}],"verdict":"accept"},
  {"item":"https://github.com/langchain-ai/langgraph/issues/9620","checks":[
    {"name":"maintainer-alive","grade":"pass","evidence":"last 5 commits 2026-11-05/06, not archived"},
    {"name":"responds-to-issues","grade":"pass","evidence":"MEMBER hntrl replied on outsiders' issues #8614 and #7432 on 2026-10-06 (thin: two triage-closing replies)"},
    {"name":"unclaimed","grade":"fail","evidence":"reporter's body: 'I would like to work on a focused fix; could a maintainer assign this issue to me'"},
    {"name":"is-a-bug-fix","grade":"pass","evidence":"event handles retained after close, with runnable repro"},
    {"name":"scope-feasible","grade":"pass","evidence":"one leak in AsyncThreadStream.events"},
    {"name":"no-special-hardware","grade":"pass","evidence":"pure Python asyncio repro, no hardware"},
    {"name":"not-docs-only","grade":"pass","evidence":"labels bug, external"},
    {"name":"ai-contribution-allowed","grade":"pass","evidence":"PR template: AI-generated description may be ignored or closed, a condition not a ban"},
    {"name":"good-to-start","grade":"fail","evidence":"labels are bug, external"},
    {"name":"has-repro","grade":"pass","evidence":"self-contained asyncio/weakref snippet"},
    {"name":"shipped-recently","grade":"pass","evidence":"release 2026-11-06"},
    {"name":"has-contributing-guide","grade":"fail","evidence":"no CONTRIBUTING.md in repo or .github (AGENTS.md only)"}],"verdict":"reject"}
]
```


---

# 2026-11-06 scan 4 (47 queries: batch 6 beginner-friendly Python repos, new label vocabulary, new rubric preferred check)

Result: 2 accepted of 5 graded. The top 5 are the same five issues as scan 3, so the grades from scan 3 carry over unchanged (I re-checked each issue's live state first: same assignees, comment counts and PR links as when graded).

## Search log

Queries 1 to 23 are the original queries, 24 to 38 are the batch 5 owner queries (kubernetes and the all-org `easy` query are no longer in the file), and 39 to 47 are the new batch 6 queries. The fixed and new ones:

| Q | Count | Notes |
|---|---|---|
| 22 | 83 | batch 4 python bugs, now with `ossf/cve-bin-tool` (3 results; the old `intel/` name returned none) |
| 23 | 1 | batch 4 good first issue (wagtail) |
| 24-38 | see below | batch 5 owner queries |
| 39 | 100 | NEW batch 6 bug, 9 new repos (erpnext 65, frappe 20, salt 8, mypy 5); hit the cap |
| 40 | 1 | NEW batch 6 good first issue (frappe 1) |
| 41 | 11 | NEW `help wanted` across the 20 listed repos (scikit-learn 7, lightning 3, zulip 1) |
| 42 | 0 | NEW `help-wanted` |
| 43 | 0 | NEW `good-first-issue` |
| 44 | 0 | NEW `Easy` |
| 45 | 0 | NEW `easyfix` |
| 46 | 0 | NEW `🌱 Good first issue` |
| 47 | 7 | NEW `first-contribution` (matplotlib 7) |

Full per-query counts: Q1 100, Q2 100, Q3 0, Q4 39, Q5 1, Q6 2, Q7 0, Q8 46, Q9 5, Q10 22, Q11 2, Q12 7, Q13 73, Q14 9, Q15 0, Q16 100, Q17 0, Q18 1, Q19 28, Q20 52, Q21 0, Q22 83, Q23 1, Q24 71, Q25 100, Q26 100, Q27 9, Q28 2, Q29 73, Q30 10, Q31 1, Q32 100, Q33 13, Q34 26, Q35 42, Q36 51, Q37 100, Q38 38, Q39 100, Q40 1, Q41 11, Q42 0, Q43 0, Q44 0, Q45 0, Q46 0, Q47 7. Queries at the 100 cap: Q1, Q2, Q16, Q25, Q26, Q32, Q37, Q39.

1396 unique results, 1096 after the ranking filters (dropped: 202 created under 3 days ago, 39 docs labels, 37 over 15 comments, 22 feature titles). Tiers: 27 bug plus a friendly label, 788 bug only, 125 beginner label only, 147 help label only, 9 other.

## Where the new repos landed

The best issue from each listed repo and its rank in the ordered list: pytorch-lightning 9, zulip 27, mypy 30, frappe/erpnext 39, frappe/frappe 46, pandas 122, ansible 201, scikit-learn 266, saltstack/salt 370, cve-bin-tool 492, matplotlib 870. httpx, hatch, Open Library, wger, requests, fastapi, scrapy and poetry produced no surviving results.

## Ranked read-out (same five as scan 3)

Accepted (fit order)
1. **openedx/openedx-platform #39114** (Q38; bug plus help wanted, 0 comments, 1st by tier then fewest comments): subsection prerequisite at min_score 0 is gated in Studio but not in the LMS. Difficulty: **moderate**. The code lives under `lms/djangoapps/gating/` and `openedx/core/lib/gating/api.py`; confirm with a maintainer whether to create the milestone or make Studio warn.
2. **openedx/openedx-platform #39119** (Q38; bug plus help wanted, 1 comment, 5th): `calculate_problem_responses_csv` fails with no visible error. Difficulty: **ambitious**. Three fixes are listed (validate and return 400, fall back to the modulestore, return a task id) and sibling issue #39209 overlaps; ask a maintainer which to build.

Rejected
- opensearch-project/query-insights-dashboards #616 (Q30/Q32, 2nd): `unclaimed` fails; a claim comment on 2026-11-05 has no maintainer reply, and `responds-to-issues` is unclear.
- apache/fesod #272 (Q25, 3rd): `unclaimed` fails; open PR #693 already addresses it.
- opensearch-project/dashboards-search-relevance #757 (Q30, 4th): `is-a-bug-fix` and `scope-feasible` fail; it is an enhancement for test coverage across modules.

The full per-check JSON for these five is in the "2026-11-06 scan 3" section above and in the "direct grading of 3 picks" section (grades identical).

## Friction

- The new beginner-friendly repos barely register: only pytorch-lightning reaches the top 10. Tier 1 ("bug label only") has 788 results and is sorted by fewest comments and then recency, so the openedx, opensearch and apache bug issues crowd everything out. A per-repo cap in the top-N (at most 1 or 2 issues per repo) would let more of the listed repos into the graded 5.
- The beginner label variants (`Easy`, `easyfix`, `help-wanted`, `🌱 Good first issue`, `good-first-issue`) returned 0 results because open, unassigned, recently updated issues with them are rare. The listed repos' beginner issues mostly already have an assignee, an open PR, or are labelled docs.
- `help wanted` is the productive label: 57 raw results across zulip, scikit-learn and lightning, but zulip's best surviving issue is an i18n issue with 15 comments and the sklearn and lightning ones are research-heavy.
- Eight queries hit the 100 cap even with newest-first sorting.


---

# 2026-11-06 scan 5 (per-repo quota of 20, top 10 graded)

Result: 2 accepted of 10 graded, the same two openedx issues as before. The five issues added by the wider top 10 were all rejected.

## What changed in this scan

- `search-strategy.md` now caps each repo at its best 20 issues, tops up any listed repo below 20 with a no-label query, and grades the top 10 (about $2 per scan).
- 63 of 70 listed repos needed a top-up, so I ran 63 per-repo top-up queries on top of the 47 base queries. Two repos were renamed and returned errors on the old names: `explodinggradients/ragas` is now `vibrantlabsai/ragas`, and `mendableai/firecrawl` is now `firecrawl/firecrawl` (fixed in the strategy; before this fix those two repos were silently dropped, like `intel/cve-bin-tool` earlier).
- 3476 unique results, 2591 after the drop filters, 1407 after the per-repo quota. Top-up raw counts included docling 100, frappe 100, home-assistant 100, transformers 100, openlibrary 100, milvus 100, ollama 100, pytorch 100, qdrant 100, salt 100, zulip 100, scikit-learn 92, pydantic-ai 95.

## Coverage of the 70 listed repos (django excluded: its issues are on Trac)

- 31 repos reach 20 issues after the filters.
- 8 repos have 0: tensorflow/tensorflow, pallets/flask, fastapi/fastapi, google/adk-python, locustio/locust, TypeSafeAI/clarity-judge, TypeSafeAI/jev-harness, TypeSafeAI/typesafe-ui (everything recent is assigned, has an open PR, or was filtered out).
- 31 repos fall between 1 and 19, the lowest being TheAlgorithms/Python 1, TypeSafeAI/typesafe-router 1, microsoft/ai-agents-for-beginners 2, keras 3, pgvector 4, httpx 5, hatch 5, poetry 5, OSWorld 5, black 6, requests 12, scrapy 11, ansible 8, cve-bin-tool 9.
- The floor of 20 cannot be met for those repos: the cause is the filters (unassigned, no open PR, updated in the last 90 days, at most 15 comments, not created in the last 3 days, no docs or feature labels), not the search.

## Ranked read-out (top 10)

Accepted (fit order)
1. **openedx/openedx-platform #39114** (Q38; tier 1, 0 comments): subsection prerequisite at min_score 0 is gated in Studio but not in the LMS. Difficulty: **moderate**. Code lives under `lms/djangoapps/gating/` and `openedx/core/lib/gating/api.py`; confirm with a maintainer whether to create the milestone or make Studio warn.
2. **openedx/openedx-platform #39119** (Q38; tier 1, 1 comment): `calculate_problem_responses_csv` fails with no visible error. Difficulty: **ambitious**. Three fixes are listed (400 response, modulestore fallback, task id) and sibling #39209 overlaps; ask a maintainer which to build.

Rejected, in rank order
- #3 opensearch query-insights-dashboards #616 (Q30/Q32): `unclaimed` fails (claim comment 2026-11-05, no maintainer reply); `responds-to-issues` unclear.
- #4 apache/fesod #272 (Q25): `unclaimed` fails (open PR #693).
- #5 opensearch dashboards-search-relevance #757 (Q30): `is-a-bug-fix` and `scope-feasible` fail (an enhancement for test coverage).
- #6 apache/maka #3328 (Q25): `scope-feasible` fails and `is-a-bug-fix` and `no-special-hardware` are unclear; a contributor says the report lacks what is needed to reproduce it.
- #7 apache/maka #3312 (Q25): near miss. It is a React crash with a forensic analysis, no claim and no open PR, but the body says it happens "when using grok-4.6 model", so `no-special-hardware` is unclear (it may need a paid model account). Maka is an AI-agent desktop app (TypeScript/React), which fits your interests: if you can confirm the crash reproduces without a model account, this issue would pass.
- #8 apache/seatunnel #10172 (Q25): `unclaimed` fails (open PR #11823 covers it); `scope-feasible` and `no-special-hardware` fail (needs a Kerberos Hadoop cluster and a classloading design).
- #9 Lightning-AI/pytorch-lightning #18645 (Q22/Q41): `unclaimed` fails (claim comment 2026-10-22, no maintainer reply) and a commenter could not reproduce it on current master.
- #10 opensearch-project/opensearch-java #611 (Q30/Q32): `unclaimed` fails (a first-time contributor asked to investigate it on 2026-10-15, no maintainer reply).

For the five issues that were also in the previous top 5, I re-checked live state (assignees, comment counts, PR links) and reused the grades.

## Friction

- Same shape as before: ranks 1 to 10 are all "bug plus a friendly label" issues from openedx, opensearch, apache and lightning. The per-repo quota of 20 did not change the top 10, because the ordering is applied after the quota and those issues are the best of their own repos. Only a per-repo cap on the graded list (for example 2 per repo) would bring in more listed repos.
- The 20-issue floor is unreachable for 39 of 70 repos because of the filters, not the search. Top-up queries gave those repos candidates, but unlabelled top-up issues rank in tier 4 (498 of the 1407), so they never reach the top 10.
- Claim comments are the main reason good-looking issues fail: 5 of the 8 rejected issues have a recent "I'd like to take this" comment with no maintainer reply (and several are from the last 3 weeks).
- Renamed repos silently return nothing; the scan should flag a `--repo` that gh cannot resolve (the error is printed on stderr only).

## Graded results

```json
[
 {
  "item": "https://github.com/openedx/openedx-platform/issues/39114",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits 2026-11-04 to 11-06, not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBERs kdmccormick and mariajgrimaldi replied on outsiders' issues #39029 and #37409"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee, 0 comments, no linked PR (linked items are end-to-end-tests issues)"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "LMS gated:false while Studio reports gated for min_score 0"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one behavior: milestone only created when min_score >= 1"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "Django LMS/Studio logic, no accelerator or device"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "labels bug, help wanted; code fix"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "openedx AI_POLICY.md allows Anthropic Claude with disclosure"
   }
  ],
  "verdict": "accept"
 },
 {
  "item": "https://github.com/openedx/openedx-platform/issues/39119",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits 2026-11-04 to 11-06, not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBER replies on outsiders' issues #39029 and #37409"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee, no linked PR; Swir's 2026-09-18 comment proposes an approach, not a claim"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "task raises UsageKeyNotInBlockStructure and the report never appears"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one failing report endpoint; fix options are a design edge, not a container"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "Django task and API logic"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "labels bug, help wanted; code fix"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "openedx AI_POLICY.md allows Claude with disclosure"
   }
  ],
  "verdict": "accept"
 },
 {
  "item": "https://github.com/opensearch-project/query-insights-dashboards/issues/616",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "commits 2026-11-06, 11-03, 10-26"
   },
   {
    "name": "responds-to-issues",
    "grade": "unclear",
    "evidence": "only maintainer reply found was dzane17 on #605, opened by opensearch-ci-bot"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "ariktadas144 commented 2026-11-05 asking to be assigned with a plan; no maintainer reply"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "save failures are silent"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one UI flow"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "TypeScript UI"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "code fix"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no policy found"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/apache/fesod/issues/272",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "commits 2026-11-05 and 11-06"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBER delei replied on #1290 and #1258"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "open linked PR #693; MEMBER delei: 'I have already submitted a PR to try to fix it'"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "formulaValue returns incorrect results"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one formula behavior"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "Java library"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "code fix"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "AGENTS.md present, no ban"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/opensearch-project/dashboards-search-relevance/issues/757",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "commits 2026-11-06, 11-03, 11-02"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBER epugh replied on iprithv's issues #942 and #974"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no assignee; the 2026-09-25 request to be assigned is stale (over 30 days); 1 closed linked PR"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "title '[ENHANCEMENT] Improve UI test coverage'; asks for tests, not a fix for wrong behavior"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "'across Search Relevance modules': codebase-wide"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "UI tests"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no policy found"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/apache/maka/issues/3328",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits 2026-10-28 to 11-06, not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBERs YongGoose and liugddx replied on outsiders' issues 2026-11-06/07"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee; 1 closed linked PR (#4626, merged) and closed issues #3445, #3866"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "unclear",
    "evidence": "Astro-Han (2026-09-17): the repeated-interruption symptom has 'not enough in the report to reproduce it'; the crash part is tracked in #3312"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "no single piece of work: a failed-tool trace with no session export, and the maintainer-side reply asks for more information or to fold it into #3312"
   },
   {
    "name": "no-special-hardware",
    "grade": "unclear",
    "evidence": "timeline shows gpt-5.6-luna model calls; reproducing needs an LLM provider account"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "labels bug, help wanted, stale; code"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md: AI use allowed with disclosure, Generated-by trailer and human review"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/apache/maka/issues/3312",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits 2026-10-28 to 11-06, not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBERs YongGoose and liugddx replied on outsiders' issues 2026-11-06/07"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee, no open linked PR, no claim comment; linked #3367 closed (not planned), #3866 closed (completed)"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "renderer crash, React error #185, with stack and forensic comment from BigDataDZ"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one crash in the nested-update cascade around use-roving-row-focus"
   },
   {
    "name": "no-special-hardware",
    "grade": "unclear",
    "evidence": "body says the crash occurs 'when using grok-4.6 model'; unclear whether it reproduces without a paid model account"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "labels bug, help wanted, stale; code"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md allows AI with disclosure"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/apache/seatunnel/issues/10172",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits all 2026-11-06"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "COLLABORATORs replied on outsiders' issues #13137, #9583, #9870 in the last 2 days"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "open PR #11823 already covers this issue (DanielLeens: 'The existing open PR #11823 already covers this issue'); Adarsh-Me asked to be assigned 2026-09-02"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "NoSuchMethodError for FsTracer.get with Hadoop 3.1.1 and Kerberos"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "a maintainer says a real fix needs an isolated dependency or classloading design"
   },
   {
    "name": "no-special-hardware",
    "grade": "fail",
    "evidence": "reproducing needs a Kerberos-enabled Hadoop 3.1.1 HDFS cluster"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "labels bug, help wanted; code"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "AGENTS.md present, no ban"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/Lightning-AI/pytorch-lightning/issues/18645",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits 2026-09-14 to 09-21 (all within 90 days), not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "COLLABORATOR deependujha replied on outsiders' issues #19230, #22077, #20970 in late October"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "sharmis-tp commented 2026-10-22 'I'd like to pick this up' with a plan; no maintainer reply, 15 days old"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "unclear",
    "evidence": "abhishekacharya200 (2026-10-11) could not reproduce on current master (2.7.0.dev); the issue may already be fixed"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one behavior: a spurious validation step when resuming with max_steps"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "CPU MNIST example, no accelerator"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "labels bug, help wanted, loops; code"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in .github/CONTRIBUTING.md"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/opensearch-project/opensearch-java/issues/611",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "commits 2026-11-06, 11-06, 11-04, 11-02, 11-01"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "COLLABORATOR reta replied on outsiders' issues #2252 and #2236 in late October"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "shalom-ath commented 2026-10-15 'I'd like to investigate this issue as my first contribution' and asked if anyone is working on it; no maintainer reply, 22 days old"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "unclear",
    "evidence": "2023 report; the claimant asks whether it still reproduces on main"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one failing client call"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "Java client"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "labels bug, good first issue; code"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present, no AI ban"
   }
  ],
  "verdict": "reject"
 }
]
```


---

# 2026-11-06 scan 6 (round-robin ranking, top 10 graded)

Result: 1 accepted of 10 graded (openedx/openedx-platform #39114). The graded list now covers 10 different repos instead of 8.

## What changed

- `search-strategy.md`: the final ranking is now round-robin. Inside each repo its capped issues are numbered 1, 2, 3, ...; the pool is sorted by that in-repo number first, then by the existing order (label priority, fewest comments, most recent update, busiest repo). So every repo's best issue is taken before any repo's second best. This replaces the 3-per-repo cap, which never kicked in.
- I reused the search results from scan 5 (47 base queries plus 63 per-repo top-ups, run earlier today, about an hour old) instead of re-running them; only the ranking and grading steps changed.

188 repos have an issue at in-repo number 1. The first 17 of them are tier 1 (bug plus a friendly label), so the top 10 is still all tier 1.

## Ranked read-out (top 10)

Accepted
1. **openedx/openedx-platform #39114** (Q38; in-repo #1): subsection prerequisite at min_score 0 is gated in Studio but not in the LMS. Difficulty: **moderate**. The code lives under `lms/djangoapps/gating/` and `openedx/core/lib/gating/api.py`; confirm with a maintainer whether to create the milestone or make Studio warn.

Rejected (grades for ranks 1 to 8 are reused from scan 5 after re-checking live state; ranks 9 and 10 are new)
- #2 opensearch query-insights-dashboards #616: `unclaimed` fails (claim comment 2026-11-05, no maintainer reply).
- #3 apache/fesod #272: `unclaimed` fails (open PR #693).
- #4 opensearch dashboards-search-relevance #757: an enhancement for test coverage, not a bug fix.
- #5 apache/maka #3328: no single piece of work, and it needs a model account to reproduce.
- #6 apache/seatunnel #10172: `unclaimed` (open PR #11823), and it needs a Kerberos Hadoop cluster.
- #7 Lightning-AI/pytorch-lightning #18645: `unclaimed` (claim comment 2026-10-22); a commenter could not reproduce it on current master.
- #8 opensearch-java #611: `unclaimed` (first-time contributor asked to take it on 2026-10-15).
- #9 open-telemetry/opentelemetry-python-contrib #1704 (Q34/Q35; NEW): `unclaimed` fails. Open PRs #5056 and #5062 are cross-referenced, and a contributor says #5056 "removes the retention instead of bounding it".
- #10 apache/beam #20983 (Q24/Q26; NEW): `no-special-hardware` fails. The failure is `start-build-env.sh` on an Apple Silicon Mac, and verifying a fix needs one. Everything else passes (a good first issue with a clear scope: remove the hardcoded amd64 from the script).

Trade-off: openedx/openedx-platform #39119, which was accepted in the earlier scans, is openedx-platform's second-best issue, so under round-robin it falls out of the graded list (it ranks well past 10). It was graded and accepted earlier today (difficulty ambitious), and nothing about it changed.

## Friction

- Round-robin puts one issue per repo in front, so a good second issue from a strong repo drops out; 39119 is an example.
- Both new entries fail on things the search cannot see (open PRs mentioned only in comments, a requirement for a specific machine). The pre-grading filters still have no signal for specific hardware.
- The number of distinct repos in the graded list went from 8 to 10; none of the 11 beginner-friendly repos from your pasted list reached the top 10, because their issues are not tier 1 (`bug` plus a friendly label).

## Graded results

```json
[
 {
  "item": "https://github.com/openedx/openedx-platform/issues/39114",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits 2026-11-04 to 11-06, not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBERs kdmccormick and mariajgrimaldi replied on outsiders' issues #39029 and #37409"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee, 0 comments, no linked PR (linked items are end-to-end-tests issues)"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "LMS gated:false while Studio reports gated for min_score 0"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one behavior: milestone only created when min_score >= 1"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "Django LMS/Studio logic, no accelerator or device"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "labels bug, help wanted; code fix"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "openedx AI_POLICY.md allows Anthropic Claude with disclosure"
   }
  ],
  "verdict": "accept"
 },
 {
  "item": "https://github.com/opensearch-project/query-insights-dashboards/issues/616",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "commits 2026-11-06, 11-03, 10-26"
   },
   {
    "name": "responds-to-issues",
    "grade": "unclear",
    "evidence": "only maintainer reply found was dzane17 on #605, opened by opensearch-ci-bot"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "ariktadas144 commented 2026-11-05 asking to be assigned with a plan; no maintainer reply"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "save failures are silent"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one UI flow"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "TypeScript UI"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "code fix"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no policy found"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/apache/fesod/issues/272",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "commits 2026-11-05 and 11-06"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBER delei replied on #1290 and #1258"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "open linked PR #693; MEMBER delei: 'I have already submitted a PR to try to fix it'"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "formulaValue returns incorrect results"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one formula behavior"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "Java library"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "code fix"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "AGENTS.md present, no ban"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/opensearch-project/dashboards-search-relevance/issues/757",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "commits 2026-11-06, 11-03, 11-02"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBER epugh replied on iprithv's issues #942 and #974"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no assignee; the 2026-09-25 request to be assigned is stale (over 30 days); 1 closed linked PR"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "title '[ENHANCEMENT] Improve UI test coverage'; asks for tests, not a fix for wrong behavior"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "'across Search Relevance modules': codebase-wide"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "UI tests"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no policy found"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/apache/maka/issues/3328",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits 2026-10-28 to 11-06, not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBERs YongGoose and liugddx replied on outsiders' issues 2026-11-06/07"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee; 1 closed linked PR (#4626, merged) and closed issues #3445, #3866"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "unclear",
    "evidence": "Astro-Han (2026-09-17): the repeated-interruption symptom has 'not enough in the report to reproduce it'; the crash part is tracked in #3312"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "no single piece of work: a failed-tool trace with no session export, and the maintainer-side reply asks for more information or to fold it into #3312"
   },
   {
    "name": "no-special-hardware",
    "grade": "unclear",
    "evidence": "timeline shows gpt-5.6-luna model calls; reproducing needs an LLM provider account"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "labels bug, help wanted, stale; code"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md: AI use allowed with disclosure, Generated-by trailer and human review"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/apache/seatunnel/issues/10172",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits all 2026-11-06"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "COLLABORATORs replied on outsiders' issues #13137, #9583, #9870 in the last 2 days"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "open PR #11823 already covers this issue (DanielLeens: 'The existing open PR #11823 already covers this issue'); Adarsh-Me asked to be assigned 2026-09-02"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "NoSuchMethodError for FsTracer.get with Hadoop 3.1.1 and Kerberos"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "a maintainer says a real fix needs an isolated dependency or classloading design"
   },
   {
    "name": "no-special-hardware",
    "grade": "fail",
    "evidence": "reproducing needs a Kerberos-enabled Hadoop 3.1.1 HDFS cluster"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "labels bug, help wanted; code"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "AGENTS.md present, no ban"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/Lightning-AI/pytorch-lightning/issues/18645",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits 2026-09-14 to 09-21 (all within 90 days), not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "COLLABORATOR deependujha replied on outsiders' issues #19230, #22077, #20970 in late October"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "sharmis-tp commented 2026-10-22 'I'd like to pick this up' with a plan; no maintainer reply, 15 days old"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "unclear",
    "evidence": "abhishekacharya200 (2026-10-11) could not reproduce on current master (2.7.0.dev); the issue may already be fixed"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one behavior: a spurious validation step when resuming with max_steps"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "CPU MNIST example, no accelerator"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "labels bug, help wanted, loops; code"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in .github/CONTRIBUTING.md"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/opensearch-project/opensearch-java/issues/611",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "commits 2026-11-06, 11-06, 11-04, 11-02, 11-01"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "COLLABORATOR reta replied on outsiders' issues #2252 and #2236 in late October"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "shalom-ath commented 2026-10-15 'I'd like to investigate this issue as my first contribution' and asked if anyone is working on it; no maintainer reply, 22 days old"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "unclear",
    "evidence": "2023 report; the claimant asks whether it still reproduces on main"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one failing client call"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "Java client"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "labels bug, good first issue; code"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present, no AI ban"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/open-telemetry/opentelemetry-python-contrib/issues/1704",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits all 2026-11-06, not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBERs aabmass, lmolkova and pmcollins replied on outsiders' issues #4953, #3609, #3808 in August 2026"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "open PRs #5056 and #5062 are cross-referenced; chrikrah (2026-10-11): '#5056 removes the retention instead of bounding it'; kellytran969 asked to work on it on 2026-10-01"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "received_messages_spans grows without bound when messages are not deleted: a memory leak"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one instrumentation (boto3sqs) and one dictionary"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "pure Python instrumentation, no hardware"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "labels bug, help wanted, instrumentation; code"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md and AGENTS.md describe responsible AI-assistant use; no ban"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/apache/beam/issues/20983",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits all 2026-11-06, not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBER-level committers shunping, kennknowles and derrickaw replied on outsiders' issues #39026, #39789, #38771 in Aug-Sep 2026"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee, no linked PR; rokadepiyush49's 2026-11-01 comment investigates but does not claim; Abacn replied 2026-11-02"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "start-build-env.sh fails with an error on Apple Silicon"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one script: remove the hardcoded amd64 platform"
   },
   {
    "name": "no-special-hardware",
    "grade": "fail",
    "evidence": "the body is from a MacBook Air M1; verifying the fix needs an Apple Silicon machine with Docker"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "labels build, P3, bug, good first issue; script fix"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no AI policy found in CONTRIBUTING.md"
   }
  ],
  "verdict": "reject"
 }
]
```


---

# 2026-11-06 scan 7 (4 reserved beginner slots, round-robin for the other 6)

Result: 1 accepted of 10 graded (openedx/openedx-platform #39114). The four reserved beginner-label slots were all rejected, because none of those issues is a bug.

## What changed

- `search-strategy.md`: 4 of the 10 graded slots are reserved for issues that carry a beginner label but no bug label (`good first issue`, `good-first-issue`, `🌱 good first issue`, `easy`, `easyfix`, `easy-fix`, `difficulty: easy`, `first-contribution`). They are filled first from that group in round-robin order (each repo's best beginner-only issue before any repo's second, then fewest comments, most recent update, busiest repo). The other 6 slots come from the main round-robin. I chose 4 because you did not give a number; say if you want a different split.
- I reused the search results from scan 5 (47 base queries plus 63 per-repo top-ups, run earlier today); only ranking and grading ran again.

The beginner-only group holds 103 issues in 39 repos. The first four by that order are all 0-comment issues: hackforla/website #8855, apache/magpie #1891, hackforla/peopledepot #731, hackforla/tdm-calculator #3399.

## Ranked read-out (graded list)

Accepted
1. **openedx/openedx-platform #39114** (main round-robin, in-repo #1, Q38): subsection prerequisite at min_score 0 is gated in Studio but not in the LMS. Difficulty: **moderate**. The code lives under `lms/djangoapps/gating/` and `openedx/core/lib/gating/api.py`; confirm with a maintainer whether to create the milestone or make Studio warn.

Rejected, main round-robin (grades reused from scans 5 and 6 after re-checking live state)
- opensearch query-insights-dashboards #616: `unclaimed` fails (claim comment 2026-11-05, no maintainer reply).
- apache/fesod #272: `unclaimed` fails (open PR #693).
- opensearch dashboards-search-relevance #757: not a bug fix (a test-coverage enhancement).
- apache/maka #3328: no single piece of work; needs a model account to reproduce.
- apache/seatunnel #10172: `unclaimed` (open PR #11823); needs a Kerberos Hadoop cluster.

Rejected, reserved beginner slots (new)
- hackforla/website #8855 (Q36/Q38; good first issue, 0 comments): `is-a-bug-fix` and `not-docs-only` fail. It fills in a github-handle value in a Markdown project file.
- apache/magpie #1891 (Q25; good first issue): `unclaimed` fails (2 closed linked PRs, #1892 and #1895, both merged) and `is-a-bug-fix` fails (a `feat(...)` eval-suite request).
- hackforla/peopledepot #731 (Q36; good first issue): `is-a-bug-fix` fails. It renames a Django model field and adds one, blocked on #719.
- hackforla/tdm-calculator #3399 (Q36; good first issue): `is-a-bug-fix` fails. It swaps one icon for a filled one.

## Friction

- The reserved slots do bring in different repos (hackforla x3, apache/magpie), but beginner-only issues are mostly maintainers' small tasks: content edits, schema updates, icon swaps and feature work, all rejected by `is-a-bug-fix`. The label identifies "easy", not "bug".
- The beginner-only ordering is fewest comments first, so it favors 0-comment issues that maintainers filed for their own team (hackforla's "role" and "size" labels mark internal work items).
- hackforla and similar volunteer projects have an assign-yourself process that the rubric does not know about.
- None of your pasted beginner-friendly Python repos (zulip, matplotlib, sklearn, mypy, frappe) appear: their beginner-labelled issues rank behind hackforla and apache on comment count and recency.

## Graded results

```json
[
 {
  "item": "https://github.com/openedx/openedx-platform/issues/39114",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits 2026-11-04 to 11-06, not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBERs kdmccormick and mariajgrimaldi replied on outsiders' issues #39029 and #37409"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee, 0 comments, no linked PR (linked items are end-to-end-tests issues)"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "LMS gated:false while Studio reports gated for min_score 0"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one behavior: milestone only created when min_score >= 1"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "Django LMS/Studio logic, no accelerator or device"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "labels bug, help wanted; code fix"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "openedx AI_POLICY.md allows Anthropic Claude with disclosure"
   }
  ],
  "verdict": "accept"
 },
 {
  "item": "https://github.com/opensearch-project/query-insights-dashboards/issues/616",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "commits 2026-11-06, 11-03, 10-26"
   },
   {
    "name": "responds-to-issues",
    "grade": "unclear",
    "evidence": "only maintainer reply found was dzane17 on #605, opened by opensearch-ci-bot"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "ariktadas144 commented 2026-11-05 asking to be assigned with a plan; no maintainer reply"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "save failures are silent"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one UI flow"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "TypeScript UI"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "code fix"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no policy found"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/apache/fesod/issues/272",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "commits 2026-11-05 and 11-06"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBER delei replied on #1290 and #1258"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "open linked PR #693; MEMBER delei: 'I have already submitted a PR to try to fix it'"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "formulaValue returns incorrect results"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one formula behavior"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "Java library"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "code fix"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "AGENTS.md present, no ban"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/opensearch-project/dashboards-search-relevance/issues/757",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "commits 2026-11-06, 11-03, 11-02"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBER epugh replied on iprithv's issues #942 and #974"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no assignee; the 2026-09-25 request to be assigned is stale (over 30 days); 1 closed linked PR"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "title '[ENHANCEMENT] Improve UI test coverage'; asks for tests, not a fix for wrong behavior"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "'across Search Relevance modules': codebase-wide"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "UI tests"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no policy found"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/apache/maka/issues/3328",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits 2026-10-28 to 11-06, not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBERs YongGoose and liugddx replied on outsiders' issues 2026-11-06/07"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee; 1 closed linked PR (#4626, merged) and closed issues #3445, #3866"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "unclear",
    "evidence": "Astro-Han (2026-09-17): the repeated-interruption symptom has 'not enough in the report to reproduce it'; the crash part is tracked in #3312"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "no single piece of work: a failed-tool trace with no session export, and the maintainer-side reply asks for more information or to fold it into #3312"
   },
   {
    "name": "no-special-hardware",
    "grade": "unclear",
    "evidence": "timeline shows gpt-5.6-luna model calls; reproducing needs an LLM provider account"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "labels bug, help wanted, stale; code"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md: AI use allowed with disclosure, Generated-by trailer and human review"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/apache/seatunnel/issues/10172",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits all 2026-11-06"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "COLLABORATORs replied on outsiders' issues #13137, #9583, #9870 in the last 2 days"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "open PR #11823 already covers this issue (DanielLeens: 'The existing open PR #11823 already covers this issue'); Adarsh-Me asked to be assigned 2026-09-02"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "NoSuchMethodError for FsTracer.get with Hadoop 3.1.1 and Kerberos"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "a maintainer says a real fix needs an isolated dependency or classloading design"
   },
   {
    "name": "no-special-hardware",
    "grade": "fail",
    "evidence": "reproducing needs a Kerberos-enabled Hadoop 3.1.1 HDFS cluster"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "labels bug, help wanted; code"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "AGENTS.md present, no ban"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/hackforla/website/issues/8855",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits 2026-11-02 to 11-06, not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBER meghan-oconnell replied on issues #8845, #8850, #8856 opened by others on 2026-11-03"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee, 0 comments, no linked PR (linked items are issues)"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "fills a github-handle value for a team member in a project file; no wrong behavior is reported"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one named edit in `_projects/ballot-nav.md`"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "Docker site build, no hardware"
   },
   {
    "name": "not-docs-only",
    "grade": "fail",
    "evidence": "the only change is a value in a Markdown content file (`_projects/ballot-nav.md`), not code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no AI policy found in CONTRIBUTING.md"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/apache/magpie/issues/1891",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits 2026-11-05 to 11-07, not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBER potiuk replied on outsiders' issues #1925 and #1768 (2026-10-23 and 11-01)"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "two linked PRs are closed (#1892 and #1895, both merged 2026-10-30 and 10-31): the rubric allows fewer than 2"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "titled 'feat(hook-manager): add Step P.3b eval suite' and asks for a new eval suite; not a bug"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "Part 1 is one eval suite; Part 2 is deferred"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "Part 1 needs no special hardware (Part 2 needs JDK 17+ and Maven)"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "adds tests/evals; code"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "AGENTS.md describes AI-assisted contributions with disclosure; no ban"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/hackforla/peopledepot/issues/731",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits 2026-10-29 to 11-02, not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBERs fyliu and egcuriel replied on others' issues #734 and #605 in late October to November"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee, 0 comments, no linked PR; depends on #719"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "asks to rename a model field and add a new one ('Update Table: Project'); a schema change task, not wrong behavior"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one model: rename current_status and add is_cop"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "Django model, no hardware"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "Django model change plus a test"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no AI policy found in CONTRIBUTING.md"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/hackforla/tdm-calculator/issues/3399",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits all 2026-11-05, not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBERs kellymsimpson and ExperimentsInHonesty replied on #3628, #3603, #2808 (2026-11-05 to 11-07)"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee, 0 comments, no linked PR; depends on #2227"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "swap the Admin notes icon for a filled one to match iconography rules; a visual update, not a bug"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one icon"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "React front end, no hardware"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "front-end code change"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no AI policy found"
   }
  ],
  "verdict": "reject"
 }
]
```


---

# 2026-11-06 scan 8 (non-bug title filter, reserved beginner slots ordered by repo diversity)

Result: 1 accepted of 10 graded (openedx/openedx-platform #39114). All four reserved beginner slots were rejected again, but they now come from four different repos instead of three hackforla and one apache issue.

## What changed

- `search-strategy.md`, new drop filter: a title that starts (case-insensitively) with `feat(`, `feat:`, `[feat`, `enhancement`, `[enhancement` or the word `update` is dropped, on top of the existing `Feature` and `[Feature` titles. It removed 107 more issues from the pool (202 dropped as feature-like titles, up from 95). Among the issues I graded before, it removes opensearch dashboards-search-relevance #757 (`[ENHANCEMENT]`) and hackforla/peopledepot #731 (`Update Table: Project`); apache/magpie #1891 (`feat(...)`) is dropped as well.
- `search-strategy.md`, reserved beginner slots: still 4 of the 10 graded slots, but now one issue per repo. Repos are ordered by their total result count across all queries (busiest first) and each repo contributes its most recently updated beginner-only issue. Comment count no longer plays a part.
- I reused the search results from scan 5 (47 base queries plus 63 per-repo top-ups, run earlier today); only filtering, ranking and grading ran again.

After the filters: 2486 issues, 1379 after the per-repo quota of 20. The beginner-only group now has 89 issues in 38 repos. The four busiest repos in it are pytorch/pytorch (102 results), frappe/frappe (101), hackforla/website (42) and matplotlib/matplotlib (40).

## Ranked read-out (graded list)

Accepted
1. **openedx/openedx-platform #39114** (main round-robin, Q38): subsection prerequisite at min_score 0 is gated in Studio but not in the LMS. Difficulty: **moderate**. The code lives under `lms/djangoapps/gating/` and `openedx/core/lib/gating/api.py`; confirm with a maintainer whether to create the milestone or make Studio warn.

Rejected, main round-robin (grades reused after re-checking live state)
- opensearch query-insights-dashboards #616: `unclaimed` fails (claim comment 2026-11-05, no maintainer reply).
- apache/fesod #272: `unclaimed` fails (open PR #693).
- apache/maka #3328: no single piece of work; needs a model account to reproduce.
- apache/seatunnel #10172: `unclaimed` (open PR #11823); needs a Kerberos Hadoop cluster.
- Lightning-AI/pytorch-lightning #18645: `unclaimed` fails (claim comment 2026-10-22, no maintainer reply); a commenter could not reproduce it on current master.

Rejected, reserved beginner slots (one per repo)
- pytorch/pytorch #89197 (Q14; good first issue): `is-a-bug-fix` unclear. A commenter reports bool tensors now work on gloo on the 2026-10-02 nightly, and no maintainer has confirmed it. The old claim (2026-07-23) is stale and does not block.
- frappe/frappe #42122 (Q39; good first issue): `is-a-bug-fix` and `scope-feasible` fail. It is a feature request for drag-and-drop tree editing, and a maintainer said the code has security problems.
- hackforla/website #8850 (Q36; good first issue): `is-a-bug-fix` and `not-docs-only` fail. It fills in a github-handle value in a Markdown project file (the sibling of #8855 graded earlier).
- matplotlib/matplotlib #32556 (Q47; first-contribution): `unclaimed` fails (a contributor claimed it on 2026-10-30 with a plan, no maintainer reply). The `ai-contribution-allowed` check is unclear: matplotlib closes AI-generated PRs for its Good first issue items, and this one has the first-contribution label.

## Friction

- The title filter works on obvious cases but cannot see that hackforla's "Populate github-handle ..." issues are content edits, so one still reached a reserved slot. Labels like `role: front end` and `size: 0.25pt` mark these work items; I did not add them to the filter since you listed only the title patterns.
- Beginner labels on very large repos (pytorch, frappe) mostly mark old issues: #89197 was filed in 2022 and may already be fixed; #42122 is a feature request mislabelled as a good first issue.
- A busy-repo ordering favors the biggest projects (pytorch, frappe), which are least likely to have a free, simple issue.
- matplotlib's policy on AI-generated PRs for good first issues is stricter than a plain "disclose AI use": it is the first repo in these scans where the label itself changes the AI rule.

## Graded results

```json
[
 {
  "item": "https://github.com/openedx/openedx-platform/issues/39114",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits 2026-11-04 to 11-06, not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBERs kdmccormick and mariajgrimaldi replied on outsiders' issues #39029 and #37409"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee, 0 comments, no linked PR (linked items are end-to-end-tests issues)"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "LMS gated:false while Studio reports gated for min_score 0"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one behavior: milestone only created when min_score >= 1"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "Django LMS/Studio logic, no accelerator or device"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "labels bug, help wanted; code fix"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "openedx AI_POLICY.md allows Anthropic Claude with disclosure"
   }
  ],
  "verdict": "accept"
 },
 {
  "item": "https://github.com/opensearch-project/query-insights-dashboards/issues/616",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "commits 2026-11-06, 11-03, 10-26"
   },
   {
    "name": "responds-to-issues",
    "grade": "unclear",
    "evidence": "only maintainer reply found was dzane17 on #605, opened by opensearch-ci-bot"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "ariktadas144 commented 2026-11-05 asking to be assigned with a plan; no maintainer reply"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "save failures are silent"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one UI flow"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "TypeScript UI"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "code fix"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no policy found"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/apache/fesod/issues/272",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "commits 2026-11-05 and 11-06"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBER delei replied on #1290 and #1258"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "open linked PR #693; MEMBER delei: 'I have already submitted a PR to try to fix it'"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "formulaValue returns incorrect results"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one formula behavior"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "Java library"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "code fix"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "AGENTS.md present, no ban"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/apache/maka/issues/3328",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits 2026-10-28 to 11-06, not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBERs YongGoose and liugddx replied on outsiders' issues 2026-11-06/07"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee; 1 closed linked PR (#4626, merged) and closed issues #3445, #3866"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "unclear",
    "evidence": "Astro-Han (2026-09-17): the repeated-interruption symptom has 'not enough in the report to reproduce it'; the crash part is tracked in #3312"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "no single piece of work: a failed-tool trace with no session export, and the maintainer-side reply asks for more information or to fold it into #3312"
   },
   {
    "name": "no-special-hardware",
    "grade": "unclear",
    "evidence": "timeline shows gpt-5.6-luna model calls; reproducing needs an LLM provider account"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "labels bug, help wanted, stale; code"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md: AI use allowed with disclosure, Generated-by trailer and human review"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/apache/seatunnel/issues/10172",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits all 2026-11-06"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "COLLABORATORs replied on outsiders' issues #13137, #9583, #9870 in the last 2 days"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "open PR #11823 already covers this issue (DanielLeens: 'The existing open PR #11823 already covers this issue'); Adarsh-Me asked to be assigned 2026-09-02"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "NoSuchMethodError for FsTracer.get with Hadoop 3.1.1 and Kerberos"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "a maintainer says a real fix needs an isolated dependency or classloading design"
   },
   {
    "name": "no-special-hardware",
    "grade": "fail",
    "evidence": "reproducing needs a Kerberos-enabled Hadoop 3.1.1 HDFS cluster"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "labels bug, help wanted; code"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "AGENTS.md present, no ban"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/Lightning-AI/pytorch-lightning/issues/18645",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits 2026-09-14 to 09-21 (all within 90 days), not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "COLLABORATOR deependujha replied on outsiders' issues #19230, #22077, #20970 in late October"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "sharmis-tp commented 2026-10-22 'I'd like to pick this up' with a plan; no maintainer reply, 15 days old"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "unclear",
    "evidence": "abhishekacharya200 (2026-10-11) could not reproduce on current master (2.7.0.dev); the issue may already be fixed"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one behavior: a spurious validation step when resuming with max_steps"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "CPU MNIST example, no accelerator"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "labels bug, help wanted, loops; code"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in .github/CONTRIBUTING.md"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/pytorch/pytorch/issues/89197",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits all 2026-11-07, not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "COLLABORATORs ngimel, nullplay and rtimpe replied on outsiders' issues #203860, #196958, #200260 on 2026-11-06"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee, no linked PR; the 2026-07-23 claim and the 2026-08-15 follow-up are over 30 days old with no PR, so they do not block"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "unclear",
    "evidence": "fitzyracing1 (2026-10-04): bool tensors now work on gloo on nightly 2.15.0.dev20261002 for broadcast, all_reduce and more; no maintainer has confirmed or closed it"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one behavior: gloo rejects torch.bool tensors in collectives"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "gloo is the CPU backend; the repro needs no GPU"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "labels oncall: distributed, good first issue, triaged; C++ and Python code"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "AI_POLICY.md allows AI tools with disclosure, human review and no raw AI text pasted into discussions"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/frappe/frappe/issues/42122",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits all 2026-11-06, not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "unclear",
    "evidence": "only one maintainer reply on an outsider's issue found in the sample (iamejaaz on #46543, 2026-11-06)"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee, no open linked PR (PR #42121 is closed)"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "body: 'Is your feature request related to a problem?'; a drag-and-drop tree editing session, not a bug"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "a multi-part feature (Edit Tree session, drag rows, Save and Discard); maintainer iamejaaz called the code poor with security issues"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "front-end and back-end code, no hardware"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "code"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": ".github/CONTRIBUTING.md and AGENTS.md present, no ban"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/hackforla/website/issues/8850",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits 2026-11-02 to 11-06, not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBER meghan-oconnell replied on #8845, #8850, #8856 on 2026-11-03"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee; the 2 comments are merge-team review notes, not claims; no linked PR"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "fills in a github-handle value for a team member in a project file; no wrong behavior is reported"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one named edit in `_projects/ballot-nav.md`"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "Docker site build, no hardware"
   },
   {
    "name": "not-docs-only",
    "grade": "fail",
    "evidence": "the only change is a value in a Markdown content file, not code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no AI policy found in CONTRIBUTING.md"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/matplotlib/matplotlib/issues/32556",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits 2026-11-04 and 11-05, not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "MEMBERs rcomer, melissawm and timhoffm replied on outsiders' issues #32574, #32397, #32560 (2026-10-30 to 11-05)"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "Akash-Dhanwate commented 2026-10-30 'I'd like to investigate this issue as a first contribution' with a plan; no maintainer reply to the claim"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "GLib io error when SIGINT arrives while the GTK icon loads, with a repro"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one failure in the GTK3 backend icon loading"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "needs a Linux GTK3 environment, no special hardware"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "labels GUI: gtk, first-contribution; code"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "unclear",
    "evidence": "matplotlib's contributing guide says AI-generated pull requests for 'Good first issue' items will be closed; this one is labelled first-contribution, and it is unclear whether the same rule applies"
   }
  ],
  "verdict": "reject"
 }
]
```


---

# 2026-11-15 scan 11 (61 queries, priority lists A and B first, patched rubric, top 20 graded): 2 accepted of 20

## Result
I'd take **openmed #5296** first, then **fastmcp #6491**.

Search: 61 queries, 28 of them for the priority repos, then label-free top-ups for 15 priority repos. 2,678 unique results, 2,078 after the drop filters. The graded 20 came from the priority repos only (list A, 17 repos, plus list B: MONAI, nnUNet, openmed). nnUNet and google/adk-python returned nothing after the filters, and MONAI only 12 issues.

## Ranked read-out

### Accepted
1. **maziyarpanahi/openmed #5296**: add a synthetic free-text quasi-identifier fixture set. Surfaced by Q23/Q24 (good first issue and help wanted, priority list B); ranked 1st as a beginner-labelled issue in the reserved slots.
   - One fixture file, a README and one integrity test; the issue names the files, the acceptance criteria and the pytest command. Open, no assignee, no comments, no linked PR.
   - The owner merged outside PRs on 2026-11-08 and 25 of 28 outsider PRs merged in 90 days. Release 2026-11-13. CONTRIBUTING.md and AGENTS.md exist; no AI rule found.
   - Difficulty: straightforward.
   - Caveat: the owner opened over 20 PRs on 2026-11-16 and could take it himself, so claim it first. Siblings #5295, #5184, #4981 and #5089 carry the same labels.
2. **PrefectHQ/fastmcp #6491**: proxy fails for server-initiated input with legacy clients. Surfaced by Q17/Q4 (bug, priority list A); ranked 8th on its tier-2 bug label, 1 comment, and being 2 days old.
   - Real error text and a results table; no assignee or linked PR; the one comment confirms the repro and does not claim it. Passes `responds-to-issues` because zzstoatzz (a committer) replied on #4920 and #5236.
   - Difficulty: ambitious (new input-required protocol, two sub-cases). Raise the approach with a maintainer first.
   - Caveat: fastmcp's bot closes PRs for unassigned issues, so ask to be assigned before coding.

### Rejected
| # | Candidate | Sunk by |
|---|---|---|
| 2 | sglang #27214 | is-a-bug-fix, scope-feasible, unclaimed (open PR #27442), no-special-hardware |
| 3 | vllm #63465 | no-special-hardware (H200) |
| 4 | paperclip #9153 | responds-to-issues (no committer replies, 0 outsider merges) |
| 5 | openclaw #197381 | unclaimed (open PR #197811) |
| 6 | dify #44722 | unclaimed (claim 2026-11-04), Cloud-only repro |
| 7 | langchain4j #7122 | scope-feasible (already fixed on main) |
| 9 | langgraph #9676 | unclaimed (claim 2026-11-12) |
| 10 | openmed #4814 | is-a-bug-fix, scope-feasible, macos-compatible (Android migration) |
| 11 | MONAI #8489 | no-special-hardware, macos-compatible (A100/Linux-only segfault) |
| 12 | sglang #37524 | tracking issue, GPU |
| 13 | browser-use #4803 | is-a-bug-fix (a question; expected behaviour) |
| 14 | deer-flow #6751 | unclaimed (assignment request + fork PR) |
| 15 | spring-ai #4809 | unclaimed (fix offered 2026-11-11) |
| 16 | opencode #36347 | scope-feasible (open design for durable waits) |
| 17 | hermes-agent #156302 | is-a-bug-fix (labelled type/feature) |
| 18 | gemini-cli #21418 | is-a-bug-fix (feature, maintainer only) |
| 19 | agent-framework #962 | not-docs-only, unclaimed |
| 20 | chrome-devtools-mcp #3422 | is-a-bug-fix (dependency bump, already handled by dependabot) |

## Search log
- Q1: 100 results — `gh search issues --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-ai,BerriAI,letta-ai,langfuse --label bug --lan...`
- Q2: 100 results — `gh search issues --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-ai,BerriAI,letta-ai,langfuse --label bug --lan...`
- Q3: 0 results — `gh search issues --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-ai,BerriAI,letta-ai,langfuse --label bug --lan...`
- Q4: 45 results — `gh search issues --repo pydantic/pydantic-ai,huggingface/smolagents,vercel/ai,microsoft/autogen,google/adk-python,Significant-Gravitas/AutoGPT,microso...`
- Q5: 1 results — `gh search issues --repo pydantic/pydantic-ai,huggingface/smolagents,vercel/ai,microsoft/autogen,google/adk-python,Significant-Gravitas/AutoGPT,microso...`
- Q6: 4 results — `gh search issues --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-ai,BerriAI,letta-ai,langfuse --label "good fir...`
- Q7: 0 results — `gh search issues --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-ai,BerriAI,letta-ai,langfuse --label "good fir...`
- Q8: 53 results — `gh search issues --repo langgenius/dify,pytorch/pytorch,aws/sagemaker-python-sdk,deepspeedai/DeepSpeed,jax-ml/jax,google-deepmind/gemma,SWE-bench/SWE-...`
- Q9: 9 results — `gh search issues --repo langgenius/dify,pytorch/pytorch,aws/sagemaker-python-sdk,deepspeedai/DeepSpeed,jax-ml/jax,google-deepmind/gemma,SWE-bench/SWE-...`
- Q10: 24 results — `gh search issues --repo langgenius/dify --label "🐞 bug" --state open --limit 100 --no-assignee --updated ">2026-08-18" --json title,repository,labels,...`
- Q11: 100 results — `gh search issues --repo confident-ai/deepeval,vllm-project/vllm,ollama/ollama,qdrant/qdrant,weaviate/weaviate,milvus-io/milvus,pgvector/pgvector,docli...`
- Q12: 8 results — `gh search issues --repo confident-ai/deepeval,vllm-project/vllm,ollama/ollama,qdrant/qdrant,weaviate/weaviate,milvus-io/milvus,pgvector/pgvector,docli...`
- Q13: 1 results — `gh search issues --repo confident-ai/deepeval,vllm-project/vllm,ollama/ollama,qdrant/qdrant,weaviate/weaviate,milvus-io/milvus,pgvector/pgvector,docli...`
- Q14: 100 results — `gh search issues --repo openclaw/openclaw,n8n-io/n8n,NousResearch/hermes-agent,browser-use/browser-use,ChromeDevTools/chrome-devtools-mcp,backnotprop/...`
- Q15: 0 results — `gh search issues --repo openclaw/openclaw,n8n-io/n8n,NousResearch/hermes-agent,browser-use/browser-use,ChromeDevTools/chrome-devtools-mcp,backnotprop/...`
- Q16: 4 results — `gh search issues --repo openclaw/openclaw,n8n-io/n8n,NousResearch/hermes-agent,browser-use/browser-use,ChromeDevTools/chrome-devtools-mcp,backnotprop/...`
- Q17: 94 results — `gh search issues --repo PrefectHQ/fastmcp,sgl-project/sglang,paperclipai/paperclip,anomalyco/opencode,google-gemini/gemini-cli --label bug --language ...`
- Q18: 4 results — `gh search issues --repo PrefectHQ/fastmcp,sgl-project/sglang,paperclipai/paperclip,anomalyco/opencode,google-gemini/gemini-cli --label "good first iss...`
- Q19: 1 results — `gh search issues --repo PrefectHQ/fastmcp,sgl-project/sglang,paperclipai/paperclip,anomalyco/opencode,google-gemini/gemini-cli --label "help wanted" -...`
- Q20: 3 results — `gh search issues --repo google/adk-python,microsoft/agent-framework,langchain-ai/langgraph,vllm-project/vllm,langgenius/dify --label "help wanted" --l...`
- Q21: 0 results — `gh search issues --repo google/adk-python,microsoft/agent-framework --label "good first issue" --language python,javascript,typescript --state open --...`
- Q22: 8 results — `gh search issues --repo Project-MONAI/MONAI,MIC-DKFZ/nnUNet,maziyarpanahi/openmed --label bug --language python,javascript,typescript --state open --l...`
- Q23: 62 results — `gh search issues --repo Project-MONAI/MONAI,MIC-DKFZ/nnUNet,maziyarpanahi/openmed --label "good first issue" --language python,javascript,typescript -...`
- Q24: 100 results — `gh search issues --repo Project-MONAI/MONAI,MIC-DKFZ/nnUNet,maziyarpanahi/openmed --label "help wanted" --language python,javascript,typescript --stat...`
- Q25: 0 results — `gh search issues "validation OR schema OR validate" --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-ai,BerriAI,...`
- Q26: 0 results — `gh search issues "validation OR schema OR validate" --repo pydantic/pydantic-ai,huggingface/smolagents,vercel/ai,google/adk-python,browser-use/browser...`
- Q27: 0 results — `gh search issues "missing test OR add tests OR test coverage" --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-a...`
- Q28: 2 results — `gh search issues --repo microsoft/ai-agents-for-beginners --state open --limit 100 --no-assignee --updated ">2026-08-18" --json title,repository,label...`
- Q29: 7 results — `gh search issues --repo TypeSafeAI/modex,TypeSafeAI/jev-harness,TypeSafeAI/typesafe-router,TypeSafeAI/typesafe-ui,TypeSafeAI/clarity-judge --state ope...`
- Q30: 100 results — `gh search issues --owner pytorch --label bug --language python --state open --limit 100 --no-assignee --updated ">2026-08-18" --json title,repository,...`
- Q31: 11 results — `gh search issues --owner pytorch --label "good first issue" --language python --state open --limit 100 --no-assignee --updated ">2026-08-18" --json ti...`
- Q32: 0 results — `gh search issues --owner scipy --label "good first issue" --language python --state open --limit 100 --no-assignee --updated ">2026-08-18" --json titl...`
- Q33: 24 results — `gh search issues --repo tensorflow/tensorflow,scikit-learn/scikit-learn,keras-team/keras,huggingface/transformers,stanfordnlp/dspy,OpenHands/OpenHands...`
- Q34: 50 results — `gh search issues --repo tensorflow/tensorflow,scikit-learn/scikit-learn,keras-team/keras,huggingface/transformers,stanfordnlp/dspy,OpenHands/OpenHands...`
- Q35: 0 results — `gh search issues --repo tensorflow/tensorflow,scikit-learn/scikit-learn,keras-team/keras,huggingface/transformers,stanfordnlp/dspy,OpenHands/OpenHands...`
- Q36: 85 results — `gh search issues --repo django/django,pallets/flask,fastapi/fastapi,wagtail/wagtail,scrapy/scrapy,pandas-dev/pandas,Lightning-AI/pytorch-lightning,mat...`
- Q37: 1 results — `gh search issues --repo django/django,pallets/flask,fastapi/fastapi,wagtail/wagtail,scrapy/scrapy,pandas-dev/pandas,Lightning-AI/pytorch-lightning,mat...`
- Q38: 63 results — `gh search issues --owner apache --label "good first issue" --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit ...`
- Q39: 100 results — `gh search issues --owner apache --label "help wanted" --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit 100 -...`
- Q40: 100 results — `gh search issues --owner apache --label bug --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit 100 --sort upda...`
- Q41: 12 results — `gh search issues --owner pytorch --label "good first issue" --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit...`
- Q42: 2 results — `gh search issues --owner pytorch --label "help wanted" --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit 100 ...`
- Q43: 100 results — `gh search issues --owner pytorch --label bug --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit 100 --sort upd...`
- Q44: 11 results — `gh search issues --owner opensearch-project --label "good first issue" --language javascript,python,typescript,css,html,java,scss,dockerfile --state o...`
- Q45: 1 results — `gh search issues --owner opensearch-project --label "help wanted" --language javascript,python,typescript,css,html,java,scss,dockerfile --state open -...`
- Q46: 100 results — `gh search issues --owner opensearch-project --label bug --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit 100...`
- Q47: 11 results — `gh search issues --owner open-telemetry --label "good first issue" --language javascript,python,typescript,css,html,java,scss,dockerfile --state open ...`
- Q48: 23 results — `gh search issues --owner open-telemetry --label "help wanted" --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --lim...`
- Q49: 42 results — `gh search issues --owner open-telemetry --label bug --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit 100 --s...`
- Q50: 87 results — `gh search issues --owner backstage,openedx,hackforla --label "good first issue" --language javascript,python,typescript,css,html,java,scss,dockerfile ...`
- Q51: 100 results — `gh search issues --owner backstage,openedx,hackforla --label "help wanted" --language javascript,python,typescript,css,html,java,scss,dockerfile --sta...`
- Q52: 33 results — `gh search issues --owner backstage,openedx,hackforla --label bug --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --...`
- Q53: 100 results — `gh search issues --repo encode/httpx,python/mypy,pypa/hatch,saltstack/salt,internetarchive/openlibrary,wger-project/wger,zulip/zulip,frappe/erpnext,fr...`
- Q54: 2 results — `gh search issues --repo encode/httpx,python/mypy,pypa/hatch,saltstack/salt,internetarchive/openlibrary,wger-project/wger,zulip/zulip,frappe/erpnext,fr...`
- Q55: 14 results — `gh search issues --repo fastapi/fastapi,scrapy/scrapy,encode/httpx,psf/requests,scikit-learn/scikit-learn,pandas-dev/pandas,Lightning-AI/pytorch-light...`
- Q56: 4 results — `gh search issues --repo fastapi/fastapi,scrapy/scrapy,encode/httpx,psf/requests,scikit-learn/scikit-learn,pandas-dev/pandas,Lightning-AI/pytorch-light...`
- Q57: 0 results — `gh search issues --repo fastapi/fastapi,scrapy/scrapy,encode/httpx,psf/requests,scikit-learn/scikit-learn,pandas-dev/pandas,Lightning-AI/pytorch-light...`
- Q58: 0 results — `gh search issues --repo fastapi/fastapi,scrapy/scrapy,encode/httpx,psf/requests,scikit-learn/scikit-learn,pandas-dev/pandas,Lightning-AI/pytorch-light...`
- Q59: 0 results — `gh search issues --repo fastapi/fastapi,scrapy/scrapy,encode/httpx,psf/requests,scikit-learn/scikit-learn,pandas-dev/pandas,Lightning-AI/pytorch-light...`
- Q60: 0 results — `gh search issues --repo fastapi/fastapi,scrapy/scrapy,encode/httpx,psf/requests,scikit-learn/scikit-learn,pandas-dev/pandas,Lightning-AI/pytorch-light...`
- Q61: 7 results — `gh search issues --repo fastapi/fastapi,scrapy/scrapy,encode/httpx,psf/requests,scikit-learn/scikit-learn,pandas-dev/pandas,Lightning-AI/pytorch-light...`

## Friction
- hermes-agent #156302 carries `type/feature`, which the label filter does not know. Add `type/feature` (and any label containing `feature`) to the label drop.
- gemini-cli #21418 is a `maintainer only` rollup. Add `maintainer only` and `workstream-rollup` to the label drop.
- Claim comments without a maintainer answer sank 6 of 20 (dify, langgraph, deer-flow, spring-ai, agent-framework, sglang). The pre-grading filters cannot see them.

## Graded results
```json
[
 {
  "item": "https://github.com/maziyarpanahi/openmed/issues/5296",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "owner merged outside PRs by Mr45401 and Shellghostt on 2026-11-08; 25 outsider PRs merged in 90 days (patched alternative)"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee, 0 comments, no linked PR; maintainer-opened clause exempt because the issue is labelled good first issue/help wanted"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "one self-contained fixture set plus integrity test with acceptance criteria (test gap)"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one bounded task; files and pytest command named"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "pass",
    "evidence": "labels: good first issue, help wanted"
   },
   {
    "name": "has-repro",
    "grade": "fail",
    "evidence": "not a bug report; acceptance criteria instead"
   },
   {
    "name": "shipped-recently",
    "grade": "pass",
    "evidence": "release 2026-11-13"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "pass",
    "evidence": "25 of 28 outsider PRs merged in 90 days"
   }
  ],
  "verdict": "accept"
 },
 {
  "item": "https://github.com/sgl-project/sglang/issues/27214",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "open PR #27442 linked; eight claim/interest comments, no maintainer answer"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "\"model: support new diffusion models\" is a running feature list"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "container list of models to add"
   },
   {
    "name": "no-special-hardware",
    "grade": "fail",
    "evidence": "diffusion model support needs GPUs"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/vllm-project/vllm/issues/63465",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "8 outsider PRs merged by maintainers in 90 days"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "unclear",
    "evidence": "distributed KV-cache connector subsystem"
   },
   {
    "name": "no-special-hardware",
    "grade": "fail",
    "evidence": "environment: \"Validation hardware: NVIDIA H200\", CUDA PyTorch"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/paperclipai/paperclip/issues/9153",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "fail",
    "evidence": "no committer reply in 7 sampled issues with comments; 0 outsider PRs merged by a maintainer in 90 days"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "skill creation shows an unexplained validation error"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "fail",
    "evidence": "steps are \"just try to create a new skill\""
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/openclaw/openclaw/issues/197381",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "open linked PR #197811 (and fork PR Ayushdevo/openclaw#2)"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "unclear",
    "evidence": "clawsweeper: needs-product-decision"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/langgenius/dify/issues/44722",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "GYJ99 claimed \"I'd like to investigate\" on 2026-11-04 with no maintainer reply (12 days old)"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "unclear",
    "evidence": "follow-up to #44080; merged PR #44731 already covers part of it"
   },
   {
    "name": "no-special-hardware",
    "grade": "fail",
    "evidence": "reproduced only on Dify Cloud with a hosted deepseek model (paid cloud account)"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/langchain4j/langchain4j/issues/7122",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "comment 2026-11-15: fixed on main by #6942 (merged 2026-11-02); no work left"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/PrefectHQ/fastmcp/issues/6491",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "zzstoatzz (Collaborator, committed 2026-11-15) replied on #4920 (2026-08-31) and #5236 (2026-09-23)"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee, no linked PR; the one comment confirms the repro and does not claim it"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "proxy raises MCPError -32602 / \"Elicitation not supported\" for legacy clients"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one proxy area, two sub-cases; open design edge on the new input-required protocol raises difficulty only"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "fail",
    "evidence": "labels: bug, server"
   },
   {
    "name": "has-repro",
    "grade": "pass",
    "evidence": "error text, a results table and an in-memory repro (script placeholder not filled in)"
   },
   {
    "name": "shipped-recently",
    "grade": "pass",
    "evidence": "release 2026-11-13"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "pass",
    "evidence": "5 of 30 outsider PRs merged in 90 days"
   }
  ],
  "verdict": "accept"
 },
 {
  "item": "https://github.com/langchain-ai/langgraph/issues/9676",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "Kylinny: \"I'd like to take this on\" on 2026-11-12, no maintainer reply"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "unclear",
    "evidence": "concurrency race in PregelLoop.put_writes; earlier fix PR #8114 was closed by a bot because the author was not assigned"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/maziyarpanahi/openmed/issues/4814",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "architecture migration: back both Kotlin engines with the core"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "multi-file migration plus migration guide, allowlist and CHANGELOG"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "fail",
    "evidence": "Android Kotlin work needs an Android toolchain"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/Project-MONAI/MONAI/issues/8489",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "ericspod (Member) replied 2026-11-15 and 2026-08-25"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "fail",
    "evidence": "segfault seen on Linux + A100 CI node; commenter could not reproduce on CPU"
   },
   {
    "name": "macos-compatible",
    "grade": "fail",
    "evidence": "reproduced only on Linux"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/sgl-project/sglang/issues/37524",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "\"GLM-5.3-Flash bug tracking\" checklist of other issues"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "umbrella issue"
   },
   {
    "name": "no-special-hardware",
    "grade": "fail",
    "evidence": "GLM serving needs GPUs"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/browser-use/browser-use/issues/4803",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "unclear",
    "evidence": "no committer replies found; 0 outsider PRs merged"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "titled \"Question\"; comment explains Chrome 137+ ignores --load-extension (expected behaviour)"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "fail",
    "evidence": "0 of 41 outsider PRs merged"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/bytedance/deer-flow/issues/6751",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "wycocoyu asked to be assigned on 2026-11-10 and has an open fork PR; two triage requests unanswered"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "unclear",
    "evidence": "no field in the 400 response identified yet"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "unclear",
    "evidence": "traceback shows Windows paths (E:\\project)"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/spring-projects/spring-ai/issues/4809",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "prathap2k offered a fix on 2026-11-11 (\"happy to open a PR\") with no maintainer reply (5 days old)"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "streamed web-search annotations are dropped"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/anomalyco/opencode/issues/36347",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "prototype work reported by gcomneno on 2026-09-24; closed side issues"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "\"We need a durable model\" for waits: open design across permissions and questions"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "unclear",
    "evidence": "a field report is from Windows"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/NousResearch/hermes-agent/issues/156302",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "teknium1 (Collaborator) replied 2026-11-15"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "labelled type/feature: expose max_retries in the kanban_create tool schema"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/google-gemini/gemini-cli/issues/21418",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "feature request to design \"deep validation\""
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "workstream-rollup / maintainer only"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/microsoft/agent-framework/issues/962",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "fabbrik: \"I'd like to take this\" on 2026-11-14"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "fail",
    "evidence": "asks for setup instructions for the integration tests (docs)"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/3422",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "vulnerable bundled dependency, not wrong behaviour; dependabot bumps of fast-uri have merged repeatedly"
   },
   {
    "name": "scope-feasible",
    "grade": "unclear",
    "evidence": "a dependency bump in a bundled SDK build"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 }
]
```


---

# 2026-11-15 scan 12 (61 queries at --limit 500, per-repo quota 100, batches of 10 until 5 accepted): 6 accepted of 50 graded

## Result
Six issues pass; I'd take them in this order: openmed #5296, chrome-devtools-mcp #3412, openmed #5121, openmed #5097, fastmcp #6398, fastmcp #6491.

Search: 61 queries at `--limit 500` (27 for the priority repos first) plus label-free top-ups for 87 listed repos under the 100-issue quota. 12,318 unique results, 9,146 after the drop filters. Queries that hit the 500 cap: Q2, Q40, Q53. Q11 and Q12 were rate-limited on the first pass and re-run. Five batches were available; the scan stopped after batch 5 because 6 accepted passed the target of 5. Accepts per batch: 2, 0, 1, 1, 2. All 50 graded issues came from the priority repos (lists A and B); nothing from the other 232 repos was needed.

## Ranked read-out

### Accepted
1. **openmed #5296**: synthetic free-text quasi-identifier fixture set (batch 1). One fixture file, a README and one integrity test; unclaimed, no PR. Difficulty: straightforward. Siblings with the same labels: #5295, #5184, #4981, #5089.
2. **chrome-devtools-mcp #3412**: an invalid `propertyName` regex in `query_heapsnapshot_objects` returns an internal TypeError (batch 4). Maintainer-confirmed (`confirmed`, `p3`), no comments, no linked PR. Sibling PR #3030 fixes the same problem for `className`, so it shows the shape of the fix. Needs the Google CLA. Difficulty: straightforward.
3. **openmed #5121**: redact OpenAI tool-call arguments as JSON and keep tool names and IDs intact (batch 3). Owner-filed P1 bug with acceptance criteria, one module (`privacy_proxy/app.py`) and a pytest command. PR #5501 touches streamed tool-call arguments, so check for overlap. Difficulty: moderate.
4. **openmed #5097**: refuse image, audio and file content parts at the privacy proxy (batch 5). Owner-filed P0 bug, bounded input validation with tests and a docs line. It edits the same file as #5121, so take one at a time. Difficulty: straightforward to moderate.
5. **fastmcp #6398**: `ListSkillsResult` accepts duplicate skill URIs (batch 5). A schema-validation tweak with a runnable repro. Caveat: mohitdixit02 posted a verification and a proposed validator on 2026-11-10 without claiming it, so expect competition; ask to be assigned first. Difficulty: straightforward.
6. **fastmcp #6491**: proxy fails for server-initiated input with legacy clients (batch 1). Difficulty: ambitious (new input-required protocol). Ask to be assigned before coding; fastmcp's bot closes PRs for unassigned issues.

### Rejected (44)
The common reasons: a claim comment or open PR (unclaimed), hardware or a paid cloud account, and not a bug fix (feature, tracking, refactor, question, docs). paperclip fails responds-to-issues every time. Full per-issue evidence is in the JSON below.

## Search log
- Q1: 262 results; `gh search issues --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-ai,BerriAI,letta-ai,langfuse --label...`
- Q2: 500 results; `gh search issues --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-ai,BerriAI,letta-ai,langfuse --label...`
- Q3: 0 results; `gh search issues --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-ai,BerriAI,letta-ai,langfuse --label...`
- Q4: 45 results; `gh search issues --repo pydantic/pydantic-ai,huggingface/smolagents,vercel/ai,microsoft/autogen,google/adk-python,Significant-Gravitas/AutoG...`
- Q5: 1 results; `gh search issues --repo pydantic/pydantic-ai,huggingface/smolagents,vercel/ai,microsoft/autogen,google/adk-python,Significant-Gravitas/AutoG...`
- Q6: 4 results; `gh search issues --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-ai,BerriAI,letta-ai,langfuse --label...`
- Q7: 0 results; `gh search issues --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-ai,BerriAI,letta-ai,langfuse --label...`
- Q8: 53 results; `gh search issues --repo langgenius/dify,pytorch/pytorch,aws/sagemaker-python-sdk,deepspeedai/DeepSpeed,jax-ml/jax,google-deepmind/gemma,SWE-...`
- Q9: 9 results; `gh search issues --repo langgenius/dify,pytorch/pytorch,aws/sagemaker-python-sdk,deepspeedai/DeepSpeed,jax-ml/jax,google-deepmind/gemma,SWE-...`
- Q10: 24 results; `gh search issues --repo langgenius/dify --label "🐞 bug" --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,reposito...`
- Q11: 339 results; `gh search issues --repo confident-ai/deepeval,vllm-project/vllm,ollama/ollama,qdrant/qdrant,weaviate/weaviate,milvus-io/milvus,pgvector/pgve...`
- Q12: 8 results; `gh search issues --repo confident-ai/deepeval,vllm-project/vllm,ollama/ollama,qdrant/qdrant,weaviate/weaviate,milvus-io/milvus,pgvector/pgve...`
- Q13: 1 results; `gh search issues --repo confident-ai/deepeval,vllm-project/vllm,ollama/ollama,qdrant/qdrant,weaviate/weaviate,milvus-io/milvus,pgvector/pgve...`
- Q14: 428 results; `gh search issues --repo openclaw/openclaw,n8n-io/n8n,NousResearch/hermes-agent,browser-use/browser-use,ChromeDevTools/chrome-devtools-mcp,ba...`
- Q15: 0 results; `gh search issues --repo openclaw/openclaw,n8n-io/n8n,NousResearch/hermes-agent,browser-use/browser-use,ChromeDevTools/chrome-devtools-mcp,ba...`
- Q16: 4 results; `gh search issues --repo openclaw/openclaw,n8n-io/n8n,NousResearch/hermes-agent,browser-use/browser-use,ChromeDevTools/chrome-devtools-mcp,ba...`
- Q17: 94 results; `gh search issues --repo PrefectHQ/fastmcp,sgl-project/sglang,paperclipai/paperclip,anomalyco/opencode,google-gemini/gemini-cli --label bug -...`
- Q18: 4 results; `gh search issues --repo PrefectHQ/fastmcp,sgl-project/sglang,paperclipai/paperclip,anomalyco/opencode,google-gemini/gemini-cli --label "good...`
- Q19: 1 results; `gh search issues --repo PrefectHQ/fastmcp,sgl-project/sglang,paperclipai/paperclip,anomalyco/opencode,google-gemini/gemini-cli --label "help...`
- Q20: 3 results; `gh search issues --repo google/adk-python,microsoft/agent-framework,langchain-ai/langgraph,vllm-project/vllm,langgenius/dify --label "help w...`
- Q21: 0 results; `gh search issues --repo google/adk-python,microsoft/agent-framework --label "good first issue" --language python,javascript,typescript --sta...`
- Q22: 8 results; `gh search issues --repo Project-MONAI/MONAI,MIC-DKFZ/nnUNet,maziyarpanahi/openmed --label bug --language python,javascript,typescript --stat...`
- Q23: 61 results; `gh search issues --repo Project-MONAI/MONAI,MIC-DKFZ/nnUNet,maziyarpanahi/openmed --label "good first issue" --language python,javascript,ty...`
- Q24: 259 results; `gh search issues --repo Project-MONAI/MONAI,MIC-DKFZ/nnUNet,maziyarpanahi/openmed --label "help wanted" --language python,javascript,typescr...`
- Q25: 0 results; `gh search issues "validation OR schema OR validate" --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-a...`
- Q26: 0 results; `gh search issues "validation OR schema OR validate" --repo pydantic/pydantic-ai,huggingface/smolagents,vercel/ai,google/adk-python,browser-u...`
- Q27: 0 results; `gh search issues "missing test OR add tests OR test coverage" --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llam...`
- Q28: 2 results; `gh search issues --repo microsoft/ai-agents-for-beginners --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,reposi...`
- Q29: 7 results; `gh search issues --repo TypeSafeAI/modex,TypeSafeAI/jev-harness,TypeSafeAI/typesafe-router,TypeSafeAI/typesafe-ui,TypeSafeAI/clarity-judge -...`
- Q30: 154 results; `gh search issues --owner pytorch --label bug --language python --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,r...`
- Q31: 11 results; `gh search issues --owner pytorch --label "good first issue" --language python --state open --limit 500 --no-assignee --updated ">2026-08-18"...`
- Q32: 0 results; `gh search issues --owner scipy --label "good first issue" --language python --state open --limit 500 --no-assignee --updated ">2026-08-18" -...`
- Q33: 24 results; `gh search issues --repo tensorflow/tensorflow,scikit-learn/scikit-learn,keras-team/keras,huggingface/transformers,stanfordnlp/dspy,OpenHands...`
- Q34: 50 results; `gh search issues --repo tensorflow/tensorflow,scikit-learn/scikit-learn,keras-team/keras,huggingface/transformers,stanfordnlp/dspy,OpenHands...`
- Q35: 0 results; `gh search issues --repo tensorflow/tensorflow,scikit-learn/scikit-learn,keras-team/keras,huggingface/transformers,stanfordnlp/dspy,OpenHands...`
- Q36: 85 results; `gh search issues --repo django/django,pallets/flask,fastapi/fastapi,wagtail/wagtail,scrapy/scrapy,pandas-dev/pandas,Lightning-AI/pytorch-lig...`
- Q37: 1 results; `gh search issues --repo django/django,pallets/flask,fastapi/fastapi,wagtail/wagtail,scrapy/scrapy,pandas-dev/pandas,Lightning-AI/pytorch-lig...`
- Q38: 63 results; `gh search issues --owner apache --label "good first issue" --language javascript,python,typescript,css,html,java,scss,dockerfile --state ope...`
- Q39: 105 results; `gh search issues --owner apache --label "help wanted" --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --l...`
- Q40: 500 results; `gh search issues --owner apache --label bug --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit 500 -...`
- Q41: 12 results; `gh search issues --owner pytorch --label "good first issue" --language javascript,python,typescript,css,html,java,scss,dockerfile --state op...`
- Q42: 2 results; `gh search issues --owner pytorch --label "help wanted" --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --...`
- Q43: 154 results; `gh search issues --owner pytorch --label bug --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit 500 ...`
- Q44: 11 results; `gh search issues --owner opensearch-project --label "good first issue" --language javascript,python,typescript,css,html,java,scss,dockerfile...`
- Q45: 1 results; `gh search issues --owner opensearch-project --label "help wanted" --language javascript,python,typescript,css,html,java,scss,dockerfile --st...`
- Q46: 174 results; `gh search issues --owner opensearch-project --label bug --language javascript,python,typescript,css,html,java,scss,dockerfile --state open -...`
- Q47: 11 results; `gh search issues --owner open-telemetry --label "good first issue" --language javascript,python,typescript,css,html,java,scss,dockerfile --s...`
- Q48: 23 results; `gh search issues --owner open-telemetry --label "help wanted" --language javascript,python,typescript,css,html,java,scss,dockerfile --state ...`
- Q49: 42 results; `gh search issues --owner open-telemetry --label bug --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --lim...`
- Q50: 87 results; `gh search issues --owner backstage,openedx,hackforla --label "good first issue" --language javascript,python,typescript,css,html,java,scss,d...`
- Q51: 179 results; `gh search issues --owner backstage,openedx,hackforla --label "help wanted" --language javascript,python,typescript,css,html,java,scss,docker...`
- Q52: 33 results; `gh search issues --owner backstage,openedx,hackforla --label bug --language javascript,python,typescript,css,html,java,scss,dockerfile --sta...`
- Q53: 500 results; `gh search issues --repo encode/httpx,python/mypy,pypa/hatch,saltstack/salt,internetarchive/openlibrary,wger-project/wger,zulip/zulip,frappe/...`
- Q54: 2 results; `gh search issues --repo encode/httpx,python/mypy,pypa/hatch,saltstack/salt,internetarchive/openlibrary,wger-project/wger,zulip/zulip,frappe/...`
- Q55: 14 results; `gh search issues --repo fastapi/fastapi,scrapy/scrapy,encode/httpx,psf/requests,scikit-learn/scikit-learn,pandas-dev/pandas,Lightning-AI/pyt...`
- Q56: 4 results; `gh search issues --repo fastapi/fastapi,scrapy/scrapy,encode/httpx,psf/requests,scikit-learn/scikit-learn,pandas-dev/pandas,Lightning-AI/pyt...`
- Q57: 0 results; `gh search issues --repo fastapi/fastapi,scrapy/scrapy,encode/httpx,psf/requests,scikit-learn/scikit-learn,pandas-dev/pandas,Lightning-AI/pyt...`
- Q58: 0 results; `gh search issues --repo fastapi/fastapi,scrapy/scrapy,encode/httpx,psf/requests,scikit-learn/scikit-learn,pandas-dev/pandas,Lightning-AI/pyt...`
- Q59: 0 results; `gh search issues --repo fastapi/fastapi,scrapy/scrapy,encode/httpx,psf/requests,scikit-learn/scikit-learn,pandas-dev/pandas,Lightning-AI/pyt...`
- Q60: 0 results; `gh search issues --repo fastapi/fastapi,scrapy/scrapy,encode/httpx,psf/requests,scikit-learn/scikit-learn,pandas-dev/pandas,Lightning-AI/pyt...`
- Q61: 7 results; `gh search issues --repo fastapi/fastapi,scrapy/scrapy,encode/httpx,psf/requests,scikit-learn/scikit-learn,pandas-dev/pandas,Lightning-AI/pyt...`

## Friction
- The label filter does not know `type/feature`, `maintainer only` or `workstream-rollup`; hermes-agent #156302 and #155769 and gemini-cli #21418 and #18280 reached grading as features. Add those labels to the drop.
- paperclip always fails responds-to-issues: its committers are its CONTRIBUTORs, so outsider merges count 0 and no committer replies are found. Consider a rubric note, or drop the repo from the priority list.
- Batches 2 and 3 mostly repeat issues rejected in scan 11, because the round-robin ranks each priority repo's best issue first. A cross-scan reject list would save grading slots.
- Five of the six accepts come from only three repos (openmed, fastmcp, chrome-devtools-mcp); the owner-filed openmed issues carry acceptance criteria and pytest commands, which is why they grade cleanly.

## Graded results
```json
[
 {
  "item": "https://github.com/maziyarpanahi/openmed/issues/5296",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "owner merged outside PRs (Mr45401, Shellghostt, 2026-11-08); 25 outsider PRs merged in 90 days"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee, 0 comments, no linked PR; maintainer-opened clause exempt (good first issue/help wanted)"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "one fixture set plus integrity test with acceptance criteria (test gap)"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one bounded task; files and pytest command named"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "pass",
    "evidence": "labels: good first issue, help wanted"
   },
   {
    "name": "has-repro",
    "grade": "fail",
    "evidence": "not a bug report; acceptance criteria instead"
   },
   {
    "name": "shipped-recently",
    "grade": "pass",
    "evidence": "release 2026-11-13"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "pass",
    "evidence": "25 of 28 outsider PRs merged in 90 days"
   }
  ],
  "verdict": "accept"
 },
 {
  "item": "https://github.com/sgl-project/sglang/issues/27214",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "open PR #27442; eight claim/interest comments, no maintainer answer"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "running list of new diffusion models to support"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "container list"
   },
   {
    "name": "no-special-hardware",
    "grade": "fail",
    "evidence": "diffusion model support needs GPUs"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/langgenius/dify/issues/45584",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "open PR #45587 and many \"taking <file>\" claims from 2026-11-04 to 11-10"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "[Chore/Refactor] use sqlalchemy engine"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "multi-file refactor already split among claimants"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/vllm-project/vllm/issues/49316",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "open linked PR #49318"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "fail",
    "evidence": "collect_env shows CUDA 13 PyTorch; vLLM parser needs GPU stack"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/openclaw/openclaw/issues/193979",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "unclear",
    "evidence": "P0, needs-maintainer-review, no verified fix"
   },
   {
    "name": "no-special-hardware",
    "grade": "fail",
    "evidence": "needs a ChatGPT account and OAuth against the hosted mcp.openclaw.ai relay"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/paperclipai/paperclip/issues/9153",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "fail",
    "evidence": "no committer replies in sampled issues; 0 outsider PRs merged by a maintainer in 90 days"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "fail",
    "evidence": "\"just try to create a new skill\""
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/langgenius/dify/issues/44722",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "GYJ99 claimed 2026-11-04, no maintainer reply (12 days)"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "fail",
    "evidence": "reproduced on Dify Cloud with a hosted deepseek model"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/langchain4j/langchain4j/issues/7122",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "comment 2026-11-15: fixed on main by #6942 (merged 2026-11-02)"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/PrefectHQ/fastmcp/issues/6491",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "zzstoatzz (Collaborator, committed 2026-11-15) replied on #4920 (2026-08-31) and #5236 (2026-09-23)"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee, no linked PR; the one comment confirms the repro and does not claim it"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "proxy raises MCPError -32602 / \"Elicitation not supported\" for legacy clients"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one proxy area, two sub-cases; open design edge only raises difficulty"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "fail",
    "evidence": "labels: bug, server"
   },
   {
    "name": "has-repro",
    "grade": "pass",
    "evidence": "error text, results table and in-memory repro (script placeholder unfilled)"
   },
   {
    "name": "shipped-recently",
    "grade": "pass",
    "evidence": "release 2026-11-13"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "pass",
    "evidence": "5 of 30 outsider PRs merged in 90 days"
   }
  ],
  "verdict": "accept"
 },
 {
  "item": "https://github.com/langchain-ai/langgraph/issues/9676",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "Kylinny: \"I'd like to take this on\" 2026-11-12, no maintainer reply"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/maziyarpanahi/openmed/issues/4814",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "architecture migration of two Kotlin engines"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "multi-file migration plus migration guide, allowlist, CHANGELOG"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "fail",
    "evidence": "Android Kotlin toolchain"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/Project-MONAI/MONAI/issues/8489",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "fail",
    "evidence": "segfault seen on Linux + A100 CI node; not reproducible on CPU"
   },
   {
    "name": "macos-compatible",
    "grade": "fail",
    "evidence": "reproduced only on Linux"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/sgl-project/sglang/issues/37524",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "\"GLM-5.3-Flash bug tracking\" checklist"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "umbrella issue"
   },
   {
    "name": "no-special-hardware",
    "grade": "fail",
    "evidence": "GLM serving needs GPUs"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/browser-use/browser-use/issues/4803",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "titled \"Question\"; Chrome 137+ ignores --load-extension (expected)"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "fail",
    "evidence": "0 of 41 outsider PRs merged"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/bytedance/deer-flow/issues/6751",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "wycocoyu asked to be assigned 2026-11-10 and has an open fork PR"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "unclear",
    "evidence": "traceback shows Windows paths"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/spring-projects/spring-ai/issues/4809",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "prathap2k offered a fix 2026-11-11 (\"happy to open a PR\"), no maintainer reply"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/anomalyco/opencode/issues/36347",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "prototype reported by gcomneno 2026-09-24"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "\"We need a durable model\" for waits: open design"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/NousResearch/hermes-agent/issues/156302",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "labelled type/feature: add max_retries to the kanban_create schema"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/google-gemini/gemini-cli/issues/21418",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "feature request to design \"deep validation\""
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "maintainer only / workstream-rollup"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/microsoft/agent-framework/issues/962",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "fabbrik: \"I'd like to take this\" 2026-11-14"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "fail",
    "evidence": "asks for setup instructions for integration tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/3422",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "vulnerable bundled dependency; dependabot fast-uri bumps already merged repeatedly"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/vllm-project/vllm/issues/63465",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "fail",
    "evidence": "environment: \"Validation hardware: NVIDIA H200\""
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/openclaw/openclaw/issues/127553",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "unclear",
    "evidence": "retained inventory is by design per the review"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "clawsweeper: no high-confidence reproduction; maintainer-protected, owner disposition pending"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/paperclipai/paperclip/issues/12296",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "fail",
    "evidence": "no committer replies; 0 outsider PRs merged by a maintainer"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "auth migration misses the account issuer column"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/PrefectHQ/fastmcp/issues/6480",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "its-amann: \"i can take this\" 2026-11-13"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/langchain-ai/langgraph/issues/9655",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "umbrella: 14 open bugs awaiting triage"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "container issue"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/maziyarpanahi/openmed/issues/5121",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "owner merged 25 outsider PRs in 90 days (patched alternative)"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee, 0 comments, no linked PR (PR #5501 touches streamed arguments only); no sibling PR from the owner found for this issue"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "redaction rewrites tool-call names and ids and can break JSON arguments"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one module (privacy_proxy/app.py) with acceptance criteria and a pytest command; size M"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "fail",
    "evidence": "labels: bug, P1, roadmap-v5"
   },
   {
    "name": "has-repro",
    "grade": "fail",
    "evidence": "acceptance criteria instead of a repro"
   },
   {
    "name": "shipped-recently",
    "grade": "pass",
    "evidence": "release 2026-11-13"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "pass",
    "evidence": "25 of 28 outsider PRs merged in 90 days"
   }
  ],
  "verdict": "accept"
 },
 {
  "item": "https://github.com/langgenius/dify/issues/44503",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "fail",
    "evidence": "load test on a Cloud staging build with a dedicated tenant"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/langchain4j/langchain4j/issues/6858",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "comment 2026-11-15: all tests pass on current main; already fixed by #6726"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/bytedance/deer-flow/issues/4138",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "comments say mechanisms were already addressed by merged PRs; labelled duplicate"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/browser-use/browser-use/issues/5747",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "KhushiKeswani: \"I'd like to contribute\" 2026-11-05"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "maintainer could not reproduce the slowdown"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/spring-projects/spring-ai/issues/1165",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "enhancement: Azure Speech SDK support"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/NousResearch/hermes-agent/issues/155769",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "labelled type/feature: config_schema enum/choices key"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/google-gemini/gemini-cli/issues/18280",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "maintainer-only feature: complex subagent schema"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/anomalyco/opencode/issues/62585",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "fail",
    "evidence": "needs the OpenCode Mobile iOS app and a paired iPhone; maintainer could not reproduce"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/microsoft/agent-framework/issues/9946",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "sudarshan24-10 is working on a typing cleanup with a maintainer reviewing"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "bot-opened dependency range validation failure (maintenance)"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/Project-MONAI/MONAI/issues/9272",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "open linked PR #9273"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "fail",
    "evidence": "missing entries in docs/source/losses.rst"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/3412",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "OrKoN (Collaborator) replied on #1794, #3470, #3038; 10 outsider PRs merged by maintainers in 90 days"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee, 0 comments; sibling PR #3030 covers className only"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "invalid propertyName regex returns an internal TypeError instead of a clear validation error"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one tool, one filter; mirrors the className fix in #3030"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no AI rule found; Google CLA required"
   },
   {
    "name": "good-to-start",
    "grade": "fail",
    "evidence": "labels: confirmed, p3"
   },
   {
    "name": "has-repro",
    "grade": "pass",
    "evidence": "exact tool call, three invalid patterns, controls"
   },
   {
    "name": "shipped-recently",
    "grade": "pass",
    "evidence": "release 2026-11-11"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "pass",
    "evidence": "27 of 37 outsider PRs merged in 90 days"
   }
  ],
  "verdict": "accept"
 },
 {
  "item": "https://github.com/openclaw/openclaw/issues/127576",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "fail",
    "evidence": "clawsweeper: needs-product-decision, no-new-fix-pr"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/vllm-project/vllm/issues/57997",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "open linked PR #58023; vikrambal claimed 2026-09-21"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "fail",
    "evidence": "environment is a GPU host"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/paperclipai/paperclip/issues/21797",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "fail",
    "evidence": "no committer replies; 0 outsider PRs merged by a maintainer"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/maziyarpanahi/openmed/issues/5097",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "owner merged 25 outsider PRs in 90 days (patched alternative)"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee, 0 comments, no linked PR (issue #5169 is a sibling issue)"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "privacy proxy redacts image/audio/file parts as text; asks for refusal with 400 (input validation)"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one module plus a docs line and tests; acceptance criteria and pytest command given"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "fail",
    "evidence": "labels: bug, P0, roadmap-v5"
   },
   {
    "name": "has-repro",
    "grade": "fail",
    "evidence": "acceptance criteria instead of a repro"
   },
   {
    "name": "shipped-recently",
    "grade": "pass",
    "evidence": "release 2026-11-13"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "pass",
    "evidence": "25 of 28 outsider PRs merged in 90 days"
   }
  ],
  "verdict": "accept"
 },
 {
  "item": "https://github.com/langgenius/dify/issues/44238",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "fail",
    "evidence": "Dify Cloud Agent Builder with plugin daemon"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/PrefectHQ/fastmcp/issues/6398",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "zzstoatzz replied on #4920 and #5236"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee, no linked PR; mohitdixit02 posted a verification and proposed validator on 2026-11-10 but did not claim it; expect competition"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "ListSkillsResult accepts duplicate skill URIs (missing validation)"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one model validator plus a test"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "fail",
    "evidence": "labels: bug, client"
   },
   {
    "name": "has-repro",
    "grade": "pass",
    "evidence": "runnable repro in the issue"
   },
   {
    "name": "shipped-recently",
    "grade": "pass",
    "evidence": "release 2026-11-13"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "pass",
    "evidence": "5 of 30 outsider PRs merged in 90 days"
   }
  ],
  "verdict": "accept"
 },
 {
  "item": "https://github.com/langchain-ai/langgraph/issues/9645",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "sakshiagra1903 offered a fix 2026-11-09, no maintainer reply"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/langchain4j/langchain4j/issues/6899",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "open linked PR #6902"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/bytedance/deer-flow/issues/1199",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "a question; later comments say the features landed"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU/accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/NousResearch/hermes-agent/issues/155671",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "scope-feasible",
    "grade": "unclear",
    "evidence": "tracks a cache cost tradeoff of an earlier policy"
   },
   {
    "name": "no-special-hardware",
    "grade": "fail",
    "evidence": "evidence needs Azure Foundry with a GPT model (paid cloud account)"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/sgl-project/sglang/issues/48822",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "open linked PRs #49289 and #50295"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "[RFC] add NPU RLHF lifecycle tests"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "fail",
    "evidence": "needs NPU accelerators"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/spring-projects/spring-ai/issues/4422",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "recent default-branch commits (Nov 13-16), repo not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "fail",
    "evidence": "asks how to generate a strict-mode schema (usage question)"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "no sign of failure in the evidence read"
   },
   {
    "name": "no-special-hardware",
    "grade": "fail",
    "evidence": "Azure OpenAI deployment (paid cloud account)"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named, or reproduces cross-platform"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no ban in contribution policy; silence or disclosure only"
   },
   {
    "name": "good-to-start",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-repro",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "shipped-recently",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "unclear",
    "evidence": "not graded: preferred checks only rank accepted issues"
   }
  ],
  "verdict": "reject"
 }
]
```


---

# 2026-11-15 direct grading of 6 picks (rubric patched; openmed owner-PR check added)

## Ranked read-out
Accepted, in fit order (profile: small isolated bug fixes, schema validation tweaks, test gaps; python/js/ts; active agentic-AI repos):
1. **chrome-devtools-mcp #3412** (direct): invalid `propertyName` regex returns an internal TypeError. Maintainer-confirmed, 0 comments, no linked PR; TypeScript, one tool. Needs the Google CLA. Difficulty: straightforward.
2. **fastmcp #6398** (direct): `ListSkillsResult` accepts duplicate skill URIs; one validator plus a test. Another contributor posted a proposed validator on 2026-11-10 without claiming it, so ask to be assigned quickly. Difficulty: straightforward.
3. **openmed #5296** (direct): synthetic free-text quasi-identifier fixture set. Passes the rubric, with a tension: the owner opened 82 PRs on 2026-11-15/16 for sibling roadmap issues, including 19 help-wanted and 2 good-first-issue ones (#5422, #5448), and none for #5296 yet. Claim it immediately and confirm no PR exists first. Difficulty: straightforward.
4. **fastmcp #6491** (direct): proxy fails for server-initiated input with legacy clients. Difficulty: ambitious (new input-required protocol); raise the approach with a maintainer first, and ask to be assigned before coding.

Rejected:
- **openmed #5121** and **openmed #5097**: `unclaimed`. Both are owner-filed with only bug/priority labels (no invite label), and the owner opened 82 PRs in the last two days for issues filed in the same 2026-11-07 batch. This reverses my scan 12 accepts, which missed those PRs.

## Graded results
```json
[
 {
  "item": "https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/3412",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits 2026-11-13; not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "OrKoN (Collaborator) replied on #1794, #3470, #3038 within 90 days; 10 outsider PRs merged by maintainers"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee, 0 comments, no linked PR (sibling PR #3030 covers className only)"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "invalid propertyName regex surfaces an internal TypeError instead of a validation error"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one tool and one filter; #3030 shows the pattern"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU, accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named; Python/Node work runs on macOS"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no AI ban found in CONTRIBUTING.md or AGENTS.md; silence passes"
   },
   {
    "name": "good-to-start",
    "grade": "fail",
    "evidence": "labels: confirmed, p3"
   },
   {
    "name": "has-repro",
    "grade": "pass",
    "evidence": "exact tool call, three invalid patterns, controls"
   },
   {
    "name": "shipped-recently",
    "grade": "pass",
    "evidence": "release 2026-11-11"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "pass",
    "evidence": "27 of 37 outsider PRs merged in 90 days"
   }
  ],
  "verdict": "accept"
 },
 {
  "item": "https://github.com/PrefectHQ/fastmcp/issues/6398",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits 2026-11-13 to 11-15"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "zzstoatzz (Collaborator) replied on #4920 (2026-08-31) and #5236 (2026-09-23)"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee, no linked PR; mohitdixit02 posted a verification and proposed validator on 2026-11-10 but did not claim it"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "ListSkillsResult accepts duplicate skill URIs (missing validation)"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one model validator plus a test"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU, accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named; Python/Node work runs on macOS"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no AI ban found in CONTRIBUTING.md or AGENTS.md; silence passes"
   },
   {
    "name": "good-to-start",
    "grade": "fail",
    "evidence": "labels: bug, client"
   },
   {
    "name": "has-repro",
    "grade": "pass",
    "evidence": "runnable repro in the issue"
   },
   {
    "name": "shipped-recently",
    "grade": "pass",
    "evidence": "release 2026-11-13"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "pass",
    "evidence": "5 of 31 outsider PRs merged in 90 days"
   }
  ],
  "verdict": "accept"
 },
 {
  "item": "https://github.com/maziyarpanahi/openmed/issues/5296",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits 2026-11-15; not archived"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "owner merged outsider PRs (Mr45401, Shellghostt, 2026-11-08); 25 outsider PRs merged in 90 days (patched alternative)"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee, 0 comments, no linked PR; maintainer-opened clause exempt (good first issue and help wanted). Tension: the owner opened 82 PRs on 2026-11-15/16 for sibling roadmap issues, including 19 help-wanted and 2 good-first-issue ones (#5422, #5448)"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "one self-contained fixture set plus integrity test with acceptance criteria (test gap)"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one bounded task; files and pytest command named"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU, accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named; Python/Node work runs on macOS"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no AI ban found in CONTRIBUTING.md or AGENTS.md; silence passes"
   },
   {
    "name": "good-to-start",
    "grade": "pass",
    "evidence": "labels: good first issue, help wanted"
   },
   {
    "name": "has-repro",
    "grade": "fail",
    "evidence": "not a bug report; acceptance criteria instead"
   },
   {
    "name": "shipped-recently",
    "grade": "pass",
    "evidence": "release 2026-11-13"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "pass",
    "evidence": "25 of 28 outsider PRs merged in 90 days"
   }
  ],
  "verdict": "accept"
 },
 {
  "item": "https://github.com/PrefectHQ/fastmcp/issues/6491",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits 2026-11-13 to 11-15"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "zzstoatzz replied on #4920 and #5236"
   },
   {
    "name": "unclaimed",
    "grade": "pass",
    "evidence": "open, no assignee, no linked PR; the one comment confirms the repro and does not claim it"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "proxy raises MCPError -32602 / \"Elicitation not supported\" for legacy clients"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one proxy area, two sub-cases; open design edge only raises difficulty"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU, accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named; Python/Node work runs on macOS"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no AI ban found in CONTRIBUTING.md or AGENTS.md; silence passes"
   },
   {
    "name": "good-to-start",
    "grade": "fail",
    "evidence": "labels: bug, server"
   },
   {
    "name": "has-repro",
    "grade": "pass",
    "evidence": "error text, results table, in-memory repro (script placeholder unfilled)"
   },
   {
    "name": "shipped-recently",
    "grade": "pass",
    "evidence": "release 2026-11-13"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "pass",
    "evidence": "5 of 31 outsider PRs merged in 90 days"
   }
  ],
  "verdict": "accept"
 },
 {
  "item": "https://github.com/maziyarpanahi/openmed/issues/5121",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits 2026-11-15"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "25 outsider PRs merged by the owner in 90 days (patched alternative)"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "maintainer-opened (owner) with no invite label (bug, P1, roadmap-v5); the owner opened 82 PRs on 2026-11-15/16 closing sibling roadmap issues filed the same day (2026-11-07), so the maintainer-opened clause applies"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "redaction rewrites tool-call names and ids and can break JSON arguments"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one module with acceptance criteria and a pytest command"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU, accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named; Python/Node work runs on macOS"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no AI ban found in CONTRIBUTING.md or AGENTS.md; silence passes"
   },
   {
    "name": "good-to-start",
    "grade": "fail",
    "evidence": "labels: bug, P1, roadmap-v5"
   },
   {
    "name": "has-repro",
    "grade": "fail",
    "evidence": "acceptance criteria instead of a repro"
   },
   {
    "name": "shipped-recently",
    "grade": "pass",
    "evidence": "release 2026-11-13"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "pass",
    "evidence": "25 of 28 outsider PRs merged in 90 days"
   }
  ],
  "verdict": "reject"
 },
 {
  "item": "https://github.com/maziyarpanahi/openmed/issues/5097",
  "checks": [
   {
    "name": "maintainer-alive",
    "grade": "pass",
    "evidence": "last 5 commits 2026-11-15"
   },
   {
    "name": "responds-to-issues",
    "grade": "pass",
    "evidence": "25 outsider PRs merged by the owner in 90 days (patched alternative)"
   },
   {
    "name": "unclaimed",
    "grade": "fail",
    "evidence": "maintainer-opened (owner) with no invite label (bug, P0, roadmap-v5); the owner opened 82 PRs on 2026-11-15/16 closing sibling roadmap issues filed the same day (2026-11-07), so the maintainer-opened clause applies"
   },
   {
    "name": "is-a-bug-fix",
    "grade": "pass",
    "evidence": "privacy proxy redacts image/audio/file parts as text; asks for a 400 refusal"
   },
   {
    "name": "scope-feasible",
    "grade": "pass",
    "evidence": "one module with acceptance criteria and a pytest command"
   },
   {
    "name": "no-special-hardware",
    "grade": "pass",
    "evidence": "no GPU, accelerator or paid account named"
   },
   {
    "name": "macos-compatible",
    "grade": "pass",
    "evidence": "no platform named; Python/Node work runs on macOS"
   },
   {
    "name": "not-docs-only",
    "grade": "pass",
    "evidence": "fix is in code or tests"
   },
   {
    "name": "ai-contribution-allowed",
    "grade": "pass",
    "evidence": "no AI ban found in CONTRIBUTING.md or AGENTS.md; silence passes"
   },
   {
    "name": "good-to-start",
    "grade": "fail",
    "evidence": "labels: bug, P0, roadmap-v5"
   },
   {
    "name": "has-repro",
    "grade": "fail",
    "evidence": "acceptance criteria instead of a repro"
   },
   {
    "name": "shipped-recently",
    "grade": "pass",
    "evidence": "release 2026-11-13"
   },
   {
    "name": "has-contributing-guide",
    "grade": "pass",
    "evidence": "CONTRIBUTING.md present"
   },
   {
    "name": "merges-newcomer-prs",
    "grade": "pass",
    "evidence": "25 of 28 outsider PRs merged in 90 days"
   }
  ],
  "verdict": "reject"
 }
]
```
