# SMC-OF — SMC Structure + Order Flow Hybrid

**Skapad:** 2026-06-17
**Uppdaterad:** 2026-06-19 (final — alla optimeringar)
**Baserat på:** SMC (ICT) + Fabio order flow + din AMD bias-metod
**Instrument:** NQ=F / MNQ 5-min
**Script:** `smc_of.py`

---

## Backtest-resultat (53 dagar)

| Metrik | Värde |
|---|---|
| **Trades** | 54 (46L + 8S) |
| **Win Rate** | **72.2%** |
| **Profit Factor** | **2.91** |
| **Total PnL** | **+$21,018** |
| **Avkastning** | **14.01%** (på $150K) |
| **R:R** | 1.12 |
| Trades/dag | 1.0 |
| Avg win | +$821 |
| Avg loss | -$734 |
| Target hits | 43 av 54 |
| Stops | 10 |
| EOD exits | 1 |

**Per entry-typ:**

| Entry | Trades | WR | PnL |
|---|---|---|---|
| ORB_OF (0p) | 21 | 81% | +$7,224 |
| SMC-OF 4p | 27 | 74% | +$14,130 |
| SMC-OF 5p+ | 6 | 33% | -$336 |

**Per riktning:**

| Riktning | Trades | Wins | PnL |
|---|---|---|---|
| Longs | 46 | 32 | +$13,348 |
| Shorts | 8 | 7 | +$7,670 |

Jämfört med SMC v1: +$6,882 på 59 dagar — **SMC-OF är ~3x bättre**.

**Ändringshistorik:**
- 18 juni: Bias-formeln fixad (var inverterad)
- 19 juni: Stop ATR×1.0 (snävare = större position)
- 19 juni: ORB-stop orb_mid (position size baserad på faktiskt stop-avstånd)
- 19 juni: Target = nästa swing-nivå (ATR×2.0 / ORB×1.5 som fallback)

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

**Stop:** **orb_mid** ((orb_high + orb_low) / 2)
**Target:** Nästa swing-nivå (ORB-range × 1.5 som fallback)

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

**Tolkning:** Om priset går upp under London-sessionen (bullish London), förväntar vi oss en nedgång i NY (bearish bias) — mean reversion.

---

## Exit & Risk

| Typ | Stop | Target |
|---|---|---|
| **SMC-OF** | **ATR × 1.0** | Närmaste swing high/low |
| **ORB_OF** | **orb_mid** | Närmaste swing high/low |

- Risk: 0.5% per trade ($750 på $150K)
- ORB position size: $750 / faktiskt stop-avstånd (entry → orb_mid)
- EOD exit kl 16:00 ET om fortfarande i trade
- Target fallback: ATR×2.0 (SMC) / ORB×1.5 (ORB) — används om inget swing inom 40 bars

---

## Fil
`/opt/data/profiles/sickan/scripts/smc_of.py`

---

## Sammanfattning — Två modeller, en strategi

| Modell | När | Typ | Entry | Stop |
|---|---|---|---|---|
| **SMC-OF** | Bias-dagar (≥0.3%) | 🔄 Reversal | MSS + sweep + absorption + SMT | ATR×1.0 |
| **ORB_OF** | Neutrala dagar (<0.3%) | 🚀 Breakout | ORB high/low + hög volym | orb_mid |

### Live — klockan 09:30 ET

```
1. Kolla London Open (03:00) vs NY Open (09:30)

   ≥0.3% under → London bearish → NY bullish → SMC-OF (letar longs)
   ≥0.3% över  → London bullish → NY bearish → SMC-OF (letar shorts)
   <0.3%       → NEUTRAL → ORB_OF

2. NEUTRAL → ORB_OF:
   - Vänta till 09:45 (ORB range satt)
   - Breakout ovanför ORB high eller under ORB low
   - Kräver OF-bekräftelse (volym >1.3x, absorption, divergence)
   - Stop: orb_mid (mitten av range)
   - Target: nästa swing low/high

3. BIAS → SMC-OF:
   - Vänta på MSS + sweep inom NY session
   - Kräver ≥4 poäng från poängsystemet
   - Stop: ATR×1.0
   - Target: nästa swing low/high
```
