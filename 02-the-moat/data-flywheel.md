# Data Flywheel Map

## Product
**BOG AI Assistant** - RAG-based Georgian/English banking chatbot, 1M conversations/month, FSR 74%

---

## Flywheel Loop Scores

| Loop | Input Signal | Output | Score (1–5) | Status | Notes |
|------|-------------|--------|-------------|--------|-------|
| **Query → Answer** | User query + RAG retrieval | Response quality | 3/5 | Active | Bot answers well on known intents; inconsistent on crypto/investment |
| **Resolution → Label** | User confirms Y/N at session end | FSR training signal | 2/5 | Partially active | Label collection exists but retraining pipeline not connected |
| **Escalation → Correction** | Human agent resolves escalated query | Correction pair (wrong → right) | 1/5 | Broken | 260K correction signals/month trapped in Genesys Cloud; never exported |
| **Session → Preference** | User behavior across sessions (reformulations, follow-ups) | Personalization signal | 1/5 | Missing | Bot is stateless; no cross-session memory; each conversation starts blank |

**Overall flywheel score: 7/20**

**Root cause of low score:** "Data goes in and gets thrown away." The resolution confirmation label exists but isn't connected to retraining. The correction corpus - the most valuable signal - sits in the contact center's system and has never been extracted.

---

## Attack Surface by Loop Weakness

**Most dangerous attack vector - memory and personalization:**
A competitor (or Google natively) that offers persistent context and learned preferences creates real switching costs that run *against* us. Three months of accumulated context from a competitor vs. our blank page at the start of every session = the moat reverses.

**Platform encroachment timeline:**
- 12–18 months: Gemini native Georgian capability reaches parity with our proprietary embedding
- At that point, our only remaining edge is the labeled resolution corpus and behavioral data - which we are currently not capturing

---

## Broken Loop: Fix Plan

**Priority 1 - Correction loop (90 days):**

| Step | Action | Owner | Timeline |
|------|--------|-------|----------|
| 1 | Build Genesys Cloud → data warehouse export (batch, weekly) | Engineering | Month 1 |
| 2 | Human agents tag resolution category on escalated tickets | Contact Center | Month 1 |
| 3 | Auto-format correction pairs as fine-tuning examples | AI team | Month 2 |
| 4 | Weekly batch upload to model improvement pipeline | AI team | Month 3 |

**Expected impact:** Correction loop score 1/5 → 3/5. Projected FSR lift: 74% → 80%+ within 2 quarters.

**Priority 2 - Preference loop (H2, requires +$3K/month experiment budget):**
- Implement persistent session threading (user ID → conversation history)
- Capture reformulation signals as negative training examples
- Expected impact: preference loop score 1/5 → 3/5; repeat-query rate -30%

---

## 90-Day Defense Priority

Fix the correction loop first. It requires no new model, no new budget, and no new data source - the data already exists in Genesys Cloud. The intervention:

1. Persistent threading to capture reformulations as training signals
2. Automated correction logging from escalations

This elevates two loops from 1/5 to ~3/5 within a quarter and begins converting the chatbot from a stateless answering machine into a compounding system.

**Strategic shift:** From "assistant that answers" → "assistant that does and remembers." An assistant that executes actions (H3) is not substitutable by a foundation model that only answers. That is the defensible end state.
