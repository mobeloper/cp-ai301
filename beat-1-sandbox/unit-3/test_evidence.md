# Test evidence: issue #72, `verify_password` on malformed stored hashes

Captured on branch `fix/72-hash-error` (commit bd57589). "Before" runs use a
checkout of `main` (unfixed `core/security.py`), with the branch's test file
copied in for the second before run.

## Before (unfixed `main`)

H-05 test with the xfail marker ignored:

```
$ pytest tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format --runxfail
>           raise exc.UnknownHashError("hash could not be identified")
E           passlib.exc.UnknownHashError: hash could not be identified
FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format
========================= 1 failed, 1 warning in 0.13s =========================
```

The branch's test file against the unfixed `core/security.py` (both tests fail,
so the new test can detect the bug):

```
$ pytest tests/unit/test_security.py -k "malformed_hash or wrong_hash_format"
E           passlib.exc.UnknownHashError: hash could not be identified
FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format
FAILED tests/unit/test_security.py::TestSecurity::test_verify_password_malformed_hash
================= 2 failed, 24 deselected, 1 warning in 0.14s ==================
```

## After (this branch)

```
$ pytest tests/unit/test_security.py -k "malformed_hash or wrong_hash_format" -v
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format PASSED [ 50%]
tests/unit/test_security.py::TestSecurity::test_verify_password_malformed_hash PASSED [100%]
================= 2 passed, 24 deselected, 1 warning in 0.11s ==================
```

## Repo checks (this branch)

```
$ make test-unit
================== 377 passed, 52 xfailed, 1 warning in 4.63s ==================
exit: 0

$ make test-integration
collecting ... collected 0 items
============================ no tests ran in 0.04s =============================
make: *** [test-integration] Error 5
exit: 2   (Path Review has no integration tests yet)

$ make lint
.venv/bin/ruff check .
All checks passed!
exit: 0

$ make typecheck
.venv/bin/mypy api/ core/ ingestion/ rag/ agent/ safety/
Success: no issues found in 76 source files
exit: 0
```

No `XPASS(strict)` or `failed` lines in the unit run. The 52 xfailed are other
seeded bugs.

## Seeded-bug marker (issue #72 / H-05)

The marker's reason reads `issue #72 (manifest H-05)` with no colon, so
`grep "issue #72:"` does not match it. Searching without the colon:

```
$ git grep -n "issue #72" main -- tests
main:tests/unit/test_security.py:220:        reason="issue #72 (manifest H-05): password verify raises UnknownHashError instead of returning False",

$ git grep -n "issue #72" HEAD -- tests
HEAD:tests/unit/test_security.py:228:        """Test verify_password fails closed on an unrecognized stored hash (issue #72)."""
```

On `main` the marker exists; on the branch only the new test's docstring
mentions #72. The branch's diff deletes it:

```
$ git diff main...HEAD -- tests | grep xfail
-    @pytest.mark.xfail(
-        reason="issue #72 (manifest H-05): password verify raises UnknownHashError instead of returning False",
```

`pyproject.toml` has no suppression naming #72, so there is nothing to remove there.
