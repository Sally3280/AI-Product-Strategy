# Three-Horizon Roadmap & Board Pitch

## Roadmap

### Horizon 1 -Now (0–3 months)
*Quick wins. Ship with existing capabilities and $9K/month budget.*

| Initiative | Metric | Confidence |
|-----------|--------|-----------|
| Fix bot inconsistency on crypto/investment queries -standardize RAG responses, eliminate URL-only deflections | FSR on crypto/investment queries: 40% → 65% | H |
| Implement session-end resolution confirmation ("Was your issue resolved?" Y/N) | Resolution label coverage: 0% → 90% of sessions | H |
| Build Genesys Cloud → data warehouse export pipeline for escalation correction data | Correction pairs captured/month: 0 → 260K | M |
| Android parity fix -resolve deep-link and rendering issues causing 73% churn in treatment arm | Android session completion rate parity with iOS | H |

---

### Horizon 2 -Next (3–9 months)
*The board ask. Requires +$3,000/month in experiment infrastructure.*

| Initiative | Metric | Confidence |
|-----------|--------|-----------|
| **LLM-as-judge expansion** -deploy 3 automated judges (factual accuracy, tone, resolution quality) running on sampled sessions weekly; replace manual QA sampling | Hallucination detection coverage: 5% → 80% of sessions | M |
| **Live corrections pipeline** -human agent corrections auto-formatted and batch-uploaded to fine-tuning; weekly model improvement cycle | FSR lift: 74% → 80%+ within 2 quarters | M |
| **Dynamic content generation** -bot generates personalized response variants based on user account tier, product holdings, and conversation history; A/B tested against static RAG | Engagement rate on personalized vs. static response | M |
| Persistent session threading -user context carried across sessions; no repeat-yourself friction | Repeat-query rate: -30% | M |
| Intent classifier cascade -route simple queries to Gemini Flash, complex to Pro | Inference cost reduction: -20% ($1,800/month saved) | H |

**Budget breakdown for +$3K/month:**
- LLM judge compute (3 judges × sampled sessions): ~$800/month
- Fine-tuning pipeline infrastructure: ~$700/month
- Dynamic content A/B testing infra: ~$600/month
- Experiment logging and evaluation tooling: ~$900/month

---

### Horizon 3 -Bet (9–18 months)
*Moonshots. High uncertainty, high potential. Requires separate investment decision.*

| Initiative | Metric | Confidence |
|-----------|--------|-----------|
| Transactional capability -bot executes pre-approved actions (standard transfers, card block/unblock, standing order setup) with 2FA confirmation | Self-service transaction rate; reduction in branch/call volume for routine actions | L |
| Proactive financial nudges -bot initiates outbound messages based on behavioral signals (unusual spend, goal progress, upcoming payment) | Nudge open rate; action rate post-nudge | L |
| Cross-product personalization -bot reads full product holding profile and proactively surfaces relevant offers (deposit rates, loan pre-approval) | Conversion rate on bot-surfaced offers | L |

---

## Board Pitch

**Thesis (1 sentence):**
BOG's chatbot moat is not Georgian language -it is the outcome-labeled resolution loop that compounds with every conversation, and the window to build it before Google closes the language gap natively is 12–18 months.

**The case:**

1. **Why now:**
Georgian-language capability in frontier models is improving fast. Our current edge -better Georgian understanding than a base model -has an 18-month shelf life at most. The durable advantage is behavioral data and labeled resolutions that a foundation model cannot acquire without our conversations. Every month we delay the correction pipeline, we waste 260,000 training signals.

2. **What's defensible:**
The bank relationship, the banking license, and the labeled resolution corpus. A third party can build a better Georgian chatbot. They cannot get a banking license, access to account data, or the 12M+ labeled conversation turns we will have in 18 months if we execute.

3. **The economics:**
Current AI spend: $9,000/month. Equivalent human support cost for the same 1M conversations: ~$1.25M/month. We are delivering 74% first-contact resolution at 1% of human agent cost. The proposed +$3,000/month gets us to 80%+ FSR and a self-improving system -the additional $3K is not an expense, it is the mechanism that makes the $9K compound.

**The risks:**

1. **Trust / failure modes:**
The bot hallucinating financial information (wrong transfer amount, incorrect policy) is a P0 risk. Mitigated by: low-confidence → human routing, adversarial golden dataset, phishing detection rule layer, no LLM generation for security-critical paths.

2. **Scale / governance:**
EU AI Act high-risk classification likely. NBG oversight required. Mitigated by: HITL architecture documented, audit cadence established, human-override available at any point in every session.

3. **Competitive:**
Google launches native Georgian banking agent. Mitigated by: banking license moat, account data residency requirements, and the resolution corpus -even if Google speaks better Georgian, they cannot access BOG customer behavioral data.

**The ask:**
Approve +$3,000/month in experiment infrastructure for Horizon 2 (LLM judges, live corrections, dynamic content). Total AI budget: $12,000/month. Target outcome: FSR 80%+ within 6 months, self-improving correction loop live within 3 months. Review checkpoint at month 3 with FSR data.

---

## M1 Baseline vs. Now

**M1 baseline (the bet as stated):**
For BOG retail banking customers, an assistant whose moat is an outcome-labeled resolution loop on our own flows -not raw Georgian language -because that's the one advantage a foundation model can't ship natively.

**Now (after full strategy build):**
The bet holds, and the path is clearer. The resolution loop is real but currently broken -correction data from 260K escalations/month sits unused in Genesys Cloud. The moat is being wasted. The 3-month priority is not building new features; it is closing the correction loop that already exists. The $3K/month ask is the cost of turning a stateless chatbot into a compounding system. The language edge buys us 12–18 months. The loop is what survives beyond that.
