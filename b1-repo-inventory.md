# Exercise B1 - Repo Artifact Inventory

- Repo URL: https://github.com/ptrstn/fastapi-sqlalchemy-pytest-example
- Commit hash: `19047d784f20b60f5772661be67c98b6ac90e248`
- Date analyzed: `2026-06-16`

## Models / Schemas

| Artifact | Type | File path |
|---|---|---|
| `User`, `Item` | SQLModel ORM models | `src/mypackage/models.py` |
| `UserBase`, `UserCreate`, `User`, `ItemBase`, `ItemCreate`, `Item` | Pydantic request/response schemas | `src/mypackage/schemas.py` |

## API Routers and Endpoints Modules

| Artifact | Type | File path |
|---|---|---|
| `api_router` and root `/api` handler | API router aggregator | `src/mypackage/api/v1/router.py` |
| Users endpoints (`GET /users/`, `POST /users/`) | Endpoint module | `src/mypackage/api/v1/endpoints/users.py` |
| Items endpoints (`GET /items/`, `POST /items/`, `POST /users/{user_id}/items/`) | Endpoint module | `src/mypackage/api/v1/endpoints/items.py` |
| FastAPI app bootstrap and router registration | Application entrypoint module | `src/mypackage/main.py` |

## CRUD / Services

| Artifact | Type | File path |
|---|---|---|
| `get_items`, `get_users`, `get_user_by_email`, `create_user`, `create_item`, `create_item_for_user` | CRUD/service functions | `src/mypackage/crud.py` |

## Database / Config / Settings

| Artifact | Type | File path |
|---|---|---|
| `DatabaseSettings`, `database_settings` | Environment-backed DB settings | `src/mypackage/settings.py` |
| `create_engine_from_config`, `engine`, `create_db_and_tables`, `drop_db`, `get_session` | DB engine/session lifecycle module | `src/mypackage/database.py` |
| Test environment variables | Environment config file | `.test.env` |
| Project/tooling configuration | Build/test/lint config | `pyproject.toml` |

## Tests

| Artifact | Type | File path |
|---|---|---|
| Pytest fixtures for DB/session/client | Test fixture module | `tests/conftest.py` |
| API behavior tests | Endpoint integration tests | `tests/test_api.py` |
| CRUD unit test | CRUD behavior test module | `tests/test_crud.py` |
| DB model/relationship tests | Persistence model tests | `tests/test_database.py` |
| Settings/database URL tests | Configuration tests | `tests/test_settings.py` |

## Utilities

| Artifact | Type | File path |
|---|---|---|
| FastAPI lifespan hook (`lifespan`) used for startup table creation | Application utility function | `src/mypackage/main.py` |
| Shared pytest setup helpers (`create_test_database`, `session`, `client`) | Test utility fixtures | `tests/conftest.py` |
