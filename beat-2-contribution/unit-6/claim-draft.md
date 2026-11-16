I'd like to take this one. Could you assign it to me?

First step: read `tests/fixtures/risk/README.md` and the `meta` and `case` row layout in `negation_traps.jsonl`, then write `text_quasi_identifiers_v1.jsonl` and `tests/unit/risk/test_text_qi_fixture.py` in that format. The test will slice each note at its span offsets and check labels with `is_recognized_label`.

All note text will be synthetic and marked `synthetic: true`. I'll comment here once `pytest tests/unit/risk/test_text_qi_fixture.py -q` passes locally, or earlier if the span format needs a decision from you.
