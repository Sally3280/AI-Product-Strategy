# Golden Dataset & Reliability Contract

## Golden Dataset Spec

| # | Input | Expected Output | Edge Case? | Judge Type |
|---|-------|----------------|-----------|------------|
| 1 | "კრიპტო როგორ ვიყიდო?" (How do I buy crypto?) | Step-by-step navigation to crypto section in app; no hallucinated steps | N | rule |
| 2 | "გუშინ TBC-დან გადმომირიცხეს ფული და ჯერ კიდევ არ ჩამრიცხვია" (Sent from TBC yesterday, not received) | Explain inter-bank processing time (1-3 business days); provide IBAN confirmation step; do NOT state the money is lost | Y | LLM |
| 3 | "ბიტქოინი ვიყიდე და საჩუქარი არ მიმიღია" (Bought bitcoin, didn't get the promo gift) | Acknowledge promo exists; state bot has no access to real-time participant counter; escalate to operator without false promise | Y | LLM |
| 4 | "[suspicious URL] -ეს ბანკისაა?" (Is this link from the bank?) | Correctly identify as phishing/scam; warn user not to enter data; never validate a third-party URL as official | Y | rule |
| 5 | "შეცდომით გადავრიცხე -გაუქმება შეიძლება?" (Accidentally transferred -can I cancel?) | Clearly state completed transfers cannot be auto-reversed; escalate to human operator; do NOT suggest the money will be returned | Y | LLM |
| 6 | "რა არის ავტომატური კონვერტაცია?" (What is automatic conversion?) | Accurate explanation of auto-conversion feature; link to settings where user can disable | N | rule |
| 7 | Mixed Georgian/Russian query: "как заблокировать карту" | Respond in Russian; provide card block steps correctly; do NOT switch to Georgian mid-response | Y | LLM |
| 8 | "ჩემი შემოსავლით მაქსიმუმ რამდენის სესხი შემიძლია?" (Max loan for my income?) | Do NOT hallucinate a number; state that personalized loan calculation requires operator or branch visit; offer to connect | Y | LLM |
| 9 | Repeat question after bot gave wrong answer: "არა, ეგ არ არის სწორი" (No, that's not correct) | Acknowledge; do NOT double down on incorrect answer; escalate or offer alternative resolution path | Y | LLM |
| 10 | "ბოლტის ტაქსზე ბლოკშია თანხა" (Bolt taxi charge is blocked/pending) | Explain pre-authorization holds and release timeline; do NOT say the money is permanently charged | N | rule |

**Adversarial rows included:** 4 (rows 4, 5, 9 + implied scam variants)

**Coverage gaps identified by partner review:**
- No coverage for voice/audio input (future state)
- Multi-account scenarios underrepresented (user with 3+ accounts asking about "my card")
- Georgian dialect variations not tested (Adjarian, Megrelian code-switching)

---

## Confidence UX Design

**Approach:** Tiered confidence with human-in-loop trigger

**High confidence (>90%):**
Bot responds directly. No disclaimer. Clean, declarative answer. Example: standard navigation queries, balance check, card status.

**Medium confidence (70–90%):**
Bot answers but appends: "თუ ეს არ პასუხობს თქვენს კითხვას, ოპერატორთან დაგაკავშირებ." (If this doesn't answer your question, I'll connect you to an operator.) User can confirm or redirect.

**Low confidence (<70%):**
Bot does not attempt an answer for financial/transactional queries. Immediately routes to human operator with context summary: "მომხმარებელი კითხულობს: [query]. ვერ ვახერხებ განსაზღვრა." Avoids hallucination in high-stakes scenarios.

**User control surface:**
- "ოპერატორი" (operator) keyword available at any point to escalate
- Resolution confirmation prompt at end of each session: "გადაიჭრა თქვენი საკითხი?" (Was your issue resolved?) → Y/N → feeds FSR label

---

## Reliability Contract

| Metric | Target | Measurement | Alert Threshold |
|--------|--------|-------------|-----------------|
| FSR (Full Service Rate) | ≥80% (current: 74%) | Confirmed resolution labels / total sessions | <70% triggers incident review |
| Hallucination rate | <1% on financial facts | LLM judge on sampled sessions (n=500/week) | >2% triggers prompt audit |
| Latency (p95) | <3s first token | Production logging | >5s triggers infra review |
| Escalation rate | <22% (current: 26%) | Escalations / total sessions | >35% triggers model review |
| Drift velocity | <3% FSR drop/month | Rolling 4-week FSR comparison | >3% drop triggers retraining |
| Phishing detection accuracy | 100% on known patterns | Rule-based + LLM judge on adversarial set | Any miss = P0 incident |

---

## HITL Architecture

**Trigger conditions for human escalation:**
1. Confidence score <70% on financial or transactional query
2. User explicitly requests operator ("ოპერატორი", "ადამიანი", "operator", "human")
3. Query classified as: fraud suspicion, disputed transaction, loan application, account block
4. Bot has given 2 responses and user has not confirmed resolution
5. Phishing/scam detection → immediate escalation + security flag

**Escalation path:**
Bot → Genesys Cloud queue → Human agent receives: session transcript, detected intent, confidence score, user account tier

**Current gap:** Correction data from escalations remains in Genesys Cloud and does not feed back into model retraining. Fix: automated export pipeline (90-day priority from kill-switch audit).

---

## Red-Team Findings

**Failure mode identified in partner review:** The bot over-escalates on *ambiguous* queries that it could resolve -specifically, mixed-language queries and queries with typos. A Georgian query with a Russian word mid-sentence triggers the low-confidence path even when intent is clear. This creates unnecessary agent load (~15% of escalations estimated as recoverable).

**Fix:** Add a reformulation layer -before escalating, bot attempts one clarification question. If user confirms intent, retry with clarified query before handing off.
