# My AI Product Strategy

> A living strategy built across 6 sessions. Each module adds one component. By Module 6, this repo IS your strategy — version-controlled, board-ready, portable.

---

## Strategy at a Glance

| Component | Module | Status | Key Artifact |
|-----------|--------|--------|-------------|
| **The Bet** | M1 | [x] | `01-the-bet/` |
| **The Moat** | M2 | [x] | `02-the-moat/` |
| **The Margin** | M3 | [x] | `03-the-margin/` |
| **The Contract** | M4 | [x] | `04-the-contract/` |
| **The Guardrails** | M5 | [x] | `05-the-guardrails/` |
| **The Pitch** | M6 | [x] | `06-the-pitch/` |

---

## The Bet (M1)

**What we're building, for whom, why now.**

- **Product:** BOG AI Assistant — RAG-based Georgian/English banking chatbot, 1M conversations/month
- **AI Value Archetype:** Automator — resolves customer requests end-to-end, not just assists human agents
- **Vulnerability Scores:** Moat 3/5 · Data 4/5 · Platform 3/5
- **Top Risk:** Georgian language advantage erodes as frontier models improve natively — window is 12–18 months
- **Confidence:** M — logic holds; real embedding accuracy vs. latest Gemini and actual switching behavior would move scores
- **Prototype:** [Resolution-loop chat prototype](https://id-preview--489aed82-a913-4b43-b1ad-88580f813fb5.lovable.app/) (built in Lovable)
- **Kill Criteria:** Kill if FSR is flat vs. non-looped baseline after a few weeks, or if a base-model upgrade closes the quality gap without the loop

→ Details: [`01-the-bet/`](01-the-bet/)

---

## The Moat (M2)

**Why this won't get copied in 6 months.**

- **Data Flywheel Score:** 7/20 (correction loop broken; preference loop missing)
- **Weakest Loop:** Escalation → Correction (1/5) — 260K correction signals/month trapped in Genesys Cloud, never used
- **Competitive Position:** Strong on proprietary Georgian corpus and labeled resolutions; weak on memory and personalization — stateless bot is substitutable
- **Encroachment Defense:** Banking license, account data residency, outcome-labeled resolution corpus — a foundation model can't acquire these without our conversations
- **Vendor Portability:** Locked — direct Gemini API calls, no abstraction layer; 30-day fix plan in progress

→ Details: [`02-the-moat/`](02-the-moat/)

---

## The Margin (M3)

**Will this make money or bleed it?**

- **Gross Margin (current):** AI cost $0.0105/conversation vs. equivalent human cost $1.25/conversation — 88× cheaper per resolution
- **Gross Margin (AI-adjusted):** $335.5K/month total (AI + escalations) vs. ~$1.25M/month full human — 73% cost reduction
- **Pricing Model:** Internal cost center; framed as cost-per-resolved-conversation ($0.0142) vs. human benchmark ($1.25)
- **Cascading Strategy:** Gemini Flash (40% simple queries) → Gemini Pro (60% complex); projected -20% inference cost (-$1,800/month)
- **Break-even at:** Already positive — $9K AI spend saves ~$914K/month in equivalent agent cost

→ Details: [`03-the-margin/`](03-the-margin/)

---

## The Contract (M4)

**Why users will trust a probabilistic system.**

- **Reliability Target:** FSR ≥80% (current: 74%), hallucination rate <1%, latency p95 <3s
- **Golden Dataset:** 10 rows, 4 adversarial (phishing detection, accidental transfer, incorrect answer recovery, mixed-language)
- **Confidence UX:** Tiered — >90% direct answer / 70–90% answer + escalation offer / <70% immediate human routing, no LLM generation
- **HITL Architecture:** 5 hard escalation triggers; bot → Genesys Cloud queue → agent receives full session context
- **Failure Mode Coverage:** Hallucination on financial facts (P0), phishing misclassification (P0), over-escalation on mixed-language queries (15% recoverable)

→ Details: [`04-the-contract/`](04-the-contract/)

---

## The Guardrails (M5)

**What breaks when this scales — and what compounds.**

- **Compounding System:** 4 feedback loops; 2 active (partially), 1 broken (correction), 1 missing (preference) — fix plan: Genesys export pipeline in 90 days
- **Governance Posture:** Hard autonomy boundaries (inform only, no fund transfers); 5 escalation triggers; weekly transcript review + quarterly external audit
- **Shadow AI Status:** 5 tools found, 5 triaged (keep 3, govern 2, kill 0); ~$200–400/month hidden spend identified
- **Agent Boundaries:** Single-agent now; H2 topology: Flash classifier → Direct Answer / Reasoning / Security agents; H3 adds Action agent with 2FA
- **Regulatory Exposure:** EU AI Act high-risk classification likely; NBG oversight required; Georgian personal data law retention policy needed

→ Details: [`05-the-guardrails/`](05-the-guardrails/)

---

## The Pitch (M6)

**How you get this funded, shipped, and adopted.**

- **Horizon 1 (Now):** Fix crypto/investment bot inconsistency; implement resolution labeling; build Genesys export pipeline; Android parity fix
- **Horizon 2 (Next):** +$3K/month for LLM judges, live corrections pipeline, dynamic content generation, persistent session threading
- **Horizon 3 (Bet):** Transactional capability, proactive financial nudges, cross-product personalization
- **Board Narrative:** BOG's chatbot moat is not Georgian language — it is the outcome-labeled resolution loop that compounds with every conversation, and the window to build it before Google closes the language gap natively is 12–18 months
- **Key Metric:** FSR — current 74%, target 80%+ within 6 months; total budget ask $12K/month (+$3K from current $9K)

→ Details: [`06-the-pitch/`](06-the-pitch/)
