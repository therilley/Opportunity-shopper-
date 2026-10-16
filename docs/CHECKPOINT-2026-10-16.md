# Opportunity Shopper checkpoint — 2026-10-16

## Proven vs proposed
User reported successful Kalshi public API connection, market titles/prices, and an order-book/liquidity screening run in Google Colab. The complete latest notebook source and runtime outputs have **not** been copied or independently verified in this GitHub repository. This document preserves requirements, not a validated production release.

## User workflow
Android Note 8; beginner-friendly single copy/paste Colab cells; mobile compact cards. Shared analysis core with platform-specific adapters and dashboards. Start with Kalshi; add other platforms after a reliable Kalshi baseline. Product concept: 'shopping insurance' for comparing opportunities, potentially commercialized later (subject to compliance and branding review).

## Working configuration (provisional)
- Starting bankroll: USD 25.00, user-editable with dated budget ledger and historical snapshots unaffected by subsequent edits.
- Per-market max exposure: USD 2.50; daily loss cap: USD 1.25. **Not yet programmatically enforced.**
- Maximum YES bid/ask spread: USD 0.03 (3 cents); compare consistent decimal-dollar units.
- Target estimated net profit: USD 0.05 per contract.
- Fee estimate: 7% coefficient, provisional. Earlier prototype formula: `ceil(0.07 * contracts * p * (1-p) * 100 - 1e-9) / 100` dollars, with `p = price_cents / 100`. Reconcile against current official fee schedule, fee rounding, market type, and entry/exit/settlement fees before any signals.
- Refresh desired: ~30 seconds when running; `AUTO_REFRESH=False` until explicit activation. Never imply background monitoring while Colab is disconnected.
- `FAIR_PROBABILITIES={}` by default. Never invent fair odds or mark an opportunity green without a defensible independently researched estimate.
- Public Kalshi API base: `https://external-api.kalshi.com/trade-api/v2`; previously tested `/markets?status=open` and `/markets/{ticker}/orderbook?depth=10`. Later scans used `limit=1000`, `mve_filter=exclude`. Verify endpoint/schema changes.

## Signal logic
- GREEN: fresh coherent bid/ask and verified depth, spread <= 3 cents, researched probability present, eligible strategy, feasible minimum/maximum entry-price window, expected net >= 5 cents, and risk constraints checked. Blink only while all checks remain true; stop blinking immediately when invalid/stale.
- YELLOW: insufficient research, waiting for a better price, uncertain event schedule, or otherwise incomplete.
- RED: missing/invalid quotes, wide spread, nonpositive net edge, outside event/trading window, or violated risk constraints.
- Show fee-aware MIN BUY / MAX BUY only when mathematically meaningful; a low price by itself is not proof of value. Show quote timestamp, liquidity and signal reason.

## Strategy-specific behavior
**Hold Until Result:** event outcome contract; model YES/NO $1/$0 settlement, probability-weighted expected value, fees, potential total loss, event start/end and resolution timing. Do **not** generate interim stop-loss/take-profit resale advice for this strategy. Track settlement outcome and status.

**Trade Before Result:** entry/exit prices, executable book depth, round-trip fees, spread, slippage, liquidity, estimated resale edge and time remaining before event start/market close. Interim resale targets apply only here.

**Equities/ETFs (future):** independent asset-price analysis, holding period, brokerage rules and distinct risk vocabulary; do not use binary event payout logic.

Record actual event date/start time **with timezone** independently from market close and settlement. If unavailable, mark `unknown`; never infer start from later trading close. Pre-event opportunity validity must use event start.

## Safety and operational constraints
Read-only, public-data-first. No order placement, auto-trading, credential collection, account balance changes or brokerage action. Treat 7% fee and fair probabilities as assumptions until verified. Respect API rate limits; retries/backoff, caching and stale-quote detection. Hard risk controls must be actually enforced in the decision engine before claiming enforcement; currently prior prototype only displayed daily limit. $25 bankroll makes fees, spread, liquidity and diversification particularly important. No guaranteed returns.

## Next steps
1. Preserve a dated copy of the actual Colab notebook; inspect existing cells and outputs; commit reviewed source to `notebooks/` or `src/` without tokens/secrets.
2. Verify official Kalshi market/order-book schema, bid/ask derivation, decimal units, volume, and latest fees/rounding.
3. Implement normalized market records with event start timezone, event end, trading close, resolution and explicit unknowns.
4. Separate hold-to-settlement and trade-before-result calculators; write tests for binary payouts, fee rounding, empty books, stale quotes and event schedule.
5. Enforce position cap, daily loss cap, exposure budget and safe refresh as *read-only alert eligibility* (no orders).
6. Build mobile UI: strategy toggle, budget edit/ledger, MIN/MAX entry window, reasons, green alert that ceases immediately on invalidation, historical snapshots.
7. Test against live read-only public data and simulated edge cases; compare with official platform quotes/fees before presenting actionable alerts.
8. Only then consider other platform adapters, multi-dashboard productization, legal/compliance review, and deployment.

## Continuity
Latest known notebook was previously shared in chat. GitHub checkpoint is a **specification backup**, not a backup of the notebook code. Future changes should be committed with a version/date and test notes.
