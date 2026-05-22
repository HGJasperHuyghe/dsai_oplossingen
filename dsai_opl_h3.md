## Hoofdstuk 3

> Kansrekening, de centrale limietstelling, betrouwbaarheidsintervallen en hypothesetoetsen.
> Alle oplossingen gaan uit van de volgende imports:
>
> ```python
> import numpy as np
> import pandas as pd
> import scipy.stats as stats
> import matplotlib.pyplot as plt
> import seaborn as sns
> ```

### Theorie

Hieronder de oefeningen die in de theorie-notebooks (`3.01`, `3.02`) verspreid staan.

#### Opgave — Complementregel (vervalste munt)
#### Vraag
A coin is tampered with such that P(head) = 0.6. What is P(tail)?

```text
P(tail) = 1 - P(head) = 1 - 0.6 = 0.4
(complementregel: P(Ā) = 1 - P(A))
```

#### Opgave — Som- en complementregel (spelconsoles)
#### Vraag
Van 250 studenten bezitten er 200 een PlayStation, 100 een Nintendo Switch, en 75 beide. Bepaal voor een willekeurige student de kans dat hij/zij (1) geen PlayStation heeft, (2) een PlayStation of Switch (of beide) heeft, (3) geen van beide heeft.

```text
1. P(geen PS) = 1 - 200/250 = 1/5 = 0.2
2. P(PS ∪ NS) = (200 + 100 - 75)/250 = 225/250 = 9/10 = 0.9   (algemene somregel)
3. P(geen van beide) = 1 - 9/10 = 1/10 = 0.1                  (complementregel)
```

#### Opgave — Voorwaardelijke kans (auto's)
#### Vraag
| | Saloon | Hatchback |
|:--|:--:|:--:|
| Silver | 65 | 59 |
| Black | 27 | 22 |
| Other | 16 | 19 |

Eén auto wordt willekeurig gekozen. Bereken de kans dat de auto een zilveren hatchback is, een hatchback is, en een hatchback is gegeven dat hij zilver is.

```python
totaal = 65+59+27+22+16+19          # 208
zilver = 65+59                       # 124
hatch  = 59+22+19                    # 100
zilver_hatch = 59

print("P(zilveren hatchback) =", zilver_hatch/totaal)        # 59/208  ≈ 0.2837
print("P(hatchback)          =", hatch/totaal)               # 100/208 ≈ 0.4808
print("P(hatchback | zilver) =", zilver_hatch/zilver)        # 59/124  ≈ 0.4758
```

#### Opgave — Onafhankelijkheid (golfer)
#### Vraag
P(wint toernooi 1) = 0.6, P(wint toernooi 2) = 0.4, P(wint beide) = 0.35. Bereken de kans dat hij geen van beide wint en toon aan dat de gebeurtenissen niet onafhankelijk zijn.

```text
P(geen van beide) = 1 - P(A ∪ B) = 1 - (0.6 + 0.4 - 0.35) = 1 - 0.65 = 0.35

Onafhankelijk zou betekenen: P(A ∩ B) = P(A)·P(B) = 0.6 · 0.4 = 0.24.
Maar P(A ∩ B) = 0.35 ≠ 0.24, dus de gebeurtenissen zijn NIET onafhankelijk.

Het zou verrassend zijn als ze onafhankelijk waren: de prestaties in twee
opeenvolgende weken hangen samen (vorm van de dag, blessures, weer, ...),
dus winst in het ene toernooi maakt winst in het andere waarschijnlijker.
```

#### Opgave — Voorwaardelijke kans (Engels/Geschiedenis/Biologie)
#### Vraag
60% volgde Geschiedenis, 30% Biologie, 10% beide. Bereken P(geen van beide) en P(Geschiedenis | precies één van beide).

```text
P(noch H noch B) = 1 - P(H ∪ B) = 1 - (0.60 + 0.30 - 0.10) = 1 - 0.80 = 0.20

Precies één: P(alleen H) = 0.60 - 0.10 = 0.50 ; P(alleen B) = 0.30 - 0.10 = 0.20
P(exact één) = 0.50 + 0.20 = 0.70
P(H | exact één) = 0.50 / 0.70 = 5/7 ≈ 0.714
```

#### Opgave — Onafhankelijkheid (leeftijd m/v)
#### Vraag
| | Under 35 | 35 and over |
|:--|:--:|:--:|
| Male | 345 | 380 |
| Female | 362 | 472 |

M = man, Y = jonger dan 35. Bereken P(M), P(M ∩ Y), ga na of M en Y onafhankelijk zijn, en bereken P(vrouw | jonger dan 35).

```python
M = 345+380; F = 362+472; Y = 345+362; totaal = M+F   # 1559
print("P(M)      =", M/totaal)                          # 725/1559  ≈ 0.4650
print("P(M ∩ Y)  =", 345/totaal)                        # 345/1559  ≈ 0.2213
print("P(M)·P(Y) =", (M/totaal)*(Y/totaal))             #            ≈ 0.2109
# 0.2213 ≠ 0.2109  → M en Y zijn NIET onafhankelijk
print("P(F | Y)  =", 362/Y)                             # 362/707   ≈ 0.5120
```

#### Opgave — Discrete kansvariabele herkennen
#### Vraag
Welke van X, Y, Z, W zijn discrete kansvariabelen? Geef een reden voor wie het niet is.

```text
Een discrete kansvariabele vereist: alle p_i ≥ 0 én Σ p_i = 1.

X: 2/10+3/10+4/10+3/10+2/10 = 14/10 ≠ 1  → GEEN kansvariabele.
Y: 2/10+3/10+1/10+3/10+1/10 = 10/10 = 1, alle ≥ 0  → WEL.
Z: 1/2+1/4+1/8+1/16+1/16 = 16/16 = 1, alle ≥ 0  → WEL.
W: bevat -2/10 < 0  → GEEN kansvariabele (negatieve kans).
```

#### Opgave — Kansverdeling bij dobbelsteen
#### Vraag
Een eerlijke dobbelsteen wordt geworpen. Geef de kansverdeling van X = ogen, Y = 2·ogen, Z = ogen², en W (0 als deler van 6, anders 1).

```text
X: waarden 1..6, elk met kans 1/6.
Y: waarden 2,4,6,8,10,12, elk met kans 1/6.
Z: waarden 1,4,9,16,25,36, elk met kans 1/6.
W: ogen 1,2,3,6 zijn delers van 6 (W=0) → P(W=0)=4/6=2/3 ; P(W=1)=2/6=1/3.
```

#### Opgave — Kansmassafunctie gebruiken (twee dobbelstenen)
#### Vraag
Bij het werpen van twee dobbelstenen: wat is de kans dat de som tussen 4 en 7 ligt (grenzen inbegrepen)?

```text
P(4 ≤ X ≤ 7) = P(4)+P(5)+P(6)+P(7) = (3+4+5+6)/36 = 18/36 = 1/2

Of via linkerstaartkansen:
P(4 ≤ X ≤ 7) = P(X ≤ 7) - P(X ≤ 3) = 21/36 - 3/36 = 18/36 = 1/2
```

#### Opgave — Onbekende kans bepalen
#### Vraag
Bepaal de ontbrekende parameter en de gevraagde kans voor X, Y en Z.

```text
X: a = 1 - (2/10+1/10+3/10+1/10) = 3/10.  P(X ≥ 2) = 1 - 2/10 = 8/10 = 0.8
Y: k+2k+2k+k = 6k = 1 → k = 1/6.  P(Y ≤ 0) = k+2k+2k = 5k = 5/6
Z: a = 1 - (4/10+2/10+3/10+1/10) = 0.  P(Z ≥ 9) = a + 1/10 = 0 + 1/10 = 1/10
```

#### Opgave — Verwachtingswaarde (bibliotheek)
#### Vraag
X = aantal geleende boeken met verdeling [0.24, 0.12, 0.14, 0.30, 0.05, 0.15] voor x = 0..5. Bereken E(X).

```python
x = np.array([0,1,2,3,4,5])
p = np.array([0.24,0.12,0.14,0.30,0.05,0.15])
print("E(X) =", np.sum(x*p))     # 2.25
```

#### Opgave — Verwachting en variantie (e-mails)
#### Vraag
X: 48 of 52 e-mails, elk 50%. Y: 0 of 100 e-mails, elk 50%. Bepaal de pmf, E(X), E(Y) en de varianties.

```python
# E(X) = E(Y) = 50 (zelfde verwachting), maar heel andere spreiding
varX = 0.5*(48-50)**2 + 0.5*(52-50)**2   # 4
varY = 0.5*(0-50)**2  + 0.5*(100-50)**2  # 2500
print("E(X)=E(Y)=50 ;  Var(X) =", varX, " Var(Y) =", varY)
# Y kent veruit de grootste variabiliteit.
```

#### Opgave — Verwachting/variantie bij twee dobbelstenen
#### Vraag
1. Y = grootste van twee dobbelstenen, bereken E(Y). 2. Y = kleinste, bereken E(Y) en Var(Y). 3. Klant: bereken gemiddelde en standaardafwijking van X en de gemiddelde wachttijd.

```python
import itertools
uitkomsten = list(itertools.product(range(1,7), repeat=2))
groter   = [max(a,b) for a,b in uitkomsten]
kleiner  = [min(a,b) for a,b in uitkomsten]
print("E(grootste) =", np.mean(groter))        # 4.4722
print("E(kleinste) =", np.mean(kleiner))        # 2.5278
print("Var(kleinste) =", np.var(kleiner))       # 1.9715

# 3. Aantal klanten X en wachttijd
x = np.array([0,1,2,3,4]); p = np.array([0.1,0.25,0.3,0.25,0.1])
mu = np.sum(x*p)
var = np.sum((x-mu)**2 * p)
print("E(X) =", mu, " sd(X) =", np.sqrt(var))   # E=2, sd=1.0954
wacht = np.array([0,2,5,8,11])
print("Gem. wachttijd =", np.sum(wacht*p))       # 5.0 minuten
```

#### Opgave — Standaardnormale kansen
#### Vraag
Als Z ~ Nor(0,1): bereken P(Z < 1.62), P(Z > 0.76), P(Z < -1.32) en P(-1.2 < Z < 1.7).

```python
print(round(stats.norm.cdf(1.62), 4))                       # 0.9474
print(round(1 - stats.norm.cdf(0.76), 4))                   # 0.2236
print(round(stats.norm.cdf(-1.32), 4))                      # 0.0934
print(round(stats.norm.cdf(1.7) - stats.norm.cdf(-1.2), 4)) # 0.8403
```

#### Opgave — Normale verdeling (stalen balken)
#### Vraag
Lengtes ~ Nor(μ=12.5, σ²=0.0004). Bruikbaar tussen 12.47 en 12.53 m. (1) Welk aandeel is onbruikbaar? (2) Idem met σ² = 0.0002.

```python
sigma1 = np.sqrt(0.0004)
bruikbaar1 = stats.norm.cdf(12.53,12.5,sigma1) - stats.norm.cdf(12.47,12.5,sigma1)
print("Onbruikbaar (σ²=0.0004):", round(1-bruikbaar1,4))    # ≈ 0.1336 (13.36%)

sigma2 = np.sqrt(0.0002)
bruikbaar2 = stats.norm.cdf(12.53,12.5,sigma2) - stats.norm.cdf(12.47,12.5,sigma2)
print("Onbruikbaar (σ²=0.0002):", round(1-bruikbaar2,4))    # ≈ 0.0339 (3.39%)
```

#### Opgave — Normale verdeling (examen)
#### Vraag
Punten ~ Nor(60.7, 12.3). (1) Slaagdrempel 40: welk % faalt? (2) Onderscheiding voor de beste 10%: minimale punt?

```python
print("Faalt:", round(stats.norm.cdf(40,60.7,12.3)*100,2), "%")   # ≈ 4.62 %
print("Onderscheiding vanaf:", round(stats.norm.ppf(0.90,60.7,12.3),2))  # ≈ 76.46
```

#### Opgave — Normale verdeling (chocolade)
#### Vraag
σ = 0.7 g, label 50 g. Hoogstens 0.5% van de repen mag minder dan 50 g bevatten. Op welk gemiddelde moet de machine ingesteld worden?

```python
# We zoeken μ zó dat P(X < 50) = 0.005, met X ~ Nor(μ, 0.7)
mu = 50 - stats.norm.ppf(0.005)*0.7
print("Stel het gemiddelde in op:", round(mu,3), "g")        # ≈ 51.803 g
```

#### Opgave — CLT (bagage in vliegtuig)
#### Vraag
43 koffers, populatiegemiddelde 12.1 kg, σ = 3.8 kg. Wat is P(gemiddelde < 11 kg)?

```python
n, mu, sigma = 43, 12.1, 3.8
se = sigma/np.sqrt(n)                       # standaardfout
print(round(stats.norm.cdf(11, mu, se), 4)) # 0.0288
```

#### Opgave — CLT (reistijd spitsuur)
#### Vraag
Reistijden: gemiddelde 23.3 min, σ = 8.9 min. Bereken P(steekproefgemiddelde van 60 < 25 min).

```python
n, mu, sigma = 60, 23.3, 8.9
se = sigma/np.sqrt(n)
print(round(stats.norm.cdf(25, mu, se), 4)) # 0.9305
```

#### Opgave — Betrouwbaarheidsinterval (cacaobonen)
#### Vraag
Gewichten ~ normaal met σ = 6 g. Steekproef: [758, 748, 749, 752, 757, 760, 751, 745, 759, 761]. Bereken een 95%-betrouwbaarheidsinterval voor het gemiddelde gewicht.

```python
data = np.array([758,748,749,752,757,760,751,745,759,761])
n = len(data); xbar = data.mean(); sigma = 6
# σ gekend → z-verdeling
z = stats.norm.ppf(0.975)
marge = z * sigma/np.sqrt(n)
print(xbar - marge, xbar + marge)   # [750.281, 757.719]
```

```text
Opmerking: met de gegeven populatie-σ = 6 levert de z-methode [750.281, 757.719].
Het in de cursus vermelde antwoord [749.939, 758.061] ontstaat wanneer men in
plaats daarvan de steekproef-standaardafwijking (s ≈ 6.55) gebruikt. Omdat σ
hier expliciet gekend is, is de z-methode hierboven de methodologisch correcte.
```

### Lab 3.01

#### Exercise 1 (Betrouwbaarheid van back-ups)
#### Vraag
Een harde schijf heeft 1% kans op crash; er zijn twee back-ups met elk 2% kans op crash. Componenten werken onafhankelijk. Data gaat enkel verloren als alle drie crashen. Wat is de kans dat de data NIET verloren gaat?

```python
p_verlies = 0.01 * 0.02 * 0.02   # alle drie crashen (onafhankelijk)
print("P(data niet verloren) =", 1 - p_verlies)   # 0.999996
```

#### Exercise 2 (Gebroken eieren)
#### Vraag
X = aantal gebroken eieren in een doos van 12, met verdeling P(0)=0.65, P(1)=0.20, P(2)=0.10, P(3)=0.04, P(4)=? Beantwoord de vijf deelvragen.

```python
P = [0.65, 0.20, 0.10, 0.04]
P4 = 1 - sum(P)
print("1. P(4)             =", round(P4,2))                 # 0.01
print("2. P(max 2 gebroken)=", 0.65+0.20+0.10)              # 0.95
# minstens 9 heel (van 12) = hoogstens 3 gebroken
print("3. P(≥9 heel)       =", 0.65+0.20+0.10+0.04)         # 0.99
# exact 9 heel = exact 3 gebroken
print("4. P(exact 9 heel)  =", 0.04)                        # 0.04
# palet van 800 dozen
E = sum(i*p for i,p in enumerate([0.65,0.20,0.10,0.04,0.01]))
print("5. Verwacht gebroken =", E*800)                      # ≈ 448 eieren
```

#### Exercise 3 (Verkeerslichten)
#### Vraag
E = stoppen aan eerste licht, F = stoppen aan tweede. P(E)=0.4, P(F)=0.3, P(E ∩ F)=0.15. Beantwoord de zes deelvragen.

```python
PE, PF, PEF = 0.4, 0.3, 0.15
print("1. Onafhankelijk? P(E)·P(F) =", PE*PF, "≠ P(E∩F) =", PEF, "→ NIET onafhankelijk")
print("2. P(minstens één) =", PE + PF - PEF)     # 0.55
print("3. P(geen van beide) =", 1-(PE+PF-PEF))   # 0.45
print("4. P(minstens eerste) = P(E) =", PE)      # 0.40
print("5. P(precies één) =", PE + PF - 2*PEF)    # 0.40
print("6. P(enkel eerste) = P(E) - P(E∩F) =", PE - PEF)  # 0.25
```

#### Exercise 4 (Briefjes met bedragen)
#### Vraag
Vijf briefjes: €1, €1, €1, €10, €25. Een speler kiest er twee en wint het grootste bedrag. X = gewonnen bedrag. (1) Bepaal de kansverdeling, (2) plot ze, (3) bereken E(X).

```python
import itertools
papers = [1, 1, 1, 10, 25]
combos = list(itertools.combinations(range(5), 2))   # 10 gelijke kansen
wins   = [max(papers[i], papers[j]) for i,j in combos]
from collections import Counter
c = Counter(wins); tot = len(combos)

# 1. Kansverdeling
for bedrag in [1,10,25]:
    print(f"P(X={bedrag}) = {c[bedrag]}/{tot} = {c[bedrag]/tot}")
# P(X=1)=3/10=0.3 ; P(X=10)=3/10=0.3 ; P(X=25)=4/10=0.4

# 2. Visualisatie
X = [1, 10, 25]
Prob_X = [c[1]/tot, c[10]/tot, c[25]/tot]
sns.barplot(x=X, y=Prob_X)
plt.xlabel("X (gewonnen bedrag)"); plt.ylabel("P(X)")
plt.show()

# 3. Verwachtingswaarde
EX = sum(b*c[b] for b in c)/tot
print("E(X) =", EX)   # 13.3 euro
```

#### Exercise 5 (Online kansspel)
#### Vraag
p(x) = k·x voor x = 1,...,5. (1) Bepaal k. (2) P(hoogstens €3)? (3) Verwachte winst bij 100 keer spelen?

```python
# 1. Σ k·x = k(1+2+3+4+5) = 15k = 1
k = 1/15
print("1. k =", k)                                # 0.0667

# 2. P(X ≤ 3) = k(1+2+3) = 6/15
print("2. P(X ≤ 3) =", k*(1+2+3))                 # 0.4

# 3. E(X) = Σ x·k·x = k·Σ x²
EX = k * sum(x**2 for x in range(1,6))
print("3. E(X) =", round(EX,4), "→ 100×:", round(EX*100,2), "euro")  # ≈ 366.67
```

#### Exercise 6 (Kansen in de standaardnormale verdeling)
#### Vraag
Bereken de gegeven kansen in Z ~ Nor(0,1) en vergelijk met de antwoorden in de tabel.

```python
m, s = 0, 1
print("1.  P(Z < 1.33)        =", round(stats.norm.cdf(1.33, m, s), 3))                 # 0.908
print("2.  P(Z > 1.33)        =", round(1 - stats.norm.cdf(1.33, m, s), 3))             # 0.092
print("3.  P(Z < -1.33)       =", round(stats.norm.cdf(-1.33, m, s), 3))                # 0.092
print("4.  P(Z > -1.33)       =", round(1 - stats.norm.cdf(-1.33, m, s), 3))            # 0.908
print("5.  P(Z < 0.45)        =", round(stats.norm.cdf(0.45, m, s), 3))                 # 0.674
print("6.  P(Z > -1.05)       =", round(1 - stats.norm.cdf(-1.05, m, s), 3))            # 0.853
print("7.  P(Z < 0.65)        =", round(stats.norm.cdf(0.65, m, s), 3))                 # 0.742
print("8.  P(-0.45 < Z < 1.20)=", round(stats.norm.cdf(1.20,m,s)-stats.norm.cdf(-0.45,m,s),3))  # 0.559
print("9.  P(-1.35 < Z < -0.10)=", round(stats.norm.cdf(-0.10,m,s)-stats.norm.cdf(-1.35,m,s),3))# 0.372
print("10. P(-2.10 < Z < -0.90)=", round(stats.norm.cdf(-0.90,m,s)-stats.norm.cdf(-2.10,m,s),3))# 0.166
```

#### Exercise 7 (Kansdichtheid plotten)
#### Vraag
Plot de kansdichtheidsfunctie (pdf) en cumulatieve verdelingsfunctie (cdf) van Nor(μ=2.5, σ=1.5). Wat is de oppervlakte onder de pdf tussen x=0.5 en x=4? (Antwoord = 0.750)

```python
mu, sigma = 2.5, 1.5
x = np.linspace(mu - 4*sigma, mu + 4*sigma, 400)

fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12,4))
ax1.plot(x, stats.norm.pdf(x, mu, sigma)); ax1.set_title("pdf")
ax2.plot(x, stats.norm.cdf(x, mu, sigma)); ax2.set_title("cdf")
plt.show()

# Oppervlakte tussen 0.5 en 4 = P(0.5 < X < 4)
opp = stats.norm.cdf(4, mu, sigma) - stats.norm.cdf(0.5, mu, sigma)
print("Oppervlakte =", round(opp, 3))    # 0.750
```

#### Exercise 8 (Theoretische vs. werkelijke kansdichtheid)
#### Vraag
Genereer 25 willekeurige standaardnormale getallen, plot een histogram met dichtheidslijn én de theoretische pdf. Doe hetzelfde voor 250 en 2500. Merk op hoe de empirische dichtheid de theoretische benadert naarmate de steekproef groeit.

```python
x_theo = np.linspace(-4, 4, 200)
for n in [25, 250, 2500]:
    sample = np.random.normal(0, 1, n)
    plt.figure()
    sns.histplot(sample, stat="density", kde=True, bins=20)
    plt.plot(x_theo, stats.norm.pdf(x_theo), 'r--', label="theoretisch")
    plt.title(f"n = {n}"); plt.legend()
    plt.show()
# Hoe groter n, hoe beter het histogram de theoretische normale curve volgt.
```

#### Exercise 9 (Typesnelheid)
#### Vraag
Nettotypesnelheid ~ Nor(μ=60 wpm, σ=15 wpm). (1) P(≤ 60)? (2) P(45 < X < 90)? (3) Verrast door iemand > 105? (4) Welke snelheden vallen bij de traagste 20%?

```python
mu, sigma = 60, 15
print("1. P(≤ 60)        =", stats.norm.cdf(60, mu, sigma))                         # 0.5
print("2. P(45 < X < 90) =", round(stats.norm.cdf(90,mu,sigma)-stats.norm.cdf(45,mu,sigma),4)) # 0.8186
print("3. P(> 105)       =", round(1-stats.norm.cdf(105,mu,sigma),4))               # 0.0013 → zeer zeldzaam, dus verrassend
print("4. Grens traagste 20% :", round(stats.norm.ppf(0.20, mu, sigma),2))          # ≈ 47.38 wpm of trager
```

#### Exercise 10 (Twee kansspelen — exacte kans)
#### Vraag
Bereken de exacte winkans van: (a) minstens één zes bij 4 worpen met één dobbelsteen; (b) minstens één "dubbel zes" bij 24 worpen met twee dobbelstenen. Vergelijk met de simulatie uit de slides.

```python
# Complementregel: P(minstens één) = 1 - P(geen enkele)
spel1 = 1 - (5/6)**4
spel2 = 1 - (35/36)**24
print("Spel 1: P(≥ één zes)        =", round(spel1, 4))   # 0.5177  > 0.5 → winst op termijn
print("Spel 2: P(≥ één dubbele zes)=", round(spel2, 4))   # 0.4914  < 0.5 → verlies op termijn
```

#### Exercise 11 (Verwachting van X − a)
#### Vraag
X = som van de ogen bij twee dobbelstenen (symmetrisch rond 7). (1) Toon E(X)=7. (2) Je betaalt €8 om te spelen; Y = winst. Verband Y↔X en E(Y)? (3) Algemeen verband tussen E(X − a) en E(X)?

```python
x = np.arange(2, 13)
p = np.array([1,2,3,4,5,6,5,4,3,2,1]) / 36
EX = np.sum(x*p)
print("1. E(X) =", EX)                       # 7.0

# 2. Y = X - 8  →  E(Y) = E(X) - 8 = 7 - 8 = -1
print("2. E(Y) = E(X) - 8 =", EX - 8)        # -1 (gemiddeld verlies van €1)
```

```text
3. Algemeen geldt de lineariteit van de verwachting:
       E(X - a) = E(X) - a
   Aftrekken van een constante a verschuift de verwachting met diezelfde a.
```

#### Exercise 12 (Variantie van X/a)
#### Vraag
X = som van de ogen bij twee dobbelstenen. (1) Bereken Var(X). (2) Je winst Y is de helft van de som; verband Y↔X en Var(Y)? (3) Algemeen verband tussen Var(X/a) en Var(X)?

```python
x = np.arange(2, 13)
p = np.array([1,2,3,4,5,6,5,4,3,2,1]) / 36
EX  = np.sum(x*p)
VarX = np.sum((x-EX)**2 * p)
print("1. Var(X) =", round(VarX, 4))         # 5.8333 (= 35/6)

# 2. Y = X/2  →  Var(Y) = Var(X)/4
print("2. Var(Y) = Var(X)/4 =", round(VarX/4, 4))   # 1.4583
```

```text
3. Voor een schaalfactor geldt:
       Var(X / a) = Var(X) / a²
   Delen door a deelt de variantie door a² (de standaardafwijking door |a|).
```

#### Exercise 13 (Verwachting en variantie van (X−μ)/σ)
#### Vraag
Bepaal, met de resultaten van de vorige twee oefeningen, de verwachting en variantie van de gestandaardiseerde variabele Z = (X − μ_X)/σ_X.

```text
Combineer E(X - a) = E(X) - a met Var(X/a) = Var(X)/a², waarbij a = μ_X resp. σ_X:

  E(Z) = E[(X - μ_X)/σ_X] = (E(X) - μ_X)/σ_X = (μ_X - μ_X)/σ_X = 0
  Var(Z) = Var[(X - μ_X)/σ_X] = Var(X)/σ_X² = σ_X²/σ_X² = 1

Een gestandaardiseerde kansvariabele heeft dus altijd verwachting 0 en variantie 1
(dit is precies wat de z-score doet).
```

### Lab 3.02

#### Exercise 1 (Cholesterol)
#### Vraag
Vrouwen 20–29 jaar: gemiddeld cholesterol 183 mg/dl, σ = 36. Steekproef van 81 vrouwen. (1) Plot de verdeling van x̄. (2) P(x̄ < 185)? (3) P(175 < x̄ < 185)? (4) P(x̄ > 190)?

```python
mu, sigma, n = 183, 36, 81
se = sigma/np.sqrt(n)        # standaardfout (CLT) = 36/9 = 4

# 1. Verdeling van het steekproefgemiddelde
x = np.linspace(mu - 4*se, mu + 4*se, 300)
plt.plot(x, stats.norm.pdf(x, mu, se))
plt.title("Verdeling van het steekproefgemiddelde x̄")
plt.xlabel("x̄"); plt.show()

# 2-4
print("2. P(x̄ < 185)       =", round(stats.norm.cdf(185, mu, se), 3))                  # 0.691
print("3. P(175 < x̄ < 185) =", round(stats.norm.cdf(185,mu,se)-stats.norm.cdf(175,mu,se),3))  # 0.669
print("4. P(x̄ > 190)       =", round(1 - stats.norm.cdf(190, mu, se), 3))              # 0.040
```

#### Exercise 2
#### Vraag
Steekproef van 64 uit een populatie met onbekende verdeling, maar μ = 20 en σ = 16. (1) Plot de verdeling van x̄. (2) z-score voor x̄₁ = 15.5. (3) z-score voor x̄₂ = 23. (4) P(16 < x̄ < 22)?

```python
mu, sigma, n = 20, 16, 64
se = sigma/np.sqrt(n)        # 16/8 = 2

# 1. Plot (CLT: x̄ is bij benadering normaal verdeeld, ook al is de populatie dat niet)
x = np.linspace(mu - 4*se, mu + 4*se, 300)
plt.plot(x, stats.norm.pdf(x, mu, se))
plt.title("Verdeling van x̄ (CLT)"); plt.xlabel("x̄"); plt.show()

# 2-3. z-scores
print("2. z(15.5) =", (15.5 - mu)/se)    # -2.25
print("3. z(23)   =", (23 - mu)/se)      #  1.5
# 4.
print("4. P(16 < x̄ < 22) =", round(stats.norm.cdf(22,mu,se)-stats.norm.cdf(16,mu,se),3))  # 0.819
```

#### Exercise 3 (Waarom geen enquête voor de bachelorproef?)
#### Vraag
Waarom raden we studenten vaak af om voor hun bachelorproef een enquête te houden? Bedenk redenen op basis van wat je leerde over steekproeven en de centrale limietstelling.

```text
- Steekproefgrootte: voor betrouwbare uitspraken (smalle betrouwbaarheidsintervallen,
  voldoende power) is vaak een grote n nodig. De standaardfout daalt slechts met
  √n, dus om de fout te halveren heb je 4× zoveel respondenten nodig. Studenten
  halen zelden voldoende, kwalitatieve respondenten binnen de beschikbare tijd.

- Representativiteit / selectiebias: een gemakkelijkheidssteekproef (vrienden,
  sociale media, medestudenten) is geen aselecte steekproef uit de doelpopulatie.
  De CLT garandeert correcte conclusies enkel bij een (a)selecte steekproef; bij
  vertekening helpt een grote n niet — je meet dan systematisch het verkeerde.

- Non-respons en meetfouten: lage responsgraad, sociaal wenselijke antwoorden en
  slecht geformuleerde vragen vertekenen de resultaten verder.

- Beter alternatief: werk met bestaande, grotere en gevalideerde datasets, of met
  een goed opgezet experiment, in plaats van een kleine zelf afgenomen enquête.
```

### Lab 3.03

#### Exercise 1 (rlanders — betrouwbaarheidsintervallen)
#### Vraag
Laad `data/rlanders.csv`, kolom `Money`. We nemen aan dat de waarden normaal verdeeld zijn rond een onbekende μ, met gekende σ = 98. Bereken de gevraagde betrouwbaarheidsintervallen.

```python
df = pd.read_csv('https://raw.githubusercontent.com/HoGentTIN/dsai-en-labs/main/data/rlanders.csv').set_index(['ID'])
money = df['Money']
n = len(money)
xbar = money.mean()
sigma = 98

def ci_z(xbar, sigma, n, conf):
    z = stats.norm.ppf(1 - (1-conf)/2)
    marge = z * sigma/np.sqrt(n)
    return xbar - marge, xbar + marge

# 99%-BI (σ gekend → z-verdeling)
print("99% BI:", ci_z(xbar, sigma, n, 0.99))     # ≈ [484.191, 516.121]

# 95%-BI (σ gekend → z-verdeling)
print("95% BI:", ci_z(xbar, sigma, n, 0.95))     # ≈ [488.008, 512.304]
```

```python
# σ ONBEKEND → gebruik steekproef-std s en de t-verdeling (n-1 vrijheidsgraden)
s = money.std()    # ddof=1 standaard in pandas
t = stats.t.ppf(0.975, df=n-1)
marge = t * s/np.sqrt(n)
print("95% BI (σ onbekend):", (xbar - marge, xbar + marge))   # ≈ [487.319, 512.993]
```

```python
# Enkel de eerste 25 observaties, σ onbekend → t-verdeling met 24 vrijheidsgraden
sub = money.iloc[:25]
n2 = len(sub); xbar2 = sub.mean(); s2 = sub.std()
t2 = stats.t.ppf(0.975, df=n2-1)
marge2 = t2 * s2/np.sqrt(n2)
print("95% BI (eerste 25):", (xbar2 - marge2, xbar2 + marge2))  # ≈ [450.291, 536.669]
```

#### Exercise 2 (Eigenschappen van betrouwbaarheidsintervallen)
#### Vraag
(1) Hoe bereken je de boven- en ondergrens van een 95%- en 99%-BI? (2) Is een 99%-BI breder/smaller/even breed als een 95%-BI? Waarom? (3) Hoe ziet een 100%-BI eruit?

```text
1. Grenzen = x̄ ± (kritieke waarde) · standaardfout.
   - σ gekend:    x̄ ± z · σ/√n        met z = 1.96 (95%)  of  z = 2.576 (99%)
   - σ onbekend:  x̄ ± t · s/√n        met t uit de t-verdeling (n-1 vrijheidsgraden)

2. Een 99%-BI is BREDER dan een 95%-BI.
   Om met meer zekerheid (99% i.p.v. 95%) de populatieparameter te "vangen", moet
   de kritieke waarde groter zijn (2.576 > 1.96), dus wordt de marge — en daarmee
   het interval — breder. Meer betrouwbaarheid kost dus aan precisie.

3. Een 100%-BI zou de hele reële rechte zijn: ]-∞, +∞[.
   Om absolute zekerheid (100%) te garanderen, moet je álle mogelijke waarden
   omvatten. Zo'n interval is oneindig breed en dus nutteloos — er valt geen
   informatie uit af te leiden.
```

### Lab 3.04

#### Exercise 1 (Marinerekruten)
#### Vraag
Lengtes van rekruten zijn normaal verdeeld met gemiddeld 69 inch. Test of het gemiddelde dit jaar hoger ligt dan 69 inch (steekproef van 64, data in `recruten.csv`). (1) Formuleer H₀ en H₁. (2) Bereken de p-waarde. (3) Trek een besluit.

```python
df = pd.read_csv('https://raw.githubusercontent.com/HoGentTIN/dsai-labs/main/data/recruten.csv',
                 sep=";", decimal=",")
lengtes = df.iloc[:, 0]          # de lengtekolom
n = len(lengtes)
xbar = lengtes.mean()
s = lengtes.std()
mu0 = 69

# 1. H0: μ = 69 (gemiddelde is niet hoger)   H1: μ > 69 (rechtszijdige toets)
# 2. p-waarde (σ onbekend → t-toets, of z-toets als σ gekend; hier t-toets)
t_stat = (xbar - mu0) / (s/np.sqrt(n))
p = 1 - stats.t.cdf(t_stat, df=n-1)
print("p-waarde =", p)           # ≈ 0.000017  (0.0017 %)
```

```text
3. Besluit: de p-waarde (≈ 0.0017 %) is veel kleiner dan α = 5 %.
   We verwerpen H0. Het is vrijwel onmogelijk dat het werkelijke gemiddelde nog
   69 inch zou zijn gegeven deze steekproef. De gemiddelde lengte van de rekruten
   van dit jaar is dus significant groter dan 69 inch.
```

#### Exercise 2 (Regenval — kritiek gebied & fouten type I/II)
#### Vraag
Regenval ~ Nor(μ=82.3, σ=15.3). Vermoeden: opwarming verhoogt het gemiddelde. Toets op 5% met het gemiddelde van de komende 5 jaar. (1) Verwerpingsgebied? (2) P(fout type I)? (3) P(fout type II) als μ in werkelijkheid 105 is?

```python
mu0, sigma, n, alpha = 82.3, 15.3, 5, 0.05
se = sigma/np.sqrt(n)

# 1. Rechtszijdige toets: verwerp H0 als x̄ > kritieke waarde g
g = stats.norm.ppf(1 - alpha, mu0, se)
print("1. Verwerpingsgebied: x̄ >", round(g, 4))     # ≈ 96.8869, dus ]96.8869, +∞[

# 2. Fout type I = α (H0 ten onrechte verwerpen)
print("2. P(type I) = α =", alpha)                   # 0.05 = 5 %

# 3. Fout type II (β): H0 niet verwerpen terwijl μ in werkelijkheid 105 is
beta = stats.norm.cdf(g, 105, se)
print("3. P(type II) = β =", round(beta, 4))         # ≈ 0.1507 (15.07 %)
```

#### Exercise 3 (Duur medische ingreep)
#### Vraag
Vroeger: gemiddelde duur 34.2 min, σ = 2.6. Nieuwe methode (steekproef 50) geeft x̄ = 33.5 min. Toets op 5% of de gemiddelde duur gedaald is.

```python
mu0, sigma, n, xbar, alpha = 34.2, 2.6, 50, 33.5, 0.05
se = sigma/np.sqrt(n)

# H0: μ = 34.2   H1: μ < 34.2 (linkszijdige z-toets, σ gekend)
p = stats.norm.cdf(xbar, mu0, se)
g = stats.norm.ppf(alpha, mu0, se)
print("p-waarde =", round(p, 5))           # 0.02847  → p < α : verwerp H0
print("kritieke waarde g =", round(g, 3))  # 33.595   → x̄ = 33.5 < g : verwerp H0
# Besluit: de gemiddelde duur is significant gedaald.
```

#### Exercise 4 (Stopafstand vrachtwagen)
#### Vraag
Stopafstand is normaal verdeeld; maximaal toegelaten = 9.15 m. Gemeten: [9.78, 9.33, 9.57, 9.26, 9.45, 9.72]. Suggereren deze waarden dat de werkelijke stopafstand groter is dan 9.15 m? Toets op 1%. Waarom hier 1% i.p.v. 5%?

```python
data = np.array([9.78, 9.33, 9.57, 9.26, 9.45, 9.72])
mu0, alpha = 9.15, 0.01
n = len(data); xbar = data.mean(); s = data.std(ddof=1)

# σ onbekend, kleine steekproef → t-toets, rechtszijdig
# H0: μ = 9.15   H1: μ > 9.15
t_stat = (xbar - mu0)/(s/np.sqrt(n))
p = 1 - stats.t.cdf(t_stat, df=n-1)
g = stats.t.ppf(1-alpha, df=n-1)*(s/np.sqrt(n)) + mu0
print("steekproefgemiddelde =", round(xbar,4))   # 9.5183
print("p-waarde =", round(p,5))                   # 0.00381 → p < α : verwerp H0
print("kritieke waarde g =", round(g,3))          # 9.437  → x̄ = 9.518 > g : verwerp H0
```

```text
Besluit: p ≈ 0.00381 < α = 0.01, dus we verwerpen H0. De werkelijke stopafstand
is significant groter dan 9.15 m.

Waarom 1% i.p.v. 5%? De stopafstand is veiligheidskritisch. Een fout type I (ten
onrechte besluiten dat de afstand te groot is) heeft hier zware gevolgen, dus
kiezen we een strenger significantieniveau (kleinere α) zodat we pas bij sterk
bewijs concluderen dat de norm overschreden wordt.
```

#### Exercise 5 (rlanders, herzien — toetsen per geslacht)
#### Vraag
`Money` (×100$): aanname μ = 500, σ = 98. Onderzoek mannen en vrouwen apart (`Gender`). Visualiseer en toets op α = 5%: (1) ligt het gemiddelde van mannen significant hoger? (2) van vrouwen significant lager? (3) bepaal het aanvaardingsgebied voor de volledige steekproef (tweezijdig).

```python
df = pd.read_csv('https://raw.githubusercontent.com/HoGentTIN/dsai-en-labs/main/data/rlanders.csv').set_index(['ID'])
mu0, sigma, alpha = 500, 98, 0.05

# Visualisatie: KDE van Money, volledig en opgesplitst per Gender
sns.kdeplot(data=df, x='Money', label='alle')
sns.kdeplot(data=df, x='Money', hue='Gender')
plt.axvline(mu0, color='black', linestyle='--', label='aangenomen μ')
plt.axvline(df['Money'].mean(), color='green', linestyle=':', label='steekproefgem.')
plt.legend(); plt.show()

def toets_eenzijdig(sample, kant):
    n = len(sample); xbar = sample.mean(); se = sigma/np.sqrt(n)
    if kant == 'rechts':
        g = stats.norm.ppf(1-alpha, mu0, se); p = 1 - stats.norm.cdf(xbar, mu0, se)
    else:
        g = stats.norm.ppf(alpha,   mu0, se); p = stats.norm.cdf(xbar, mu0, se)
    return round(xbar,3), round(g,3), round(p,4)

mannen  = df[df['Gender']=='Male']['Money']
vrouwen = df[df['Gender']=='Female']['Money']

print("1. Mannen (rechtszijdig):", toets_eenzijdig(mannen, 'rechts'))
#    x̄ ≈ 507.535, g ≈ 511.456, p ≈ 0.1396  → p > α : H0 NIET verwerpen (niet significant hoger)
print("2. Vrouwen (linkszijdig):", toets_eenzijdig(vrouwen, 'links'))
#    x̄ ≈ 472.058, g ≈ 477.646, p ≈ 0.0199  → p < α : H0 verwerpen (significant lager)

# 3. Aanvaardingsgebied volledige steekproef (tweezijdig)
n = len(df); se = sigma/np.sqrt(n)
onder = stats.norm.ppf(alpha/2,   mu0, se)
boven = stats.norm.ppf(1-alpha/2, mu0, se)
print("3. Aanvaardingsgebied:", (round(onder,3), round(boven,3)))   # ≈ [487.852, 512.148]
```

```text
Besluit:
1. Het gemiddelde inkomen van de mannen (507.535) ligt visueel hoger, maar is
   NIET significant hoger dan verwacht (p ≈ 0.1396 > 0.05). H0 niet verwerpen.
2. Het gemiddelde inkomen van de vrouwen (472.058) is WEL significant lager
   (p ≈ 0.0199 < 0.05). H0 verwerpen.
3. Het tweezijdige aanvaardingsgebied is [487.852, 512.148].
```

#### Exercise 6 (Bindend studieadvies — BSA)
#### Vraag
Vóór BSA: 44 studiepunten/jaar, σ = 6.2. Na invoering: steekproef van 72 studenten met gemiddeld 46.2 studiepunten. (1) Plot. (2) Toets of het slaagrendement verbeterde; welke toets, H₀ en H₁? (3) Kritieke waarde bij α = 2.5%. (4) p-waarde. (5) Betekenis van α = 2.5%.

```python
mu0, sigma, n, xbar, alpha = 44, 6.2, 72, 46.2, 0.025
se = sigma/np.sqrt(n)

# 1. Plot: verdeling van het steekproefgemiddelde + lijnen voor μ en x̄
x = np.linspace(mu0 - 4*se, mu0 + 4*se, 300)
plt.plot(x, stats.norm.pdf(x, mu0, se))
plt.axvline(mu0, color='black', linestyle='--', label='populatiegem.')
plt.axvline(xbar, color='red', linestyle=':', label='steekproefgem.')
plt.legend(); plt.show()

# 2. σ gekend, grote steekproef → z-toets, rechtszijdig
#    H0: μ = 44   H1: μ > 44
# 3. Kritieke waarde
g = stats.norm.ppf(1-alpha, mu0, se)
print("kritieke waarde g =", round(g, 1))     # ≈ 45.4  (en x̄ = 46.2 > g → verwerp H0)
# 4. p-waarde
p = 1 - stats.norm.cdf(xbar, mu0, se)
print("p-waarde =", round(p, 4))               # ≈ 0.0013  < α = 0.025 → verwerp H0
```

```text
2. Besluit: x̄ = 46.2 ligt voorbij de kritieke waarde g ≈ 45.4, dus binnen het
   verwerpingsgebied. We verwerpen H0 → het BSA verhoogt het slaagrendement.

4. p ≈ 0.0013 < α = 0.025: de p-waarde is kleiner dan het significantieniveau,
   dus we verwerpen H0.

5. α = 2.5% is de kans op een fout type I: de kans dat we H0 ten onrechte
   verwerpen. Er is dus 2.5% kans dat we foutief besluiten dat het slaagrendement
   gestegen is, terwijl dat in werkelijkheid niet zo is.
```
