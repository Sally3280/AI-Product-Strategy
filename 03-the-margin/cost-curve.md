# Cost Curve & Pricing Strategy

## Cost Model

| Cost Category | Per-Conversation | Per-Month (1M convos) | Notes |
|--------------|-----------------|----------------------|-------|
| Inference -Gemini (primary) | $0.009 | $9,000 | Full production load, all query types |
| Inference -triage/classifier | $0.0005 | $500 | Gemini Flash for intent routing |
| Infrastructure (vector DB, hosting) | $0.001 | $1,000 | RAG retrieval layer, logging |
| Human-in-the-loop (escalation cost) | $1.25 | $325,000 | 26% escalation rate × 5 min agent time @ $15/hr |
| **Total AI COGS (model only)** | **$0.0105** | **$10,500** | Excluding human agent costs |
| **Total w/ escalation** | **$0.335** | **$335,500** | Full cost per conversation including escalated share |

**Key insight:** The bot resolves 74% of 1M conversations without human involvement. Equivalent human cost for 740K conversations = ~$925,000/month. Net AI savings vs. full human support: **~$914,500/month**. Current AI spend is 1% of equivalent agent cost.

---

## Cascading Strategy

**Triage model:** Gemini Flash (or equivalent lightweight classifier)
**Frontier model:** Gemini 1.5 Pro (primary RAG + generation)
**Routing rule:** Simple intent categories (balance check, card block, transport discount) → Flash. Complex, multi-turn, ambiguous, or financial advice queries → Pro.
**Expected cascade ratio:** 40% Flash / 60% Pro (estimated; requires instrumentation to confirm)
**Projected savings from cascade:** ~$1,800–2,500/month on inference once routing is live

---

## Pricing Model

**Current model:** Internal cost center -chatbot is funded as a support infrastructure cost, not a revenue line. No per-user charge to customers.

**Proposed AI pricing framing for board:**
- Frame as cost-per-resolved-conversation: **$0.0142** (total AI COGS / 740K resolved)
- Benchmark: human agent cost per resolved contact = **$1.25** minimum
- AI cost is **88× cheaper** per resolution

**Model:** Cost center → outcome-based accountability (track cost-per-FSR, not cost-per-call)

---

## Stress Tests

| Scenario | Impact on Margin | Response |
|----------|-----------------|----------|
| Gemini inference costs 3× ($27K/month) | AI COGS rises to $28.5K -still <3% of equivalent human cost | Trigger cascade routing; negotiate volume discount; activate abstraction layer for provider switch |
| Heaviest segment (crypto/investment) doubles volume | +200K complex queries → +$1,800 inference; escalation rate may rise | Add dedicated classifier for crypto/investment; route to specialized RAG index |
| Google raises API prices 50% ($13.5K/month) | +$4,500/month | Leverage abstraction layer (from kill-switch audit); negotiate 12-month lock-in; evaluate Azure OpenAI or Anthropic fallback |
| FSR drops from 74% to 60% | Escalation volume rises by 140K → +$175K/month human cost | Trigger red-alert: activate live correction pipeline immediately; review drift metrics |

---

## Board One-Pager: Before vs. After

**Before (human-only support):**
- ~1M customer contacts/month handled by contact center agents
- Average handle time: 5 min @ $15/hr = **$1.25/contact**
- Total support cost estimate: **~$1.25M/month**
- No compounding -each call costs the same in year 5 as year 1

**After (AI-first with human escalation):**
- 740K contacts resolved by bot @ **$0.0142/resolution**
- 260K escalated to humans @ $1.25 = $325K
- Total: **$335.5K/month** (including all AI infrastructure)
- Compounding: each labeled resolution improves future FSR -cost per resolution falls over time

**Net margin shift:** -$914,500/month in support cost (73% reduction)
**Additional investment ask:** +$3,000/month for experiment infrastructure (judges, live corrections, dynamic content generation) → projected FSR improvement to 80%+ within 2 quarters
