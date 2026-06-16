# Exercise B2 - API Endpoint Map

- Repo URL: https://github.com/ptrstn/fastapi-sqlalchemy-pytest-example
- Commit hash: `19047d784f20b60f5772661be67c98b6ac90e248`
- Date analyzed: `2026-06-16`

## External HTTP Routes

| Method | Path | Handler file | Function | Description |
|---|---|---|---|---|
| `GET` | `/api/` | `src/mypackage/api/v1/router.py` | `get_home` | Health/home-style API root that returns a simple greeting payload. |
| `GET` | `/api/users/` | `src/mypackage/api/v1/endpoints/users.py` | `read_users` | List users with pagination (`skip`, `limit`). |
| `POST` | `/api/users/` | `src/mypackage/api/v1/endpoints/users.py` | `create_user` | Create a user; returns `422` when email already exists. |
| `GET` | `/api/items/` | `src/mypackage/api/v1/endpoints/items.py` | `read_items` | List items with pagination (`skip`, `limit`). |
| `POST` | `/api/items/` | `src/mypackage/api/v1/endpoints/items.py` | `create_item` | Create an item without binding to a specific user in the path. |
| `POST` | `/api/users/{user_id}/items/` | `src/mypackage/api/v1/endpoints/items.py` | `create_item_for_user` | Create an item for a specific user; returns `404` if user is missing. |

## Route Grouping Diagram

```mermaid
graph TD
    A[/api/] --> H[get_home]
    B[/api/users/] --> U1[read_users]
    B --> U2[create_user]
    C[/api/items/] --> I1[read_items]
    C --> I2[create_item]
    D[/api/users/{user_id}/items/] --> I3[create_item_for_user]
```
