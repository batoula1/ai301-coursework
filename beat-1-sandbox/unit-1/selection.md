# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

[The individual Path Review issue page. A link to the repository or the issue list
does not satisfy this field.]

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/63

**Verdict output**

[Your skill's live-mode output for this issue, pasted verbatim and ending with the
fenced JSON verdict block. A summary does not satisfy this field.]

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

````
issue-select, live mode: codepath/pathreview-ai301-fa26-s3 #63, #64, #54
Graded 2026-10-04 against rubric.md (unchanged) and scope.md (fit profile filled in; repo and house rule unchanged).

RANKED READ-OUT

Accepted, in fit order:
1. #63, the README scorer test's sample README is too short for its own word-count check.
   Fit: this matches every stated preference in the profile.
   - It's a test correction ("correcting tests").
   - It starts by reproducing a failure: `pytest tests/unit/test_readme_scorer.py -q` fails with `assert 51 > 100` ("reproducing bugs").
   - It's one bounded task: extend the test's sample README, or correct the assertion.
   - It has a clear way to verify the fix: that test passes.
   Preferred check: newcomer-support passes ('good first issue' label).
   It's the only accepted issue, so it ranks first by default.

Rejected:
- #64, the "partial overlap" test for the relevance scorer actually uses a full overlap. Failed check: available. PRs #75 (2026-09-27) and #87 (2026-10-04) are open.
- #54, resume section detection misses indented section headers. Failed check: available. PR #76 (2026-09-27) is open.
  (#64 would also have fit the profile well as a test correction, but fit cannot change a verdict.)

CHECKS (the same for all three unless noted)
- maintainer-active: pass. Newest commit 2f4e82f is by Aburke225 (Collaborator) on 2026-09-16, within 90 days. Aburke225 also opened all three issues.
- repo-in-use: pass. Not archived and no releases, but a human push on 2026-09-16 is within 90 days.
- bounded-scope: pass for all three. Each asks for one fix and gives a command or code to reproduce the failure.
- available: #63 passes (no assignee, no linked or mentioned PR among the repo's 17 PRs). #64 and #54 fail (open PRs listed above). Classmates' claim comments are ignored under the classroom rule in scope.md.
- ai-workflow-allowed: pass. docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md say nothing about AI-assisted work, and there is no AI policy file. CONTRIBUTING's conditions still apply: all five CI jobs must pass, and any xfail marker covering the fixed test must be removed.
- newcomer-support (preferred): pass for all three ('good first issue').

NOTE ON THE RUBRIC: The classroom rule's reasoning (credit comes from opening your own PR, not from getting it merged) suggests classmates' open PRs shouldn't block an issue either. The rubric says they do, so #64 and #54 are rejected as the rubric is written.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/63",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "Human commit 2f4e82f by Aburke225 on 2026-09-16 (18 days before 2026-10-04); Aburke225 is COLLABORATOR and opened this issue 2026-09-10."},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived: false; no releases, but human push 2026-09-16 within 90 days."},
      {"name": "bounded-scope", "grade": "pass", "evidence": "One test fix: 'Extend the fixture (or correct the assertion)' in test_readme_with_all_quality_signals; steps to reproduce given; no prior attempts."},
      {"name": "available", "grade": "pass", "evidence": "No assignees, no linked or mentioned PRs (searched all 17 repo PRs); only student claim comments, ignored per house rule."},
      {"name": "ai-workflow-allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md contain no AI restriction; no AI policy file in repo root."},
      {"name": "newcomer-support", "grade": "pass", "evidence": "Labeled 'good first issue' (also 'tier-1')."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/64",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "Human commit 2f4e82f by Aburke225 on 2026-09-16; Aburke225 is COLLABORATOR and opened this issue 2026-09-10."},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived: false; no releases, but human push 2026-09-16 within 90 days."},
      {"name": "bounded-scope", "grade": "pass", "evidence": "One test fix: 'Fix the fixture so the overlap is genuinely partial' in test_query_with_partial_overlap."},
      {"name": "available", "grade": "fail", "evidence": "No assignee, but implementation PRs #75 (2026-09-27) and #87 (2026-10-04) are open."},
      {"name": "ai-workflow-allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PR template contain no AI restriction; no AI policy file in repo root."},
      {"name": "newcomer-support", "grade": "pass", "evidence": "Labeled 'good first issue' (also 'tier-1')."}
    ],
    "verdict": "reject"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/54",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "Human commit 2f4e82f by Aburke225 on 2026-09-16; Aburke225 is COLLABORATOR and opened this issue 2026-09-10."},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived: false; no releases, but human push 2026-09-16 within 90 days."},
      {"name": "bounded-scope", "grade": "pass", "evidence": "One bug: _detect_sections() misses indented headers; steps to reproduce and three named failing tests given."},
      {"name": "available", "grade": "fail", "evidence": "No assignee, but implementation PR #76 (2026-09-27, 'fix: detect indented resume section headings') is open."},
      {"name": "ai-workflow-allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PR template contain no AI restriction; no AI policy file in repo root."},
      {"name": "newcomer-support", "grade": "pass", "evidence": "Labeled 'good first issue' (also 'tier-1')."}
    ],
    "verdict": "reject"
  }
]
```
````

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

[The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.]

1. First full-run attempt: no score. It failed on all 20 issues because Claude was not
   logged in, and it produced no saved run.
2. Second full run, after logging in and getting a successful OK check: 18/20, the run
   recorded in `eval-run.txt`:

   > `agreement: 18/20 scored items  (bar: 18/20: PASS)`
   >
   > `categories: claimed 4/4  clear-accept 7/8  dead-repo 3/3  policy 1/1  scope 3/4`

   It passed every category floor. I made no rubric revisions and no partial `--only`
   runs between the two attempts. The rubric hash recorded in `eval-run.txt`
   (`rubric.md  sha256:a2ee7584a3274554`) matches the `rubric.md` in `tools/issue-select/`.

**Issue analysis**

[One scored issue, identified by id (`issue-01` through `issue-20`; the `calib-`
issues are not scored). State your rubric's decision, the gold label, and the
reasoning that produced your rubric's result.]

**issue-19** (zxcalc/zxlive#517, "Selecting large subgraphs in proof mode freezes the UI").

- Gold label: **accept**. My rubric's decision: **reject**.
- From `eval-run.txt`:
  > `issue-19  accept  reject   NO     failed: bounded-scope, newcomer-support (preferred)`

The only required check that failed was `bounded-scope`. `newcomer-support` also failed,
but it is a preferred check: my verdict rule says "Preferred checks never change
accept/reject," so it did not cause the rejection. The other required checks had
nothing to fail on in the bundle: there were recent human commits ("2026-08-04 by
RazinShaikh: Edge tool can draw multiple parallel edges at once (#521)"), a recent
release ("v1.0.0 (2026-04-29)"), "assignees: none; linked PRs: none", and "no statement
on AI or contribution tooling".

`eval-run.txt` records which checks failed, but not the grader's per-check evidence, so
the following is the tension in the issue text that bounded-scope had to resolve, not
the model's own words. The issue reports one observable bug, a UI freeze: "Selecting
large subgraphs in proof mode freezes the UI." But the body names more than one fix:
"There are two potential causes which should be fixed: 1. The matchers are slow for
certain rewrites (quadratic instead of linear) 2. UI update is waiting for the matching
thread to finish". It then adds "Additional suggestions", including "We should use
multi-processing to use all the cores to match rewrites in parallel" and "Applying the
rewrite should also happen in a separate thread." My bounded-scope check passes only if
the issue "Requests one identifiable contribution" and rejects "maintainer-confirmed
changes to core internals." Read as a list of matcher-algorithm and threading changes,
the issue fails that check. Read as one freeze to fix, it passes, which is how the gold
label reads it.

**Check rationale**

[One check from the `rubric.md` uploaded to `tools/issue-select/`, quoted as it is
currently written, with the reasoning behind its current form.]

The `bounded-scope` row, as currently written in `tools/issue-select/rubric.md`:

> | bounded-scope | Issue body, labels, maintainer comments, and linked PR history. | Requests one identifiable contribution. Reject umbrella/tracking work, unresolved design debates, pure usage questions, or maintainer-confirmed changes to core internals. Also reject issues open over 2 years with at least 2 closed unmerged attempts. A short description or missing reproduction steps alone does not fail a bounded task. | required |

Evidence sources: the check reads only what is in the issue itself: the body, labels,
maintainer comments, and linked PR history. It doesn't use repo-level facts, because
scope depends on what this one issue asks for.

Purpose: a first contribution should be one piece of work I can finish and verify.
Each rejection clause names a pattern that isn't that:
- umbrella or tracking issues are several tasks in one;
- unresolved design debates have no settled answer to implement;
- usage questions aren't contributions;
- maintainer-confirmed core-internals changes are too deep for a newcomer;
- an issue "open over 2 years with at least 2 closed unmerged attempts" has shown it is
  harder than it looks.

The last sentence, "A short description or missing reproduction steps alone does not
fail a bounded task," is there so the check judges the size of the work, not how
polished the write-up is. A terse issue can still be a good first issue.

**Trade-offs**

[What the quoted check gives up. Any one of these is a complete answer: an issue whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]

The check is conservative: when an issue names several causes or suggested changes, it
can reject work that is really valid. I accept that it will miss cases like
**issue-19**. Gold says accept, but the check rejected it because the body lists
matcher-speed and threading changes alongside the one UI freeze. My rubric would rather
pass up a valid issue than hand a newcomer an issue that turns into a multi-part
redesign.

Nothing changed elsewhere, and here is how I know: I made no rubric revisions after the
eval run. The `rubric.md` hash in `eval-run.txt` (`sha256:a2ee7584a3274554`) is the same
file now in `tools/issue-select/`, so the 18/20 result still describes this rubric.

There is a separate limitation I'm also keeping as is: the `available` check. It fails
an issue with any "open implementation PR," and the Path Review house rule in
`scope.md` excuses only "other students' claim comments." In my live run, that rejected
Path Review #64 (open PRs #75 and #87) and #54 (open PR #76), even though classmates'
open PRs don't cost me course credit. I left the rubric unchanged, so I chose #63, which
the rubric accepts as written.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

[Answer all three:

1. The issue's fit to your interests and to the time available.
2. What the verdict identified correctly, and what you weighed that the rubric could
   not.
3. The anticipated difficulty in claiming it.]

1. I wrote and edited code during my PeopleShores apprenticeship, and I use GitHub,
   Terminal, and Claude Code for this course. I want to get better at reproducing bugs,
   correcting tests, and contributing through pull requests. Issue #63 is one test
   fixture correction, and it has a clear way to check the fix: the issue says
   `pytest tests/unit/test_readme_scorer.py -q` currently fails with `assert 51 > 100`,
   so after the fix that test should pass. I have a few hours a week for this, and one
   small, bounded task like this fits that time.

2. The verdict correctly found that the repo has an active maintainer, that #63 has no
   assignee and no open pull request, that it asks for one bounded change, that
   CONTRIBUTING doesn't restrict AI-assisted work, and that it has the good-first-issue
   label. What the rubric couldn't weigh is whether the issue matches what I want to
   learn, and this one is about correcting a test. The issue also lets me choose: "Extend
   the fixture (or correct the assertion)." I still need to decide which one matches
   what the test is meant to check. I haven't reproduced or fixed the issue yet.

3. Two classmates have already posted claim comments on #63. The house rule says that
   is normal and doesn't block me, but a classmate may open a pull request before I do.
   My PR also has to meet the repo's requirements: all five CI jobs must pass, and I
   need to remove any xfail marker on the test I fix. If this is my first PR from a
   fork, CI may wait for a maintainer to approve it before it runs.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
