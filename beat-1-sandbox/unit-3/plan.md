# Plan: issue #63, README scorer test fixture is too short

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/63
My reproduction: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/63#issuecomment-5988839576
Base: `main` at `2f4e82f52efbcfcc57d65b3fa5348672163ca088`
Branch (on my fork `batoula1/pathreview-ai301-fa26-s3`): `fix/63-readme-scorer-fixture-length`

## Reproduction evidence this plan builds on

From my posted report (macOS 14.6.1, Python 3.13.7, pytest 9.1.1, clean
checkout of the commit above):

```
$ .venv/bin/python -m pytest tests/unit/test_readme_scorer.py -q
x......................                                                  [100%]
22 passed, 1 xfailed in 0.85s
```

```
$ .venv/bin/python -m pytest "tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals" --runxfail -vv --tb=short
tests/unit/test_readme_scorer.py:60: in test_readme_with_all_quality_signals
    assert data["word_count"] > 100
E   assert 51 > 100
----------------------------- Captured stdout call -----------------------------
... [info     ] readme_scored                  category=minimal score=0.8717142857142858 word_count=51
FAILED tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals - assert 51 > 100
```

The report also said, from reading the code: "the scorer counts words
with `len(content.split())` and calls fewer than 100 words `"minimal"`,
100–499 `"adequate"`, and 500 or more `"comprehensive"`."

## Diagnosis

The fixture is wrong, not the scorer. In
`agent/tools/readme_scorer.py`, `_score_readme` sets
`word_count = len(content.split())` (line 65) and categorizes it at
lines 68-73: `< 100` is `"minimal"`, `< 500` is `"adequate"`, anything
else is `"comprehensive"`. The fixture in
`test_readme_with_all_quality_signals` has 51 words, so the scorer's
`word_count=51` and `category=minimal` in my log are correct output
for that input.

The test then asserts two things the fixture cannot meet:
`data["word_count"] > 100` (line 60, where my run stopped) and
`data["word_count_category"] == "comprehensive"` (line 61, never
reached). The second one needs 500 or more words. So a fixture just
over 100 words would pass line 60 and then fail line 61 with
`"adequate"`.

Before writing this plan, Claude Code ran a read-only check on main
for me (no files changed). It pulled the fixture string out of the test
file with `ast` and scored it, then scored plain text at the
boundaries:

```
$ .venv/bin/python - <<'EOF'
import ast, pathlib
from agent.tools.readme_scorer import ReadmeScorer
src = pathlib.Path("tests/unit/test_readme_scorer.py").read_text()
for node in ast.walk(ast.parse(src)):
    if isinstance(node, ast.FunctionDef) and node.name == "test_readme_with_all_quality_signals":
        readme = node.body[1].value.value
d = ReadmeScorer().execute({"readme_content": readme}).data
print("fixture:", d["word_count"], d["word_count_category"], round(d["overall_score"], 4))
for n in (99, 100, 499, 500):
    print(n, ReadmeScorer().execute({"readme_content": "word " * n}).data["word_count_category"])
EOF
fixture: 51 minimal 0.8717
99 minimal
100 adequate
499 adequate
500 comprehensive
```

(structlog's `readme_scored` lines are trimmed from the output above.)

## Why grow the fixture instead of changing the assertions

The issue allows either "Extend the fixture (or correct the
assertion)". Keeping the 51-word fixture would mean asserting
`word_count < 100` and `"minimal"`, which contradicts what this test is
for (its docstring: "README with all quality signals returns high
score"). Any assertion that keeps `> 100` still needs a longer fixture.
Since the fixture has to grow either way, growing it to 500+ words is
the same size of edit and keeps every existing assertion as written.

## Scope

In scope, one file, one test: `tests/unit/test_readme_scorer.py`,
`test_readme_with_all_quality_signals` only.

Not in scope: `agent/tools/readme_scorer.py` (thresholds, word-count
method, section regexes), the other 22 tests in the file, any other
test file, or lint or type baseline suppressions.

## Approach

1. Create the branch `fix/63-readme-scorer-fixture-length` from `main`
   on my fork.
2. In `test_readme_with_all_quality_signals`, extend the inline
   `readme` string to about 600 words of realistic README text (a
   longer project description, more installation and usage detail,
   configuration, contributing, and license sections). Aim for about
   600 so a later small edit cannot drop it under 500. Keep every
   existing signal the test checks: the `## Installation` and
   `## Usage` headings, the two badge images, the `## Tech Stack`
   section, and the `## Live Demo` / "Try it here" link. No repeated
   filler words.
3. Leave every assertion in the test unchanged, including
   `word_count > 100`, `"comprehensive"`, and `overall_score > 0.7`.
4. Delete the `@pytest.mark.xfail(strict=True, reason="issue #63: ...")`
   decorator from this test. `docs/CONTRIBUTING.md` says removing the
   marker is part of fixing a seeded bug, because a passing strict
   xfail test fails CI with `XPASS(strict)`.
5. Commit with the repo's convention, for example
   `test(agent): extend README scorer fixture to match its comprehensive assertion`
   with `Fixes #63` in the body.

## Test plan

All commands run from the repo root with the project venv.

1. Before the change (the repro, already run on main):
   `.venv/bin/python -m pytest "tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals" --runxfail -vv --tb=short`
   Expected: `FAILED ... assert 51 > 100`, log shows `word_count=51`,
   `category=minimal`. This is the "before" I will paste next to the
   "after".
2. After step 2 of the approach, with the marker still in place:
   `.venv/bin/python -m pytest "tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals" -rX -vv`
   Expected: the test is reported `XPASS(strict)` and counted as
   failed. That shows the new fixture now meets every assertion, and
   that the marker must go.
3. After removing the marker:
   `.venv/bin/python -m pytest "tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals" -vv -rP`
   Expected: `1 passed`, and the captured log line shows
   `category=comprehensive` with `word_count` of 500 or more.
4. The rest of the file:
   `.venv/bin/python -m pytest tests/unit/test_readme_scorer.py -q`
   Expected: `23 passed`, no `xfailed` (my baseline was
   `22 passed, 1 xfailed`).
5. The repo's required checks, per `docs/CONTRIBUTING.md`:
   `make check && make test-unit`
   Expected: both exit 0. I have not run these on main yet, so if
   either fails before my change I will record that baseline and say
   so in the PR rather than fix unrelated findings.
6. On the PR: all five CI jobs (`lint`, `typecheck`, `test-unit`,
   `test-integration`, `frontend`) green, as `docs/CONTRIBUTING.md`
   requires.

## Risks

- The fixture must stay at 500 words or more as counted by
  `content.split()`. Markdown symbols such as `##`, `-`, and code
  fences count as words too. I will check the count from the step 3
  log line, not estimate it.
- Adding text cannot turn any signal from true to false, because every
  signal is a positive regex search. `overall_score` only rises, since
  the word-count component reaches its 1.0 cap at 500 words.
- Line length is not a lint risk here: `pyproject.toml` ignores E501
  under `tests/**`.

## Unknowns

- A maintainer may prefer a shorter fixture with a different
  assertion. I picked the longer fixture for the reason above, and I
  will switch if a maintainer asks.
- I have not yet run `make check` (ruff, black, mypy) or the full
  `make test-unit` suite on this machine, so I do not know their
  baseline on Python 3.13.7.
- Several classmates have posted plans on this issue. This plan is
  built from my own reproduction and code reading; the course house
  rules say a shared issue does not block it.

## AI use

Claude Code helped me with this plan: it re-read the test and the
scorer, ran the read-only threshold check above, and drafted this
file. In my reproduction it also set up the environment and ran the
commands, as my posted report says. I reviewed the plan and the
evidence it quotes.

## Deviations

The change itself held: one test in one file, fixture extended, all
assertions unchanged, and the #63 xfail marker removed. What differed
from the plan:

1. **`black --check .` instead of `make check`.** The plan said to run
   `make check`. That target runs `make format`, which is `black .`,
   and that rewrites files across the whole repo. I did not want a
   format pass touching files outside this change, so I ran the same
   three tools without writing anything: `.venv/bin/ruff check .`,
   `.venv/bin/black --check .`, and
   `.venv/bin/mypy api/ core/ ingestion/ rag/ agent/ safety/` (the
   same mypy command `make typecheck` runs). All three were clean
   before and after the change: `All checks passed!`,
   `110 files would be left unchanged.`, and
   `Success: no issues found in 76 source files`. `make test-unit` was
   run as planned.
2. **Baseline recorded before editing.** The plan listed the full
   checks as an unknown on main. I ran them on main before editing, and
   all were green: `make test-unit` gave `375 passed, 53 xfailed`.
   After the change it gave `376 passed, 52 xfailed`. The single
   `1 warning` line is the same before and after.
3. **Fixture size and sections.** The plan said "about 600 words". The
   fixture now counts `word_count=596` (`category=comprehensive`,
   `score=1.0`). I added only new lines: two overview paragraphs,
   setup notes under Installation, usage notes under Usage, a note
   under Live Demo, and four new sections (Configuration, Running the
   Tests, Contributing, License). The plan named configuration,
   contributing, and license; "Running the Tests" is the one extra
   section. No original fixture line was changed or removed.
4. **Results matched the plan's expectations.** With the marker still
   in place the test reported `[XPASS(strict)]` and was counted as
   failed. After removing the marker, the single test gave `1 passed`
   and the file gave `23 passed` (from `22 passed, 1 xfailed`).
5. **Commit message.** The plan suggested
   `test(agent): extend README scorer fixture to match its comprehensive assertion`
   with `Fixes #63` in the body. The commit uses
   `fix: extend README scorer fixture for issue #63` instead, with no
   `Fixes #63` line. It is still a Conventional Commits type, but it
   has no scope, and the PR description will need to link #63.
6. **Not done yet.** The change is committed on
   `fix/63-readme-scorer-fixture-length` as `45797c6`, with only
   `tests/unit/test_readme_scorer.py` in the commit. No pull request
   is open, so CI (plan test step 6) has not run.

Commands, outputs, and exit codes are saved in `baseline-tests.log`
(before editing), `after-fixture-tests.log` (fixture extended, marker
still on), and `final-tests.log` (marker removed).
