Hi! I'd like to pick up #57 as my first contribution here. Reading `agent/tools/tech_detector.py`, the detector counts every file it's given toward the language tally, including anything under `node_modules/` or `build/`, so a repo with a couple of Python source files and several bundled/vendored JS files gets reported as primarily JavaScript.

Next I'm going to reproduce the issue's exact snippet locally, then look at where the detector should exclude vendored/build paths before counting, and report back with what I find before opening a PR.
