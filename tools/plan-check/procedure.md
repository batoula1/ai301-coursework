# Procedure: how this skill grades a plan package

## Read order

1. Eval mode: confirm the input is a package bundle and use only its
   text. Live mode: read `scope.md` first and stop without grading if
   the issue is outside the scoped repo; then read `voice-guide.md`.
2. Read the issue section. Write down, in one line, the reported
   defect: what input or action, what wrong result, what expected
   result.
3. Read the repro-evidence block before the plan. For every numbered
   step, control run, and artifact, write one line: what was run, what
   came out, and what that rules in or out. Controls matter most: a
   control that runs a component and gets correct output rules that
   component out as the cause; a step that shows the data already
   wrong at some stage rules out every later stage. This list is
   written before the plan is read so that the plan's confidence
   cannot shape what the evidence is taken to show.
4. Read the thread highlights. List every comment by an OWNER, MEMBER,
   COLLABORATOR, or CONTRIBUTOR-role participant that names a culprit,
   prefers or rejects an approach, posts a patch or test build, or
   points to a fix PR. Write "no maintainer-side direction" if there
   is none.
5. Read the repo-facts block. Copy out the contribution policy's AI
   rule word for word (or "no stated AI policy"), and any explicit
   contribution requirement that would apply to a code or test change.
6. Only now read the candidate plan, then the candidate plan comment.

## Evidence gathering

Use `references/evidence-guide.md` for where each family lives. For
each check, record the fact before grading:

1. diagnosis-grounded: quote the plan's cause in one line. Next to it,
   name each line from Read order step 3 that the cause must explain,
   and mark each "fits" or "contradicts".
2. scope-bounded: list every change the plan commits to (numbered
   changes, files, "while in the area" items, new options, upgrades,
   CI jobs). Mark each as "fixes the reproduced defect", "test or doc
   for the fix", "same fix at a sibling site", or "extra". Items the
   plan names only as deferred or out of scope are not listed.
3. executable: copy the named files or code areas, and the one
   concrete change. Copy any hedge word that leaves a choice open
   ("somewhere", "not sure", "whichever", "investigate", "profile").
4. test-plan-decisive: copy each test-plan item and write the
   observable outcome it names, or "none named".
5. thread-direction: for each item from Read order step 4, write
   whether the plan follows it, the comment names it and explains a
   departure, or neither.
6. ai-policy-disclosure: copy the policy line from Read order step 5
   and the comment's AI-use sentence, or "none in comment".
7. repo-conventions: write each applicable requirement from Read order
   step 5 and whether the plan includes or contradicts it.
8. unknowns-honest: copy the plan's risk or unknown lines, and any
   "verified" or "tested" claim with whether the package shows it.

In live mode, the reproduction evidence is the student's posted repro
comment on the issue, plus whatever the drafts quote from it. The
issue body and thread come from the live issue. Repo conventions come
from the repo's CONTRIBUTING file and the scope file's house rules.

## Check execution

1. Run the checks in rubric table order. Grade each check only from
   the notes gathered above; reopen the package only to find a
   specific missing fact, never to re-judge the plan as a whole.
2. Apply the check's pass condition word for word. If the condition
   holds, grade `pass`; if any listed fail case holds, grade `fail`.
3. When a check's "pass when there is no ..." clause applies (no
   maintainer-side direction, no disclosure requirement, no applicable
   convention), grade `pass`.
4. Grade `unclear` only when the fact the check needs is missing from
   the package entirely. Do not use `unclear` for a judgment that is
   hard; decide it from the pass condition.
5. Write one evidence line per check: the quoted fact that decided it
   (for a fail, the contradicting control, the extra item, the hedge
   word, the empty test outcome, the ignored maintainer comment, or
   the missing disclosure).
6. Grade every check, even after one fails. Eval mode always grades
   the full set.
7. Live mode only: after the checks, read the draft comment against
   each rule in `voice-guide.md` and list any rule it breaks, quoting
   the rule. These notes go in the summary and do not change any
   grade.

## Verdict assembly

1. Count the required checks graded `fail` or `unclear`. If the count
   is zero, the verdict is `accept`. Otherwise it is `reject`.
2. Ignore preferred checks when deciding the verdict.
3. For a `reject`, name the first failing required check in table
   order as the deciding check, and quote its evidence line in the
   summary.
4. Write a short summary (one line per check), then the JSON block
   from SKILL.md with every check, its grade, and its evidence line.
   The JSON block is the last thing in the output.
