fix: return False from verify_password on malformed stored hash

## Summary
`verify_password()` in `core/security.py` called `pwd_context.verify()` with no
exception handling, so a stored hash that passlib cannot identify raised
`passlib.exc.UnknownHashError` to the caller instead of returning `False`. It
now catches `UnknownHashError` and returns `False`, so a malformed or
unrecognized stored hash fails closed, the same way `decode_access_token()`
already fails closed on a bad token. The seeded-bug `xfail` marker on the H-05
test is removed, and one regression test covers the input from my repro.

## Issue
Closes #72

## Changes
- `core/security.py`: import `UnknownHashError` from `passlib.exc`, wrap the
  `pwd_context.verify()` call in `try`/`except UnknownHashError: return False`,
  and update the docstring "Returns" line to say it returns `False` for an
  unrecognized stored hash.
- `tests/unit/test_security.py`: remove the `@pytest.mark.xfail(strict=True, ...)`
  marker from `test_verify_with_wrong_hash_format`.
- `tests/unit/test_security.py`: add `test_verify_password_malformed_hash`,
  which asserts `verify_password("test-password", "not-a-valid-password-hash") is False`.

## Testing
- [ ] **CI is green on this PR (all five jobs)** — required for review
- [x] Unit tests pass (`make test-unit`)
- [ ] Integration tests pass (`make test-integration`)
- [x] Linter passes (`make lint`)
- [x] Type checker passes (`make typecheck`)
- [x] New/updated tests cover the changes
- [x] If this fixes a seeded bug: removed its `@pytest.mark.xfail` marker (and any
      matching suppression in `pyproject.toml`)

**Before (unfixed `main`):**
```
$ pytest tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format --runxfail
E           passlib.exc.UnknownHashError: hash could not be identified
FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format
========================= 1 failed, 1 warning in 0.13s =========================
```
With this branch's test file on the unfixed `core/security.py`, the new test
fails the same way:
```
FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format
FAILED tests/unit/test_security.py::TestSecurity::test_verify_password_malformed_hash
================= 2 failed, 24 deselected, 1 warning in 0.14s ==================
```

**Seeded-bug marker:** the diff deletes the `@pytest.mark.xfail(strict=True, ...)`
block whose reason reads `issue #72 (manifest H-05)`. `pyproject.toml` has no
suppression for #72.

**After (this branch):**
- Both tests above pass: `2 passed, 24 deselected, 1 warning in 0.11s`.
- `make test-unit`: 377 passed, 52 xfailed, 1 warning (the pydantic
  deprecation warning in `core/config.py`), with no `XPASS(strict)` failures.
  The fix's new test, `test_verify_password_malformed_hash`, is among the
  passes.
- `make test-integration`: no tests ran. Path Review has no integration tests
  yet, so the box is unticked. Output as is:
  `collected 0 items`, `no tests ran in 0.05s`,
  `make: *** [test-integration] Error 5`.
- `make lint`: `ruff check .` reported "All checks passed!".
- `make typecheck`: mypy reported "Success: no issues found in 76 source files".
- CI is unticked because it only runs after the PR is open.

## Screenshots / Demo
N/A: this is a backend logic change with no UI, so there is nothing to show
beyond the test output above.

## Notes for Reviewers
**AI use:** I used Claude Code to help write my plan, check my drafts against
a rubric, and review this description against my diff. I read the diff and ran
the commands myself, and I am responsible for the change.

**Differences from my plan:** the code change matches the plan (same two files,
same `UnknownHashError` catch, marker removed, one new test). The only
difference is the branch name, `fix/72-hash-error` instead of the planned
`fix/72-verify-password-malformed-hash`, renamed to fit the
`fix/<issue-number>-<slug>` rule. This is recorded under Deviations in my plan.

**Left out on purpose, for your call:**
- A hash that looks like bcrypt but is corrupt raises a different error. On the
  fixed code, `verify_password("password", "$2b$12$abc")` still raises
  `ValueError: salt too small (bcrypt requires exactly 22 chars)`, while
  `"not-a-valid-password-hash"` and `""` return `False`. The issue names only
  `UnknownHashError`, so I did not widen the `except`. Should it also catch
  `ValueError`?
- Returning `False` silently makes a corrupt stored hash look like a wrong
  password. I did not add logging because the issue does not ask for it and
  `decode_access_token()` does not log either. Say if you want it.
