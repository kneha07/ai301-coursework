# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

- Where it lives: in an eval bundle, the plan's stated cause is the "Cause:" line or the "Diagnosis" section of the **Candidate plan**. The behavior it must explain is in the **Repro evidence** block: the numbered steps, the artifact (output, timings, exit codes), the control run, and the Expected/Actual lines. If the plan leans on the thread, the claimed cause is in **Thread highlights**. In live mode, the same facts are in `plan.md`'s diagnosis, in the student's posted repro comment on the issue, and in the live thread.
- What good looks like: the stated cause accounts for both the failing run and the control in the repro evidence, and names the mechanism, not just the symptom. A cause taken from a commenter is acceptable only when a repro artifact is consistent with it; when the repro shows something the borrowed cause cannot explain (for example, the slowdown persists with the pager removed), the repro wins and the diagnosis fails.

## Scope

- Where it lives: the "Scope" section or the "In: ... Out: ..." sentences of the **Candidate plan**, plus the list of changes or files it names. In live mode, the same lines in `plan.md`.
- What good looks like: one change that fits in one pull request, with an explicit not-in-scope line. A bounded plan fixes the diagnosed cause and nothing else. A drive-by rewrite adds refactors, cleanups, adjacent bugs, or new features, or states no boundary at all.

## Executability

- Where it lives: the "Approach", "Changes", or "Files" part of the **Candidate plan**: file paths, function names, the edit at each, and the order of work. Check each named site against the repro evidence and the **Thread highlights**, since a maintainer may have ruled a site out.
- What good looks like: a stranger could open the named file, find the named function, and know what to change without asking the author. "Poke around the editor code and figure out where the history lives" is not executable. A named site that the repro never touches, or that a maintainer has said cannot work, is not executable either.

## Test plan

- Where it lives: the "Test" or "Test plan" part of the **Candidate plan**, read against the numbered steps and the Expected/Actual lines in the **Repro evidence**. In live mode, `plan.md`'s test plan next to the repro steps in the posted repro comment.
- What good looks like: it re-runs the repro steps, or names a test that fails today, and states the specific output that will differ after the fix (the value, the timing, the color, the exit code). A check that would pass whether or not the fix landed ("run the full suite", "undo works") proves nothing.

## Honesty

- Where it lives: the risks, unknowns, and assumptions in the **Candidate plan** (a "Risks" or "Unknowns" section, or hedged phrases inside the diagnosis and approach), compared with the confidence of its claims. Contested or hard points are in **Thread highlights**. In live mode, `plan.md`'s risks and unknowns section, and the `## Deviations` heading at the end, where a mid-build change is recorded.
- What good looks like: claims about cause and fix are backed by something in the repro evidence or are marked as unverified, and the plan names at least one real risk or open question where one exists (a behavior change, an unconfirmed fix site, a maintainer's reservation). False confidence looks like a root cause stated as fact with no artifact behind it, or "should be doable in a few evenings" over a problem the maintainer called hard.

## Comms

- Where it lives: the **Candidate plan comment**, read against the **Candidate plan** (does it say the same thing?), the **Thread highlights** (what maintainers said), and the **Repo facts** block (bug-report template asks, contribution policy, AI-use policy). In live mode, the draft `comment.md`, the live issue thread, and the repo's CONTRIBUTING and AI-policy files.
- What good looks like: the comment states the plan's diagnosis, scope, and test; answers any relevant maintainer statement from the thread by name or content; follows the repo's stated asks (AI disclosure when the policy requires it, own words when the policy requires it); and promises a plan, never a fix date. Boilerplate that fits any issue, a comment that overclaims what the plan supports, or silence about a maintainer's stated limit does not meet the bar.
