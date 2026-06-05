# Cumulative Volume Delta (CVD) — Komplett guide

Baserat på Fabio Valentinis order flow-metodik.

---

## Vad är CVD?

CVD = summan av all köpvolym minus all säljvolym, ackumulerat över tid.

```
CVD = Σ(Köpvolym) - Σ(Säljvolym)
```

Varje trade har en **aggressor** — den som trycker ordern in i orderboken (market order), inte den som sitter på limit. Köp-market order = +1 delta. Sälj-market order = -1 delta.

---

## 3 saker CVD avslöjar

### 1. Trend — vem kontrollerar?

| CVD | Pris | Betyder |
|-----|------|---------|
| ⬆ stiger | ⬆ upp | **Äkta upptrend** — köpare aggressiva |
| ⬇ faller | ⬇ ner | **Äkta nedtrend** — säljare aggressiva |
| ⬆ stiger | ⬇ ner | **Bullish divergence** — köpare laddar i smyg |
| ⬇ faller | ⬆ upp | **Bearish divergence** — säljare laddar i smyg |

> *"If you don't see a follow-up on CVD on price, it's a discrepancy."* — Fabio

### 2. Absorption — när marknaden luras

Fabios bread and butter:

```
Pris faller, men CVD planar ut eller vänder upp
    ↓
Säljare trycker priset ner, men köpare absorberar all volym
    ↓
Delta failure = säljarna orkar inte mer
    ↓
Köpare tar över → pris vänder upp
```

CVD gör en **higher low** medan pris gör en **lower low** = köpare steppar in. Detta är reversalsignalen.

### 3. Exhaustion — falska breakout

```
Pris spikar till ny high, men CVD når inte ny high
    ↓
Rörelsen saknar volym — ingen tror på den
    ↓
Falskt breakout → pris vänder
```

---

## Fabios 3-steg med CVD

### Steg 1 — Identifiera key levels + CVD
- Använd CVD för att se **divergens** mot pris
- Volume leder price — CVD ger tidiga reversalsignaler

### Steg 2 — Large Transaction Volume (Big Bubbles)
- Bekräftar om CVD-rörelser kommer från **institutionella aktörer**
- Leta efter koncentrerade large sells som pushar priset
- Större volym + större prisrörelse = bättre signal

### Steg 3 — Price Action Confirmation
- Gå short först när pris **reboundar tillbaka** till nivån där stora säljordrar låg
- Använd INTE limit orders — vänta tills pris är där
- Kolla att det **inte finns ovanligt stora buy orders** på vägen

---

## CVD + Volume Profile = Edge

```
1. Är vi high eller low i curve? (Volume Profile)
2. Finns CVD-divergens mot priset?
3. Ser vi absorption vid en key level?
4. Bekräftar Big Bubbles (large orders)?
5. Gå med momentum eller trapped buyers?
```

### 20R-exemplet
- High in curve + CVD falling = short bias ✅
- Buyers absorption (CVD failure vid support) = sellers tar över ✅
- Big sell orders push through = momentum ✅
- Traila stop tills CVD visar att buyers är tillbaka ✅

---

## Checklista för att läsa CVD

- [ ] CVD trend matchar pris trend? → Äkta rörelse
- [ ] CVD divergerar mot pris? → Vändning snart
- [ ] CVD planar ut vid nivå? → Absorption pågår
- [ ] CVD spikar men pris står still? → Nån stoppar den — stor aktör
- [ ] CVD och pris bryter samtidigt? → Följ med momentum

---

## Vanligaste CVD-misstagen

| Misstag | Sanning |
|---------|---------|
| "CVD är grön = köp" | CVD kan vara grön mitt i en crash om småköpare panikköper mot storsäljare |
| "En bar med hög delta = trend" | En bar betyder inget. Kolla CVD-trend över tid |
| "CVD funkar på alla timeframes" | Fungerar bäst på **volume-based charts** (2 500 volym per bar, inte 1 min) |
| "Stigande pris + stigande CVD = alltid long" | Inte om priset är high-in-curve och nära motstånd |

---

## Verktyg för CVD

| Platform | CVD-indikator |
|----------|--------------|
| **Deep Charts** | Inbyggd CVD (Fabios platform) |
| **ATAS** | Cumulative Delta, Delta Correlated Candles |
| **TradingView** | "AMT CVD" av hardiman |
| **Sierra Chart** | Inbyggd, mest avancerad |
