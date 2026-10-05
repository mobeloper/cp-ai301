# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis | The plan's `### Diagnosis` read against the issue title and body and the `## Repro evidence` block (live: the student's posted repro comment and the issue thread). See "Diagnosis and grounding" in `references/evidence-guide.md`. | Passes if the plan states what causes the bug AND cites evidence supporting that cause (a repro step, output, traceback, or code location), AND the diagnosis is relevant to the issue title (it explains the behavior the title names). Fails if no cause is stated, the cause is asserted with no supporting evidence, the cause contradicts a fact in the repro evidence, or it addresses a different problem than the issue title. | required |
| scope | The plan's `### Scope` (in scope / not in scope lines) and `### Files` list. See "Scope" in `references/evidence-guide.md`. | Passes if the plan names every file it will change, says what it will not touch, and gives the reason for the changes (what each change is for). Fails if any changed file is unnamed, there is no not-touching statement, or no reasoning is given. | required |
| changes | The plan's `### Files` and `### Approach` sections: each file named, why it is modified, and any file added or deleted. See "Executability" in `references/evidence-guide.md`. | Passes if the plan says which files are modified and why for each; any added or deleted file is explicitly justified; and the change is minimal (the described work plausibly stays under 500 lines changed). Fails if a file has no stated reason, an addition or deletion is unjustified, or the described change is a large rewrite (over 500 lines). | required |
| test | The plan's `### Test plan` and `### Files`/`### Approach` mentions of tests. See "Test plan" in `references/evidence-guide.md`. | Passes if the plan adds at least one test (a new or extended test case, naming where it goes). Fails if it only says to run existing tests or verify manually, or names no test. | required |

## Verdict rule

Ready (`accept`) if every required check passes. If any required check
fails, the verdict is `reject`. `unclear` counts as fail. There are no
preferred checks, so nothing else changes the verdict.
