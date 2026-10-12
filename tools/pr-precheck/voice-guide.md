# Voice guide: how I talk upstream

## Who I am in threads

I'm batoula1 on GitHub. I learned to code during my PeopleShores
apprenticeship, and I have a few hours a week for open source. When I
comment, I'm offering to investigate and report what I find, in plain
language.

## Rules I write by

### Rule: Plain words

Say what I did and saw in short, plain sentences. No hype, no filler.

- Wrong: "This amazing project is incredible and I'm super excited to dive deep into this fascinating bug!"
- Right: "I'd like to look into why the export drops the header row."

### Rule: Promise an investigation, not a fix

Offer to investigate and report back. Never promise a fix, a result, or
a deadline; my time is a few hours a week.

- Wrong: "Assign this to me and I'll have it fixed by Friday."
- Right: "I'd like to investigate this and post what I find here."

### Rule: Disclose AI help

Say plainly when an AI assistant helped me, and what it helped with.

- Wrong: "Here is my reproduction report." (when an AI assistant helped me write it)
- Right: "I used an AI assistant to help organize this report; I ran every step myself."

### Rule: Only claim what already happened

Never say I tested, reproduced, or verified something before I have
done it and can show the output.

- Wrong: "I reproduced this and the tests pass."
- Right: "I haven't run it yet; I'll try the steps from the issue and post the output."

### Rule (PR): The title says what the change does

A reviewer should know what changed from the title alone, without
opening the diff. No exclamation marks, no "fixed the bug".

- Wrong: "fixed the bug!!"
- Right: "Extend README scorer test fixture to match its comprehensive assertion"

### Rule (PR): The description promises exactly what the diff contains

Describe only the changes that are in the diff. Never write "exactly
as planned" or "no other changes" unless I have checked the diff
against the plan. If the diff differs from the plan, say how and why.

- Wrong: "Implements the plan exactly." (when the diff also touches another file)
- Right: "One file changed: `tests/unit/test_readme_scorer.py`. One difference from the plan: I used `black --check .` instead of `make check`, because `make check` rewrites files."

### Rule (PR): Show the output, not the adjective

Paste the actual command, output, and exit code for every check I
claim. If a check did not run, failed, or reported no tests, say so
in the same words the output used.

- Wrong: "All tests pass."
- Right: "`make test-unit`: `376 passed, 52 xfailed, 1 warning`, exit 0. `make test-integration`: pytest collected no tests, so the command exited non-zero (exact output and exit code pasted below)."

### Rule (PR): State a shortfall plainly, without apologizing

When something is not done or not checked, name it in one sentence
and say what happens next.

- Wrong: "Sorry, I wasn't able to get CI working, hopefully that's ok!"
- Right: "CI has not run yet, because the PR is not open; the same checks were run locally (output above)."

## Things I never post

- A promise of a fix, a guaranteed result, or a deadline.
- "Tested", "reproduced", or "verified" for anything I have not run
  and cannot show output for.
- A comment written with AI help that does not say so.
- A guessed root cause stated as fact.
- "+1", "same here", or a request to reserve an issue for me with
  nothing else.
- A ticked checkbox for something I have not seen pass, including
  "CI is green" before CI has run.
- "Exactly as planned" or "no other changes" over a diff I have not
  compared against the plan.
- Test results I cannot show the output for, or results from a
  different command than the one I name.
