# Evidence guide: where proof lives in a reproduction package

For every kind of proof the rubric names, this guide says where to find
it and what sufficient evidence looks like. Judge sufficiency, not
shape: a field answered in one plain line counts the same as one under
a heading.

## Environment

- Where it lives:
  - Eval bundle: the "Environment:" line or paragraph at the top of the
    "Candidate repro report" section; the issue's stated target
    (versions, OS, browser, driver, backend) in the "Issue" section;
    maintainer environment notes in "Thread highlights"; the
    "bug reports:" line of the "Repo facts" block for what the repo
    asks contributors to record; "latest release" in "Repo facts" for
    what current means.
  - Live mode: the environment paragraph of the student's draft repro
    comment; the issue body and thread on GitHub for the target
    environment; the repo's `.github/ISSUE_TEMPLATE/` files for the
    fields it asks.
- What good looks like: the record names the tool's version or
  revision (a release number counts as the revision; a commit SHA or
  branch is needed only for unreleased or source builds), the
  install method when the template asks, and the OS/platform, plus
  every dependency, runtime, build profile, or config value the issue
  or thread says affects the behavior. The versions match what the
  issue targets, are a newer/current release, or the difference is
  named. An older version than the issue's, or a different platform,
  that the report does not mention is a silent deviation and fails.

## Steps

- Where it lives:
  - Eval bundle: the steps, commands, and inline file/config contents
    in the "Candidate repro report"; the issue body's own steps for
    comparison.
  - Live mode: the steps in the student's draft repro comment,
    including any linked public gist or repo; the issue's steps.
- What good looks like: a stranger could rerun the main trigger with
  only public material: the trigger command is shown exactly, and every
  input it depends on (file contents, config, data, flags, settings
  such as browser language or driver) is inline, publicly linked, or
  described precisely enough to build an equivalent ("a minimal
  env.yml with a valid dependencies list plus an unrecognized
  `category:` section" names every property that matters). Secondary
  variants and comparison runs may be described in prose. A vague
  stand-in for an input the trigger depends on ("my usual config", "a
  big project") is not sufficient. The commands exercise the issue's
  trigger itself, with the
  same syntax and conditions; a reduction is fine only when it still
  yields the issue's behavior. Steps that live in a private repo, an
  unshared config, or that change the issue's input (a different
  operator, range form, or expression) are not sufficient.

## Behavior shown

- Where it lives:
  - Eval bundle: fenced output blocks, exit codes, logs, and
    described screenshots inside the "Candidate repro report", and its
    "Expected:" / "Actual:" statements; the issue body's described
    actual behavior and quoted error for comparison.
  - Live mode: the output pasted in the draft repro comment
    (and any attached screenshot or log); the issue's description of
    the failure.
- What good looks like: a captured artifact shows the specific
  behavior the issue reports: the same error message or crash kind,
  the same wrong value, the same missing output, and the report says
  what was expected next to what was observed. Compare the artifact
  with the issue, not with the report's narration: a graceful
  validation error is not a panic, a compile error is not a runtime
  path error, garbled output with a live process is not a crash, and a
  version banner or session list only shows that the tool runs. The
  main trigger run needs a captured artifact; once it is shown, a
  control run, variant, or repeat count may be described in prose
  ("dropping `-r` from the same command reports 1, 4, 7, 10"; "ran
  this 5 times") without its own transcript. A control run is strong
  supporting evidence either way. For a cannot-reproduce, the artifact
  must show the real outcome of at least one run of the issue's
  trigger.

## Honesty

- Where it lives:
  - Eval bundle: every sentence in the "Candidate claim comment" and
    "Candidate repro report" that asserts reproduction, cause, scope,
    or certainty, set against the artifacts in the report.
  - Live mode: the same sentences in the student's drafts, against
    what the drafts actually show.
- What good looks like: the report says exactly what happened.
  The main outcome ("reproduced" or "could not reproduce") is backed by
  a shown artifact of the main trigger run; secondary observations (a
  described control result, a repeat count) may be prose when they are
  concrete and consistent with that artifact. Inferences ("I think the
  cause is Y", "my padding may not achieve that") are labeled as
  inferences and need no artifact.
  An honest cannot-reproduce passes when the attempt is shown, the
  outcome is stated plainly, and what differed from the reporter's
  setup is named. It fails when the package claims more than its
  artifacts show: a "verified" root cause with no transcript,
  "guaranteed reproducible" with no output, success narrated over an
  artifact showing a different behavior, or a result generalized to
  environments the attempt did not cover.

## Comms

- Where it lives:
  - Eval bundle: the "Candidate claim comment" read against the
    issue's title and body; both comments read against the
    "contribution policy" line of "Repo facts" (including any AI-use
    policy) and the "bug reports:" template asks.
  - Live mode: the student's draft comments, read against the issue,
    the repo's `CONTRIBUTING.md`, any `AI_POLICY.md` /
    `AI_USAGE_POLICY.md`, and issue templates; in Path Review, the
    house rules in `scope.md`.
- What good looks like: the claim comment could only belong to this
  issue: it names the specific behavior or code area and a concrete,
  modest next step (investigate, test a patch, report back). It does
  not demand assignment, reserve the issue, or promise a fix, a
  guarantee, or a deadline. AI-use disclosure: treat the package as
  AI-assisted work. When the repo's policy requires disclosing AI use
  that covers issues or comments, the comments contain an explicit
  statement that AI assisted, with the tool and extent if the policy
  asks. When the policy states no AI rule, scopes disclosure to pull
  requests only, or only requires comments in the contributor's own
  words, no disclosure is required and comments that read as the
  contributor's own words pass.
