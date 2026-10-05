# Plan for #57: tech detector counts vendored and build-output files

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/57
Repro comment I posted: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/57#issuecomment-5864988223

## Diagnosis

My repro (commit `2f4e82f`) shows the issue's snippet printing `JavaScript`, while the same
list without the vendored/build paths prints `Python` (my control). Both named tests fail with
`--runxfail`: `AssertionError: assert 'JavaScript' == 'Python'`.

My claim comment said the detector "counts every file it's given". That was imprecise, and I
checked it against the code before planning. `TechDetector._detect_tech` already filters with
`_should_skip_file`, but that method's patterns all carry a leading slash
(`"/node_modules/"`, `"/build/"`, `"/vendor/"`, `"/dist/"`, `"/.git/"`, `"/__pycache__/"`,
`"/.venv/"`, `"/venv/"`) and are tested with a plain substring match on the path. A repo-root-relative
path such as `node_modules/lib/index.js` has no slash in front of its first directory, so the
pattern never matches. Calling `_should_skip_file` directly on my fork:

```
node_modules/lib/index.js False
build/bundle.js           False
src/node_modules/a.js     True
a/build/b.js              True
/abs/build/x.js           True
vendor/x.js               False
dist/x.js                 False
.venv/lib/x.py            False
```

This explains my repro and my control. The failing snippet's vendored paths are all top-level
(`node_modules/...`, `build/...`), so they are not skipped and `JavaScript` enters the language set.
Nested paths (`src/node_modules/a.js`) are skipped, which is why the filter looks like it works in
casual use. Running the detector on `['main.py','core/app.py','src/node_modules/a.js']` returns
`Python`, consistent with that. The cause is the path matching in `_should_skip_file`,
not the language counting.

## Scope

In scope: one change to `TechDetector._should_skip_file` so a directory pattern matches when the
directory is the first path component. Plus removing the two `xfail(strict=True)` markers that
exist for this issue, and one small regression test for the skip helper.

Not in scope:

- How the primary language is chosen. `_detect_tech` uses `sorted(languages)[0]` (alphabetical
  order), not "most common" as its comment says. I am not touching that; see Risks.
- Adding or removing entries in the skip-pattern list.
- Backslash (Windows) path separators.
- `test_vendor_files_excluded`, which has no assertion today.
- Any other file in `agent/tools/`.

## Files I will touch

- `agent/tools/tech_detector.py`: `_should_skip_file` only.
- `tests/unit/test_tech_detector.py`: remove the `@pytest.mark.xfail(...)` decorator from
  `test_node_modules_excluded` and `test_build_directory_excluded`; add one parametrized test of
  `_should_skip_file`.

No `pyproject.toml` change: I grepped it and there is no suppression tied to #57.

## Approach

1. In `_should_skip_file`, test the patterns against `"/" + filepath` instead of `filepath`, so
   `node_modules/lib/index.js` is checked as `/node_modules/lib/index.js`. The pattern list stays
   exactly as it is. Absolute paths still match (`//abs/build/x.js` contains `/build/`).
2. Remove the two `xfail` markers. They are `strict=True`, so once the fix lands they would XPASS
   and fail the suite if I left them.
3. Add `test_should_skip_file_top_level_and_nested` (parametrized): top-level and nested
   `node_modules/`, `build/`, `vendor/`, `dist/` paths return `True`; `src/main.py` and a file
   named like `builder.js` return `False` (a path with a directory that merely starts with
   `build` must not match).
4. Run the checks in the test plan below; commit; push to a branch on my fork.

## Test plan

Re-run my unit 2 repro steps against the change and compare with the output I posted.

| Step | Before the fix (posted in my repro) | Expected after the fix |
|---|---|---|
| Issue snippet: `print(t.execute({'files': files}).data['primary_language'])` | `JavaScript` | `Python` |
| Control: `['main.py','core/app.py']` only | `Python` | `Python` (unchanged) |
| `python3 -m pytest tests/unit/test_tech_detector.py -v -k "node_modules or build_directory" --runxfail` | `2 failed, 25 deselected` with `assert 'JavaScript' == 'Python'` | `2 passed` |
| Same two tests without `--runxfail`, after the markers are removed | (not applicable: xfail swallowed the failure) | `2 passed`, no XPASS(strict) failures |
| `_should_skip_file('node_modules/lib/index.js')` | `False` | `True` |
| `python3 -m pytest tests/unit/test_tech_detector.py` (whole file) | all green because of xfail | all pass, including the new test |

I will also run `make lint` and `make typecheck` on the changed files, since the PR template asks for
them. Python 3.14.3 is what I reproduced on (project targets 3.11); the change is plain string handling,
and CI on 3.11 will confirm.

## Risks and unknowns

- Behavior beyond the issue's two named directories. Because the fix changes how every pattern
  matches, top-level `vendor/`, `dist/`, `.venv/`, `venv/`, `__pycache__/` and `.git/` paths will now
  also be skipped. That matches what the list was clearly meant to do and what nested paths already
  do, but the issue only names `node_modules/` and `build/`. I have not confirmed maintainers want
  the wider effect and will say so in the PR.
- Primary language is picked alphabetically (`sorted(languages)[0]`), so a repo with real Python and
  real JavaScript source will report `JavaScript` regardless of file counts. I have not reproduced
  that separately and I am not fixing it here; it would be a separate issue. The fix for #57 makes the
  issue's example correct because only `Python` remains after filtering.
- I verified the cause by calling `_should_skip_file` and by running the snippet, but I have not
  searched the rest of the repo for other callers of `_should_skip_file`. I will grep before editing.
- Windows-style `\` paths are untested; I assume callers pass `/` paths, like the issue and tests do.

## Deviations

Nothing material changed; the plan held. The diagnosis, the single edit to `_should_skip_file`
(`"/" + filepath`), the two removed `xfail` markers, and the one parametrized test all went in as written,
on branch `fix/57-skip-top-level-vendor-paths`. Three small differences from what I wrote:

- I did the "check for other callers" step I had listed as unknown: `grep` finds only one caller,
  `_detect_tech` in the same file, so that risk is closed.
- I ran `ruff check`, `ruff format --check` and `mypy` directly on the two changed files instead of
  `make lint` / `make typecheck`; all clean.
- The `-k "node_modules or build_directory"` filter in my test plan also selects two cases of the new
  parametrized test (their ids contain `node_modules` and `build`), so that command now reports 4 passed
  rather than 2. The whole test file reports 35 passed.

The risks I listed (primary language chosen alphabetically, wider top-level skip effect, Windows paths) are
unchanged and still out of scope, so the comment I posted is still true.
