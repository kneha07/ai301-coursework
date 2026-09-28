# Evidence guide: where proof lives in a reproduction package

## Environment

- Where it lives: in an eval bundle, the "Environment:" line at the start of
  the **Candidate repro report** section. In live mode, the same line in the
  student's draft repro report; cross-check the tool/package version named
  there against the repo-facts block's "latest release" and the issue's
  stated target version.
- What good looks like: the version of the software under test, the OS/
  platform, and any dependency the issue's own text names as relevant (a
  driver version, a shell, a library version, a kernel version) are all
  present. A report that names the tool version and OS but silently omits a
  dependency the issue explicitly blames (e.g. a specific driver or library
  version) does not clear this bar.

## Steps

- Where it lives: the "Steps:" section of the **Candidate repro report**,
  read against the issue's own "Steps to reproduce" / trigger description. In
  live mode, also check whether any referenced config, fixture, or link is
  something a stranger (not logged into the student's machine or org) could
  actually obtain.
- What good looks like: exact commands or inputs, copy-pasteable, from a
  named starting state, using the *same* trigger syntax/scenario the issue
  describes (not a nearby or modified one — that is the "wrong-target"
  failure mode). A report that reproduces from a private monorepo, an
  un-posted internal config, or a link only the author can reach fails this
  even if the transcript looks complete, because nobody else can re-run it.

## Behavior shown

- Where it lives: the artifact block(s) in the **Candidate repro report**
  (command output, exit code, log excerpt, screenshot description), read
  side-by-side with the issue's own "Expected"/"Actual" text.
- What good looks like: the artifact shows the same class of failure the
  issue names — same exit code, same error type, same visible symptom — not
  a different failure produced by an altered trigger (a changed flag, a
  changed operator, a different argument shape). When the artifact's own
  content contradicts the report's narrative (e.g. the output shows a
  graceful error or the terminal still alive, but the prose calls it a
  crash), the artifact wins: read what it actually shows, not what the report
  claims it shows. When the tested version differs from the one the issue
  targets (an older release, a different branch than "latest"/"main"), the
  report must say so; an unacknowledged version mismatch means the artifact
  is evidence about that other version, not about the reported bug.

## Honesty

- Where it lives: the gap (or its absence) between the repro report's stated
  conclusion ("reproduced", "could not reproduce", a root-cause claim) and
  the artifacts actually shown above it.
- What good looks like: every claim of confidence is backed by a shown
  transcript. An honest "could not reproduce" that describes what was tried
  and what differed from the issue's conditions is a pass — it is real
  evidence of a real attempt. A confident claim ("guaranteed reproducible",
  "I verified the race condition", "confirmed on every machine") with no
  matching artifact is a fail regardless of how detailed or certain the
  prose sounds; confidence is not evidence.

## Comms

- Where it lives: the **Candidate claim comment** text, read against the
  issue it names and the repo-facts block's "contribution policy" line; in
  live mode, also the repo's actual issue/PR templates and CONTRIBUTING.md
  or AI-policy files.
- What good looks like: the claim comment names this issue's specific
  behavior (not generic praise or boilerplate that could be pasted onto any
  issue) and promises investigation only — no guaranteed fix, no guaranteed
  timeline, no "reserve this for me" framing. Separately, when the stated
  contribution policy requires disclosing AI assistance, both the claim
  comment and the repro report say so plainly; when the policy is silent or
  only asks for responsible/reviewed use with no disclosure requirement, no
  disclosure is needed and its absence is not a fail.
