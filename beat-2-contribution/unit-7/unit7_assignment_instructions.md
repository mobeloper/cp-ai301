🛠️ The work, in order
Install the ship-gate


1. DONE! Get your Unit 7 materials: clone your Unit 7 materials (git clone https://github.com/codepath/ai301-unit7-starter.git, or Code, then Download ZIP on the repo page). The folder is ai301-unit7-starter, or ai301-unit7-starter-main if you downloaded the ZIP.

2. DONE! Install the ship-gate: the ship-gate is a hook that runs before Claude Code opens a pull request. It runs the project's own checks, then your pr-precheck from unit 4, and stops the PR if anything fails. To install it, you need the full path to your clone of your issue's repo (the fork you cloned in unit 6, step 10). Run pwd in the folder you cloned into (not a folder inside it) and copy the path. Then go into your Unit 7 materials folder and run this, with that path in place of <path to your clone>:

python3 skill/install.py --repo <path to your clone>
It puts a new file, ship-manifest.md, in the top folder of your clone. It also keeps that file and the working files you'll make there (plan.md, test_evidence.md, and pr_draft.md) out of your commits. If it prints a cp command for repro-check or plan-check, run that too, from your Unit 6 materials folder. If it stops, it says why and what to do, and section 1 of skill/INSTALL.md in your Unit 7 materials explains each stop. Then quit any open claude session, because Claude Code only loads hooks when a session starts. Stuck? Ask in #dts-fa26-ai301-build-support.

3. DONE! Write your manifest: open ship-manifest.md in your clone and read the comment at its top. Then:

Title: replace <owner/repo> with the original repo's owner and name from your issue's link, like psf/requests.
First table: list the two to four checks the project says to run before a PR, usually its tests and lint. For each one, write its command and where you found it: CONTRIBUTING.md, the CI files in .github/workflows/, or the Build and test commands in your CLAUDE.local.md.
Commands: the ship-gate runs each one in a fresh terminal, without your virtual environment, so write each command the way it runs there. Use uv run pytest if the project uses uv (its CONTRIBUTING.md says uv sync), or your virtual environment's own copy, like .venv/bin/pytest. To test them, open a new terminal, cd into your clone without activating anything, and run each one before you change any code.
Second table: if a check already fails, for a reason your fix won't touch, move it here with today's date and the reason.
Then delete the comment.

4. DONE! Test the hook: from your clone, run claude "run this command: gh pr create --help". You should see two stops, one from your voice guard and one from the ship-gate (its line starts with ship-gate: STOP.). Two stops means both hooks work, since there's no PR text for either to check. If the command runs instead, section 2 of INSTALL.md in your Unit 7 materials has the fixes.


# Plan and build your fix

5. DONE! Write up the issue and your plan: finish your story's Reproduction Process section first if it isn't done (unit 6, step 12), because your plan comes from what you found. Then, in beat-2-contribution/contribution-story.md, fill in Understanding the Issue (four short answers) and its Toolkit block. Next, fill in Solution Approach:

Analysis: the cause, traced to the files and functions involved.
Proposed Solution: your fix, and what you won't change.
Implementation Plan: the steps you'll take.
Copy the Solution Approach text into a new file, plan.md, in the top folder of your clone. Your plan-check reads it in step 6, and the ship-gate reads it before your PR opens.


6. DONE! Check your plan: in the top folder of your clone, write a short plan comment in a new file, plan-comment.md. Say what's causing the bug and how you'll fix it. Run echo plan-comment.md >> .git/info/exclude so it stays out of your commits. Then check both files:

claude "/plan-check grade my plan in plan.md and draft comment in plan-comment.md for issue https://github.com/maziyarpanahi/openmed/issues/5295"
Revise plan.md and plan-comment.md until it says accept, and copy any plan changes back into Solution Approach. Don't post the comment on the issue's thread. Fill in Solution Approach's Toolkit block, about plan-check, and commit it on GitHub.

7. DONE! Build the fix on a branch: in your clone, make a branch, like git switch -c fix/212-empty-shuffle (use your issue's number, or the naming CONTRIBUTING.md asks for). Build the fix and add a test that fails without it, if the project has tests. Make each commit message say what that commit does. Keep every change inside your plan, because a fix for something else belongs in its own PR. As you go, add dated entries to your story's Implementation Notes. If your plan changes, update plan.md and your Implementation Plan, and say so in your PR description.

8. DONE! Test it and save the evidence: run your reproduction steps again on your branch, and run the checks in your manifest. In the top folder of your clone, make a new file, test_evidence.md. Paste in your reproduction's output from before the fix (from your story, or run your steps again on main to get it), its output after the fix, and the checks' output. Then fill in your story's Testing Strategy section, and the Toolkit blocks for Testing Strategy and Implementation Notes.

Open your PR
9. DONE! Draft your PR: write a title that says what your fix does, and keep it handy for steps 10 and 12. Leave quotes, apostrophes, and backticks out of it, because they break those commands. Then write the description in a new file, pr_draft.md, in the top folder of your clone (the title goes on the command in step 10, not in the file). If the repo has a PR template (look in .github/), copy it in and answer every item. N/A with a reason counts, but deleting an item doesn't. Either way, the description needs four things:

What the fix changes and why.
The issue it fixes, as Fixes #<issue number>.
Your before and after output, and the checks passing, from test_evidence.md.
What AI helped with, in your own words.


10. DONE! Run the ship-gate on your branch: from your clone, on your branch, run:

python3 ~/.claude/ship-gate/check_gate.py --branch --title "<your PR title>" --body-file pr_draft.md
It runs your manifest's checks, then your pr-precheck on your branch, plan, evidence, and description. That takes a few minutes, and each run uses some of your course credit. If the repo takes PRs on a branch other than main, add --base <that branch>. Run it again after each fix until it says ship-gate: PASS. If it stops, here's what to do:

A repo check failed: fix your code. If the check fails on main too, for a reason your fix doesn't touch, move it to your manifest's second table instead. If it says exited 127, the command wasn't found, so rewrite that row the way it runs in a fresh terminal (step 3), like uv run pytest or .venv/bin/pytest.
Your pr-precheck said reject: it lists the checks your branch failed. Fix your branch. If a check asks for something this repo doesn't ask for, fix that check in your rubric instead, and say so in your Unit 7 Toolkit entry (step 14).
It says REFUSAL: your pr-precheck skill still has template files you haven't filled in, and the message names them. Fill them in, then run the ship-gate again.
It names a setup step: do what it says. Section 4 of INSTALL.md in your Unit 7 materials lists every stop.


11. DONE! Push your branch, and point gh at the original repo: in unit 6 you cloned your fork, so origin is your fork. From your clone, run:

git push -u origin <your branch>
gh repo set-default <owner>/<repo>
In the second command, use the original repo's owner and name, not your fork's. For https://github.com/psf/requests, that's psf/requests. Without it, Claude Code opens the PR on your fork, where the maintainers never see it. You only need to run gh repo set-default once per clone.

12. DONE! Open your PR through Claude Code: from your clone, run this with the exact title from step 10 (and the same --base, if you added one):

claude "run this command: gh pr create --title '<your PR title>' --body-file pr_draft.md"
Your voice guard checks the description. If the ship-gate passed in the last 30 minutes and nothing has changed, it lets the PR through without running again. If the voice guard stops it, fix the line it quotes in pr_draft.md and run the command again. Then check the link it prints, which should point at the original repo. If it points at your fork, close that PR on GitHub, run step 11's gh repo set-default command, and open it again.

13. Record your PR in your story: put the PR's link on the Pull Request line at the top of your contribution story, under the Issue line you filled in unit 6. Put it on the PR Link line in the Pull Request section too. Under PR Description, paste or summarize your description, and set Status to Awaiting review. Then fill in the section's Toolkit block, about the ship-gate, and commit it on GitHub.

14. Write your Unit 7 Toolkit entry: in the Toolkit section near the top of your story, find the Unit 7 entry. Keep "Unit 7:" at the start of its heading, and replace the brackets after it with one line on your tool work. Then replace the bracketed paragraph under it with two to four sentences about the ship-gate and your manifest. Say what it said about your branch, what stopped or confused you, and what you changed because of it. Commit it on GitHub.

Not ready to open your PR this week? Keep building, and open it before you submit. If you don't open one, the rest of your story still earns its points.

📦 What you submit
Nothing this week. Next week, you submit your course repo link once, from the unit 5 Assignment tab. Graders read only beat-2-contribution/contribution-story.md, your issue's thread, and your pull request. Your ship-gate, your manifest, and your working files (plan.md, plan-comment.md, test_evidence.md, and pr_draft.md) stay on your computer.

🎯 Where this week's work counts
The full rubric is on the Grading Rubrics page under Course Info. This week's work feeds most of it:

Format (30): the Understanding the Issue fields (step 5) and the PR link in your story's header (step 13).
Reproduction, planning, and testing (50): your Analysis and Implementation Plan (step 5) and your Testing Strategy (step 8).
Your pull request (60): its description, evidence, and diff (steps 9 to 12).
The Toolkit thread (40): the Toolkit blocks from steps 5, 6, 8, and 13, and your Unit 7 entry (step 14).
Learnings, Implementation Notes, and resources (20): your dated Implementation Notes (step 7).