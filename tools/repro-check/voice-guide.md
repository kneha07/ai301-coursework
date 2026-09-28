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
