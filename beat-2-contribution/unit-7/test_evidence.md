# Test evidence: openmed #5295 (successive-release fixture)

Run on 2026-11-17, macOS, Python 3.13.9, `.venv` from `pip install -e ".[dev]"`.
Branch `test/issue-5295-successive-release-fixture` (3 commits on `master`
`e8325586e`, which equals `upstream/master`).

## 1. Reproduction before the fix (`master`, `e8325586e`)

The issue reports a gap, not a crash: the fixture, its README contract and the
per-release test do not exist. Same steps as my story's Reproduction Process.

```text
$ ls tests/fixtures/risk/successive_releases_v1.jsonl tests/unit/risk/test_successive_release_fixture.py
ls: tests/fixtures/risk/successive_releases_v1.jsonl: No such file or directory
ls: tests/unit/risk/test_successive_release_fixture.py: No such file or directory

$ .venv/bin/python -m pytest tests/unit/risk/test_successive_release_fixture.py -q
ERROR: file or directory not found: tests/unit/risk/test_successive_release_fixture.py

no tests ran in 0.07s

$ grep -n -i "successive\|intersect" tests/fixtures/risk/README.md
(no output)

$ .venv/bin/python -m pytest tests/unit/risk/test_regression_suites.py tests/unit/risk/test_kanon.py -q
........uuu.....................uuuu.uu.....                             [100%]
35 passed, 9 subtests passed in 1.30s
```

A repo-wide `grep -rIl "successive_release"` on `master` matched only my
untracked local files (`CLAUDE.local.md`, `plan.md`, `plan-comment.md`) and
the pytest cache, and no tracked file.

## 2. Reproduction after the fix (branch, `8be36858a`)

```text
$ ls tests/fixtures/risk/successive_releases_v1.jsonl tests/unit/risk/test_successive_release_fixture.py
tests/fixtures/risk/successive_releases_v1.jsonl
tests/unit/risk/test_successive_release_fixture.py

$ .venv/bin/python -m pytest tests/unit/risk/test_successive_release_fixture.py -q
...                                                                      [100%]
3 passed in 0.13s

$ grep -n -i "successive\|intersect" tests/fixtures/risk/README.md
25:Record rows for `successive_releases_v1.jsonl` contain:
42:Expected intersection result for later composition tests: joining the two
46:`successive_releases_v1.jsonl` does not score composition; it only fixes the

$ .venv/bin/python -m pytest tests/unit/risk/test_regression_suites.py tests/unit/risk/test_kanon.py -q
........uuu.....................uuuu.uu.....                             [100%]
35 passed, 9 subtests passed in 0.98s
```

The issue's validation command (`.venv/bin/python -m pytest
tests/unit/risk/test_successive_release_fixture.py -q`) passes, and the 35
existing tests still pass.

## 3. The committed values, measured

`kanon_report` run on each release alone with `quasi_identifiers=["age_band",
"area"]`, and a plain join on `privacy_unit_id` (a throwaway script, not
committed) for the intersection:

```text
release_1 24 rows  k 3  classes 8  sizes [3, 3, 3, 3, 3, 3, 3, 3]
release_2 24 rows  k 2  classes 8  sizes [3, 3, 3, 3, 3, 3, 2, 4]
intersection k 1  isolated ['pu-03', 'pu-04', 'pu-21']
```

These match `meta.expected` in the fixture (`k` 3 and 2, intersection `k` 1,
isolated `pu-03`, `pu-04`, `pu-21`) and the README paragraph.

## 4. The test fails when the data is wrong

With one release-1 row changed (`pu-04` moved from `Fernwick` to a one-row
area), the k check fails:

```text
E       assert 1 == 3
FAILED tests/unit/risk/test_successive_release_fixture.py::test_each_release_alone_meets_its_committed_k[release_1]
1 failed, 2 passed in 0.13s
```

With the fixture file removed, all 3 test cases failed with a missing-file
error. I restored the file after both runs; the tree is clean.

## 5. Checks from my ship manifest (branch, `8be36858a`)

```text
$ .venv/bin/ruff check .
All checks passed!

$ .venv/bin/ruff format --check .
3623 files already formatted

$ .venv/bin/python -m pytest tests/unit/risk -q
.............uuuu................uuu................uuuuuu.              [100%]
543 passed, 308 subtests passed in 2.22s

$ python3 scripts/release/check_repo_policy.py --commits origin/master HEAD
Commit attribution policy passed: 3 commits checked

$ .venv/bin/python -m pytest tests/unit/risk/test_successive_release_fixture.py -v
...::test_fixture_is_synthetic_with_two_releases_of_20_to_40_rows PASSED [ 33%]
...::test_each_release_alone_meets_its_committed_k[release_1] PASSED [ 66%]
...::test_each_release_alone_meets_its_committed_k[release_2] PASSED [100%]
3 passed in 0.12s
```

Not run: `.venv/bin/mypy`. It is in my manifest's excluded table because it
fails on `master` before my change (numpy stubs need Python 3.12 syntax in
this `.venv`), and this change adds only a fixture, a test and a README
paragraph.

## 6. Diff

```text
 tests/fixtures/risk/README.md                      | 24 ++++++++++
 tests/fixtures/risk/successive_releases_v1.jsonl   | 49 ++++++++++++++++++++
 tests/unit/risk/test_successive_release_fixture.py | 54 ++++++++++++++++++++++
 3 files changed, 127 insertions(+)
```

Nothing under `openmed/risk/` changed.
