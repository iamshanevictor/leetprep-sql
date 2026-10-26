# LeetPrep-SQL

LeetPrep-SQL is a SQL interview practice app built around a guided learning roadmap. Work through short lessons, run queries against example data, and solve a boss problem at the end of each module.

## What you can do

- Follow 10 ordered modules covering joins, aggregation, window functions, and other interview patterns.
- Learn with tutorials, guided examples, schemas, sample data, hints, and expected results.
- Run and submit read-only SQL against lesson-specific, in-memory DuckDB data.
- Keep lesson completion and draft queries in your browser's local storage, without an account.

The roadmap is the working practice experience. The separate `/problems` list currently has no standalone problems.

## Quick start

Use Python 3.11, Node.js 22, and npm. From the repository root, start the API in one terminal:

```powershell
cd backend
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python run.py
```

Start the app in another terminal:

```powershell
cd frontend
npm ci
npm run dev
```

Open [http://localhost:5173/roadmap](http://localhost:5173/roadmap). The frontend uses `http://localhost:5000/api` by default. If you create `frontend/.env` from the included example, set `VITE_API_BASE_URL=http://localhost:5000/api` for local use.

## Stack and documentation

Vue 3 and Vite power the frontend; Flask serves the API; DuckDB runs practice queries. Content lives in JSON files under `backend/learning_content/`.

- [Architecture and API](docs/architecture.md)
- [Development and content validation](docs/development.md)

Licensed under [MIT](LICENSE).
