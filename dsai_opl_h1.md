# Oplossingen Oefeningen Data Science

## Hoofdstuk 1

### Lab 1.01

#### Stap 1 — Imports
```python
# Importing the necessary packages
import numpy as np
import scipy.stats as stats

import pandas as pd
from pandas.api.types import CategoricalDtype

import matplotlib.pyplot as plt
from statsmodels.graphics.mosaicplot import mosaic
import seaborn as sns
```

#### Stap 2 — Dataset inlezen
```python
# Inlezen van de dataset via de juiste RAW URL
ais = pd.read_csv('https://raw.githubusercontent.com/HoGentTIN/dsai-en-labs/main/data/ais.csv')

# Toon de eerste paar observaties
ais.head()
```

#### Stap 3 — Algemene informatie opvragen
```python
# 1. Aantal rijen en kolommen
print(f"Aantal rijen: {ais.shape[0]}")
print(f"Aantal kolommen: {ais.shape[1]}\n")

# 2 & 3. Algemene informatie en datatypes per kolom
ais.info()
```

```text
# 4. Theoretische meetniveaus (voorbeeldinterpretatie)
- id: identificator / index
- sex, sport: nominaal
- rcc, wcc, hc, hg, ferr, bmi, ssf, pcBfat, lbm, ht, wt: ratio
```
The column "id" is not an actual variable, but an index. Mark it as such.
```python
ais = ais.set_index('id')

```

The variables that are now considered "object" are qualitative variables. Change the type of each of these variables to "category". For ordinal variables, also define a type and impose an order. Verify that the conversion was successful by requesting info about the types again.
```python
# Convert 'sex' and 'sport' to the category type
ais['sex'] = ais['sex'].astype('category')
ais['sport'] = ais['sport'].astype('category')

# Note: In the ais dataset, 'sex' and 'sport' are nominal. 
# If there were ordinal variables (e.g., 'rank'), you would define them like this:
# from pandas.api.types import CategoricalDtype
# rank_type = CategoricalDtype(categories=['low', 'medium', 'high'], ordered=True)
# ais['rank'] = ais['rank'].astype(rank_type)

# Verify the changes
print(ais.info())

```

Describe the columns `ferr`, `bmi`, `sex` and `sport` and the unique values in each of these columns. Do you recognize the characteristics of qualitative and quantitative variables in the result?
```python
# 1. Beschrijvende statistiek voor kwantitatieve variabelen (ferr, bmi)
print("--- Beschrijvende statistiek (Kwantitatief) ---")
print(ais[['ferr', 'bmi']].describe())

# 2. Frequentieverdeling en unieke waarden voor kwalitatieve variabelen (sex, sport)
print("\n--- Unieke waarden en frequenties (Kwalitatief) ---")
print(f"Unieke waarden 'sex': {ais['sex'].unique()}")
print(f"Aantal per 'sex':\n{ais['sex'].value_counts()}")
print(f"\nUnieke waarden 'sport': {ais['sport'].unique()}")
print(f"Aantal per 'sport':\n{ais['sport'].value_counts()}")

```

Select following elements from the dataset:

- the second row (ids = 2)
- rows 4 to 6 (ids = 5 to 7)
- Columns 6 to 8 (`ferr`, `bmi`, `ssf`)
- the variable `pcBfat` (by name!). There are multiple ways to retrieve this!
- all observations for the sport "Netball"
- just the variable `wt` of the observations for "Netball"
- which sports are played by athletes with a BMI higher than 26? Also, provide a list of the unique values and a frequency table of how often each sport occurs.
```python
# 1. De tweede rij (id=2)
# Omdat id nu de index is, gebruiken we .iloc voor de positie (index 1 is de 2e rij)
print("--- Tweede rij (id=2) ---")
print(ais.iloc[1])

# 2. Rijen 4 tot 6 (id=5 tot 7)
# .iloc indexen zijn 0-based, dus rij 4 is index 3, tot (exclusief) index 6
print("\n--- Rijen 4 tot 6 ---")
print(ais.iloc[3:6])

# 3. Kolommen 6 tot 8 (ferr, bmi, ssf)
# Dit zijn de kolommen op positie 5 t/m 7 (0-based)
print("\n--- Kolommen 6 tot 8 ---")
print(ais.iloc[:, 5:8])

# 4. De variabele 'pcBfat' (op verschillende manieren)
print("\n--- Variabele 'pcBfat' (3 methoden) ---")
print(ais['pcBfat'].head())       # Via kolomnaam
print(ais.pcBfat.head())          # Via dot-notatie
print(ais.loc[:, 'pcBfat'].head()) # Via .loc

# 5. Alle observaties voor sport "Netball"
netball_athletes = ais[ais['sport'] == 'Netball']
print("\n--- Observaties voor 'Netball' ---")
print(netball_athletes)

# 6. Enkel de variabele 'wt' voor "Netball"
print("\n--- Gewicht ('wt') van 'Netball' spelers ---")
print(netball_athletes['wt'])

# 7. Sporten met BMI > 26
high_bmi = ais[ais['bmi'] > 26]
unique_sports = high_bmi['sport'].unique()
sport_freq = high_bmi['sport'].value_counts()

print("\n--- Sporten met BMI > 26 ---")
print(f"Unieke sporten: {unique_sports}")
print("\nFrequentietabel:")
print(sport_freq)

```

### Lab 1.02

#### Stap 1 — Imports
```python
# Importing the necessary packages
import numpy as np
import scipy.stats as stats

import pandas as pd
from pandas.api.types import CategoricalDtype

import matplotlib.pyplot as plt
from statsmodels.graphics.mosaicplot import mosaic
import seaborn as sns
```

#### Stap 2 — Dataset inlezen (met correcte separator)
```python
# Dataset inlezen met het juiste scheidingsteken (sep=';')
android_persistence = pd.read_csv(
    'https://raw.githubusercontent.com/HoGentTIN/dsai-en-labs/main/data/android_persistence_cpu.csv',
    sep=';'
)

# Toon de eerste paar observaties om de kolommen te controleren
android_persistence.head()
```

#### Stap 3 — Dataset verkennen & datatypes omzetten
```python
# 1. Aantal variabelen en observaties
print(f"Aantal observaties (rijen): {android_persistence.shape[0]}")
print(f"Aantal variabelen (kolommen): {android_persistence.shape[1]}\n")

# 2. Meetniveaus (theoretische interpretatie)
# - Time: Ratio
# - PersistenceType: Nominaal
# - DataSize: Ordinaal (Small < Medium < Large)

# 3. Omzetten van kwalitatieve variabelen
android_persistence['PersistenceType'] = android_persistence['PersistenceType'].astype('category')
datasize_type = CategoricalDtype(categories=['Small', 'Medium', 'Large'], ordered=True)
android_persistence['DataSize'] = android_persistence['DataSize'].astype(datasize_type)

# 4. Lijst van datatypes tonen
print("--- Gecorrigeerde datatypes ---")
print(android_persistence.dtypes)
```

#### Stap 4 — Variabelen beschrijven
```python
# Beschrijf alle kolommen (zowel numeriek als gecategoriseerd)
android_persistence.describe(include='all')
```

#### Stap 5 — Unieke waarden en frequenties
```python
print("--- Unieke waarden & frequenties: PersistenceType ---")
print(android_persistence['PersistenceType'].value_counts())

print("\n--- Unieke waarden & frequenties: DataSize ---")
print(android_persistence['DataSize'].value_counts())
```

#### Stap 6 — Kruistabel / contingency table (crosstab)
```python
print("--- Contingency Table (Kruistabel) ---")
pd.crosstab(
    android_persistence['PersistenceType'],
    android_persistence['DataSize'],
    margins=True
)
```

### Lab 1.03

#### Oefening 2 — Steekproefmethoden (Stratified sample)
```text
Casus / Vraag:
Een onderzoeker wil de consumptiegewoonten van inwoners van 18 jaar en ouder in een bepaalde gemeente met 3 woonwijken zo nauwkeurig mogelijk onderzoeken. Ze onderscheidt 4 leeftijdsgroepen, waardoor ze uiteindelijk in totaal 12 subgroepen heeft. Bij de gemeente vraagt ze de procentuele opbouw van de bevolking op, en op basis van die cijfers berekent ze het aantal benodigde respondenten voor elke leeftijdsgroep. Dit type steekproef noemen we een gestratifieerde steekproef (stratified sample).
```

```text
Is dit een toevalsteekproef (random sample)? Waarom (niet)?

Modelantwoord:
Ja, mits de selectie binnen de subgroepen (strata) op toeval berust. De beschrijving in de opgave laat de exacte selectiemethode open. Als de onderzoeker binnen elke leeftijdscategorie willekeurig mensen kiest (bijv. via het bevolkingsregister), is het een gestratifieerde toevalsteekproef. Als ze echter de straat opgaat en willekeurige voorbijgangers aanspreekt tot de quota vol zijn, is het een quotasteekproef (geen toevalsteekproef).
```

```text
Is deze steekproef representatief voor de populatie?

Modelantwoord:
Ja. Omdat de steekproef qua verhoudingen (leeftijd en woonwijk) exact een weerspiegeling is van de werkelijke populatieopbouw van de gemeente, is deze op deze kenmerken representatief.
```

```text
Welke typen steekproeffouten kunnen hier optreden?

Modelantwoord:
Zowel een toevallige steekproeffout (random sampling error  die treedt altijd op omdat je een deel van de populatie meet) als eventuele systematische fouten (systematic sampling errors), zoals non-response bias wanneer geselecteerde personen weigeren mee te werken.
```

```text
Wat zijn de voor- en nadelen?

Modelantwoord:
Voordeel: Het garandeert dat ook kleinere subgroepen in de populatie voldoende vertegenwoordigd zijn, wat de nauwkeurigheid van de resultaten per subgroep sterk verhoogt.

Nadeel: Het vereist vooraf zeer gedetailleerde en up-to-date kennis over de populatieverhoudingen, wat administratief complex en tijdrovend kan zijn.
```

#### Oefening 3 — Steekproefmethoden (Waspunten)
```text
Casus / Vraag:
Een onderzoeksbureau wil het koopgedrag van wasproducten onderzoeken. Ze ondervragen een aantal vrouwen tussen de 25 en 55 jaar omdat ze ervan uitgaan dat deze representatief zijn voor de meeste klanten.
```

```text
Is dit een toevalsteekproef (random sample)? Waarom (niet)?

Modelantwoord:
Nee. Dit is een doelgerichte steekproef (purposive sampling of judgment sample). De onderzoekers selecteren zelf bewust en subjectief een specifieke doelgroep op basis van hun eigen aannames, waardoor niet elk lid van de populatie een kans heeft om gekozen te worden.
```

```text
Is deze steekproef representatief voor de populatie?

Modelantwoord:
Nee. De beoogde populatie bestaat uit alle kopers van wasproducten. Door alleen vrouwen tussen de 25 en 55 te ondervragen, worden bijvoorbeeld alleenwonende mannen, ouderen en jongeren volledig uitgesloten. De aanname dat deze groep representatief is, hoeft in werkelijkheid niet te kloppen.
```

```text
Welke steekproeffout(en) is/officieel gemaakt?

Modelantwoord:
Er is sprake van een systematische fout, specifiek selectiebias of dekkingsfout (undercoverage bias), omdat een groot deel van de daadwerkelijke populatie geen enkele kans had om in de steekproef te worden opgenomen.
```

```text
Welke verbeteringen zou je voorstellen voor de opbouw van deze steekproef?

Modelantwoord:
Trek een aselecte (toevallige) steekproef uit een database van daadwerkelijke wasmiddelkopers. Dit kan bijvoorbeeld door willekeurige klanten bij de kassa van verschillende supermarkten te bevragen (ongeacht geslacht of leeftijd) of via klantenkaarten willekeurig respondenten te selecteren.
```

#### Oefening 4 — Steekproefmethoden (IT-bedrijf vakbond)
```text
Casus / Vraag:
De vakbond wil de werkomstandigheden van de werknemers van een IT-bedrijf onderzoeken. Dit bedrijf heeft in totaal 3200 werknemers verspreid over 12 filialen. Omdat het totale aantal werknemers vrij groot is, worden er uit elk filiaal willekeurig 40 werknemers geselecteerd. De steekproefgrootte is dus n = 480.
```

```text
Is dit een toevalsteekproef (random sample)? Waarom (niet)?

Modelantwoord:
Ja. De werknemers worden binnen elk filiaal willekeurig (at random) geselecteerd uit de personeelslijst.
```

```text
Is de steekproef representatief voor de populatie? Zo niet, wanneer zou het representatief kunnen zijn?

Modelantwoord:
Nee, over het algemeen niet. Het is alleen representatief als alle 12 filialen toevallig exact even groot zijn (dus elk filiaal heeft exact 266 of 267 werknemers). Als filialen verschillen in grootte (bijv. Hoofdkantoor heeft 1500 man, een klein lokaal kantoor heeft 50 man), dan is de kans voor een werknemer uit het kleine kantoor om gekozen te worden veel groter (40/50) dan voor iemand op het hoofdkantoor (40/1500).
```

```text
Wat voor soort fout(en) wordt hier gemaakt?

Modelantwoord:
Dit is een systematische fout veroorzaakt door een gedisproportioneerd steekproefdesign. Grote filialen worden ondervertegenwoordigd en kleine filialen oververtegenwoordigd in het eindresultaat.
```

```text
Welke verbeteringen zou je voorstellen voor de opbouw van deze steekproef?

Modelantwoord:
Pas een proportioneel gestratifieerde steekproef toe. In plaats van een vast aantal van 40 personen per filiaal, kies je een vast percentage (bijvoorbeeld 15%) van het aantal werknemers per filiaal. Hierdoor wegen de stemmen uit grotere filialen zwaarder mee, passend bij de werkelijkheid.
```

#### Oefening 5 — Steekproefmethoden (Enquête HoGent)
```text
Casus / Vraag:
We willen een enquete houden naar de kwaliteit van ons onderwijs aan Hogeschool Gent. Hiervoor worden de studenten die aanwezig zijn bij een bepaald hoorcollege ondervraagd.
```

```text
Is dit een toevalsteekproef (random sample)? Waarom (niet)?

Modelantwoord:
Nee. Dit is een gemaksteekproef of gemakkelijkheidssteekproef (convenience sample). De onderzoeker kiest simpelweg de groep die op dat moment het makkelijkst beschikbaar is; er is geen sprake van een actieve, willekeurige trekking waarbij iedere HoGent-student een gelijke kans maakt om geselecteerd te worden.
```

```text
Is de steekproef representatief voor de populatie?

Modelantwoord:
Nee. Studenten van een specifiek hoorcollege zijn niet representatief voor de gehele populatie (alle studenten van alle verschillende opleidingen en campussen van HoGent).
```

```text
What kind of error(s) is/are being made here?

Modelantwoord:
Systematische selectiebias (of dekkingsfout). Studenten die niet naar de les komen (bijvoorbeeld omdat ze ontevreden zijn over de kwaliteit, of juist omdat ze via zelfstudie werken) hebben een kans van 0% om in de steekproef te komen.
```

```text
Stel dat de docent die het vak geeft aanwezig is tijdens de ondervraging. Welke invloed kan dit hebben en welke fout wordt in dit geval gemaakt?

Modelantwoord:
Dit veroorzaakt social desirability bias (sociale wenselijkheidsbias). Studenten durven mogelijk niet 100% eerlijk of kritisch te antwoorden uit angst dat dit hun cijfers of de relatie met de docent negatief beinvloedt.
```

```text
Stel dat de ondervraging niet tijdens een college wordt gehouden, maar direct na een examen. Welke fout kan er dan gemaakt worden?

Modelantwoord:
Dit veroorzaakt een meetfout (measurement bias due to emotional/situational influence). De actuele emotie rondom het examen (bijvoorbeeld frustratie over een heel moeilijk examen of opluchting na een makkelijk examen) zal de antwoorden over de algemene kwaliteit van de gehele opleiding sterk kleuren.
```

```text
Welke verbeteringen zou je voorstellen voor de opbouw van deze steekproef?

Modelantwoord:
Trek een aselecte (willekeurige) steekproef uit de centrale studentendatabase van HoGent. Nodig deze willekeurig geselecteerde studenten vervolgens via e-mail uit om anoniem een digitale vragenlijst in te vullen.
```

#### Oefening 6 — Retrieval practice: sampling errors
```text
Vraag:
Maak gebruik van de procedure voor retrieval practice uit Oefening 1 om de typen steekproeffouten te bestuderen. Maak een lijst van de verschillende steekproeffouten, beschrijf ze en geef een voorbeeld.
```

```text
Modelantwoord (Overzicht van de belangrijkste fouten):

Toevallige steekproeffout (Random sampling error): De onvermijdelijke afwijking tussen de steekproef en de populatie, puur omdat je een steekproef trekt in plaats van de hele populatie te meten. Voorbeeld: Als je drie keer achter elkaar een munt 10 keer opgooit, zul je niet telkens exact 5 keer kop gooien.

Dekkingsfout (Undercoverage / Selection bias): Treedt op wanneer het steekproefkader (de lijst waaruit je trekt) niet overeenkomt met de werkelijke populatie. Voorbeeld: Een telefonische enquete via vaste telefoonlijnen om het mediagedrag van jongeren te meten (jongeren hebben vaak geen vaste lijn meer).

Non-response bias: Treedt op wanneer de groep mensen die niet reageert op een enquete wezenlijk verschilt van de groep die wel reageert. Voorbeeld: Een enquete over tevredenheid met de klantenservice die alleen ingevuld wordt door extreem boze of extreem tevreden klanten.
```

#### Oefening 7 — Meetniveaus (Measurement levels)
```text
Vraag:
Wat is het meetniveau van elk van deze variabelen:

De functie van een werknemer (executive, managerial, ...)

Modelantwoord:
Kwalitatief — Ordinaal (Er zijn tekstuele categorien, maar er zit een duidelijke, logische rangorde of hinarchie in).
```

```text
Het salaris van de werknemer

Modelantwoord:
Kwantitatief — Ratio (Het zijn numerieke waarden met een natuurlijk, absoluut nulpunt: 0 betekent geen salaris, en iemand met 4000 verdient exact dubbel zoveel als iemand met 2000).
```

```text
De naam van de afdeling van de werknemer

Modelantwoord:
Kwalitatief — Nominaal (Categorien op basis van namen/labels zonder ingebouwde rangorde, zoals 'HR', 'IT' of 'Marketing').
```

```text
De ancinniteit van de werknemer (= het aantal dienstjaren)

Modelantwoord:
Kwantitatief — Ratio (Numerieke schaal met een absoluut nulpunt: 0 dienstjaren is de start, en 10 jaar in dienst is exact twee keer zo lang als 5 jaar in dienst).
```

#### Oefening 8 — Telefonische enquete Vlaamse Overheid
```text
Casus / Vraag:
De Vlaamse overheid wil het sociaal welzijn van haar inwoners onderzoeken. Ze laat daarom een computer willekeurig gsm-nummers kiezen uit een database met alle bekende gsm-nummers van Vlamingen. Wanneer iemand niet antwoordt, wordt er herhaaldelijk teruggebeld tot er antwoord is of tot een maximaal aantal pogingen is bereikt.
```

```text
Is dit een toevalsteekproef? Leg uit.

Modelantwoord:
Ja. Omdat de computer nummers volledig willekeurig (at random) selecteert uit de beschikbare database.
```

```text
Welk type fout(en) wordt hier gemaakt?

Modelantwoord:
Vooral een dekkingsfout (undercoverage bias). Niet alle Vlamingen staan geregistreerd in die gsm-database (denk aan jonge kinderen, ouderen die geen gsm bezitten, of mensen met geheime nummers/buitenlandse simkaarten). Daarnaast blijft er een risico op non-response bias voor mensen die principieel nooit opnemen voor onbekende nummers, ondanks het herhaaldelijk terugbellen.
```

```text
Is dit een goede steekproef? Leg uit.

Modelantwoord:
Matig. Hoewel het herhaaldelijk terugbellen een uitstekende methode is om non-response te verlagen, is de dekkingsfout problematisch voor dit specifieke onderzoeksdoel. Juist bij een onderzoek naar 'sociaal welzijn' wil je kwetsbare groepen (zoals ouderen of lagere inkomensklassen zonder gsm-abonnement) bereiken. Die vallen er nu buiten, waardoor de resultaten vertekend kunnen zijn.
```

### Lab 1.04

1. We want to add a separate column showing the total number of women and the total Number_of_Men by age.  
Before we can sum, however, we need to replace all NaNs with 0. Otherwise, the total sum by age will also be NaN. We are nothing with that.
```python
bel_pop = bel_pop.fillna(0) 
```
2. Add two new columns:

   - the total number of women by age
   - the total Number_of_Men by age
  
```python
# Gebruik vierkante haken en aanhalingstekens voor de kolommen
bel_pop['Total_Number_of_Women_byAge'] = (
    bel_pop['Women_Unmarried'] + 
    bel_pop['Women_Married'] + 
    bel_pop['Women_Widowed'] + 
    bel_pop['Women_Divorced']
)

bel_pop['Total_Number_of_Men_byAge'] = (
    bel_pop['Men_Unmarried'] + 
    bel_pop['Men_Married'] + 
    bel_pop['Men_Widowed'] + 
    bel_pop['Men_Divorced']
)
```
3. What is the total number of women and men in Belgium?
 ```python
# Totaal aantal mannen en vrouwen samen
print((bel_pop['Total_Number_of_Men_byAge'] + bel_pop['Total_Number_of_Women_byAge']).sum())

# Totaal aantal mannen
print(bel_pop['Total_Number_of_Men_byAge'].sum())

# Totaal aantal vrouwen
print(bel_pop['Total_Number_of_Women_byAge'].sum())
```
4. What is the average age for women and men in Belgium? Do women still outlive men?
  ```python
# 1. Gemiddelde leeftijd voor vrouwen
# Formule: (Som van [Leeftijd * Aantal per leeftijd]) / (Totaal aantal vrouwen)
avg_age_women = (bel_pop['Age'] * bel_pop['Total_Number_of_Women_byAge']).sum() / bel_pop['Total_Number_of_Women_byAge'].sum()

# 2. Gemiddelde leeftijd voor mannen
avg_age_men = (bel_pop['Age'] * bel_pop['Total_Number_of_Men_byAge']).sum() / bel_pop['Total_Number_of_Men_byAge'].sum()

print(f"Gemiddelde leeftijd vrouwen: {avg_age_women:.2f}")
print(f"Gemiddelde leeftijd mannen: {avg_age_men:.2f}")

# 3. Vergelijking: overleven vrouwen mannen?
if avg_age_women > avg_age_men:
    print("Ja, vrouwen hebben gemiddeld een hogere leeftijd dan mannen.")
else:
    print("Nee, mannen hebben gemiddeld een hogere leeftijd dan vrouwen.")
```
5. A first look at the data shows, that the distribution of girls vs boys is not 50/50. This is a well-known scientific phenomenon.  
Calculate the total number of girls through age 5, and the total number of boys through age 5. Based on this, calculate the exact distribution of girls vs boys.

  ```python
# 1. Filter de dataset voor de leeftijdscategorie 0 t/m 5
# Stel dat 'Age' de index is of als kolom aanwezig is.
# Gebruik .loc om rijen 0 tot en met 5 te selecteren.
data_0_to_5 = bel_pop[bel_pop['Age'] <= 5]

# 2. Bereken de totalen voor meisjes en jongens in deze categorie
total_girls_0_5 = data_0_to_5['Total_Number_of_Women_byAge'].sum()
total_boys_0_5 = data_0_to_5['Total_Number_of_Men_byAge'].sum()

# 3. Bereken de verdeling (percentages)
total_children = total_girls_0_5 + total_boys_0_5
percentage_girls = (total_girls_0_5 / total_children) * 100
percentage_boys = (total_boys_0_5 / total_children) * 100

# 4. Resultaten tonen
print(f"Totaal meisjes (t/m 5 jaar): {total_girls_0_5}")
print(f"Totaal jongens (t/m 5 jaar): {total_boys_0_5}")
print(f"Verdeling: {percentage_girls:.2f}% meisjes vs {percentage_boys:.2f}% jongens")
```
6. At birth, there are more boys than girls. However, boys die younger, partly due to reckless behavior.  
From what age do women outnumber men?
```python
# Filter de dataset op rijen waar er meer vrouwen dan mannen zijn
women_outnumber_men = bel_pop[bel_pop['Total_Number_of_Women_byAge'] > bel_pop['Total_Number_of_Men_byAge']]

# De laagste leeftijd waarvoor dit geldt
first_age = women_outnumber_men['Age'].min()

print(f"Vrouwen zijn vanaf de leeftijd {first_age} in de meerderheid.")
```
7. Make the following overview
```python
import numpy as np
import pandas as pd
from scipy.interpolate import interp1d

# 1. Bereken het totaal aantal getrouwde vrouwen en mannen per leeftijd
# (Zorg dat 'Women_Married' en 'Men_Married' in je bel_pop DataFrame staan)
# We vullen eventuele NaN-waarden op met 0
bel_pop["Women_Married"] = bel_pop["Women_Married"].fillna(0)
bel_pop["Men_Married"] = bel_pop["Men_Married"].fillna(0)

# 2. Bereken de cumulatieve som van de getrouwde populatie
cum_women_married = bel_pop["Women_Married"].cumsum()
cum_men_married = bel_pop["Men_Married"].cumsum()

# 3. Zet deze cumulatieve sommen om naar percentages (0% - 100%)
# We delen door het totale aantal getrouwde mensen (de laatste waarde van de cumsum)
pct_women_married = (cum_women_married / cum_women_married.iloc[-1]) * 100
pct_men_married = (cum_men_married / cum_men_married.iloc[-1]) * 100

# Leeftijdenas definiëren (bijv. van 0 tot de maximale leeftijd in je dataset)
ages = bel_pop["Age"].values

# 4. Maak interpolatiefuncties (We willen de leeftijd vinden op basis van het percentage)
# 'bounds_error=False' en 'fill_value="extrapolate"' vangen eventuele randgevallen op
get_age_women = interp1d(
    pct_women_married, ages, kind="linear", bounds_error=False
)
get_age_men = interp1d(pct_men_married, ages, kind="linear", bounds_error=False)

# 5. Definieer de doelpercentages uit de screenshot
target_percentages = [15, 20, 25, 30, 35, 40, 45, 50, 55]

# 6. Loop door de percentages en print het resultaat (afgerond op gehele getallen)
for pct in target_percentages:
    age_w = int(np.round(get_age_women(pct)))
    age_m = int(np.round(get_age_men(pct)))

    print(f"Age at which {pct}% of the women are married = {age_w}")
    print(f"Age at which {pct}% of the men are married = {age_m}")
    print()  # Leeregel tussen de blokken zoals in de screenshot
```

Can you draw any conclusions based on this overview if you assume that every Belgian is married to another Belgian (which is not completely true of course)?

```text
Women marry younger than men: Across every single percentage tier, women reach that marriage milestone earlier than men. For example, by age 27, 15% of women are married, whereas men don't hit that same 15% mark until age 29.

The age gap widens as people get older: For the first 15% of marriages, the age difference between men and women is only 2 years (27 vs 29). However, by the time we reach the 45% and 50% milestones, the age gap widens to 5 years (e.g., 43 for women vs 48 for men).
```

8. It is said that
['Loneliness is as destructive to health as smoking fifteen cigarettes a day.'](https://www.bnnvara.nl/joop/artikelen/eenzaamheid-net-zo-gevaarlijk-als-roken). Calculate the average age of women and men over 45 (45 included) for each marital status.
```python
# 1. Filter de dataset direct voor iedereen van 45 jaar en ouder
bel_pop_45plus = bel_pop[bel_pop["Age"] >= 45]
leeftijden = bel_pop_45plus["Age"]

print("--- Mannen en Vrouwen Samen (45+) ---")

# ONGEHUWD
ongehuwd_samen = (
    bel_pop_45plus["Women_Unmarried"] + bel_pop_45plus["Men_Unmarried"]
)
avg_ongehuwd = (leeftijden * ongehuwd_samen).sum() / ongehuwd_samen.sum()
print(f"Ongehuwd: {avg_ongehuwd:.1f} jaar")

# GETROUWD
getrouwd_samen = (
    bel_pop_45plus["Women_Married"] + bel_pop_45plus["Men_Married"]
)
avg_getrouwd = (leeftijden * getrouwd_samen).sum() / getrouwd_samen.sum()
print(f"Getrouwd: {avg_getrouwd:.1f} jaar")

# VERWEDUWD
verweduwd_samen = (
    bel_pop_45plus["Women_Widowed"] + bel_pop_45plus["Men_Widowed"]
)
avg_verweduwd = (leeftijden * verweduwd_samen).sum() / verweduwd_samen.sum()
print(f"Verweduwd: {avg_verweduwd:.1f} jaar")

# GESCHEIDEN
gescheiden_samen = (
    bel_pop_45plus["Women_Divorced"] + bel_pop_45plus["Men_Divorced"]
)
avg_gescheiden = (leeftijden * gescheiden_samen).sum() / gescheiden_samen.sum()
print(f"Gescheiden: {avg_gescheiden:.1f} jaar")
```
