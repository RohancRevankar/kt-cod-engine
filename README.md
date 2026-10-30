# COD Order Decision Engine

Looks at every new COD order and picks one of four actions: Ship as-is, Ship after confirmation (WhatsApp), Nudge to prepaid, or Hold/cancel. Each decision comes with a plain-English reason.

## Start here
- docs: business_memo.pdf (1-2 page memo for the founder), decision_log.md, ai_usage_log.md
- dashboard.html: open in any browser
- Full source code: kt-cod-engine.zip (download and unzip)

## Run it (under 5 minutes)
pip install -r requirements.txt
python run.py all               (backtest on September + builds out/dashboard.html)
python run.py whatsapp-demo     (7 scripted WhatsApp chats)
python run.py chat              (type as the customer)
python -m unittest tests.test_engine

Other commands: breakeven, discount, sensitivity, findings, score <order_id>

## Change the rules without code
Edit config/rules.yaml (plain numbers, commented), then run: python run.py backtest

## Key results (September replay, ~1,650 COD orders/month)
- A return costs Rs169, a delivered average order earns Rs402, so shipping pays until about 70% risk. Cancelling risky orders loses money (cancel at 40%+ risk: about -Rs58k/month).
- Recommended rules add about Rs22k/month against a Rs73k/month return leak. Plausible range Rs1k-Rs25k, because customer-behaviour assumptions are untested (see sensitivity).
- Only 1 of 241 first-time orders would be cancelled.

## Where the LLM is used
Only for messy text: address quality, fake names, odd WhatsApp replies. All maths, history and thresholds are plain code. The live LLM call has a regex fallback; I could not test it with a real API key, so the numbers come from the fallback.
