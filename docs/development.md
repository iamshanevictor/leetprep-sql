# Development and content validation

Run the backend and frontend as described in the [README](../README.md#quick-start). The default API is `http://localhost:5000/api`; Vite serves the frontend on `http://localhost:5173`.

## Configuration

Backend settings are defined in `backend/app/config.py` and `backend/app/services/sql_runner.py`. `backend/.env.example` lists common values; a `backend/.env` file is optional for local development.

| Setting | Default | Use |
| --- | --- | --- |
| `SECRET_KEY` | `dev-secret-key-change-me` | Flask secret key |
| `DATABASE_URL` | SQLite under `backend/instance/` | App database configuration; roadmap progress is browser-local |
| `PROBLEMS_DIR` | `backend/problems` | Configured location for future standalone problem files |
| `CORS_ORIGINS` | Local Vite origins and a hosted frontend origin | Allowed API origins |
| `FRONTEND_ORIGIN` | Unset | Alternate origin setting when `CORS_ORIGINS` is unset |
| `SQL_MAX_QUERY_LENGTH` | `5000` | Maximum learner query characters |
| `SQL_MAX_RESULT_ROWS` | `200` | Maximum result rows |
| `SQL_QUERY_TIMEOUT_SECONDS` | `5` | Query execution limit |
| `DUCKDB_MEMORY_LIMIT` | `128MB` | DuckDB memory cap |

The frontend defaults to `http://localhost:5000/api` when `VITE_API_BASE_URL` is unset. `frontend/.env.example` contains a hosted API URL; set a local URL explicitly if you copy it into `frontend/.env` for development.

## Checks

From `backend/` after installing requirements:

```powershell
python scripts/validate_learning_content.py
python -m pytest
```

From `frontend/` after `npm ci`:

```powershell
npm run lint
npm run test
npm run build
npm run test:e2e
```

Playwright's end-to-end command uses Chromium and starts the Vite server. Install its browser separately with `npx playwright install chromium`. The GitHub Actions workflow runs these checks on pushes to `main` and pull requests.

## Learning content

`backend/learning_content/roadmap.json` defines module order and summaries. Each module directory under `backend/learning_content/modules/` contains lesson JSON and a `boss_problem.json`. Follow an existing module's structure when authoring content, then run `python scripts/validate_learning_content.py`: it validates required fields and executes expected queries against seed data.

Standalone practice problem JSON has not been added to `backend/problems/`; its directories are currently empty. The learning content is the supported path for adding working exercises.

