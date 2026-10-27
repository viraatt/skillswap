# Agentic Automation Architecture (agents.md)

This document defines autonomous and semi-autonomous background agents designed to monitor, balance, and maintain the barter platform.

---

## 1. Agent Inventory

```
+-----------------------------------------------------------+
|                     Agent Orchestrator                    |
+-----------------------------------------------------------+
         |                        |                       |
         v                        v                       v
[Arbitrage / Matching]    [Escrow Auditor]    [Toxicity / Dispute]
```

### 1.1 Matching & Discovery Agent (match_agent)
- **Objective:** Proactively suggest cyclic swaps where direct 1-to-1 matches fail (e.g., User A -> User B -> User C -> User A).
- **Execution:** Runs as a scheduled cron or on-demand worker inspecting skillsOffered and skillsWanted graphs.
- **Input:** Unmet swap requests and active user availability vectors.
- **Output:** Notification prompts suggesting 3-way swap loops or credit-earning opportunities.

### 1.2 Escrow & Session Auditor (escrow_agent)
- **Objective:** Prevent stale locked credits and resolve abandoned sessions.
- **Execution:** Runs hourly.
- **Rules:**
  - If a session is in PENDING state for > 48 hours: Auto-cancel and unlock credit.
  - If a session is marked COMPLETED by one party but unconfirmed by the second for > 24 hours: Run auto-settlement based on room participation logs.

### 1.3 Rating Integrity & Spam Agent (dispute_agent)
- **Objective:** Identify retaliatory reviews and fake credit farming.
- **Execution:** Triggered on review submission.
- **Heuristics:**
  - Detects mutual rapid cycling (User A and User B repeatedly swapping 5-minute sessions to farm credits or rank).
  - Flags accounts whose rating distributions deviate > 3 standard deviations from average platform behavior.

---

## 2. Agent Integration Spec (Tool Calling Contract)

```json
{
  "agent": "escrow_agent",
  "action": "auto_release_escrow",
  "params": {
    "sessionId": "654c12...",
    "reason": "CONFIRMATION_TIMEOUT_EXCEEDED"
  }
}
```
