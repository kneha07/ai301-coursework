Reproduction report for #57. **Result: reproduced** on `main` at commit `2f4e82f52efbcfcc57d65b3fa5348672163ca088`.

**Environment**

- Repo: my fork at commit `2f4e82f` (matches upstream `main` at fork time)
- Python 3.14.3 (macOS arm64, Darwin 23.3.0). Note: the project targets `requires-python = ">=3.11"` and CI/mypy pin 3.11; 3.14.3 is above that target, not on it — flagging in case it matters, though this bug is plain path-string handling with no version-sensitive behavior.
- Installed with `python3 -m venv .venv && pip install -e ".[dev]"`. No Docker/Postgres/Redis needed: `agent/tools/tech_detector.py` is pure Python and touches no service.

**Steps**

Issue's own snippet, run directly:

```python
from agent.tools.tech_detector import TechDetector
t = TechDetector()
files = ['main.py','core/app.py','node_modules/lib/index.js','node_modules/lib/util.js','node_modules/x/a.js','node_modules/y/b.js','build/bundle.js','build/vendor.js']
print(t.execute({'files': files}).data['primary_language'])
```

Output:

```
JavaScript
```

**Control** (same files, minus the vendored/build paths):

```python
from agent.tools.tech_detector import TechDetector
t = TechDetector()
print(t.execute({'files': ['main.py','core/app.py']}).data['primary_language'])
```

Output:

```
Python
```

**Named unit tests, with the xfail marker defeated:**

```
$ python3 -m pytest tests/unit/test_tech_detector.py -v -k "node_modules or build_directory" --runxfail
FAILED tests/unit/test_tech_detector.py::TestTechDetector::test_node_modules_excluded - AssertionError: assert 'JavaScript' == 'Python'
FAILED tests/unit/test_tech_detector.py::TestTechDetector::test_build_directory_excluded - AssertionError: assert 'JavaScript' == 'Python'
2 failed, 25 deselected in 0.68s
```

Both tests are marked `@pytest.mark.xfail(strict=True, reason="issue #57: ...")`, so the suite reports green as shipped; `--runxfail` shows the real failure.

**Expected:** `primary_language` reports `Python` in both the issue's example and the two named tests, since the only real source files are Python.

**Actual:** with `node_modules/` and `build/` paths present, the detector counts them toward the language tally and reports `JavaScript`; with those paths removed, it correctly reports `Python`. The failure is specific to unfiltered vendored/build paths, not to language detection generally.

Next I'll look at excluding `node_modules/` and `build/` (and similar vendored/output directories) from the file list before counting, remove the two `xfail` markers, and open a PR.
