---
name: agent-introspection-debugging
description: Structured self-debugging workflow for AI agent failures using capture, diagnosis, contained recovery, and introspection reports.
---

# 🕵️ Agent Introspection & Failure Injection Debugging Skill

This skill governs structured self-debugging and failure injection testing for AI agents and distributed systems.

---

## 🛠️ Failure Injection Protocol

1. **Synthetic Failure Scenarios:**
   - **Scenario A (Database Timeout):** Simulate PostgreSQL / Redis connection drop. Verify fallback memory cache triggers cleanly.
   - **Scenario B (Rate Limit 429):** Inject simulated LLM HTTP 429 response. Verify exponential backoff retry logic.
   - **Scenario C (Missing Credentials):** Run without API keys. Verify graceful failure log and administrator alert dispatch.

2. **System Recovery & Verification Map:**
   - Document **Verified Components** (tested empirically under failure assertions).
   - Document **Unverified Gaps** (edge cases requiring additional test coverage).
