# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Environment recorded | The repro report's environment line/section | Pass if it names the tool/package version under test, the OS/platform, and any other dependency version the issue's behavior plausibly depends on (e.g. a driver, a shell, a library version named in the issue). Fail if any of version, OS, or a issue-relevant dependency is missing entirely (not just terse — genuinely absent). | required |
| Steps are followable by a stranger | The repro report's steps/commands | Pass if the steps are exact commands or inputs, starting from a stated starting state, that someone else could copy and run themselves. Fail if reproduction depends on a private/unshared resource (an internal monorepo, an un-posted config, a link only the author can reach) that a stranger could not obtain, or if a required input is described rather than given verbatim. | required |
| Behavior matches the issue, not an adjacent one | The shown artifact (output, exit code, log excerpt, screenshot) read against the issue's stated trigger and expected/actual behavior | Pass if the artifact demonstrates the same behavior the issue describes: the same trigger syntax/scenario (not a modified or different one), the same class of failure (e.g. the same exit code/error type), and — when the tested version differs from the one the issue targets — the report explicitly says so rather than silently presenting an old or unrelated version's behavior as confirmation. Fail if the steps were altered enough to produce a different (even if superficially similar) failure, or if the narrative claims more than the shown artifact demonstrates (e.g. calling a graceful error message a "crash", or calling live garbled output a confirmed crash). | required |
| Outcome stated honestly | The report's claimed result, compared against what it actually shows | Pass if the stated result matches the evidence: a "could not reproduce" verdict backed by a real, described attempt (what was tried, what differed from the issue's conditions) is a pass; a "confirmed"/"guaranteed" claim is a pass only when backed by a shown artifact. Fail on any claim of confidence, diagnosis, or certainty ("verified", "guaranteed reproducible", root-caused) that is not backed by a shown transcript or artifact — regardless of how detailed or confident the prose reads. | required |
| Claim comment is specific and promises investigation only | The candidate claim comment's text | Pass if it names the specific issue/behavior being investigated and promises only investigation/next steps (no guaranteed fix, no guaranteed timeline, no "keep this reserved for me" framing). Fail if it is interchangeable boilerplate that could be pasted onto any issue (generic flattery, "assign it to me", a promised fix date) rather than naming this issue's specifics. | required |
| Conventions and AI-disclosure followed | The repo-facts block's contribution policy, read against the candidate claim comment and repro report | Pass if, when the stated policy requires disclosing AI assistance, the comments disclose it as required; pass automatically if the policy is silent or only permissive-with-responsibility (no disclosure ask). Fail only on a stated disclosure requirement that the comments do not meet. | required |
| Includes a control run | The repro report | Pass if the report includes a control/comparison run (e.g. the non-triggering case, a working input, an earlier or later version) alongside the triggering one, to isolate the reported behavior. | preferred |
| Concrete next step named | The claim comment or the repro report's closing line | Pass if the package names a concrete next investigative step rather than ending on the reproduction alone. | preferred |

## Verdict rule

Accept (ready to post) only if every `required` check passes. Any `required`
check graded `fail` or `unclear` holds the package. `preferred` checks never
change the verdict. On a claim-only draft, checks whose evidence is the repro
report (Steps are followable, Behavior matches the issue, Outcome stated
honestly, Includes a control run) are not yet applicable and are left out of
the verdict rule entirely; the verdict then answers only whether the claim
comment and any conventions/AI-disclosure obligations it already triggers are
ready to post. `unclear` on any check that is part of the verdict counts as
`fail`: proof that cannot be verified from what is given is not ready to post.
