# Evidence guide: where evidence lives in a plan package

Examples below come from the practice packages in
`ai301-unit3-starter/eval/packages/`. In an eval bundle the sections
are: `## Repo facts`, `## Issue`, `## Thread highlights`,
`## Repro evidence`, `## Candidate plan`, `## Candidate plan comment`.

## Diagnosis and grounding

- Where it lives, eval: the cause is in the candidate plan's
  "Diagnosis" line or section (sometimes "Cause:" or "Problem
  statement"), and is often repeated in the plan comment. The behavior
  it must explain is in `## Repro evidence`: the numbered steps, the
  "Control" runs, the artifact output, and the closing
  "Expected/Actual" lines.
- Where it lives, live: the plan's diagnosis in `plan.md`, read
  against the student's posted reproduction comment on the issue and
  any repro output the drafts quote.
- What good looks like: the cause names a mechanism or code site, and
  every control fits it. pkg-02: the plan blames the background-fill
  underflow, and both controls fit (no background, no subtraction;
  width 2, no overshoot).
- What a contradiction looks like: a control runs the blamed part and
  gets correct output, or a step shows the damage happening earlier.
  pkg-01 blames the item tokenizer, but control step 2 parses the same
  items fine without `-v`, and step 4 shows argparse failing before
  the item parser runs. pkg-07 blames FES being tree-shaken out, but
  control step 3 prints a friendly error from the same build. pkg-11
  blames collect, but the top-level collect control returns length 1.
  pkg-16 blames the post-read cast, but step 4 shows the zeros already
  gone in pyarrow's inferred int64 table. calib-03 adopts a thread
  claim about key bindings, but step 3 shows the same 26 s with no
  pager at all. The repro evidence outranks the issue and the thread.

## Scope

- Where it lives: the plan's "Scope", "In scope / Not in scope",
  "Proposed changes", "Files and areas", and numbered approach steps;
  extra work also hides in the comment ("while I'm in there", "starting
  with the migration").
- What good looks like: one change aimed at the reproduced defect,
  plus its regression tests, with larger work named as deferred.
  pkg-08 fixes the grouping in `delpaths_sorted` and defers the
  tombstone rework; pkg-13 splits the mouse-selection symptom into its
  own issue; pkg-02 applies the same clamp at the sibling site in the
  same function.
- What scope creep looks like: the fix wrapped in a redesign. pkg-06
  adds preload regeneration, a containerd upgrade, a cross-runtime
  abstraction, and a CI matrix. pkg-12 adds a printer rewrite, a new
  option, and a module split. pkg-15 adds an undici migration, a
  settings field, UI rework, and a retry framework on top of a correct
  one-constant fix. pkg-19 adds a state-machine rewrite, a new prop,
  and a test-harness migration.

## Executability

- Where it lives: the plan's "Files", "Change", and "Approach"
  sections; named functions, handlers, or paths.
- What good looks like: a named file or code area plus one chosen
  change. calib-01 names the push callback in
  `pkg/gui/controllers/sync_controller.go` and what to add. pkg-14
  names the reattach path in `zellij-server` and the drain approach,
  and says the exact function will be pinned with `zellij --debug`;
  that stated unknown still passes.
- What unbuildable looks like: pkg-10 ("profile ... optimize whatever
  the profiling turns up", no files); pkg-17 ("gocui? tcell? not
  sure"); pkg-18 (recover() "somewhere", "upstream or vendored,
  whichever turns out to be easier"); calib-02 ("poke around the
  editor components").

## Test plan

- Where it lives: the plan's "Test plan" or "Test" line, read against
  the repro steps and their expected result.
- What good looks like: the repro re-run with a named post-fix result.
  pkg-02: re-run the repro, expect drawn output and exit 0, controls
  unchanged. pkg-05: backdate the cache file by 25 hours and expect A
  and B both to print and a second request in the access log.
  pkg-19's test (custom class removed at transition end) is also
  decisive; that package fails on scope, not on its test.
- What a vague test plan looks like: calib-04 ("run the full test
  suite and make sure nothing regresses"); pkg-10 ("should feel
  fast"); pkg-17 ("nothing else should feel broken"); calib-02 ("undo
  works after toggling" with no steps).

## Honesty

- Where it lives: "Risk", "Unknown", or "Open question" lines in the
  plan, hedges in the comment, and any "verified" or "tested" claim.
  Live, the `## Deviations` section at the end of `plan.md` records
  what changed during the build.
- What good looks like: pkg-13 says the clamp site "may move one level
  during implementation (named as an unknown, not a certainty)";
  pkg-20 says the per-print cost has not been measured yet; pkg-03
  separates the tools it verified from the ones it will check.
- What false confidence looks like: a guessed cause stated as fact
  ("The TypeError is the tell", pkg-07), or "I traced this" when the
  repro evidence points elsewhere (calib-03).

## Comms

- Where it lives: the candidate plan comment, read against
  `## Thread highlights` (role in parentheses after each name) and the
  "contribution policy" line in `## Repo facts`. Live: the issue
  thread, the repo's CONTRIBUTING and AI policy files, and `scope.md`
  house rules.
- Maintainer direction: comments by OWNER, MEMBER, COLLABORATOR, or a
  CONTRIBUTOR who proposes or rejects an approach. Thread-aware looks
  like pkg-05 (wires the TTL constant in the function the member
  named), pkg-09 (picks option 2 from the collaborator's list and
  names the open PR), pkg-20 (follows the recompute-only-on-change
  direction and avoids the rejected recompute-everywhere approach).
  Thread-blind looks like pkg-04: the owner pointed at
  `src/tui/light_windows.go` and posted a patched binary, and the plan
  is a docs-only workaround whose comment never mentions either.
- AI policy: the policy line decides what the comment must say.
  ghostty (pkg-20) requires disclosing all AI usage, so a comment with
  no disclosure fails. ripgrep (pkg-03, calib-04) requires comments in
  the contributor's own words; those comments say so. fd (pkg-09)
  asks for disclosure in the pull request only, so the comment needs
  none. "No stated AI policy" (pkg-02, pkg-08, calib-01) needs none.
- Repo conventions: only explicit requirements that apply to this
  change. In Path Review, CONTRIBUTING requires removing a seeded
  bug's `@pytest.mark.xfail` marker once the fix makes its test pass,
  and `scope.md` requires a branch named `fix/<issue-number>-<slug>`
  on the student's own fork.
