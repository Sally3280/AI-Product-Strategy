# Compounding System Design

## Feedback Loops

| Loop | Input | Output | Compounds? | Status |
|------|-------|--------|-----------|--------|
| Resolution loop | User confirms Y/N at session end | FSR label per conversation → retraining signal | Y | active (label collection live; retraining pipeline missing) |
| Correction loop | Human agent resolution after escalation | Labeled correction pair (wrong bot answer → correct answer) | Y | broken -data trapped in Genesys Cloud, not exported |
| Reformulation loop | User rephrases same query (detected via session threading) | Intent disambiguation signal | Y | missing -bot is stateless; no session memory across turns |
| Preference loop | User behavior post-response (clicks, follow-ups, satisfaction score) | Personalized response ranking | N | missing -no behavioral signal captured |

**Broken loop identified by partner:** Correction loop. Every escalation generates a high-value correction pair -the exact case where the bot failed and how a human resolved it. This data sits in Genesys Cloud, never exported, never used. Estimated 260K correction signals/month going to waste.

**Fix plan (90-day):**
1. Build Genesys → data warehouse export (weekly batch, then real-time)
2. Human agents tag resolution category (resolved / partially resolved / policy block)
3. Correction pairs auto-formatted as fine-tuning examples
4. Weekly batch upload to model improvement pipeline

---

## Context Connectivity

**Current state (siloed):**
- Bot session data: isolated per conversation, no cross-session memory
- Escalation transcripts: Genesys Cloud only, not accessible to AI team
- Product analytics: separate BI system, not connected to bot performance
- Customer profile: CRM exists but bot has no read access to account history

**Target state (connected):**
- Persistent session threading: user ID → conversation history → context window on next session
- Cross-team signal flow: Bot performance metrics → Product squad weekly review → Model team sprint input
- CRM integration (H2): bot reads account tier, product holdings, last 3 interactions → personalizes response without asking user to repeat themselves

**Where knowledge silos today:**
The biggest silo is between the **contact center team** (who see failure modes daily) and the **AI/product team** (who only see aggregate FSR). Fix: weekly 30-min review of top 20 escalated transcripts, shared between teams.

---

## Governance Policy

**Scope:** BOG AI Assistant covers all self-service banking queries via mobile app chat. Explicitly excludes: investment advice, insurance product sales, mortgage origination, any query requiring regulatory sign-off.

**Autonomy boundaries:**
- Bot CAN: provide information, navigate user to correct app section, initiate standard card actions (block/unblock), confirm account details
- Bot CANNOT: execute fund transfers, approve loans, override fraud flags, make commitments on behalf of the bank, validate third-party URLs as official

**Escalation triggers (hard rules, not model decisions):**
- Suspected fraud → immediate human + security team flag
- Disputed transaction > 500 GEL → human required
- User distress signals (repeated "help", "urgent", "emergency") → human
- Any query about deceased account holders → human + legal flag

**Audit cadence:**
- Weekly: FSR trend review, top 20 escalation transcript review
- Monthly: Golden dataset re-evaluation (new adversarial cases added)
- Quarterly: Full model drift audit, hallucination rate sample review
- Annual: External AI audit (EU AI Act alignment check)

**Regulatory exposure:**
- NBG (National Bank of Georgia) guidance on automated financial services applies
- EU AI Act: High-risk AI system classification likely given financial services context -requires human oversight documentation, bias testing, and transparency disclosures
- GDPR equivalent (Georgian personal data law): conversation data retention policy required; user right to deletion must be honored

---

## Agent Topology

**Current architecture (single-agent):**
One Gemini-based agent handles all query types. No sub-agents. RAG retrieval is a tool call within the same session.

**Proposed topology (H2):**

```
User Query
    │
    ▼
Intent Classifier (Gemini Flash)
    │
    ├─► Simple / navigational → Direct Answer Agent (rule-based + RAG)
    ├─► Complex / financial → Reasoning Agent (Gemini Pro + CoT)
    ├─► Adversarial / scam → Security Agent (rule-based, no LLM)
    └─► Transactional (future H3) → Action Agent (requires banking API access)
```

**What each agent can do:**
- Direct Answer Agent: read-only, informational, navigation
- Reasoning Agent: multi-turn, clarification, complex policy interpretation
- Security Agent: deterministic rules only -never LLM-generated response for phishing/fraud
- Action Agent (H3): execute pre-approved transaction types with explicit user confirmation + 2FA

**Who approves what:** Any new agent capability requires: Product Owner sign-off, Security review, Legal/Compliance review, NBG notification if materially new functionality.

---

## Shadow AI Audit

| Tool | Owner | Risk Level | Decision |
|------|-------|-----------|----------|
| Gemini API (Google) | AI/Product team | H -single provider, no abstraction layer | Govern: implement abstraction layer (kill-switch audit finding); negotiate SLA |
| Genesys Cloud | Contact Center | M -houses correction data we're not using | Govern: build export pipeline; establish data sharing agreement |
| Internal vector DB (RAG index) | AI team | L -proprietary, controlled | Keep: core moat asset |
| Ad-hoc GPT-4 usage by content team | Marketing | M -untracked spend, unreviewed outputs | Govern: route through approved AI gateway; add to cost tracking |
| Lovable (prototype tool) | Product | L -prototype only, no production data | Keep for prototyping; enforce no-PII policy |

**Total tools found:** 5
**Tools after triage:** 5 (keep 3, govern 2, kill 0)
**Estimated hidden spend:** ~$200-400/month (ad-hoc GPT-4 usage by content team -untracked)
