# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/57

**Verdict output**

```
Scope: candidates confirmed inside codepath/pathreview-ai301-fa26-s3 (the only
scoped source). House rule applied: student claim comments in the thread are
ignored as a claim signal on this repo; an open linked pull request still
counts as a real claim and is graded as such.

Ranked read-out (accepted, in fit order):

1. issue-57 — Tech detector counts vendored and build-output files, skewing
   language detection. Fit: a real, bounded code bug in the agent-tooling
   family (agent/tools/tech_detector.py) with an exact repro snippet, no
   open linked PR, matches the stated interest in tracing a bug through an
   unfamiliar service's tests rather than a fixture-only fix.
2. issue-67 — Review creation does not verify profile ownership. Fit: a
   clear, single-function authorization bug (api family) with no open
   linked PR; slightly lower fit than issue-57 only because it carries no
   good-first-issue label and no repro snippet is given in the issue body.
3. issue-73 — README and .env.example disagree about which LLM API key to
   set. Fit: bounded and unclaimed, but a docs-only fix, which trains the
   PR workflow less than a real code bug does.

No rejected candidates in this batch.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/57",
    "checks": [
      {"name": "Maintainer alive", "grade": "pass", "evidence": "default branch pushed 2026-09-16, 11 days before capture; repo owner Aburke225 (COLLABORATOR) filed the issue itself"},
      {"name": "Repo not dead", "grade": "pass", "evidence": "archived: false; last push to any branch 2026-09-16"},
      {"name": "Bounded scope", "grade": "pass", "evidence": "single fix: exclude node_modules/ and build/ paths in tech_detector.py, with an exact before/after repro snippet in the body"},
      {"name": "Unclaimed", "grade": "pass", "evidence": "assignees: none; no open linked PR in the timeline (3 student comments only, which the Path Review house rule does not treat as a claim)"},
      {"name": "No AI-contribution ban", "grade": "pass", "evidence": "docs/CONTRIBUTING.md states no policy on AI tooling at all"},
      {"name": "Good-first-issue label or maintainer-filed", "grade": "pass", "evidence": "labels include good first issue; opened by Aburke225 (COLLABORATOR)"},
      {"name": "Small blast radius", "grade": "pass", "evidence": "change is confined to one file, agent/tools/tech_detector.py"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/67",
    "checks": [
      {"name": "Maintainer alive", "grade": "pass", "evidence": "same repo facts: last push 2026-09-16"},
      {"name": "Repo not dead", "grade": "pass", "evidence": "archived: false; last push 2026-09-16"},
      {"name": "Bounded scope", "grade": "pass", "evidence": "single-function fix: scope create_review() through Profile.user_id the same way get_review()/list_reviews() already do"},
      {"name": "Unclaimed", "grade": "pass", "evidence": "assignees: none; no comments; no open linked PR"},
      {"name": "No AI-contribution ban", "grade": "pass", "evidence": "same CONTRIBUTING.md, no AI policy stated"},
      {"name": "Good-first-issue label or maintainer-filed", "grade": "pass", "evidence": "no good-first-issue label, but opened by Aburke225 (COLLABORATOR)"},
      {"name": "Small blast radius", "grade": "pass", "evidence": "confined to core/services/review_service.py"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73",
    "checks": [
      {"name": "Maintainer alive", "grade": "pass", "evidence": "same repo facts: last push 2026-09-16"},
      {"name": "Repo not dead", "grade": "pass", "evidence": "archived: false; last push 2026-09-16"},
      {"name": "Bounded scope", "grade": "pass", "evidence": "make README.md and .env.example agree on the LLM API key name, a two-file docs edit"},
      {"name": "Unclaimed", "grade": "pass", "evidence": "assignees: none; no open linked PR (26 comments are student claim comments, not counted as a block per house rule)"},
      {"name": "No AI-contribution ban", "grade": "pass", "evidence": "same CONTRIBUTING.md, no AI policy stated"},
      {"name": "Good-first-issue label or maintainer-filed", "grade": "pass", "evidence": "labels include good first issue; opened by Aburke225 (COLLABORATOR)"},
      {"name": "Small blast radius", "grade": "pass", "evidence": "confined to README.md and .env.example"}
    ],
    "verdict": "accept"
  }
]
```
```

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. `--limit 6` smoke run (issue-01,02,03,04,05,06): 5/6 agreement, issue-01 missed
   on "Bounded scope" (a 5-file docs checklist misread as an umbrella).
2. Revised "Bounded scope" wording to distinguish a coherent multi-file task from
   a true umbrella; re-ran the same `--limit 6` smoke set: 6/6 agreement.
3. Full 20-issue run: 18/20 agreement (bar: 18/20: PASS), category floor met in
   all five categories. Misses: issue-15 (scope, graded accept) and issue-19
   (clear-accept, graded reject on "Bounded scope").
4. Revised "Bounded scope" again (one bug with multiple named causes is not an
   umbrella) and revised "Unclaimed" to add a staleness carve-out, aimed at
   issue-09; re-ran a full confirming pass with `--save-run`: agreement dropped
   to 17/20 — the new "Unclaimed" wording flipped issue-09 to a match, but two
   other issues flipped the other way on this run (model grading has run-to-run
   variance on borderline cases), landing below the bar.
5. Investigated the two flips from run 4 (issue-09, issue-20) directly against
   their bundles. Added a "Bounded scope" clause for issues that hinge on an
   explicitly unresolved ("TBD") detail with no maintainer decision, to catch
   issue-20 (an unspecified logo asset). Re-ran `--only` on that issue plus six
   previously-passing accepts to check for regressions: it broke two of them
   (issue-01, issue-04), so this clause was reverted as over-broad.
6. Full 20-issue confirming run (`--save-run eval-run.txt`, the one committed):
   **19/20 agreement (bar: 18/20: PASS)**, category floor met in all five
   categories (claimed 4/4, clear-accept 8/8, dead-repo 3/3, policy 1/1,
   scope 3/4). This is the run recorded in `eval-run.txt`; its single miss is
   issue-15.

**Issue analysis**

`issue-15` (category: scope). Gold verdict: `reject` — "years of design debate
and two abandoned PRs behind a friendly label". My committed run graded it
`accept`. The bundle shows a 2021 feature request with a `good first issue`
label, two linked PRs (`zulip/zulip#20840` and `#23123`), both **closed**
(unmerged), and repeated claim/unassign churn going back to 2022. My rubric's
"Unclaimed" check is written to fail only on an *open* linked PR or an
acknowledged claim with no evidence of staleness; since both linked PRs here are
closed, the check passes by its literal wording, and "Bounded scope" also passes
because the request itself (split `command` and `text` fields) reads as one
coherent, well-specified change. What my rubric never encodes is the
evidence-guide's fourth family signal for this exact pattern: age plus a
history of abandoned attempts is itself evidence the work is harder than it
looks, independent of whether a PR is currently open. My checks treat "closed
PR" only as "not currently claimed," never as "previously tried and dropped,"
so this category's harder cases (issue-15, and the calibration case calib-04)
are where the rubric is weakest.

**Check rationale**

The "Unclaimed" check, quoted as currently written in `tools/issue-select/rubric.md`:

> Fail if assignees is non-empty, or if any linked PR is open, or if the comment
> thread contains a "working on this" / "I'll take this" claim that a
> maintainer acknowledged with no evidence the claimer went stale. Pass if the
> only claim is a year+ old acknowledged claim with no PR ever produced and a
> bot's stale-issue notice afterward (nobody followed through), or a single
> unanswered claim comment with no linked PR.

I wrote it this way after `issue-09` in the eval set: a maintainer acknowledged
a claim comment in 2022, nobody ever opened a PR, and a stale-bot notice fired a
year later — gold says `accept` because the claim is dead in practice, not
because assignees/linked-PR were empty (they were). The check's first pass at
just checking assignees/linked-PRs missed this, so I added the staleness carve-out
naming the exact evidence (age, no PR, bot notice) that turns an acknowledged
claim into a non-claim, rather than leaving "stale" as an unstated judgment call.

**Trade-offs**

The "Unclaimed" check's staleness carve-out only fires for a specific evidence
pattern (year-plus age, no PR, a stale-bot notice). It would still fail-closed
(reject) a claim that is genuinely dead but lacks that exact paper trail — for
example, a maintainer-acknowledged claim from 8 months ago with no bot notice
and no PR. I accept that miss: on a first-issue rubric, requiring concrete
evidence of abandonment before overriding a maintainer's acknowledged claim is
the safer failure mode, since the cost of wrongly rejecting a genuinely
available issue is lower than the cost of steering a newcomer onto one someone
else is quietly still working.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. Fit to interests and time available: issue-57 is a bounded, single-file bug in
   the agent-tools code, with an exact repro snippet already in the issue body,
   which matches wanting to get better at tracing a bug through an unfamiliar
   service without taking on a multi-day investigation.
2. What the verdict identified correctly, and what I weighed that it could not:
   the verdict correctly confirmed the repo is active, the issue is unclaimed
   (no open linked PR despite several claim comments, which the Path Review
   house rule discounts), and the fix is scoped to one file. What the rubric
   can't weigh is how many other students have already piled claim comments
   onto it (three, as of this writing) — the verdict can't tell me whether I'll
   be racing someone to a PR, only that the issue itself is still open and
   sound.
3. Anticipated difficulty in claiming it: low technically (the fix is a path
   exclusion check, and the repro already shows the exact before/after), but
   there is real contention risk since it already has three claim comments
   from other students, so the actual work of Unit 2 will be opening a PR
   quickly rather than diagnosing the bug.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
