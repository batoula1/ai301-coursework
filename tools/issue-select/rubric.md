# Rubric: is this a good first issue?

## Evidence rules

In eval mode, measure dates against the snapshot capture date. In live mode, measure against today's date. Use the issue body, comment thread, and specific sources below. Labels and stars alone do not prove suitability.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-active | Repo facts: last 5 default-branch commits and maintainer first-response sample; issue comments with OWNER, MEMBER, or COLLABORATOR association. Live: commit history and recent issue threads. | At least one human default-branch commit, merge of a human-authored PR, or maintainer issue response within 90 days. Bot-only updates do not pass. | required |
| repo-in-use | Repo facts: archived flag, latest release, last push to any branch, and commit authors. Live: archive banner, Releases, and branch history. | Repository is not archived, and has a release within 365 days or a human contribution pushed within 90 days. A bot-only push or high star count is insufficient. | required |
| bounded-scope | Issue body, labels, maintainer comments, and linked PR history. | Requests one identifiable contribution. Reject umbrella/tracking work, unresolved design debates, pure usage questions, or maintainer-confirmed changes to core internals. Also reject issues open over 2 years with at least 2 closed unmerged attempts. A short description or missing reproduction steps alone does not fail a bounded task. | required |
| available | Repo facts: assignees and linked PR states; comment thread including mentioned PRs, dated claims, and withdrawals. Live: Assignees, Development, and full thread. | No assignee, open implementation PR, or unwithdrawn claim made or reaffirmed within 30 days. Closed unmerged PRs alone do not block availability. In Path Review live mode, apply scope.md's house rule: other students' claim comments do not block an issue. | required |
| ai-workflow-allowed | Repo facts: contribution policy. Live: root or .github/CONTRIBUTING.md, linked contributor rules, AI policy files, and PR templates. | No explicit ban applicable to AI-assisted contributions. Disclosure, understanding, testing, and human-review conditions pass and must be followed. Explicit policy silence passes; unavailable policy evidence is unclear. | required |
| newcomer-support | Issue labels and maintainer comments. | Has a good-first-issue label or an explicit maintainer offer to guide a newcomer. | preferred |

## Verdict rule

Accept only if every required check passes. Reject if any required check fails or is unclear. Explicit evidence that no restriction or claim exists is different from missing evidence.

Preferred checks never change accept/reject. Rank accepted candidates using the fit profile in scope.md, then newcomer support. For each check, report the evidence and why it passes, fails, or is unclear. Do not invent missing facts.
