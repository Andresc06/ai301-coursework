# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

Andresc06

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/62#issuecomment-5850141696
First contribution here, so flagging that up front. I can reproduce this: `GET /health` returns 503 with `"redis": "unhealthy"` even though Redis is up, matching what's described here. Poking around `api/routes/health.py`, it looks like the broad exception handler might be swallowing an `AttributeError` on `settings.redis_host` — `Settings` in `core/config.py` only has `redis_url`, not separate host/port fields. Want to check that against the actual logs before I say for sure. Repro report with the request/response and log excerpt coming next.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/62#issuecomment-5850335862
Environment: Python 3.14.7 (`.venv` from `make setup`), macOS (Darwin 27.0.0, arm64), fastapi 0.141.1, sqlalchemy 2.1.1, redis-py 8.1.0. Repo is my fork of `codepath/pathreview-ai301-fa26-s1` at current `main`; the issue doesn't name a version to diff against, and there's no release tag on this project.

Steps:
1. `cp .env.example .env` (defaults, no API key needed — `LLM_PROVIDER=mock`)
2. `docker compose up -d` — starts Postgres (:5433), Redis (:6379), Chroma (:8001)
3. `make setup`
4. `make run` — starts uvicorn on :8000 and the Vite dev server on :5173 (proxies `/api/*` to :8000, stripping the prefix)
5. `GET http://localhost:5173/api/health`

Behavior: got `503 Service Unavailable`:
```json
{
  "detail": {
    "status": "unhealthy",
    "dependencies": {
      "postgres": "unhealthy",
      "redis": "unhealthy",
      "vector_db": "healthy"
    },
    "safety_events_last_hour": 0,
    "timestamp": "2026-09-26T21:53:41.462235"
  }
}
```
Backend log at the same request (`request_id=f4e69621-...`):
```
2026-09-26 16:53:41 [error] redis_health_check_failed error="'Settings' object has no attribute 'redis_host'" request_id=f4e69621-08e5-4850-9717-68f255571db8
```
This is exactly the issue's described behavior: `AttributeError` on `settings.redis_host` in `api/routes/health.py`, since `Settings` in `core/config.py` only defines `redis_url`. Confirmed Redis itself was healthy at the time — `docker compose ps` showed the `redis` container `Up ... (healthy)` via its own `redis-cli ping` healthcheck — so this is the code bug, not a real outage.

Note: the response also shows "postgres": "unhealthy" — that's a separate SQLAlchemy 2.x issue (unrelated log error), not part of this bug.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Before spending anything on the harness, I traced each of the rubric's checks by hand
against all 24 packages (20 scored + 4 calibration) and the gold labels to confirm each
check's pass condition would actually separate the categories it needed to. I then did a
free hand-check of `calib-02.md` against the rubric as a warm-up (no environment line, no
steps, no real artifact, boilerplate claim → reject, matching gold) before running the
harness at all.

One full run, confirmed: `agreement: 19/20 scored items  (bar: 18/20: PASS)`, with every category covered: `categories: clear-accept 7/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`. This is the run committed at
`beat-1-sandbox/unit-2/eval-run.txt`. No `--only` revision loop was needed.

**Package analysis**

`pkg-07` (`processing/p5.js#7168`). Gold label: `"verdict": "accept"`, category
`clear-accept`, note: "language-priority repro with an English-first control; version
delta stated; discloses AI assistance as p5.js's stated policy requires, which is what
the conditional-policy pass looks like." My rubric's verdict: `reject`, `note: failed:
env-recorded`.

Why it read that way: the candidate repro report's environment line is "Environment:
p5.js 1.11.7 (CDN single file), Chrome 139.0 on macOS 14.6," and it reconciles against
the issue's own version ("The issue was filed against 1.9.4/1.10.0; it is still present
on 1.11.7"). But the package's repo-facts block gives "latest release: v2.3.2
(2026-07-30)," and the report never mentions that gap. My `env-recorded` check's pass
condition reads: "If the version is different from what the issue names or from the
latest release, the report says so instead of leaving that unmentioned" — evidence is
defined as "compared to the version the issue names and the repo facts block's latest
release line." Because the report only reconciled against the issue's version and stayed
silent on the much larger drift to the current latest release, it failed that check
under its literal wording, even though the reproduction itself was faithful, followable,
and correctly disclosed AI use per p5.js's policy — which is why gold still called it a
clear accept.

**Check rationale**

Quoting `ai-disclosure` from `rubric.md` exactly as it reads: "If the policy requires
disclosing AI assistance in comments, the claim comment or repro report says AI was used
and how much. If it only asks that comments be human-written, the comment reads like
specific, first-person writing instead of something generic. If no such policy exists,
or it says nothing about comments, that counts as a pass automatically."

I rejected an earlier wording I had drafted for this check: `no explicit ban on
AI-generated/AI-assisted contributions; if no such policy file exists, that counts as a
pass`. That version is based on whether AI use is *banned*. It breaks on exactly the
package this check exists for: `pkg-20` (`ghostty-org/ghostty#13604`), the eval set's one
disclosure-category item. Ghostty's policy doesn't ban AI assistance at all; it requires
disclosing it, stating the tool and the extent. Under the rejected wording, "no ban"
would read as a pass even though the candidate's comments never disclose anything, so
nothing in the rubric would catch it and the category floor would fail outright on that
one item. The check has to test whether disclosure was given when the policy demands it,
not whether AI is banned. That's why it reads as a conditional on what the policy
actually asks for, rather than a single blanket rule.

**Trade-offs**

Quoting the verdict rule in `rubric.md`: "`honest-outcome` is the one `preferred` check.
It never changes accept to reject or the other way around, since everything it would
fail is already caught by `behavior-shown`." I originally had `honest-outcome` as a
required, gating check alongside the other five. I demoted it to preferred once tracing
it against the packages showed every package it would fail for overclaiming already fails `behavior-shown` for the same underlying reason: the artifact doesn't back the claim. That demotion changed nothing in this run's actual verdicts. So it's a case where
simplifying the rubric cost nothing on this set. What I accept it gives up: if a future
package ever showed genuine overclaiming *without* an accompanying `behavior-shown`
failure, this rubric would no longer catch it on its own.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
