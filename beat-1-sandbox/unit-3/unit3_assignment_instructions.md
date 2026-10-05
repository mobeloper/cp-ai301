# Assignment 3: Plan and Build

https://courses.codepath.org/courses/ai301/unit/3#!projects

Steps 1-6 build and test your skill, 7-11 plan and build your fix, and 12 submits.


1. DONE! Install your skill: clone your Unit 3 materials (git clone https://github.com/codepath/ai301-unit3-starter.git, or download the ZIP from the repo page). Copy everything inside the clone's skill/ folder into a new folder, ~/.claude/skills/plan-check/, so that SKILL.md ends up at ~/.claude/skills/plan-check/SKILL.md. Do all your editing in this installed copy, so your eval runs and your plan check in step 8 use the same files. Then, in the installed scope.md, replace the placeholder on the Repo: line with your Path Review repo.

Your Path Review repo:
https://github.com/codepath/pathreview-ai301-fa26-s3


2. DONE! Write your rubric: start from "Our rubric" in your group's activity worksheet here: https://docs.google.com/document/d/1c0VLT7XUVGye6P-aafrYJ7bBoBSQ4lhXBTH_LOdgTbo/edit?tab=t.0 . Put each check in a row of rubric.md's table (Check, Evidence, Pass condition, Weight). Under the table, write a verdict rule that says how the checks add up to ready or hold, including what a ? does. The skill writes a ? as unclear, and treats it as a fail if your rule doesn't say. Missed the activity? Start with one check each for the diagnosis, the scope, and the test plan.


3. DONE! Write your procedure: copy the worksheet's four "Our procedure" boxes into procedure.md, one under each heading: "What to read first" under Read order, "How to gather the evidence" under Evidence gathering, "How to grade each check" under Check execution, and "How to reach the verdict" under Verdict assembly. Turn each box into numbered steps Claude can follow without asking what they mean. They're steps for grading a plan, not for making one. Missed the activity? Start from the numbered steps under ## Workflow in your unit 2 skill, ~/.claude/skills/repro-check/SKILL.md, and rewrite each one so it's about grading a plan.


4. DONE! Write your evidence guide: this is a new guide, not your unit 2 one, because a plan's evidence lives in different places. Open ~/.claude/skills/plan-check/references/evidence-guide.md next to a practice submission, such as calib-01.md. They're all in the eval/packages/ folder of your Unit 3 materials, and the eval harness calls them packages. Under each heading in the guide, write where that evidence lives in a practice submission and what good looks like.


5. DONE! Copy in your voice guide: run cp ~/.claude/skills/repro-check/voice-guide.md ~/.claude/skills/plan-check/voice-guide.md, which replaces the template with your unit 2 guide. If plan comments need a rule your claim and repro comments didn't, add it.


6. DONE! Run the eval harness: from the eval/ folder of your Unit 3 materials, run python3 run_eval.py --rubric ~/.claude/skills/plan-check/rubric.md --evidence ~/.claude/skills/plan-check/references/evidence-guide.md --save-run eval-run.txt. This full run grades the 20 scored practice submissions with your files (it finds procedure.md next to your rubric), compares each verdict with the gold labels (the staff answer key), and saves the results to eval-run.txt in that folder. After each full run, revise, re-grade only the ones you disagreed on (Retries below says how), then run in full again. Keep going until 18 of the 20 agree with the gold labels and every category on the output's categories: line has at least one match. The eval-run.txt from your last full run is the file you submit.


Write your plan: work in the top folder of your fork's clone.My fork clone repo is here https://github.com/mobeloper/pathreview/tree/main. Create plan.md there and, working from the repro comment you posted in unit 2, write your diagnosis, your scope (what you'll change and what you won't), the files you'll touch, your approach, your test plan (your unit 2 repro steps re-run, with what you expect to see after the fix), and your risks and unknowns. Quote the repro evidence you rely on, because the skill grades only what your drafts contain. End plan.md with a ## Deviations heading, which you fill in after the build. Then draft your plan comment in comment.md, in the same folder.

Don't have a reproduced issue? Ask a TF in #dts-fa26-ai301-solution-planning on Slack for a house issue and its repro pack (the staff's reproduction of that issue). Use the repro pack wherever this page says your unit 2 repro.

Check your plan with your skill: from the top folder of your fork's clone, run claude "plan-check: grade my plan in plan.md and draft comment in comment.md for issue <URL>", with your issue's URL in place of <URL>. It prints a grade for each check and ends with a JSON block whose verdict is accept (ready) or reject (hold). Revise your drafts and re-run until it says accept. If it stops with a message about scope, your Repo: line still has the placeholder (step 1). If you see neither, the skill didn't run.

Post your plan comment: post the text of comment.md as a comment on your issue's GitHub thread, then copy the comment's link (its ... menu, then Copy link) for step 12. A classmate's plan on the same issue doesn't block yours, so post your own, built from your own reproduction. "Same approach as above" doesn't count as a plan.

Build the change: in your fork's clone, create a branch named with a type prefix (fix/, docs/, feat/, test/, refactor/, perf/, or chore/), your issue number, and a short description, for example fix/1234-null-check. Work through the plan in small steps. Claude makes each edit, and you read every diff before you keep it. Keep plan.md and comment.md out of your commits (check git status before each one), because this branch becomes your pull request in unit 4. If the build ends up different from the plan, write what changed and why under ## Deviations, re-run step 8, and add a comment on the issue if your posted plan is no longer true. If nothing changed, write that under Deviations in your own words.

Run your test plan: re-run your unit 2 repro steps against the change, and save the commands and output from before and after the fix. The before can be the output you posted in unit 2. If your repro can't run against the real change (for example, it was a stand-in script that copied the bug), turn its inputs into a check that runs through the real code, and save that check's before and after instead.