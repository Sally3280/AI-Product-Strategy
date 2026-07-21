# Three-Axis Vulnerability Diagnostic
## Product

<!-- Name the product you're diagnosing. Real product at your company — not a hypothetical. -->
**Chatbot** — BOG AI Assistant, a RAG-based Georgian/English banking chatbot

**Cluster Product Owner**

---

## Scores
### Contextual Moat — 3/5

*Workflow depth × switching cost. Would users leave in a weekend if a competitor showed up?*

**Score rationale:** The high switching cost comes from the bank itself — accounts, cards, salary deposits — not from the bot. The assistant is still "convenient support," not yet the reason a customer stays. Deeper hooks (real transactions, resolution, persistent threading) are what would raise this.

**Named attacker (from partner challenge):** A competitor bank's chatbot, or an Open Banking third party offering the same convenient support on top of the same account data.

---

### Data Advantage — 4/5

*Proprietary signal that compounds with usage. What do you see that OpenAI doesn't?*

**Score rationale:** A Georgian banking conversation corpus, a proprietary GE-EN embedding model, and a dialogue-quality rubric — signals a foundation model never sees. They compound with usage and feed product improvement, and a competitor can't recreate them quickly.

**Named attacker (from partner challenge):** Google/Gemini — as the base model's Georgian gets stronger, the raw-language edge of the embedding shrinks.

---

### Platform Exposure — 3/5

*Encroachment risk × pivot speed. If Apple/Google/OpenAI ships your hero feature native — then what?*

**Score rationale:** The whole stack sits on Gemini, so the roadmap is Google's to set, and pivoting inside a regulated bank is slow. The buffer is regulation, data residency, and account access that a platform can't ship natively.

**Named attacker (from partner challenge):** Google (native multilingual + banking agents), Apple (OS-level assistant), Open Banking agents.

---

## Top Vulnerability

<!-- One line: what's the single biggest strategic risk? -->
Your strongest moat (Georgian language/data) and your biggest platform risk share one root — "we understand Georgian better" is a wedge Google will eventually cover natively.

## Confidence Level

<!-- H / M / L — how confident are you in this bet after the diagnostic? -->
M — the logic holds, but the scores are assumptions; the real embedding accuracy vs. the latest Gemini, and actual switching behavior, would move them.
