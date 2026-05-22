## Hoofdstuk 4

### Lab 4.01

#### Opgave 1 — Age vs Preference

#### Opgave
Onderzoek met een chi-kwadraat (χ²) onafhankelijkheidstoets of de drankvoorkeur (`Preference`) onafhankelijk is van de leeftijdscategorie (`Age`).

```python
# 1. Kruistabel (contingency table) maken
cross_age = pd.crosstab(softdrinks["Age"], softdrinks["Preference"])
print("Kruistabel Age vs Preference:")
print(cross_age)
print("-" * 50)

# 2. Chi-kwadraat onafhankelijkheidstoets uitvoeren
chi2_age, p_age, dof_age, expected_age = stats.chi2_contingency(cross_age)

print(f"Chi-kwadraat statistiek (χ²): {chi2_age:.4f}")
print(f"p-waarde: {p_age:.4f}")
print(f"Vrijheidsgraden: {dof_age}")
```

```text
Uitleg:
Nulhypothese (H0): Drankvoorkeur en leeftijd zijn onafhankelijk.
Alternatieve hypothese (H1): Drankvoorkeur is afhankelijk van leeftijd.
Interpretatie: De berekende p-waarde is 0.2771; groter dan α=0.05, dus H0 kan niet worden verworpen.
```

#### Opgave 2 — Gender vs Preference

#### Opgave
Onderzoek of `Preference` onafhankelijk is van `Gender`. (Bij 2×2-tabellen past scipy standaard Yates-correctie; hier gebruiken we `correction=False` voor reproduceerbare resultaten.)

```python
# 1. Kruistabel maken
cross_gender = pd.crosstab(softdrinks["Gender"], softdrinks["Preference"])
print("Kruistabel Gender vs Preference:")
print(cross_gender)
print("-" * 50)

# 2. Chi-kwadraat toets (zonder Yates-correctie)
chi2_gender, p_gender, dof_gender, expected_gender = stats.chi2_contingency(cross_gender, correction=False)

print(f"Chi-kwadraat statistiek (χ²): {chi2_gender:.4f}")
print(f"p-waarde: {p_gender:.4f}")
print(f"Vrijheidsgraden: {dof_gender}")
```

```text
Uitleg:
Nulhypothese (H0): Drankvoorkeur en geslacht zijn onafhankelijk.
Interpretatie: p ≈ 0.2354 (of 0.1834 met Yates); beide > 0.05 ⇒ H0 niet verwerpen.
```

---

### Lab 4.02 — NBA salaries (positie vs salaris)

#### Opgave
Onderzoek of positie van een speler (`Position`) onafhankelijk is van zijn salarisklasse (kwartielen op basis van `Annual Salary`).

```text
Stap 1: Salaris opschonen — forceer naar string, verwijder '$' en '.' en zet om naar int.
Stap 2: Posities opschonen — neem alleen het eerste deel van samengestelde posities (C-F → C).
Stap 3: Salaris opdelen in kwartielen met `pd.qcut` en label Q1..Q4.
Stap 4: Maak mozaïekdiagram en voer χ²-toets uit op kruistabel(Position × Salary_Class).
```

```python
import numpy as np
import scipy.stats as stats
import pandas as pd
import matplotlib.pyplot as plt
from statsmodels.graphics.mosaicplot import mosaic
import seaborn as sns

# Data inlezen
nba = pd.read_csv('https://raw.githubusercontent.com/HoGentTIN/dsai-labs/main/data/NBA.csv', sep=';')

# 1. Salaris opschonen (veilig omzetten naar string en dan tekens vervangen)
nba['Annual Salary'] = (
    nba['Annual Salary']
    .astype(str)
    .str.replace('$', '', regex=False)
    .str.replace('.', '', regex=False)
)
nba['Annual Salary'] = nba['Annual Salary'].astype(int)

# 2. Posities opschonen (bijv. C-F → C)
nba['Position'] = nba['Position'].str.split('-').str[0]

# 3. Salaris opdelen in kwartielen
nba['Salary_Class'] = pd.qcut(nba['Annual Salary'], q=4, labels=['Q1', 'Q2', 'Q3', 'Q4'])

# 4. Mozaïekplot
plt.figure(figsize=(10,6))
mosaic(nba, ['Position', 'Salary_Class'], title='Mozaïekdiagram Positie vs Salaris')
plt.show()

# 5. Chi-kwadraat toets
kruistabel = pd.crosstab(nba['Position'], nba['Salary_Class'])
chi2, p_value, dof, expected = stats.chi2_contingency(kruistabel)
print(f"χ²-waarde: {chi2:.4f}")
print(f"p-waarde: {p_value:.4f}")
```

```text
Uitleg:
Resultaat: χ² ≈ 3.0344, p ≈ 0.8045 ⇒ H0 (onafhankelijkheid) niet verwerpen. Conclusie: geen verband tussen positie en salarisklasse.
```

---

### Lab 4.03

**Exercise 3 - Discrimination in schoolteacher hiring**

African Americans in a St. Louis suburb sued the city claiming they were discriminated against in schoolteacher hiring. Of the city's population, 5.7% were African American; of 405 teachers in the school system, 15 were African American. Set up appropriate hypotheses and determine whether African Americans are underrepresented.  
Calculate the standardized residuals.

**Results of the main calculations:**
- Chi-squared        χ² = 3.0027
- Critical value      g = 3.8415
- p-value             p = 0.0831
- standardized residuals for african american = -1.7328 > -2

```python
# Antwoord:
import numpy as np
import scipy.stats as stats

# 1. Gegevens definiëren
# Geobserveerde aantallen: 15 Afro-Amerikaanse leraren, de rest (405 - 15 = 390) is overig.
observed = np.array([15, 390])

# Verwachte percentages op basis van de populatie (5.7% Afro-Amerikaans)
expected_pct = np.array([0.057, 1 - 0.057])
total_teachers = 405

# Verwachte aantallen berekenen
expected = total_teachers * expected_pct

# 2. Chi-kwadraat aanpassingstoets (Goodness-of-Fit) uitvoeren
chi2_stat, p_value = stats.chisquare(f_obs=observed, f_exp=expected)

# Kritieke waarde bepalen bij een betrouwbaarheid van 95% (alpha = 0.05) en df = 1
critical_value = stats.chi2.ppf(0.95, df=1)

# 3. Gestandaardiseerde (gecorrigeerde) residuen berekenen
# Formule: (O - E) / sqrt(n * p * (1 - p))
adj_residuals = (observed - expected) / np.sqrt(total_teachers * expected_pct * (1 - expected_pct))

# 4. Resultaten netjes printen
print("--- Resultaten van de Chi-kwadraat toets ---")
print(f"Geobserveerd:            {observed}")
print(f"Verwacht:                {expected}")
print(f"Chi-kwadraat (χ²):       {chi2_stat:.4f}")
print(f"Kritieke waarde (g):     {critical_value:.4f}")
print(f"p-waarde:                {p_value:.4f}")
print(f"Gestandaardiseerd residu (African American): {adj_residuals[0]:.4f}")
```

```text
Statistische Interpretatie

Toetsing: Omdat de berekende χ² (3.0027) kleiner is dan de kritieke waarde (3.8415) én de p-waarde (0.0831) groter is dan het significantieniveau α = 0.05, kunnen we H0 niet verwerpen. Er is net niet genoeg statistisch bewijs om harde discriminatie of significante ondervertegenwoordiging aan te tonen op basis van deze steekproef alleen.

Residu: Het gestandaardiseerde residu voor de Afro-Amerikaanse leraren is −1.7328. Omdat deze waarde groter is dan −2 (dus minder extreem dan 2 standaardafwijkingen onder het verwachte getal), valt de afwijking binnen wat we nog aan "toevallige variatie" kunnen toeschrijven.
```

---

### Lab 4.04

A popular wisdom states that more children are born during certain phases of the lunar cycle, especially during the full moon. A classification of the number of births according to the lunar cycle was done in 2005.  
A sample of the number of births during different lunar cycles is given below.  
Is there a relationship between the lunar phase and the number of births?

```python
# Data sample: aantal geboorten per maanfase
dfmoon = pd.DataFrame(data={
    'lunar_phase': ['new moon', 'young crescent', 'first quarter', 'waxing moon', 'full moon', 'waning moon', 'last quarter', 'ashen moon'],
    'number_of_days': [24, 152, 24, 149, 24, 150, 24, 152],
    'number_of_births': [7680, 48442, 7579, 47814, 7711, 47595, 7733, 48230]
})

dfmoon.head(10)
```

**Answers:**

- chi-square statistic $\chi^2 \approx$ 0.1129
- $p \approx$ 0.999996485

```python
import numpy as np
import pandas as pd
import scipy.stats as stats

# 1. Dataframe aanmaken
dfmoon = pd.DataFrame(data={
    'lunar_phase': ['new moon', 'young crescent', 'first quarter', 'waxing moon', 'full moon', 'waning moon', 'last quarter', 'ashen moon'],
    'number_of_days': [24, 152, 24, 149, 24, 150, 24, 152],
    'number_of_births': [7680, 48442, 7579, 47814, 7711, 47595, 7733, 48230]
})

# 2. Bereken het gemiddeld aantal geboorten per dag voor elke maanfase (Geobserveerd)
dfmoon['births_per_day'] = dfmoon['number_of_births'] / dfmoon['number_of_days']

# 3. Bereken de verwachte waarde onder H0 (het algemene gemiddelde van de geboorten per dag)
expected_per_day = dfmoon['births_per_day'].mean()
dfmoon['expected_per_day'] = expected_per_day

# 4. Voer de Chi-kwadraat aanpassingstoets (Goodness-of-Fit) uit
chi2_stat, p_value = stats.chisquare(f_obs=dfmoon['births_per_day'], f_exp=dfmoon['expected_per_day'])

# Resultaten tonen
print(f"Chi-kwadraat statistiek (χ²): {chi2_stat:.4f}")
print(f"p-waarde:                    {p_value:.9f}")
```

```text
Stapsgewijze Uitleg

1. Waarom corrigeren voor het aantal dagen?
De verschillende fasen van de maanfase duren in de dataset niet allemaal even lang (sommige duren 24 dagen, andere rond de 150 dagen). Het is logisch dat er in een fase van 152 dagen meer kinderen worden geboren dan in een fase van 24 dagen. Daarom moeten we eerst kijken naar het gemiddeld aantal geboorten per dag binnen elke fase:
    Geboorten per dag = number_of_births / number_of_days

2. De Hypothesen opstellen
H0 (Nulhypothese): Er is geen verband tussen de maanfase en het aantal geboorten. Het gemiddeld aantal geboorten per dag is in elke fase gelijk.
H1 (Alternatieve hypothese): Er is wel een verband; in bepaalde maanfasen (zoals bij volle maan) worden er gemiddeld per dag meer of minder kinderen geboren.

3. De Verwachte Waarde bepalen
Onder de nulhypothese (H0) verwachten we dat het aantal geboorten per dag voor elke fase precies gelijk is aan het totale gemiddelde van alle fasen. Dit gemiddelde is ≈ 319.19 geboorten per dag. Dit getal dient als de verwachte waarde (E) voor alle 8 categorieën.

4. De Chi-kwadraat aanpassingstoets
Met de functie stats.chisquare() vergelijken we de geobserveerde geboorten per dag met de verwachte geboorten per dag.

Statistische Interpretatie en Conclusie
De p-waarde: De berekende p-waarde is 0.999996485 (nagenoeg 1 of 100%).

Conclusie: Omdat de p-waarde vele malen groter is dan het standaard significantieniveau (α = 0.05), kunnen we H0 absoluut niet verwerpen. Er is dus totaal geen statistisch bewijs voor de volkswijsheid dat er tijdens bepaalde maanfasen meer kinderen geboren worden.
```

---

### Lab 4.05

#### 1. Data inlezen en voorbereiden

**Opgave**

Laad het bestand `data/survey.csv`. Let op met ontbrekende waarden (NA): zorg ervoor dat de waarde `'None'` in de kolom `Exer` niet per ongeluk als een ontbrekende waarde wordt gezien. Zet daarna `Exer` en `Smoke` om naar ordinale variabelen met een logische volgorde.

**Oplossing (Code)**

```python
# Inlezen van de data met specifieke NA-instellingen
survey = pd.read_csv('data/survey.csv', na_values=['NA'], keep_default_na=False)

# Definiëren van de ordinale volgorde
exer_type = CategoricalDtype(categories=['None', 'Some', 'Freq'], ordered=True)
smoke_type = CategoricalDtype(categories=['Never', 'Occas', 'Regul', 'Heavy'], ordered=True)

# Typeconversie toepassen
survey['Exer'] = survey['Exer'].astype(exer_type)
survey['Smoke'] = survey['Smoke'].astype(smoke_type)
```

**Verklaring**

Standaard ziet pandas de string `'None'` als een synoniem voor `NaN` (ontbrekende data). Omdat studenten bij de vraag naar sporten letterlijk "Geen" (None) kunnen antwoorden, moeten we `keep_default_na=False` gebruiken en expliciet meegeven dat alleen de letterlijke tekst `'NA'` een ontbrekende waarde is. Door de kolommen om te zetten naar een geordende `CategoricalDtype` staan de categorieën straks in een logische volgorde in tabellen en grafieken.

---

#### 2. Functie voor Cramér's V

**Opgave**

Om voor elke combinatie de effectgrootte (Cramér's V) te berekenen, schrijven we eerst een herbruikbare Python-functie.

**Oplossing (Code)**

```python
def cramers_v(contingency_table):
    chi2 = stats.chi2_contingency(contingency_table)[0]
    n = contingency_table.sum().sum()
    r, k = contingency_table.shape
    return np.sqrt(chi2 / (n * min(r - 1, k - 1)))
```

---

#### 3. Analyse van de Variabelenparen

Voor elk paar voeren we telkens dezelfde stappen uit: een kruistabel maken, de χ²-onafhankelijkheidstoets uitvoeren en Cramér's V berekenen. De betrouwbaarheid is telkens α = 0.05.

**Paar 1: Exer (Sporten) vs. Smoke (Roken)**

**Oplossing (Code)**

```python
# 1. Kruistabel (onafhankelijke variabele eerst = rijen)
exer_smoke_table = pd.crosstab(survey['Exer'], survey['Smoke'])
print("--- Frequentietabel Exer/Smoke ---")
print(exer_smoke_table)

# 2. Chi-kwadraat toets
chi2, p, dof, expected = stats.chi2_contingency(exer_smoke_table)
g = stats.chi2.ppf(1 - 0.05, dof)
v = cramers_v(exer_smoke_table)

print(f"Chi-kwadraat (χ²): {chi2:.3f}")
print(f"Kritieke g-waarde: {g:.3f}")
print(f"p-waarde:          {p:.3f}")
print(f"Cramér's V:        {v:.3f}")
```

**Verklaring & Conclusie**

Resultaat: χ² ≈ 5.489, g ≈ 12.592, p ≈ 0.483, Cramér's V ≈ 0.089

Analyse: De berekende χ²-waarde (5.489) is veel kleiner dan de kritieke g-grens (12.592). Bovendien is de p-waarde (0.483) veel groter dan α = 0.05.

Conclusie: We aanvaarden de nulhypothese (H₀). Er is geen statistisch significant verband tussen hoe vaak een student sport en hoe vaak deze rookt.

---

**Paar 2: W.Hnd (Dominante hand) vs. Fold (Armen kruisen)**

**Oplossing (Code)**

```python
w_fold_table = pd.crosstab(survey['W.Hnd'], survey['Fold'])
print("\n--- Frequentietabel W.Hnd/Fold ---")
print(w_fold_table)

chi2, p, dof, expected = stats.chi2_contingency(w_fold_table)
g = stats.chi2.ppf(1 - 0.05, dof)
v = cramers_v(w_fold_table)

print(f"Chi-kwadraat (χ²): {chi2:.3f}")
print(f"Kritieke g-waarde: {g:.3f}")
print(f"p-waarde:          {p:.3f}")
print(f"Cramér's V:        {v:.3f}")
```

**Verklaring & Conclusie**

Resultaat: χ² ≈ 1.581, g ≈ 5.992, p ≈ 0.454, Cramér's V ≈ 0.083

Conclusie: We aanvaarden de nulhypothese (H₀). Welke hand dominant is, heeft geen invloed op welke arm bovenop ligt als een student de armen kruist.

---

**Paar 3: Sex (Geslacht) vs. Smoke (Roken)**

**Oplossing (Code)**

```python
sex_smoke_table = pd.crosstab(survey['Sex'], survey['Smoke'])
print("\n--- Frequentietabel Sex/Smoke ---")
print(sex_smoke_table)

chi2, p, dof, expected = stats.chi2_contingency(sex_smoke_table)
g = stats.chi2.ppf(1 - 0.05, dof)
v = cramers_v(sex_smoke_table)

print(f"Chi-kwadraat (χ²): {chi2:.3f}")
print(f"Kritieke g-waarde: {g:.3f}")
print(f"p-waarde:          {p:.3f}")
print(f"Cramér's V:        {v:.3f}")
```

**Verklaring & Conclusie**

Resultaat: χ² ≈ 3.554, g ≈ 7.815, p ≈ 0.314, Cramér's V ≈ 0.123

Conclusie: We aanvaarden de nulhypothese (H₀). Er is geen significant verband tussen het geslacht van de student en het rookgedrag.

---

**Paar 4: Sex (Geslacht) vs. W.Hnd (Dominante hand)**

**Oplossing (Code)**

```python
sex_hand_table = pd.crosstab(survey['Sex'], survey['W.Hnd'])
print("\n--- Frequentietabel Sex/W.Hnd ---")
print(sex_hand_table)

chi2, p, dof, expected = stats.chi2_contingency(sex_hand_table)
g = stats.chi2.ppf(1 - 0.05, dof)
v = cramers_v(sex_hand_table)

print(f"Chi-kwadraat (χ²): {chi2:.3f}")
print(f"Kritieke g-waarde: {g:.3f}")
print(f"p-waarde:          {p:.3f}")
print(f"Cramér's V:        {v:.3f}")
```

**Verklaring & Conclusie**

Resultaat: χ² ≈ 0.236, g ≈ 3.842, p ≈ 0.627, Cramér's V ≈ 0.032

Conclusie: We aanvaarden de nulhypothese (H₀). Of een student man of vrouw is, staat volledig los van het feit of ze links- of rechtshandig zijn.

---

### Lab 4.06

Hier vind je de volledige opbouw van het labo over de invloed van achtergrondmuziek op het aankoopgedrag van wijn.

#### 1. Data inlezen en opschonen

**Opgave**

Lees het bestand `data/MuziekWijn.csv` in. Controleer de kolomnamen en pas ze aan waar nodig om eventuele fouten (zoals spaties of typefouten) te corrigeren.

**Oplossing (Code)**

```python
# Inlezen van de dataset
df = pd.read_csv('data/MuziekWijn.csv')

# Kolomnamen controleren
print("Oorspronkelijke kolomnamen:", df.columns.tolist())

# Kolomnamen hernoemen (indien er onzichtbare spaties in zitten)
df = df.rename(columns={'Muziek': 'Muziek', ' Wijn': 'Wijn'})

print("Aangepaste kolomnamen:", df.columns.tolist())
```

**Verklaring**

Bij het inlezen van externe CSV-bestanden komt het vaak voor dat kolomnamen ongewenste spaties bevatten (bijv. `' Wijn'` in plaats van `'Wijn'`). Door gebruik te maken van `.rename(columns=...)` trekken we dit recht.

---

#### 2. Kruistabel en Marginale Totalen (Vraag 1 & 2)

**Opgave**

Stel de juiste kruistabel op waarbij de onafhankelijke variabele (`Muziek`) de rijen vormt en de afhankelijke variabele (`Wijn`) de kolommen. Bepaal de marginale totalen.

**Oplossing (Code)**

```python
# 1. Opstellen van de kruistabel (geobserveerde waarden)
kruistabel = pd.crosstab(df['Muziek'], df['Wijn'])
print("--- Kruistabel (Geobserveerde waarden) ---")
print(kruistabel)

# 2. Marginale totalen berekenen
kruistabel_margins = pd.crosstab(df['Muziek'], df['Wijn'], margins=True, margins_name="Totaal")
print("\n--- Kruistabel met Marginale Totalen ---")
print(kruistabel_margins)
```

**Verklaring**

De kruistabel geeft de werkelijk waargenomen frequenties weer. De marginale totalen tonen de sommen per rij en per kolom. Deze randtotalen zijn essentieel voor het berekenen van de verwachte waarden onder de nulhypothese.

---

#### 3. Verwachte Resultaten & Chi-kwadraat toets (Vraag 3 & 4)

**Opgave**

Bepaal de verwachte resultaten en bereken de χ²-toetsstatistiek.

**Oplossing (Code)**

```python
# Uitvoeren van de Chi-kwadraat onafhankelijkheidstoets
chi2, p_waarde, dof, expected_matrix = stats.chi2_contingency(kruistabel)

# 3. Omzetten van de verwachte resultaten naar een overzichtelijke DataFrame
verwachte_tabel = pd.DataFrame(expected_matrix, index=kruistabel.index, columns=kruistabel.columns)
print("--- Verwachte Resultaten ---")
print(verwachte_tabel.round(3))

# 4. Weergeven van de Chi-kwadraat statistiek
print(f"\nChi-kwadraat statistiek (χ²): {chi2:.3f}")
print(f"p-waarde:                    {p_waarde:.3f}")
print(f"Vrijheidsgraden (dof):       {dof}")
```

**Verklaring**

Verwachte resultaten worden berekend via de formule: Verwacht = (Rijtotaal × Kolomtotaal) / Algemeen Totaal

De χ²-statistiek (≈ 18.279) meet hoe sterk de geobserveerde waarden afwijken van de verwachte waarden. Omdat de bijbehorende p-waarde (0.001) kleiner is dan α = 0.05, verwerpen we de nulhypothese.

---

#### 4. Cramér's V berekenen (Vraag 5)

**Opgave**

Bereken Cramér's V en formuleer de eindconclusie over de sterkte van het verband.

**Oplossing (Code)**

```python
def bereken_cramers_v(tabel):
    chi2 = stats.chi2_contingency(tabel)[0]
    n = tabel.sum().sum()
    r, k = tabel.shape
    return np.sqrt(chi2 / (n * min(r - 1, k - 1)))

v = bereken_cramers_v(kruistabel)
print(f"Cramér's V: {v:.3f}")
```

**Verklaring & Conclusie**

Resultaat: Cramér's V ≈ 0.194.

Conclusie: Omdat Cramér's V tussen 0.10 en 0.30 ligt, spreken we van een zwak tot matig positief verband. De achtergrondmuziek heeft een aantoonbare invloed op het aankoopgedrag (Franse muziek stimuleert de verkoop van Franse wijn, Italiaanse muziek die van Italiaanse wijn), maar het is niet de enige of allesbepalende factor voor de consument.

---

#### 5. Visualisatie van de Dataset

**Opgave**

Maak de volgende drie grafieken aan:
1. Een staafdiagram dat de percentages van de verkochte wijnsoorten toont wanneer er geen muziek speelde.
2. Een geclusterd staafdiagram van de gehele dataset.
3. Een gestapeld staafdiagram van de gehele dataset.

**Oplossing (Code)**

```python
# --- Grafiek 1: Percentages wijnsoorten bij 'Geen muziek' ---
geen_muziek_data = kruistabel.loc['Geen']
geen_muziek_percentages = (geen_muziek_data / geen_muziek_data.sum()) * 100

plt.figure(figsize=(6, 4))
geen_muziek_percentages.plot(kind='bar', color=['royalblue', 'orange', 'green'])
plt.title("Percentage verkochte wijn bij 'Geen muziek'")
plt.ylabel("Percentage (%)")
plt.xlabel("Wijnsoort")
plt.xticks(rotation=0)
plt.grid(axis='y', linestyle='--', alpha=0.7)
plt.show()

# --- Grafiek 2: Geclusterd staafdiagram ---
kruistabel.plot(kind='bar', figsize=(8, 5), width=0.8)
plt.title("Wijnverkoop per achtergrondmuziek (Geclusterd)")
plt.ylabel("Aantal verkochte flessen")
plt.xlabel("Achtergrondmuziek")
plt.xticks(rotation=0)
plt.legend(title="Wijnsoort")
plt.grid(axis='y', linestyle='--', alpha=0.5)
plt.show()

# --- Grafiek 3: Gestapeld staafdiagram ---
kruistabel_relatief = kruistabel.div(kruistabel.sum(axis=1), axis=0) * 100

kruistabel_relatief.plot(kind='bar', stacked=True, figsize=(8, 5), colormap='viridis')
plt.title("Aandeel wijnsoorten per muziekstijl (Gestapeld %)")
plt.ylabel("Percentage (%)")
plt.xlabel("Achtergrondmuziek")
plt.xticks(rotation=0)
plt.legend(title="Wijnsoort", loc='upper left', bbox_to_anchor=(1, 1))
plt.grid(axis='y', linestyle='--', alpha=0.5)
plt.show()
```

---

### Lab 4.07

**Opgave: Exercise 7 - Digimeter sample**

Elk jaar voert Imec een studie uit naar het gebruik van digitale technologieën in Vlaanderen: de Digimeter (n = 2164). In deze oefening controleren we of de steekproef van de Digimeter representatief is voor de Vlaamse bevolking wat betreft de leeftijdscategorieën van de deelnemers.

Er wordt gebruikgemaakt van twee (fictieve) databestanden:
- `data/leeftijden-digimeter.csv`: Relatieve frequenties (percentages) van de leeftijd van deelnemers.
- `data/leeftijden-bestat-vl.csv`: Absolute frequenties van de verschillende leeftijdscategorieën van de totale Vlaamse bevolking (BelStat).

**De Deelvragen:**

1. Groepeer de BelStat-data zodat je exact dezelfde categorieën krijgt als de Digimeter (15-19, 20-29, 30-39, 40-49, 50-59, 60-64, 65+).
2. Bereken de absolute waargenomen frequenties (observed values) in de Digimeter-steekproef (n = 2164) op basis van de gegeven percentages.
3. Bereken de verwachte percentages (πᵢ) voor de totale Vlaamse bevolking op basis van de samengevoegde BelStat-data.
4. Voer een chi-kwadraat (χ²) aanpassingstest (goodness-of-fit test) uit. Is de steekproef representatief voor de Vlaamse bevolking?

---

#### Stap 0: Inlezen van de data

```python
# Inlezen van de digimeter data
digimeter = pd.read_csv('data/leeftijden-digimeter.csv')

if digimeter['Percentage'].max() > 1.0:
    digimeter['Percentage'] = digimeter['Percentage'] / 100

print(digimeter)
```

```python
# Inlezen van de BelStat data
bestat = pd.read_csv('data/leeftijden-bestat-vl.csv')
print(bestat)
```

---

#### Stap 1: Categorieën samenvoegen

```python
mapping = {
    '15-19': ['15-19'],
    '20-29': ['20-24', '25-29'],
    '30-39': ['30-34', '35-39'],
    '40-49': ['40-44', '45-49'],
    '50-59': ['50-54', '55-59'],
    '60-64': ['60-64'],
    '65+':   ['65-69', '70-74', '75-79', '80-84', '85-89', '90+']
}

population_counts = []
categories = ['15-19', '20-29', '30-39', '40-49', '50-59', '60-64', '65+']

for cat in categories:
    allowed_subcats = mapping[cat]
    total_for_cat = bestat[bestat['Categorie'].isin(allowed_subcats)]['Aantal'].sum()
    population_counts.append(total_for_cat)

bestat_grouped = pd.DataFrame({'Categorie': categories, 'Aantal': population_counts})
print(bestat_grouped)
```

---

#### Stap 2: Absolute waargenomen frequenties berekenen

```python
n = 2164  # Steekproefgrootte Digimeter

observed = digimeter['Percentage'] * n
print("Observed absolute frequencies:")
print(observed.values)
```

**Oplossing / Output:** `[142.824 307.288 324.6   352.732 374.372 157.972 502.048]`

---

#### Stap 3: Verwachte percentages (πᵢ) berekenen

```python
total_population = bestat_grouped['Aantal'].sum()
expected_percentages = bestat_grouped['Aantal'] / total_population

print("Expected percentages (pi_i):")
print(expected_percentages.values)
```

**Oplossing / Output:** `[0.06915147 0.1438298  0.15293412 0.17959016 0.16540295 0.07153788 0.21755361]`

---

#### Stap 4: Chi-kwadraat aanpassingstest uitvoeren

```python
expected = expected_percentages * n

chi2_stat, p_val = stats.chisquare(f_obs=observed, f_exp=expected)

alpha = 0.05
df = len(observed) - 1
g = stats.chi2.ppf(1 - alpha, df)

print(f"χ² (Chi-kwadraat)  : ≈ {chi2_stat:.3f}")
print(f"Degrees of freedom : {df}")
print(f"g (Kritieke waarde): ≈ {g:.3f}")
print(f"p-waarde           : ≈ {p_val:.3f}")
```

```python
if p_val < alpha:
    print("Conclusie: We verwerpen de nulhypothese (H0). De steekproef is NIET representatief.")
else:
    print("Conclusie: We kunnen de nulhypothese (H0) NIET verwerpen. De steekproef is WEL representatief voor de Vlaamse bevolking.")
```

**Eindconclusie / Oplossing**

Uit de statistische berekeningen komen de volgende resultaten naar voren:

- χ² ≈ 6.700
- df = 6
- g ≈ 12.592
- p ≈ 0.350

Omdat de behaalde teststatistiek (χ² = 6.700) kleiner is dan de kritieke waarde (g = 12.592) en de p-waarde (0.350) groter is dan het significantieniveau (α = 0.05), verwerpen we de nulhypothese niet.

**Conclusie:** Er is geen statistisch significant verschil tussen de leeftijdsverdeling in de steekproef en die van de Vlaamse bevolking. De steekproef van de Digimeter 2016 is dus representatief voor de Vlaamse bevolking op het gebied van leeftijdscategorieën.

---

### Lab 4.08

**Opgave: Exercise 8 - Off days**

Je runt een klein bedrijf met vier werknemers: Susan, Jimmy, Albert en Camilla. Omdat je altijd drie werknemers op de werkvloer nodig hebt, kan er telkens maar één werknemer per week vrij krijgen op zaterdag. Een van de werknemers beschuldigt je ervan dat je bepaalde collega's voortrekt.

**Doel:** Visualiseer de data en voer een geschikte statistische test uit om te bepalen of er sprake is van vriendjespolitiek, of dat de vrije dagen eerlijk verdeeld zijn.

---

#### Stap 1: Data inlezen en verkennen

```python
# 1. Inlezen van de dataset
df = pd.read_csv('data/off-days.csv')
print(df.head())

# 2. Bereken de waargenomen frequenties (observed) per werknemer
observed_counts = df['Employee'].value_counts()
print("\nWaargenomen vrije zaterdagen per werknemer:")
print(observed_counts)

n = len(df)
print(f"\nTotaal aantal geanalyseerde weken: {n}")
```

---

#### Stap 2: Visualisatie (Staafdiagram)

```python
plt.figure(figsize=(8, 5))
sns.countplot(data=df, x='Employee', order=observed_counts.index, palette='viridis')
plt.title('Aantal vrije zaterdagen per werknemer')
plt.xlabel('Werknemer')
plt.ylabel('Aantal keren vrij')
plt.show()
```

---

#### Stap 3: Hypothesetest (Chi-kwadraat aanpassingstest)

Als er geen sprake is van voortrekken (H₀), heeft elke werknemer een even grote kans (p = 0.25) om vrij te krijgen. De verwachte frequentie voor iedereen is dus: n / 4.

```python
k = len(observed_counts)
expected_counts = [n / k] * k

chi2_stat, p_val = stats.chisquare(f_obs=observed_counts, f_exp=expected_counts)

alpha = 0.05
df_degrees = k - 1
g = stats.chi2.ppf(1 - alpha, df_degrees)

print(f"χ² (Chi-kwadraat teststatistiek): ≈ {chi2_stat:.3f}")
print(f"Degrees of freedom (vrijheidsgraden): {df_degrees}")
print(f"g (Kritieke waarde)            : ≈ {g:.3f}")
print(f"p-waarde                       : ≈ {p_val:.3f}")

print("\n--- Conclusie ---")
if p_val < alpha:
    print("p < alpha: We verwerpen de nulhypothese (H0).")
    print("Er is een statistisch significant verschil! Je trekt waarschijnlijk werknemers voor.")
else:
    print("p >= alpha: We kunnen de nulhypothese (H0) niet verwerpen.")
    print("De verschillen zijn statistisch niet significant. De vrije dagen zijn eerlijk verdeeld.")
```

**Formele Hypothesen:**

- H₀: De vrije zaterdagen zijn uniform verdeeld over de vier werknemers (er is geen favoritisme).
- H₁: De vrije zaterdagen zijn niet uniform verdeeld (bepaalde werknemers worden voorgetrokken of benadeeld).

Significantieniveau: α = 0.05

---

### Lab 4.09

**Opgave: Exercise Golf balls**

Callaway overweegt om de golfbalmarkt te betreden. Het bedrijf zal winst maken als het marktaandeel meer dan 20% is. Een marktonderzoek toont aan dat 140 van de 624 kopers van golfballen een Callaway golfbal zouden kopen.

**Vraag:** Is er voldoende bewijs om Callaway te overtuigen de golfbalmarkt te betreden?

**Verwachte uitkomsten:**
- χ² = 2.3141
- p-waarde = 0.1282

---

#### Cel 1: Waargenomen en verwachte waarden definiëren

```python
n_total = 624
observed_callaway = 140
observed_other = n_total - observed_callaway

observed_counts = np.array([observed_callaway, observed_other])

p_callaway = 0.20
p_other = 1.00 - p_callaway

expected_counts = np.array([n_total * p_callaway, n_total * p_other])

print("Waargenomen aantallen [Callaway, Overig]:", observed_counts)
print("Verwachte aantallen   [Callaway, Overig]:", expected_counts)
```

---

#### Cel 2: Chi-kwadraat test uitvoeren

```python
chi2_stat, p_val = stats.chisquare(f_obs=observed_counts, f_exp=expected_counts)

alpha = 0.05
df_degrees = len(observed_counts) - 1
g = stats.chi2.ppf(1 - alpha, df_degrees)

print(f"χ² (Chi-kwadraat teststatistiek): ≈ {chi2_stat:.4f}")
print(f"Degrees of freedom (vrijheidsgraden): {df_degrees}")
print(f"g (Kritieke waarde)            : ≈ {g:.4f}")
print(f"p-waarde                       : ≈ {p_val:.4f}")
```

---

#### Cel 3: Evaluatie en Conclusie

```python
print("--- Statistische Evaluatie ---")
if p_val < alpha:
    print(f"p-waarde ({p_val:.4f}) < alpha ({alpha}): We verwerpen de nulhypothese (H0).")
    print("Het marktaandeel wijkt significant af van de 20%.")
else:
    print(f"p-waarde ({p_val:.4f}) >= alpha ({alpha}): We kunnen de nulhypothese (H0) niet verwerpen.")
    print("Het verschil met de verwachte 20% is statistisch niet significant.")
```

**Formele Hypotheseproces & Conclusie**

**Hypothesen:**
- H₀: Het marktaandeel van Callaway is gelijk aan 20% (π = 0.20).
- H₁: Het marktaandeel van Callaway wijkt af van de 20% (π ≠ 0.20).

**Resultaten:**
- χ² ≈ 2.3141
- df = 1
- p-waarde ≈ 0.1282

**Conclusie:** Omdat de p-waarde (0.1282) groter is dan het standaard significantieniveau (α = 0.05), kunnen we de nulhypothese niet verwerpen. De geobserveerde 140 kopers (≈ 22.4%) wijken weliswaar licht af van de vereiste 20%, maar deze afwijking is statistisch gezien te klein om met zekerheid te zeggen dat het niet op toeval berust. Er is op dit moment niet genoeg statistisch bewijs om Callaway met zekerheid te overtuigen de golfbalmarkt te betreden.

---

### Example exam questions 4.10

Hier vind je de opgaven, de bijbehorende Python-code en de statistische uitwerkingen voor alle oefeningen uit het voorbeeldexamen van Hoofdstuk 4.

---

#### Exercise 1: Weather and Crime

Criminologen debatteren al lang over de vraag of er een verband bestaat tussen weersomstandigheden en het aantal gepleegde misdrijven. In 1988 werden 1361 moorden geclassificeerd per seizoen. Is er een associatie tussen het seizoen en het aantal gepleegde moorden?

Omdat we hier één kwalitatieve variabele (seizoen) testen tegenover een verwachte gelijke (uniforme) verdeling, gebruiken we de **Chi-kwadraat aanpassingstest (Goodness-of-Fit test)**.

**Code**

```python
observed_murders = dfmurders['number_of_murders'].values
total_murders = observed_murders.sum()

expected_murders = [total_murders / 4] * 4

chi2_stat, p_val = stats.chisquare(f_obs=observed_murders, f_exp=expected_murders)

alpha = 0.05
df = len(observed_murders) - 1
g = stats.chi2.ppf(1 - alpha, df)

print(f"χ² (Chi-kwadraat)  : ≈ {chi2_stat:.4f}")
print(f"Degrees of freedom : {df}")
print(f"g (Kritieke waarde): ≈ {g:.4f}")
print(f"p-waarde           : ≈ {p_val:.4f}")
```

**Uitwerking & Conclusie**

Hypothesen:
- H₀: Er is geen associatie tussen het seizoen en het aantal moorden (de moorden zijn gelijk verdeeld).
- H₁: Er is wel een associatie tussen het seizoen en het aantal moorden.

Resultaat: χ² ≈ 4.0132, df = 3, p ≈ 0.2599, g ≈ 7.8147.

Conclusie: Omdat p ≈ 0.2599 > 0.05 (en χ² < g), kunnen we de nulhypothese niet verwerpen. Er is onvoldoende bewijs dat het seizoen invloed heeft op het aantal moorden.

---

#### Exercise 2: Education and Spirituality

Er is een steekproef genomen waarbij individuen zijn ingedeeld op basis van hun opleidingsniveau/discipline en hun mate van spiritualiteit. Is er een associatie tussen het opleidingsniveau en het gevoel van spiritualiteit?

Omdat we hier te maken hebben met een kruistabel van twee kwalitatieve variabelen, gebruiken we de **Chi-kwadraat onafhankelijkheidstest (Test for Independence)**.

**Code**

```python
contingency_table = dfspirituality.set_index('sense_of_spirituality')

chi2_stat, p_val, df, expected = stats.chi2_contingency(contingency_table)

alpha = 0.05
g = stats.chi2.ppf(1 - alpha, df)

print(f"χ² (Chi-kwadraat)  : ≈ {chi2_stat:.4f}")
print(f"Degrees of freedom : {df}")
print(f"g (Kritieke waarde): ≈ {g:.4f}")
print(f"p-waarde           : ≈ {p_val:.4e}")
```

**Uitwerking & Conclusie**

Hypothesen:
- H₀: Opleidingsniveau en spiritualiteit zijn onafhankelijk (er is geen associatie).
- H₁: Opleidingsniveau en spiritualiteit zijn afhankelijk (er is wel een associatie).

Resultaat: χ² ≈ 123.7997, df = 6, p ≈ 3.3283 × 10⁻²⁴, g ≈ 12.5916.

Conclusie: De p-waarde is extreem klein (p < 0.05) en χ² is vele malen groter dan de kritieke waarde g. We verwerpen de nulhypothese. Er is een zeer sterke, statistisch significante associatie tussen iemands achtergrond/opleiding en diens mate van spiritualiteit.

---

#### Exercise 4: Drug Experiment and Patient State

In een experiment kregen drie groepen patiënten een placebo, een halve dosis, of een volledige dosis van een nieuw medicijn. Er is geregistreerd hoe zij zich achteraf voelden (beter, gelijk, slechter). Is er op een 5% significantieniveau bewijs dat er een verschil is in de conditie tussen de drie groepen patiënten?

Ook dit is een vergelijking tussen meerdere categorische groepen, dus we gebruiken de **Chi-kwadraat onafhankelijkheidstest / homogeniteitstest**.

**Code**

```python
chi2_stat, p_val, df, expected = stats.chi2_contingency(dfexperiment)

alpha = 0.05
g = stats.chi2.ppf(1 - alpha, df)

print(f"χ² (Chi-kwadraat)  : ≈ {chi2_stat:.4f}")
print(f"Degrees of freedom : {df}")
print(f"g (Kritieke waarde): ≈ {g:.4f}")
print(f"p-waarde           : ≈ {p_val:.4f}")
```

**Uitwerking & Conclusie**

Hypothesen:
- H₀: Er is geen verschil in de conditie tussen de drie behandelingsgroepen.
- H₁: Er is wel een verschil in de conditie tussen de drie behandelingsgroepen.

Resultaat: χ² ≈ 1.5187, df = 4, p ≈ 0.8234, g ≈ 9.4877.

Conclusie: Omdat de p-waarde (0.8234) aanzienlijk groter is dan α = 0.05, kunnen we de nulhypothese niet verwerpen. Er is op basis van dit experiment geen statistisch bewijs dat de verandering in de toestand van de patiënt verschilt tussen de placebo-, halve dosis- of volledige dosisgroep.
