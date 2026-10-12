# Procedure: how this tool grades a PR package

## Read order

1. Live mode only: read `scope.md` first, and stop or refuse as
   SKILL.md says. Eval mode: confirm the input is one package bundle
   and use only its text.
2. Read the repo facts (live: the PR template, CONTRIBUTING, and any AI
   policy file). Write down every explicit ask that applies to a pull
   request: the template sections or checklist items, the issue-link
   line, changelog or whatsnew entries, the title convention, the test
   or check commands, and the AI policy line copied word for word (or
   "no stated AI policy").
3. Read the issue and the thread highlights (live: the issue and its
   comments, with each author's role). List every comment by an OWNER,
   MEMBER, COLLABORATOR, or CONTRIBUTOR-role participant that asks
   implementers a question or sets a direction. Write "none" if there
   are none.
4. Read the plan (live: `plan.md`, including `## Deviations`), before
   the PR. Write down:
   - the files and areas it names;
   - its in-scope change, broken into each separate deliverable;
   - its not-in-scope line;
   - every deviation or deferral note;
   - its test plan, broken into each named repro case or failure mode
     with its expected-after, then the controls, then the suite or
     check commands.

   The plan is read first so that it, not the description, sets the
   yardstick for the diff.
5. Read the diff and the commit list (live: `git diff main...HEAD` and
   `git log --oneline main..HEAD`) before the description. Write down
   each changed file and what each hunk does, in one line per hunk.
   The diff is read before the description so that the description's
   claims cannot shape what the diff is taken to contain.
6. Read the test evidence (live: the evidence pasted in the
   description, then the student's evidence file).
7. Read the title and description last.

## Evidence gathering

Use `references/evidence-guide.md` for where each family lives. Record
these pairings before grading anything:

1. **Diff against plan (plan-fidelity).** For each hunk from Read order
   step 5, mark it with one label: "planned change", "test",
   "required entry" (with the ask from step 2 it satisfies), "covered
   by deviation note" (quote the note), or "extra". Then, for each
   deliverable from step 4, mark it "in diff", "absent, noted" (quote
   the note), or "absent, silent". Also list any commit whose title
   names work the plan does not name.
2. **Description against diff (plan-fidelity).** Copy each claim the
   description makes about what the PR contains or excludes. Next to
   each, write "true" or "contradicted", naming the hunk or the
   missing file.
3. **Evidence against test plan (test-evidence).** For each repro case
   or failure mode from step 4, copy the evidence line that re-runs it
   and its after-result, or write "not re-run". Then do the same for
   each control. For each suite or check command from steps 2 and 4,
   copy its stated outcome and exit code, or write "never run". If the
   evidence runs a case that differs from the plan's repro, write down
   that case and how it differs.
4. **Hunks against debris tells (diff-clean).** For each hunk, write
   any debug print, commented-out code, dead function, unrelated TODO,
   or format-only/import-only/identical-reprint change, quoting the
   line. Do not flag removed-and-re-added lines inside the fix's own
   hunk.
5. **Asks against PR (repo-standards).** For each ask from step 2,
   copy where the PR meets it (section text, diff file, title) or
   write "missing". For each ticked checklist box, copy the evidence
   that shows it true. For each maintainer item from step 3, copy the
   description line that engages it, or the diff hunk that follows it,
   or write "ignored".
6. **Policy against description (ai-disclosure).** Copy the policy
   line, list each element it requires, and copy the description's
   disclosure sentence (or write "none"). Mark each element "present"
   or "missing".
7. **Title and commits (title-commits).** Copy the title and each
   commit message.

## Check execution

1. Run the checks in rubric table order. Grade each one from the
   records above. Reopen the package only to find one specific missing
   fact; never re-judge the whole PR from an impression.
2. Apply the pass condition word for word. If a listed fail case
   holds, grade `fail`. If every part of the pass condition holds,
   grade `pass`.
3. When a check's "pass when ... no ..." clause applies, grade `pass`.
4. Grade `unclear` only when the fact the check needs is missing from
   the package entirely. Do not use `unclear` for a judgment that is
   merely hard.
5. Do not let one problem fail two checks unless it meets both pass
   conditions' fail cases on its own. For example, extra scope is
   plan-fidelity; debris is diff-clean.
6. Write one evidence line per check. For a pass, cite the fact that
   decided it. For a fail, cite the extra hunk, the missing
   deliverable, the contradicted claim, the missing repro, the debris
   line, the missing ask, or the missing disclosure element.
7. Grade every check, even after one fails.
8. Live mode only: after the checks, read the title and description
   against each rule in `voice-guide.md`. Quote any rule the draft
   breaks. These notes go in the summary and do not change any grade.

## Verdict assembly

1. Count the required checks graded `fail` or `unclear`. If the count
   is zero, the verdict is `accept`. Otherwise it is `reject`.
2. Ignore preferred checks for the verdict.
3. For a `reject`, the deciding check is the first failing required
   check in rubric table order. Quote its evidence line in the summary
   as the reason to hold.
4. Write the summary (one line per check; in live mode, add voice notes
   and any procedure gaps), then the JSON block from SKILL.md with
   every check, its grade, and its evidence line. The JSON block is
   the last thing in the reply.
