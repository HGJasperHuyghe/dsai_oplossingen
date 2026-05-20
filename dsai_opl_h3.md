## Hoofdstuk 3
### 3.01 Probability
#### Oefening 1 (na Axioma's van waarschijnlijkheid)
#### Opgave:
Een munt is vervalst zodat P(kop) = 0,6. Wat is P(munt)?

Antwoord:
```text
P(munt) = 0,4

```
Uitleg:
```text
Volgens de complementregel is de kans op het complement van een gebeurtenis gelijk aan 1 min de kans op de gebeurtenis. Omdat kop en munt elkaar uitsluiten en samen de hele uitkomstenruimte vormen, geldt:
P(munt) = 1 - P(kop) = 1 - 0,6 = 0,4.

```
#### Oefening 2 (PlayStation & Nintendo Switch)
#### Opgave:
Van de 250 studenten bezitten er 200 een PlayStation, 100 een Nintendo Switch en 75 beide.
Kies willekeurig een student. Bereken de kans dat:

deze student geen PlayStation bezit

deze student een PlayStation of een Nintendo Switch (of beide) bezit

deze student geen van beide bezit.

Antwoord:
```text

50
250
=
1
5
=
0
,
2
250
50

 = 
5
1

 =0,2

225
250
=
9
10
=
0
,
9
250
225

 = 
10
9

 =0,9

25
250
=
1
10
=
0
,
1
250
25

 = 
10
1

 =0,1

```
Uitleg:
```text

Totaal aantal studenten = 250.

PlayStation: 200, dus geen PlayStation: 250 - 200 = 50 - kans = 50/250 = 1/5.

Voor de unie gebruiken we de algemene somregel:
P(PS U Switch) = P(PS) + P(Switch) - P(beide) = 200/250 + 100/250 - 75/250 = 225/250 = 9/10.
## Hoofdstuk 3

### Lab 3.01 — Probability

#### Oefening 1 (na Axioma's van waarschijnlijkheid)
#### Opgave
Een munt is vervalst zodat P(kop) = 0,6. Wat is P(munt)?

```text
P(munt) = 0,4
```

```text
Uitleg:
Volgens de complementregel is de kans op het complement van een gebeurtenis gelijk aan 1 min de kans op de gebeurtenis. Omdat kop en munt elkaar uitsluiten en samen de hele uitkomstenruimte vormen, geldt:
P(munt) = 1 - P(kop) = 1 - 0,6 = 0,4.
```

#### Oefening 2 — PlayStation & Nintendo Switch
#### Opgave
Van de 250 studenten bezitten er 200 een PlayStation, 100 een Nintendo Switch en 75 beide. Kies willekeurig een student. Bereken de kans dat:
- deze student geen PlayStation bezit
- deze student een PlayStation of een Nintendo Switch (of beide) bezit
- deze student geen van beide bezit.

```text
Geen PlayStation: 50/250 = 1/5 = 0,2
PS of Switch (unie): 225/250 = 9/10 = 0,9
Geen van beide: 25/250 = 1/10 = 0,1
```

```text
Uitleg:
Totaal aantal studenten = 250.
P(PS U Switch) = P(PS) + P(Switch) - P(beide) = 200/250 + 100/250 - 75/250 = 225/250 = 9/10.
Complement = 1 - 9/10 = 1/10.
```

#### Oefening 3 — Autosurvey
#### Opgave
Gegeven tabel met kleuren en types autos.

```text
Totaal aantal autos: 65+59+27+22+16+19 = 208
Zilveren hatchback: 59/208 ≈ 0,284
Totaal hatchbacks: (59+22+19)/208 = 100/208 ≈ 0,481
P(hatchback | zilver) = 59/(65+59) = 59/124 ≈ 0,476
```

```text
Uitleg:
Gebruik de definitie van voorwaardelijke kans: P(A|B) = P(A en B)/P(B).
```

#### Oefening 4 — Golfer toernooien
#### Opgave
Een golfer schat P(1e)=0,6, P(2e)=0,4 en P(both)=0,35.

```text
P(geen van beide) = 1 - P(1e U 2e) = 1 - 0,65 = 0,35
```

```text
Onafhankelijkheid:
Als onafhankelijk: P(1e ∩ 2e) = 0,6×0,4 = 0,24, maar gegeven is 0,35 → niet onafhankelijk.
```

```text
Uitleg:
Sportprestaties kunnen elkaar beïnvloeden (vorm, vertrouwen), dus afhankelijkheid is plausibel.
```

#### Oefening 5 — Engels, Geschiedenis, Biologie
#### Opgave
60% nam geschiedenis, 30% biologie, 10% beide.

```text
P(geen van beide) = 1 - (0,6 + 0,3 - 0,1) = 0,2
P(geschiedenis | precies één) = 0,5 / 0,7 ≈ 0,714
```

```text
Uitleg:
Precies één = (0,6-0,1)+(0,3-0,1) = 0,7; alleen geschiedenis = 0,5.
```

#### Oefening 6 — Leeftijd en geslacht
#### Opgave
Tabel:

Onder 35 | 35 en ouder
Man 345 | 380
Vrouw 362 | 472

```text
Totaal = 1559
P(M) = 725/1559 ≈ 0,465
P(M ∧ Y) = 345/1559 ≈ 0,221
P(vrouw | Y) = 362/707 ≈ 0,512
```

```text
Uitleg:
P(M|Y) = 345/707 ≈ 0,488 ≠ P(M), dus M en Y zijn niet onafhankelijk.
```

#### Oefening 7 — Eerlijke dobbelsteen
#### Opgave
Wat is de kans op elke uitkomst bij één eerlijke dobbelsteen?

```text
x: 1..6  → P(X=x) = 1/6
```

```text
Uitleg:
Zes gelijke uitkomsten ⇒ elk 1/6.
```

#### Oefening 8 — Som van twee dobbelstenen
#### Opgave
Wat is de kans op elke mogelijke som (2..12)?

```text
P(2..12) = [1,2,3,4,5,6,5,4,3,2,1] / 36 respectievelijk
```

```text
Uitleg:
36 paren; aantal manieren per som geeft bovengenoemde verhoudingen.
```

#### Oefening 9 — Discrete kansverdelingen
```text
X: som ≠ 1 → geen
Y: som = 1 → ja
Z: som = 1 → ja
W: negatieve kans → geen
```

```text
Uitleg:
Alle kansen moeten ≥ 0 en som 1.
```

#### Oefening 10 — Afgeleide variabelen van dobbelsteen
```text
Y = 2 - score → waarden {2,4,6,8,10,12} elk 1/6
Z = score^2 → {1,4,9,16,25,36} elk 1/6
W = 0 als score ∈ {1,2,3,6} anders 1 → P(W=0)=4/6, P(W=1)=2/6
```

```text
Uitleg:
Transformaties op discrete uitkomsten; kansen volgen uit oorspronkelijke verdeling.
```

#### Oefening 11 — Som tussen 4 en 7 (inclusief)
```text
P(4≤X≤7) = (3+4+5+6)/36 = 18/36 = 1/2
```

```text
Uitleg:
Tel de kansen uit oefening 8 op.
```

#### Oefening 12 — Onbekende kansen a en k
```text
X: 2/10+1/10+3/10+a+1/10 = 1 ⇒ a = 3/10
P(X≥2) = 0,8

Y: k+2k+2k+k = 6k = 1 ⇒ k = 1/6
P(Y≤0) = 5/6
```

```text
Uitleg:
Som van kansen moet 1 zijn.
```

#### Oefening 13 — Verwachting bibliotheek
```text
E(X) = Σ x·P(x) = 2,25
```

```text
Uitleg:
Verwachting = som van waarde maal kans.
```

#### Oefening 14 — Verwachting & variantie voorbeelden
```text
X: {48,52} p=0.5 elk → E=50, Var=4
Y: {0,100} p=0.5 elk → E=50, Var=2500
```

```text
Uitleg:
Gelijke verwachting, maar Y heeft veel grotere spreiding.
```

#### Oefening 15 — Grootste/kleinste ogen, discrete verwachtingen
```text
Resultaten en berekeningen voor E(Y) en Var(Y) zijn afgeleid uit combinatoriek van twee dobbelstenen; verdere berekeningen zoals gegeven in de oorspronkelijke tekst.
```

```text
Wachttijd en discrete X: bereken E en Var via Σ x·p(x) en Σ (x-μ)^2 p(x).
```

### Lab 3.02 — Centrale Limietstelling (CLT)

#### Opgave — Voorbeeld (koffers)
```text
Gegeven: n=43, μ=12.1 kg, σ=3.8 kg. Bereken P( Xbar < 11 ).
```

```python
# Voorbeeldberekening (gebruik numpy/scipy in het notebook)
import numpy as np
import scipy.stats as stats

n = 43
mu = 12.1
sigma = 3.8
target = 11
se = sigma / np.sqrt(n)
z = (target - mu) / se
prob = stats.norm.cdf(z)
print(se, z, prob)
```

#### Opgave — Voorbeeld (reistijden)
```text
Voorbeeld: μ=23.3, σ=8.9, n=60. Zoek P(Xbar < 25) (antwoord ≈ 0.9305).
```

```python
mu = 23.3; sigma = 8.9; n = 60; target = 25
se = sigma / np.sqrt(n)
z = (target - mu) / se
prob = stats.norm.cdf(z)
print(prob)
```

#### Opgave — 95% CI (cocoa bags)
```python
steekproef = [758,748,749,752,757,760,751,745,759,761]
import numpy as np
import scipy.stats as stats
alpha = 0.05
m = np.mean(steekproef)
n = len(steekproef)
s = np.std(steekproef, ddof=1)
t = stats.t.isf(alpha/2, n-1)
lo = m - t * s/np.sqrt(n)
hi = m + t * s/np.sqrt(n)
print(m, s, lo, hi)
```

### Lab 3.03 — Confidence Intervals

#### Oefening 1 — Gross annual salary (rlanders.csv)
```python
import pandas as pd
import numpy as np
import scipy.stats as stats

df = pd.read_csv('https://raw.githubusercontent.com/HoGentTIN/dsai-en-labs/main/data/rlanders.csv').set_index(['ID'])
money = df['Money']
n = len(money)
x_bar = money.mean()

# 99% CI (σ=98 bekend)
print(stats.norm.interval(0.99, loc=x_bar, scale=98/np.sqrt(n)))

# 95% CI (σ=98 bekend)
print(stats.norm.interval(0.95, loc=x_bar, scale=98/np.sqrt(n)))

# 95% CI (σ onbekend -> t)
s = money.std(ddof=1)
print(stats.t.interval(0.95, df=n-1, loc=x_bar, scale=s/np.sqrt(n)))

# 95% CI (eerste 25 observaties)
money_25 = money.head(25)
print(stats.t.interval(0.95, df=len(money_25)-1, loc=money_25.mean(), scale=money_25.std(ddof=1)/np.sqrt(len(money_25))))
```

### Lab 3.04 — Hypothesis Testing

#### Oefening 1 — Navy recruits (rechtszijdige t-toets)
```python
import pandas as pd
import scipy.stats as stats
df_recruits = pd.read_csv('https://raw.githubusercontent.com/HoGentTIN/dsai-labs/main/data/recruten.csv', sep=';', decimal=',')
heights = df_recruits['Height']
mu = 69
s = heights.std(ddof=1)
n = len(heights)
x_bar = heights.mean()
stat = (x_bar - mu) / (s/np.sqrt(n))
p_val = 1 - stats.t.cdf(stat, df=n-1)
print(p_val)
```

#### Oefening 2 — Rainfall (rechtszijdige Z-toets)
```python
mu = 82.3; sigma = 15.3; n = 5; alpha = 0.05
g = stats.norm.ppf(1 - alpha, loc=mu, scale=sigma/np.sqrt(n))
mu_alt = 105
beta = stats.norm.cdf(g, loc=mu_alt, scale=sigma/np.sqrt(n))
print(g, beta)
```

#### Oefening 3 — Medical procedure (linkszijdige Z-toets)
```python
mu = 34.2; sigma = 2.6; n = 50; x_bar = 33.5; alpha = 0.05
p_val = stats.norm.cdf(x_bar, loc=mu, scale=sigma/np.sqrt(n))
g = stats.norm.ppf(alpha, loc=mu, scale=sigma/np.sqrt(n))
print(p_val, g)
```

#### Oefening 4 — Stopping distance (rechtszijdige t-toets)
```python
import numpy as np
distances = np.array([9.78,9.33,9.57,9.26,9.45,9.72])
mu = 9.15
n = len(distances)
x_bar = distances.mean()
s = distances.std(ddof=1)
stat = (x_bar - mu) / (s/np.sqrt(n))
p_val = 1 - stats.t.cdf(stat, df=n-1)
g = stats.t.ppf(0.99, df=n-1, loc=mu, scale=s/np.sqrt(n))
print(p_val, g)
```

#### Oefening 5 — Salary split by Gender (rlanders)
```python
df = pd.read_csv('https://raw.githubusercontent.com/HoGentTIN/dsai-en-labs/main/data/rlanders.csv').set_index(['ID'])
mu = 500; sigma = 98; alpha = 0.05
men = df[df['Gender']=='Male']['Money']
women = df[df['Gender']=='Female']['Money']
p_men = 1 - stats.norm.cdf(men.mean(), loc=mu, scale=sigma/np.sqrt(len(men)))
p_women = stats.norm.cdf(women.mean(), loc=mu, scale=sigma/np.sqrt(len(women)))
print(p_men, p_women)
```

#### Oefening 6 — BSA (rechtszijdige Z-toets)
```python
mu = 44; sigma = 6.2; n = 72; x_bar = 46.2; alpha = 0.025
g = stats.norm.ppf(1 - alpha, loc=mu, scale=sigma/np.sqrt(n))
p_val = 1 - stats.norm.cdf(x_bar, loc=mu, scale=sigma/np.sqrt(n))
print(g, p_val)
```
# Resultaten printen

print(f"Standaardfout (SE): {se:.4f} kg")

print(f"Z-score:            {z_score:.4f}")

print(f"Kans (probability): {kans:.4f}")



The times for a particular journey during rush hour in a large city have mean 23.3 minutes and standard deviation 8.9 minutes.  



