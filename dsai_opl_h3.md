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

Geen van beide is het complement van de unie: 1 - 9/10 = 1/10 = 25/250.

```
#### Oefening 3 (autosurvey)
#### Opgave:
Gegeven tabel met kleuren en types autos.

Saloon	Hatchback
Silver	65	59
Black	27	22
Other	16	19
Kies willekeurig een auto. Bereken de kans dat de gekozen auto:

een zilveren hatchback is

een hatchback is

een hatchback is, gegeven dat hij zilver is.

Antwoord:
```text

Totaal aantal autos: 65+59+27+22+16+19 = 208

Zilveren hatchback: 59 - kans = 59/208  0,284

Totaal hatchbacks: 59+22+19 = 100 - kans = 100/208  0,481

Voorwaardelijke kans: P(hatchback | zilver) = (aantal zilveren hatchbacks) / (totaal zilver) = 59 / (65+59) = 59/124  0,476

```
Uitleg:
```text
We gebruiken de definitie van voorwaardelijke kans: P(A|B) = P(A en B)/P(B). Hier is B = zilver, A = hatchback. De aantallen staan in de tabel.

```
#### Oefening 4 (golfer toernooien)
#### Opgave:
Een golfer schat de kans dat hij het eerste toernooi wint op 0,6, het tweede op 0,4 en beide op 0,35.

Bereken de kans dat hij geen van beide wint.

Toon aan dat winnen van het eerste en winnen van het tweede niet onafhankelijk zijn.

Leg uit waarom het verrassend zou zijn als deze gebeurtenissen onafhankelijk waren.

Antwoord:
```text

P(geen van beide) = 1 - P(1e en 2e) = 1 - (0,6 + 0,4 - 0,35) = 1 - 0,65 = 0,35

Onafhankelijkheid zou betekenen P(1e en 2e) = 0,6 x 0,4 = 0,24, maar de gegeven waarde is 0,35, dus niet onafhankelijk.

Toernooien in opeenvolgende weken: de uitslag van het eerste kan het zelfvertrouwen of de vorm beinvloeden, dus de kansen zijn afhankelijk.

```
Uitleg:
```text

Unie: P(1e en 2e) = 0,6+0,4-0,35 = 0,65. Complement geeft 0,35.

Voor onafhankelijkheid moet het product van de kansen gelijk zijn aan de kans op doorsnede. Dat is hier niet het geval.

Afhankelijkheid is aannemelijk door verband in sportprestaties.

```
#### Oefening 5 (Engels, Geschiedenis, Biologie)
#### Opgave:
60% nam geschiedenis, 30% biologie, 10% beide. Kies willekeurig een student uit de Engelse studenten.

Bereken de kans dat de student geen van beide vakken nam.

Gegeven dat de student precies een van beide vakken nam, bereken de kans dat het geschiedenis was.

Antwoord:
```text

P(geen van beide) = 1 - (0,6 + 0,3 - 0,1) = 1 - 0,8 = 0,2

P(geschiedenis | precies een) = P(alleen geschiedenis) / P(precies een) = (0,6 - 0,1) / (0,6+0,3-2*0,1) = 0,5 / 0,7  0,714

```
Uitleg:
```text

Unie = 0,8. Complement = 0,2.

Precies een = (0,6-0,1)+(0,3-0,1) = 0,5+0,2=0,7. Alleen geschiedenis = 0,5.

Voorwaardelijke kans = 0,5/0,7.

```
#### Oefening 6 (leeftijd en geslacht)
#### Opgave:
Tabel:

Onder 35	35 en ouder
Man	345	380
Vrouw	362	472
Kies willekeurig een persoon. Laat M = man, Y = onder 35.

Bereken P(M)

Bereken P(M en Y)

Zijn M en Y onafhankelijk?

Gegeven dat de persoon onder 35 is, bereken de kans op vrouw.

Antwoord:
```text

Totaal = 345+380+362+472 = 1559

P(M) = (345+380)/1559 = 725/1559  0,465

P(M en Y) = 345/1559  0,221

Onafhankelijk? P(M)-P(Y) = (725/1559)-((345+362)/1559) = (725/1559)-(707/1559)  0,210  0,221 - niet onafhankelijk.

P(vrouw | Y) = 362/(345+362) = 362/707  0,512

```
Uitleg:
```text

Onafhankelijkheidstoets: P(M|Y) = 345/707  0,488, P(M)  0,465 - niet gelijk, dus afhankelijk.

```
#### Oefening 7 (kansverdeling van n dobbelsteen)
#### Opgave:
Wat is de kans op elke uitkomst bij het werpen van een eerlijke dobbelsteen? Vul tabel in.

Antwoord:
```text

x	1	2	3	4	5	6
P(X=x)	1/6	1/6	1/6	1/6	1/6	1/6
```
Uitleg:
```text
Een eerlijke dobbelsteen heeft zes gelijkwaardige uitkomsten, elke kans is 1/6.

```
#### Oefening 8 (kansverdeling van som van twee dobbelstenen)
#### Opgave:
Wat is de kans op elke uitkomst (som) bij het werpen van twee dobbelstenen?

Antwoord:
```text

x	2	3	4	5	6	7	8	9	10	11	12
P(X=x)	1/36	2/36	3/36	4/36	5/36	6/36	5/36	4/36	3/36	2/36	1/36
```
Uitleg:
```text
Er zijn 36 mogelijke paren. Het aantal manieren om een som te krijgen:
som 2: (1,1) - 1 manier; som 3: (1,2),(2,1) - 2;  som 7: (1,6),(2,5), - 6; symmetrisch daarna.

```
#### Oefening 9 (discrete stochastische variabelen)
#### Opgave:
Bepaal of de volgende tabellen een discrete stochastische variabele voorstellen.

X: P(1)=2/10, P(2)=3/10, P(3)=4/10, P(4)=3/10, P(5)=2/10 - som = 14/10 > 1, dus geen.

Y: P(-2)=2/10, P(-1)=3/10, P(0)=1/10, P(1)=3/10, P(2)=1/10 - som = 10/10 = 1, alle kansen niet-negatief - wel.

Z: P(5)=1/2, P(4)=1/4, P(10)=1/8, P(15)=1/16, P(20)=1/16 - som = 1, wel.

W: P(1)=2/10, P(2)=3/10, P(3)=4/10, P(4)=3/10, P(5)='2/10 - negatieve kans, geen.

Uitleg:
```text
Een discrete stochastische variabele vereist dat alle kansen 0 zijn en som 1.

```
#### Oefening 10 (dobbelsteen  afgeleide variabelen)
#### Opgave:
Een eerlijke dobbelsteen wordt geworpen. Geef de kansverdeling voor:

X = score op de dobbelsteen

Y = 2 - score

Z = kwadraat van de score

W = 0 als score een deler is van 6, anders 1.

Antwoord:
```text

Zelfde als oefening 7.

Y: waarden 2,4,6,8,10,12 met elk kans 1/6.

Z: 1,4,9,16,25,36 met elk kans 1/6.

Delers van 6: 1,2,3,6 - W=0 voor deze, W=1 voor 4,5. Dus P(W=0)=4/6=2/3, P(W=1)=2/6=1/3.

```
Uitleg:
```text
Transformeer simpelweg de oorspronkelijke waarden.

```
#### Oefening 11 (som tussen 4 en 7)
#### Opgave:
Bij twee dobbelstenen: kans dat de som tussen 4 en 7 ligt (inclusief).

Antwoord:
```text
P(4  X  7) = P(4)+P(5)+P(6)+P(7) = 3/36+4/36+5/36+6/36 = 18/36 = 1/2.

```
Uitleg:
```text
Optellen van de kansen uit de tabel van oefening 8.

```
#### Oefening 12 (onbekende kansen a en k)
#### Opgave:

X: P(1)=2/10, P(2)=1/10, P(3)=3/10, P(4)=a, P(5)=1/10.

Bepaal a.

Bereken P(X  2).

Y: P(-2)=k, P(-1)=2k, P(0)=2k, P(1)=k.

Bepaal k.

Bereken P(Y  0).

Antwoord:
```text

Som = 2/10+1/10+3/10+a+1/10 = 7/10 + a = 1 - a = 3/10.
P(X  2) = 1/10+3/10+3/10+1/10 = 8/10 = 0,8.

Som = k+2k+2k+k = 6k = 1 - k = 1/6.
P(Y  0) = P(-2)+P(-1)+P(0) = 1/6+2/6+2/6 = 5/6.

```
Uitleg:
```text
Voor een kansverdeling moet de som van alle kansen 1 zijn.

```
#### Oefening 13 (bibliotheek  verwachting)
#### Opgave:
Aantal geleende boeken X met verdeling:

x	0	1	2	3	4	5
P	0,24	0,12	0,14	0,3	0,05	0,15
Bereken E(X).

Antwoord:
```text
E(X) = 0-0,24 + 1-0,12 + 2-0,14 + 3-0,3 + 4-0,05 + 5-0,15
= 0 + 0,12 + 0,28 + 0,9 + 0,2 + 0,75 = 2,25.

```
Uitleg:
```text
Verwachting = som van (waarde - kans).

```
#### Oefening 14 (e'mail  verwachting en variantie)
#### Opgave:
X: 48 of 52 e'mails per dag, elk met kans 0,5. Y: 0 e'mails met kans 0,5, 100 met kans 0,5.

Bepaal kansmassafuncties van X en Y.

Bereken E(X) en E(Y). Wat valt op?

Wie ervaart grotere variabiliteit?

Bereken Var(X) en Var(Y).

Antwoord:
```text

X: P(48)=0,5, P(52)=0,5. Y: P(0)=0,5, P(100)=0,5.

E(X)=0,5-48+0,5-52=50. E(Y)=0,5-0+0,5-100=50. Beide verwachtingen zijn gelijk.

Y heeft grotere spreiding.

Var(X) = 0,5-(48'50) + 0,5-(52'50) = 0,5-4 + 0,5-4 = 4.
Var(Y) = 0,5-(0'50) + 0,5-(100'50) = 0,5-2500 + 0,5-2500 = 2500.

```
Uitleg:
```text
Variantie meet de spreiding; bij Y liggen de waarden veel verder van het gemiddelde.

```
#### Oefening 15 (grootste en kleinste ogen bij twee dobbelstenen, en wachttijden)
#### Opgave:

Y = grootste ogen bij twee dobbelstenen. Bereken E(Y).

Y = kleinste ogen. Bereken E(Y) en Var(Y).

X = aantal klanten in winkel: P(0)=0,1; P(1)=0,25; P(2)=0,3; P(3)=0,25; P(4)=0,1.

Bereken gemiddelde en standaarddeviatie van X.

Wachttijd Y (minuten): 0,2,5,8,11 respectievelijk. Bereken gemiddelde wachttijd.

Antwoord (1):
Grootste ogen:
P(Y=k) = (2k'1)/36 voor k=1..6.
E(Y) =  k(2k'1)/36 = (11 + 23 + 35 + 47 + 59 + 611)/36 = (1+6+15+28+45+66)/36 = 161/36  4,472.

Antwoord (2):
Kleinste ogen:
P(Y=k) = (13'2k)/36 voor k=1..6.
E(Y) =  k(13'2k)/36 = (111 + 29 + 37 + 45 + 53 + 61)/36 = (11+18+21+20+15+6)/36 = 91/36  2,528.
Var(Y) = E(Y) - E(Y).
E(Y) = (111 + 29 + 37 + 45 + 53 + 61)/36 = (11+36+63+80+75+36)/36 = 301/36  8,361.
Var = 8,361 - (91/36) = 8,361 - (8281/1296) = 8,361 - 6,389  1,972.

Antwoord (3):
E(X) = 00,1 +10,25+20,3+30,25+40,1 = 0+0,25+0,6+0,75+0,4 = 2,0.
E(X) = 00,1 +10,25+40,3+90,25+160,1 = 0+0,25+1,2+2,25+1,6 = 5,3.
Var(X) = 5,3 - 4 = 1,3. Standaarddeviatie = 1,3  1,14.
Gemiddelde wachttijd = 00,1 + 20,25 + 50,3 + 80,25 + 110,1 = 0 + 0,5 + 1,5 + 2,0 + 1,1 = 5,1 minuten.

Uitleg:
```text
De formules voor grootste en kleinste ogen bij twee dobbelstenen zijn afleidbaar uit de symmetrie. Voor de wachttijd is het de verwachting van de functie Y = f(X) met de gegeven kansen

```
### 3.02 CLT
Gegeven dataSteekproefgrootte (aantal koffers): $n = 43$Populatiegemiddelde gewicht: $\mu = 12.1 \text{ kg}$Populatiestandaarddeviatie: $\sigma = 3.8 \text{ kg}$Gevraagde kans: $P(\bar{X} < 11)$, waarbij $\bar{X}$ het steekproefgemiddelde is van de $43$ koffers.
# Gegeven variabelen
n = 43                  # aantal koffers (steekproefgrootte)
mu = 12.1               # populatiegemiddelde
sigma = 3.8            # populatiestandaarddeviatie
target_weight = 11      # de grens waar het gemiddelde onder moet liggen

# 1. Bereken de standaardfout van het gemiddelde (Standard Error) volgens de CLT
# we gebruiken np.sqrt() in plaats van math.sqrt() aangezien np is gemporteerd
se = sigma / np.sqrt(n)

# 2. Bereken de z-score
z_score = (target_weight - mu) / se

# 3. Bereken de cumulatieve kans (onder de grens, dus links in de staart)
# stats.norm.cdf berekent de kans P(Z < z) voor een standaardnormale verdeling
kans = stats.norm.cdf(z_score)

# Resultaten printen
print(f"Standaardfout (SE): {se:.4f} kg")
print(f"Z-score:            {z_score:.4f}")
print(f"Kans (probability): {kans:.4f}")

The times for a particular journey during rush hour in a large city have mean 23.3 minutes and standard deviation 8.9 minutes.  

Find the probability that the mean time taken for a random sample of 60 of the times for this journey is under 25 minutes. (Answer: 0.9305) 

# Gegeven variabelen
mu = 23.3               # Populatiegemiddelde (in minuten)
sigma = 8.9             # Populatiestandaarddeviatie (in minuten)
n = 60                  # Steekproefgrootte (random sample van 60 ritten)
target_time = 25        # De grens waar het steekproefgemiddelde onder moet liggen

# 1. Bereken de standaardfout van het gemiddelde (Standard Error) volgens de Centrale Limietstelling
se = sigma / np.sqrt(n)

# 2. Bereken de bijbehorende z-score
z_score = (target_time - mu) / se

# 3. Bereken de cumulatieve kans onder de 25 minuten
# stats.norm.cdf() geeft de kans links van de z-score in de standaardnormale verdeling
kans = stats.norm.cdf(z_score)

# Resultaten tonen
print(f"Standaardfout (SE): {se:.4f} minuten")
print(f"Z-score:            {z_score:.4f}")
print(f"Kans (probability): {kans:.4f}")


The weights, in grams, of bags of cocoa beans are normally distributed and are known to have a standard deviation of 6 grams. A random sample of bags is weighed with the following results:  [758, 748, 749, 752, 757, 760, 751, 745, 759, 761]  

Calculate a 95% confidence interval for the mean weight of a bag of cocoa beans (Answer: [749.9393, 758.0607]).

# Gegeven steekproefdata
steekproef = [758, 748, 749, 752, 757, 760, 751, 745, 759, 761]
alpha = 0.05  # Voor een 95% betrouwbaarheidsinterval (1 - 0.95)

# 1. Bereken de steekproefstatistieken
m = np.mean(steekproef)          # Steekproefgemiddelde (Sample mean)
n = len(steekproef)              # Steekproefgrootte (n = 10)
df = n - 1                       # Vrijheidsgraden (Degrees of freedom = 9)

# Merk op: Het officile antwoord maakt gebruik van de steekproefstandaarddeviatie (s)
# en de t-verdeling om het interval te berekenen.
s = np.std(steekproef, ddof=1)   # Steekproefstandaarddeviatie (ddof=1 voor onzuivere schatter)

# 2. Bepaal de kritieke t-waarde
t_score = stats.t.isf(alpha / 2, df)

# 3. Bereken de onder- en bovengrens van het betrouwbaarheidsinterval
lo = m - t_score * s / np.sqrt(n)
hi = m + t_score * s / np.sqrt(n)

# Resultaten tonen
print(f"Steekproefgemiddelde (m):          {m:.1f}")
print(f"Steekproefstandaarddeviatie (s): {s:.4f}")
print(f"Kritieke t-waarde (df=9):         {t_score:.5f}")
print(f"95% Betrouwbaarheidsinterval:     [{lo:.4f}, {hi:.4f}]")



### Lab 3.01
#### Oefening 1 - Reliability of backupsOpgave: Schijf crasht met $1\%$. Twee back-ups elk $2\%$ crashkans. Data is pas weg als alles crasht. Wat is de kans dat data niet verloren gaat?Antwoord: $99,9996\%$ (of $0,999996$)Uitleg: Kans op totale crash is het product van de individuele crashkansen: $0,01 \times 0,02 \times 0,02 = 0,000004$. De kans dat dit niet gebeurt is $1 - 0,000004$.Oefening 2 - EierdozenOpgaven & Antwoorden:Wat is $P(4)$? 0,01Kans op maximaal 2 kapotte eieren? 0,95Kans op minstens 9 hele eieren? 0,95 (minstens 9 heel = maximaal 3 kapot)Kans op exact 9 hele eieren? 0,04 (exact 9 heel = exact 3 kapot)Verwacht aantal kapotte eieren op 800 dozen? 448 eierenUitleg: * Vraag 1 t/m 4: Kansrekenen via de tabel (totale som van kansen is 1).Vraag 5: Bereken eerst de verwachtingswaarde per doos: $\mu = (0 \times 0,65) + (1 \times 0,20) + (2 \times 0,10) + (3 \times 0,04) + (4 \times 0,01) = 0,56$. Voor 800 dozen: $800 \times 0,56 = 448$.Oefening 3 - VerkeerslichtenOpgaven & Antwoorden: ($P(E)=0,4$; $P(F)=0,3$; $P(E \cap F)=0,15$)Onafhankelijk? Nee, want $P(E) \times P(F) = 0,12 \neq 0,15$.Minstens n licht stop? 0,55 ($P(E \cup F) = 0,4 + 0,3 - 0,15$)Geen van beide lichten? 0,45 ($1 - 0,55$)Minstens bij het eerste licht? 0,40 (is gewoon gegeven als $P(E)$)Exact n licht? 0,40 (($0,4 - 0,15) + (0,3 - 0,15)$)Alleen het eerste licht? 0,25 ($0,4 - 0,15$)Oefening 4 - Geldbriefjes trekkenOpgaven & Antwoorden: (Briefjes: 1, 1, 1, 10, 25. Je pakt er twee en wint de hoogste).Kansen: $P(X=1) = 0,3$ | $P(X=10) = 0,4$ | $P(X=25) = 0,3$Verwachtingswaarde: 11,80Uitleg: Totaal aantal combinaties om 2 briefjes uit 5 te trekken is $\binom{5}{2} = 10$.Winst 1: alleen als je twee briefjes van 1 pakt ($\binom{3}{2} = 3$ kansen $\rightarrow 3/10 = 0,3$).Winst 10: n van 10 en n van 1 ($1 \times 3 = 3$ kansen $\rightarrow 0,4$).Verwachting: $(1 \times 0,3) + (10 \times 0,4) + (25 \times 0,3) = 11,8$.Oefening 5 - Online kansspel (Recht evenredig)Opgaven & Antwoorden: ($p(x) = k \cdot x$ voor $x = 1, \dots, 5$)Waarde k: $1/15$ (of $\approx 0,0667$)Kans op maximaal 3? $6/15 = 0,40$Verwachte winst na 100 keer spelen? 366,67Uitleg: * Som van alle kansen moet 1 zijn: $k(1+2+3+4+5) = 1 \rightarrow 15k = 1 \rightarrow k = 1/15$.Per spel is de verwachtingswaarde $\mu = \sum x \cdot p(x) = \frac{1^2+2^2+3^2+4^2+5^2}{15} = \frac{55}{15} = 3,667$. Bij 100 keer: $100 \times 3,667$.Oefening 6 & 7 & 8 - Normaalverdelingen (Python/Theorie)Oefening 6: Dit zijn standaard controle-antwoorden voor de Z-tabel (gegeven in de tabel).Oefening 7: Oppervlakte tussen $x=0,5$ en $x=4$ bij $\mu=2.5$ en $\sigma=1.5$ is 0,750. (In Python: stats.norm.cdf(4, 2.5, 1.5) - stats.norm.cdf(0.5, 2.5, 1.5)).Oefening 8: (Visuele oefening): Hoe groter de steekproef ($N=2500$), hoe beter het histogram de theoretische klokvorm (Gauss-curve) volgt (Wet van de grote aantallen).Oefening 9 - Typisits (Normaalverdeling)Opgaven & Antwoorden: ($\mu = 60$, $\sigma = 15$)Kans op hoogstens 60 wpm? $50\%$ (exact het gemiddelde)Kans tussen 45 en 90 wpm? $81,85\%$Verrast bij snelheid > 105 wpm? Ja, kans is slechts $0,13\%$ ($105$ is $\mu + 3\sigma$).Grens voor traagste 20%? 47,38 wpm of minder (Python: stats.norm.ppf(0.20, 60, 15))Oefening 10 - Dobbelsteen paradox (Chevalier de Mr)Opgaven & Antwoorden:Spel 1 (minstens n 6 in 4 worpen): $51,77\%$ ($1 - (5/6)^4$)Spel 2 (minstens n dubbel 6 in 24 worpen): $49,14\%$ ($1 - (35/36)^{24}$)Uitleg: Bereken via de complementaire kans (de kans om geen zes te gooien). Spel 1 is mathematisch gunstiger dan Spel 2.Oefening 11 t/m 13 - Regels voor Verwachting en VariantieOefening 11 ($X-a$): $E(X - a) = E(X) - a$. Als de inleg 8 is, is je winst $Y = X - 8$. Verwachte winst $E(Y) = 7 - 8 = -1$.Oefening 12 ($X/a$): $Var(X/a) = \frac{Var(X)}{a^2}$. De variantie schaalt met het kwadraat van de deler.Oefening 13 (Standaardisatie): Voor $Z = \frac{X-\mu}{\sigma}$ geldt altijd: $E(Z) = 0$ en $Var(Z) = 1$. (Dit vormt de basis van de standaard normaalverdeling).

### Lab 3.02
#### Oefening 1 - CholesterolOpgave: Vrouwen (20-29 jaar): $\mu = 183$, $\sigma = 36$. Steekproef $n = 81$.Antwoorden:Plot: Een klokcurve (normaalverdeling) gecentreerd rond $183$ met een breedte gebaseerd op $\sigma_{\overline{x}} = 4$.$P(\overline{x} < 185)$: $69,1\%$ (of $0,6915$)$P(175 < \overline{x} < 185)$: $66,9\%$ (of $0,6687$)$P(\overline{x} > 190)$: $4,0\%$ (of $0,0401$)Uitleg: Bereken eerst de standaardfout: $\sigma_{\overline{x}} = \frac{36}{\sqrt{81}} = \frac{36}{9} = 4$. Gebruik daarna deze $\sigma_{\overline{x}} = 4$ en $\mu = 183$ in stats.norm.cdf() om de gevraagde kansen onder de curve te berekenen.Oefening 2 - Onbekende verdelingOpgave: Onbekende populatie met $\mu = 20$ en $\sigma = 16$. Steekproef $n = 64$.Antwoorden:Plot: Opnieuw een normale klokcurve gecentreerd rond $20$ met $\sigma_{\overline{x}} = 2$. (Dankzij de CLT is de steekproefverdeling normaal, omdat $n \geq 30$).Z-score voor $\overline{x} = 15,5$: $-2,25$Z-score voor $\overline{x} = 23$: $+1,5$$P(16 < \overline{x} < 22)$: $81,9\%$ (of $0,8186$)Uitleg: De standaardfout is $\sigma_{\overline{x}} = \frac{16}{\sqrt{64}} = \frac{16}{8} = 2$.De Z-score formule is: $Z = \frac{\overline{x} - \mu}{\sigma_{\overline{x}}}$.Voor $15,5$: $\frac{15,5 - 20}{2} = -2,25$.Voor $23$: $\frac{23 - 20}{2} = 1,5$.Oefening 3 - Waarom afraden van enqutes?Opgave: Waarom proberen we studenten te dissuaderen (ontmoedigen) om een enqute te houden voor hun bachelorproef, kijkend naar de Centrale Limietstelling?Antwoord & Uitleg:Te kleine steekproef ($n$): Volgens de CLT heb je vaak minstens $n \geq 30$ (en bij scheve verdelingen veel meer) volledig willekeurige respondenten nodig om betrouwbare statistische uitspraken te doen. Studenten halen dit aantal vaak niet of vullen het aan met vrienden/familie.Selectiebias (Geen random steekproef): De wetten van de CLT werken alleen als de steekproef aselect (random) is. Studenten enquteren vaak via social media (convenience sampling). Hierdoor is de steekproef niet representatief en zijn de daaropvolgende berekeningen mathematisch ongeldig.Non-response & Kwaliteit: De foutenmarge is vaak te groot binnen de beperkte tijd van een bachelorproef om bruikbare conclusies te trekken.

### Lab 3.03
Lab 3.03 - Confidence Intervals
#### Oefening 1 - Gross annual salary (rlanders.csv)
```python
```
# Gegevens laden (vervang de URL indien nodig)
df = pd.read_csv('https://raw.githubusercontent.com/HoGentTIN/dsai-en-labs/main/data/rlanders.csv').set_index(['ID'])
money = df['Money']
n = len(money)
x_bar = money.mean()

# 1. 99% betrouwbaarheidsinterval ( = 98 bekend)
print(stats.norm.interval(0.99, loc=x_bar, scale=98/np.sqrt(n)))

# 2. 95% betrouwbaarheidsinterval ( = 98 bekend)
print(stats.norm.interval(0.95, loc=x_bar, scale=98/np.sqrt(n)))

# 3. 95% betrouwbaarheidsinterval ( onbekend -> t-verdeling gebruiken)
s = money.std(ddof=1)
print(stats.t.interval(0.95, df=n-1, loc=x_bar, scale=s/np.sqrt(n)))

# 4. 95% betrouwbaarheidsinterval (enkel eerste 25 observaties)
money_25 = money.head(25)
n_25 = len(money_25)
print(stats.t.interval(0.95, df=n_25-1, loc=money_25.mean(), scale=money_25.std(ddof=1)/np.sqrt(n_25)))
Lab 3.04 - Statistical Hypothesis Testing
#### Oefening 1 - Navy recruits (Rechtszijdige Z-toets)
```python
```
# Gegevens inladen
df_recruits = pd.read_csv('https://raw.githubusercontent.com/HoGentTIN/dsai-labs/main/data/recruten.csv', sep=";", decimal=",")
heights = df_recruits['Height']

mu = 69
# Ga uit van de steekproef-standaardafwijking bij gebrek aan een populatie-sigma
s = heights.std(ddof=1) 
n = len(heights)
x_bar = heights.mean()

# p-waarde berekenen (Rechtszijdig)
p_val = 1 - stats.t.cdf((x_bar - mu) / (s / np.sqrt(n)), df=n-1)
print(f"p-waarde: {p_val:.6%}")
#### Oefening 2 - Rainfall (Rechtszijdige Z-toets)
```python
mu = 82.3
sigma = 15.3
n = 5
alpha = 0.05

```
# 1. Kritieke grens g
g = stats.norm.ppf(1 - alpha, loc=mu, scale=sigma / np.sqrt(n))
print(f"Kritieke grens g: {g:.4f}")

# 3. Type II-fout (beta) bij mu_alt = 105
mu_alt = 105
beta = stats.norm.cdf(g, loc=mu_alt, scale=sigma / np.sqrt(n))
print(f"Type II-fout (beta): {beta:.4%}")
#### Oefening 3 - Medical procedure (Linkszijdige Z-toets)
```python
mu = 34.2
sigma = 2.6
n = 50
x_bar = 33.5
alpha = 0.05

```
# p-waarde & kritieke grens g
p_val = stats.norm.cdf(x_bar, loc=mu, scale=sigma / np.sqrt(n))
g = stats.norm.ppf(alpha, loc=mu, scale=sigma / np.sqrt(n))

print(f"p-waarde: {p_val:.4f}, Kritieke grens g: {g:.3f}")
#### Oefening 4 - Stopping distance of a truck (Rechtszijdige t-toets)
```python
distances = np.array([9.78, 9.33, 9.57, 9.26, 9.45, 9.72])
mu = 9.15
n = len(distances)
x_bar = distances.mean()
s = distances.std(ddof=1)
alpha = 0.01

```
# p-waarde & kritieke grens g
p_val = 1 - stats.t.cdf((x_bar - mu) / (s / np.sqrt(n)), df=n-1)
g = stats.t.ppf(1 - alpha, df=n-1, loc=mu, scale=s / np.sqrt(n))

print(f"p-waarde: {p_val:.5f}, Kritieke grens g: {g:.3f}")
#### Oefening 5 - Salary split by Gender
```python
mu = 500
sigma = 98
alpha = 0.05

```
# Filteren op mannen en vrouwen
men = df[df['Gender'] == 'Male']['Money']
women = df[df['Gender'] == 'Female']['Money']

# 1. Mannen (Rechtszijdig)
p_men = 1 - stats.norm.cdf(men.mean(), loc=mu, scale=sigma / np.sqrt(len(men)))
print(f"Mannen p-waarde: {p_men:.4f}")

# 2. Vrouwen (Linkszijdig)
p_women = stats.norm.cdf(women.mean(), loc=mu, scale=sigma / np.sqrt(len(women)))
print(f"Vrouwen p-waarde: {p_women:.4f}")

# 3. Tweezijdig acceptatiegebied (totaal)
print("Acceptatiegebied:", stats.norm.interval(1 - alpha, loc=mu, scale=sigma / np.sqrt(len(df))))
#### Oefening 6 - Bindend Studieadvies (BSA) (Rechtszijdige Z-toets)
```python
mu = 44
sigma = 6.2
n = 72
x_bar = 46.2
alpha = 0.025

```
# Kritieke grens g & p-waarde
g = stats.norm.ppf(1 - alpha, loc=mu, scale=sigma / np.sqrt(n))
p_val = 1 - stats.norm.cdf(x_bar, loc=mu, scale=sigma / np.sqrt(n))

print(f"Kritieke grens g: {g:.2f}, p-waarde: {p_val:.4f}")

### Lab 3.04
#### Oefening 1 - Navy recruits (Rechtszijdige t-toets)Opgave: Test of de gemiddelde lengte van de rekruten van dit jaar significant groter is dan 69 inch ($H_0: \mu = 69, H_1: \mu > 69$).Pythondf_recruits = pd.read_csv('https://raw.githubusercontent.com/HoGentTIN/dsai-labs/main/data/recruten.csv', sep=";", decimal=",")
heights = df_recruits['Height']

mu = 69
n = len(heights)
x_bar = heights.mean()
s = heights.std(ddof=1) # Steekproef-standaardafwijking

# p-waarde en kritieke grens g
p_val = 1 - stats.t.cdf((x_bar - mu) / (s / np.sqrt(n)), df=n-1)
g = stats.t.ppf(1 - 0.05, df=n-1, loc=mu, scale=s / np.sqrt(n))

print(f"p-waarde: {p_val:.6%}\nKritieke grens g: {g:.4f}\nSteekproefgemiddelde: {x_bar:.4f}")
# Conclusie: p < 0.05 of x_bar > g -> H0 verwerpen
#### Oefening 2 - Rainfall (Rechtszijdige Z-toets)Opgave: Toets of de neerslag is gestegen ($\mu = 82.3, \sigma = 15.3, n = 5, \alpha = 0.05$). Bereken ook de Type II-fout ($\beta$) als $\mu$ in werkelijkheid $105$ is.Pythonmu = 82.3
sigma = 15.3
n = 5
alpha = 0.05

# 1. Kritieke grens g
g = stats.norm.ppf(1 - alpha, loc=mu, scale=sigma / np.sqrt(n))
print(f"1. Regio van verwerping: ]{g:.4f}, +inf[") #

# 2. Kans op Type I-fout is altijd gelijk aan alpha
print(f"2. Kans op Type I-fout: {alpha:.0%}") #

# 3. Kans op Type II-fout (beta) bij mu_alt = 105
mu_alt = 105
beta = stats.norm.cdf(g, loc=mu_alt, scale=sigma / np.sqrt(n))
print(f"3. Kans op Type II-fout (beta): {beta:.4%}") #
#### Oefening 3 - Medical procedure (Linkszijdige Z-toets)Opgave: Toets of de gemiddelde duur van de nieuwe methode korter is dan 34.2 minuten ($\mu = 34.2, \sigma = 2.6, n = 50, \overline{x} = 33.5, \alpha = 0.05$).Pythonmu = 34.2
sigma = 2.6
n = 50
x_bar = 33.5
alpha = 0.05

# p-waarde & kritieke grens g
p_val = stats.norm.cdf(x_bar, loc=mu, scale=sigma / np.sqrt(n))
g = stats.norm.ppf(alpha, loc=mu, scale=sigma / np.sqrt(n))

print(f"p-waarde: {p_val:.5f}\nKritieke grens g: {g:.3f}")
# Conclusie: p < 0.05 of x_bar < g -> H0 verwerpen
#### Oefening 4 - Stopping distance of a truck (Rechtszijdige t-toets)Opgave: Toets of de werkelijke remafstand groter is dan de limiet van 9.15 meter ($\mu = 9.15, \alpha = 0.01$).Pythondistances = np.array([9.78, 9.33, 9.57, 9.26, 9.45, 9.72])
mu = 9.15
n = len(distances)
x_bar = distances.mean()
s = distances.std(ddof=1)
alpha = 0.01

# p-waarde & kritieke grens g
p_val = 1 - stats.t.cdf((x_bar - mu) / (s / np.sqrt(n)), df=n-1)
g = stats.t.ppf(1 - alpha, df=n-1, loc=mu, scale=s / np.sqrt(n))

print(f"p-waarde: {p_val:.5f}\nKritieke grens g: {g:.3f}")
# Conclusie: p < 0.01 -> H0 verwerpen
#### Oefening 5 - rlanders.csv, revisited (Z-toetsen opgesplitst)Opgave: Toets of mannen significant meer verdienen dan $\mu=500$ (rechtszijdige toets) en vrouwen significant minder verdienen (linkszijdige toets) ($\sigma=98, \alpha=0.05$).Pythondf = pd.read_csv('https://raw.githubusercontent.com/HoGentTIN/dsai-en-labs/main/data/rlanders.csv').set_index(['ID'])
mu = 500
sigma = 98
alpha = 0.05

men = df[df['Gender'] == 'Male']['Money']
women = df[df['Gender'] == 'Female']['Money']

# 1. Mannen (Rechtszijdig)
p_men = 1 - stats.norm.cdf(men.mean(), loc=mu, scale=sigma / np.sqrt(len(men)))
g_men = stats.norm.ppf(1 - alpha, loc=mu, scale=sigma / np.sqrt(len(men)))
print(f"Mannen: sample mean = {men.mean():.3f}, g = {g_men:.3f}, p = {p_men:.4f}") #

# 2. Vrouwen (Linkszijdig)
p_women = stats.norm.cdf(women.mean(), loc=mu, scale=sigma / np.sqrt(len(women)))
g_women = stats.norm.ppf(alpha, loc=mu, scale=sigma / np.sqrt(len(women)))
print(f"Vrouwen: sample mean = {women.mean():.3f}, g = {g_women:.3f}, p = {p_women:.4f}") #

# 3. Tweezijdig acceptatiegebied (totale steekproef)
print("3. Regio van acceptatie (totaal):", stats.norm.interval(1 - alpha, loc=mu, scale=sigma / np.sqrt(len(df)))) #
#### Oefening 6 - Bindend Studieadvies (BSA) (Rechtszijdige Z-toets)Opgave: Toets of het gemiddelde aantal studiepunten na invoering van het BSA significant hoger is dan 44 ($\mu = 44, \sigma = 6.2, n = 72, \overline{x} = 46.2, \alpha = 0.025$).Pythonmu = 44
sigma = 6.2
n = 72
x_bar = 46.2
alpha = 0.025

# Kritieke grens g & p-waarde
g = stats.norm.ppf(1 - alpha, loc=mu, scale=sigma / np.sqrt(n))
p_val = 1 - stats.norm.cdf(x_bar, loc=mu, scale=sigma / np.sqrt(n))

print(f"Kritieke grens g: {g:.4f}\np-waarde: {p_val:.4f}")
# Conclusie: x_bar > g en p < alpha -> H0 verwerpen







