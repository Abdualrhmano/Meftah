# Meftah

A real-estate marketplace project intended to connect **property owners** and **clients** for listing discovery and direct messaging.

## Current Repository Status

This repository is currently in an **incomplete/archival state** on `main`:

- No application source files are present in the current tree.
- The latest `main` commit (`d6ebef9`) removed `miftah-real-estate.zip`.
- Historical scope can be verified from the previous upload commit (`608599d`), which contained a full project bundle.

Because the live tree is empty, the sections below document the verified historical implementation (not an active checked-in codebase).

## Verified Project Scope (from archived source)

The archived project (`miftah/`) was a full-stack web application with:

- Role-based auth (`owner`, `client`)
- Property publishing and management for owners
- Property browsing/filtering and details pages for clients
- Direct messaging between client and owner
- Email verification and password reset flows
- Media upload for images/videos
- Arabic/English UI with RTL/LTR support

## Architecture

### Frontend
- **Next.js 15 (App Router)**
- **React 19 + TypeScript**
- UI routes under `frontend/app/*`
- Reusable components under `frontend/components/*`
- Shared API/auth/i18n utilities under `frontend/lib/*`

### Backend
- **FastAPI**
- **SQLAlchemy 2 (async) + aiomysql**
- Routers: `auth`, `properties`, `media`, `conversations`
- Token/email/rate-limit/security modules in `backend/app/*`

### Data & Storage
- **MySQL 8**
- Media storage abstraction:
  - local disk (`uploads/`) for development
  - AWS S3-compatible storage for production

### Container Setup
- `docker-compose.yml` defined services for:
  - `db` (MySQL)
  - `backend` (FastAPI)
  - `frontend` (Next.js)

## Technology Stack

- Next.js, React, TypeScript
- FastAPI, Pydantic, SQLAlchemy, Alembic
- MySQL 8
- JWT + bcrypt
- Leaflet + OpenStreetMap
- Docker / Docker Compose

## Setup Requirements (archived app)

- Docker + Docker Compose, **or**
- Node.js 20+ and npm
- Python 3.11+
- MySQL 8

## Environment Variables (archived app)

### Backend (`backend/.env`)
Key variables (from `.env.example`):

- `DATABASE_URL`
- `AUTO_CREATE_TABLES`
- `SECRET_KEY`
- `ACCESS_TOKEN_EXPIRE_MINUTES`
- `CORS_ORIGINS`
- `FRONTEND_URL`
- `COOKIE_SECURE`
- `SMTP_HOST`, `SMTP_PORT`, `SMTP_USER`, `SMTP_PASSWORD`, `SMTP_FROM`, `SMTP_USE_TLS`
- `REQUIRE_EMAIL_VERIFICATION`
- `STORAGE_BACKEND`, `LOCAL_UPLOAD_DIR`, `PUBLIC_BASE_URL`
- `S3_BUCKET`, `S3_REGION`, `S3_PUBLIC_BASE_URL`
- `MAX_IMAGE_MB`, `MAX_VIDEO_MB`

### Frontend (`frontend/.env.local`)
- `NEXT_PUBLIC_API_URL`
- `API_URL`

## Installation & Run Commands (archived app)

### Docker
```bash
docker compose up --build
```
Frontend: `http://localhost:3000`  
Backend docs: `http://localhost:8000/docs`

### Manual
```bash
# backend
cd backend
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
uvicorn app.main:app --reload

# frontend (new terminal)
cd frontend
npm install
cp .env.local.example .env.local
npm run dev
```

## API & UI Behavior (verified)

### API areas
- `/api/auth/*`: register/login/me/profile/password/logout/verify/reset
- `/api/properties/*`: listing CRUD, status toggle, public listing/query/detail
- `/api/media/upload`: owner-only media upload
- `/api/conversations/*`: conversation start/list, message list/send, unread count

### UI areas
- Public home and property browsing
- Owner dashboard and property form (including map pin and media uploads)
- Property detail pages with server-side metadata generation
- Account management page (profile, password, account delete)
- Messaging inbox/thread with polling

## Security Notes (verified from archived backend)

- Password hashing via `bcrypt`
- JWT auth with role-based protection
- HTTP-only auth cookie support (alongside bearer token)
- Email verification and reset tokens stored as **SHA-256 hashes**
- Basic in-memory rate limiting for sensitive auth endpoints
- Upload validation by MIME type, size limits, and file signature checks

## Troubleshooting

- **Empty repository tree:** expected in current state; source was previously shipped as a zip and then removed.
- **Need the old code for recovery:** inspect historical commit `608599d`.
- **SMTP not configured:** archived backend printed verification/reset links to server logs when `SMTP_HOST` was empty.
- **Database schema management:** archived setup supported Alembic; avoid relying on auto-create in production.

## Contributing

At the moment, there is no active source tree on `main`. Recommended flow:

1. Open an issue describing intended restoration or next milestone.
2. Reintroduce source as normal tracked files (not a zip artifact).
3. Add tests and CI before major feature work.
4. Submit focused pull requests.

## License

No license file is currently present in this repository. All rights are reserved by default until a license is explicitly added.
