# Architecture Specification

## 1. High-Level Monorepo Architecture

```
[ Client: Vite / React / Monospaced CSS ]
                   |
             HTTP / REST (JSON)
                   v
[ Express.js Gateway & Router ]
         |                 |
  [Auth Middleware]   [Zod Request Validation]
  (Firebase Admin)         |
         |                 v
         +--------> [ Controllers & Business Logic ]
                           |
             +-------------+-------------+
             |                           |
             v                           v
     [ Mongoose Models ]      [ Dynamic Aggregation ]
             |                           |
             +-------------+-------------+
                           |
                           v
                   [ MongoDB Atlas ]
```

---

## 2. Directory Separation & Responsibilities

- **client/ (Frontend Engine):**
  - Text-dense, table-driven presentation.
  - Stateless auth lifecycle via Firebase Client SDK.
  - Thin API client (src/services/api.js) forwarding Firebase bearer tokens.
- **server/ (Backend & Ledger Layer):**
  - **middlewares/authMiddleware.js**: Decodes and verifies incoming Bearer tokens against the Firebase Admin SDK and injects user context.
  - **controllers/swapController.js**: Enforces credit checks, session locking, and status transitions.
  - **controllers/userController.js**: Executes MongoDB aggregation pipelines to calculate ranks and ratings on-the-fly.

---

## 3. Security & Transaction Integrity

- **Escrow Mechanism:** Booking a 1-hour session locks 1 credit from the student into an escrow state. The credit is transferred to the instructor only when both parties mark the session complete or valid room attendance is verified.
- **Input Validation:** Strict Zod schemas guard every route to prevent arbitrary MongoDB query injections or malformed timestamp submissions.
