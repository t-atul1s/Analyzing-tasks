# Exercise I3 — Small Safe Change in Unfamiliar Repo

## Source repos

| Role | URL |
|------|-----|
| Upstream (analyzed) | https://github.com/ptrstn/fastapi-sqlalchemy-pytest-example |
| Base commit (before change) | `19047d784f20b60f5772661be67c98b6ac90e248` |
| My fork | https://github.com/t-atul1s/fastapi-sqlalchemy-pytest-example |
| Branch | `feature/get-user-by-id` |
| PR (open on fork) | https://github.com/t-atul1s/fastapi-sqlalchemy-pytest-example/pull/new/feature/get-user-by-id |
| Change commit | `b67d8d5` |

## Change summary

Added **`GET /api/users/{user_id}`** to fetch a single user by ID. The repo already had list (`GET /users/`) and create (`POST /users/`) but no read-by-id endpoint.

## Files changed and why

| File | Why |
|------|-----|
| `src/mypackage/crud.py` | Added `get_user_by_id()` — minimal DB lookup reused by the new route |
| `src/mypackage/api/v1/endpoints/users.py` | Added `read_user` handler; returns 404 with same message style as items routes |
| `tests/test_api.py` | Added `test_get_user_by_id` (200) and `test_get_user_by_id_not_found` (404) |

## Diff stats

```
 src/mypackage/api/v1/endpoints/users.py | 11 +++++++++++
 src/mypackage/crud.py                   |  4 ++++
 tests/test_api.py                       | 16 ++++++++++++++++
 3 files changed, 31 insertions(+)
```

## Test command and result

```bash
cd fastapi-sqlalchemy-pytest-example
pip install -e ".[test]"
python3 -m pytest -v
```

```
============================= test session starts ==============================
platform darwin -- Python 3.13.7, pytest-9.1.0, pluggy-1.6.0
collected 17 items

tests/test_api.py::test_read_main PASSED
tests/test_api.py::test_get_items PASSED
tests/test_api.py::test_get_users PASSED
tests/test_api.py::test_post_user PASSED
tests/test_api.py::test_get_users_after_post PASSED
tests/test_api.py::test_create_item PASSED
tests/test_api.py::test_create_item_for_user PASSED
tests/test_api.py::test_get_user_by_id PASSED
tests/test_api.py::test_get_user_by_id_not_found PASSED
tests/test_crud.py::test_get_user_by_email PASSED
tests/test_database.py::test_create_users_and_items PASSED
tests/test_settings.py::test_in_memory_sqlite PASSED
tests/test_settings.py::test_in_memory_sqlite_no_filename PASSED
tests/test_settings.py::test_sqlite_file PASSED
tests/test_settings.py::test_mongodb PASSED
tests/test_settings.py::test_postgres PASSED
tests/test_settings.py::test_postgres_no_port PASSED

======================== 17 passed, 1 warning in 0.11s =========================
```

## Risk assessment

| Risk | Level | Notes |
|------|-------|-------|
| Breaking existing API | **Low** | New route only; no changes to existing endpoints |
| Route conflict | **Low** | `/users/{user_id}` does not clash with `/users/` |
| Data exposure | **Low** | Uses existing `schemas.User` response (password never returned) |
| Test isolation | **Low** | New tests appended after existing user setup tests |

## Agent suggested vs manually verified

| Item | Agent / implementation | Manually verified |
|------|------------------------|-------------------|
| CRUD helper | `db.get(User, user_id)` pattern | Confirmed matches `create_item_for_user` style |
| 404 message | `User with id '{user_id}' not found` | Matches items endpoint wording |
| Tests | Two cases: found + not found | Ran full suite — 17/17 pass |
| Test order | New tests at end of file | Confirmed no interference with existing tests |

## How to verify

```bash
git clone https://github.com/t-atul1s/fastapi-sqlalchemy-pytest-example.git
cd fastapi-sqlalchemy-pytest-example
git checkout feature/get-user-by-id
pip install -e ".[test]"
python3 -m pytest -v
```
