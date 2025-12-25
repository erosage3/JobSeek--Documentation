# Resume Screening System

A full-stack resume screening application (React frontend, FastAPI backend, SQL storage, MinIO for file storage, and scheduled tasks).

## Overview
- Frontend: React (Vite) app in webapp/.
- Backend: FastAPI app in core/ (entry: core/main.py).
- Database: Relational SQL (schema in schema.sql).
- Object storage: MinIO (env in minio.env, volume minio-data).
- Scheduler: APScheduler-style tasks in core/tasks/ (e.g., scheduler.py).

## Features
- Resume upload and parsing
- Job posting and applicant management
- Automated background tasks (scoring, sweeps)
- REST API for frontend and integrations

## Repository layout
- core/  FastAPI application and business logic
  - core/api/  route modules
  - core/service/  service layer (resume_service, job_service, etc.)
  - core/repo/  DB access and repository helpers
  - core/tasks/  scheduled/background jobs
  - core/util/  utilities (pdf_generator, jwt_utils, etc.)
  - core/main.py  FastAPI app entrypoint
  - core/setting.py  configuration
- webapp/  React frontend (Vite)
- minio-data/  persisted MinIO storage (Docker volume)
- docker-compose.yml  local compose setup
- schema.sql  DB schema and table definitions
- 
requirements.txt / pyproject.toml  backend dependencies
- package.json / package-lock.json  frontend deps (if frontend at root)

## Quickstart (development)
1. Backend setup

`powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
`

Create a .env (or update the existing .env) with at least the database and MinIO settings. Common vars used in this project:

`
DATABASE_URL=postgresql://user:password@localhost:5432/resume_db
MINIO_ENDPOINT=localhost:9000
MINIO_ACCESS_KEY=minioadmin
MINIO_SECRET_KEY=minioadmin
SCHEDULER_ENABLED=true
SECRET_KEY=your-secret
`

Run the backend (from repo root):

`powershell
uvicorn core.main:app --reload --port 8000
`

2. Frontend

`powershell
cd webapp/JobSeek   # or cd webapp if your React app is at webapp/
npm install
npm run dev
`

3. Docker (optional)

`powershell
docker compose up --build
`

This will start the backend, MinIO, and any other services defined in docker-compose.yml.

## Scheduler
Scheduled jobs live in core/tasks/ (see scheduler.py). For production, run a single scheduler instance to avoid duplicate job runs  either run it alongside the API in one container (single replica) or run a dedicated worker container.

## Database
- Schema available in schema.sql  apply with psql or your DB client.
- core/repo/db.py contains the database connection/initialization logic.

## MinIO / File storage
- MinIO config in minio.env. Uploaded resumes are stored in MinIO (see core/service/minio_client.py).
- minio-data/ is used for persistent storage in development.

## Useful commands
- Run backend: uvicorn core.main:app --reload --port 8000
- Run frontend: 
pm run dev (from frontend folder)
- Run docker-compose: docker compose up --build
- Install backend deps: pip install -r requirements.txt

## Tests & linting
- Backend tests (if present): pytest
- Frontend: use 
pm test or the scripts defined in webapp/JobSeek/package.json.

