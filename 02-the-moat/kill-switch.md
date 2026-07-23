# Kill Switch Audit

## Vendor Dependency Assessment

| Dimension | Current State | Risk Level | 48-Hour Action |
|-----------|--------------|------------|---------------|
| **Provider** |	Entire stack on Google Gemini. Single provider, single model family, no validated fallback. | H  |	Provision keys + quota on a backup provider (Anthropic or Azure/OpenAI) so it's reachable, even if unused.
| **Abstraction** |	Direct Gemini API calls throughout app code. No llm.generate() interface — provider is hard-coded, not a config value. | H |	Wrap every direct SDK call behind one internal interface. This is the precondition for everything else.
| **Routing** |	No routing layer. Every request goes straight to Gemini; can't shift traffic by config. | H | 	Stand up a minimal router (even hardcoded) that can flip to a fallback provider via flag.|
| **Eval** | Dialogue-quality rubric + labeling team exist, but no cross-provider golden eval set. | M |	Freeze ~100 Georgian banking Q&A pairs as a golden set to score any candidate provider apples-to-apples. |

## Portability Score
<!--Locked --> 
Locked. Provider, Abstraction, and Routing are all High — with direct API calls, switching off Gemini is a refactor, not a config change. Eval is the one relative strength (the labeling infra gives you a way to measure a switch), and it's what moves you toward "Partial" fastest once the abstraction layer exists.

## If Google doubles pricing tomorrow:
<!-- If Google (Gemini) doubles pricing tomorrow:-->

We can't cut over in 48 hours today, so the honest 48-hour response is absorb + negotiate, not switch: (1) pull current + projected spend into one number for the director; (2) escalate on commercials — a regulated Georgian bank is a reference logo Google wants, so there's more leverage than the price sheet implies; (3) if the abstraction existed, route high-volume/low-complexity intents to a cheaper model — but it doesn't yet, which is why the wrap-layer is the #1 action. The real breakage is structural: no abstraction layer is what turns a price shock into a crisis.

## If Google ships a competing product:
<!-- If Google (Gemini) ships a competing product (native Georgian banking assistant):-->

What's defensible — and it's not the model:

Proprietary resolution + banking-behavior data — what BOG's customers ask, in what order, and what actually resolves their issue. Gemini has the language; it doesn't have this.
Regulatory + data-residency moat — a regulated bank inside Georgian compliance boundaries; account access and settlement a platform can't ship natively.
The bank relationship itself — accounts, cards, salary. Google can ship a better bot; it can't ship being the bank.

What is not defensible: "we understand Georgian better" — that's the exact edge Gemini erodes. This is why the flywheel work (Corrections + Preferences) matters here too: it converts defensibility from language (rentable) into behavior + memory (owned).

