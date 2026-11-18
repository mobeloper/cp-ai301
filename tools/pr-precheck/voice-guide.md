# Voice guide: how I talk upstream

## Who I am in threads

<!-- 2-3 lines. Who is talking when you comment on an issue: your
experience level stated plainly, what you are doing in this repo, what
readers can expect from you. This is the register your rules protect. -->

I am a technical contributor/user who is comfortable reading code, reproducing problems, and describing concrete behavior. When I open an issue, I am trying to help the maintainer understand exactly what is happening and what needs to be checked—not just report that something "doesn't work."

I write like an engineer talking to another engineer: specific, concise, factual, and focused on the behavior, version, reproduction, and evidence.

## Rules I write by

<!-- 3-5 rules, drafted from the lecture's slide-12 moment. Each rule
needs a wrong/right pair from your own hand: one line you might
actually have written that breaks the rule, and the line you would
post instead. The pair is what makes a rule executable; a rule without
one is a wish.

Format each rule like this:

### Rule: <short name>

<The rule, one or two sentences.>

- Wrong: "<a line that breaks it>"
- Right: "<the line to post instead>"
-->

### Rule: Name the version and behavior

Always identify the relevant version and the specific behavior that is wrong or incompatible. Avoid vague descriptions such as "this bug" or "it doesn't work."

Wrong: "This bug happens with the latest version."
Right: "v1.30.6 is not compatible with func grad_dim()."


### Rule: Describe what actually happens

Separate what I observed from what I think the cause is. State the actual behavior first and avoid presenting guesses as facts.

Wrong: "The library is broken because the new API changed something internally."
Right: "With v1.30.6, func grad_dim() returns X instead of Y. This did not happen with v1.30.5."


### Rule: Give evidence, not adjectives

I do not use words like "amazing," "terrible," "crazy," or "completely broken" to communicate importance. I show the concrete behavior instead.

Wrong: "This is a really bad regression."
Right: "The same input produces a different result in v1.30.6 than in v1.30.5."


### Rule: Keep it concise and technical

I remove anything that does not help someone reproduce, understand, or verify the issue. I do not add long introductions or generic praise.

Wrong: "Amazing project!! Thanks so much for all the work. I just wanted to mention that I think I may have found a small issue..."
Right: "Picking this up: v1.30.6 is not compatible with func grad_dim(). Repro below."


### Rule: State the next useful action without promising a timeline

When I can contribute a reproduction, test, patch, or additional information, I say what I will provide without promising when it will be done.

Wrong: "I'll have the reproduction ready by tomorrow!"
Right: "I can provide a minimal reproduction."


### Rule: Prefer exact examples over general claims

If I can show the input, output, error, version, or expected behavior, I use that instead of making a broad statement.

Wrong: "This function behaves incorrectly in several cases."
Right: "grad_dim([1, 2, 3]) returns 2; expected 3."


### Rule: Do not overstate certainty

If I have confirmed the behavior but not the root cause, I say so. I distinguish observation, hypothesis, and confirmed cause.

Wrong: "The problem is definitely caused by the new parser."
Right: "The regression appears after the parser change, but I have not confirmed the root cause yet."


### Rule: Title says what changed and where (PR title)

The PR title names the behavior that changed and the function or file it changed in, in one line. It does not use "fix bug," "updates," or the issue number alone.

Wrong: "Fix bug in security"
Right: "fix: return False from verify_password on malformed stored hash"


### Rule: Claim only what the diff does (PR description)

Every sentence in the Summary and every Changes bullet must be visible in the diff, and every behavior change in the diff must be described. I never write "implements the plan exactly" or "no other changes" unless I have checked the diff against my plan.

Wrong: "Implements the plan exactly, with no other changes."
Right: "Changes core/security.py and tests/unit/test_security.py. The only difference from my plan is the branch name, recorded under Deviations."


### Rule: Show test output, not test claims (PR description)

In Testing I paste the before and after output and each check's result. I do not write "tested locally" or "all tests pass" with nothing shown, and I tick only the boxes the pasted output supports.

Wrong: "Tested locally, all tests pass."
Right: "Before: --runxfail raises UnknownHashError. After: pytest tests/unit/test_security.py, 26 passed. make lint and make typecheck pass."



## Things I never post

<!-- A short list. Promises you cannot keep, tones you refuse,
shortcuts you know you reach for when tired. The skill quotes this
list back at you when a draft crosses it. -->

- Empty praise such as "Amazing project!!" or "Great work!!!"
- Vague reports such as "this is broken" without the relevant version and behavior.
- Claims about the root cause that I have not verified.
- Promises about when I will provide a reproduction, patch, PR, or follow-up.
- Artificial urgency unless there is a concrete reason for it.
- Marketing language, hype, or exaggerated descriptions.
- Long explanations when a concrete example would communicate the same thing.
- "Works on my machine" without the environment or reproduction details.
- Passive-aggressive comments about maintainers or previous responses.
- Bot-like phrases such as "I hope this message finds you well" or generic AI-generated filler.
- Comments that sound like I am trying to impress the maintainer rather than help diagnose the issue.
- A PR title like "fix bug" or "updates" that does not say what changed and where.
- "Implements the plan exactly" or "no other changes" without checking the diff.
- "Tested locally" or "all tests pass" with no output shown, or a ticked Testing box the output does not support.
- Promising follow-up PRs or a timeline for them ("I'll send another PR for that tomorrow").

## Upstream: how I talk to maintainers

Maintainers are strangers who owe me nothing, so what I post has to be easy to act on and easy to trust.

### Claims I post

#### Rule: Claim with a first step, never a date

A claim says I want the issue, names the first concrete step with the files involved, and says what event I will report back on. It gives no delivery date.

- Wrong: "I'll take this and have a PR up by Friday!"
- Right: "I'd like to take this one. Could you assign it to me? First step: read tests/fixtures/risk/README.md, then write the fixture and its test. I'll comment here once the test passes locally."

#### Rule: Never claim over someone, and release honestly

Before I claim, I read the whole thread and any linked PRs. If a stranger already claimed or opened a PR, I do not claim; I say so and move on. If I stop working, I say that in the thread.

- Wrong: "Can I work on this too?" (when a claim or open PR already covers it)
- Right: "I see #5682 covers this scope, so I'm leaving it and picking #5295 instead." / "I'm not able to continue with this one, so it's free for someone else."

### Questions I ask

#### Rule: One answerable question, with what I already tried

A question names the file or behavior I looked at, what I tried or read, and asks one thing a maintainer can answer in a sentence.

- Wrong: "How does this work? Also where are the tests, and should I update the docs?"
- Right: "I read tests/fixtures/risk/README.md and kanon_report and could not tell whether the privacy-unit column must be unique per release. Should each release repeat the same keys?"

### Pings I send

#### Rule: Ping on the repo's rhythm, once, with something new

I ping only after the repo's own reply time has passed (I check how fast maintainers answered recent threads), only once, and I say what I will do if I hear nothing. I do not add pressure or count days.

- Wrong: "Any update? It has been 2 days now."
- Right: "Following up on my question above about the release layout. Maintainers here usually answer within a day, so I'll continue with the layout in the README unless you say otherwise."
