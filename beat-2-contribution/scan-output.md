Scan result: 1 accepted, 4 rejected of the top 5 (15 queries, 411 unique issues, 319 after ranking filters).

ACCEPTED (fit order)
1. langchain-ai/openwiki#1066 "[Bug]: Visualizer: in-page links to pages with non-ASCII filenames return 404" https://github.com/langchain-ai/openwiki/issues/1066
   Fit: TypeScript frontend bug in a langchain-ai agentic tool; one bounded fix in rewriteLinks() (src/visualize/client.ts); has repro; collaborator colifran answers outsiders' issues; last release 2026-10-09.
   Surfaced by: query 2 (owners, label bug, typescript). Ranking signals: bug label, 0 comments, updated 2026-10-19, ranked 4th (the three above it are rejected below).
   Caveats: a few open visualizer link PRs exist (#654, #886, #608) but none is linked to this issue; I judged size from the issue text, not the code; other openwiki issues attract "I'd like to take this" claims within a day.

REJECTED (rank order, with the check that sank each)
- pytorch/tensordict#3043 https://github.com/pytorch/tensordict/issues/3043: unclaimed fails (opened by Collaborator peterdsharpe, who files issues and fix PRs in same-day batches; sibling fix PR opened 2026-10-19T06:08); responds-to-issues fails (no maintainer reply to an outsider's issue since 2026-07-20); small-and-easy unclear. Surfaced by query 13 (pytorch owner, bug, python); signals: bug label, 0 comments, updated 2026-10-19, ranked 1st.
- pytorch/tensordict#3042 https://github.com/pytorch/tensordict/issues/3042: same sinking checks as #3043 (maintainer batch pattern, no outsider replies). Surfaced by query 13; signals: bug label, 0 comments, updated 2026-10-19, ranked 2nd.
- modelcontextprotocol/servers#5176 https://github.com/modelcontextprotocol/servers/issues/5176: small-and-easy fails (done-when is a python-sdk release; label waiting-on-sdk; a server-side workaround is explicitly not wanted); unclaimed fails (opened by Member cliffhall as part of his own PR batch). Surfaced by query 3 (owners, bug, javascript); signals: bug label, 0 comments, updated 2026-10-19, ranked 3rd.
- modelcontextprotocol/python-sdk#3729 https://github.com/modelcontextprotocol/python-sdk/issues/3729: is-a-bug-fix and small-and-easy are unclear (the reporter asks whether the inclusive boundary is intentional; no maintainer has confirmed it is a bug). Surfaced by query 1 (owners, bug, python); signals: bug label, 0 comments, updated 2026-10-18, ranked 5th.

```json
[
  {
    "item": "https://github.com/langchain-ai/openwiki/issues/1066",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "last 5 default-branch commits 2026-10-17, 10-13, 10-09, 10-09, 10-09; not archived"},
      {"name": "responds-to-issues", "grade": "pass", "evidence": "COLLABORATOR colifran replied to others' issues: #1002 on 2026-10-07 'thanks - this was on my radar as a bug. will take a look!', #1014 on 2026-10-09 'Resolved with #1008'"},
      {"name": "unclaimed", "grade": "pass", "evidence": "open, 0 assignees, 0 comments, no linked or cross-referenced PR; opened by outside user zhangjke (NONE)"},
      {"name": "is-a-bug-fix", "grade": "pass", "evidence": "Expected in-app navigation, actual native navigation to a percent-encoded URL with 404; ASCII-named targets work"},
      {"name": "small-and-easy", "grade": "pass", "evidence": "one bounded client-side fix; issue names rewriteLinks() in the visualize client (src/visualize/client.ts); no maintainer scope warning"},
      {"name": "not-docs-only", "grade": "pass", "evidence": "label is bug only; the fix is in TypeScript source"},
      {"name": "ai-contribution-allowed", "grade": "pass", "evidence": "CONTRIBUTING.md 'A note for AI agents': binding rules (single-concern PRs), no ban"},
      {"name": "good-to-start", "grade": "fail", "evidence": "labels: bug only"},
      {"name": "has-repro", "grade": "pass", "evidence": "numbered Steps to Reproduce with example markdown link and ASCII control"},
      {"name": "shipped-recently", "grade": "pass", "evidence": "latest release 2026-10-09"},
      {"name": "has-contributing-guide", "grade": "pass", "evidence": "CONTRIBUTING.md present"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/pytorch/tensordict/issues/3043",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "last 5 default-branch commits all 2026-10-19; not archived"},
      {"name": "responds-to-issues", "grade": "fail", "evidence": "no Owner/Member/Collaborator reply on an issue opened by someone else since 2026-07-20; the only maintainer comments are on peterdsharpe's own issues"},
      {"name": "unclaimed", "grade": "fail", "evidence": "opened by COLLABORATOR peterdsharpe, who opened a fix PR for sibling #3005 at 2026-10-19T06:08 and files issue+PR pairs same day"},
      {"name": "is-a-bug-fix", "grade": "pass", "evidence": "sub-tensordict add_/mul_/clamp_ on a lazy-stack source silently lose writes; expected same writes as dense source"},
      {"name": "small-and-easy", "grade": "unclear", "evidence": "body names a likely cause in _SubTensorDict/_foreach_* internals but no maintainer scoping; fix size not established"},
      {"name": "not-docs-only", "grade": "pass", "evidence": "label bug; code fix"},
      {"name": "ai-contribution-allowed", "grade": "pass", "evidence": "AGENTS.md gives house rules for AI-driven contributions, no ban"},
      {"name": "good-to-start", "grade": "fail", "evidence": "labels: bug only"},
      {"name": "has-repro", "grade": "pass", "evidence": "runnable snippet with dense vs lazy output"},
      {"name": "shipped-recently", "grade": "pass", "evidence": "latest release 2026-10-17"},
      {"name": "has-contributing-guide", "grade": "pass", "evidence": "CONTRIBUTING.md present"}
    ],
    "verdict": "reject"
  },
  {
    "item": "https://github.com/pytorch/tensordict/issues/3042",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "last 5 default-branch commits all 2026-10-19; not archived"},
      {"name": "responds-to-issues", "grade": "fail", "evidence": "no Owner/Member/Collaborator reply on an issue opened by someone else since 2026-07-20"},
      {"name": "unclaimed", "grade": "fail", "evidence": "opened by COLLABORATOR peterdsharpe two seconds before #3043, same batch as sibling fix PR opened 2026-10-19T06:08"},
      {"name": "is-a-bug-fix", "grade": "pass", "evidence": "keyword tensor operands raise or give wrong values on lazy stacks; positional call works"},
      {"name": "small-and-easy", "grade": "unclear", "evidence": "cause named in _maybe_broadcast_other (base.py) but spans dense and lazy paths; no maintainer scoping"},
      {"name": "not-docs-only", "grade": "pass", "evidence": "label bug; code fix"},
      {"name": "ai-contribution-allowed", "grade": "pass", "evidence": "AGENTS.md gives house rules for AI-driven contributions, no ban"},
      {"name": "good-to-start", "grade": "fail", "evidence": "labels: bug only"},
      {"name": "has-repro", "grade": "pass", "evidence": "runnable snippet with RuntimeError and wrong-values cases"},
      {"name": "shipped-recently", "grade": "pass", "evidence": "latest release 2026-10-17"},
      {"name": "has-contributing-guide", "grade": "pass", "evidence": "CONTRIBUTING.md present"}
    ],
    "verdict": "reject"
  },
  {
    "item": "https://github.com/modelcontextprotocol/servers/issues/5176",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "last 5 default-branch commits 2026-10-11 x4 and 2026-10-10; not archived"},
      {"name": "responds-to-issues", "grade": "unclear", "evidence": "recent Member comments (cliffhall, 2026-10-19) are on his own PRs/issues; no reply to an outsider's issue seen"},
      {"name": "unclaimed", "grade": "fail", "evidence": "opened by MEMBER cliffhall as a follow-up from his own PR #5175 review; cross-references his PR #5166"},
      {"name": "is-a-bug-fix", "grade": "pass", "evidence": "the string 'null' arrives as None for base_branch, contains, not_contains"},
      {"name": "small-and-easy", "grade": "fail", "evidence": "Done when: 'The SDK release that fixes python-sdk#3055 is locked in src/git'; label waiting-on-sdk; server-side workaround 'is not wanted'"},
      {"name": "not-docs-only", "grade": "pass", "evidence": "labels bug, server-git; code and protocol tests"},
      {"name": "ai-contribution-allowed", "grade": "pass", "evidence": "CONTRIBUTING.md asks for the exact prompt text if a change was generated; AGENTS.md present; no ban"},
      {"name": "good-to-start", "grade": "fail", "evidence": "labels: bug, server-git, waiting-on-sdk, v2"},
      {"name": "has-repro", "grade": "pass", "evidence": "table of affected tools and parameters with the cause"},
      {"name": "shipped-recently", "grade": "pass", "evidence": "latest release 2026-10-11"},
      {"name": "has-contributing-guide", "grade": "pass", "evidence": "CONTRIBUTING.md present"}
    ],
    "verdict": "reject"
  },
  {
    "item": "https://github.com/modelcontextprotocol/python-sdk/issues/3729",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "last 5 default-branch commits 2026-10-17, 10-16, 10-13, 10-13, 10-12; not archived"},
      {"name": "responds-to-issues", "grade": "pass", "evidence": "MEMBER Kludex replied to others' issues: #3727 on 2026-10-17, #3102 on 2026-10-16, #1295 on 2026-10-14"},
      {"name": "unclaimed", "grade": "pass", "evidence": "open, 0 assignees, 0 comments, no linked PR"},
      {"name": "is-a-bug-fix", "grade": "unclear", "evidence": "reporter asks 'Could you confirm whether the inclusive boundary is intentional'; impact is only intermittent downstream test failures"},
      {"name": "small-and-easy", "grade": "unclear", "evidence": "likely a one-operator change in is_token_valid(), but no maintainer has confirmed the intended behavior"},
      {"name": "not-docs-only", "grade": "pass", "evidence": "labels bug, v1, v2; code fix"},
      {"name": "ai-contribution-allowed", "grade": "pass", "evidence": "CONTRIBUTING.md 'AI-Assisted Contributions': 'We use AI tooling constantly and have no problem with you using it too'; a person must be accountable"},
      {"name": "good-to-start", "grade": "fail", "evidence": "labels: bug, v2, v1"},
      {"name": "has-repro", "grade": "pass", "evidence": "deterministic mocked-clock snippet showing True at expiry, False after"},
      {"name": "shipped-recently", "grade": "pass", "evidence": "latest release 2026-10-02"},
      {"name": "has-contributing-guide", "grade": "pass", "evidence": "CONTRIBUTING.md present"}
    ],
    "verdict": "reject"
  }
]
```
