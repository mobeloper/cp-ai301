# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

mobeloper

<!-- [Your GitHub username, exactly as it appears on your profile - no @, no
profile URL. Your comment upstream is identified by this name, and it is
the only thing that ties it to you. Several students may plan the same
house issue, so this is what keeps their comments off your score and
yours off theirs.] -->



**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5986391009

Text of the comment as posted:

> Here is my plan for #72, built from my own reproduction (comment above: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5865096659). 
>
> **Observed:** `verify_password("test-password", "not-a-valid-password-hash")` raises `passlib.exc.UnknownHashError` instead of returning `False`. The H-05 test is `xfail(strict=True)`.
>
> **Diagnosis:** `verify_password()` in `core/security.py` calls `pwd_context.verify(...)` with no exception handling, so passlib's `UnknownHashError` from identifying the hash format reaches the caller. This comes from my repro plus reading the code; I will re-confirm it with a `--runxfail` run before changing anything.
>
> **Change (two files, about 15 lines):**
> - `core/security.py`: catch `UnknownHashError` in `verify_password()` and return `False`.
> - `tests/unit/test_security.py`: remove the `xfail` marker on `test_verify_with_wrong_hash_format` (strict xfail would fail CI once fixed) and add one test with the input from my repro.
>
> **Not touching:** `hash_password`, the JWT functions, the `CryptContext` config, and other exception types. A corrupt hash that looks like bcrypt may raise `ValueError` instead; I have not checked that and will leave it out unless a maintainer wants it covered.
>
> **Test plan:** re-run the repro and the H-05 test; both should return `False` after the fix. `pytest tests/unit/test_security.py` should pass in full so valid hashes still verify, and `make lint` and `make typecheck` should pass.
>
> Open question for review: returning `False` silently makes a corrupt stored hash look like a wrong password. I am not adding logging; say if you want it.


<!-- [Link to the comment where you posted your plan on the issue. Use the comment's own
permalink. **Then paste the text of that comment underneath the link** — the pasted text is
what this field is graded on, so copy across what you actually posted.] -->



---

## Your branch

**Branch**

fix/72-hash-error



<!-- [The name of the branch you built the change on, exactly as it appears in your fork. The
naming shape is a type prefix, then the issue number, then a short description. **The issue
number in the branch name must be the number of the issue you claimed** — a name carrying
any other number does not satisfy this field.] -->


**Evidence**

I re-ran my unit 2 repro against the real code, before and after the fix. The full commands and output are saved in `test-evidence.txt` (kept out of the branch's commits). The fix is commit `0eae38a` on `fix/72-hash-error`. For the "before" I ran the same commands on its parent commit in a temporary checkout, which I removed afterwards. Both runs used the project's `.venv` (Python 3.13.9, passlib 1.7.4, bcrypt 4.3.0).

```text
# Issue #72 test evidence (2026-10-05); env: Python 3.13.9, passlib 1.7.4, bcrypt 4.3.0

## BEFORE the fix (parent commit c457fbd)
$ python -c "from core.security import verify_password; print(verify_password(\"test-password\", \"not-a-valid-password-hash\"))"
           ~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^
  File "/Users/EricMichel/projects/pathreview/.venv/lib/python3.13/site-packages/passlib/context.py", line 1132, in identify_record
    raise exc.UnknownHashError("hash could not be identified")
passlib.exc.UnknownHashError: hash could not be identified

$ pytest tests/unit/test_security.py -k wrong_hash_format --runxfail -q   # marker ignored
        reason="issue #72 (manifest H-05): password verify raises UnknownHashError instead of returning False",
>           raise exc.UnknownHashError("hash could not be identified")
E           passlib.exc.UnknownHashError: hash could not be identified
/Users/EricMichel/projects/pathreview/.venv/lib/python3.13/site-packages/passlib/context.py:1132: UnknownHashError
FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format

$ pytest tests/unit/test_security.py -q   # as shipped, strict xfail in place
-- Docs: https://docs.pytest.org/en/stable/how-to/capture-warnings.html
=========================== short test summary info ============================
XFAIL tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format - issue #72 (manifest H-05): password verify raises UnknownHashError instead of returning False
24 passed, 1 xfailed, 1 warning in 4.14s

## AFTER the fix (commit 0eae38a)
$ python -c "from core.security import verify_password; print(verify_password(\"test-password\", \"not-a-valid-password-hash\"))"
False

$ pytest tests/unit/test_security.py -k "wrong_hash_format or malformed_hash" -v
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format PASSED [ 50%]
tests/unit/test_security.py::TestSecurity::test_verify_password_malformed_hash PASSED [100%]
================= 2 passed, 24 deselected, 1 warning in 0.10s ==================

$ pytest tests/unit/test_security.py -q   # full file

-- Docs: https://docs.pytest.org/en/stable/how-to/capture-warnings.html
26 passed, 1 warning in 4.10s
```

Notes:
- My unit 2 repro was run on Python 3.12.x and CI uses 3.11/3.12; this re-run used 3.13.9, so I will say so in the pull request.
- I have not yet run `make lint` or `make typecheck`, which my plan lists as checks.

<!-- [Your Unit 2 reproduction steps re-run against the built change: the before, then the
after. Paste both, including the commands you ran and their output.] -->




## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Command used for every run, from `eval/`:

`python3 run_eval.py --rubric ~/.claude/skills/plan-check/rubric.md --evidence ~/.claude/skills/plan-check/references/evidence-guide.md --save-run ../eval-run.txt`

**Run 1: 18/20 (bar 18/20: PASS).** wrong-cause 4/4, scope-creep 4/4, unbuildable 3/3, clear-accept 6/7, thread-convention 1/2. Two disagreements with the gold labels:
- pkg-14 (gold accept, my rubric reject): failed `scope`, `changes` and `test`.
- pkg-20 (gold reject, my rubric accept): my rubric had no check that reads the thread or the contribution policy.

Why: pkg-14's plan names an area ("the client attach/reattach path in `zellij-server`") and says the exact functions will be pinned after tracing, and it verifies by re-running the repro rather than adding a test. My rubric required "names each file" and "adds a test". pkg-20's comment has no AI-use disclosure, and ghostty's policy requires all AI usage to be disclosed.

Changes I made before run 2:
- `scope` and `changes`: accept "the specific module, component, or code path" when exact files are not yet known, still requiring a not-touching statement and the reasons.
- `test`: "adds a test" became "a decisive test": an automated test, or a repeatable check from the repro steps with an observable pass result. "Run the full suite" still fails.
- New required `comms` check: the comment engages the maintainer's direction and any claimed effort, and states any AI-use disclosure the policy requires for the comment or plan.
- Matching updates to `procedure.md` (five checks) and `evidence-guide.md` (new examples). Verdict rule: accept only if all five checks pass; `unclear` counts as fail.

**Run 2: 20/20 (bar 18/20: PASS).** Every category matched: clear-accept 7/7, thread-convention 2/2, scope-creep 4/4, unbuildable 3/3, wrong-cause 4/4. pkg-14 and pkg-20 now match the gold labels, and no other package changed.

**Run 3: 20/20 (bar 18/20: PASS).** Same rubric, evidence guide and procedure, run again to check for variation since the grader is a model. Same result. This is the run saved in `eval-run.txt`.

<!-- [The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.] -->

**Package analysis**

**pkg-14** (zellij-org/zellij#5174, category clear-accept). The gold label is **accept** (note: "honestly scoped-down: reattach handshake fix with a regression-window repro; defers the untestable Windows variant and says so; arguable on the deferral, ready as scoped").

In run 1 my rubric decided **reject**, failing `scope`, `changes` and `test`. It read the package that way because my rubric said a plan must name "each file" and "adds at least one test". The plan names the reattach handshake on Unix clients in `zellij-server`'s client connection handling and `zellij-client`'s terminal query issuance, says "exact functions to be pinned in the PR after tracing", and verifies with the repro loop ("5 consecutive SSH reattach cycles with no rgb strings") instead of a new test. Terminal and SSH behaviour is hard to unit test. The plan is bounded and honest, and my rubric was too literal.

After I changed `scope`/`changes` to accept a bounded area and `test` to accept a repeatable repro-based check with an observable result, run 2 and run 3 both graded pkg-14 **accept**, matching the gold label.

<!-- [Pick one scored package (`pkg-01` through `pkg-20` — the four `calib-` packages are never
scored). Name it by id, say what your rubric decided and what the gold label said, and
explain why your rubric read it that way.] -->



**Check rationale**

| scope | The plan's in-scope / not-in-scope statements (`### Scope`, or `In:` / `Out:`) and the files or areas it names. See "Scope" in `references/evidence-guide.md`. | Passes if the plan names each file it will change, or, where exact files are not yet known, the specific module, component, or code path (a bounded area a stranger could locate; an honest "exact functions to be pinned after tracing" is fine); says what it will not touch; and gives the reason for the changes (what they are for). Fails if the change location is unnamed or unbounded ("the codebase", "the parser"), there is no not-touching statement, or no reasoning is given. | required |

It reads that way because run 1 graded pkg-14 reject: my first version said "Passes if the plan names every file it will change", and pkg-14 names a bounded area (the reattach handshake in `zellij-server`'s client connection handling) with the exact functions to be pinned after tracing. Gold says accept. I replaced "names every file" with "each file, or the specific module, component, or code path" so a bounded area counts. I kept the required not-touching statement and the reasons, so "the codebase" or "the parser" still fails. I rejected just dropping the file requirement, because that would let a vague plan with no change location through.

<!-- [Quote one check from the `rubric.md` you uploaded to `tools/plan-check/`, exactly as it reads now.
Then say why it reads that way — what you revised to get there, or what you rejected in
favour of it.] -->


**Trade-offs**

The `test` check changed the result of pkg-14. Run 1 required "adds at least one test (a new or extended test case, naming where it goes)" and graded pkg-14 reject (gold accept). Run 2 reads "a decisive test: an automated test, OR, when automation is not practical, a repeatable check taken from the repro steps with an observable pass result", and pkg-14 is now accept, because its plan re-runs five SSH reattach cycles and expects no `rgb:` strings.

What the looser `test` check gives up: it will let through a plan whose only test is a manual repro re-run, even though a reviewer might prefer an automated test. A weak manual check can still pass if it names an output that looks observable but would not actually differ on the unfixed code. The only guard is the "would differ on the unfixed code" wording, and calib-04 ("run the full test suite") is the case that shows it still fails the vague kind.

The new `comms` check changed pkg-20 from accept to reject, which matches the gold label, because its comment has no AI-use disclosure and ghostty's policy requires disclosing all AI usage. What it gives up: it treats every plan as AI-assisted, so it would also reject a plan written entirely by hand if the policy asks for disclosure. It passes pkg-09, whose policy asks for disclosure only in the pull request, so it is not just "always demand a disclosure line".

Nothing else changed: the other 18 scored packages had the same verdict in run 1 and run 2 (all matched the gold labels), so the edits did not move any other package.

<!-- [Every check gives something up. Any one of these is a complete answer: a package whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.] -->


---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
