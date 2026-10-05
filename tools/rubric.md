# Rubric: is this reproduction package ready to post?

Every check judges whether the evidence is sufficient, never the
write-up's shape: the number of steps, the length, or the presence of
template headings never decides a grade. A terse report whose artifacts
prove the behavior passes; a long, confident report whose artifacts do
not, fails. Locations named below are defined in
`references/evidence-guide.md`.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| env-recorded | The repro report's environment record (Environment section), read against the issue's stated target versions/platform and the repo-facts block's bug-report template asks | The record names the tool's version or revision (a release number, or a commit/branch for source builds), how it was obtained when the template asks, and the OS/platform; and it names every dependency, runtime, build profile, or configuration value that the issue or thread says affects the trigger. Fail if there is no environment record, or if a factor the issue says changes the behavior is missing. | required |
| env-faithful | The environment record compared with the issue body's stated versions/platform and the thread highlights (e.g. where maintainers could or could not reproduce) | The attempt runs on the issue's target environment, or on a newer/current release of it, or every deviation that could change the outcome (an older version than the issue's, a different OS/driver/shell/backend) is named in the report. Fail if the report silently uses an older version or a different platform than the issue requires, or extends its result to an environment its artifacts do not cover. | required |
| steps-rerunnable | The repro report's steps, commands, and inputs (file contents, config, sample data, flags) | A stranger with only public material could rerun the main trigger: the trigger command is shown exactly, and every input it depends on is shown, publicly linked, or described precisely enough that a stranger would build an equivalent one (every property the trigger depends on is named). Secondary variants and comparison runs may be described in prose. Fail if the steps rely on private code, an unshared config or dataset, or omit or leave vague an input or setting the trigger needs (e.g. the driver on a driver-specific issue, a config whose trigger-relevant contents are not stated). | required |
| trigger-faithful | The commands/inputs in the steps compared line by line with the trigger described in the issue body (syntax, input, flags, conditions) | The steps exercise the same trigger the issue describes; any change to it is justified and shown to preserve the trigger (e.g. a minimal reduction that still produces the issue's behavior). Fail if the input, syntax, or conditions were altered so that a different code path or error is exercised. | required |
| behavior-shown | The report's artifacts (captured output, exit codes, logs, screenshots described with concrete content) read against the issue's described actual behavior, and the report's expected-vs-actual statement | For a reproduction: a captured artifact of the main trigger run shows the issue's specific behavior (the same error, wrong value, crash type, or symptom), and the report states what was expected versus what was observed. For a cannot-reproduce: a captured artifact shows the actual outcome of at least one real run of the issue's trigger, and the report states that result. Once the main artifact is shown, comparison runs, variants, and repeat counts may be described in prose without a transcript of their own. Fail if there is no artifact of the main trigger run, if artifacts show only that the tool runs, or if the artifact shows an adjacent behavior (a graceful validation error presented as a crash, a different error message, a different symptom). | required |
| claims-match-evidence | Every claim in the claim comment and repro report (reproduced, root cause, scope, certainty) set against the artifacts that back it | The main outcome claim (reproduced, or could not reproduce) is backed by a shown artifact of the main trigger run. Secondary statements (a described control or variant result, a repeat count) may stay in prose when they are concrete (name the change made and the result observed) and consistent with the shown artifact. Diagnoses, hypotheses, and explanations of why a cannot-reproduce may not have triggered are labeled as inference ("I think", "likely", "may") and need no artifact. A cannot-reproduce is stated as such, with the attempt shown and what differed named. Fail if the main outcome rests on no shown artifact, if the package claims success, a verified cause, or a scope ("guaranteed", "every version", "verified the race") that its artifacts do not show, if secondary prose claims a behavior different from or beyond the shown artifact, or if it narrates an artifact as something it is not. | required |
| claim-specific | The candidate claim comment, read against the issue's title and body | The comment refers to this issue's specific behavior or code area and states a concrete, modest next step (investigate, test, report back). Fail if it is interchangeable boilerplate or a bare +1, demands assignment or reservation, or promises a fix, a guaranteed outcome, or a deadline. | required |
| ai-disclosure | The repo-facts block's contribution policy (AI-use policy) read against the claim comment and repro report. Treat every package as AI-assisted work. | If the policy requires disclosure of AI use that covers issues or comments (or "all AI usage in any form"), the comments contain an explicit disclosure that AI assisted, including the tool and the extent when the policy asks for them. If the policy states no AI policy, permits assistive use without a disclosure ask for comments, scopes disclosure to pull requests only, or requires only that comments be human-written, the check passes when the comments read as the contributor's own words. Fail if a required disclosure is absent. | required |
| control-run | The repro report's artifacts | The report includes a control or comparison run (e.g. the working configuration, the other language setting, the non-triggering input) that isolates the trigger. | preferred |
| template-coverage | The repo-facts block's bug-report template asks, compared with the report | Every field the repo's bug-report template asks for is answered somewhere in the report, in any format. | preferred |

## Verdict rule

- **accept** if every `required` check grades `pass`.
- **reject** if any `required` check grades `fail` or `unclear`. An
  `unclear` on a required check means the needed evidence is absent
  from the package, and proof that cannot be verified is not ready to
  post.
- `preferred` checks are reported but never change the verdict,
  whatever their grade.
- Claim-only drafts (live mode): checks whose evidence is the repro
  report (env-recorded, env-faithful, steps-rerunnable,
  trigger-faithful, behavior-shown, control-run, template-coverage) are
  graded `unclear` with evidence `not yet applicable: claim-only draft`
  and are left out of the rule; claims-match-evidence then reads only
  the claim comment's own claims. The verdict is accept only if
  claim-specific, ai-disclosure, and claims-match-evidence all pass.
