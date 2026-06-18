# SMC-OF — SMC Structure + Order Flow Hybrid

**Skapad:** 2026-06-17
**Uppdaterad:** 2026-06-18 (fixed bias-formel)
**Baserat på:** SMC (ICT) + Fabio order flow + din AMD bias-metod
**Instrument:** NQ=F / MNQ 5-min
**Script:** `smc_of.py`

---

## Backtest-resultat (53 dagar)

| Metrik | Värde |
|---|---|
| Trades | 53 (32 SMC-OF + 21 ORB_OF) |
| Win rate | **79.2%** |
| Profit factor | **3.78** |
| Total PnL | **+$15,858** |
| Return | **10.57%** (på $150K) |
| Trades/dag | 1.0 |
| Avg win | +$513 |
| Avg loss | -$518 |
| Target hits | 47 av 53 |
| Longs | 45 tr · 35 wins (+$10,727) |
| Shorts | 8 tr · 7 wins (+$5,131) |

Jämfört med SMC v1: +$6,882 på 59 dagar — **SMC-OF är ~2.3x bättre**.

**Viktig fix 18 juni:** Bias-formeln var inverterad. Använder nu `diff = (ny_open - lo_open) / lo_open` — positiv diff = London bullish → NY bear bias (shorts), negativ diff = London bearish → NY bull bias (longs).

---

## Två strategier i en

### 1. SMC-OF (bias-dagar)

**När:** |London Open − NY Open| ≥ 0.3%

**Entry-krav (≥4 poäng):**
- MSS after sweep (+2p)
- SMT divergence NQ vs ES (+1p)
- London bias matchar (+1p)
- VP-zon (discount/premium) (+1p)
- Absorption bars (+2p) — body <30%, volym >1.3x MA(24)
- Volymdivergens (+2p) — 24-bar low/high utan volym

**Kontext:**
- NY session (09:30-16:00 ET)
- Daily trend (SMA20 > SMA50 för longs)
- London bias matchar riktning

### 2. ORB_OF (neutrala dagar)

**När:** London bias < 0.3% (neutral)

**ORB-range:** Första 15 min av NY (09:30-09:45 ET)

**Entry:** Breakout ovanför ORB high eller under ORB low
**Krav:** Måste ha OF-bekräftelse (absorption, volymdivergens eller volym >1.3x)
**Max:** 1 trade/riktning/dag

**Stop:** ORB low − 0.5 punkter (longs)
**Target:** ORB-range × 1.5 eller nästa swing

---

## London Bias (din metod)

```
diff = (NY_open - London_open) / London_open

diff ≥ +0.3%  → London bullish → NY bearish bias (letar SHORTS)
diff ≤ −0.3%  → London bearish → NY bullish bias (letar LONGS)
|diff| < 0.3% → NEUTRAL → ORB_OF breakout
```

London open = första 5-min baren kl 03:00 ET
NY open = första 5-min baren kl 09:30 ET

**Tolkning:** Om priset går upp under London-sessionen (bullish London), förväntar vi oss en nedgång i NY (bearish bias) — mean reversion. Om priset går ner under London (bearish London), förväntar vi oss uppgång i NY (bullish bias).

---

## Exit & Risk

| Typ | Stop | Target |
|---|---|---|
| SMC-OF | ATR × 1.5 | Nästa swing low/high eller ATR × 2.0 |
| ORB_OF | ORB low − 0.5 | ORB-range × 1.5 |

- Risk: 0.5% per trade ($750 på $150K)
- EOD exit kl 16:00 ET om fortfarande i trade

---

## Resultat per entry-typ

| Entry | Trades | Wins | PnL |
|---|---|---|---|
| ORB_OF (0p) | 21 | 20 (95%) | +$7,793 |
| SMC-OF 4p | 26 | 20 (77%) | +$8,350 |
| SMC-OF 5p | 5 | 2 (40%) | +$26 |
| SMC-OF 6p | 1 | 0 (0%) | -$311 |

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
