# SkillSwap 

SkillSwap is an ultra-lightweight, zero-bloat peer-to-peer skill barter platform. It removes transactional monetary overhead by allowing developers and learners to trade knowledge directly through a time-credit ledger.

Built with a high-density, text-first user interface inspired by platforms like Codeforces and the CSES Problem Set.

---

## 1. Core Principles

- **Utilitarian & Minimal:** No landing page fluff, heavy carousels, or multi-megabyte bundle sizes. Fast table-driven data presentation.
- **Time-Banking Ledger:** 1 hour of teaching = 1 Swap Credit earned. 1 hour of learning = 1 Swap Credit spent. Eliminates the strict requirement for direct reciprocal wants.
- **Credibility & Rating Engine:** A competitive programming-style rating rank derived from session completion, punctuality, and post-session peer reviews.

---

## 2. Competitive Rank System

Users earn tiers based on dynamic credibility scores and verified swap sessions:

| Rank Tier | Required Rating | Condition |
| :--- | :--- | :--- |
| **Newbie** | 0.0 - 2.9 | Baseline on profile creation (2 free starter credits). |
| **Pupil** | 3.0 - 3.7 | Minimum 2 successful swaps completed. |
| **Specialist** | 3.8 - 4.4 | Minimum 5 successful swaps; punctuality >= 80%. |
| **Expert** | 4.5 - 4.8 | Minimum 10 successful swaps; zero dispute penalties. |
| **Grandmaster**| 4.9 - 5.0 | Minimum 25 successful swaps; 95%+ recommendation rate. |

---

## 3. Quickstart

### Prerequisites
- Node.js >= 18.x
- MongoDB (Local instance or Atlas connection string)
- Firebase Project for Client Authentication

### Server Setup
```bash
cd server
npm install
cp .env.example .env
npm run dev
```

### Client Setup
```bash
cd client
npm install
npm run dev
```
