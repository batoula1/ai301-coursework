# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

batoula1

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/63#issuecomment-5988701287

> Hi, I'd like to investigate the word-count mismatch in `test_readme_with_all_quality_signals`: the test asserts `word_count > 100` and a `"comprehensive"` category, but the issue reports the fixture README at about 51 words. I haven't run anything yet. Next I'll run the test from the issue (with `--runxfail`, since it is marked xfail for #63) and post a report here with my environment, commands, and the output I see.
>
> Claude Code helped me draft this comment. I reviewed it, and I'll run every step myself.

repro-check verdict (live mode, claim-only draft, graded before posting; the posted text is identical to the graded draft):

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/63",
  "checks": [
    {"name": "env-recorded", "grade": "unclear", "evidence": "not yet applicable: claim-only draft"},
    {"name": "env-faithful", "grade": "unclear", "evidence": "not yet applicable: claim-only draft"},
    {"name": "steps-rerunnable", "grade": "unclear", "evidence": "not yet applicable: claim-only draft"},
    {"name": "trigger-faithful", "grade": "unclear", "evidence": "not yet applicable: claim-only draft"},
    {"name": "behavior-shown", "grade": "unclear", "evidence": "not yet applicable: claim-only draft"},
    {"name": "claims-match-evidence", "grade": "pass", "evidence": "assertions (word_count > 100, 'comprehensive'), ~51-word fixture, and xfail-for-#63 marker all match the issue and test file; 'I haven't run anything yet' claims no reproduction"},
    {"name": "claim-specific", "grade": "pass", "evidence": "names test_readme_with_all_quality_signals and its mismatch; next step is run the test and post environment, commands, output; no fix, deadline, or assignment request"},
    {"name": "ai-disclosure", "grade": "pass", "evidence": "no AI policy in docs/CONTRIBUTING.md or .github templates; draft discloses 'Claude Code helped me draft this comment' anyway"},
    {"name": "control-run", "grade": "unclear", "evidence": "not yet applicable: claim-only draft"},
    {"name": "template-coverage", "grade": "unclear", "evidence": "not yet applicable: claim-only draft"}
  ],
  "verdict": "accept"
}
```

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/63#issuecomment-5988839576

(First posted 2026-10-05T05:45:46Z with the wrong clipboard text pasted by mistake; edited 2026-10-05T06:00:19Z to the report below. Text copied from the GitHub API, unchanged.)

> ## Reproduction report
>
> I reproduced the reported failure on a clean checkout of current main.
>
> ### Environment
>
> - Repository: my fork `batoula1/pathreview-ai301-fa26-s3` at commit `2f4e82f52efbcfcc57d65b3fa5348672163ca088` (the same commit as upstream `main` when I ran this), clean working tree
> - macOS 14.6.1 (arm64)
> - Python 3.13.7 in a fresh virtual environment
> - pytest 9.1.1, pluggy 1.6.0, structlog 26.1.0
> - Installed with `pip install -e ".[dev]"`
>
> Setup note: I only ran the Python part of `make setup` (create `.venv`, upgrade pip, `pip install -e ".[dev]"`). I skipped Docker, `.env`, migrations, seeding, pre-commit, and the frontend, because this is a `unit` test with no external dependencies.
>
> ## Describe the Bug
>
> `test_readme_with_all_quality_signals` asserts `word_count > 100` and `word_count_category == "comprehensive"`, but the scorer counts its fixture README at 51 words, so the test fails at the word-count assertion.
>
> ## To Reproduce
>
> 1. Set up the environment:
>
> ```bash
> git clone https://github.com/batoula1/pathreview-ai301-fa26-s3
> cd pathreview-ai301-fa26-s3
> git checkout 2f4e82f52efbcfcc57d65b3fa5348672163ca088
> python3 -m venv .venv
> .venv/bin/python -m pip install --upgrade pip setuptools wheel
> .venv/bin/pip install -e ".[dev]"
> ```
>
> 2. Run the command from the issue. The test is marked `xfail(strict=True)` for #63, so the file passes as a whole:
>
> ```
> $ .venv/bin/python -m pytest tests/unit/test_readme_scorer.py -q
> x......................                                                  [100%]
> 22 passed, 1 xfailed in 0.85s
> ```
>
> Exit code: 0
>
> 3. Run the single test with `--runxfail` to see the failure behind the marker:
>
> ```
> $ .venv/bin/python -m pytest "tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals" --runxfail -vv --tb=short
> ...
> tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals FAILED [100%]
>
> =================================== FAILURES ===================================
> ____________ TestReadmeScorer.test_readme_with_all_quality_signals _____________
> tests/unit/test_readme_scorer.py:60: in test_readme_with_all_quality_signals
>     assert data["word_count"] > 100
> E   assert 51 > 100
> ----------------------------- Captured stdout call -----------------------------
> 2026-10-04 22:35:09 [info     ] readme_scored                  category=minimal score=0.8717142857142858 word_count=51
> =========================== short test summary info ============================
> FAILED tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals - assert 51 > 100
> ============================== 1 failed in 0.20s ===============================
> ```
>
> Exit code: 1
>
> ## Expected Behavior
>
> The test's fixture should meet its own assertions: `word_count > 100` and `word_count_category == "comprehensive"`.
>
> Observed: the scorer reports `word_count=51` and `category=minimal`, so the test stops at `assert 51 > 100`. That matches the issue.
>
> From reading `agent/tools/readme_scorer.py` (not from a test run): the scorer counts words with `len(content.split())` and calls fewer than 100 words `"minimal"`, 100–499 `"adequate"`, and 500 or more `"comprehensive"`. So 51 → `"minimal"` looks like correct scorer behavior for this fixture. I think a fixture just over 100 words would pass the first assertion but still fail the `"comprehensive"` one, which the test never reached in my run.
>
> ## Relevant Files
>
> - `tests/unit/test_readme_scorer.py` (the test, its fixture, and the `xfail` marker)
> - `agent/tools/readme_scorer.py` (word count and category thresholds)
>
> ## Acceptance Criteria
>
> - [ ] `test_readme_with_all_quality_signals` passes without `--runxfail`, and its `@pytest.mark.xfail` marker is removed, as CONTRIBUTING.md asks for seeded bugs
> - [ ] The rest of `tests/unit/test_readme_scorer.py` still passes
>
> I haven't changed any code or run the full test suite. Claude Code helped me with this: it helped set up the environment, chose and ran the commands, and drafted this report. I reviewed the output and the report.

repro-check verdict (live mode, full package: the posted claim plus this posted report, graded after the edit with the unchanged skill):

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/63",
  "checks": [
    {"name": "env-recorded", "grade": "pass", "evidence": "posted: commit 2f4e82f (clean), macOS 14.6.1 arm64, Python 3.13.7 venv, pytest 9.1.1, pip install -e \".[dev]\""},
    {"name": "env-faithful", "grade": "pass", "evidence": "tested commit equals current upstream main 2f4e82f; Python 3.13.7 meets >=3.11; skipped Docker/DB/frontend steps named with reason"},
    {"name": "steps-rerunnable", "grade": "pass", "evidence": "public fork URL, checkout SHA, exact venv/pip/pytest commands; no private inputs"},
    {"name": "trigger-faithful", "grade": "pass", "evidence": "runs the issue's exact `pytest tests/unit/test_readme_scorer.py -q`, then the same test with --runxfail past the xfail(strict=True) marker; no code changed"},
    {"name": "behavior-shown", "grade": "pass", "evidence": "posted output: `E   assert 51 > 100` at test_readme_scorer.py:60, log `category=minimal ... word_count=51`, exit code 1, matching the issue"},
    {"name": "claims-match-evidence", "grade": "pass", "evidence": "'I reproduced the reported failure' backed by shown output; threshold reasoning labeled 'not from a test run' and 'I think'; no code change or full-suite claim"},
    {"name": "claim-specific", "grade": "pass", "evidence": "posted claim names test_readme_with_all_quality_signals, its assertions, the ~51-word fixture, and a run-and-report next step; no fix or deadline"},
    {"name": "ai-disclosure", "grade": "pass", "evidence": "repo states no AI policy; both posted comments disclose Claude Code; report: 'helped set up the environment, chose and ran the commands, and drafted this report'"},
    {"name": "control-run", "grade": "fail", "evidence": "xfailed vs --runxfail runs only expose the marker; no passing comparison isolating fixture length"},
    {"name": "template-coverage", "grade": "pass", "evidence": "posted report answers all bug_report.md fields: Describe the Bug, To Reproduce, Expected Behavior, Relevant Files, Acceptance Criteria"}
  ],
  "verdict": "accept"
}
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run, first rubric: **17/20** (below the 18/20 bar; category floor held). Misses: pkg-03, pkg-05, pkg-09, all gold accepts graded reject.
2. Partial run (`--only pkg-03,pkg-05,pkg-09,pkg-02,pkg-15,pkg-18,pkg-20`), revised rubric: **7/7** (partial runs never decide the bar).
3. Full run, revised rubric (no edits after run 2): **20/20** — `agreement: 20/20 scored items  (bar: 18/20: PASS)`, the agreement line in the committed `eval-run.txt`.

Who did the work: Claude Code wrote both versions of the rubric and evidence guide and ran all three evaluations. After run 1, I asked for a narrow revision and set its limits: concrete evidence for the main trigger and outcome; secondary comparisons and repeat counts allowed in prose; essential inputs still required; and unsupported outcomes, unrelated behavior, private prerequisites, and missing required AI disclosure still rejected.

Between runs 1 and 2, three checks changed, along with the matching Steps, Behavior shown, and Honesty sections of the evidence guide:

- `steps-rerunnable` used to require that "every command and input that the trigger depends on is shown or publicly linked." Now the main trigger command must be shown exactly, and an input may be "described precisely enough that a stranger would build an equivalent one (every property the trigger depends on is named)." Secondary variants may be prose. Private code, unshared config, and vague essential inputs still fail.
- `behavior-shown` used to accept any captured artifact. Now it requires "a captured artifact of the main trigger run". A cannot-reproduce must show "at least one real run of the issue's trigger", and "comparison runs, variants, and repeat counts may be described in prose without a transcript of their own" once that artifact is shown.
- `claims-match-evidence` used to require that "each claim of observation is backed by a shown artifact." Now only "the main outcome claim (reproduced, or could not reproduce)" needs a shown artifact. Concrete, consistent secondary statements may stay in prose, and labeled inferences need no artifact. It still fails unsupported outcomes, unproven "verified" causes, "guaranteed" scope, and prose that claims more than the artifact shows.

**Package analysis**

pkg-09 (sharkdp/fd#2033), gold label **accept** ("honest cannot-reproduce"). The package tries to trigger scenario 2, where a later `--exec-batch` command runs before an earlier one when the argument-size limit is hit. It shows one real run, whose `order.log` reads ONE, ONE, ONE, TWO, TWO, TWO. It then says in prose: "I ran this 5 times and also re-ran with the second command's arguments padded to be ~40% longer than the first's", and names what may have differed (uniform name lengths, the 2 MiB ARG_MAX).

- **Run 1 rejected it.** It failed three required checks.
  - `steps-rerunnable`: "the padded variant meant to make TWO hit the limit first is described only as 'a longer wrapper string' with no command shown"
  - `behavior-shown`: "padded runs have no artifact"
  - `claims-match-evidence`: "'ran 5 times', 'in every run' and the padded re-run are unbacked by shown output"

  The first rubric asked for a shown artifact behind every observation, so a described repeat count or variant counted as an unsupported claim. That is stricter than the gold label, which accepts the package because the one attempt that matters is shown and the outcome is stated honestly.
- **Run 3 accepted it.**
  - `behavior-shown` passed: "order.log output ONE x3 then TWO x3 shown from the real run; expected vs actual stated".
  - `claims-match-evidence` passed: "Cannot-reproduce backed by artifact; scope limited to scenario 2; explanations hedged ('may', 'appears to')".

  The two preferred checks still failed (`control-run`: "no non-triggering control isolating the trigger"; `template-coverage`: "No confirmation of reading the troubleshooting section"). Preferred checks never change the verdict.

**Check rationale**

From `tools/repro-check/rubric.md`, exactly as it reads now:

| claims-match-evidence | Every claim in the claim comment and repro report (reproduced, root cause, scope, certainty) set against the artifacts that back it | The main outcome claim (reproduced, or could not reproduce) is backed by a shown artifact of the main trigger run. Secondary statements (a described control or variant result, a repeat count) may stay in prose when they are concrete (name the change made and the result observed) and consistent with the shown artifact. Diagnoses, hypotheses, and explanations of why a cannot-reproduce may not have triggered are labeled as inference ("I think", "likely", "may") and need no artifact. A cannot-reproduce is stated as such, with the attempt shown and what differed named. Fail if the main outcome rests on no shown artifact, if the package claims success, a verified cause, or a scope ("guaranteed", "every version", "verified the race") that its artifacts do not show, if secondary prose claims a behavior different from or beyond the shown artifact, or if it narrates an artifact as something it is not. | required |

It reads this way because the first version ("Each claim of observation is backed by a shown artifact") rejected two honest gold accepts.

- **pkg-03:** failed because "Claims that dropping -r gives 1,4,7,10 correctly, but no output for that run is shown".
- **pkg-09:** failed for its "ran 5 times" repeat count.

Both were concrete, true-sounding secondary details next to a shown main artifact. The revision kept the artifact requirement, because the no-evidence packages (pkg-15's "I verified this race condition", pkg-13's "guaranteed reproducible") have to keep failing. Instead it splits claims into two tiers:

- **The main outcome** must have a shown artifact.
- **Secondary detail** may be prose only when it is concrete and agrees with that artifact.

The "narrates an artifact as something it is not" clause is kept for the wrong-target packages (pkg-02, pkg-08, calib-03), where a confident story sits over an artifact showing a different behavior.

**Trade-offs**

The loosening trusts prose for secondary detail, so a package could invent a convenient control result and pass, as long as that result is concrete and agrees with the main artifact. That miss is accepted: the main outcome still needs a shown artifact, so an invented secondary result can't turn an unproven reproduction into a pass.

To check that no correct rejection flipped, the partial run followed the README's canary rule. Alongside the three misses, Claude Code re-ran `--only` with one rejection that already matched from each category the change could touch:

- **pkg-02** (wrong-target): still rejected on `trigger-faithful`, `behavior-shown`, `claims-match-evidence`.
- **pkg-15** (no-evidence): still rejected on five required checks, including `claims-match-evidence`.
- **pkg-18** (unfollowable-comms): still rejected on `steps-rerunnable`.
- **pkg-20** (disclosure, the README's live case): still rejected on `ai-disclosure`.

The cost showed up in pkg-18, whose reproduction lives in a private monorepo. In run 1 it failed seven required checks; after the revision only `steps-rerunnable` holds it. It still rejects for the right reason, but with no margin left: if that check is loosened again, pkg-18 flips. The final full run confirmed nothing else moved. The only verdicts that changed from run 1 were pkg-03, pkg-05, and pkg-09.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
