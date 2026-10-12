# Rubric: is this pull request ready to submit?

Source note: no group worksheet was available, so this rubric adapts
my Unit 3 plan-check rubric, as the assignment allows. Its checks were
calibrated against the four worksheet (calibration) packages that ship
with the Unit 4 eval set. Each required check maps to one harness
category: plan-fidelity → silent-drift, test-evidence → not-tested,
diff-clean → unreviewable, repo-standards and ai-disclosure →
standards-wall.

Every check reads one part of the package against another. None grades
length, headings, or tone.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan-fidelity | The diff's changed files and hunks, and the commit list, read against the plan's scope, files, and not-in-scope lines and its deviation or deferral notes. Then the description's claims ("exactly as planned", "no other changes", "docs updated", "CHANGELOG added", "implements the plan in full") read against the diff. | Pass when all three hold. (1) Every change in the diff is inside the plan's scope, is a test for the fix, is an entry the repo's stated asks require (CHANGELOG, whatsnew, i18n string, docs line for the changed behavior), or is covered by a deviation note. (2) Every deliverable the plan promises is in the diff, or its absence is recorded in the plan's deviation or deferral note AND stated in the description. (3) Nothing the description claims is contradicted by the diff. Fail on any extra feature, option, setting, flag, rename pass, refactor, or rewrite the plan does not name; on any change the plan's not-in-scope line excludes; on a planned deliverable silently missing; or on a description claiming work the diff does not contain, or claiming fidelity ("exactly", "no changes beyond the plan") while the diff holds unplanned changes. A commit titled `refactor:`, `feat:`, or "add ... option" that the plan does not name is itself evidence of extra scope. | required |
| test-evidence | The PR's test evidence (the evidence section, and before/after pasted in the description) read against the plan's test plan and the reproduction's steps, plus the repo's stated test or check commands. | Pass when all three hold. (1) The main reproduction from the plan's test plan is re-run on the fixed code, shown as the command or steps plus the observed after-result, and that result matches the plan's expected-after. The before-result is shown or quoted from the repro evidence. (2) Every other failure mode or repro case the test plan names has its after-result reported, as output or as one concrete observed result. Controls and "unchanged" checks may be one-line statements. (3) When the test plan or the repo's asks name a test suite or check command, the evidence states that it was run and its outcome. Fail when the evidence is only "tested locally", "works now", "verified on my machine", or "all tests pass" with no repro shown; when it exercises a different case from the fix (a control or single-file path only, a different method or path, or a test of something the change does not touch); when a named failure mode or repro case is skipped without a note; or when a named suite or check was never run. A check that could not run, or reported no tests, passes only if the evidence says so plainly with its real output and exit code; a check that failed fails this check unless the failure is shown to exist without the change. | required |
| diff-clean | Every hunk of the diff, and the commit messages. | Pass when every hunk serves the fix, its tests, or a required entry. Lines inside the fix's own hunk that the diff shows as removed and re-added unchanged are diff-rendering noise, not churn. Fail on any of: a debug print or log left in (live or commented out); commented-out code or an earlier attempt kept "for reference"; a dead or unused function, or `allow(dead_code)`-style suppressions; a TODO unrelated to the fix; a hunk whose only changes are formatting, import reordering, re-indenting, or re-printing identical lines in code the fix does not otherwise change; or an unrelated drive-by edit. | required |
| repo-standards | The repo-facts block's PR-template asks and contribution asks (live: `.github/PULL_REQUEST_TEMPLATE.md`, `CONTRIBUTING`, and the `scope.md` house rules), read against the title, description, and diff. Also the thread highlights' maintainer comments read against the description. | Pass when every explicit ask that applies to this PR is met with real content. This includes each required template section or checklist, filled with real content (not left blank, and not boilerplate that contradicts the PR); a required issue link line (`Closes #N`, "fixes #N"); a required changelog or whatsnew entry present in the diff; a required title convention; and a required checklist item either truthfully ticked or explained. When a maintainer (OWNER, MEMBER, COLLABORATOR, or a CONTRIBUTOR proposing an approach) asked implementers a direct question or set a direction in the thread, the description must engage it or the change must follow it. A checklist item that can only be satisfied after the PR is opened (CI on the PR) passes when left honestly unticked, with the same checks shown run locally. Pass when the repo states no applicable ask. Fail when a required section, link line, or entry is missing; when a box is ticked without the description or evidence showing it true; or when explicit maintainer direction is ignored. | required |
| ai-disclosure | The repo-facts block's contribution or AI policy (live: CONTRIBUTING, any AI policy file, and the template) read against the PR description. Treat every package as AI-assisted work. | Pass when the stated policy has no disclosure requirement that covers PRs. When the policy requires disclosing AI use (in PRs, or "in any form"), pass only when the description discloses every element the policy lists: for example, both the tool used and the extent of the help when the policy asks for both. When the policy requires text written in the contributor's own words, fail only if the description says it was AI-written. Fail when a required disclosure is missing or leaves out an element the policy names. | required |
| title-commits | The PR title and the commit messages. | Pass when the title says what the change does, in words a reviewer can understand without opening the diff, and the commit messages name what each commit changes. Fail on titles like "fixed the bug!!" and commit messages like "wip", "fix", or "misc cleanups". | preferred |

## Verdict rule

- `accept` when every `required` check grades `pass`.
- `reject` when any `required` check grades `fail` or `unclear`.
- An `unclear` on a required check counts as `fail`. `unclear` is
  allowed only when the fact the check needs is genuinely missing from
  the package. When a check's "pass when ... no ..." clause applies
  (no applicable template ask, no disclosure requirement), grade
  `pass`, not `unclear`.
- `preferred` checks never change the verdict.
- An honestly recorded and stated shortfall is not a failure: a PR
  that delivers less than the whole plan can be `accept` when
  plan-fidelity's deviation clause is met.
- The deciding check for a `reject` is the first failing required
  check in table order.
