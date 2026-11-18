🛠️ The work, in order
Install the PR monitor
1. DONE! Get your Unit 8 materials: clone your Unit 8 materials (git clone https://github.com/codepath/ai301-unit8-starter.git, or Code, then Download ZIP on the repo page). The folder is ai301-unit8-starter, or ai301-unit8-starter-main if you downloaded the ZIP.

2. DONE! Install the PR monitor: the PR monitor is a program you run from your terminal. It reads your PR on GitHub (its checks, reviews, comments, and how long it's been quiet), then asks Claude to draft your next move into a file. It never posts anything, so every word a maintainer reads still comes from you. From your Unit 8 materials folder, run:

mkdir -p ~/.claude/pr-monitor
cp skill/monitor.py skill/move-forms.md skill/playbook-template.md skill/INSTALL.md ~/.claude/pr-monitor/
cp ~/.claude/pr-monitor/playbook-template.md ~/.claude/pr-monitor/playbook.md
Run the last command only once, because it replaces playbook.md with a blank copy. There's no hook this time and nothing to add to your Claude Code settings, because the monitor only runs when you run it.

3. DONE! Check that it can see your PR: in the clone of your fork, switch to your PR's branch (git switch <your branch>) and run:

python3 ~/.claude/pr-monitor/monitor.py --state
It prints your PR's state: its checks, the review decision, the comments, and its latest activity. This makes no Claude call, so it uses no credit. If it says NO PR YET:

You haven't opened your PR: that's expected. Finish steps 9 to 13 on the unit 7 Assignment tab, then come back.
You've opened your PR (even if it's merged or closed now): read the line that names the repository. If it names your fork, run unit 7's gh repo set-default <owner>/<repo> with the original repo's owner and name. When it works, it prints a line starting with ✓ that names the repo. Then run --state again. If it names the original repo, check that you're on your PR's branch.
If it warns that the PR on this branch isn't yours, you're on the wrong branch, so switch to your PR's branch and run it again.

Any other stop says why and what to do. Section 4 of skill/INSTALL.md in your Unit 8 materials lists every stop.


Write your playbook
4. DONE! Write your playbook: open ~/.claude/pr-monitor/playbook.md. In the activity, Monty's playbook had one rule in each of four sections. Yours has the same four sections, written from what your repo actually does. Under each heading, write one to three rules:

Response rules: what your replies to a reviewer open with, and what they never do.
Staleness thresholds: how many quiet days are normal for your repo before you'd nudge. If the repo has a CONTRIBUTING.md, check it for a stated wait. Then, on the repo's Pull requests page, open the Closed tab and pick five recent PRs marked Merged. Skip PRs that a maintainer or a bot opened, because those usually merge fast. Count how many days each waited for a maintainer's first comment, review, or merge, then pick a number a few days past the usual wait.
Nudge etiquette: what your one nudge says and who it tags. The pings rules in your voice guide's Upstream section, from unit 6, are a good start.
Walk-away criteria: when you'd close your own PR, and what your close note would say.
Keep the four headings as they are, because the monitor finds them by name. Put your name in the title line, delete the instruction blocks between <!-- and -->, and keep the whole file under a page. While a section is empty, the monitor stops before it calls Claude, so a run costs nothing and drafts nothing. --state tells you when all four sections are filled in.

Run it on your PR
5. DONE! Run the monitor: from your clone, on your branch, run python3 ~/.claude/pr-monitor/monitor.py. It reads your PR, makes one Claude call (a few cents, up to about $0.25 on a long thread), and writes two files in ~/.claude/pr-monitor/: run-report.md, your PR's state, and drafted-move.md, the move it drafted. Depending on your PR, the draft is a reply to a reviewer, a rebase plan, a nudge or a note to wait, a thank-you after a merge, or a reply to a maintainer who closed it. If nothing has changed since your last run, it says so and makes no Claude call.

6. DONE! Decide what to do with the draft: read it next to your PR's thread, the way your room read Monty's drafts in the activity. The monitor can be wrong, so you decide what happens.

A reply to a reviewer: if the reviewer asked for changes, make them on your branch first and push them all at once. Then copy the draft's text into a new file, my-reply.md, in the top folder of your clone, and rewrite it until it answers every comment in your own words. Run echo my-reply.md >> .git/info/exclude once, so the file stays out of your commits. Don't post drafted-move.md itself, because it carries notes written to you. From your clone, post your file through Claude Code, so your voice guard checks it. Your PR's URL is on the PR: line that --state prints.

claude "run this command: gh pr comment <PR URL> --body-file my-reply.md"
If the guard stops it, fix the line it quotes and run the command again. Then click the re-request review button beside the reviewer's name under Reviewers on the PR page.

A nudge, or a note to wait: post a nudge only if your own threshold is met, the same way as a reply. A note to wait is for you, so it stays off the thread.

A thank-you after a merge, or a reply to a close: keep it short, with one specific thank-you, and post it the same way as a reply.

A rebase plan: run it yourself, on your branch. If git fetch upstream fails, add the original repo as a remote first: git remote add upstream https://github.com/<owner>/<repo>.git. Before you push, run each command in your ship-manifest.md's first table again. The rebase rewrote your branch, so push with git push --force-with-lease origin <your branch>. Section 6 of skill/INSTALL.md in your Unit 8 materials shows what to do if git stops on a conflict.

Nothing worth sending: don't send it. A bot's welcome or a +1 doesn't need a reply.

Run the monitor again whenever something new lands on your PR.

>> Decision: Nothing worth sending


Finish your story and submit
7. DONE! Update your Pull Request section: in beat-2-contribution/contribution-story.md, replace the two bracketed lines under Maintainer Feedback with dated lines: what came in, and how you answered it. Add another pair for each review round. If nothing has come in, write the date and "none yet" on the first line and delete the second. Then set Status to Awaiting review (waiting on the maintainer), Iterating (making changes a reviewer asked for), Merged, or Closed. The section's Toolkit block, which you wrote about the ship-gate in unit 7, doesn't need to change. Commit it on GitHub.

8. DONE! Write your Unit 8 Toolkit entry: in the Toolkit section near the top of your story, find the Unit 8 entry. Keep "Unit 8:" at the start of its heading, and replace the brackets after it with one line on your tool work. Then replace the bracketed paragraph under it with two to four sentences about the PR monitor and your playbook. Say what it drafted for your PR and what you did with the draft, what fell short or confused you (or that nothing did, and how you know), and what you changed because of it. A draft you decided not to send counts, and so does a rule you rewrote. Commit it on GitHub.

9. DONE! Finish the rest of your story: fill in Learnings and Reflections (all three parts) and Resources Used, and add any missing dated entries to Implementation Notes. Then read the whole story once, top to bottom, with the rubric open on the Grading Rubrics page under Course Info. Check that:

all ten section headers are there under their exact names, from Toolkit to Resources Used
the Issue and Pull Request lines at the top hold your issue's link and your PR's link
every Toolkit block and all four weekly entries, Unit 5 to Unit 8, are filled in
the Tool line under Why I Chose This Issue says how you used your scout, because the template's own wording there earns no points
every bracketed placeholder is replaced or deleted, except Contribution Number's [1] and the example block at the top of the Toolkit section
Commit any fixes on GitHub.


10. DONE! Submit: go to the unit 5 Assignment tab and submit your course repo link. Submit as soon as your story is complete, because you don't have to wait for Sunday. If you revise it, resubmit from the same tab. If you submitted by the deadline, you can keep resubmitting through the last day of the course (Nov 22).

Haven't opened your PR yet? You can still open it before you submit, with steps 9 to 13 on the unit 7 Assignment tab. If you submit without one, the rubric rows that read your PR (up to 66 of the 250 points) score zero, but the rest of your story still earns its points.

📦 What you submit
One thing: your course repo link, from the unit 5 Assignment tab, by Sunday. This tab has no submit button. Graders read only beat-2-contribution/contribution-story.md, your issue's thread, and your pull request. Your PR monitor, your playbook, and its reports and drafts stay on your computer, and you'll use them in unit 9's portfolio.


🎯 Where this week's work counts
The full rubric is on the Grading Rubrics page under Course Info. This week's work finishes it:

The Toolkit thread (40): your Unit 8 entry (step 8). The four weekly entries earn 12 of these points together: 12 with all four filled in, 6 with one missing, and none with two missing.
Learnings, Implementation Notes, and resources (20): step 9.
Format (30): the ten section headers and the two links at the top of your story (step 9).
Your PR is graded as it was when you opened it. Review comments, a merge, a close, or silence never change your score, so a PR that's still waiting for review on Sunday is a complete story.