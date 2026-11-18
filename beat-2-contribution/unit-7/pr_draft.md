# Pull Request

## Description
Adds a small synthetic fixture with two releases of one population, generalized differently, so later composition tests have a known example, and a test that checks each release alone with `kanon_report`. Neither release isolates a unit by itself, but joining them on the privacy-unit key does. `kanon_report` already measures one release at a time, so no library code changes.

Fixes #5295

## Type of Change
- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [x] Documentation update
- [ ] Code refactoring
- [ ] Performance improvement
- [x] Test addition/improvement

## Changes Made
- `tests/fixtures/risk/successive_releases_v1.jsonl` (new): a `meta` row and two releases of 24 synthetic rows each. Every row has `synthetic: true`, a `release` label, an opaque `privacy_unit_id` shared by both releases, and the quasi-identifiers `age_band` and `area`. Release 1 uses ten-year age bands and a town, release 2 uses five-year age bands and a district. All values are invented, with no real names, dates or places. `meta.expected` commits the per-release k and the intersection result.
- `tests/unit/risk/test_successive_release_fixture.py` (new): checks every row is synthetic and each release has 20-40 rows, then runs `kanon_report` on each release alone, with `privacy_unit_id` left out of the quasi-identifiers, and asserts the committed k (3 for `release_1`, 2 for `release_2`).
- `tests/fixtures/risk/README.md`: documents the row format and the expected intersection result for later composition tests. Joining the releases on `privacy_unit_id` and combining both releases' quasi-identifiers gives k 1, with `pu-03`, `pu-04` and `pu-21` each alone in their class.

## Testing
- [x] I have added tests that prove my fix is effective or that my feature works
- [ ] New and existing unit tests pass locally with my changes
  - Not fully: the 3 new tests and all 543 tests in `tests/unit/risk` pass, but the full suite has 6 failures in 2 unrelated tests that also fail on `master` (see Notes for Reviewers).
- [ ] I have tested this change with different models/inputs
  - N/A: the change is an offline fixture and a test, with no model or input handling.

Before, on `master` (`e8325586e`):

```text
$ ls tests/fixtures/risk/successive_releases_v1.jsonl tests/unit/risk/test_successive_release_fixture.py
ls: tests/fixtures/risk/successive_releases_v1.jsonl: No such file or directory
ls: tests/unit/risk/test_successive_release_fixture.py: No such file or directory

$ .venv/bin/python -m pytest tests/unit/risk/test_successive_release_fixture.py -q
ERROR: file or directory not found: tests/unit/risk/test_successive_release_fixture.py
no tests ran in 0.07s
```

After, on this branch, in the environment CONTRIBUTING asks for (`uv sync --frozen --extra dev`):

```text
$ uv run --frozen --extra dev pytest tests/unit/risk/test_successive_release_fixture.py -q
...                                                                      [100%]
3 passed in 0.16s

$ uv run --frozen --extra dev pytest tests/unit/risk -q
543 passed, 308 subtests passed in 2.81s

$ uv run --frozen --extra dev pytest tests/ -q
6 failed, 35040 passed, 405 skipped, 4 xfailed, 61 warnings, 11711 subtests passed in 816.01s (0:13:36)

$ make format
uv run --frozen --extra dev ruff check --fix .
All checks passed!
uv run --frozen --extra dev ruff format .
3623 files left unchanged

$ make lint
uv run --frozen --extra dev ruff check .
All checks passed!
uv run --frozen --extra dev python scripts/security/check_effect_paths.py --check
effect_paths_verified

$ make format-check
uv run --frozen --extra dev ruff format --check .
3623 files already formatted

$ make type-check
uv run --frozen --extra dev mypy
Success: no issues found in 4 source files

$ python3 scripts/release/check_repo_policy.py --commits origin/master HEAD
Commit attribution policy passed: 3 commits checked
```

Measured values, from `kanon_report` on each release alone and a throwaway join on `privacy_unit_id` that is not committed:

```text
release_1 24 rows  k 3  classes 8  sizes [3, 3, 3, 3, 3, 3, 3, 3]
release_2 24 rows  k 2  classes 8  sizes [3, 3, 3, 3, 3, 3, 2, 4]
intersection k 1  isolated ['pu-03', 'pu-04', 'pu-21']
```

With one release-1 row moved into a one-row area, the k check fails with `assert 1 == 3`.

## Documentation
- [x] I have updated the documentation accordingly
  - The fixture format and its expected intersection result are in `tests/fixtures/risk/README.md`.
- [ ] I have added docstrings to new functions/classes
  - N/A: the new code is two test functions and a loader helper; the test module has a docstring.
- [ ] I have updated the CHANGELOG.md
  - N/A: no behavior changes, and the earlier risk fixture PR (#351) did not add an entry.

## Code Quality
- [x] I ran `make format`, `make lint`, and `make format-check`
  - All three pass with output above, and `make format` changed no files. I also ran `make type-check`, which passes.
- [ ] For Swift/OpenMedKit changes, I ran `make format-swift` and `make lint-swift`
  - N/A: no Swift or OpenMedKit changes.
- [x] I have performed a self-review of my own code
- [ ] I have commented my code, particularly in hard-to-understand areas
  - N/A: the new test code is a loader and two short assertions, with nothing hard to follow, so it has a module docstring and no inline comments.
- [x] My changes generate no new warnings

## Dependencies
- [x] I have not added any new dependencies
- [ ] OR I have added new dependencies and they are justified because: N/A

## Checklist
- [x] I have read the [contributing guide](https://github.com/maziyarpanahi/openmed/blob/master/CONTRIBUTING.md), [Code of Conduct](https://github.com/maziyarpanahi/openmed/blob/master/CODE_OF_CONDUCT.md), and [maintainer guide](https://github.com/maziyarpanahi/openmed/blob/master/MAINTAINERS.md)
- [x] My commits have clear, descriptive messages
- [x] I have squashed/organized my commits appropriately
  - Three commits: the fixture, the test, then the README.

## Related Issues
Fixes #5295

## Screenshots/Examples
N/A: no UI or docs-site change. The measured values are above.

## Notes for Reviewers
- Full suite: `pytest tests/ -q` in the locked environment shows 6 failures in 2 tests, neither in `tests/unit/risk` and neither touching this change: `tests/fuzz/test_deidentify_fuzz.py::test_malformed_iscii_preserves_clinical_text_and_identifier_spans` (4 subtest failures, "masked clinical text mismatch") and `tests/unit/structured/test_decision_server.py::test_local_media_is_scrubbed_and_sent_only_to_loopback[video]` (a `DecisionMediaError` on the video case, which uses a local ffmpeg). I ran the same two tests on `master` (`e8325586e`) in the same environment and the same 6 failed, so they fail without my commits. I did not fix them: they are outside this issue.
- Left out on purpose, per the issue's boundary: composition scoring. The test checks per-release k only. The intersection result is documented in the README and `meta.expected` and is not asserted in code.
- AI-use statement: not included, following this repo's `AGENTS.md`: "Do not include assistant/tool/vendor names, co-author trailers, generation footers, or worker attribution in issue text, branch names, commit messages, PR titles, PR bodies, or labels."
- Nothing under `openmed/risk/` changes.
- Small differences from my plan: the test file is 54 lines, the `meta` row also carries `quasi_identifiers` and `privacy_unit_column` so the test reads the column names from the fixture, and the README paragraph is 24 lines. The scope did not change.
