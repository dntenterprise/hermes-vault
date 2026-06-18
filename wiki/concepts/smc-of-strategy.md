# SMC-OF — SMC Structure + Order Flow Hybrid

**Skapad:** 2026-06-17
**Baserat på:** SMC (ICT) + Fabio order flow + din AMD bias-metod
**Instrument:** NQ=F / MNQ 5-min
**Script:** `smc_of.py`

---

## Backtest-resultat (53 dagar)

| Metrik | Värde |
|---|---|
| Trades | 38 (17 SMC-OF + 21 ORB_OF) |
| Win rate | **86.8%** |
| Profit factor | **6.67** |
| Total PnL | **+$19,703** |
| Trades/dag | 0.7 (~1/dag) |
| Avg win | +$702 |
| Stop-losses | 4 st (−$750 var) |
| Target hits | 33 av 38 |

Jämfört med SMC v1: +$6,882 på 59 dagar — **SMC-OF är ~3x bättre**.

---

## Två strategier i en

### 1. SMC-OF (bias-dagar)

**När:** London Open ≥0.3% under/över NY Open

**Entry-krav (≥4 poäng):**
- MSS after sweep (+2p)
- SMT divergence NQ vs ES (+1p)
- London bias rätt (+1p)
- VP-zon (discount/premium) (+1p)
- Absorption bars (+2p) — body <30%, volym >1.3x MA(24)
- Volymdivergens (+2p) — 24-bar low/ high utan volym

**Kontext:**
- NY session (09:30-16:00 ET)
- Daily trend (SMA20 > SMA50 för longs)
- London bias matchar

### 2. ORB_OF (neutrala dagar)

**När:** London bias <0.3% (neutral)

**ORB-range:** Första 15 min av NY (09:30-09:45 ET)

**Entry:** Breakout ovanför ORB high eller under ORB low
**Krav:** Måste ha OF-bekräftelse (absorption, volymdivergens eller volym >1.3x)
**Max:** 1 trade/riktning/dag

**Stop:** ORB low − 0.5 punkter (longs)
**Target:** ORB-range × 1.5 eller nästa swing

---

## London Bias (din metod)

```
London Open vs NY Open (0.3% tröskel)

London Open ≥0.3% UNDER NY Open → London bearish → NY bullish → LÅNG
London Open ≥0.3% ÖVER NY Open  → London bullish → NY bearish → KORT
<0.3% skillnad → NEUTRAL → ORB_OF
```

---

## Exit & Risk

| Typ | Stop | Target |
|---|---|---|
| SMC-OF | ATR × 1.5 | Nästa swing low/high eller ATR × 2.0 |
| ORB_OF | ORB low − 0.5 | ORB-range × 1.5 |

- Risk: 0.5% per trade ($750 på $150K)
- EOD exit kl 16:00 ET om fortfarande i trade

---

## Fil
`/opt/data/profiles/sickan/scripts/smc_of.py`

---

## Sammanfattning — Två modeller, en strategi

| Modell | När | Typ | Entry |
|---|---|---|---|
| **SMC-OF** | Bias-dagar (≥0.3%) | 🔄 Reversal | MSS + sweep + absorption + divergence = trendvändning |
| **ORB_OF** | Neutrala dagar (<0.3%) | 🚀 Breakout | ORB high/low + hög volym + rätt delta = fortsättning |

### Live — klockan 09:30 ET

```
1. Kolla London Open (03:00) vs NY Open (09:30)

   ≥0.3% under → London bearish → NY bullish → SMC-OF (letar longs)
   ≥0.3% över  → London bullish → NY bearish → SMC-OF (letar shorts)
   <0.3%       → NEUTRAL → ORB_OF

2. NEUTRAL → ORB_OF:
   - Vänta till 09:45 (ORB range satt: high/low av 09:30-09:45)
   - Breakout ovanför ORB high eller under ORB low
   - Kräver OF-bekräftelse:
     a) Volym högre än snitt (>1.3x)
     b) Delta candle grön (long) / röd (short)
     c) MA(12)-linjen pekar i rätt riktning
   - Stop: under ORB low (long) / över ORB high (short)

3. BIAS → SMC-OF:
   - Vänta på MSS + sweep inom NY session
   - Kräver ≥4 poäng från poängsystemet
   - Stop: ATR × 1.5
   - Target: nästa swing eller ATR × 2.0
```

### Skillnad

- **SMC-OF** = fångar botten/toppen (reversal)
- **ORB_OF** = fångar impulsen när den väl startat (breakout)
