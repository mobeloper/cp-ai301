# Search strategy: where your scout looks, and why

<!--
THIS IS THE PART YOU WRITE. The scout in SKILL.md executes whatever
queries and filters you define here. It ships empty on purpose: the
strategy is your judgment about where YOUR first contribution lives,
and a scan without authored queries is just someone else's defaults.

A filled strategy must contain:

1. At least one query in the Queries section. Each query is a real
   `gh search issues` invocation, and each should trace to a sentence
   of your fit profile in scope.md: if you cannot say which sentence
   produced a query, the query is probably not yours. Search breadth
   is the point at this stage: prefer generous limits and let the
   ranking filters do the narrowing.

2. At least one ranking filter in the Ranking filters section. A
   filter is a condition or an ordering over fields the search
   results already carry (last activity date, assignee presence,
   label set, comment count, repo). It must be checkable from the
   result fields alone: if deciding it would need opening the issue
   page, it belongs in your rubric, not here. Ranking is the free
   stage; keep it free. Make at least one filter an ordering, or
   state your tie-break: with drop conditions alone, the scout takes
   the top of whatever order the searches happened to return, and
   the graded candidates stop being traceable to you.

3. The top-N line, kept or deliberately changed.
-->

## Queries

<!-- Real gh search invocations, one per line, each with a comment
naming the fit-profile sentence it came from. Example SHAPE (write
your own; these values belong to nobody):

    [EXAMPLE ONLY, NOT A QUERY] gh search issues --label "good first issue" --language python --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url
-->

# Profile: "I write python... Avoid java or other languages" + "reputable agentic AI tools and libraries" + "small bug fixes" (reputable orgs, python bugs)
gh search issues --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-ai,BerriAI,letta-ai,langfuse --label bug --language python --state open --limit 100 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,updatedAt,url -- -linked:pr

# Profile: "javascript, and typescript" + "reputable agentic AI tools and libraries" + "Both frontend and backend issues" + "small bug fixes" (reputable orgs, typescript bugs)
gh search issues --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-ai,BerriAI,letta-ai,langfuse --label bug --language typescript --state open --limit 100 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,updatedAt,url -- -linked:pr

# Profile: "javascript" + "reputable agentic AI tools and libraries" + "small bug fixes" (reputable orgs, javascript bugs)
gh search issues --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-ai,BerriAI,letta-ai,langfuse --label bug --language javascript --state open --limit 100 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,updatedAt,url -- -linked:pr

# Profile: "python" + "reputable agentic AI tools and libraries" + "small bug fixes" (named repos from orgs too broad to scope by owner, python bugs)
gh search issues --repo pydantic/pydantic-ai,huggingface/smolagents,vercel/ai,microsoft/autogen,google/adk-python,Significant-Gravitas/AutoGPT,microsoft/agent-framework --label bug --language python --state open --limit 100 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,updatedAt,url -- -linked:pr

# Profile: "typescript" + "reputable agentic AI tools and libraries" + "frontend and backend" + "small bug fixes" (named repos, typescript bugs)
gh search issues --repo pydantic/pydantic-ai,huggingface/smolagents,vercel/ai,microsoft/autogen,google/adk-python,Significant-Gravitas/AutoGPT,microsoft/agent-framework --label bug --language typescript --state open --limit 100 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,updatedAt,url -- -linked:pr

# Profile: "python" + "easy difficulty" (good-first-issue labelled, reputable orgs; rare but best signal)
gh search issues --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-ai,BerriAI,letta-ai,langfuse --label "good first issue" --language python --state open --limit 100 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,updatedAt,url -- -linked:pr

# Profile: "typescript" + "easy difficulty" (good-first-issue labelled, reputable orgs)
gh search issues --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-ai,BerriAI,letta-ai,langfuse --label "good first issue" --language typescript --state open --limit 100 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,updatedAt,url -- -linked:pr

# Profile: "python" + "reputable agentic AI tools and libraries" + "small bug fixes" (added repos: pytorch, DeepSpeed, jax, gemma, SWE-bench, OSWorld, sagemaker, dify; renamed repos use their current names)
gh search issues --repo langgenius/dify,pytorch/pytorch,aws/sagemaker-python-sdk,deepspeedai/DeepSpeed,jax-ml/jax,google-deepmind/gemma,SWE-bench/SWE-bench,xlang-ai/OSWorld --label bug --language python --state open --limit 100 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,updatedAt,url -- -linked:pr

# Profile: "python" + "easy difficulty" (good-first-issue in the added repos; pytorch and jax use this label heavily)
gh search issues --repo langgenius/dify,pytorch/pytorch,aws/sagemaker-python-sdk,deepspeedai/DeepSpeed,jax-ml/jax,google-deepmind/gemma,SWE-bench/SWE-bench,xlang-ai/OSWorld --label "good first issue" --language python --state open --limit 100 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,updatedAt,url -- -linked:pr

# Profile: "python" + "Both frontend and backend issues" + "small bug fixes" (dify labels bugs "🐞 bug", not "bug"; no language flag because its frontend is TypeScript)
gh search issues --repo langgenius/dify --label "🐞 bug" --state open --limit 100 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,updatedAt,url -- -linked:pr

# Profile: "reputable agentic AI tools and libraries" + "small bug fixes" (ai-agents-for-beginners is a Jupyter repo, so a language flag would drop it, and it rarely uses a bug label; the rubric checks language and size)
gh search issues --repo microsoft/ai-agents-for-beginners --state open --limit 100 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,updatedAt,url -- -linked:pr

# Profile: "typescript" + "Both frontend and backend issues" + "small bug fixes" (TypeSafeAI repos are small and mostly unlabelled, so no label flag)
gh search issues --repo TypeSafeAI/modex,TypeSafeAI/jev-harness,TypeSafeAI/typesafe-router,TypeSafeAI/typesafe-ui,TypeSafeAI/clarity-judge --state open --limit 100 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,updatedAt,url -- -linked:pr

# Profile: "python" + "small bug fixes" (every repo under the pytorch org: tensordict, vision, executorch, rl, TensorRT, ao, examples, etc.)
gh search issues --owner pytorch --label bug --language python --state open --limit 100 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,updatedAt,url -- -linked:pr

# Profile: "python" + "easy difficulty" (good-first-issue across the pytorch org)
gh search issues --owner pytorch --label "good first issue" --language python --state open --limit 100 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,updatedAt,url -- -linked:pr

# Profile: "scipy" + "easy difficulty" (good-first-issue across the pytorch org)
gh search issues --owner scipy --label "good first issue" --language python --state open --limit 100 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,updatedAt,url -- -linked:pr



## Ranking filters

<!-- Conditions and orderings over the free result fields, applied in
the order written. State each so a stranger could apply it and get
your ordering. Example SHAPES (write your own):

- drop any result with an assignee
- drop any result not updated in the last 60 days
- prefer fewer comments over more
-->

- drop any result with an assignee (the queries already pass `--no-assignee`; recheck the field)
- drop any result not updated in the last 90 days (supports "an active repo project")
- for results from queries that carry an `--owner` or `--repo` flag only (the all-owners queries are exempt), drop any result whose `repository.nameWithOwner` is not under an owner named in an `--owner` flag (langchain-ai, anthropics, openai, modelcontextprotocol, crewAIInc, run-llama, mastra-ai, BerriAI, letta-ai, langfuse, pytorch) or equal to a repo named in a `--repo` flag (guards against off-target leaks from any query)
- drop any result from `anthropics/claude-code` (its repo is an issue tracker only, so there is no source code to fix)
- drop any result with more than 15 comments (long threads are rarely "small" or "easy")
- drop any result whose title starts with `Feature` or `[Feature` (unlabelled repos still mark features in titles);
- drop any result whose labels include `question`, `documentation`, `feature`, `feature request` or `discussion` (profile: "not a new feature")
- drop any result whose labels include `topic: documentation`, `documentation`, `docs`, `module: docs` or any label containing `doc` (docs-only work is not the code bug fix in the profile)
- order what remains: results labelled `bug` or `🐞 bug` first, then results labelled `good first issue`, then fewest comments first, then most recently updated first (a `good first issue` label without a bug label is often a stale, claimed or non-bug issue)
- tie-break: if still tied, prefer the repo with the most total results across all queries (a busy repo is more likely to answer questions)


## How many to grade

Grade the top **5** ranked results per scan.

<!-- Five is the default because grading is the only stage that costs
money: about $0.20 per candidate, so a scan is about $1, and you can
iterate the strategy several times for less than one full regression
run. You need one good issue, not ten. Raising this number is
allowed, but it is a decision you state here in a sentence, with the
cost you are accepting; it is not a number you bump because a scan
disappointed you once. -->
