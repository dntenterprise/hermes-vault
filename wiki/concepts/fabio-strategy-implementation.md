# Fabio Valentini Trading Strategy — Implementation

Baserad på Fabio Valentinis order flow-metodik. Både **long** och **short**, två setup-typer.

---

## Core Principles

1. **CVD (Cumulative Volume Delta)** — visar vem som aggressivt kontrollerar marknaden
2. **Volume Profile** — premium/discount-zoner (high/low in curve)
3. **Absorption** — institutionella aktörer som samlar/distribuerar
4. **Narrative Volume (NV)** — bar length = volym-visualisering
5. **Progressive trailing** — lås profit stegvis

---

## Setup Type A: Momentum (Trend-följande)

**Bäst R:R.** Fånga den dominanta riktningen när den accelererar.

| | Long | Short |
|---|---|---|
| **Kontext** | Daily SMA20 > SMA50 (upptrend) | Daily SMA20 < SMA50 (nedtrend) |
| **Entry-zon** | Pullback till discount (under VAL) | Retrace till premium (över VAH) |
| **Order flow** | CVD stiger med priset | CVD faller med priset |
| **Trigger** | Absorption vid VAL ➔ köpare tar över | Absorption vid VAH ➔ säljare tar över |
| **Exit** | Target 2.0 ATR + trail + CVD fade | Target 2.0 ATR + trail + CVD fade |

**Signal:** Pris gör en lägre low (long) men CVD stiger = köpare steppar in i smyg. Vänta på att absorption bekräftar sen följer momentum upp.

---

## Setup Type B: Trapped Buyers / Sellers (Contrarian)

**Minst lönsam per trade men hög win rate.** Fånga reversaler vid extremer.

| | Long | Short |
|---|---|---|
| **Kontext** | Daily SMA20 > SMA50 (upptrend) | Daily SMA20 < SMA50 (nedtrend) |
| **Entry-zon** | Pris under VAL (discount) | Pris över VAH (premium) |
| **Order flow** | CVD divergence (pris low, delta pos) | CVD divergence (pris high, delta neg) |
| **Trigger** | Sellers exhausted → CVD-planar ut | Buyers exhausted → CVD-planar ut |
| **Exit** | Target 1.5 ATR + CVD fade | Target 1.5 ATR + CVD fade |

**Signal:** Pris gör ny low men CVD gör higher low = säljare orkar inte mer. Reversal upp.

---

## Scoring System

| Condition | Weight | Long | Short |
|---|---|---|---|
| Trend-kontext (SMA20/50) | KRAV | SMA20 > SMA50 | SMA20 < SMA50 |
| Discount/Premium-zon | ×2 | Pris < VAL (20p) | Pris > VAH (20p) |
| CVD divergence | ×2 | Pris low, CVD higher low | Pris high, CVD lower high |
| Absorption | ×2 | Hög volym + liten body | Hög volym + liten body |
| ROC oversold/overbought | ×1 | ROC(12) < -0.5% | ROC(12) > +0.5% |

**Entry kräver ≥ 3 poäng.**
**A-setup (högre konvikt) kräver ≥ 6 poäng.** → 2× position size.

---

## Position Sizing

```
Risk per trade = Kapital × 0,5%
Stop distance = ATR(14) × 1,5
Kontrakt = max(0.1, min(10, Risk / (Stop distance × 20)))

A-setup (≥6p):  kontrakt × 2
```

---

## Exit Rules

| Exit | När | Typ |
|---|---|---|
| **Stop loss** | Pris når entry - ATR×1.5 (long) / entry + ATR×1.5 (short) | Alltid aktiv |
| **Target** | Profit når ATR×2.0 (momentum) / ATR×1.5 (trapped) | Limit order |
| **Trail** | Profit > ATR×1.0 → aktivera, lås 50% av profit | Löpande |
| **CVD fade** | CVD vänder medan trade är i profit | Market order |
| **EOD** | Stäng alla trades vid 16:00 ET | Market order |

---

## Session

- **NY session:** 09:30 - 16:00 ET (15:30 - 22:00 svensk tid sommar)
- Endast trades under NY session
- Högst aktivitet första 2h och sista 1h

---

## Risk Management (Fabios setup A/B/C)

| Setup | Risk | När |
|---|---|---|
| **C** | 1 × bas-risk | Minst konfluens, ≥3p |
| **B** | 1.5 × bas-risk | Medelkonfluens, ≥4p |
| **A** | 2 × bas-risk | All konfluens, ≥6p |
