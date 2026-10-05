# Voice guide: how I talk upstream

<!--
THIS IS A CARRY-OVER SLOT, not a new hole. You wrote this guide in
week 2; paste your filled week-2 voice-guide.md here, whole. It is not
re-authored and it is not graded as new work this week.

Then reread it with the plan comment in mind. Your claim and repro
comments promised and reported; a plan comment commits you to an
approach in front of the people who maintain the code. If your rules
do not cover that register (for example: how you state an approach you
are not certain of, or how you respond when a maintainer already
suggested a direction), extend the guide with what it needs. Extending
is allowed and encouraged; starting over is not required.

Live mode reads this file before your plan comment goes out and
reports any rule your draft breaks. Eval mode ignores it entirely,
because your voice is yours and carries no gold labels.
-->

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

