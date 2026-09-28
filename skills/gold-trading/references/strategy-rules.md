# Gold (XAUUSD) Trading Strategy — Rules Reference

Campaign 1 baseline strategy. Reviewed and adjusted by the self-learning
loop in `data/strategy-weights.json` and logged in `data/journal.md`.

## Trend filter (Daily)
- EMA20_daily vs EMA50_daily (k = 2/21, 2/51) plus swing high/low structure
  determines BULLISH / BEARISH / RANGING.
- RANGING → no new trades that session.

## Entry setup (H1), all 5 required
1. Daily trend aligned with trade direction (LONG only in BULLISH, SHORT
   only in BEARISH).
2. Latest H1 close within 0.5% of a key swing level (support for LONG,
   resistance for SHORT), from swing highs/lows over the last 90 daily
   candles rounded to the nearest 50.
3. Rejection candle pattern on the last 1-3 H1 candles: Pin Bar (wick >
   2x body, pointing away from the level), Engulfing, or a 2-candle
   reversal.
4. RSI14_h1 < 40 for LONG (oversold) or > 60 for SHORT (overbought).
5. R:R >= 2.0 to TP1.

Also requires setup_type weight >= 0.65 in strategy-weights.json.

## Risk management
- SL: key_level ∓ 0.3 * ATR14_h1.
- TP1: 2R, TP2: 4R.
- On TP1: move SL to breakeven, let remainder run to TP2.
- Risk 1% of current balance per trade.
- Max 2 concurrent open trades. No new entries on weekends/low liquidity.

## Self-learning
- WIN: setup weight +0.03. LOSS: weight -0.08 + written lesson. BE: no
  change (capital preserved).
- 3 consecutive losses on a setup_type → weight reset to 0.10.
- Weights capped [0.10, 1.00].

## Campaign gate
- 1000+ trades closed and win rate >= 80% → freeze strategy into
  `data/STRATEGY.md` and `bot/GoldBot.mq5`.
- 1000+ trades and win rate < 80% → archive, increment campaign_number,
  reset trades/performance, redesign setup mix.
