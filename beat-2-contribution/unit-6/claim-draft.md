Hi @maziyarpanahi! I'd like to take this one. Could you assign it to me? (You offered this issue on #5296's thread)

First step: read `tests/fixtures/risk/README.md`, `quasi_identifier_uniqueness.jsonl` and `kanon_report` in `openmed/risk/kanon.py`, then write `successive_releases_v1.jsonl` (a `meta` row and two releases of 20-40 rows each, with a keyed privacy-unit column) and `tests/unit/risk/test_successive_release_fixture.py`. The test will run `kanon_report` on each release alone and assert the committed k values. I'll document the format and the expected intersection result in the README, and leave composition scoring out.

All rows will be synthetic and marked as such, with no real names, dates or places. I'll comment here once `.venv/bin/python -m pytest tests/unit/risk/test_successive_release_fixture.py -q` passes locally, or earlier if the fixture format needs a decision from you.
