# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

kneha07

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/57#issuecomment-5864961166

> Hi! I'd like to pick up #57 as my first contribution here. Reading `agent/tools/tech_detector.py`, the detector counts every file it's given toward the language tally, including anything under `node_modules/` or `build/`, so a repo with a couple of Python source files and several bundled/vendored JS files gets reported as primarily JavaScript.
>
> Next I'm going to reproduce the issue's exact snippet locally, then look at where the detector should exclude vendored/build paths before counting, and report back with what I find before opening a PR.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/57#issuecomment-5864988223

> Reproduction report for #57. **Result: reproduced** on `main` at commit `2f4e82f52efbcfcc57d65b3fa5348672163ca088`.
>
> **Environment**
>
> - Repo: my fork at commit `2f4e82f` (matches upstream `main` at fork time)
> - Python 3.14.3 (macOS arm64, Darwin 23.3.0). Note: the project targets `requires-python = ">=3.11"` and CI/mypy pin 3.11; 3.14.3 is above that target, not on it — flagging in case it matters, though this bug is plain path-string handling with no version-sensitive behavior.
> - Installed with `python3 -m venv .venv && pip install -e ".[dev]"`. No Docker/Postgres/Redis needed: `agent/tools/tech_detector.py` is pure Python and touches no service.
>
> **Steps**
>
> Issue's own snippet, run directly:
>
> ```python
> from agent.tools.tech_detector import TechDetector
> t = TechDetector()
> files = ['main.py','core/app.py','node_modules/lib/index.js','node_modules/lib/util.js','node_modules/x/a.js','node_modules/y/b.js','build/bundle.js','build/vendor.js']
> print(t.execute({'files': files}).data['primary_language'])
> ```
>
> Output:
>
> ```
> JavaScript
> ```
>
> **Control** (same files, minus the vendored/build paths):
>
> ```python
> from agent.tools.tech_detector import TechDetector
> t = TechDetector()
> print(t.execute({'files': ['main.py','core/app.py']}).data['primary_language'])
> ```
>
> Output:
>
> ```
> Python
> ```
>
> **Named unit tests, with the xfail marker defeated:**
>
> ```
> $ python3 -m pytest tests/unit/test_tech_detector.py -v -k "node_modules or build_directory" --runxfail
> FAILED tests/unit/test_tech_detector.py::TestTechDetector::test_node_modules_excluded - AssertionError: assert 'JavaScript' == 'Python'
> FAILED tests/unit/test_tech_detector.py::TestTechDetector::test_build_directory_excluded - AssertionError: assert 'JavaScript' == 'Python'
> 2 failed, 25 deselected in 0.68s
> ```
>
> Both tests are marked `@pytest.mark.xfail(strict=True, reason="issue #57: ...")`, so the suite reports green as shipped; `--runxfail` shows the real failure.
>
> **Expected:** `primary_language` reports `Python` in both the issue's example and the two named tests, since the only real source files are Python.
>
> **Actual:** with `node_modules/` and `build/` paths present, the detector counts them toward the language tally and reports `JavaScript`; with those paths removed, it correctly reports `Python`. The failure is specific to unfiltered vendored/build paths, not to language detection generally.
>
> Next I'll look at excluding `node_modules/` and `build/` (and similar vendored/output directories) from the file list before counting, remove the two `xfail` markers, and open a PR.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Hand-graded `calib-02.md` (worksheet warm-up, not scored) against the drafted rubric
   before spending any credit: a "+1, obviously the null result thing" package with no
   environment, no steps, and no artifact. Every required check failed as expected
   (environment, steps, behavior-shown, honesty, claim specificity all fail), giving
   `reject`, matching the worksheet's own answer.
2. `--limit 6` smoke run (pkg-01 through pkg-06): 6/6 agreement, one match per category
   present in that slice (clear-accept, no-evidence, unfollowable-comms, wrong-target).
3. Full 20-package confirming run (`--save-run eval-run.txt`, the one committed):
   **20/20 agreement (bar: 18/20: PASS)**, category floor met in all five categories
   (clear-accept 8/8, disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3,
   wrong-target 4/4). No revision loop was needed past this point; this is the run
   recorded in `eval-run.txt`.

**Package analysis**

`pkg-20` (category: disclosure — the one-package category the floor exists for). Gold
verdict: `reject` — "excellent repro on every proof check; ghostty's stated AI policy
requires disclosing all AI usage and the comments do not disclose (course packages are
treated as AI-assisted work); the one-item category the floor exists for." My rubric
also graded it `reject`. The package's repo-facts block states ghostty's contribution
policy in full: "All AI usage in any form must be disclosed, stating the tool used and
the extent of the assistance." The candidate claim comment and repro report are both
technically excellent (a real GTK/Wayland repro with a control run, exact commands,
matching artifacts) but neither comment mentions AI assistance anywhere. My rubric's
"Conventions and AI-disclosure followed" check reads the contribution-policy line first
and treats it as a required, standalone gate independent of every other check: a
package can pass environment, steps, behavior, and honesty perfectly and still be held
if the one stated disclosure requirement in the repo-facts block goes unmet. That
separation — never letting proof quality buy back a missed disclosure requirement — is
exactly what this category is built to test, and is why the check is written as its own
row rather than folded into "Comms" generally.

**Check rationale**

The "Conventions and AI-disclosure followed" check, quoted as it now reads in
`tools/repro-check/rubric.md`:

> Pass if, when the stated policy requires disclosing AI assistance, the comments
> disclose it as required; pass automatically if the policy is silent or only
> permissive-with-responsibility (no disclosure ask). Fail only on a stated disclosure
> requirement that the comments do not meet.

I wrote it this way after reading pkg-05, pkg-07, pkg-09, and pkg-20 side by side: the
eval set includes repos with three different postures (conda: permissive-with-
responsibility and no disclosure ask; p5.js and fd: conditional, disclosure required
alongside understanding/testing conditions; ghostty: disclosure required outright,
full stop). A check that fails on "any AI use" would wrongly reject conda- and
fd-style permissive cases; a check that only ever passes would miss ghostty. Writing
the pass condition around the repo's *stated* requirement, rather than around whether
AI was used at all, is what lets one check clear all three postures correctly.

**Trade-offs**

This check only reads the repo-facts block's stated policy text; it does not weigh how
strictly that policy is enforced in practice, or whether a disclosure that exists but
is vague ("I used some AI tools") would satisfy a policy asking for the tool name and
the extent of assistance. On the eval set this doesn't cost anything, since every
policy-bearing package either states no requirement or states one clearly enough to
grade as met/unmet outright (pkg-20's ghostty policy is unmet by total silence, not by
a vague attempt). I accept that a partially-worded disclosure in a live-mode draft
might need a human judgment call the check's binary pass condition doesn't capture,
since "states the tool and the extent" is a strictness the check doesn't itself
enforce — it only checks that disclosure happened at all when required. I re-ran
`--only pkg-05,pkg-07,pkg-09,pkg-20` (one canary per policy posture, plus the scored
disclosure case) after finalizing this check's wording, and all four still agreed with
gold, so the wording did not regress any of the other postures it has to distinguish.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
