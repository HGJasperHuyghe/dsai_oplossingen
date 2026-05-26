# Tussentijse Herhaling

#### Voorbereiding: Package imports

```python
import numpy as np
import scipy.stats as stats
```

---

## Vraag 1

### Exercise 1: Sinaasappelsap (Normale verdeling)

#### Overzicht

We werken hier met een normale verdeling waarbij $\mu = 133$ en $\sigma = 11{,}3$.

#### Stap 1: Kans op tussen 133 gram en 142 gram sap

Je berekent de cumulatieve kans tot 142 gram en trekt daar de cumulatieve kans tot 133 gram van af.

**Wiskundig**: $P(133 < X < 142)$. Aangezien 133 het gemiddelde is, is het gebied eronder 50%. De z-score voor 142 is $(142-133)/11.3 \approx 0.796$. De kans is **28,71%**.

```python
import scipy.stats as stats
mu, sigma = 133, 11.3
kans = stats.norm.cdf(142, loc=mu, scale=sigma) - stats.norm.cdf(133, loc=mu, scale=sigma)
print(f"Kans: {kans:.4f}")  # 0.2871
```

#### Stap 2: 77% van de sinaasappels bevat minstens hoeveel gram sap?

"Minstens" betekent dat we de grens zoeken waarbij 77% van de verdeling rechts van deze waarde ligt, en 23% links ervan. We zoeken dus het 23ste percentiel (de inverse CDF of PPF).

**Wiskundig**: $P(X > x) = 0.77 \Rightarrow P(X \le x) = 0.23$. De bijbehorende grenswaarde is **124,65 gram**.

```python
grens_minstens = stats.norm.ppf(0.23, loc=mu, scale=sigma)
print(f"Minstens: {grens_minstens:.2f} gram")  # 124.65
```

#### Stap 3: 80% (symmetrisch) ligt tussen welke twee waarden?

Als 80% in het midden ligt, blijft er 20% over voor de uiteinden (10% links en 10% rechts). Je zoekt dus het 10de en 90ste percentiel.

**Waarden**: Tussen **118,52 gram** en **147,48 gram**.

```python
ondergrens = stats.norm.ppf(0.10, loc=mu, scale=sigma)
bovengrens = stats.norm.ppf(0.90, loc=mu, scale=sigma)
print(f"Tussen {ondergrens:.2f} en {bovengrens:.2f} gram")
```

---

## Vraag 2

### Exercise 2: Spelshow (Kansbomen & Verwachte waarde)

#### Overzicht

Dit is een complexere oefening in voorwaardelijke kansen.

**Regels**: Start met 2 rode (R) en 2 groene (G). Bij R: prijs + terugleggen. Bij G: geen prijs + niet terugleggen. Het spel stopt na 3 beurten OF na 2 groene ballen.

#### Stap 1: Kans op geen enkele prijs (0 rode ballen)

Dit kan enkel als de speler direct 2 keer groen trekt (G $\rightarrow$ G).

- Trekking 1 (G): $2/4$
- Trekking 2 (G, er is nog 2R, 1G over): $1/3$

**Kans**: $\frac{2}{4} \times \frac{1}{3} = \frac{2}{12} = \frac{1}{6}$ (of 16,67%)

#### Stap 2: Kans op exact 1 rode bal

**Paden**: G $\rightarrow$ R $\rightarrow$ G of R $\rightarrow$ G $\rightarrow$ G

- Pad GRG: $(\frac{2}{4}) \times (\frac{2}{3}) \times (\frac{1}{3}) = \frac{4}{36} = \frac{1}{9}$
- Pad RGG: $(\frac{2}{4}) \times (\frac{2}{4}) \times (\frac{1}{3}) = \frac{4}{48} = \frac{1}{12}$

**Kans**: $\frac{1}{9} + \frac{1}{12} = \frac{4}{36} + \frac{3}{36} = \frac{7}{36}$ (of 19,44%)

#### Stap 3: Kans op exact 2 rode ballen

**Paden**: G $\rightarrow$ R $\rightarrow$ R of R $\rightarrow$ G $\rightarrow$ R of R $\rightarrow$ R $\rightarrow$ G

- Pad GRR: $(\frac{2}{4}) \times (\frac{2}{3}) \times (\frac{2}{3}) = \frac{8}{36} = \frac{2}{9}$
- Pad RGR: $(\frac{2}{4}) \times (\frac{2}{4}) \times (\frac{2}{3}) = \frac{8}{48} = \frac{1}{6}$
- Pad RRG: $(\frac{2}{4}) \times (\frac{2}{4}) \times (\frac{2}{4}) = \frac{8}{64} = \frac{1}{8}$

**Kans**: $\frac{2}{9} + \frac{1}{6} + \frac{1}{8} = \frac{16}{72} + \frac{12}{72} + \frac{9}{72} = \frac{37}{72}$ (of 51,39%)

#### Stap 4: Kans op exact 3 rode ballen

**Pad**: R $\rightarrow$ R $\rightarrow$ R

**Kans**: $(\frac{2}{4}) \times (\frac{2}{4}) \times (\frac{2}{4}) = \frac{8}{64} = \frac{1}{8}$ (of 12,50%)

#### Stap 5: Verwachte prijs (Expected Value)

**Formule**: $E(X) = \sum (x_i \cdot P(x_i))$ waarbij de prijs per rode bal $\$1000$ is.

$$E(X) = (\$0 \times \tfrac{12}{72}) + (\$1000 \times \tfrac{14}{72}) + (\$2000 \times \tfrac{37}{72}) + (\$3000 \times \tfrac{9}{72})$$

$$E(X) = \frac{14000 + 74000 + 27000}{72} = \frac{115000}{72} \approx \$1597{,}22$$

---

## Vraag 3

### Exercise 3: CO₂ Betrouwbaarheidsinterval (Confidence Interval)

#### Overzicht

Gegeven: $n = 50$, $\bar{x} = 654.16$, $s = 164.43$.

Omdat we de steekproefstandaardafwijking ($s$) gebruiken en niet de populatiestandaardafwijking ($\sigma$), gebruiken we de **t-verdeling** met $n-1 = 49$ vrijheidsgraden (degrees of freedom).

#### Stap 1: Berekening

**Wiskundig**: $\text{CI} = \bar{x} \pm t_{\alpha/2} \cdot \frac{s}{\sqrt{n}}$

- $SE = \frac{164.43}{\sqrt{50}} \approx 23.25$
- $t$-waarde (95% CI, df=49) $\approx 2.0096$
- Margin of Error $= 2.0096 \times 23.25 \approx 46.73$
- Interval: $[654.16 - 46.73,\ 654.16 + 46.73] =$ **[607.43, 700.89]**

```python
n, mean, std = 50, 654.16, 164.43
interval = stats.t.interval(0.95, df=n-1, loc=mean, scale=std/np.sqrt(n))
print(f"95% CI: {interval}")
```

#### Stap 2: Interpretatie

We zijn 95% zeker dat het ware, gemiddelde CO₂-niveau in de gehele populatie van dergelijke huizen tussen 607,43 ppm en 700,89 ppm ligt.

---

## Vraag 4

### Exercise 4: Schietclub (Binomiale verdeling)

#### Overzicht

Dit is een klassiek binomiaal experiment waarbij $n = 4$ en $p = 0.25$.

#### Stap 1: Kans dat Jan ten minste 1 keer raakt

Dit is $1 - P(\text{exact } 0 \text{ keer})$.

$$1 - (0.75)^4 = 1 - 0.3164 = 0{,}6836 \text{ (of 68,36\%)}$$

#### Stap 2: Kans dat Jan exact 2 keer raakt

$$P(X=2) = \binom{4}{2} \cdot (0.25)^2 \cdot (0.75)^2 = 6 \cdot 0.0625 \cdot 0.5625 = 0{,}2109 \text{ (of 21,09\%)}$$

#### Stap 3: Kans dat Jan ten minste 3 keer raakt

Dit is $P(X=3) + P(X=4)$.

- $P(X=3) = \binom{4}{3} \cdot (0.25)^3 \cdot (0.75)^1 = 4 \cdot 0.0156 \cdot 0.75 = 0.046875$
- $P(X=4) = \binom{4}{4} \cdot (0.25)^4 \cdot (0.75)^0 = 0.003906$

**Som**: $0.046875 + 0.003906 = 0{,}0508$ (of 5,08%)

```python
n_trials, p_succes = 4, 0.25
print("Min 1:", 1 - stats.binom.pmf(0, n_trials, p_succes))
print("Exact 2:", stats.binom.pmf(2, n_trials, p_succes))
print("Min 3:", stats.binom.sf(2, n_trials, p_succes))  # sf = survival function (kans op > 2)
```

---

## Vraag 5

### Exercise 5: Meetniveaus

#### Overzicht

Bepaal het meetniveau van elke variabele en motiveer.

#### Stap 1: De titel van een dokter (huisarts, specialist, professor) → Ordinaal

**Reden**: Er zit een duidelijke academische hiërarchie of rangorde in deze titels, maar de "afstand" tussen de titels kun je niet meten of kwantificeren. (Opmerking: als het louter als beroepencategorie wordt gezien, kan dit door sommige docenten als nominaal worden bestempeld, maar binnen de medische wereld is dit een rangorde.)

#### Stap 2: Of een arts geconventioneerd is → Nominaal (specifiek: dichotoom)

**Reden**: Het is een simpele "ja" of "nee" zonder rangorde.

#### Stap 3: Het aantal euro voor een consultatie → Ratio

**Reden**: Het is kwantitatief, met gelijke intervallen én een absoluut nulpunt (0 euro betekent de afwezigheid van kosten; je kunt stellen dat 50 euro de helft is van 100 euro).

#### Stap 4: De specialisatie van een arts (oncologie, oogarts) → Nominaal

**Reden**: Het zijn categorieën zonder enige intrinsieke rangorde (een oogarts is niet "hoger" of "meer" dan een oncoloog).

---

## Vraag 6

### Exercise 6: Frisdrankflessen (Normale verdeling)

#### Overzicht

We werken met $\mu = 2.0$ liter en $\sigma = 0.05$ liter.

#### Stap 1: Tussen 1,90 en 2,0 liter?

**Wiskundig**: Dit is de linkerhelft tussen de grens en het gemiddelde. De z-score voor 1,90 is $(1.90 - 2.0) / 0.05 = -2$. De oppervlakte tussen $Z = -2$ en $Z = 0$ bedraagt **47,72%**.

#### Stap 2: Minder dan 1,90 of meer dan 2,10 liter?

**Wiskundig**: $Z_{1.90} = -2$ en $Z_{2.10} = +2$. Je zoekt de extreme staarten. $P(Z < -2) \approx 0.0228$ en $P(Z > 2) \approx 0.0228$.

**Som**: $0.0228 + 0.0228 = 0{,}0456$ (of 4,56%)

#### Stap 3: 99% van de flessen bevat minstens hoeveel frisdrank?

Hier zoek je het 1ste percentiel ($P(X < x) = 0.01$), aangezien 99% van de verdeling rechts van deze waarde moet liggen.

**Wiskundig**: $Z$-waarde voor 1% in de linkerstaart is $\approx -2.326$.

$$x = \mu + (Z \cdot \sigma) = 2.0 + (-2.326 \cdot 0.05) = 1{,}8837 \text{ liter}$$

```python
mu_f, sigma_f = 2.0, 0.05
# 1. Tussen 1.9 en 2.0
print(stats.norm.cdf(2.0, mu_f, sigma_f) - stats.norm.cdf(1.9, mu_f, sigma_f))  # 0.4772
# 2. Buiten 1.9 en 2.1
print(stats.norm.cdf(1.9, mu_f, sigma_f) + stats.norm.sf(2.1, mu_f, sigma_f))   # 0.0455
# 3. Minstens 99%
print(stats.norm.ppf(0.01, mu_f, sigma_f))  # 1.8837
```

---

## Samenvatting Voorbeeldexamen (Deel 3)

Dit derde deel behandelt de volgende technieken:

### Verdelingen

1. **Normale verdeling**: CDF voor kansen tussen grenzen, PPF (inverse) voor percentielen en symmetrische intervallen
2. **Binomiale verdeling**: `stats.binom.pmf` / `stats.binom.sf` voor "exact", "minstens" en "hoogstens"
3. **Kansbomen & verwachte waarde**: Voorwaardelijke kansen langs paden, $E(X) = \sum x_i \cdot P(x_i)$

### Schatten & interpreteren

- **Betrouwbaarheidsinterval**: t-verdeling wanneer $\sigma$ onbekend is ($\bar{x} \pm t_{\alpha/2} \cdot \frac{s}{\sqrt{n}}$)
- **Meetniveaus**: Nominaal, ordinaal, interval, ratio — bepaald door rangorde, gelijke intervallen en het bestaan van een absoluut nulpunt
