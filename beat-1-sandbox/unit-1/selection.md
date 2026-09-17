# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

[The individual Path Review issue page. A link to the repository or the issue list
does not satisfy this field.]

**Verdict output**

[Your skill's live-mode output for this issue, pasted verbatim and ending with the
fenced JSON verdict block. A summary does not satisfy this field.]

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

`python3 run_eval.py --rubric ../issue-select/rubric.md`

```
grading 20 bundle(s) with rubric.md, model sonnet, 5 worker(s)...
  issue-02: reject
  issue-04: accept
  issue-01: reject
  issue-03: reject
  issue-05: reject
  issue-07: reject
  issue-06: accept
  issue-08: reject
  issue-09: reject
  issue-11: reject
  issue-10: reject
  issue-13: reject
  issue-12: accept
^[[B^[[B^[[B  issue-14: accept
  issue-17: reject
  issue-16: reject
  issue-15: reject
  issue-18: reject
  issue-19: reject
  issue-20: reject

item      gold    verdict  agree  note
issue-01  accept  reject   NO     failed: responds-to-issues (preferred), good-to-start
issue-02  reject  reject   yes    
issue-03  reject  reject   yes    
issue-04  accept  accept   yes    
issue-05  reject  reject   yes    
issue-06  accept  accept   yes    
issue-07  reject  reject   yes    
issue-08  reject  reject   yes    
issue-09  accept  reject   NO     failed: unclaimed, responds-to-issues (preferred)
issue-10  reject  reject   yes    
issue-11  accept  reject   NO     failed: shiped-recently (preferred), good-to-start
issue-12  reject  accept   NO     graded accept
issue-13  reject  reject   yes    
issue-14  accept  accept   yes    
issue-15  reject  reject   yes    
issue-16  accept  reject   NO     failed: responds-to-issues (preferred), good-to-start
issue-17  reject  reject   yes    
issue-18  reject  reject   yes    
issue-19  accept  reject   NO     failed: shiped-recently (preferred), good-to-start
issue-20  reject  reject   yes    

categories: claimed 4/4  clear-accept 3/8  dead-repo 3/3  policy 0/1  scope 4/4
agreement: 14/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in policy)
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

[The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.]

**Issue analysis**

[One scored issue, identified by id (`issue-01` through `issue-20`; the `calib-`
issues are not scored). State your rubric's decision, the gold label, and the
reasoning that produced your rubric's result.]

**Check rationale**

[One check from the `rubric.md` uploaded to `tools/issue-select/`, quoted as it is
currently written, with the reasoning behind its current form.]

**Trade-offs**

[What the quoted check gives up. Any one of these is a complete answer: an issue whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

[Answer all three:

1. The issue's fit to your interests and to the time available.
2. What the verdict identified correctly, and what you weighed that the rubric could
   not.
3. The anticipated difficulty in claiming it.]

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
