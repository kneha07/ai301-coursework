# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

kneha07

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/57#issuecomment-5988583710

> Plan for #57, built from my own reproduction above (commit `2f4e82f`).
>
> **Cause.** One correction to my earlier claim comment: the detector does filter vendored paths. `_should_skip_file` in `agent/tools/tech_detector.py` matches patterns like `"/node_modules/"` and `"/build/"` by substring, and those patterns need a leading slash. Top-level paths like `node_modules/lib/index.js` have none, so they aren't skipped. I called it directly: `node_modules/lib/index.js` -> `False`, `build/bundle.js` -> `False`, but `src/node_modules/a.js` -> `True`. That matches my repro: the snippet prints `JavaScript`, and the control without those paths prints `Python`.
>
> **Change (one PR).** In `_should_skip_file`, test the patterns against `"/" + filepath` so a top-level directory matches. The pattern list stays as it is. I'll also remove the two `xfail(strict=True)` markers on `test_node_modules_excluded` and `test_build_directory_excluded` and add one small parametrized test for the helper.
>
> **Not touching.** How the primary language is chosen (it uses `sorted(languages)[0]`, i.e. alphabetical, not file counts), the pattern list, Windows `\` paths.
>
> **How I'll check it.** Re-run the issue's snippet: `JavaScript` today, expected `Python`. Re-run the two tests with `--runxfail`: 2 failed today, expected 2 passed. Control (`main.py`, `core/app.py`) should keep printing `Python`.
>
> **Unknowns.** The fix also makes top-level `vendor/`, `dist/`, `.venv/` and `__pycache__/` skip, which goes beyond the two directories the issue names. I think that's the list's intent, but please say if you'd rather scope it narrower. I haven't yet checked for other callers of `_should_skip_file`; I'll do that before editing.
>
> I'm using Claude to help with the investigation and edits; I'm running and reading every command and diff myself.

---

## Your branch

**Branch**

fix/57-skip-top-level-vendor-paths

**Evidence**

Before (my unit 2 repro, re-run on `main` at `2f4e82f` before touching any code):

```
$ python3 - <<'PY'
from agent.tools.tech_detector import TechDetector
t = TechDetector()
files = ['main.py','core/app.py','node_modules/lib/index.js','node_modules/lib/util.js','node_modules/x/a.js','node_modules/y/b.js','build/bundle.js','build/vendor.js']
print(t.execute({'files': files}).data['primary_language'])
PY
JavaScript

$ python3 - <<'PY'      # control: same files minus the vendored/build paths
from agent.tools.tech_detector import TechDetector
t = TechDetector()
print(t.execute({'files': ['main.py','core/app.py']}).data['primary_language'])
PY
Python

$ python3 -m pytest tests/unit/test_tech_detector.py -v -k "node_modules or build_directory" --runxfail
tests/unit/test_tech_detector.py::TestTechDetector::test_node_modules_excluded FAILED [ 50%]
tests/unit/test_tech_detector.py::TestTechDetector::test_build_directory_excluded FAILED [100%]
======================= 2 failed, 25 deselected in 0.19s =======================
```

After (same three commands on branch `fix/57-skip-top-level-vendor-paths`, commit `d689861`):

```
$ python3 - <<'PY'     # the issue's snippet, unchanged
...
PY
Python

$ python3 - <<'PY'     # control, unchanged
...
PY
Python

$ python3 -m pytest tests/unit/test_tech_detector.py -v -k "node_modules or build_directory" --runxfail
tests/unit/test_tech_detector.py::TestTechDetector::test_node_modules_excluded PASSED [ 25%]
tests/unit/test_tech_detector.py::TestTechDetector::test_build_directory_excluded PASSED [ 50%]
tests/unit/test_tech_detector.py::TestTechDetector::test_should_skip_file_top_level_and_nested[node_modules/lib/index.js-True] PASSED [ 75%]
tests/unit/test_tech_detector.py::TestTechDetector::test_should_skip_file_top_level_and_nested[src/node_modules/lib/index.js-True] PASSED [100%]
======================= 4 passed, 31 deselected in 0.14s =======================

$ python3 -m pytest tests/unit/test_tech_detector.py -q
...................................                                      [100%]
35 passed in 0.12s

$ ruff check agent/tools/tech_detector.py tests/unit/test_tech_detector.py
All checks passed!
$ ruff format --check agent/tools/tech_detector.py tests/unit/test_tech_detector.py
2 files already formatted
$ mypy agent/tools/tech_detector.py
Success: no issues found in 1 source file
```

(The `-k` filter now selects 4 tests rather than 2 because two ids of the new parametrized test contain `node_modules` and `build`.)

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full 20-package run (`--save-run eval-run.txt`, the one committed): **18/20 agreement
   (bar: 18/20: PASS)**. Category floor met in all five categories: `clear-accept 6/7
   scope-creep 4/4 thread-convention 1/2 unbuildable 3/3 wrong-cause 4/4`. Disagreements:
   pkg-14 (gold accept, I graded reject) and pkg-20 (gold reject, I graded accept).

No second full run was needed, since the first met the bar and the floor. I changed none of
the skill files after it.

**Package analysis**

`pkg-20` (category: thread-convention). Gold label: `reject`. In the saved full run my rubric
graded it `accept`, so that is one of the two disagreements in `eval-run.txt`. The package is
a ghostty plan with a strong diagnosis (stale `prev` pointer after mid-print page growth,
isolated by a control run), a bounded scope that follows the maintainer's stated direction, and
a stated risk. Its repo-facts block says: "All AI usage in any form must be disclosed, stating
the tool used and the extent of the assistance", and the candidate plan comment never mentions
AI. The failing check is "Comment is consistent and thread-aware", whose pass condition requires
meeting "any stated repo requirement, including disclosing AI assistance when the policy requires
it". Every other check on this package is a genuine pass, so the package is only held by that one
check, and a grader that lets the strong diagnosis buy back the missing disclosure accepts it.

To see why, I re-graded just pkg-20 afterwards (`--only pkg-20`, about $0.20, no file changes).
That time it returned `reject` and failed exactly that check: "repo AI_POLICY.md requires
disclosure of all AI usage stating tool and extent, but the candidate plan comment contains no
AI-use disclosure at all". So the check text is doing the right thing, and the saved run's
accept came from the grader skipping the policy line once, not from a gap in the rubric. I left
the rubric alone rather than tune it to one sample. The partial run doesn't touch `eval-run.txt`,
which still shows the accept.

**Check rationale**

The "Comment is consistent and thread-aware" check, quoted as it reads in
`tools/plan-check/rubric.md`:

> Pass if the comment states the same diagnosis, scope, and test the plan does, responds to what a maintainer said in the thread when one said something relevant (a stated limit, a rejected direction, a hard-fix warning), and meets any stated repo requirement, including disclosing AI assistance when the policy requires it and writing in the author's own words when the policy requires it. Fail if the comment contradicts or oversells the plan, ignores a maintainer's relevant statement, promises a fix or a date instead of a plan, is boilerplate that fits any issue, or misses a stated repo requirement. A repo whose policy has no disclosure ask needs none.

It reads that way because the thread-convention category has only two scored packages, so a
rubric with no comms check cannot buy them back on volume. I wrote it after reading the four
calibration packages: calib-02's comment promises "a PR up soon" with no plan (promise and
boilerplate clause), calib-04's repo policy asks for human-written comments and its owner said
the deep fix is hard (the own-words and maintainer-statement clauses), and calib-03's comment
repeats a commenter's diagnosis (the consistency-with-plan clause, with the diagnosis check
catching the borrowed cause). I chose one check that reads the comment against the plan, the
thread highlights and the repo-facts block rather than three separate ones, because each
failure shares the same evidence trio and I wanted the pass condition to describe the outcome (does
the comment match the plan and the room) rather than a list of sections. The last sentence
("A repo whose policy has no disclosure ask needs none") is there so a repo with a permissive
policy does not fail a comment for lacking a disclosure it was never asked for.

**Trade-offs**

The check I'm describing gives up leniency on one thing: it treats a missing stated repo
requirement as a hard fail however good the rest is. That is the right call for pkg-20, and I
would keep it, but it means it cannot weigh a partial or vaguely worded disclosure. I haven't
tested a package like that.

The case my rubric does miss is pkg-14 (gold accept, I graded reject). My "A stranger could
start building" check requires the plan to name "the files or functions to change", and pkg-14
says "exact functions to be pinned in the PR after tracing the query issuance with debug logs".
The staff label treats that as buildable in a large unfamiliar codebase; my check holds it, and
my "Unknowns stated" check failed it too. I accept that miss: a plan with no named function is
not something I'd want to post upstream either, and loosening the check could let the unbuildable
packages through (pkg-10, pkg-17 and pkg-18 all agreed).

Because I changed no checks after the full run, I added no canaries and have no flip to report;
the full run's tallies are the evidence nothing else moved.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
