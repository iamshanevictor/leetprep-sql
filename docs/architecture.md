# Architecture and API

LeetPrep-SQL has a Vue single-page frontend (`frontend/src/`) and a Flask API (`backend/app/`). The API reads roadmap, lesson, and boss-problem JSON from `backend/learning_content/`; the current learning flow does not use the app database.

## Practice flow

1. The frontend fetches roadmap and module metadata from the API.
2. A lesson or boss workspace fetches its authored schema, seed data, prompt, and expected query. The API also computes an expected result for display.
3. A learner runs or submits a query. The backend validates it as read-only, creates an in-memory DuckDB database with that content's tables and seed rows, and executes the query.
4. Submission grading compares result columns and rows with the expected query's output. Row order is checked when the content's `order_matters` setting requires it.
5. The frontend stores completed lessons, completed boss problems, draft queries, and visit data in browser local storage. Lessons and modules unlock in sequence; a boss problem unlocks after its module's lessons are complete. Unlock checks are frontend behavior; the API routes do not enforce progress prerequisites.

Query length, returned row count, execution time, and DuckDB memory are limited by settings in `backend/app/services/sql_runner.py`.

## Main API routes

All routes have an `/api` prefix.

| Method | Route | Purpose |
| --- | --- | --- |
| GET | `/health` | Health check |
| GET | `/roadmap`, `/modules` | Roadmap and module lists |
| GET | `/modules/<module_id>` | Module details |
| GET | `/modules/<module_id>/lessons/<lesson_id>` | Lesson content |
| POST | `/modules/<module_id>/lessons/<lesson_id>/run` | Execute lesson SQL |
| POST | `/modules/<module_id>/lessons/<lesson_id>/submit` | Grade lesson SQL |
| GET | `/modules/<module_id>/boss` | Boss-problem content |
| POST | `/modules/<module_id>/boss/run` | Execute boss SQL |
| POST | `/modules/<module_id>/boss/submit` | Grade boss SQL |

Run and submit endpoints accept JSON with a `query` string. The standalone `/problems` list and `/attempts` list currently return empty lists; they do not provide persistent problem or attempt tracking.

## Code map

- `backend/app/api/`: Flask route handlers.
- `backend/app/services/`: content loading, DuckDB execution, and grading.
- `backend/app/utils/sql_safety.py`: read-only SQL validation.
- `backend/learning_content/`: roadmap metadata and module JSON.
- `frontend/src/pages/` and `frontend/src/components/`: app screens and workspaces.
- `frontend/src/services/progressStorage.js`: browser-local progress and drafts.

