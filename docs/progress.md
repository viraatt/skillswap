# Project Progress & Roadmap Tracker

## Legend
- [x] Completed
- [-] In Progress
- [ ] Planned

---

## Milestone 1: Prototype Scaffolding & Setup
- [x] Initial monorepo directory layout (client/, server/, docs/).
- [x] Root .gitignore configured to exclude keys, builds, and dependencies.
- [x] Git repository initialized and baseline tracked.
- [ ] Install base dependencies (express, mongoose, firebase-admin, zod for backend; vite, react for frontend).

## Milestone 2: Backend Core & Database
- [ ] MongoDB Atlas connection setup in server/src/config/db.js.
- [ ] User, SwapSession, and Review Mongoose models implemented.
- [ ] Firebase Admin middleware for route authorization.
- [ ] REST endpoints for /api/users and /api/swaps.
- [ ] Dynamic rating aggregation pipeline query implementation.

## Milestone 3: Minimalist Frontend (CSES Aesthetic)
- [ ] Monospaced, zero-bloat CSS stylesheet (cses.css).
- [ ] User filter bar (filtering by skill offered, skill wanted, and minimum rating).
- [ ] Dense Swapper Data Table component.
- [ ] Swap request trigger modal.
- [ ] Minimalist user profile with rating and rank badges.

## Milestone 4: Escrow & Session Workflow
- [ ] Credit deduction and escrow holding logic on session booking.
- [ ] Session status state-machine transitions (PENDING -> ACCEPTED -> COMPLETED).
- [ ] Jitsi auto-room link generation per session.
- [ ] Dual-confirmation credit release logic.

## Milestone 5: Agentic Systems & ML Integration
- [ ] Cron framework for escrow_agent audit runs.
- [ ] Cyclic barter graph solver (match_agent).
- [ ] Embedding-based semantic search for related tech skills.
