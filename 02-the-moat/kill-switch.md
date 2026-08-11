# Kill Switch Audit

## Product
**BOG AI Assistant** -entire inference stack on Google Gemini (1.5 Pro). No abstraction layer. No validated fallback provider.

---

## Vendor Dependency Assessment

| Dimension | Current State | Risk Level | Portability |
|-----------|--------------|------------|-------------|
| **Provider concentration** | 100% on Google Gemini -single provider, single model family | HIGH | None -no alternative credentials provisioned |
| **Code abstraction** | Direct Gemini API calls throughout app code. No `llm.generate()` interface -provider is hard-coded, not a config value | HIGH | Cannot switch without code changes across multiple modules |
| **Traffic routing** | No routing layer -all queries go to one model regardless of complexity or cost | HIGH | No ability to split traffic or failover |
| **Data portability** | Georgian conversation corpus and embedding model stored in GCP infrastructure | MEDIUM | Data is ours; re-embedding on another provider would take weeks |
| **Evaluation portability** | Golden dataset is provider-agnostic (inputs/outputs only) | LOW | Can re-run evals on any provider within days |

**Overall vendor lock-in severity: HIGH**
If Google doubles API pricing or deprecates Gemini 1.5 Pro → we negotiate from weakness, not strength. Current architecture prevents rapid switching.

---

## 48-Hour Action Items

| Action | Owner | Why it matters |
|--------|-------|---------------|
| Provision backup API credentials for 1 alternative provider (Anthropic Claude or Azure OpenAI) | Engineering lead | Creates optionality without committing to a switch |
| Wrap all Gemini SDK calls behind an internal `llm.generate(query, model=config.MODEL)` interface | Backend engineer | Decouples provider from business logic; one config change = full switch |
| Build minimal routing layer: `simple_intent → Flash`, `complex_intent → Pro` | AI team | Reduces cost 20% and enables cascade architecture |
| Freeze 100-example Georgian banking eval dataset | AI/QA team | Cross-provider evaluation baseline -measures quality parity before any switch |

---

## Alternative Provider Comparison

| Provider | Georgian language quality | Banking domain | Latency | Price (vs Gemini) | Switching effort |
|----------|--------------------------|---------------|---------|------------------|-----------------|
| **Google Gemini 1.5 Pro** *(current)* | Strong | Good via RAG | Fast | Baseline ($9K/month) | -|
| **Anthropic Claude Sonnet** | Good | Good via RAG | Comparable | ~10–15% higher | Medium -once abstraction layer is built |
| **Azure OpenAI (GPT-4o)** | Good | Strong | Comparable | ~5–10% higher | Medium -requires Azure account + data residency check |
| **Mistral (self-hosted)** | Weak on Georgian | Requires fine-tuning | Variable | Potentially lower | High -infra overhead, Georgian quality unvalidated |

**Recommendation:** Anthropic Claude as primary fallback. Similar quality profile, API-compatible enough that abstraction layer makes switching a config change, not a rewrite. Validate on 100-example eval dataset before finalizing.

---

## What's Defensible vs. Vulnerable

**Defensible -provider cannot take it from us:**
- Proprietary Georgian banking conversation corpus (our data, our cloud storage)
- Outcome-labeled resolution pairs (FSR labels, correction data)
- Customer behavioral patterns and account history
- Banking license and regulatory positioning
- The bank relationship itself

**Vulnerable -erodes as foundation models improve:**
- Raw Georgian language understanding advantage (Gemini's Georgian gets better natively)
- Embedding model edge (foundation models will close this gap in 12–18 months)
- Any feature that is just "prompt + Gemini" with no proprietary data behind it

**Strategic implication:** The abstraction layer is not just a resilience measure -it is the prerequisite for negotiating leverage. A vendor who knows you can't leave sets the price. A vendor who knows you have a tested fallback does not.

---

## Business Resilience Scenarios

| Scenario | Without abstraction layer | With abstraction layer |
|----------|--------------------------|----------------------|
| Gemini pricing doubles (+$9K/month) | Absorb cost or negotiate from weakness; no fallback | Negotiate from strength; switch 20% traffic to fallback as signal |
| Gemini deprecates 1.5 Pro | Emergency rewrite -weeks of downtime risk | Config change -hours to switch |
| Google launches competing banking product | Conflict of interest; Google controls our roadmap | Same risk, but data moat is clearer; can migrate stack |
| Georgian quality regression in new Gemini version | Discovered in production; no quick rollback | Detected on eval dataset before rollout; fallback ready |

---

## 30-Day Implementation Plan

**Week 1:** Provision Anthropic credentials. Freeze Georgian eval dataset (100 examples from golden dataset).

**Week 2:** Build `llm.generate()` abstraction wrapper. Migrate 3 highest-traffic query types to use wrapper (no behavior change -same provider, new interface).

**Week 3:** Run full eval dataset on both Gemini and Claude. Compare FSR-equivalent scores. Document quality delta.

**Week 4:** Build minimal routing layer (Flash for simple intents, Pro for complex). Test in staging. Deploy cascade to production.

**Output:** Full provider portability achieved. Cost reduced ~20% from cascade. Negotiation leverage with Google restored.
