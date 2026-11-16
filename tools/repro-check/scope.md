# Scope: where your issue lives now (the wild)

<!--
This is the real-world scope for repro-check. It replaces the unit-2
sandbox scope.md in your INSTALLED skill (the unit 6 Assignment
tab has the copy command): the field is no longer the Path Review repo, and the
classroom house rules do not travel. Like every scope file, this one
says where the skill works, live mode only: the rubric (rubric.md)
decides whether a package or a claim draft is READY; the scope
decides which issues the skill may work against at all, and what the
rules are where that issue lives. Eval runs never read this file.

Staff wrote this file. It ships filled; there is nothing to edit.
-->

## Where your issue lives

Any public repository, one issue at a time: the issue your claim
names. That issue comes to you through your own evidence trail, the
scout's saved read-out (a site pick graded in direct mode, usually
saved in `~/.claude/skills/issue-scout/scan-output.md`, or a scan's
top five). Do not grade
drafts or packages for an issue your scout never graded.

## The wild's rules (the sandbox rules do not travel)

Path Review was a classroom with a coordination channel. Out here
the readers are strangers, and three rules flip:

- **A stranger's claim blocks you.** A claim comment or an assignee
  on an issue means someone got there first, and you have no channel
  to coordinate with them. Never claim over one; pick again. First
  claim stands, including yours: if a stranger claims after you, your
  claim holds, and if you stop working, release your claim honestly
  in the thread.
- **Never piggyback, and never duplicate.** If the thread already
  holds a reproduction, do not post "same here, can confirm". Add a
  comment only when it carries something new from your environment,
  in your words.
- **The repo's own rules outrank yours.** CONTRIBUTING.md and any
  stated contribution or AI policy are the standard you are held to
  whether you read them or not. When a stated ask conflicts with a
  course habit, the repo wins; say so in your evidence if it changes
  what you do.

## Your fit profile (kept in sync with issue-scout/scope.md)


I write python, javascript, typescript, css, scss, html, java, react, and dockerfile languages. Avoid other languages (go, c++, rust). Avoid any issue that needs special hardware (GPU, TPU, accelerators, special devices). I like working with repos about reputable agentic AI tools and libraries, and with large established open-source orgs: pytorch, open-telemetry, apache, opensearch-project, backstage, kubernetes, openedx, and hackforla. I look for issues labelled good first issue, help wanted, bug, or easy. Both frontend and backend issues are interesting to me. I want small bug fixes that I can finish in two weeks or less with easy difficulty, not a new feature. I want an active repo project where people answer questions. I develop on a MacBook Pro (macOS), so avoid any issue that is Windows-only or tied to another OS or platform (Linux-only, Android, iOS, WSL); an issue that also reproduces on macOS is fine. Many repos on my list do not maintain a good first issue label, or have heavy PR competition, so the cleanest path to a merge is an isolated fix: a self-contained bug fix, a schema or input validation tweak, or a test gap, touching one module and needing no design discussion. A docs gap counts only when the ticket also needs a test or code change; documentation-only tickets (docs, docstring, README or typo-only) are excluded.
