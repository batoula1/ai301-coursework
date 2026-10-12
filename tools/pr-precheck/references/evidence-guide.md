# Evidence guide: where evidence lives in a PR package

Examples come from the practice packages in
`ai301-unit4-starter/eval/packages/`. An eval bundle has these
sections: `## Repo facts`, `## Issue`, `## Thread highlights`,
`## Plan context`, and `## Candidate PR`. The candidate PR holds
`### Title`, `### Description`, `### Commits`, `### Diff`, and
`### Test evidence`.

Live mode on Path Review reads these sources instead:

| Evidence | Live source |
|---|---|
| Plan, including `## Deviations` | `plan.md` in the fork's top folder |
| Diff | `git diff main...HEAD` |
| Commits | `git log --oneline main..HEAD` |
| Title and description | `pr_draft.md` |
| Captured output | `test_evidence.md` |
| PR template | `.github/PULL_REQUEST_TEMPLATE.md` |
| Contribution rules | `docs/CONTRIBUTING.md` |
| Thread | the live issue thread |

## Plan fidelity (harness category: silent-drift)

**Where it lives.** The plan context's "Scope", "Not in scope",
"Files", and deviation or deferral notes; live, `plan.md` and its
`## Deviations`. The diff's file headers (`--- a/...`) and hunks. The
commit list. Each description claim about what the PR contains.

**What good looks like.** Every changed file falls inside the plan's
files, or is a test, or is an entry the repo requires. Every promised
deliverable is in the diff, or its absence is noted in the plan AND
restated in the description.
- pkg-02 changes `rich/console.py`, `tests/test_console.py`, and
  `CHANGELOG.md`, exactly the plan's three files.
- pkg-13 omits `%VAR%` handling, but the plan's deferral note and the
  description both say so.
- pkg-16 defers the start-path behavior the same way.

**What silent drift looks like.**
- *More than the plan.*
  - pkg-03 adds a `truncate()` rewrite and a `resolve_symlinks` option
    (commits "refactor(directory)", "feat(directory)") under "Implements
    the posted plan exactly".
  - pkg-06 adds a member-rename pass and a `MenuFollowsWindowTheme`
    setting under "No functional changes outside the system menu fix".
  - pkg-09 adds `--time-format`, help-text rewording, and an `error.rs`
    edit under "exactly as planned".
  - calib-03 silently rewrites `print/property.js`, a file the plan
    never names, under "no changes beyond it".
- *Less than the plan.* pkg-17's plan promises a docs update; the diff
  has only `reactive.ts` and its test, yet the description says the
  docs "now document the limitation".

## Test evidence (harness category: not-tested)

**Where it lives.**
- The plan context's "Test plan": each named repro case, its
  expected-after, the controls, and the suite command.
- The repro evidence's own commands.
- The PR's `### Test evidence`, plus any before/after in the
  description.
- The repo facts' stated test commands.
- Live: the before/after and check output pasted into the
  description, with `test_evidence.md` as backing.

**What decisive looks like.** The plan's repro command, re-run on the
fix, with its after-output, next to the before.
- pkg-02 shows the repro script's three output lines before and after.
- pkg-19 shows `diff <(./yq . source.yml) source.yml && echo IDENTICAL`.
- calib-01 shows `parsed OK` after a `DuplicateOptionError` before.
- The suite is named with its outcome: "`pytest tests/ -q` passes
  (1462 passed, 12 skipped)".

**What not-tested looks like.**
- pkg-04: "Tested locally ... colors work now", with no repro command
  and no before/after.
- pkg-10: "verified working on my machine for a full day", with the
  plan's delayed-connect repro and `yarn test` never shown.
- pkg-07: only the single-file control, the path that never broke, is
  run.
- pkg-14: the evidence runs a GET with header casing, not the issue's
  POST Content-Type repro.
- calib-04: the plan names two failure modes and the evidence re-runs
  only the first.

## Diff quality (harness category: unreviewable)

**Where it lives.** Every hunk in `### Diff` (live:
`git diff main...HEAD`), and the commit messages.

**What reviewable looks like.** Each hunk is the fix, its test, or a
required entry.
- pkg-11: one function plus tests.
- pkg-19: one emitter hunk plus a test.

Lines shown as removed and re-added unchanged inside the fix's own
hunk are diff noise, not churn. Examples are pkg-02's `export_html(`
lines and pkg-12's comment line.

**Debris tells.**
- pkg-12: a commented-out `eprintln!("DBG ...")`, a commented-out
  "first attempt", a dead `_unused_buffer_probe` fn, a `// TODO: ring
  buffer?` on an import, a separate hunk re-indenting an unchanged
  `while` loop, and commits "wip", "fix", "fmt + cleanup".
- pkg-15: a live `eprintln!("DEBUG accept err ...")`, commented-out
  lsof experiments, a dead `count_open_fds` behind
  `#[allow(dead_code)]`, import reshuffling, and a struct-literal
  reformat; the commit is "misc cleanups while debugging".
- pkg-18: a commented-out debug `eprintln!`, and separate hunks that
  re-print identical `println!` lines.

## Standards and comms (harness category: standards-wall)

**Where it lives.**
- *Eval:* the repo facts' "pull requests" line (template sections,
  checklist, issue-link line, changelog or whatsnew asks, title
  convention) and "contribution policy" line (AI rules), plus the
  thread highlights' maintainer comments (role in parentheses).
- *Live:* `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`,
  and the `scope.md` house rules.

**What compliant looks like.**
- pkg-08 answers lazygit's checklist item by item.
- pkg-13 fills neovim's Problem and Solution sections.
- pkg-16 has "fixes #23471" and before/after examples.
- pkg-05 answers fdncred's uniqueness question and
  kronberger-droid's caveat by name.
- pkg-11 meets ripgrep's own-words rule: "this text is in my own
  words".

**What a standards wall looks like.**
- pkg-01: pandas requires a checklist with "closes #xxxx" and a
  whatsnew entry. The description has neither, and the diff has no
  `doc/source/whatsnew` file.
- pkg-20: ghostty's policy says "All AI usage in any form must be
  disclosed, stating the tool used and the extent of the assistance".
  The description has no disclosure. A disclosure that named only the
  extent and not the tool would also fail.

**Path Review specifics (live).**
- The template's sections are Summary, Issue (`Closes #`), Changes,
  Testing checklist, Screenshots / Demo, and Notes for Reviewers.
- The checklist's "CI is green on this PR" box can only be true after
  the PR is opened. Leave it unticked before then, and show the same
  checks run locally.
- CONTRIBUTING requires removing a seeded bug's
  `@pytest.mark.xfail` marker and any matching `pyproject.toml`
  suppression.
- The `make test-integration` target runs pytest on an empty
  `tests/integration/`. pytest exits 5 ("no tests collected"), and CI
  treats exit 5 as success. Evidence must show that honestly, not
  claim integration tests passed.
