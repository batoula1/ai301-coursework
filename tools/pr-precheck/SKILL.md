---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer one question about one PR package: **is this pull request ready
to submit?** Do not answer any other question. Do not judge whether the
issue is worth fixing, whether the plan was a good plan, or whether you
would have written the code differently. Grade exactly one package per
run.

A PR package is the candidate pull request: its title, its
description, its commit list, its diff, and its test evidence. Read
those against three things:

- the plan the PR claims to implement, including the plan's deviation
  or deferral notes;
- the issue the plan belongs to, including its thread;
- the repo's stated standards: the PR template, the contribution
  guide, and any AI-use policy.

## Inputs and modes

Run in exactly one of two modes.

**Live mode** (the student's own PR, before it is opened). Gather these
inputs, from these sources:

1. **Plan:** `plan.md` in the repo working copy's top folder, read in
   full, including its `## Deviations` section.
2. **Diff:** the output of `git diff main...HEAD` (three dots), run from
   the working copy on the PR branch. Also run
   `git log --oneline main..HEAD` for the commit list, and
   `git branch --show-current` for the branch name. The diff is
   everything the branch changes relative to `main`. Files that are
   untracked or uncommitted are not part of the PR.
3. **Title and description:** the draft file the student names (for
   example `pr_draft.md`). The first line is the title; the rest is the
   description.
4. **Test evidence:** the evidence file the student names (for example
   `test_evidence.md`). Count only what the description itself pastes
   or links as evidence the PR shows a reviewer. Anything only in the
   local file is supporting material: note it in the summary, but do
   not credit it for checks that grade the description.
5. **Issue:** the issue URL the student gives. Read the issue body and
   every comment with its author's role (`author_association`), using
   `gh` or the public GitHub API.
6. **Repo standards:** `.github/PULL_REQUEST_TEMPLATE.md` and
   `docs/CONTRIBUTING.md` (or `CONTRIBUTING.md`) from the working copy,
   and any AI policy file the repo has.

A house-chain student reads the house plan and the house repro pack in
place of their own `plan.md` and reproduction. The same checks grade
the same things there.

**Eval mode** (a package bundle). The bundle is the whole world. Take
every fact from the bundle text. Do not fetch, open, or run anything
else. Ignore `scope.md` and `voice-guide.md`. Grade every check in the
rubric and apply the full verdict rule. Eval mode never grades part of
a package.

## The scope seam (live mode only)

In live mode, read `scope.md` before any other file.

- If its `Repo:` line still holds a bracketed placeholder, stop. Do not
  grade. Tell the student to fill the `Repo:` line in `scope.md` with
  their section's Path Review repo. Never guess a scope.
- If the PR's issue or base repo is not the repo named on that line,
  refuse to grade and say which repo the scope allows.
- Apply the scope's house rules as repo standards, alongside the
  template and contribution guide. Examples: the branch is a
  `fix/<issue-number>-<slug>` branch on the student's fork, and the PR
  delivers one bounded change for one issue.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after grading the checks, read `voice-guide.md`. Hold
the draft PR title and description against each of its rules. For
every rule the draft breaks, quote the rule and the line that breaks
it in the summary. Voice notes never change a check's grade or the
verdict, unless a rubric check reads the voice guide itself. Voice is
the student's own, and the rubric holds the universal standards.

In eval mode, ignore `voice-guide.md` entirely.

## Component reads

- `rubric.md` defines the checks, each check's evidence and pass
  condition, and the verdict rule. The rubric decides.
- `references/evidence-guide.md` maps where each evidence family lives
  in a bundle and in live mode, and what good looks like.
- `procedure.md` is the operating procedure. Execute it as written, in
  order: read order, evidence gathering, check execution, verdict
  assembly.

If the procedure is silent on a step you need, do not improvise around
it. Take the most literal reading of the rubric, and name the gap
under "Procedure gaps" in the summary.

If `rubric.md` has no filled checks, or `procedure.md` has no written
steps (only headings and template comments), refuse to grade. Say
which file is empty. Do not invent checks at runtime.

## Verdict and output

The verdict is binary:

- `accept` means ready to submit: open the PR.
- `reject` means hold it and fix the failing checks first.

There is no third verdict and no score. Put reservations in the
evidence lines, never in the verdict.

You may write a short readable summary first: one line per check, then
voice notes and procedure gaps in live mode. End the reply with this
fenced JSON block, filled in. It must be valid JSON, it must be the
last thing in the reply, and nothing may follow it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

In live mode, before a PR exists, use the issue URL plus the branch
name as the `item`.

## Grading discipline

- **Evidence first.** Name the fact or quote that decided every check:
  a diff line, a description sentence, an evidence output line, a
  template ask. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** A terse PR can be ready, and a
  polished one can hide drift. Read the diff against the plan, the
  evidence against the test plan, and the description against the
  diff. Never grade length, tone, or formatting.
- **The rubric decides.** If a check passes by its stated condition but
  feels wrong, it passes. Note the tension in the summary; the fix
  belongs in the rubric.
- **The procedure decides how.** Follow `procedure.md` as written and
  report its gaps.
- **Unclear defaults to fail.** Grade `unclear` only when the fact a
  check needs is genuinely missing. If the rubric's verdict rule does
  not say otherwise, an `unclear` required check counts as a fail: a
  PR you cannot verify from the package is not ready to submit.
- **Honest shortfalls are not failures.** A PR that delivers less than
  the whole plan can still be ready, when the plan records the
  shortfall and the description states it plainly. Only an
  undisclosed gap fails.
