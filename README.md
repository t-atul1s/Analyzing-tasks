# PML Coding Agent — Repo Analysis Exercises

Analysis artifacts for exercises **B1**, **B2**, **B3**, and **I1** from the coding-agent assessment.  
Source code was **not** copied here — only documentation produced by inspecting an open-source repo locally.

## Related repos

| Repo | Purpose |
|------|---------|
| [balance-api](https://github.com/t-atul1s/balance-api) | B4 — FastAPI balance/transactions API (code) |
| This repo | B1, B2, B3, I1 — OSS repo analysis (docs only) |

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

## How to verify B3

Clone the analyzed repo and run tests as documented in [b3-test-discovery.md](./b3-test-discovery.md):

```bash
git clone https://github.com/ptrstn/fastapi-sqlalchemy-pytest-example.git
cd fastapi-sqlalchemy-pytest-example
git checkout 19047d784f20b60f5772661be67c98b6ac90e248
pip install -e ".[test]"
pytest -v
```
