# PML Coding Agent — Repo Analysis Exercises

Analysis artifacts for exercises **B1**, **B2**, **B3**, **I1**, and **I3** from the coding-agent assessment.  
Source code was **not** copied here — only documentation produced by inspecting an open-source repo locally.

## Related repos

| Repo | Purpose |
|------|---------|
| [balance-api](https://github.com/t-atul1s/balance-api) | B4 — FastAPI balance/transactions API (code) |
| This repo | B1, B2, B3, I1, I3 — OSS repo analysis + I3 writeup |
| [fastapi-sqlalchemy-pytest-example (fork)](https://github.com/t-atul1s/fastapi-sqlalchemy-pytest-example) | I3 — code change on branch `feature/get-user-by-id` |

## Analyzed source repo

| Field | Value |
|-------|-------|
| URL | https://github.com/ptrstn/fastapi-sqlalchemy-pytest-example |
| Commit | `19047d784f20b60f5772661be67c98b6ac90e248` |
| Date | 2026-06-16 |

## Exercises

| ID | Exercise | Document | Summary |
|----|----------|----------|---------|
| B1 | Repo artifact inventory | [b1-repo-inventory.md](./b1-repo-inventory.md) | Models, routers, CRUD, config, tests |
| B2 | API endpoint map | [b2-api-map.md](./b2-api-map.md) | All HTTP routes → handler files |
| B3 | Test discovery & execution | [b3-test-discovery.md](./b3-test-discovery.md) | pytest setup, commands, full output |
| I1 | ER diagram from repo | [i1-er-diagram.md](./i1-er-diagram.md) | Tables, keys, relationships, Mermaid ER |
| I3 | Small safe change | [i3-small-safe-change.md](./i3-small-safe-change.md) | PR branch, diff, tests, risk notes |

## How to verify B3

Clone the analyzed repo and run tests as documented in [b3-test-discovery.md](./b3-test-discovery.md):

```bash
git clone https://github.com/ptrstn/fastapi-sqlalchemy-pytest-example.git
cd fastapi-sqlalchemy-pytest-example
git checkout 19047d784f20b60f5772661be67c98b6ac90e248
pip install -e ".[test]"
pytest -v
```
