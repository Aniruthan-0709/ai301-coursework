# Plan: #61 Health check DB probe passes a raw SQL string

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61
My repro comment: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61#issuecomment-5861325937

## Diagnosis

`health_check()` in `api/routes/health.py` runs the Postgres probe as
`await db.execute("SELECT 1")`. SQLAlchemy 2.x (I ran 2.1.1) refuses a
plain string in `execute()`, so the call raises before anything is sent
to the database. The `except Exception` block catches that error and
marks Postgres "unhealthy", even though the database is up.

My unit 2 repro shows this. Calling the handler directly at commit
2f4e82f, with Postgres running in Docker:

```
=== Step 1: call health_check() directly (as the endpoint does) ===
[error    ] postgres_health_check_failed   error="Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')"
[error    ] redis_health_check_failed      error="'Settings' object has no attribute 'redis_host'"
[debug    ] vector_db_health_check_passed
HTTPException status=503
Detail: {'status': 'unhealthy', 'dependencies': {'postgres': 'unhealthy', 'redis': 'unhealthy', 'vector_db': 'healthy'}, ...}
```

The control in the same script rules out the database itself. Same
session type, same database, the only difference is wrapping the SQL
in `text()`:

```
=== Step 2: confirm text() fixes the underlying call ===
INFO sqlalchemy.engine.Engine SELECT 1
text('SELECT 1') result: 1
```

So the database is reachable and the cause is how the probe passes its
SQL, not the connection.

## Scope

In scope:
- Wrap the probe's SQL in `sqlalchemy.text()` in `api/routes/health.py`.
- A unit test for the Postgres probe.

Not in scope:
- The Redis failure in the same output (`'Settings' object has no
  attribute 'redis_host'`). That is a separate seeded bug, #62, and I'm
  not touching it.
- The `api.routes.health` mypy override in `pyproject.toml`. I ran mypy
  with that override removed: all three suppressed codes come from the
  Redis lines (`attr-defined`, `call-overload`) and the status dict
  (`index`), not from the probe line, so this fix doesn't make any of
  them obsolete. It stays as is.
- Any other cleanup in the health route (the vector DB placeholder, the
  `datetime.utcnow()` call, the safety-events placeholder).

## Files

- `api/routes/health.py`: import `text` from `sqlalchemy` and change
  the probe to `await db.execute(text("SELECT 1"))`.
- `tests/unit/test_health.py` (new): unit test for the Postgres probe.

## Approach

1. Create branch `fix/61-health-check-text-sql` from `main` on my fork.
2. In `api/routes/health.py`, add `from sqlalchemy import text` and
   change the one probe line to use `text("SELECT 1")`. Nothing else in
   the function changes.
3. Add `tests/unit/test_health.py` with a test that calls
   `health_check()` with a mocked `AsyncSession` and checks that
   `db.execute` was called with a `TextClause`, and that the returned
   status reports `postgres: healthy`. Because the Redis check still
   fails (#62), the handler will still raise a 503, so the test reads
   the dependency status from the exception's `detail` instead of
   expecting a 200.
4. Run `make lint`, `make typecheck`, and `make test-unit` before each
   commit. Keep `plan.md` and `comment.md` out of the commits.

## Test plan

Re-run my unit 2 repro script (`repro_health.py`) against the change,
same setup steps as in my repro comment.

Before (from my repro): Step 1 logs `postgres_health_check_failed` with
the `Textual SQL expression 'SELECT 1'` error, and the detail shows
`'postgres': 'unhealthy'`.

Expected after the fix:
- Step 1 logs `postgres_health_check_passed` and no
  `postgres_health_check_failed` line.
- The detail shows `'postgres': 'healthy'`.
- The overall result is still `HTTPException status=503` with
  `'status': 'unhealthy'`, because Redis still fails for the #62 reason.
  That is expected and not part of this fix.
- Step 2 (the `text()` control) still prints `text('SELECT 1') result: 1`.

The new unit test passes with `make test-unit`, and fails if I revert
the `text()` change.

## Risks and unknowns

- I haven't checked whether anything else in the repo calls
  `execute()` with a plain string. I'll grep for it, but if I find any
  I'll note them on the issue rather than fixing them here.
- Because Redis still fails, `GET /health` keeps returning 503 after
  this fix until #62 is fixed. Someone reading only the HTTP status
  might think the fix didn't work, so I'll say this in the PR.
- My repro ran on Windows with SQLAlchemy 2.1.1. I don't expect the fix
  to behave differently on other platforms, but I haven't checked.

## Deviations

<!-- Fill in after the build: what changed from this plan and why, or
that nothing changed. -->
