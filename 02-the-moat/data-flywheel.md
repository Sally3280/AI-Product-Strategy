# Data Flywheel Map

> Score each loop 1-5. Your weakest loop is where competitors attack first.
> The four loops below are the M2 starting point - adapt if your product has 2 or 6 loops instead of 4.

## Flywheel Loops

| Loop | What It Measures | Score 1 | Score 5 | Score |
|------|------------------|---------|---------|-------|
| **Correction** | Do users fix AI outputs? Is that signal captured and reused? | No capture | Automated retraining | 1/5 |
| **Preference** | Does the product learn individual / team preferences over time? | Stateless | Deep personalization | 1/5 |
| **Domain Context** | Does usage in one area improve quality in adjacent areas? | Siloed | Cross-domain transfer | 2/5 |
| **Network** | Does each new user / team make the product better for everyone? | Isolated | Strong network effects | 3/5 |

### Correction Loop - 1/5

**What you capture today:** Full dialogue logs, session-level classification against a custom rubric (Resolved, Scoped Out, Deflected) partly via LLM-as-annotator, and FSR as a reporting metric. When a session escalates, the Genesys Cloud agent writes the correct answer — so human correction happens every day.

**How it compounds:** It doesn't. The agent's correct answer stays inside Genesys and returns to the knowledge base only through manual, sporadic review. User reformulation — the strongest implicit "that answer was wrong" signal — is not stored as a labelled pair at all. FSR is measured for reporting, not fed back as a training input. The loop is closed by hand, quarterly, not automatically.

### Preference Loop - 1/5

**What you capture today:** Almost nothing. Sessions are stateless; persistent conversation threading is still only in the planned UX redesign. Core banking data gives us the customer's product profile, but it does not shape assistant behaviour.

**How it compounds:** It doesn't. The hundredth session is identical to the first — same answer length, same tone, same clarifying questions. A user who has asked about card limits forty times still gets the full explanation. Smart Offers Search filters with an LLM but does not learn which offers a specific user consistently ignores.

### Domain Context Loop - 2/5

**What you capture today:** KB unification (chatbot KB plus Genesys Cloud KB) is creating a shared knowledge layer. The Georgian-English embedding model trained on one domain's text also helps adjacent domains. The lifestyle offers integration is the first real cross-domain link.

**How it compounds:** Weakly and in one direction only. Volume in credit questions does not improve answer quality in insurance or FX. Retrieval is shared; learning is not. Cross-domain transfer today happens at the architecture level (shared embedding), not at the data level (shared learning).

### Network Loop - 3/5

**What you capture today:** The strongest loop. Every new user adds Georgian-language banking dialogue to a corpus no one else has at our volume. Question frequency shapes KB priorities; new intents surface in trending questions before they exist in the KB.

**How it compounds:** It genuinely compounds, but at low frequency. The benefit is aggregate (everyone gets a better KB) rather than personal, and closing the loop still depends on a human. The network effect is real but defensible only while Georgian remains a language barrier.

**Total Flywheel Score: 7/20**

**Weakest Loop:** Correction and Preference, both 1/5. Readout: no flywheel — data goes in and gets thrown away.

**Fix for weakest loop:** One intervention that closes both — a Correction Capture Layer. (1) On escalation, automatically log `(question, bot_answer, human_answer, KB_gap)` as a structured record feeding a weekly KB triage queue. (2) Detect in-session reformulation and treat it as an implicit negative label, no manual review needed. (3) Use persistent threading, already on the roadmap, as the foundation of the preference loop: the thread ID becomes the first unit of memory. Low cost, moves both loops from 1/5 to roughly 3/5 within a quarter, no new model required.

---

## Encroachment Threat Assessment

### 1. Platform Encroachment

**Attacker:** Google / Gemini, with OpenAI as a second vector.

**Vector:** Our main technical moat — the proprietary Georgian embedding model — sits on the platform's roadmap. Once frontier models cover Georgian at native quality, eighteen months of work becomes a commodity API call. The platform doesn't come around us; it comes from underneath us.

**Time-to-threat:** 12-18 months

**% of value at risk:** ~40% — the language and retrieval layer. Core banking integration survives.

### 2. Vertical Competitor

**Attacker:** TBC, or any local bank that ships an agentic assistant before we do.

**Vector:** Not a better chatbot — a transactional agent. "Transfer this," "cancel that," "raise my limit," instead of "here are the instructions." If a competitor moves from informational to actionable assistant first, FSR comparison becomes meaningless: their user completes the task, ours reads about it.

**Time-to-threat:** 6-12 months

**% of value at risk:** ~50%

### 3. Adjacent Expansion

**Attacker:** CCaaS/CRM vendors (Genesys, Salesforce, Microsoft), or a neobank like Revolut/Wise expanding into the region.

**Vector:** Genesys already sits in our escalation layer and sees the correction data we aren't using. If it offers a native GenAI agent on an "already integrated" argument, the internal build becomes hard to justify — especially with a weak flywheel as the backdrop.

**Time-to-threat:** 12-24 months

**% of value at risk:** ~25%

---

## 90-Day Encroachment Plan

*Your partner played the Big Tech attacker. What was their plan to kill you?*

**Attacker:** Google / Gemini

**Attack vector (target the weakest loop):** Hit the preference loop. Their assistant is stateless — it remembers nothing. We build an assistant that remembers everything and give it away free, outside the bank.

**Weeks 1-4 - what they ship:** A native-quality Georgian language update plus personal financial context pulled from Gmail, Drive and uploaded statements. One question — "where is most of my money going" — and the answer is better than anything inside the banking app.

**Weeks 5-8 - how they poach users:** Not by migration, but by bypass. Users still open the banking app to transact, but they ask their questions somewhere else. MAU declines slowly while FSR stays healthy, because the hard questions stop arriving. We don't notice until it's late.

**Weeks 9-12 - why users don't come back:** Because the other assistant remembers. Three months of accumulated context versus our blank page at the start of every session. The switching cost is now real, and it runs against us.

**Your defense:**
1. Memory before intelligence — persistent threading plus a preference layer this quarter. It is the one thing the platform cannot build in our position: transactional truth and history in the same place.
2. Turn on correction capture now — make the agent's answer a training asset rather than a chat log. It is the only data Google does not have.
3. Move from information to action. An assistant that *does* is not substitutable by an assistant that *answers*; the banking licence and core integration are on our side of that line.
4. Fix the metric. Track questions asked per active user alongside MAU — its decline is the early signal of encroachment, and FSR hides it.
