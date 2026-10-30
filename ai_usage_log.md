# AI usage log

**Tools:** Claude (chat, with a code sandbox) for data exploration, writing the code, tests, memo and these docs. I reviewed outputs against the raw CSV and the brief.
*(Founder-facing note: edit this file so it reflects how YOU used AI; the examples below are what actually went wrong in this build.)*

## Where AI gave something wrong or weak, and how it was caught
1. **Nudge accounting produced nonsense.** First version reported "lost" = -Rs1,992 for the prepaid bucket (negative loss). Caught by reading the bucket table rather than just the total. Cause: sales rescued by prepaid were booked in the wrong column. Fix: split into saved (returns avoided + sales rescued), cost (discount, gateway, WhatsApp) and net; added a check that net equals the formula by hand (Rs649, 35% risk -> ~Rs22/order).
2. **Wrong volume scale.** First backtest scaled September to 3,000 orders/month, but the brief says 55% of 3,000 are COD, so the engine sees ~1,650. Every rupee figure was 1.8x too high. Caught when re-reading the brief against the "per month" line; moved volume into rules.yaml and re-ran everything.
3. **Wrong first story about Hinglish.** The initial heuristic assumed Hinglish landmark addresses were risky (30% vs 23%). Splitting by city showed it is mostly a small-city effect (21% vs 18% in big cities), so Hinglish is not penalised.
4. **Threshold search over-trusted my own assumptions.** The grid said "nudge from 20%". That answer is only as good as the made-up acceptance rate, so I chose a milder 25% and logged why, and added the sensitivity table.
5. **Data traps spotted by looking, not assuming:** duplicate rows (30), "Rto"/"delivered" spellings, an IN_TRANSIT row, 3 zero-value orders, avg order Rs920 vs the Rs649 stated.

## What I did NOT let the AI decide
Break-even logic, the LLM/code split, cancel bars, and the claim "cancelling is mostly wrong" - each verified by replaying September with known outcomes.
