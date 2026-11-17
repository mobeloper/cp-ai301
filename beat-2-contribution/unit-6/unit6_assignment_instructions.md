

The work, in order

1. DONE! Run the commands in steps 2, 7, 8, and 12 from there.

2. DONE! Install the voice guard: follow skill/INSTALL.md, steps 1 to 5. You'll add an Upstream section to your voice guide, with rules for the claims, questions, and pings you post, then install the hook and test that it fires. In step 5, run only the wiring test for now, because you check your claim draft in step 8 below. No unit 2 voice guide? INSTALL.md step 1 shows you how to start one. Then give repro-check and plan-check the scopes for real repos, so they stop expecting the sandbox:

cp skill/repro-check-scope-wild.md ~/.claude/skills/repro-check/scope.md
cp skill/plan-check-scope-wild.md ~/.claude/skills/plan-check/scope.md


3. DONE! Patch your rubric's scope check: real issues are rarely as tidy as the sandbox's, so a strict scope check rejects most of them. Open skill/rubric-scope-patch.md. In ~/.claude/skills/issue-scout/rubric.md, replace the check that grades whether an issue is one piece of work a newcomer could take on (you named it yourself in unit 1) with the scope-feasible row, and add the difficulty note section under your checks table. Then add a dated line to ~/.claude/skills/issue-scout/scan-notes.md that names the check you replaced.

4. DONE! Grade your picks again: run claude "issue-scout: grade <issue URL> <issue URL>" with the two or three picks you graded last week (about $0.20 an issue). Add the read-out to ~/.claude/skills/issue-scout/scan-output.md, below last week's. Each accepted issue now comes with a difficulty note: straightforward, moderate, or ambitious.

>> Grade my picks again: run claude "issue-scout: grade
  https://github.com/maziyarpanahi/openmed/issues/5296
  https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/3412
  https://github.com/maziyarpanahi/openmed/issues/5121
  https://github.com/maziyarpanahi/openmed/issues/5097
  https://github.com/PrefectHQ/fastmcp/issues/6398
  https://github.com/PrefectHQ/fastmcp/issues/6491. Add the read-out to ~/scan-output.md, at the end.

5. DONE! Choose one: pick the issue that fits your profile and that you'd be happy to work on for the next two weeks. Your scout's verdict is evidence, so you can choose an issue it rejected if your story says why. Then read every comment on the issue's GitHub thread. If someone has already asked to work on it, it's taken even if nobody is assigned, so choose another. If the difficulty note says ambitious, it names an open question, so ask about it first in your claim. Then, on the Issue Selection site, click Unclaim on any picks you're not using, so classmates can choose them.

>> Selected issue: https://github.com/maziyarpanahi/openmed/issues/5295

6. DONE! For te chosen repo issue: "openmed#5296", read the repo's rules: find its CONTRIBUTING.md (in the repo's main folder, .github/, or docs/) and read it end to end. Note what a PR needs before maintainers will accept it, the scope rules, the test commands, and what it says about AI. If the repo doesn't accept AI-assisted contributions, you can't contribute there honestly, so go back to step 5 and choose again. 


7. DONE! Draft your claim: in your Unit 6 materials folder, write your claim comment in a new file, claim-draft.md. Say you'd like to work on the issue, name your first step, and say when you'll report back. Then check it:

>> claude "/repro-check grade my draft claim comment in claim-draft.md for issue https://github.com/maziyarpanahi/openmed/issues/5295"
Revise until it passes.


8. Post your claim through Claude Code, so the voice guard checks it first (a few cents of course credit):

>>claude "run this command: gh issue comment https://github.com/maziyarpanahi/openmed/issues/5295 --body-file claim-draft.md"

If the guard stops it, it quotes the rule your draft broke. Fix the draft and run the command again. If you post on GitHub's website instead, run python3 ~/.claude/voice-guard/check_voice.py --draft claim-draft.md first, because the guard can't see your browser. Did someone claim it before you? Go back to step 5.

Issue claim comment: 
https://github.com/maziyarpanahi/openmed/issues/5295#issuecomment-6933573207 


9. DONE! Write Why I Chose This Issue: in your course repo, open beat-2-contribution/contribution-story.md. Fill in the top of the file: the issue's title, your name, your GitHub username, and the issue's link. Then fill in the Why I Chose This Issue section in your own words: where the issue came from, what your scout's verdict and difficulty note said, why it fits you, what you hope to learn, an honest read of how your scout did, and the link to your claim comment (on GitHub, click ... on your comment, then Copy link). Then fill in the Toolkit block already at the end of that section, about your scout, including the scope patch from step 3. Not ready to claim yet? Write claim pending and the date where the link goes, and add the link once you post. 

Commit your changes on GitHub, the way you have since unit 1.



10. Write your repo's memory file: if the repo isn't on your computer yet, fork it on GitHub and clone your fork. From your Unit 6 materials folder, run cp skill/claude-local-template.md <path to your clone>/CLAUDE.local.md, then open that file and follow the instructions inside it. Claude Code reads this file at the start of every session in that repo, so Claude doesn't have to relearn the repo each time. The template also shows you how to keep the file out of your PR.

Write your Unit 6 Toolkit entry: this is the Toolkit section near the top of your contribution story, not the block from step 9. Skip the example block there, and find the Unit 6 entry. Keep "Unit 6:" at the start of its heading, and replace the brackets after it with one line on your tool work. Then replace the bracketed paragraph under it with two to four sentences about the voice guard and your memory file: what you built, what broke or fell short, and what you changed because of it. Commit it on GitHub.

Start reproducing the issue: show that what the issue describes is real today, with evidence someone else could check. Write it up in a new file, repro-draft.md, in your Unit 6 materials folder next to claim-draft.md. Then grade the two together, the way you did in unit 2:

claude "repro-check: grade my claim comment in claim-draft.md and my repro report in repro-draft.md for issue <issue URL>"
When it passes, move your report into the three parts of your story's Reproduction Process section: Environment Setup, Steps to Reproduce, and Reproduction Evidence. For the evidence, an output excerpt, a log, or a commit link from your fork all work (no commit? Write none on the commit line). Then fill in that section's Toolkit block, about repro-check. Don't post the report on the issue. Your PR description carries the proof when you open it.

📦 What you submit
Nothing this week. You submit Assignment 5 once, your course repo link, from the unit 5 Assignment tab. Graders read only beat-2-contribution/contribution-story.md, your issue's thread, and your pull request. Your voice guard, voice guide, and memory file stay on your computer, and your tools are graded once, in the unit 9 portfolio.

🎯 Where this week's work counts
The full rubric is on the Grading Rubrics page under Course Info. This week's work feeds four parts of it: choosing your issue (50 points, step 9), reproducing it (part of the 50 for reproduction, planning, and testing, step 12), the link to your claim comment (part of the 60 for your pull request), and the Toolkit thread (40 points, steps 9 and 11). Nobody grades whether you picked the right issue.


