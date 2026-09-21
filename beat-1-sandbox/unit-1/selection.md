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

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/62

**Verdict output**

```
Summary

Repo-level checks (apply to all three issues, codepath/pathreview-ai301-fa26-s1):
- active-maintainer: pass — 5 most recent commits all by Andrew Burke (human), most recent 2026-09-16, well within 60 days
- not-archived: pass — archived: false
- active-repo: pass — last push 2026-09-16, within 30 days of today (2026-09-21)
- ai-contributions: pass — no CONTRIBUTING.md/AI_POLICY.md found (404s); PR template has no AI-disclosure requirement or ban
- replies (preferred): unclear — sampled last 3 active issues (#70, #68, #69), all comments are from students claiming/reproducing; no maintainer reply visible in-sample

Ranked read-out:

1. #72 — verify_password raises UnknownHashError → accept. Single bounded bug in core/security.py, explicit 1–2hr estimate, one xfail test to un-mark, no assignee/PR/comments. Fits your backend/testing/debugging interest well and gives a clean scope.
2. #62 — Health check references settings.redis_host → accept. Single bounded bug (wrong attribute name causing a silently-swallowed AttributeError), no assignee/PR/comments. Also a strong backend/API fit, though it lacks an explicit effort estimate.
3. #60 — Faithfulness checker crashes on text: None → reject. unclaimed check fails: PR #74 ("fix(rag): handle None text in faithfulness checker (#60)") is open and unmerged against this exact issue — someone is already actively working it, so it's no longer a free first contribution regardless of the classroom claim-comment house rule (which exempts claim comments, not active linked PRs).

[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72",
    "checks": [
      {"name": "active-maintainer", "grade": "pass", "evidence": "5 most recent commits all by Andrew Burke (human), latest 2026-09-16"},
      {"name": "replies", "grade": "unclear", "evidence": "sampled recent issues (#70,#68,#69) show only student comments, no maintainer reply in sample"},
      {"name": "not-archived", "grade": "pass", "evidence": "repo API field archived: false"},
      {"name": "active-repo", "grade": "pass", "evidence": "pushed_at 2026-09-16, within 30 days of 2026-09-21"},
      {"name": "bounded-scope", "grade": "pass", "evidence": "single fix in core/security.py with named test and 1-2hr estimate; not an umbrella issue"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], comments: 0, no PR found via search for '#72'"},
      {"name": "ai-contributions", "grade": "pass", "evidence": "no CONTRIBUTING.md/AI_POLICY.md (404s), PR template has no AI restriction"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/62",
    "checks": [
      {"name": "active-maintainer", "grade": "pass", "evidence": "5 most recent commits all by Andrew Burke (human), latest 2026-09-16"},
      {"name": "replies", "grade": "unclear", "evidence": "sampled recent issues (#70,#68,#69) show only student comments, no maintainer reply in sample"},
      {"name": "not-archived", "grade": "pass", "evidence": "repo API field archived: false"},
      {"name": "active-repo", "grade": "pass", "evidence": "pushed_at 2026-09-16, within 30 days of 2026-09-21"},
      {"name": "bounded-scope", "grade": "pass", "evidence": "single attribute-mismatch bug in api/routes/health.py, one file, one cause"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], comments: 0, no PR found via search for '#62'"},
      {"name": "ai-contributions", "grade": "pass", "evidence": "no CONTRIBUTING.md/AI_POLICY.md (404s), PR template has no AI restriction"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60",
    "checks": [
      {"name": "active-maintainer", "grade": "pass", "evidence": "5 most recent commits all by Andrew Burke (human), latest 2026-09-16"},
      {"name": "replies", "grade": "unclear", "evidence": "sampled recent issues (#70,#68,#69) show only student comments, no maintainer reply in sample"},
      {"name": "not-archived", "grade": "pass", "evidence": "repo API field archived: false"},
      {"name": "active-repo", "grade": "pass", "evidence": "pushed_at 2026-09-16, within 30 days of 2026-09-21"},
      {"name": "bounded-scope", "grade": "pass", "evidence": "single fix in rag/evaluator/faithfulness_checker.py with a named failing test"},
      {"name": "unclaimed", "grade": "fail", "evidence": "PR #74 'fix(rag): handle None text in faithfulness checker (#60)' is open and unmerged, created 2026-09-21"},
      {"name": "ai-contributions", "grade": "pass", "evidence": "no CONTRIBUTING.md/AI_POLICY.md (404s), PR template has no AI restriction"}
    ],
    "verdict": "reject"
  }
]
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Full run (20 items): `agreement: 14/20 scored items  (bar: 18/20: below the bar)`, with `categories: claimed 4/4  clear-accept 4/8  dead-repo 3/3  policy 1/1  scope 2/4`. Six items disagreed with gold: `issue-01`, `issue-04`, `issue-06`, `issue-15`, `issue-19`, `issue-20`.
2. Partial re-run on the six disagreements only (`--only issue-01,issue-04,issue-06,issue-15,issue-19,issue-20`), after revising `bounded-scope` and `active-repo` in the rubric:
   ```
   issue-01  accept  accept   yes
   issue-04  accept  accept   yes
   issue-06  accept  reject   NO     failed: active-maintainer, active-repo
   issue-15  reject  reject   yes
   issue-19  accept  accept   yes
   issue-20  reject  accept   NO     graded accept
   ```
   4/6 agreed; `issue-06` and `issue-20` still disagreed.
3. Full confirming run (20 items), after aligning `active-repo`'s evidence and pass condition on "any branch": `agreement: 19/20 scored items  (bar: 18/20: PASS)`, with `categories: claimed 4/4  clear-accept 8/8  dead-repo 3/3  policy 1/1  scope 3/4`. This is the run saved to `eval-run.txt`.

**Issue analysis**

`issue-20`. My rubric's verdict: **accept**. Gold label: **reject**. My rubric read the issue as a normal, bounded feature request: one toolbar shape, described with a success criterion ("logo tool in the shapes toolbar → place/resize/move like other elements → correct export"), and none of my required checks (`active-maintainer`, `not-archived`, `active-repo`, `bounded-scope`, `unclaimed`, `ai-contributions`) is written to fail on it: the repo is very active, nothing about it is claimed, and the ask reads as one deliverable. What my rubric didn't weigh: the issue was opened by `cursor[bot]` (an AI agent, not a maintainer or a human user), carries no labels, and has zero comments, meaning no maintainer has ever looked at this as real work. Two details are still explicitly unresolved ("Logo asset **TBD**", "possibly app wiring in `excalidraw-app` **if needed**"). Gold treats "nobody with authority has vetted this, and the ask itself admits open questions" as a scope failure; my rubric's `bounded-scope` check only fails on explicit umbrella issues, undecided-in-thread debates, or abandoned linked PRs.

**Check rationale**

`bounded-scope`, as currently written in `rubric.md`:

> passes if the issue describes one problem or one feature, even when it lists several sub-steps that all serve that outcome. Fails if it is an explicit tracking/umbrella issue meant to be split into separate PRs, if the thread shows the approach is still undecided, if 2+ linked PRs on it are closed/unmerged, or if it is a feature request with no maintainer engagement (no labels, no maintainer reply) and the implementation itself is left unresolved

It's shaped this way because my first draft ("no more than 1 main deliverable... no checklist of separate tasks") was too literal: it rejected real accepted issues just for listing multiple sub-steps under one goal. The current one looks for the signals that actually predict hidden difficulty, such as an issue explicitly meant to be split, a trail of abandoned PRs, or an untriaged feature request nobody with authority has vetted.

**Trade-offs**

This version of `bounded-scope` still misses `issue-20`: an untriaged, bot-filed feature request with no maintainer engagement and unresolved implementation details passes it, because none of the three fail conditions (explicit umbrella, thread debate, abandoned PRs) technically applies when there is no thread at all to read. I know this because the `--only` re-run after fixing `active-repo` still showed `issue-20  reject  accept  NO  graded accept` so the check accepted an issue gold rejected for lack of maintainer vetting, a sign this check doesn't look for yet.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. The issue's fit to your interests and to the time available: 
Yes, it does align to my skills. I am a software develper with experience in backend development, so the issue #62 is about a bug in api/routes/health.py, which is basically testing and debugging the backend. Even when there is no explicit time estimated, the issue gives steps and names the exact fields involved, so the work is narrow.

2. What the verdict identified correctly, and what you weighed that the rubric could not: 
Issues 72 and 62 were in an exact tie, but what broke the tie for me was that issue 62 was more interesting and the fact that it is a swallowed-exception, makes it a more instructive class of bug than 72. Moreover, issue 72 seemed to have 2 people that already selected it rather than one for 62.

3. The anticipated difficulty in claiming it:
So far i see no assignee, no PR against the actual repo, and only one classmate cross-reference found, so i dont see any dificulties. However the real difficulty is understanding core/config.py's Settings.
---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
