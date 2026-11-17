# Repro report: openmed #5295 (no successive-release fixture exists)

Issue: https://github.com/maziyarpanahi/openmed/issues/5295, "Add a synthetic successive-release fixture for composition tests". The issue reports a gap, not a crash: `tests/fixtures/risk/` has QI uniqueness and negation-trap fixtures "but nothing with two releases". I reproduced the gap on current `master`.

## Environment

- macOS 26.5.1 (MacBook Pro), Python 3.13.9. `uv` is not installed, so I used the pip fallback from CONTRIBUTING.md.
- Repo: fork mobeloper/openmed at `master` commit `e8325586e1ce2da73e9e865f34bd0345bb64301a` (2026-11-16), identical to `upstream/master` (`git rev-parse upstream/master` returns the same hash). Package version `openmed` 3.3.0 (`openmed/__about__.py`). The issue's "Release: v4.3" footer is a roadmap milestone, not a version where the behavior was seen. The bug template's Model ID, PyTorch and Transformers fields do not apply to an offline fixture.
- Setup: `python3 -m venv .venv`, `.venv/bin/python -m pip install --upgrade pip`, `.venv/bin/python -m pip install -e ".[dev]"`. It installed with no errors and `import openmed` reports 3.3.0.

## Steps to reproduce

From the repo root, on the commit above:

1. Check that the files the issue asks for are absent:
   `ls tests/fixtures/risk/successive_releases_v1.jsonl tests/unit/risk/test_successive_release_fixture.py`
2. Run the issue's validation command:
   `.venv/bin/python -m pytest tests/unit/risk/test_successive_release_fixture.py -q`
3. Search the repo for any existing successive-release fixture:
   `grep -rIl "successive_release" . --exclude-dir=.venv --exclude-dir=.git` and `grep -n -i "successive\|intersect" tests/fixtures/risk/README.md`
4. Confirm the existing building blocks work: run `kanon_report` on the closest existing fixture, and run the existing risk tests:
   `.venv/bin/python -m pytest tests/unit/risk/test_regression_suites.py tests/unit/risk/test_kanon.py -q`

## Expected behavior

Per the issue's acceptance criteria, the repo should contain `successive_releases_v1.jsonl` (a `meta` row and two releases of 20-40 synthetic rows with a keyed privacy-unit column), a README section describing it and the expected intersection result, and `test_successive_release_fixture.py`, so the pytest command in step 2 passes and asserts each release's k.

## Actual behavior

- Step 1: both files are missing: `No such file or directory` for each.
- Step 2: the pytest command cannot run: `ERROR: file or directory not found: tests/unit/risk/test_successive_release_fixture.py` / `no tests ran in 0.48s`.
- Step 3: no tracked file in the repo matches `successive_release`. The README mentions only "release-gate fixtures", with no successive releases or intersection result. `tests/fixtures/risk/` holds only `dp_budget_scenarios.json`, `negation_traps.jsonl`, `quasi_identifier_uniqueness.jsonl` and `README.md`, and none has a release field.
- Step 4: the building blocks already work, so the gap is only the missing fixture and test. The existing suites pass: `35 passed, 9 subtests passed in 11.96s`. `kanon_report` on `quasi_identifier_uniqueness.jsonl` (4 records, QIs `age`, `city`, `visit_date`) returns `k = 2`, `class_count = 2`: it measures one release at a time, and nothing in the repo supplies a second release to intersect.

## Findings

The issue's description matches the repo today. The missing pieces are the fixture, its README contract and the per-release test, and nothing else is broken. The same `kanon_report` call can measure each release alone, so I do not expect to touch `openmed/risk/`. I have not yet checked the privacy-unit wording in `openmed/risk/population.py`, so the privacy-unit column name is still open.
