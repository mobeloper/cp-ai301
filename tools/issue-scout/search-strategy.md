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

    [EXAMPLE ONLY, NOT A QUERY] gh search issues --label "good first issue" --language python --state open --limit 500 --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url
-->

# Profile: "I write python... Avoid java or other languages" + "reputable agentic AI tools and libraries" + "small bug fixes" (reputable orgs, python bugs)
gh search issues --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-ai,BerriAI,letta-ai,langfuse --label bug --language python --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "javascript, and typescript" + "reputable agentic AI tools and libraries" + "Both frontend and backend issues" + "small bug fixes" (reputable orgs, typescript bugs)
gh search issues --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-ai,BerriAI,letta-ai,langfuse --label bug --language typescript --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "javascript" + "reputable agentic AI tools and libraries" + "small bug fixes" (reputable orgs, javascript bugs)
gh search issues --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-ai,BerriAI,letta-ai,langfuse --label bug --language javascript --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "python" + "reputable agentic AI tools and libraries" + "small bug fixes" (named repos from orgs too broad to scope by owner, python bugs)
gh search issues --repo pydantic/pydantic-ai,huggingface/smolagents,vercel/ai,microsoft/autogen,google/adk-python,Significant-Gravitas/AutoGPT,microsoft/agent-framework --label bug --language python --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "typescript" + "reputable agentic AI tools and libraries" + "frontend and backend" + "small bug fixes" (named repos, typescript bugs)
gh search issues --repo pydantic/pydantic-ai,huggingface/smolagents,vercel/ai,microsoft/autogen,google/adk-python,Significant-Gravitas/AutoGPT,microsoft/agent-framework --label bug --language typescript --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "python" + "easy difficulty" (good-first-issue labelled, reputable orgs; rare but best signal)
gh search issues --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-ai,BerriAI,letta-ai,langfuse --label "good first issue" --language python --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "typescript" + "easy difficulty" (good-first-issue labelled, reputable orgs)
gh search issues --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-ai,BerriAI,letta-ai,langfuse --label "good first issue" --language typescript --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "python" + "reputable agentic AI tools and libraries" + "small bug fixes" (added repos: pytorch, DeepSpeed, jax, gemma, SWE-bench, OSWorld, sagemaker, dify; renamed repos use their current names)
gh search issues --repo langgenius/dify,pytorch/pytorch,aws/sagemaker-python-sdk,deepspeedai/DeepSpeed,jax-ml/jax,google-deepmind/gemma,SWE-bench/SWE-bench,xlang-ai/OSWorld --label bug --language python --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "python" + "easy difficulty" (good-first-issue in the added repos; pytorch and jax use this label heavily)
gh search issues --repo langgenius/dify,pytorch/pytorch,aws/sagemaker-python-sdk,deepspeedai/DeepSpeed,jax-ml/jax,google-deepmind/gemma,SWE-bench/SWE-bench,xlang-ai/OSWorld --label "good first issue" --language python --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "python" + "Both frontend and backend issues" + "small bug fixes" (dify labels bugs "🐞 bug", not "bug"; no language flag because its frontend is TypeScript)
gh search issues --repo langgenius/dify --label "🐞 bug" --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "reputable agentic AI tools and libraries" + "small bug fixes" (ai-agents-for-beginners is a Jupyter repo, so a language flag would drop it, and it rarely uses a bug label; the rubric checks language and size)
gh search issues --repo microsoft/ai-agents-for-beginners --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "typescript" + "Both frontend and backend issues" + "small bug fixes" (TypeSafeAI repos are small and mostly unlabelled, so no label flag)
gh search issues --repo TypeSafeAI/modex,TypeSafeAI/jev-harness,TypeSafeAI/typesafe-router,TypeSafeAI/typesafe-ui,TypeSafeAI/clarity-judge --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "python" + "small bug fixes" (every repo under the pytorch org: tensordict, vision, executorch, rl, TensorRT, ao, examples, etc.)
gh search issues --owner pytorch --label bug --language python --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "python" + "easy difficulty" (good-first-issue across the pytorch org)
gh search issues --owner pytorch --label "good first issue" --language python --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "scipy" + "easy difficulty" (good-first-issue across the pytorch org)
gh search issues --owner scipy --label "good first issue" --language python --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "python" + "reputable agentic AI tools and libraries" + "small bug fixes" (added repos: deepeval, vllm, ollama, qdrant, weaviate, milvus, pgvector, docling, firecrawl, haystack, openllmetry, ragas; run-llama/llama_index skipped because the run-llama owner is already searched; the language flag drops their non-python code)
gh search issues --repo confident-ai/deepeval,vllm-project/vllm,ollama/ollama,qdrant/qdrant,weaviate/weaviate,milvus-io/milvus,pgvector/pgvector,docling-project/docling,firecrawl/firecrawl,deepset-ai/haystack,traceloop/openllmetry,vibrantlabsai/ragas --label bug --language python --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "typescript" + "Both frontend and backend issues" + "small bug fixes" (same added repos; firecrawl and ollama have TypeScript code)
gh search issues --repo confident-ai/deepeval,vllm-project/vllm,ollama/ollama,qdrant/qdrant,weaviate/weaviate,milvus-io/milvus,pgvector/pgvector,docling-project/docling,firecrawl/firecrawl,deepset-ai/haystack,traceloop/openllmetry,vibrantlabsai/ragas --label bug --language typescript --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "python" + "easy difficulty" (good-first-issue in the same added repos)
gh search issues --repo confident-ai/deepeval,vllm-project/vllm,ollama/ollama,qdrant/qdrant,weaviate/weaviate,milvus-io/milvus,pgvector/pgvector,docling-project/docling,firecrawl/firecrawl,deepset-ai/haystack,traceloop/openllmetry,vibrantlabsai/ragas --label "good first issue" --language python --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "python" + "reputable agentic AI tools and libraries" + "small bug fixes" (added repos: tensorflow, scikit-learn, keras, transformers, dspy, OpenHands, opencv-python, ultralytics, spaCy, chroma, promptfoo; All-Hands-AI/OpenHands is now OpenHands/OpenHands; the rest of your list was already covered by existing owner or repo queries or the previous batch: pytorch, jax-ml/jax, vllm, ollama, langchain, langgraph, crewAI, autogen, openai-python, llama_index, letta, mastra, modelcontextprotocol, langfuse, litellm)
gh search issues --repo tensorflow/tensorflow,scikit-learn/scikit-learn,keras-team/keras,huggingface/transformers,stanfordnlp/dspy,OpenHands/OpenHands,opencv/opencv-python,ultralytics/ultralytics,explosion/spaCy,chroma-core/chroma,promptfoo/promptfoo --label bug --language python --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "typescript" + "Both frontend and backend issues" + "small bug fixes" (same added repos; OpenHands and promptfoo are mostly TypeScript)
gh search issues --repo tensorflow/tensorflow,scikit-learn/scikit-learn,keras-team/keras,huggingface/transformers,stanfordnlp/dspy,OpenHands/OpenHands,opencv/opencv-python,ultralytics/ultralytics,explosion/spaCy,chroma-core/chroma,promptfoo/promptfoo --label bug --language typescript --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "python" + "easy difficulty" (good-first-issue in the same added repos)
gh search issues --repo tensorflow/tensorflow,scikit-learn/scikit-learn,keras-team/keras,huggingface/transformers,stanfordnlp/dspy,OpenHands/OpenHands,opencv/opencv-python,ultralytics/ultralytics,explosion/spaCy,chroma-core/chroma,promptfoo/promptfoo --label "good first issue" --language python --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr


# Profile: "python" + "small bug fixes" + "an active repo project where people answer questions" (added general-purpose Python repos; scikit-learn and keras were already in the third batch, so they are skipped; the language flag drops non-python code)
gh search issues --repo django/django,pallets/flask,fastapi/fastapi,wagtail/wagtail,scrapy/scrapy,pandas-dev/pandas,Lightning-AI/pytorch-lightning,matplotlib/matplotlib,psf/black,pydantic/pydantic,python-poetry/poetry,psf/requests,Textualize/rich,TheAlgorithms/Python,ossf/cve-bin-tool,home-assistant/core,locustio/locust,ansible/ansible --label bug --language python --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "python" + "easy difficulty" (good-first-issue in the same added general-purpose Python repos)
gh search issues --repo django/django,pallets/flask,fastapi/fastapi,wagtail/wagtail,scrapy/scrapy,pandas-dev/pandas,Lightning-AI/pytorch-lightning,matplotlib/matplotlib,psf/black,pydantic/pydantic,python-poetry/poetry,psf/requests,Textualize/rich,TheAlgorithms/Python,ossf/cve-bin-tool,home-assistant/core,locustio/locust,ansible/ansible --label "good first issue" --language python --state open --limit 500 --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr


# ===== Batch 5: large OSS orgs, any listed language, one query per label (gh ANDs repeated labels, so OR needs separate queries; --sort updated keeps the newest if the 100 cap truncates) =====
# Profile: "apache" org + "good first issue" label + listed languages (python, javascript, typescript, css, scss, html, java, dockerfile); easy difficulty
gh search issues --owner apache --label "good first issue" --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "apache" org + "help wanted" label + listed languages (python, javascript, typescript, css, scss, html, java, dockerfile); small bug fixes / unclaimed work
gh search issues --owner apache --label "help wanted" --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "apache" org + "bug" label + listed languages (python, javascript, typescript, css, scss, html, java, dockerfile); small bug fixes
gh search issues --owner apache --label bug --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr


# Profile: "pytorch" org + "good first issue" label + listed languages (python, javascript, typescript, css, scss, html, java, dockerfile); easy difficulty
gh search issues --owner pytorch --label "good first issue" --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "pytorch" org + "help wanted" label + listed languages (python, javascript, typescript, css, scss, html, java, dockerfile); small bug fixes / unclaimed work
gh search issues --owner pytorch --label "help wanted" --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "pytorch" org + "bug" label + listed languages (python, javascript, typescript, css, scss, html, java, dockerfile); small bug fixes
gh search issues --owner pytorch --label bug --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "opensearch-project" org + "good first issue" label + listed languages (python, javascript, typescript, css, scss, html, java, dockerfile); easy difficulty
gh search issues --owner opensearch-project --label "good first issue" --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "opensearch-project" org + "help wanted" label + listed languages (python, javascript, typescript, css, scss, html, java, dockerfile); small bug fixes / unclaimed work
gh search issues --owner opensearch-project --label "help wanted" --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "opensearch-project" org + "bug" label + listed languages (python, javascript, typescript, css, scss, html, java, dockerfile); small bug fixes
gh search issues --owner opensearch-project --label bug --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "open-telemetry" org + "good first issue" label + listed languages (python, javascript, typescript, css, scss, html, java, dockerfile); easy difficulty
gh search issues --owner open-telemetry --label "good first issue" --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "open-telemetry" org + "help wanted" label + listed languages (python, javascript, typescript, css, scss, html, java, dockerfile); small bug fixes / unclaimed work
gh search issues --owner open-telemetry --label "help wanted" --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "open-telemetry" org + "bug" label + listed languages (python, javascript, typescript, css, scss, html, java, dockerfile); small bug fixes
gh search issues --owner open-telemetry --label bug --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "backstage, openedx and hackforla (small orgs grouped)" org + "good first issue" label + listed languages (python, javascript, typescript, css, scss, html, java, dockerfile); easy difficulty
gh search issues --owner backstage,openedx,hackforla --label "good first issue" --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "backstage, openedx and hackforla (small orgs grouped)" org + "help wanted" label + listed languages (python, javascript, typescript, css, scss, html, java, dockerfile); small bug fixes / unclaimed work
gh search issues --owner backstage,openedx,hackforla --label "help wanted" --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "backstage, openedx and hackforla (small orgs grouped)" org + "bug" label + listed languages (python, javascript, typescript, css, scss, html, java, dockerfile); small bug fixes
gh search issues --owner backstage,openedx,hackforla --label bug --language javascript,python,typescript,css,html,java,scss,dockerfile --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr



# ===== Batch 6: beginner-friendly Python projects (django is NOT searchable: its GitHub issues are disabled and its 'Easy pickings' tickets live in Trac, so it cannot be scanned or graded with gh; intel/cve-bin-tool is now ossf/cve-bin-tool and the batch-4 queries were fixed to use the new name; pasted list repos already in batch 4 are not repeated in the NEW set) =====
# Profile: "python" + "small bug fixes" + "an active repo project where people answer questions" (new repos: httpx, mypy, hatch, salt, open library, wger, zulip, erpnext, frappe)
gh search issues --repo encode/httpx,python/mypy,pypa/hatch,saltstack/salt,internetarchive/openlibrary,wger-project/wger,zulip/zulip,frappe/erpnext,frappe/frappe --label bug --language python,javascript,typescript --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "python" + "easy difficulty" (good first issue in the same new repos)
gh search issues --repo encode/httpx,python/mypy,pypa/hatch,saltstack/salt,internetarchive/openlibrary,wger-project/wger,zulip/zulip,frappe/erpnext,frappe/frappe --label "good first issue" --language python,javascript,typescript --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "easy difficulty" + "help wanted" label vocabulary of the listed repos (unclaimed work; used by zulip, sklearn, lightning, scrapy)
gh search issues --repo fastapi/fastapi,scrapy/scrapy,encode/httpx,psf/requests,scikit-learn/scikit-learn,pandas-dev/pandas,Lightning-AI/pytorch-lightning,matplotlib/matplotlib,python/mypy,python-poetry/poetry,pypa/hatch,saltstack/salt,ossf/cve-bin-tool,ansible/ansible,internetarchive/openlibrary,home-assistant/core,wger-project/wger,zulip/zulip,frappe/erpnext,frappe/frappe --label "help wanted" --language python,javascript,typescript --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "easy difficulty" + "help wanted" label vocabulary of the listed repos (hyphenated variant; used by salt and home-assistant)
gh search issues --repo fastapi/fastapi,scrapy/scrapy,encode/httpx,psf/requests,scikit-learn/scikit-learn,pandas-dev/pandas,Lightning-AI/pytorch-lightning,matplotlib/matplotlib,python/mypy,python-poetry/poetry,pypa/hatch,saltstack/salt,ossf/cve-bin-tool,ansible/ansible,internetarchive/openlibrary,home-assistant/core,wger-project/wger,zulip/zulip,frappe/erpnext,frappe/frappe --label help-wanted --language python,javascript,typescript --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "easy difficulty" + "help wanted" label vocabulary of the listed repos (hyphenated variant; used by mypy)
gh search issues --repo fastapi/fastapi,scrapy/scrapy,encode/httpx,psf/requests,scikit-learn/scikit-learn,pandas-dev/pandas,Lightning-AI/pytorch-lightning,matplotlib/matplotlib,python/mypy,python-poetry/poetry,pypa/hatch,saltstack/salt,ossf/cve-bin-tool,ansible/ansible,internetarchive/openlibrary,home-assistant/core,wger-project/wger,zulip/zulip,frappe/erpnext,frappe/frappe --label good-first-issue --language python,javascript,typescript --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "easy difficulty" + "help wanted" label vocabulary of the listed repos (scikit-learn's difficulty label)
gh search issues --repo fastapi/fastapi,scrapy/scrapy,encode/httpx,psf/requests,scikit-learn/scikit-learn,pandas-dev/pandas,Lightning-AI/pytorch-lightning,matplotlib/matplotlib,python/mypy,python-poetry/poetry,pypa/hatch,saltstack/salt,ossf/cve-bin-tool,ansible/ansible,internetarchive/openlibrary,home-assistant/core,wger-project/wger,zulip/zulip,frappe/erpnext,frappe/frappe --label Easy --language python,javascript,typescript --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "easy difficulty" + "help wanted" label vocabulary of the listed repos (ansible's label)
gh search issues --repo fastapi/fastapi,scrapy/scrapy,encode/httpx,psf/requests,scikit-learn/scikit-learn,pandas-dev/pandas,Lightning-AI/pytorch-lightning,matplotlib/matplotlib,python/mypy,python-poetry/poetry,pypa/hatch,saltstack/salt,ossf/cve-bin-tool,ansible/ansible,internetarchive/openlibrary,home-assistant/core,wger-project/wger,zulip/zulip,frappe/erpnext,frappe/frappe --label easyfix --language python,javascript,typescript --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "easy difficulty" + "help wanted" label vocabulary of the listed repos (matplotlib's emoji label)
gh search issues --repo fastapi/fastapi,scrapy/scrapy,encode/httpx,psf/requests,scikit-learn/scikit-learn,pandas-dev/pandas,Lightning-AI/pytorch-lightning,matplotlib/matplotlib,python/mypy,python-poetry/poetry,pypa/hatch,saltstack/salt,ossf/cve-bin-tool,ansible/ansible,internetarchive/openlibrary,home-assistant/core,wger-project/wger,zulip/zulip,frappe/erpnext,frappe/frappe --label "🌱 Good first issue" --language python,javascript,typescript --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "easy difficulty" + "help wanted" label vocabulary of the listed repos (matplotlib's second beginner label)
gh search issues --repo fastapi/fastapi,scrapy/scrapy,encode/httpx,psf/requests,scikit-learn/scikit-learn,pandas-dev/pandas,Lightning-AI/pytorch-lightning,matplotlib/matplotlib,python/mypy,python-poetry/poetry,pypa/hatch,saltstack/salt,ossf/cve-bin-tool,ansible/ansible,internetarchive/openlibrary,home-assistant/core,wger-project/wger,zulip/zulip,frappe/erpnext,frappe/frappe --label first-contribution --language python,javascript,typescript --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# ===== Batch 7: agentic AI repos from the pasted list that no earlier query covered (already covered, so skipped: OpenHands/OpenHands [All-Hands-AI renamed], dify, pydantic-ai, vllm, langchain, langfuse, promptfoo, dspy, ollama; spring-ai and langchain4j are Java, which the profile lists; llama.cpp is mostly C++ so the language flag keeps only its python/js/ts/java issues) =====
# Profile: "reputable agentic AI tools and libraries" + "small bug fixes" + "Both frontend and backend issues" (new repos from the list)
gh search issues --repo openclaw/openclaw,n8n-io/n8n,NousResearch/hermes-agent,browser-use/browser-use,ChromeDevTools/chrome-devtools-mcp,backnotprop/plannotator,spring-projects/spring-ai,langchain4j/langchain4j,bytedance/deer-flow,ggml-org/llama.cpp,open-webui/open-webui --label bug --language python,javascript,typescript,java --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "reputable agentic AI tools and libraries" + "easy difficulty" (good first issue in the same new repos)
gh search issues --repo openclaw/openclaw,n8n-io/n8n,NousResearch/hermes-agent,browser-use/browser-use,ChromeDevTools/chrome-devtools-mcp,backnotprop/plannotator,spring-projects/spring-ai,langchain4j/langchain4j,bytedance/deer-flow,ggml-org/llama.cpp,open-webui/open-webui --label "good first issue" --language python,javascript,typescript,java --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "reputable agentic AI tools and libraries" + "help wanted" label + unclaimed work (same new repos)
gh search issues --repo openclaw/openclaw,n8n-io/n8n,NousResearch/hermes-agent,browser-use/browser-use,ChromeDevTools/chrome-devtools-mcp,backnotprop/plannotator,spring-projects/spring-ai,langchain4j/langchain4j,bytedance/deer-flow,ggml-org/llama.cpp,open-webui/open-webui --label "help wanted" --language python,javascript,typescript,java --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# ===== Batch 9 (2026-11-15): repos added from the pasted list. Already covered by earlier queries, so skipped in the NEW set: hermes-agent, deer-flow, browser-use, chrome-devtools-mcp, openclaw, spring-ai, langchain4j (batch 7); dify, vllm, langgraph, adk-python, agent-framework have only bug and/or good-first-issue queries, so the gap query below adds their missing labels. Java repos stay under batch 7, which already lists java. =====
# Profile: "reputable agentic AI tools and libraries" + "small bug fixes" (new repos: fastmcp, sglang, paperclip, opencode, gemini-cli)
gh search issues --repo PrefectHQ/fastmcp,sgl-project/sglang,paperclipai/paperclip,anomalyco/opencode,google-gemini/gemini-cli --label bug --language python,javascript,typescript --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "reputable agentic AI tools and libraries" + "easy difficulty" (good first issue in the same new repos)
gh search issues --repo PrefectHQ/fastmcp,sgl-project/sglang,paperclipai/paperclip,anomalyco/opencode,google-gemini/gemini-cli --label "good first issue" --language python,javascript,typescript --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "reputable agentic AI tools and libraries" + "help wanted" label + unclaimed work (same new repos)
gh search issues --repo PrefectHQ/fastmcp,sgl-project/sglang,paperclipai/paperclip,anomalyco/opencode,google-gemini/gemini-cli --label "help wanted" --language python,javascript,typescript --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "reputable agentic AI tools and libraries" + "help wanted" label + unclaimed work (gap fill: these listed repos had no help-wanted query)
gh search issues --repo google/adk-python,microsoft/agent-framework,langchain-ai/langgraph,vllm-project/vllm,langgenius/dify --label "help wanted" --language python,javascript,typescript --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "reputable agentic AI tools and libraries" + "easy difficulty" (gap fill: adk-python and agent-framework had no good-first-issue query)
gh search issues --repo google/adk-python,microsoft/agent-framework --label "good first issue" --language python,javascript,typescript --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# ===== Batch 10 (2026-11-15): priority list B, medical imaging and health AI (MONAI, nnUNet, openmed) =====
# Profile: "python" + "small bug fixes" (priority list B)
gh search issues --repo Project-MONAI/MONAI,MIC-DKFZ/nnUNet,maziyarpanahi/openmed --label bug --language python,javascript,typescript --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "python" + "easy difficulty" (priority list B)
gh search issues --repo Project-MONAI/MONAI,MIC-DKFZ/nnUNet,maziyarpanahi/openmed --label "good first issue" --language python,javascript,typescript --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "python" + "help wanted" label + unclaimed work (priority list B)
gh search issues --repo Project-MONAI/MONAI,MIC-DKFZ/nnUNet,maziyarpanahi/openmed --label "help wanted" --language python,javascript,typescript --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# ===== Batch 8 (2026-11-15): isolated fixes for repos without a reliable good-first-issue label (schema validation and test gaps; no label flag, keyword search instead) =====
# Profile: "small bug fixes" + "isolated fix: schema or input validation tweak" (agentic AI orgs and named repos)
gh search issues "validation OR schema OR validate" --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-ai,BerriAI,letta-ai,langfuse --language python,typescript,javascript --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr
gh search issues "validation OR schema OR validate" --repo pydantic/pydantic-ai,huggingface/smolagents,vercel/ai,google/adk-python,browser-use/browser-use,n8n-io/n8n,deepset-ai/haystack,stanfordnlp/dspy,confident-ai/deepeval,promptfoo/promptfoo --language python,typescript,javascript --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

# Profile: "small bug fixes" + "isolated fix: test gap" (same orgs)
gh search issues "missing test OR add tests OR test coverage" --owner langchain-ai,anthropics,openai,modelcontextprotocol,crewAIInc,run-llama,mastra-ai,BerriAI,letta-ai,langfuse --language python,typescript,javascript --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

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
- for results from queries that carry an `--owner` or `--repo` flag only (the all-owners queries are exempt), drop any result whose `repository.nameWithOwner` is not under an owner named in an `--owner` flag (langchain-ai, anthropics, openai, modelcontextprotocol, crewAIInc, run-llama, mastra-ai, BerriAI, letta-ai, langfuse, pytorch, open-telemetry, apache, opensearch-project, backstage, kubernetes, openedx, hackforla) or equal to a repo named in a `--repo` flag (guards against off-target leaks from any query)
- drop any result from `anthropics/claude-code` (its repo is an issue tracker only, so there is no source code to fix)
- (removed 2026-11-15: the drop of results created less than 3 days ago; fresh tickets are now preferred, see the ordering below. The rubric's responds-to-issues check already passes a 0-comment issue when the repo sample shows maintainers answering others.)
- drop any result with more than 15 comments (long threads are rarely "small" or "easy")
- drop any result whose title starts with `Feature` or `[Feature` (unlabelled repos still mark features in titles);
- drop any result whose title, case-insensitively, starts with `feat(`, `feat:`, `[feat`, `enhancement`, `[enhancement` or the word `update` (e.g. `Update Table: Project`); these are features, enhancements or maintenance tasks, not bugs (profile: "not a new feature"; decided 2026-11-06 after reserved beginner slots filled with non-bugs)
- drop any result whose labels include `question`, `feature`, `feature request` or `discussion` (profile: "not a new feature"; `documentation` handled by the conditional docs drop below)
- drop any result whose labels include `easy close` (fastapi's label for issues that are easy to close, not easy to fix)
- drop any result whose labels include `documentation`, `docs`, `topic: documentation`, `module: docs` or any label containing `doc`, unless its title also contains a code or test word (`test`, `bug`, `fix`, `error`, `crash`, `fail`, `validation`, `schema`, `exception`, `type`); documentation-only tickets are excluded, docs gaps that also need tests or code are kept (profile: "docs gap counts only when the ticket also needs a test or code change"; revised 2026-11-15; the rubric's not-docs-only check reads the body for the rest)
- drop any result whose title or labels, case-insensitively, name a non-macOS platform without also naming macOS/darwin/mac: `windows`, `win32`, `win10`, `win11`, `wsl`, `powershell`, `linux`, `ubuntu`, `android`, `ios`, `os: windows`, `platform: windows` (profile: MacBook Pro, no Windows or other-OS tickets; added 2026-11-15)
- order what remains, in this priority: (1) a bug label (`bug`, `🐞 bug`) together with a friendly label; (2) a bug label alone; (3) a beginner label without a bug label; (4) a help label alone; then, within the same tier, issues whose title or labels contain `validation`, `validate`, `schema`, `pydantic`, `typo`, `missing test`, `add test`, `test coverage` or `tests` come first (isolated-fix types; added 2026-11-15), then issues created within the last 3 days first (`createdAt`; catch tickets right as they are triaged, before others claim them; added 2026-11-15), then fewest comments first, then most recently updated first. Labels are compared case-insensitively. Beginner labels: `good first issue`, `good-first-issue`, `🌱 good first issue`, `easy`, `easyfix`, `easy-fix`, `difficulty: easy`, `first-contribution`. Help labels: `help wanted`, `help-wanted`. (profile: small bug fixes, easy, unclaimed; a friendly label without a bug label is often a stale, claimed or non-bug issue)
- tie-break: if still tied, prefer the repo with the most total results across all queries (a busy repo is more likely to answer questions)

### Per-repo coverage (decided 2026-11-06: every listed repo gets up to 100 issues into the ranked pool; quota raised from 20 to 100 on 2026-11-15)

A "listed repo" is any repo named in a `--repo` flag above, except `django/django` (its GitHub issues are disabled; its tickets live in Trac, so it has nothing to search).

- **Quota:** after the drop filters and the ordering above, keep at most the best **100** issues of each repo (listed or not, so owner-scoped queries such as apache or pytorch are capped per repo too). Everything beyond a repo's 20th is dropped before the final ranking.
- **Top-up (floor):** if a listed repo has fewer than 100 issues after the drop filters, run one top-up query for that repo with no label and no language flag, so repos that do not use these labels can still fill its quota. The ordering above already places unlabelled issues last within the pool. Top-up query template (one repo per run, not a query in itself):

      [TEMPLATE ONLY, NOT A QUERY] gh search issues --repo OWNER/REPO --state open --limit 500 --sort updated --order desc --no-assignee --updated ">2026-08-18" --json title,repository,labels,assignees,commentsCount,createdAt,updatedAt,url -- -linked:pr

  Top-up results go through the same drop filters. A repo may still end below 100 (for example when most of its recent issues are assigned or have open PRs); the scan reports each listed repo's final count.
- **Final ranking (round-robin, decided 2026-11-06 so the graded list spreads across repos):** inside each repo, number its capped issues 1, 2, 3, ... in the order above (its own best issue is number 1). Then sort the whole pool by that in-repo number first, and within the same number by the order above (label priority, fewest comments, most recent update, repo with the most total results). Take the top N of that sort (priority repos first; see Priority repos below). In effect the graded list takes every repo's best issue before any repo's second best.
- **Reserved beginner slots (decided 2026-11-06, 4 of the graded slots; ordering changed to repo diversity the same day):** a beginner label without a bug label (`good first issue`, `good-first-issue`, `🌱 good first issue`, `easy`, `easyfix`, `easy-fix`, `difficulty: easy`, `first-contribution`) puts an issue in the "beginner-only" group. Fill 4 graded slots first from that group (priority repos before others; see Priority repos below), **one issue per repo**: order the repos by their total result count across all queries, busiest first (a busy repo is more likely to answer questions), and take each repo's most recently updated beginner-only issue. Comment count plays no part in this ordering. Fill the remaining slots from the rest of the pool with the round-robin above, skipping issues already taken. If the beginner-only group has fewer than 4 repos, the leftover slots go to the main round-robin.



### Priority repos (decided 2026-11-15: scanned first, ranked first)

These 17 repos (list A) from the user's pasted list (15 plus the two Java repos, spring-ai and langchain4j) come before every other repo:

NousResearch/hermes-agent, bytedance/deer-flow, browser-use/browser-use, PrefectHQ/fastmcp, vllm-project/vllm, sgl-project/sglang, langchain-ai/langgraph, google/adk-python, microsoft/agent-framework, langgenius/dify, openclaw/openclaw, paperclipai/paperclip, anomalyco/opencode, google-gemini/gemini-cli, ChromeDevTools/chrome-devtools-mcp, spring-projects/spring-ai, langchain4j/langchain4j

- **Search first:** run every query that names one of these repos or its owner (Batch 7, Batch 9, the batch-1 owner queries for `langchain-ai`, the dify queries, and the adk-python and agent-framework queries) before any other query, and run the top-up query for each priority repo that ends under 100 issues before top-ups for other repos.
- **Rank first:** the drop filters, ordering and per-repo quota above apply unchanged, but a priority repo's issues form the first pool. Build the final ranking (round-robin and reserved beginner slots below) from the priority pool alone. Issues from every other repo are used only to fill slots the priority pool cannot (for example when fewer than the graded count of priority issues survive, or fewer than 4 priority repos have a beginner-only issue), and then they follow the same round-robin among themselves.
- **No other change:** priority never overrides a drop filter or a rubric verdict; a priority repo whose issues all fail simply yields no graded slots.

### Priority list B (decided 2026-11-15: medical imaging and health AI)

Project-MONAI/MONAI, MIC-DKFZ/nnUNet, maziyarpanahi/openmed

- Treated exactly like the priority repos above: its queries (Batch 10) run first, its issues join the priority pool, and the top-up query runs for each of its repos that ends under 100 issues. Lists A and B share the same pool and round-robin.
- Expect hardware rejections: much of MONAI and nnUNet work needs a GPU or large datasets, which the `no-special-hardware` check rejects.

### Grading until enough are accepted (decided 2026-11-15)

Wanted: more accepted issues to choose from. Grade the top 10 first. If fewer than **5** are accepted, grade the next 10 from the same ranking, then the next, until 5 are accepted, the pool is exhausted, or **5 batches** (50 graded issues, about $10) have been graded, whichever comes first. If the cap is hit with fewer than 5 accepted, report what was accepted and say the cap stopped the scan. Past the priority pool the ranking continues into every other repo found by any query (all owners and listed repos, not only the priority ones), using the same round-robin and drop filters. Each extra batch of 10 costs about $2. Report accepted issues from all batches together, priority repos first.

## How many to grade

Grade the top **10** ranked results per scan.

<!-- Ten again (lowered from twenty on 2026-11-15; extra batches of 10 are graded until 5 are accepted). Twenty (raised from ten on 2026-11-15 because a one-slot miss kept MONAI #8489 out of scan 10; cost accepted: about $0.20 per candidate, so a scan is about $4). Ten (raised from the default five on 2026-11-06 so a wider pool of listed repos reaches grading; cost accepted: about $0.20 per candidate, so a scan is about $2). Five was the default because grading is the only stage that costs
money: about $0.20 per candidate, so a scan is about $1, and you can
iterate the strategy several times for less than one full regression
run. You need one good issue, not ten. Raising this number is
allowed, but it is a decision you state here in a sentence, with the
cost you are accepting; it is not a number you bump because a scan
disappointed you once. -->
