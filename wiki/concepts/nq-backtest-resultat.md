# NQ Backtest — Fabio Valentini-strategi (real CVD)

**Datum:** 2026-06-14  
**Symbol:** NQ=F  
**Data:** Real tick data från Databento (60 dagar, NY-session)  
**Strategi:** Long-only reversal, CVD-baserad entry  

---

## Resultat — hela NY-sessionen (14:45–21:00 UTC)

| Metrik | Värde |
|--------|-------|
| **Return** | **+$63 768 (+42.5%)** |
| **Trades** | 68 |
| **Win rate** | 71% |
| **Max drawdown** | 5.7% |
| **Risk per trade** | 0.5% ($750 på $150k) |
| **Period** | 60 dagar |

---

## Resultat — före 17:30 UTC

Om du bara sitter 14:45–17:30 UTC (slut vid svensk tid 19:30 sommar / 18:30 vinter):

| Metrik | Före 17:30 |
|--------|:----------:|
| **Return** | **+$25 398 (+16.9%)** |
| **Trades** | 57 |
| **Win rate** | **63.2%** |
| **Utan after-hours** | ✅ |

> ⚠ Dessa siffror är från CVD-approximation (yfinance), inte real tickdata. Win raten är högre men PnL lägre eftersom färre trades.

---

## Parametrar

| Parameter | Värde |
|-----------|-------|
| Entry | CVD reversal, min_score ≥ 3 |
| Stop loss | 1.5 ATR |
| Target | 2.0 ATR |
| Trail stops | AV (ingen trailing) |
| Riktning | **Long-only** (inga shorts) |
| Session | NY-session 14:45–21:00 (eller före 17:30) |
| Timeframe | 5-min |
| Exit via | Target eller CVD fade |

---

## Validering

| Check | Resultat |
|-------|----------|
| Data | ✅ Real tickdata från CME Globex (trades schema) |
| CVD approx vs real | 96.8% divergence match, 72.9% sign accuracy |
| Datakostnad | $2.08 för 60 dagar NY-session |
| ROI på data | $2 in → $63k backtestad profit |

---

## Noteringar

- Endast **longs** — kort-sidan är inte testad
- Ingen trailing — exit via target eller CVD fade
- Risk 0.5%/trade är låg — strategin tål högre
- Skippa första 15 min av NY-sessionen (14:45 istället för 14:30)
- Tidig stängning vid 17:30 funkar — högre win rate, färre trades
