# Git Workflow: ClinicFlow

## 1. Branches
- main: always working, protected. Nobody commits directly.
- One branch per issue, named: type/ISSUE-short-description

Examples:
- feature/S-04-login-endpoint
- fix/S-14-slot-taken-error
- chore/T-01-project-folders
- docs/S-31-readme

## 2. Commit messages (Conventional Commits)
Format: type: short description (present tense, under 70 characters)

| Type | Use for |
|---|---|
| feat | a new feature |
| fix | a bug fix |
| docs | documentation only |
| test | adding or changing tests |
| chore | setup, tooling, config |
| refactor | restructuring code, same behavior |

Examples:
- feat: add login endpoint
- fix: return 409 when slot is taken
- test: add concurrent booking test
- chore: add docker compose for postgres

Rules:
- Small commits: one idea per commit.
- Never commit secrets (.env, passwords, tokens).

## 3. Pull requests
- One PR per issue.
- The PR description says what changed, why, and how I tested it.
- Write "Closes #N" so the issue closes automatically on merge.
- Before merging, I read my own diff and tick the checklist.
- Merge method: Squash and merge (keeps main history clean).

## 4. Issues
- Every piece of work starts as an issue (Goal, Requirement, Tasks, Done when).
- Every branch and PR points back to an issue number.

## 5. Secrets and environment variables
- Real secrets live only in .env (ignored by Git).
- .env.example lists the variable names with fake values and IS committed.
- If a secret is ever committed by mistake, change it immediately.

## 6. Code review checklist (for my own PRs)
- [ ] Does it do what the issue says?
- [ ] Do the tests pass, including a failure case?
- [ ] Any secrets, debug prints, or commented-out code left?
- [ ] Is business logic in the service layer (not in routes)?
- [ ] Can I explain every file I changed?
- [ ] Learning notes added?