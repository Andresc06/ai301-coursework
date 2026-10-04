# Plan: fix #62 — health check references `settings.redis_host`, which does not exist on `Settings`

## Diagnosis

The Redis probe in `api/routes/health.py` builds its client with `settings.redis_host` and
`settings.redis_port`:

```python
r = redis.Redis(
    host=settings.redis_host,
    port=settings.redis_port,
    db=0,
    decode_responses=True,
)
```

`Settings` in `core/config.py` does not define either field — it only defines a single
`redis_url: str = Field(default="redis://localhost:6379/0")`. Accessing `settings.redis_host`
raises an `AttributeError`, which the surrounding `except Exception` catches and reports as
`"redis": "unhealthy"`, even when Redis is actually reachable.

This matches my Unit 2 reproduction exactly. Backend log at the time of the request:

> `redis_health_check_failed error="'Settings' object has no attribute 'redis_host'"`

And Redis itself was confirmed healthy at the same moment:

> "Confirmed Redis itself was healthy at the time — `docker compose ps` showed the `redis`
> container `Up ... (healthy)` via its own `redis-cli ping` healthcheck — so this is the code
> bug, not a real outage."

## Scope

**In scope:** the Redis probe block inside `health_check()` in `api/routes/health.py` — how it
builds its Redis client.

**Not in scope:**
- The Postgres probe in the same function. My Unit 2 repro showed `"postgres": "unhealthy"`
  at the same time as this bug, for an unrelated reason: it calls `db.execute("SELECT 1")`
  with a raw string, which SQLAlchemy 2.x rejects (the DB probe needs `sqlalchemy.text(...)`,
  tracked separately as #61). Fixing that is a different diagnosed cause and out of scope here.
- The vector_db probe in the same function. It reported `"healthy"` in my repro, but I haven't
  verified it beyond reading the code: it only checks that `settings.vector_db_url` is
  non-empty, it doesn't actually connect. I'm not touching it because I have no evidence it's
  broken, not because I've confirmed it's correct.
- The broad `except Exception` pattern shared by all three probes. It's working as designed
  (catch any failure, report unhealthy); narrowing exception handling repo-wide is a larger,
  unrelated change and not needed to fix this bug.
- Adding new fields to `core/config.py`. `redis_url` already exists and is the correct field
  to use; no config changes needed.

## Files I'll touch

- `api/routes/health.py` — the Redis probe's client-construction lines only.
- `tests/unit/` — one new regression test covering this probe.

## Approach

Replace the host/port constructor call with `redis.Redis.from_url(settings.redis_url,
decode_responses=True)`, which redis-py supports natively — `redis_url` is a connection-string
field, the same shape as `database_url`/`vector_db_url`, but neither of those is read by this
fix; see Scope for why those probes aren't touched. No change to the surrounding `try/except`
structure: the probe still reports `"unhealthy"` on any real connection failure, it just no
longer does so because of this attribute typo.

## Test plan

Re-run my Unit 2 repro steps against the fix:

1. `cp .env.example .env`, `docker compose up -d`, `make setup`, `make run` (same setup as
   Unit 2).
2. `GET http://localhost:5173/api/health` with Redis running.

**Before (Unit 2, unpatched):** `503` with `"redis": "unhealthy"` in the response body, and
`redis_health_check_failed error="'Settings' object has no attribute 'redis_host'"` in the
log.

**Expected after:** `"redis": "healthy"` in the dependencies block, no `AttributeError` in the
log, and `docker compose ps` still shows Redis `Up ... (healthy)` throughout — confirming it's
the same reachable Redis as before, just correctly detected now.

**Additional case:** stop the Redis container (`docker compose stop redis`) and re-hit
`/health` — expect `"redis": "unhealthy"` to still correctly report a real outage, so the fix
doesn't just make the check always report healthy.

## Risks and unknowns

- I haven't yet confirmed `redis.Redis.from_url` accepts `decode_responses=True` alongside a
  URL in the installed `redis-py` version (8.1.0 per my Unit 2 environment) — I expect it does,
  per redis-py's documented API, but I'll confirm this when I build rather than assert it here.
- I have not audited the Postgres or vector_db probes for a similar attribute-mismatch bug.
  Postgres is already known to fail, for a different reason (#61's raw `SELECT 1` string);
  vector_db only checks that `settings.vector_db_url` is set, without connecting. Both are
  outside this fix's scope, not because I've confirmed they're correct.

## Deviations

Nothing changed; the build matched the plan exactly. The fix is the one-line change in
`api/routes/health.py`'s Redis probe (`redis.Redis.from_url(settings.redis_url,
decode_responses=True)`, replacing the `host=settings.redis_host, port=settings.redis_port`
call), plus one new test, `tests/unit/test_health.py`, covering the client construction and
guarding against regressing to the old `redis_host`/`redis_port` fields. The only thing beyond
the plan was an automatic `ruff` formatting fix applied by the repo's pre-commit hook during
`git commit` — a style fix, not a logic or scope change.

