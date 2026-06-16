# Exercise B3 — Test Discovery and Execution

- Repo URL: https://github.com/ptrstn/fastapi-sqlalchemy-pytest-example
- Commit hash: `19047d784f20b60f5772661be67c98b6ac90e248`
- Date analyzed: `2026-06-16`

## Test Framework

| Item | Value |
|---|---|
| Framework | **pytest** |
| HTTP client for API tests | `httpx` (via FastAPI `TestClient`) |
| Coverage plugin | `pytest-cov` |
| Env loading plugin | `pytest-dotenv` |

## Configuration Files

| File | Purpose |
|---|---|
| `pyproject.toml` | Declares `[project.optional-dependencies].test` (pytest, pytest-cov, pytest-dotenv, httpx, black, flake8); `[tool.pytest.ini_options]` loads `.test.env`; coverage omit/report settings |
| `.test.env` | Sets `DB_DIALECT=sqlite` and `DB_DATABASE=:memory:` so tests use an in-memory SQLite database |
| `tests/conftest.py` | Shared fixtures: module-scoped DB create/drop (`create_test_database`), per-function DB session (`session`), module-scoped FastAPI `TestClient` (`client`) |
| `.flake8` | Lint config (used in CI, not by pytest directly) |
| `.github/workflows/python-package.yml` | CI runs `pip install -e .[test]` then `pytest --cov --cov-report=xml` |

Note: `pyproject.toml` defines optional deps under `[test]`, not `[dev]`. README and CI both use `.[test]`.

## Test Files

| File | Purpose |
|---|---|
| `tests/conftest.py` | Pytest fixtures for in-memory DB lifecycle, session injection, and FastAPI test client |
| `tests/test_api.py` | Integration tests for HTTP endpoints: root, users CRUD, items CRUD, duplicate-user error, item-for-user with 404 |
| `tests/test_crud.py` | Unit test for `get_user_by_email` returning `None` when email does not exist |
| `tests/test_database.py` | ORM relationship test for `User`/`Item` ownership, transient vs persisted state, and SQLAlchemy warnings on refresh |
| `tests/test_settings.py` | Unit tests for `DatabaseSettings.database_url` across SQLite, MongoDB, and PostgreSQL dialect configurations |

## Commands Run

```bash
cd /Users/atul/Desktop/repos/fastapi-sqlalchemy-pytest-example
pip install -e ".[test]"
pytest -v                    # failed: pytest not on PATH
python3 -m pytest -v         # succeeded
```

Install used `.[test]` per `pyproject.toml` and README (no `[dev]` extra defined). `pytest` was installed to `/Users/atul/Library/Python/3.13/bin`, which is not on PATH; `python3 -m pytest` was used instead.

## Pytest Output (full)

```
============================= test session starts ==============================
platform darwin -- Python 3.13.7, pytest-9.1.0, pluggy-1.6.0 -- /Library/Frameworks/Python.framework/Versions/3.13/bin/python3
cachedir: .pytest_cache
rootdir: /Users/atul/Desktop/repos/fastapi-sqlalchemy-pytest-example
configfile: pyproject.toml
plugins: cov-7.1.0, dotenv-0.5.2, anyio-4.11.0, langsmith-0.4.43
collecting ... collected 15 items

tests/test_api.py::test_read_main PASSED                                 [  6%]
tests/test_api.py::test_get_items PASSED                                 [ 13%]
tests/test_api.py::test_get_users PASSED                                 [ 20%]
tests/test_api.py::test_post_user PASSED                                 [ 26%]
tests/test_api.py::test_get_users_after_post PASSED                      [ 33%]
tests/test_api.py::test_create_item PASSED                               [ 40%]
tests/test_api.py::test_create_item_for_user PASSED                      [ 46%]
tests/test_crud.py::test_get_user_by_email PASSED                        [ 53%]
tests/test_database.py::test_create_users_and_items PASSED               [ 60%]
tests/test_settings.py::test_in_memory_sqlite PASSED                     [ 66%]
tests/test_settings.py::test_in_memory_sqlite_no_filename PASSED         [ 73%]
tests/test_settings.py::test_sqlite_file PASSED                          [ 80%]
tests/test_settings.py::test_mongodb PASSED                              [ 86%]
tests/test_settings.py::test_postgres PASSED                             [ 93%]
tests/test_settings.py::test_postgres_no_port PASSED                     [100%]

=============================== warnings summary ===============================
tests/test_api.py::test_post_user
  /Library/Frameworks/Python.framework/Versions/3.13/lib/python3.13/site-packages/anyio/_backends/_asyncio.py:976: DeprecationWarning: 'HTTP_422_UNPROCESSABLE_ENTITY' is deprecated. Use 'HTTP_422_UNPROCESSABLE_CONTENT' instead.
    result = context.run(func, *args)

-- Docs: https://docs.pytest.org/en/stable/how-to/capture-warnings.html
======================== 15 passed, 1 warning in 0.15s =========================
```

## Result Interpretation

**All 15 tests passed.** No failures.

One deprecation warning during `test_post_user`: FastAPI/Starlette is deprecating `HTTP_422_UNPROCESSABLE_ENTITY` in favor of `HTTP_422_UNPROCESSABLE_CONTENT`. This is triggered when the duplicate-email path returns a 422 response in `src/mypackage/api/v1/endpoints/users.py`; it does not affect test correctness today.
