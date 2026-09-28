# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Maintainer alive | Repo facts: "last 5 default-branch commits" (dates and, if visible, whether any merge an outside PR) and "maintainer first-response sample" | Pass if at least one of the last 5 default-branch commits is dated within 180 days of the capture date, OR at least one entry in the maintainer first-response sample shows a maintainer/owner/collaborator comment within 30 days of that issue's open date. Fail otherwise. | required |
| Repo not dead | Repo facts: "archived:" flag, "last push to any branch", "latest release" | Fail immediately if archived: yes. Otherwise pass if last push to any branch is within 365 days of the capture date. Fail otherwise (a stale release alone does not fail this check; many healthy repos release rarely). | required |
| Bounded scope | Issue title, body, and comment thread | Fail if the issue explicitly says its sub-items are meant to be split into separate tracked issues/PRs (a true tracking issue), if the thread shows the design is still being debated with no maintainer decision recorded, or if a maintainer states the fix touches core internals. Fail if it is a pure usage/support question ("how do I...") with no requested code change. A checklist of edits that all serve one coherent outcome (e.g., add one new doc page and update the handful of existing pages that should now point to it; or one bug diagnosed with multiple named root causes to fix) is one bounded task, not an umbrella — pass it. Only fail as an umbrella when the sub-items are independent features/fixes that could each stand as their own separate issue. A terse body or an unreproduced bug report are also not by themselves unscoped. | required |
| Good-first-issue label or maintainer-filed | Issue labels and "opened by" line | Pass if the issue carries a "good first issue" (or equivalent) label, or was opened by a maintainer (OWNER/MEMBER/COLLABORATOR author association), or was opened by a regular CONTRIBUTOR/NONE author but describes a single well-specified, self-contained task. | preferred |
| Unclaimed | Repo facts: "this issue: assignees:" and "linked PRs:" line, plus the Comments section | Fail if assignees is non-empty, or if any linked PR is open, or if the comment thread contains a "working on this" / "I'll take this" claim that a maintainer acknowledged with no evidence the claimer went stale. Pass if the only claim is a year+ old acknowledged claim with no PR ever produced and a bot's stale-issue notice afterward (nobody followed through), or a single unanswered claim comment with no linked PR. | required |
| No AI-contribution ban | Repo facts: "contribution policy" line | Fail only if the policy states an outright ban on AI-generated code or documentation. Pass if the policy states conditions (disclosure, human review, testing) or is silent/absent. | required |
| Small blast radius | Issue body | Pass if the described change serves one coherent outcome and stays within one area of the codebase (a handful of related files for one feature/doc topic counts as one area), rather than sweeping across unrelated modules or the whole codebase. | preferred |

## Verdict rule

Accept only if every `required` check passes. Any `required` check that
grades `fail` or `unclear` rejects the issue. `preferred` checks never
change the verdict; they only rank accepted issues (more passing
preferred checks ranks higher). `unclear` on a required check counts as
`fail`: a first issue whose liveness, scope, claim status, or policy you
cannot verify from the given evidence is not one to take.
