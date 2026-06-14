# NQ Backtest — Fabio Valentini-strategi (real CVD)

**Datum:** 2026-06-14  
**Symbol:** NQ=F  
**Data:** Real tick data från Databento (60 dagar, NY-session)  
**Strategi:** Long-only reversal, CVD-baserad entry  

---

## Resultat

| Metrik | Värde |
|--------|-------|
| **Return** | **+$63 768 (+42.5%)** |
| **Trades** | 68 |
| **Win rate** | 71% |
| **Max drawdown** | 5.7% |
| **Risk per trade** | 0.5% ($750 på $150k) |
| **Period** | 60 dagar |

---

## Parametrar

| Parameter | Värde |
|-----------|-------|
| Entry | CVD reversal, min_score ≥ 3 |
| Stop loss | 1.5 ATR |
| Target | 2.0 ATR |
| Trail stops | AV (ingen trailing) |
| Riktning | Long-only |
| Session | NY-session 14:45–21:00 UTC |
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

- Ingen trailing använd — alla exits via target (2.0 ATR) eller CVD fade-signal
- Risk 0.5% per trade är låg — strategin tål högre risk (testat upp till 2.5% med +61k på 60d)
- Strategin presterar bäst i NY-session, skippa första 15 min
- Trendfilter (kort när daglig trend är ner) används inte för long-only
