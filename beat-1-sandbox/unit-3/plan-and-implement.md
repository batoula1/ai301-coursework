# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

batoula1

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/63#issuecomment-6112683403

Posted text, copied exactly as it appears on the issue:

````markdown
## Plan

Following up on my reproduction above (https://github.com/codepath/pathreview-ai301-fa26-s3/issues/63#issuecomment-5988839576). With `--runxfail`, the test stopped at:

```
tests/unit/test_readme_scorer.py:60: in test_readme_with_all_quality_signals
    assert data["word_count"] > 100
E   assert 51 > 100
... readme_scored  category=minimal score=0.8717142857142858 word_count=51
```

**Diagnosis:** I think the fixture is the problem, not the scorer. `agent/tools/readme_scorer.py` counts words with `len(content.split())` and only returns `"comprehensive"` at 500 words or more (lines 68-73). Claude Code ran a read-only check on main for me, with no files changed. It scored the fixture string from the test and plain `"word " * n` text at the boundaries (log lines trimmed):

<details><summary>Script</summary>

```python
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
```

</details>

```
fixture: 51 minimal 0.8717
99 minimal
100 adequate
499 adequate
500 comprehensive
```

The test asserts both `word_count > 100` and `"comprehensive"`, so a fixture just over 100 words would still fail the second assertion.

**Change (one file, one test):** in `tests/unit/test_readme_scorer.py`, `test_readme_with_all_quality_signals` only:
1. Extend the inline README fixture to about 600 words of real README sections, keeping the Installation, Usage, Tech Stack, badge, and Live Demo signals.
2. Leave every assertion as it is.
3. Remove the test's `xfail(strict=True)` marker for #63, as `docs/CONTRIBUTING.md` asks.

I chose a longer fixture over changing the assertions because the fixture has to grow past 100 words either way, and the test is meant to be a README with every quality signal. If you'd rather assert `"adequate"` with a shorter fixture, I'll switch.

**Not touching:** the scorer's thresholds or regexes, or any other test.

**How I'll check it (not run yet):**
- With the marker still on, the test should report `XPASS(strict)`.
- After removing it, the single test should pass, and its log should show `category=comprehensive` with `word_count` of 500 or more.
- `pytest tests/unit/test_readme_scorer.py -q` should go from `22 passed, 1 xfailed` to `23 passed`.
- I'll also run `make check && make test-unit`. I haven't run those on main yet, so I'll report any failure that was already there. On the PR, I'll confirm all five CI jobs are green.

I'll build this on `fix/63-readme-scorer-fixture-length` in my fork and post what I find.

Claude Code helped me with this plan: it re-read the test and scorer, ran the read-only threshold check, and drafted this comment. I reviewed it.
````

---

## Your branch

**Branch**

`fix/63-readme-scorer-fixture-length` on my fork `batoula1/pathreview-ai301-fa26-s3`. The fix is commit `45797c6` (`fix: extend README scorer fixture for issue #63`), which changes only `tests/unit/test_readme_scorer.py`.

**Evidence**

Commands were run from the repo root with the project's venv, on macOS 14.6.1, Python 3.13.7, pytest 9.1.1. Claude Code ran these commands for me and saved the logs; I reviewed the output.

**Before:** my Unit 2 reproduction, run on `main` at `2f4e82f` on 2026-10-05 (saved in `test-output.log`):

```
$ .venv/bin/python -m pytest tests/unit/test_readme_scorer.py -q
x......................                                                  [100%]
22 passed, 1 xfailed in 0.85s
[exit code: 0]

$ .venv/bin/python -m pytest tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals --runxfail -vv --tb=short
============================= test session starts ==============================
platform darwin -- Python 3.13.7, pytest-9.1.1, pluggy-1.6.0 -- /Users/halimaallouch/Downloads/pathreview-ai301-fa26-s3/.venv/bin/python
cachedir: .pytest_cache
hypothesis profile 'default'
benchmark: 5.3.0 (defaults: timer=time.perf_counter disable_gc=False min_rounds=5 min_time=0.000005 max_time=1.0 calibration_precision=10 warmup=False warmup_iterations=100000)
rootdir: /Users/halimaallouch/Downloads/pathreview-ai301-fa26-s3
configfile: pyproject.toml
plugins: cov-7.1.0, hypothesis-6.168.4, asyncio-1.4.0, platformdirs-4.12.3, benchmark-5.3.0, pytest_httpserver-1.1.5, anyio-4.15.1
asyncio: mode=Mode.STRICT, debug=False, asyncio_default_fixture_loop_scope=None, asyncio_default_test_loop_scope=function
collecting ... collected 1 item

tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals FAILED [100%]

=================================== FAILURES ===================================
____________ TestReadmeScorer.test_readme_with_all_quality_signals _____________
tests/unit/test_readme_scorer.py:60: in test_readme_with_all_quality_signals
    assert data["word_count"] > 100
E   assert 51 > 100
----------------------------- Captured stdout call -----------------------------
2026-10-04 22:35:09 [info     ] readme_scored                  category=minimal score=0.8717142857142858 word_count=51
=========================== short test summary info ============================
FAILED tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals - assert 51 > 100
============================== 1 failed in 0.20s ===============================
[exit code: 1]

```

I ran the same commands again on the fix branch before editing anything (2026-10-11, from `baseline-tests.log`), and got the same result: `22 passed, 1 xfailed` [exit code: 0], then `E   assert 51 > 100` with `category=minimal ... word_count=51` and `1 failed` [exit code: 1]. The full unit suite was `375 passed, 53 xfailed, 1 warning` [exit code: 0].

**After:** the same two reproduction commands re-run against the fixed code on `fix/63-readme-scorer-fixture-length` at commit `45797c6` (from `after-repro.log`):

```
$ .venv/bin/python -m pytest tests/unit/test_readme_scorer.py -q
.......................                                                  [100%]
23 passed in 0.30s
[exit code: 0]

$ .venv/bin/python -m pytest tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals --runxfail -vv --tb=short
============================= test session starts ==============================
platform darwin -- Python 3.13.7, pytest-9.1.1, pluggy-1.6.0 -- /Users/halimaallouch/Downloads/pathreview-ai301-fa26-s3/.venv/bin/python
cachedir: .pytest_cache
hypothesis profile 'default'
benchmark: 5.3.0 (defaults: timer=time.perf_counter disable_gc=False min_rounds=5 min_time=0.000005 max_time=1.0 calibration_precision=10 warmup=False warmup_iterations=100000)
rootdir: /Users/halimaallouch/Downloads/pathreview-ai301-fa26-s3
configfile: pyproject.toml
plugins: cov-7.1.0, hypothesis-6.168.4, asyncio-1.4.0, platformdirs-4.12.3, benchmark-5.3.0, pytest_httpserver-1.1.5, anyio-4.15.1
asyncio: mode=Mode.STRICT, debug=False, asyncio_default_fixture_loop_scope=None, asyncio_default_test_loop_scope=function
collecting ... collected 1 item

tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals PASSED [100%]

============================== 1 passed in 0.19s ===============================
[exit code: 0]
```

The other checks after the fix (from `final-tests.log`):

```
$ .venv/bin/python -m pytest tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals -vv -rP
2026-10-11 12:20:43 [info     ] readme_scored                  category=comprehensive score=1.0 word_count=596
============================== 1 passed in 0.28s ===============================
[exit code: 0]

$ .venv/bin/python -m pytest tests/unit/test_readme_scorer.py -q
23 passed in 0.18s
[exit code: 0]

$ make test-unit
================= 376 passed, 52 xfailed, 1 warning in 11.33s ==================
[exit code: 0]

$ .venv/bin/ruff check .
All checks passed!
[exit code: 0]

$ .venv/bin/black --check .
110 files would be left unchanged.
[exit code: 0]

$ .venv/bin/mypy api/ core/ ingestion/ rag/ agent/ safety/
Success: no issues found in 76 source files
[exit code: 0]

```

`black --check .` stands in for `make check`, because that target's `make format` step runs `black .`, which rewrites files. This is recorded in Deviations in `plan.md`. CI has not run, because no pull request is open yet.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run, all 20 scored packages: **20/20** (`agreement: 20/20 scored items  (bar: 18/20: PASS)`), with `categories: clear-accept 7/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`.

That was the only run. It agreed with every gold label, so there was nothing to revise and no `--only` or canary re-runs. The committed `eval-run.txt` is this run.

**Package analysis**

**pkg-20** (ghostty-org/ghostty#11261, category `thread-convention`). My rubric decided **reject**, and the gold label is **reject**.

Six of the seven required checks passed. The diagnosis fits the repro: the no-hyperlink control passes, so growth during print is the trigger. The scope is one generation counter plus regression tests. The test is decisive (`zig build test` with both fuzz cases). The plan also follows mitchellh's direction in the thread to recompute `prev` only when capacity changed, and it stays away from the recompute-everywhere approach he rejected.

The one required check that failed was `ai-policy-disclosure`. The grader's evidence line was: "Policy: 'All AI usage in any form must be disclosed, stating the tool used and the extent'; the comment has no AI-use disclosure."

My rubric reads it this way because that check takes its pass condition from the repo's own policy line, not from a blanket rule. ghostty's policy covers AI use "in any form", and the eval treats every package as AI-assisted work, so a comment with no disclosure fails. The verdict rule rejects on any failed required check, so a strong plan is still held. That matches the gold note: "excellent bounded plan ... but the comment contains no AI-use disclosure."

**Check rationale**

Quoted exactly from `tools/plan-check/rubric.md`:

| ai-policy-disclosure | The repo-facts block's contribution policy (live: the repo's CONTRIBUTING or AI policy) read against the plan comment. Treat every package as AI-assisted work. | Pass when the stated policy has no disclosure requirement that covers issue comments. When the policy requires disclosing AI use in comments, or in any form, pass only when the comment says that AI was used and what it helped with. When the policy requires comments in the contributor's own words, fail only if the comment says it was AI-written. Disclosure asked only for pull requests does not apply to a plan comment. | required |

**Why it reads this way:** I rejected the simpler rule "the comment must disclose AI use", because the eval packages show disclosure duties differ by repo. pkg-05 (conda: "generative AI tools welcome; you are responsible ...") is a gold accept with no disclosure in its comment. So are pkg-02 and calib-01, whose repos state no AI policy. A blanket rule would have rejected all three. fd (pkg-09) asks for disclosure only in the pull request, which is why the last sentence keeps PR-only rules from applying to a plan comment. ripgrep (pkg-03, calib-04) asks for comments in the contributor's own words, which is a different duty from disclosure, so it gets its own narrower clause. I did not revise this check: the first and only full eval run passed both thread-convention packages with it as written.

**Trade-offs**

This check only asks that the comment say AI was used and what it helped with. It does not check every detail a stricter policy asks for. ghostty's policy (pkg-20) says disclosure must state "the tool used and the extent of the assistance". A comment saying only "AI helped me draft this" would pass my check, because it says AI was used and what it helped with, even though it never names the tool. So for a repo like ghostty, my rubric can accept a comment that the repo's own policy would still find incomplete. I accepted that because the eval set never tests the gap: pkg-20's comment has no disclosure at all. A tighter wording would be "meets every element the stated policy lists".

A second limitation shows up in live mode. The check can only read the policy it can find. If a repo keeps its AI rules in a separate file (for example `AI_POLICY.md`) that the grader never opens, the check takes the "no disclosure requirement" branch and passes by default.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
