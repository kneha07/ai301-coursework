# Procedure: how this skill grades a plan package

## Read order

1. Read `rubric.md` and `references/evidence-guide.md` first. List every check and its weight, and the verdict rule. In live mode, read `scope.md` before that and confirm the issue is in the scoped repo.
2. Read the issue context next (title, body, labels). Note the reporter's exact trigger and the Expected/Actual behavior.
3. Read the thread highlights. Write down every maintainer or collaborator statement about cause, limits, rejected directions, or difficulty. Write down any cause a commenter claims, and mark it "claimed", not "established".
4. Read the repo-facts block. Write down the bug-report template asks, the contribution asks, and the AI policy exactly as worded (disclosure required, own-words required, permissive, or silent).
5. Read the repro evidence before the plan. Write down: the steps, the artifact (output, timings, exit codes), the control run, and the Expected/Actual. This is what the plan's cause must explain, so read it before the plan so the plan cannot frame it for you.
6. Read the candidate plan. Write down its stated cause, its in-scope and out-of-scope lines, its files and edits, its test plan, and its stated risks or unknowns.
7. Read the candidate plan comment last. Write down its diagnosis, scope, and test as stated, and any promise, date, AI-use statement, or reference to the thread.

## Evidence gathering

1. Diagnosis: from step 6 of the read order take the plan's cause, then from step 5 take the control and artifact. Record one line: "cause says X; evidence shows Y; control shows Z". If the cause came from the thread, record who said it and whether the repro evidence agrees.
2. Scope: copy the plan's in-scope and out-of-scope lines, and count the distinct changes in its change list. Record any change the diagnosis does not require.
3. Executability: list each file or function the plan names and the edit it plans there. Record any step that says only "look into", "find", or "figure out". Check each named site against the repro evidence and the thread to see whether it can actually produce the fix, or whether a maintainer has ruled it out.
4. Test plan: copy the test plan's expected result. Record whether it names the repro steps (or a failing test), and the exact output expected to differ after the fix. Record whether the same check would also pass without the fix.
5. Unknowns: list each claim the plan makes about cause or fix, and mark each "backed" (a repro fact supports it), "marked unverified", or "asserted". Record the risks or open questions the plan lists, and any contested or hard point from the thread that the plan does not mention.
6. Comms: compare the comment's diagnosis, scope, and test to the plan's, line by line. Record the maintainer statements from the thread highlights and whether the comment answers them. Record whether the comment has AI-use disclosure or own-words wording, and compare it to the policy recorded from the repo-facts block.
7. Quoted evidence (preferred check): record whether the plan quotes or cites specific repro output, commands, or control results.
8. Use only the package text in eval mode. Do not fetch anything. In live mode, gather issue-side facts from the places the evidence guide names, and the drafts are the candidate side.

## Check execution

1. Run the checks in table order. Grade each one using the notes from evidence gathering, not by re-reading the whole package; re-read only the one part the check names when a note is missing.
2. Apply the pass condition exactly as written. Grade `pass`, `fail`, or `unclear`. Never grade on length, headings, or polish.
3. Grade `fail` when the pass condition's fail clause is met, even if the rest of the plan is strong.
4. Grade `unclear` only when the evidence the check needs is genuinely absent from the package (for example, the plan has no test plan section at all and the comment has none either). If the evidence is present but weak, grade `fail`, not `unclear`.
5. For every grade, write one line naming the fact or quote that decided it. A grade with no quoted fact is not finished.
6. If the procedure or rubric is silent on something you had to decide, add one line to the summary saying what was missing. Do not invent a rule silently.
7. Do not let one check's result change another's grade. Each check is graded on its own evidence.

## Verdict assembly

1. Apply the rubric's verdict rule: `accept` only if every `required` check is `pass`; `reject` if any required check is `fail`.
2. Treat every `unclear` on a required check as `fail`. `preferred` checks are reported but never change the verdict.
3. In the output, name the deciding check or checks: for a `reject`, quote the evidence line of each failed required check; for an `accept`, say that every required check passed and quote the evidence line of the check that was closest to failing.
4. Emit the JSON block last, with one entry per check (including preferred checks) and the verdict, and nothing after it.
