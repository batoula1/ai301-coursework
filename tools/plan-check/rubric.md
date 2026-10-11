# Rubric: is this plan ready to post and build from?

Source note: no group worksheet was available, so these checks follow
the assignment's fallback: the failure families named in the template,
calibrated against the four worksheet (calibration) packages that ship
with the eval set.

Every check reads the plan or comment AGAINST something else in the
package (the repro evidence, the issue, the thread, the repo facts).
None of them grades length, headings, or tone. A short plan can pass
every check.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | The plan's stated cause (its diagnosis line or section, and the cause the comment repeats) read against every step, control run, and artifact in the repro-evidence block. Where the plan adopts a cause from the issue or thread, read that cause against the repro evidence too. | Pass when the stated cause names a specific mechanism or code site AND no repro step, control, or artifact contradicts it. A contradiction is: a control that runs the blamed component and gets correct output; a step that shows the failure happening before, or without, the blamed component; or a step that shows the data already wrong before the stage the plan blames. Agreeing with the issue or a thread comment does not rescue a cause the repro evidence rules out; the repro evidence wins. Fail when the plan calls the repro evidence a "red herring" or "side effect" without showing why. | required |
| scope-bounded | The plan's list of changes, files, and areas (in-scope and not-in-scope lines) read against the defect the issue reports and the repro evidence reproduces. | Pass when every change the plan commits to is needed to fix the reproduced defect, or is a regression test, a doc note for the changed behavior, or the same fix applied at a sibling site in the same function or file that shows the same failure pattern. Fail when the plan also commits to any of: a dependency upgrade or library migration; a new user-facing option, setting, prop, or UI; a rewrite, restructure, or "unification" of a module or of other constructs; a new CI job or matrix; fixes for other bugs "while in the area"; scanning or fixing other components "for the same pattern". Offering to split extra work into a PR series still fails: the extra work is in the plan. Work named only as deferred or as a separate issue does not count against the plan. | required |
| executable | The plan's files or code areas and its approach steps. | Pass when the plan names the file(s), or a specific code area (a named function, handler, branch, or module path), where the change goes, AND commits to one concrete change a stranger could start making without asking the author anything. Not knowing the exact line or function inside a named area still passes when the plan states that as an unknown and says how it will be pinned. Fail when the location is "somewhere", the layer is undecided ("X? Y? not sure"), the approach is a choice left open ("upstream or vendored, whichever is easier"), or the plan is investigate-first ("profile / dig in / figure out, then fix whatever shows up"). | required |
| test-plan-decisive | The plan's test plan read against the repro evidence's steps, commands, and expected results. | Pass when the test plan names at least one check whose observable outcome tells the fixed build apart from the broken one: a specific output, value, exit code, assertion, class list, or visible behavior, normally the repro steps re-run with the post-fix result stated. Fail when the only checks are "the test suite passes", "nothing regresses", "it works now", "should feel fast", "nothing else should feel broken", or another outcome nobody could observe as a pass or fail. | required |
| thread-direction | The plan comment and the plan, read against the thread highlights (and, live, the issue thread). Maintainer-side direction means a comment by an OWNER, MEMBER, or COLLABORATOR, or by a CONTRIBUTOR-role participant who proposes or rejects an approach, that names a culprit location, prefers or rejects an approach, posts a patch or test build, or points to an existing fix PR. | Pass when the thread holds no maintainer-side direction. Otherwise pass only when the plan follows that direction, or the comment names it and explains why the plan departs from it. Fail when the plan takes an approach a maintainer-side comment already rejected, or when the comment never engages a maintainer's culprit, patch, test build, or fix PR (for example, planning a docs workaround after the owner pointed at the code culprit and posted a patched build). | required |
| ai-policy-disclosure | The repo-facts block's contribution policy (live: the repo's CONTRIBUTING or AI policy) read against the plan comment. Treat every package as AI-assisted work. | Pass when the stated policy has no disclosure requirement that covers issue comments. When the policy requires disclosing AI use in comments, or in any form, pass only when the comment says that AI was used and what it helped with. When the policy requires comments in the contributor's own words, fail only if the comment says it was AI-written. Disclosure asked only for pull requests does not apply to a plan comment. | required |
| repo-conventions | The repo-facts block (live: CONTRIBUTING and the scope file's house rules) read against the plan's approach and test plan. Only explicit requirements that apply to this specific change count; bug-report template asks, CLAs, and general style guides do not. | Pass when the plan does not contradict any explicit, applicable contribution requirement, and includes any step such a requirement makes part of this change (for example: removing the xfail marker from a test the fix makes pass, running the required check commands). Pass when the repo facts state no such requirement. | required |
| unknowns-honest | The plan's risk, unknown, or open-question lines, and any claim of something already verified, read against the repro evidence. | Pass when the plan states its real uncertainties as uncertainties (or has none worth naming), and claims nothing as verified that the package does not show was run. | preferred |

## Verdict rule

- `accept` when every `required` check grades `pass`.
- `reject` when any `required` check grades `fail` or `unclear`.
- An `unclear` on a required check counts as `fail`. `unclear` is only
  allowed when the evidence the check needs is genuinely missing from
  the package. When a check's own "pass when there is no ..." clause
  applies (no maintainer direction, no disclosure requirement, no
  applicable convention), grade `pass`, not `unclear`.
- `preferred` checks never change the verdict, whatever their grade.
- The deciding check for a `reject` is the first failing required
  check in table order.
