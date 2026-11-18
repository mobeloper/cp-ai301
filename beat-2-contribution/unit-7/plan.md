## Solution Approach

**Analysis**

The cause is a coverage gap, not a defect in code. `kanon_report` in `openmed/risk/kanon.py` takes one table, builds equivalence classes from the quasi-identifiers (`_equivalence_key`) and returns `k` (smallest class size) and `class_count`. It has no notion of a release, so it measures one release at a time, and my repro showed it works (35 tests pass). What is missing sits in test data: `tests/fixtures/risk/` has `quasi_identifier_uniqueness.jsonl` (one table, 4 records) and `negation_traps.jsonl`, and neither has a release field or a privacy-unit key. `tests/unit/risk/test_regression_suites.py` only loads those two files, and `tests/fixtures/risk/README.md` documents only their formats. So later composition work has no committed example where two releases are each safe alone but isolate rows when intersected.

**Proposed Solution**

Add exactly what issue #5295 asks for and change no library code.

- `tests/fixtures/risk/successive_releases_v1.jsonl`: a `meta` row (same shape as the existing fixtures, with the committed per-release k in `expected`) and two releases of one synthetic population, 20-40 rows each, plus a keyed privacy-unit column (`privacy_unit_id`) shared by both releases. The two releases generalize the quasi-identifiers differently. Every row is marked `synthetic: true` and holds invented values only: no real names, dates or places.
- `tests/unit/risk/test_successive_release_fixture.py` (about 40 lines): loads the fixture, checks every row is marked synthetic, runs `kanon_report` on each release alone with explicit `quasi_identifiers` (so the ID column stays out), and asserts the committed k for each.
- `tests/fixtures/risk/README.md`: a paragraph describing the format and the expected intersection result for later composition tests.

I am not changing `openmed/risk/` (composition scoring is out of scope per the issue), the existing fixtures, or the existing tests, and I am not adding anything the issue does not list: no extra test helpers, no CHANGELOG entry, no docs beyond the README paragraph.

**Implementation Plan**

1. Branch from the latest `master`: `test/issue-5295-successive-release-fixture`.
2. Design the population on paper: about 24 units, each with a `privacy_unit_id`. Release 1 and release 2 generalize the quasi-identifiers differently, so each release alone has k of at least 2 and the intersection leaves at least one unit alone in its class. Confirm the k values and the intersection with `kanon_report` and a throwaway script I do not commit.
3. Write the fixture with the per-release k in `meta.expected`.
4. Write the test file.
5. Add the README paragraph, including the expected intersection result.
6. Run `.venv/bin/python -m pytest tests/unit/risk/test_successive_release_fixture.py -q`, then my manifest checks (`ruff check`, `ruff format --check`, `pytest tests/unit/risk -q`) and `python scripts/release/check_repo_policy.py --commits origin/master HEAD`.
7. Commit with imperative `test:` and `docs:` messages, then open one PR with `Closes #5295`, following `.github/PULL_REQUEST_TEMPLATE.md` and listing the exact validation commands.

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]
