# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/152

**Branch**

`fix/63-readme-scorer-fixture-length` (on my fork `batoula1/pathreview-ai301-fa26-s3`; PR #152 fixes issue #63)

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Harness runs (`python3 run_eval.py --skill ~/.claude/skills/pr-precheck`), in order:

1. Smoke run, `--limit 1` (pkg-01 only): **1/1** (`agreement: 1/1 scored items`). This was a partial run, so the harness did not write `eval-run.txt`.
2. Full run, all 20 scored packages, with `--save-run eval-run.txt`: **20/20**: `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with `categories: clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`.

The full run agreed with every gold label, so I made no revisions and ran no `--only` or canary re-runs. The committed `eval-run.txt` is run 2.

These are separate from the harness runs: I ran the installed skill three times in live mode on my own PR package. That is not an eval run and has no agreement score. The verdicts were:
1. reject: the frontend check had not run, and the commit message had no `type(scope)`;
2. accept, after I ran the frontend check, amended the commit message to `test(agent): ...` (commit `fa972f1`), re-ran every check on that commit, and updated the drafts;
3. accept, on a final re-check before opening the PR.

**Package analysis**

**pkg-07** (golangci/golangci-lint#6671, category `not-tested`). My rubric decided **reject**, and the gold label is **reject**.

Five of the six checks passed. The diff stays inside the plan: it groups suggested-fix edits by target file and warns and skips unresolvable positions, both in `pkg/goanalysis/runner_fix.go`. The diff is clean, the repo's only template ask (a brief description) is met, and the repo has no AI policy.

The one failing check was `test-evidence`. The grader's evidence line was: "Evidence runs only the one-file control (`golangci-lint run --fix ./onefile/`); the plan's two-file a.go/b.go repro with `go build ./...` is never re-run, and only `go test ./pkg/goanalysis/...` is shown rather than the project suite."

My rubric reads it this way because test-evidence compares the evidence against the plan's test plan, not against how confident the PR sounds. The plan's own repro evidence says the single-file case "applies cleanly", so it was always the control: the path that never broke. The bug only shows across two files. Passing evidence therefore proves nothing about the fix, and the rubric names that fail case directly ("a control or single-file path only"). That matches the gold note: "the evidence runs only the single-file control (the path that never breaks)".

**Check rationale**

Quoted exactly from `tools/pr-precheck/rubric.md`:

| test-evidence | The PR's test evidence (the evidence section, and before/after pasted in the description) read against the plan's test plan and the reproduction's steps, plus the repo's stated test or check commands. | Pass when all three hold. (1) The main reproduction from the plan's test plan is re-run on the fixed code, shown as the command or steps plus the observed after-result, and that result matches the plan's expected-after. The before-result is shown or quoted from the repro evidence. (2) Every other failure mode or repro case the test plan names has its after-result reported, as output or as one concrete observed result. Controls and "unchanged" checks may be one-line statements. (3) When the test plan or the repo's asks name a test suite or check command, the evidence states that it was run and its outcome. Fail when the evidence is only "tested locally", "works now", "verified on my machine", or "all tests pass" with no repro shown; when it exercises a different case from the fix (a control or single-file path only, a different method or path, or a test of something the change does not touch); when a named failure mode or repro case is skipped without a note; or when a named suite or check was never run. A check that could not run, or reported no tests, passes only if the evidence says so plainly with its real output and exit code; a check that failed fails this check unless the failure is shown to exist without the change. | required |

**Why it reads this way:** I did not revise this check; the first full run agreed 20/20. The wording comes from choices I made while reading the packages, before that run:
- I rejected "every repro case must show full before/after output", because the gold accepts don't meet it. pkg-05 reports its second case (same-key redefine) in one sentence, "Same-key redefine prints the one-time warning". pkg-02 and pkg-19 state their controls in prose. Requiring output for everything would reject clear accepts. So the main repro must be shown as command plus output, and other cases need only one concrete observed result.
- I rejected a check that only asks "is there test evidence?", because each not-tested package has evidence of some kind:
  - pkg-04: "colors work now";
  - pkg-10: "a full day of WebDAV syncs";
  - pkg-07: a passing control;
  - pkg-14: a GET request.

  That is why the fail cases name each of those shapes.
- The sentence about checks that "could not run, or reported no tests" came from my own Path Review PR. `make test-integration` collects no tests and exits 2. I wanted that to pass only when it is reported honestly with its real output.

**Trade-offs**

The cost is in clause (2): for any failure mode beyond the main repro, the check accepts "one concrete observed result" as a sentence, without requiring output. That is what lets pkg-05 pass. It is also a case I accept the check will miss.

calib-04 shows the gap. Its plan names two failure modes. The evidence re-runs repro 1 and says nothing about repro 2 (`-j 200000 -x echo .`), so the check fails it on silence. But a version of that PR adding one unsupported line, "repro 2 now also gives the argument error", would pass clause (2) without any output. The check can catch a skipped case, but it can't catch a claimed case that was never actually run.

I kept the looser wording because tightening it would reject pkg-05, a gold accept. The full run confirms the trade-off cost nothing on this set: all 7 clear-accepts and all 4 not-tested packages agreed. Nothing changed elsewhere, because this was the first and only full run, so no package flipped.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
