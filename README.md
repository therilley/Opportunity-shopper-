# Opportunity Shopper

Mobile-first, multi-platform, read-only opportunity research monitor. Kalshi is the first adapter; additional platforms will share a common analysis engine.

## Architecture
- core: normalized quotes, fee/edge math, independent probabilities, budget ledger, risk eligibility, historical observations.
- adapters/kalshi: public read-only market and orderbook ingestion.
- strategies/event_hold: binary outcome settlement ($1/$0), expected value, event and resolution schedule; no interim resale stop signals.
- strategies/event_trade: short-term buy/resell, spread, liquidity, slippage and round-trip fees.
- strategies/equities: future independent stock/ETF analysis.
- dashboard: Android-friendly cards, MIN/MAX entry window, event timestamps, dynamic green/yellow/red status.

## Safety
No order placement or automatic trading. Never claim displayed limits are enforced. Do not infer event start from trading close. No green alert without validated fresh quotes, verified fees and independent fair probability. Keep read-only API usage.

## Checkpoint
See [October 16 checkpoint](docs/CHECKPOINT-2026-10-16.md) for provisional $25 settings, risk parameters, complete requirements, and next steps. This repository currently preserves a design checkpoint, **not the latest runnable Colab notebook**; import and test that source next.
