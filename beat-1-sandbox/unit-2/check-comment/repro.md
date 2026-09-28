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
