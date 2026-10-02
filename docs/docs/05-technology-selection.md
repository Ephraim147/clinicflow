# Technology Selection: ClinicFlow

How we chose: for each decision we list the options, compare them against
our real needs (from Phases 2-4), pick one, and record the trade-off we accept.

Our needs that drive the choices:
- Prevent double-booking at database level (R-1, NFR-5)
- Clear layers: routes, services, repositories (Phase 4)
- Beginner-friendly, free to run, easy for AI tools to generate and for me to read
- Strong testing and a simple CI pipeline

## TS-1: Backend language and framework

| Option | Strengths | Weaknesses for this project |
|---|---|---|
| FastAPI (Python) | Explicit structure, automatic input validation (Pydantic), automatic API docs page, modern and widely used | No built-in admin or login; we assemble pieces ourselves |
| Django (Python) | Login, admin panel, ORM all built in | More "magic"; hides the layers we want to learn; heavier for a JSON API |
| Flask (Python) | Very small and simple | We would add validation, docs, and structure by hand |
| Node.js + Express | Same language as a React frontend | Switches away from Python, our strongest language |

Decision: FastAPI.
Trade-off accepted: we write login and role checks ourselves. That is extra
work, but it is exactly the security knowledge we want to learn and explain.

## TS-2: Database

| Option | Strengths | Weaknesses |
|---|---|---|
| PostgreSQL | Supports the exclusion constraint that blocks overlapping bookings; strong transactions; industry standard | Needs a running server (we use Docker) |
| MySQL | Popular, familiar | No direct equivalent of our overlap constraint; we would need locking tricks |
| SQLite | Zero setup | Weak concurrency; no exclusion constraints; hides the very problem we need to solve |
| MongoDB | Flexible documents | Our data is relational (doctors, patients, appointments); we need strict constraints |

Decision: PostgreSQL.
Trade-off accepted: slightly more setup. This choice is non-negotiable
because the no_double_booking constraint from Phase 4 depends on it.

## TS-3: Database access and migrations

| Option | Strengths | Weaknesses |
|---|---|---|
| SQLAlchemy 2.x + Alembic | Industry standard; models in Python; versioned schema migrations | Learning curve |
| Raw SQL only | Full control, very transparent | Repetitive; no migration history |
| Django ORM | Simple | Only available with Django |

Decision: SQLAlchemy + Alembic.
Trade-off accepted: the exclusion constraint has no simple ORM syntax, so we
add it in a migration using raw SQL. That is normal practice and a good
interview story.

## TS-4: Authentication

| Option | Strengths | Weaknesses |
|---|---|---|
| JWT access tokens (signed, expiring) | Simple, stateless, works well with a separate frontend | Cannot be revoked easily before expiry |
| Server sessions in database | Easy to revoke | More tables and logic |
| Third-party login (Auth0, Google) | Less security code to write | Hides what we want to learn; adds outside dependency |

Decision: JWT with a 30-minute expiry. Passwords hashed with Argon2 (or bcrypt).
Trade-off accepted: no instant logout of a stolen token. Short expiry limits
the damage. A refresh-token flow is future scope.

## TS-5: Frontend

| Option | Strengths | Weaknesses |
|---|---|---|
| React + Vite | Most common in job listings; matches our JSON API design | More concepts (components, state) |
| Server-rendered pages (Jinja + HTMX) | Less JavaScript, simpler | Does not match the separate-frontend design; less recognizable on a resume |
| Streamlit | Very fast to build | Not suited to a multi-role booking app with login |

Decision: React + Vite, kept minimal: plain CSS, no heavy UI library.
Trade-off accepted: extra learning for a part that is not the heart of the
project. We keep the UI to five screens and let AI generate most of it.

## TS-6: Testing

Decision: pytest with FastAPI's TestClient.
Rule: tests run against a real PostgreSQL database, not SQLite, because the
double-booking constraint only exists in PostgreSQL. A test that uses SQLite
would pass while production fails.

## TS-7: Local environment and containers

Decision: Docker Compose runs PostgreSQL on our machine with one command.
Trade-off accepted: Docker must be installed. In return, everyone (and CI)
gets the same database version, and no manual PostgreSQL install is needed.

## TS-8: Version control, CI, and quality tools
- Git + GitHub for code, Issues, and the project board.
- GitHub Actions for CI: on every push, run lint and tests.
- Ruff for code style checks (fast, one tool).
- Python logging for application logs (NFR-9).

## TS-9: Deployment (Phase 10 detail later)
Plan: backend and database on a free or student-friendly host (for example
Render, Railway, Fly.io, or a free PostgreSQL provider such as Neon), and the
frontend on a static host (for example Vercel or Netlify).
Free tiers change often, so we confirm current limits when we reach Phase 10.

## What we deliberately do NOT use
| Technology | Why not |
|---|---|
| Microservices | One small team, one app; adds complexity without benefit |
| Redis / Celery | We have no background jobs in the MVP (reminders are future scope) |
| Kubernetes | Far beyond our scale |
| GraphQL | REST is enough and simpler to test |
| Any ML library | No-show prediction is out of MVP scope |

## Final Stack
Python 3.11+ | FastAPI | Pydantic | SQLAlchemy 2.x | Alembic | PostgreSQL |
JWT + Argon2 | React + Vite | pytest | Docker Compose | GitHub Actions | Ruff