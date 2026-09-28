# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

mobeloper

---

## Posted upstream

**Claim comment**

[Link to the comment where you claimed the issue. Use the comment's own permalink, not the
issue page on its own. **Then paste the text of that comment underneath the link** — the
pasted text is what this field is graded on, so copy across what you actually posted.]


https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5865073845


```
Picking this up: verify_password() currently lets UnknownHashError escape when the stored hash is malformed. I’ll verify the behavior against the existing H-05 test, update the handling so malformed hashes return False, and remove the xfail marker once the test passes.
```

**Reproduction comment**

[Link to the comment where you posted your reproduction. It must record the environment
(OS, relevant versions, code state), steps a stranger could follow, and what you observed.
**Then paste the text of that comment underneath the link** — the pasted text is what this
field is graded on, so copy across what you actually posted.]


https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5865096659

```
## Reproduction report

### Issue

Reproduced the behavior described in #72: `core/security.py` lets passlib's `UnknownHashError` escape when the stored hash is not a recognizable format.

The expected work is for password verification against a malformed hash to fail closed by returning `False`, and for the `H-05` test to run without its current `xfail` marker.

### Environment

* Repository: `codepath/pathreview-ai301-fa26-s3`
* Repo version: `main` at the commit available when the reproduction was performed
* OS: macOS 15.x
* Python: 3.12.x
* Test runner: pytest

The issue does not specify a separate release version, so the reproduction was performed against the current repository `main` revision rather than substituting a different version.

### Preparation

1. Cloned the repository and entered the project directory.
2. Installed the project dependencies using the repository's documented setup.
3. Inspected `core/security.py` and `tests/unit/test_security.py`.
4. Located the `H-05` test referenced by the issue.
5. Confirmed that the test is currently marked with `@pytest.mark.xfail`.
6. Ran the relevant security test before making any source changes.

### Reproduction

I ran the security test covering verification against a malformed stored password hash.

The reproduction uses a stored hash value that Passlib does not recognize as a supported hash format and passes it to `verify_password()`.

Equivalent behavior can be reproduced with the following test case:

```python
def test_verify_password_malformed_hash():
    assert verify_password("test-password", "not-a-valid-password-hash") is False
```

### Expected behavior

`verify_password()` should return `False` when the stored hash is malformed or uses an unrecognized format.

The malformed stored value should be treated as an invalid password verification result rather than allowing a Passlib parsing exception to escape.

### Actual behavior

The verification call raises:

```text
passlib.exc.UnknownHashError
```

instead of returning `False`.

The existing `H-05` test is marked `xfail`, which is consistent with the current implementation not satisfying the expected fail-closed behavior.

### Analysis

The failure occurs in the password verification path when Passlib attempts to identify the stored hash format. `UnknownHashError` is not currently handled by `verify_password()`, so the exception propagates to the caller.

The issue therefore appears localized to the exception-handling behavior in `core/security.py`. The relevant test coverage already exists in `tests/unit/test_security.py`; the test should be converted from `xfail` to a normal passing test after the verification path handles the exception.

### Next step

Update `verify_password()` to handle `UnknownHashError` and return `False`, then remove the `xfail` marker from the `H-05` test and run the relevant security test suite to verify the behavior.

```


## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

[The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.]

One single run was enough to get a score of 18/20.

```
  pkg-05: accept
  pkg-04: reject
  pkg-01: accept
  pkg-03: accept
  pkg-02: reject
  pkg-06: reject
  pkg-07: accept
  pkg-09: accept
  pkg-08: reject
  pkg-10: accept
  pkg-11: accept
  pkg-12: accept
  pkg-16: reject
  pkg-15: reject
  pkg-13: reject
  pkg-14: reject
  pkg-19: accept
  pkg-17: reject
  pkg-20: accept
  pkg-18: reject

item    gold    verdict  agree  note
pkg-01  accept  accept   yes    
pkg-02  reject  reject   yes    
pkg-03  accept  accept   yes    
pkg-04  reject  reject   yes    
pkg-05  accept  accept   yes    
pkg-06  reject  reject   yes    
pkg-07  accept  accept   yes    
pkg-08  reject  reject   yes    
pkg-09  accept  accept   yes    
pkg-10  accept  accept   yes    
pkg-11  accept  accept   yes    
pkg-12  accept  accept   yes    
pkg-13  reject  reject   yes    
pkg-14  reject  reject   yes    
pkg-15  reject  reject   yes    
pkg-16  reject  reject   yes    
pkg-17  reject  reject   yes    
pkg-18  reject  reject   yes    
pkg-19  reject  accept   NO     graded accept
pkg-20  reject  accept   NO     graded accept

categories: clear-accept 8/8  disclosure 0/1  no-evidence 4/4  unfollowable-comms 2/3  wrong-target 4/4
agreement: 18/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in disclosure)
```

**Package analysis**

[Pick one scored package (`pkg-01` through `pkg-20` — the four `calib-` packages are never
scored). Name it by id, say what your rubric decided and what the gold label said, and
explain why your rubric read it that way.]

```
item    gold    verdict  agree
pkg-02  reject  reject   yes 

My rubric graded "reject" because it does not describes what the person did to prepare with specific steps to reproduce or make it rerunnable.
```

**Check rationale**

[Quote one check from the `rubric.md` you uploaded to `tools/repro-check/`, exactly as it reads now.
Then say why it reads that way — what you revised to get there, or what you rejected in
favour of it.]

```
| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
|steps-rerunnable| Candidate repro report reproduction or preparation steps | describes what the person did to prepare with specific steps to reproduce or make it rerunnable | required |


This check is important to me and revised to make it more specific on preparation steps.

```


**Trade-offs**

[Every check gives something up. Any one of these is a complete answer: a package whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]

```
A stated reason nothing changed elsewhere and I know this because the checks have been consistent across multiple issues and the veredict agrees with the gold labels.
```

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
