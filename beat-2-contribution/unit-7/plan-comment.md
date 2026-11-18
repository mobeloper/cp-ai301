The gap is in the test data, not in the code. `kanon_report` in `openmed/risk/kanon.py` measures one table at a time and works, and `tests/fixtures/risk/` has no fixture with two releases of one population, so there is nothing for later composition tests to run against.

I plan to add what the issue lists and nothing in `openmed/risk/`:

- `tests/fixtures/risk/successive_releases_v1.jsonl`: a `meta` row and two releases of 20-40 synthetic rows, with a shared `privacy_unit_id` column, generalized differently in each release. The per-release k is committed in the `meta` row.
- `tests/unit/risk/test_successive_release_fixture.py`: checks every row is marked synthetic and runs `kanon_report` on each release alone, asserting the committed k.
- A paragraph in `tests/fixtures/risk/README.md` describing the format and the expected intersection result.

Composition scoring stays out of scope.
