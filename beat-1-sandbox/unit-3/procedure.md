# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides the order (issue first? repro evidence first?) and says why
the order matters for the checks that come later. -->

Read the Issue and Repro evidence at the first.
Read the candidate plan, including Diagnosis, Scope, Changes that are planned and then Test plan.
Finally, read the Candidate plan comment.

## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which page or
thread location per your evidence guide) to pull the fact from, and
what to record. A complete procedure leaves no check whose evidence an
executor would have to hunt for. -->

You will gather the evidence from the issue body at the following sections:
Issue - grab the issue title and the task in hand.
Repro evidence - if repro evidence results all show the same output, try to come up with test cases that produce new output which could narrow down the source of the bug
Candidate Plan - Diagnosis, Scope, Changes, Test
Compare the Candidate plan with the issue in hand, the repro evidence for consistency and alignment.


## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->

diagnosis (required): passes if the plan says what causes the bug and also provides the evidence to support the diagnosis in agreement with the issue title (diagnosis is relevant to the issue).
Pass if 
the Diagnosis plan specifies what causes the bug. 
Repro evidence is given. 
If missing, fail the check
scope (required): look where the plan says what it will change; passes if it names each file it will change and says what it won’t touch. Provide reasoning for what the changes are for. 
Pass if
Candidate plan has files that will change
(optional) any files that will not change. 
Reasoning is provided for what the changes are for.
If missing, fail the check. 
changes (required): Which files are you modifying and why. If changes involve adding/deleting, it must justify why adding or deleting. Changes should be minimal (under 500 lines).
Pass if 
the Candidate plan specifies the files being modified. 
If adding/deleting, fail if no justification is given. 
the changes are less than 500 lines
If missing, fail the check. 
test (required): passes if the plan adds a test.
Pass if the Candidate plan has a test provided. 
If missing, fail the check. 


## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check. A
complete procedure produces the same verdict from the same grades,
every time. -->

All required checks should pass. Preferred checks will not change the verdict
