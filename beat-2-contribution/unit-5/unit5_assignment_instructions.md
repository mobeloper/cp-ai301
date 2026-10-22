# The work, in order

1. Check gh: run gh auth status in your terminal. If it says you're logged in, you're set. If the command isn't found, install the GitHub CLI from cli.github.com, then run gh auth login and follow the prompts. Stuck? Ask in #dts-fa26-ai301-tech-help.

2. Save your fit profile: open ~/.claude/skills/issue-select/scope.md. Copy the sentences you wrote under "Your fit profile" (not the comment above them) somewhere safe, because the next step replaces that file.

3. Install the scout over your unit 1 skill: clone your Unit 5 materials (git clone https://github.com/codepath/ai301-unit5-starter.git, or Code, then Download ZIP on the repo page). Go into the folder (cd ai301-unit5-starter, or ai301-unit5-starter-main if you downloaded the ZIP) and run these two commands once:

cp skill/SKILL.md skill/scope.md skill/search-strategy.md ~/.claude/skills/issue-select/
mv ~/.claude/skills/issue-select ~/.claude/skills/issue-scout
This replaces SKILL.md and scope.md, adds search-strategy.md, and renames the folder. Your rubric.md and references/evidence-guide.md stay as they are. If either one is missing afterward, download it from tools/issue-select/ in your course repo on GitHub and put it back where it was, under ~/.claude/skills/issue-scout/. If cp says issue-select is not a directory, the install already ran.

4. DONE! Update your fit profile: in the new ~/.claude/skills/issue-scout/scope.md, replace "(Paste your fit profile here.)" with your profile. Your search queries in the next step come from it, so name the languages, kinds of work, and kinds of project you actually want, and anything you want to avoid.




5. DONE! Write your search strategy: open ~/.claude/skills/issue-scout/search-strategy.md and fill in its sections. The comments in the file explain each one.

    Queries: below the comment block, write at least one gh search issues command on its own line, with the profile sentence it came from on the line above it. Start from the filters that worked in the activity, using your own profile's words. The filter key in the activity worksheet (linked on the Activity tab) gives the gh flag for each browser filter. Add --state open and --limit 100, because otherwise gh also returns closed issues and stops at 30 results. Every flag is in the gh search issues manual, and every browser filter is in GitHub's guide to searching issues.

    Ranking filters: write at least one rule that uses only what gh returns with each result (assignees, last update, labels, comment count, repo). Make at least one rule an order, such as "fewest comments first".

    How many to grade: leave it at 5.
    
Before you trust a query to the scout, run it in your terminal and read the results.



I filled in issue-scout/search-strategy.md with seven queries, eight ranking rules and the top-5 grading line. I ran the query shapes in your terminal first, and the flag set behaves as written. I didn't run the seven written commands verbatim, because the test helper put the flags in a different order.

Queries. Each one has the profile sentence it came from on the line above it, and all of them use --state open --limit 100 --no-assignee --updated ">2026-08-18" -- -linked:pr.
- Searching all of GitHub for "agent" returned obscure repos, such as Dpro-at/Tel-Agent, which doesn't fit "reputable". So I scoped the queries to repos instead.
- Five queries search --label bug in a curated set of repos. They run per language (python, typescript, javascript), as gh takes one language per search.
  - Orgs (--owner): langchain-ai, anthropics, openai, modelcontextprotocol, crewAIInc, run-llama, mastra-ai, BerriAI, letta-ai, langfuse. GitHub caps ownership filters per query, so I kept this list to ten.
  - Individual repos (--repo): pydantic-ai, smolagents, vercel/ai, autogen, google/adk-python, AutoGPT, microsoft/agent-framework. I used named repos for these because Microsoft and Google as whole orgs return mostly unrelated projects.
- Two more queries search good first issue in python and typescript. They return very little (1 and 0 results in my tests), but I kept them as the best signal when something does turn up.

Ranking filters. The rules use only the fields gh returns.
- Drops:
  - Any result with an assignee.
  - Any result not updated in the last 60 days.
  - Anything from anthropics/claude-code, which is an issue tracker with no source to fix. It was 79 of the 100 typescript results in my test.
  - Anything with more than 10 comments.
  - Anything labelled question, enhancement, feature request or discussion.
- Order: good first issue first, then fewest comments, then most recently updated. A busy repo wins ties.
- Diversity cap: at most 2 results per repo, so langchain and litellm can't fill all five slots.

How many to grade: left at 5.

Things to know before the scan:
- The date cutoff 2026-08-18 is hard-coded, so refresh it when you rerun later.
- The javascript queries return few results, since most of these repos are typescript.





6. DONE! Run one scan and save it: run claude "issue-scout: run a scan". Your scout searches with your queries, ranks the results with your filters, and grades the top five with your rubric, which costs about $1 of your course credit. Copy the read-out, from the first line of the summary through the end of the JSON block, into a new file, ~/.claude/skills/issue-scout/scan-output.md.


 issue-scout: run a scan

Skill(issue-scout)
Successfully loaded skill

  Ran 4 shell commands

Status: the search and ranking ran, but the top results look off-target. The --owner queries seem to be returning repos outside the named orgs. I'm checking that before I grade anything.

  Ran 2 shell commands

I ran the scan but didn't grade any candidates, because the top 5 came from repos outside your profile.

Search: I ran all 15 queries and got 500 unique issues. Queries 1–3 hit the 100-result cap. After the ranking filters, 353 issues remained.

Why it failed: The queries that pass a comma-separated list of 10 orgs to --owner (queries 1, 2, 3, 6 and 7) don't filter to those orgs. They returned issues from unrelated small repos. Examples are Jeffy123-zhu, SoroScan and waynerobotics, which none of your queries name. The ranking put those on top, mostly "good first issue" items with 0 comments. Nothing in the top 5 is a reputable agentic AI repo, and several aren't Python or TypeScript bug fixes. Grading them would only have produced rejections.

The queries that name repos with --repo (4, 5), and the --owner pytorch queries (14, 15), returned the right repos. Query 14 gave 58 PyTorch results. Those were outranked by the junk "good first issue" results.





7. Fix what your scan got wrong: read the five verdicts. If a check misread something about a real repo, fix that check in ~/.claude/skills/issue-scout/rubric.md. If a query or filter let the wrong issues through, fix search-strategy.md. Run another scan only if you want to see a fix work (about $1 each). Log what you changed and why as a dated line in ~/.claude/skills/issue-scout/scan-notes.md (create it), because your Toolkit entry in step 10 and your unit-9 portfolio are written from those lines. "Nothing broke" is a fine answer when it's true, as long as you can say how you know.


    What was wrong: The "Q1" that returned Jeffy123-zhu and SoroScan was the example line inside the HTML comment in search-strategy.md. It starts with gh search issues and has no --owner, so the scout ran it as a 15th query. That also explains why the scout counted 15 queries when the file has 14. I indented that line and labelled it [EXAMPLE ONLY, NOT A QUERY].



==

Re-run:

- The 14 queries returned 403 unique issues, and 308 survived the ranking filters.
- GitHub rate-limited the back-to-back queries, so I re-ran with an 8-second pause between them. Query 13 was one of the ones the rate limit had silently zeroed.


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



Grading, all rejected:

Candidate: pytorch/examples#391
Verdict: reject
Why: Repo's last cot's a 2018 issue
────────────────────────────────────────
Candidate: pytorch/pytorch#89197
Verdict: reject
Why: Claimed in July and August, and a 2026-10-04 comment says it's
already fixed on ni
────────────────────────────────────────
Candidate: pytorch/executorch#10819
Verdict: reject
Why: A refactor checklist, not a bug, and mostly done already
───────────────────
Candidate: pytorch/ao#1920
Verdict: reject
Why: Research benchix
────────────────────────────────────────
Candidate: pytorch/tensordict#3005
Verdict: reject
Why: The responds-to-issues check is unclear (see below)

tensordict#3005 is  wrong-values bugwith a repro, the root cause and the fix location (tensordict/base.py:783-793) already identified, and a precedent
fix (#2365). It hasts, the repo is veryactive, and an AGENTS.md shows AI-assisted contributions are
welcome. The reasononly maintainercomments in the last 90 days are one maintainer, peterdsharpe, replying on their own issues. They also tend to fix their own
issues the same daysed in one day), sothey may fix #3005 first.

======

I re-ran the scan, graded the top five, and wrote the read-out to ~/.claude/skills/issue-scout/scan-output.md. It runs from the summary's first line through the closing JSON block, and the JSON parses with all 11 checks for each of the 5 candidates.
I ran the scan, ranking and grading myself, not through the Skill tool. So the Skill tool's token usage and the course-credit cost aren't a figure I can give you. The grading was for the top 5, which is the number in your strategy.
Result: 1 accepted, 4 rejected. The scan used 15 queries and returned 411 unique issues, and 319 survived the ranking filters.
Accepted: langchain-ai/openwiki#1066.
- Bug and fit: in-page links to non-ASCII filenames 404 in openwiki visualize. It's a TypeScript frontend bug in a langchain-ai agentic tool.
- Checks: it passes all 7 required checks. It has a repro, a recent release and a contributing guide, but no good first issue
- Status: still open, 0 comments, no assignee and no linked PR.
- Caveats: I judged the fix size from the issue text, not from src/visualize/client.ts. Open visualizer link PRs (#654, #886,
#608) exist but aren't linked to this issue.
Rejected:
- tensordict#3043 and #3042: the maintainer files issue and fix-PR batches, so they fail unclaimed. They also fail responds-to-issues, since no maintainer reply to an outsider's issue shows up since July 20.
- servers#5176: the fix is waiting on a python-sdk release, and the maintainer who opened it is running a related PR batch.
- python-sdk#3729: the reporter asks whether the boundary behavior is intentional, so it isn't a confirmed bug yet.



8. Pick two or three issues on the Issue Selection site: your real issue comes from this site. Open the AI301 Issue Selection site and sign in with GitHub. Set its Language and Tag filters from your profile, the way you turned the sample profile into a search in the activity. If your profile names a project you want, set Organization too. Then read the issues and pick two or three that fit. You can click Claim to hold a pick from your classmates while you decide (up to three at a time). It doesn't post anything on GitHub. If your scan found an issue you'd rather do, ask your instructor.

Grade your picks: run claude "issue-scout: grade <issue URL> <issue URL>" with your picks' links. Grading costs about $0.20 an issue. Add the read-out to scan-output.md, below your scan. If your scout rejects an issue you still want, you can still choose it. Just say why in your story.

Start your contribution story: in your course repo, open beat-2-contribution/contribution-story.md and read its headings once, top to bottom. Under the Toolkit section, find the Unit 5 entry. Keep "Unit 5:" at the start of its heading, and replace the brackets after it with one line on your tool work. Then replace the bracketed paragraph under it with two to four sentences: what you built, what broke or fell short, and what you changed because of it. Edit and commit it on GitHub the same way you've edited your course repo since unit 1. Leave "Why I Chose This Issue" for next week, when you choose your issue and claim it.