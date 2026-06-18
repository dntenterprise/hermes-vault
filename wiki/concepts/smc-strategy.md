# SMC Strategy — AMD Liquidity + MSS + VP Breakout

**Baserat på:** ICT/SMC — AMD (Asia-Manipulation-Distribution), Liquidity sweeps, MSS, SMT
**Instrument:** NQ=F (Nasdaq-100 futures)
**Data:** yfinance 5-min (97.8% vs Databendo enligt tidigare validering)

---

## Backtest-parametrar

| Parameter | Värde |
|-----------|-------|
| Period | 2026-04-17 → 2026-06-15 (59 dagar) |
| Timeframe | 5-min |
| Kapital | $150 000 |
| Risk/trade | 0.5% ($750) eller 1% ($1 500) |
| Stop | ATR × 1.5 |
| Target | Nästa swing hög/lå (likviditetszon) |
| MSS-lookback | 12 bars (~1 timme) |
| Swing-span | 2 bars var sida |
| Trend | SMA20 vs SMA50 (dagligen) |
| Session | Endast NY (14:30-21:00 UTC) |

## Entry-logik — Long

KRAV:
- NY session (09:30-16:00 ET / 14:30-21:00 UTC)
- Upptrend dagligen (SMA20 > SMA50)
- Discount-zon (pris under VAL från föregående dags VP)

Trigger (minst 1):
1. **MSS efter SSL sweep** — Pris sweeper under senaste swing low, stänger ovan, bryter structure upp
2. **VP breakout** — Pris bryter över VAH med volym × 1.3+
3. **MSS + SMT** (A+++) — MSS med SMT divergence mellan NQ och ES

Bias:
- **London bearish** (pris sjönk 03:00-07:00 ET) → NY bias bullish

## Entry-logik — Short

KRAV:
- NY session
- Nedtrend dagligen (SMA20 < SMA50)
- Premium-zon (pris över VAH)

Trigger (minst 1):
1. **MSS efter BSL sweep**
2. **VP breakout short**
3. **MSS + SMT**

Bias:
- **London bullish** → NY bias bearish

## Resultat ($750 risk)

| Metrik | Värde |
|--------|-------|
| **Trades** | 30 |
| **Win rate** | **76.7%** |
| **Total PnL** | **+$6 882 (4.59%)** |
| **Profit factor** | **2.39** |
| **Max DD** | ~$1 929 (1.3%) |
| **Per dag** | +$146 |

### Exitfördelning

| Exit | Trades | Wins | PnL |
|------|--------|------|-----|
| TARGET | 25 | 23 (92%) | **+$10 193** |
| STOP | 4 | 0 | -$3 000 |
| EOD | 1 | 0 | -$311 |

### Riktning

| Riktning | Trades | Wins | PnL |
|----------|--------|------|-----|
| Long | 29 | 22 | **+$6 455** |
| Short | 1 | 1 | +$427 |

### Entry-typ

| Typ | Trades | Wins | PnL |
|-----|--------|------|-----|
| Bias (MSS/sweep/breakout) | 28 | 21 | +$5 170 |
| SMT (MSS + SMT divergence) | 2 | 2 | **+$1 712** |

## Hur man kör backtest

```bash
# 1. Aktivera venv
source /opt/data/profiles/sickan/backtest-venv/bin/activate

# 2. Kör
cd /opt/data/profiles/sickan/scripts
python smc_v1.py

# 3. Resultat sparas till
#    /opt/data/profiles/sickan/scripts/smc_results.csv
```

### Ändra risk

I `smc_v1.py`, uppdatera:

```python
RISK_PCT = 0.005  # 0.5% = $750
# eller
RISK_PCT = 0.01   # 1% = $1 500
```

### Ändra period

```python
START = "2026-04-17"
END = "2026-06-15"
```

5-min data begränsad till ~59 dagar från Yahoo Finance. För längre period, använd 1-hour (max 720 dagar):

```bash
python smc_1h.py
```
