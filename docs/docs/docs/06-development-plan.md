# Development Plan: ClinicFlow

Pace assumption: about 10 hours per week. Sprint length: 1 week.
Size: S = under 2 hours, M = 2-5 hours, L = 5-10 hours.

## 1. Epics
| ID | Epic | Based on |
|---|---|---|
| E0 | Project setup and tooling | Phase 5 |
| E1 | Authentication and authorization | FR-1, NFR-1 to 3 |
| E2 | Doctor management | FR-3, FR-4 |
| E3 | Patient management | FR-5, FR-6 |
| E4 | Appointment management (core) | FR-2, FR-7 to 10, FR-13, FR-14 |
| E5 | Schedule view | FR-11, FR-12 |
| E6 | Frontend UI | all user stories |
| E7 | Testing, CI, logging | NFR-9, NFR-10 |
| E8 | Deployment | Phase 10 |
| E9 | Documentation and portfolio | Phases 11-12 |

## 2. Milestones
| Milestone | Goal | Target |
|---|---|---|
| M0 Foundation | Repo, backend skeleton, database runs in Docker, CI runs | Week 1 |
| M1 Secure access | Login works, roles enforced | Weeks 2-3 |
| M2 People and hours | Doctors, working hours, patients work via API | Week 4 |
| M3 Booking core | Book, cancel, reschedule, no double-booking (proven by tests) | Weeks 5-6 |
| M4 Usable product | Daily schedule + frontend screens | Weeks 7-8 |
| M5 Hardening | Logging, full test suite, security checks | Week 9 |
| M6 Release | Deployed, documented, resume-ready | Week 10 |

## 3. Product Backlog (in build order)
| ID | Story or task | Epic | Size | Milestone |
|---|---|---|---|---|
| T-01 | Create project folders (backend, frontend, docs) | E0 | S | M0 |
| T-02 | Set up Python virtual environment and dependencies | E0 | S | M0 |
| T-03 | Docker Compose with PostgreSQL | E0 | S | M0 |
| T-04 | FastAPI app with a /health endpoint | E0 | S | M0 |
| T-05 | Configuration from environment variables (.env) | E0 | S | M0 |
| T-06 | GitHub Actions: run Ruff and pytest on every push | E0 | M | M0 |
| T-07 | Set up SQLAlchemy and Alembic, first empty migration | E0 | M | M0 |
| S-01 | Users table and migration | E1 | M | M1 |
| S-02 | Password hashing utility (Argon2) | E1 | S | M1 |
| S-03 | Admin creates a user with a role (US-9) | E1 | M | M1 |
| S-04 | Login endpoint returns JWT (FR-1) | E1 | M | M1 |
| S-05 | "Current user" dependency that reads the token | E1 | M | M1 |
| S-06 | Role-check dependency (403 if not allowed) | E1 | M | M1 |
| S-07 | Auth tests: wrong password, expired token, wrong role | E1 | M | M1 |
| S-08 | Doctors table, add and list doctors (FR-3) | E2 | M | M2 |
| S-09 | Working hours table and endpoint (FR-4, US-8) | E2 | M | M2 |
| S-10 | Patients table, add patient (FR-5) | E3 | M | M2 |
| S-11 | Search patients by name or phone (FR-6, US-5) | E3 | M | M2 |
| S-12 | Appointments table with the no_double_booking constraint | E4 | L | M3 |
| S-13 | Book appointment: validation and working-hours check (US-1) | E4 | L | M3 |
| S-14 | Map the constraint error to 409 SLOT_TAKEN (US-2) | E4 | M | M3 |
| S-15 | Concurrency test: two simultaneous bookings, one wins | E4 | M | M3 |
| S-16 | Cancel appointment (US-3) | E4 | M | M3 |
| S-17 | Reschedule appointment in one transaction (US-4) | E4 | L | M3 |
| S-18 | Audit log written on every change (FR-14) | E4 | M | M3 |
| S-19 | Daily schedule endpoint (FR-11) | E5 | M | M4 |
| S-20 | Doctor sees only own schedule (FR-12, US-7) | E5 | M | M4 |
| S-21 | Frontend: login screen and token handling | E6 | M | M4 |
| S-22 | Frontend: patient search and add | E6 | M | M4 |
| S-23 | Frontend: booking form with error messages | E6 | L | M4 |
| S-24 | Frontend: daily schedule with cancel/reschedule | E6 | L | M4 |
| S-25 | Application logging with timestamps (NFR-9) | E7 | M | M5 |
| S-26 | Fill test gaps for all business rules BR-1 to BR-11 (NFR-10) | E7 | L | M5 |
| S-27 | Security checks: no password in responses, no data without token | E7 | M | M5 |
| S-28 | Seed script with synthetic data only (NFR-6) | E7 | S | M5 |
| S-29 | Production configuration and Dockerfile | E8 | M | M6 |
| S-30 | Deploy database, backend, frontend | E8 | L | M6 |
| S-31 | README, API docs, architecture docs, setup guide | E9 | L | M6 |

## 4. Example: one story broken into tasks (S-13, Book appointment)
1. Write the request schema (doctor_id, patient_id, start_time).
2. Reject times that are not :00 or :30, or are in the past (BR-2, BR-6).
3. Compute end_time = start + 30 minutes (BR-1).
4. Load the doctor's working hours and check the booking fits (BR-3).
5. Insert the appointment through the repository.
6. Add tests: success, outside hours, past time, bad minute value.

## 5. Definition of Ready (before starting a story)
- It links to a requirement ID.
- Acceptance criteria exist (or I write them first).
- Its dependencies are finished.

## 6. Definition of Done (before closing a story)
- Code works and I have run it myself.
- Tests exist and pass, including at least one failure case.
- Ruff shows no errors.
- No secrets in the code.
- I can explain the code, and my notes are in docs/learning-notes.md.
- Merged to main through a pull request.

## 7. Weekly rhythm
- Monday: choose stories from the top of the backlog (the sprint plan).
- During the week: one story at a time, one branch per story.
- Sunday: short review. What finished? What blocked me? What changes next week?
  Write 3 lines in docs/retrospectives.md.