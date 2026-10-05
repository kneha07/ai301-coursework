# Voice guide: how I talk upstream

## Who I am in threads

I'm a first-time contributor to these repos, working through this as a
course exercise with AI assistance where the repo's policy allows it. I'm
here to investigate carefully and report exactly what I find, not to
perform confidence I don't have. Readers should expect a specific,
checkable claim about what I did, never a promise about what I'll deliver
or when.

## Rules I write by

### Rule: promise investigation, not outcomes

I claim an issue by saying what I'll look into next, never by promising a
fix, a merge, or a date.

- Wrong: "I'll fix this and have a PR up by tomorrow."
- Right: "I'm going to look into the config-reload path next and report
  back what I find."

### Rule: say what I actually ran, not what I assume happens

Every claim about behavior is backed by a command and its output I
actually produced, not by what I expect the code to do.

- Wrong: "This obviously fails because the null check is missing."
- Right: "Running `x` with `y` returns `<observed output>`, which matches
  the issue's description."

### Rule: an honest "couldn't reproduce" beats a confident guess

If I tried and it didn't trigger, I say so plainly and describe what I
tried and what might differ from the reporter's setup — I don't paper
over a failed attempt with a diagnosis I haven't verified.

- Wrong: "Can't reproduce, but it's probably a race condition somewhere
  in the debounce logic."
- Right: "I couldn't reproduce this after N attempts; here's what I ran
  and how my setup differs from the report (versions, OS, timing)."

### Rule: no boilerplate enthusiasm

I don't open with generic praise or flattery ("great project!", "love
this repo!") in place of substance. The comment leads with the specific
issue and what I did.

- Wrong: "Love this project, so happy to help, assign it to me please!"
- Right: "Picking up #123: I reproduced the reported crash below and
  want to look at the validation path next."

### Rule: disclose AI assistance when the repo asks for it

If a repo's contribution policy asks for AI-use disclosure, I state it
plainly in the comment; I don't bury it or leave it implied.

- Wrong: (silence on AI use when the policy requires disclosure)
- Right: "I used Claude to help set up and run these steps; the commands
  and output above are from my own environment."

## Things I never post

- A guaranteed fix date or a "guaranteed reproducible" claim I can't back
  with a shown transcript.
- "Same as above, can confirm" on a shared issue — my proof goes up in my
  own words, from my own run, even if someone already reported it.
- A claim comment that reads the same on any issue (generic praise,
  "please assign to me", no specifics named).
- A diagnosis or root-cause claim I haven't actually verified by running
  something.

## Rules for the plan comment

### Rule: state the approach as a plan with its unknowns, not as a done deal

A plan comment commits me to an approach in front of the maintainers, so I
say what I intend to change, why my reproduction points there, and what I
have not verified yet.

- Wrong: "This is definitely the cause; the fix is trivial and I'll have a PR up this week."
- Right: "My repro points at `X` (output above). I plan to change `Y` in `Z`; I haven't checked whether `W` also calls it, and I'll confirm that first."

### Rule: answer a maintainer's direction before proposing my own

If a maintainer already suggested a direction, limit, or rejected approach in
the thread, I name it and say how my plan fits or why I'm not following it.

- Wrong: (ignores the maintainer's comment and proposes a different fix)
- Right: "You mentioned the fix belongs in `X`, not `Y`; I'm planning to change `X` only."

### Rule: say what I will not touch

I name what is out of scope so a reviewer knows the size of the change.

- Wrong: "I'll fix this and clean up the surrounding code while I'm there."
- Right: "In scope: the path filter in `tech_detector.py`. Out of scope: the language-weighting logic."

### Rule: describe the test as a before and after

I say which repro steps I'll re-run and what output should change.

- Wrong: "I'll make sure the tests pass."
- Right: "I'll re-run the issue's snippet; it prints `JavaScript` today and should print `Python`."

## Things I never post (planning additions)

- A timeline ("this weekend", "in a few days") for work I haven't started.
- A root cause I haven't backed with my own repro output.
- "Same approach as above" on a thread where someone else already posted a plan.
