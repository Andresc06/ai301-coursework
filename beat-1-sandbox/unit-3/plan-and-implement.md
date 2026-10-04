# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

Andresc06

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/62#issuecomment-5984011360
Following up on the reproduction above: the `AttributeError` is exactly what it looks like, `Settings` in `core/config.py` only defines `redis_url`, not `redis_host`/`redis_port`, so the Redis probe in `api/routes/health.py` can never succeed as written, regardless of whether Redis is actually up.

Planned fix: build the Redis client from `settings.redis_url` directly (via `redis.Redis.from_url`, redis-py 8.1.0). Scope stays to that one probe block, not touching the Postgres or vector_db probes or the exception-handling pattern.

Test plan: re-run my reproduction (`GET /health` with Redis up) and confirm the `redis` entry reports healthy with no `AttributeError` in the logs, plus a new case stopping the Redis container to confirm a real outage still correctly reports unhealthy. Adding both as a regression test. Note: `/health` will likely still return an overall `503` after this fix, since the Postgres probe fails for the separate, already-reported raw-SQL issue (#61); this PR only fixes the Redis attribute bug.

This is my first contribution here, so flagging that up front again. Happy to adjust the approach if a maintainer prefers a different fix shape.

---

## Your branch

**Branch**

fix/62-redis-health-check-url

**Evidence**

**Before (Unit 2, unpatched — from `reproduction.md`):**

```
GET http://localhost:5173/api/health
```
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
Backend log at the same request:
```
2026-09-26 16:53:41 [error] redis_health_check_failed error="'Settings' object has no attribute 'redis_host'" request_id=f4e69621-08e5-4850-9717-68f255571db8
```

**After (built change, Redis running):**

```
curl -s http://localhost:5173/api/health | jq .
```
```json
{
  "detail": {
    "status": "unhealthy",
    "dependencies": {
      "postgres": "unhealthy",
      "redis": "healthy",
      "vector_db": "healthy"
    },
    "safety_events_last_hour": 0,
    "timestamp": "2026-10-04T20:41:58.886963"
  }
}
```
`redis` now reports `"healthy"` with no `AttributeError` — the diagnosed bug is fixed.
`postgres` stays `"unhealthy"`, as the plan predicted: a separate, already-tracked issue
(#61's raw-SQL `SELECT 1` under SQLAlchemy 2.x), out of this fix's scope.

**After, additional case (Redis stopped, confirming a real outage still reports correctly):**

```
docker compose stop redis
curl -s http://localhost:5173/api/health | jq .
```
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
    "timestamp": "2026-10-04T20:42:12.582739"
  }
}
```
With Redis actually down, `redis` correctly reports `"unhealthy"` again — the fix detects a
real outage, it doesn't just always report healthy.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run (20 packages), first attempt: `agreement: 17/20 scored items  (bar: 18/20: below the bar)`, with `categories: clear-accept 4/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`. Three items disagreed with gold, all inside `clear-accept`: `pkg-02` (failed `scope`), `pkg-09` (failed `test`), `pkg-14` (failed `executable-by-a-stranger`, `honest-about-unknowns`).
2. Full confirming run, after revising `scope`, `test`, `executable-by-a-stranger`, and `honest-about-unknowns` in the rubric: `agreement: 19/20 scored items  (bar: 18/20: PASS)`, with `categories: clear-accept 6/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`. This is the run saved to `eval-run.txt`.

**Package analysis**

`pkg-09` — gold label: **accept** (ready). My final rubric's verdict: **reject** (hold), `failed: executable-by-a-stranger`. The plan names specific files (`src/walk.rs`, `src/main.rs`, `tests/tests.rs`) and a specific site ("candidate normalization at the match site"), but it never names the exact function to change, and unlike another package I graded correctly (`pkg-14`, which said "exact functions to be pinned in the PR after tracing... which I have working"), `pkg-09` doesn't point to any already-demonstrated method for finding the precise location — just the general site. My `executable-by-a-stranger` check requires either the exact function or a concrete already-demonstrated method to pin it down, and `pkg-09` provides neither explicitly, which is why it reads as not-quite-specific-enough even though the overall fix approach (normalize `\` to `/` for the match candidate, Windows-gated) is clearly explained.

**Check rationale**

`executable-by-a-stranger`, as currently written in `rubric.md`:

> names the specific file(s) or component(s) to change, and either the exact function, or a concrete already-demonstrated method to pin it down (e.g. a debugging trace already run) — not just a promise to investigate

It's shaped this way because my first draft required naming the exact function immediately, which incorrectly rejected `pkg-14` (gold: accept): that plan said the exact functions would be "pinned in the PR after tracing... which I have working" — honest deferral backed by a real, already-run diagnostic method, not vagueness. Rewriting the check to accept either the exact function *or* a demonstrated method fixed `pkg-14`, but `pkg-09` shows the boundary is still imperfect: it names a site but neither names the function nor demonstrates a method, so it still fails even though its approach is otherwise well-explained.

**Trade-offs**

This version of `executable-by-a-stranger` still misses `pkg-09`: a plan that names the exact files and the specific site of the change, but not the exact function or a demonstrated method, reads as insufficiently concrete even when its approach is otherwise clearly explained. I know this because the confirming run's own output line shows it directly: `pkg-09  clear-accept  accept  reject  NO  failed: executable-by-a-stranger`. Loosening the check further to accept a named site (without a function or method) risks letting back in the genuinely vague "I'll investigate" plans the check was written to catch, so I left it as the one known, accepted miss rather than widen it further.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
