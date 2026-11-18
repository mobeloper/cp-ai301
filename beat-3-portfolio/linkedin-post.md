I opened a fix to OpenMed, an open source healthcare AI project, and built a set of automatic checkpoints that reviewed my work at every step. The fix adds test data showing that two privacy-protected data releases can each look safe on their own, yet together single out three people.

I wrote the rules for each checkpoint myself and kept a human gate between steps, so nothing reached the project without my approval. The checkpoints caught real problems along the way:

• The first issue I claimed was taken by another contributor, so I accepted the maintainer's offer of a related one.
• My final check stopped me three times before I opened the fix. One stop was a clash: my checklist asked for an AI-use note, but the project's own rules forbid one. I followed the project's rule and changed my checker.

Today the fix is open and waiting for its first review, and my tracking tool says to wait four days before following up. The main thing it taught me is that tools which act for you are only as trustworthy as the rules and checkpoints you put around them.

https://github.com/maziyarpanahi/openmed/pull/5698

#OpenSource #AI #Privacy
