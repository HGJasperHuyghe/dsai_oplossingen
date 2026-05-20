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

### Lab 4.03 
 Exercise 3 - Discrimination in schoolteacher hiring

African Americans in a St. Louis suburb sued the city 
claiming they were discriminated against in schoolteacher hiring. Of the city's population, 5.7% were 
African American; of 405 teachers in the school system, 15 were African American. Set up appropriate 
hypotheses and determine whether African Americans 
are underrepresented.  
Calculate the standardized residuals. 

Results of the main calculations:
- Chi-squared        χ² = 3.0027
- Critical value      g = 3.8415
- p-value             p = 0.0831
- standardized residuals for african american = -1.7328 > - 2
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
Statistische InterpretatieToetsing: Omdat de berekende $\chi^2$ ($3.0027$) kleiner is dan de kritieke waarde ($3.8415$) én de $p$-waarde ($0.0831$) groter is dan het significantieniveau $\alpha = 0.05$, kunnen we $H_0$ niet verwerpen. Er is net niet genoeg statistisch bewijs om harde discriminatie of significante ondervertegenwoordiging aan te tonen op basis van deze steekproef alleen.

Residu: Het gestandaardiseerde residu voor de Afro-Amerikaanse leraren is $-1.7328$. Omdat deze waarde groter is dan $-2$ (dus minder extreem dan 2 standaardafwijkingen onder het verwachte getal), valt de afwijking binnen wat we nog aan "toevallige variatie" kunnen toeschrijven.
```

### Lab 4.04
### Lab 4.04
A popular wisdom states that more children are born during certain phases of the lunar cycle, especially during the full moon. A classification of the number of births according to the lunar cycle was done in 2005.
A sample of the number of births during different lunar cycles is given below.  
Is there a relationship between the lunar phase and the number of births?
```python
# Data sample: aantal geboorten per maanfase
dfmoon = pd.DataFrame(data={'lunar_phase': ['new moon', 'young crescent', 'first quarter', 'waxing moon', 'full moon', 'waning moon', 'last quarter', 'ashen moon'],
                            'number_of_days': [24, 152, 24, 149, 24, 150, 24, 152],
                            'number_of_births': [7680, 48442, 7579, 47814, 7711, 47595, 7733, 48230]})

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
$$\text{Geboorten per dag} = \frac{\text{number\_of\_births}}{\text{number\_of\_days}}$$

2. De Hypothesen opstellen
$H_0$ (Nulhypothese): Er is geen verband tussen de maanfase en het aantal geboorten. Het gemiddeld aantal geboorten per dag is in elke fase gelijk.
$H_1$ (Alternatieve hypothese): Er is wel een verband; in bepaalde maanfasen (zoals bij volle maan) worden er gemiddeld per dag meer of minder kinderen geboren.

3. De Verwachte Waarde bepalen
Onder de nulhypothese ($H_0$) verwachten we dat het aantal geboorten per dag voor elke fase precies gelijk is aan het totale gemiddelde van alle fasen. Dit gemiddelde is $\approx 319.19$ geboorten per dag. Dit getal dient als de verwachte waarde ($E$) voor alle 8 de categorieën.

4. De Chi-kwadraat aanpassingstoets
Met de functie stats.chisquare() vergelijken we de geobserveerde geboorten per dag met de verwachte geboorten per dag. De formule die op de achtergrond wordt uitgevoerd is:
$$\chi^2 = \sum \frac{(O - E)^2}{E}$$

Dit levert een extreem lage $\chi^2$-waarde op van $0.1129$.

Statistische Interpretatie en Conclusie
De $p$-waarde: De berekende $p$-waarde is $0.999996485$ (nagenoeg $1$ of $100\%$).

Conclusie: Omdat de $p$-waarde vele malen groter is dan het standaard significantieniveau ($\alpha = 0.05$), kunnen we $H_0$ absoluut niet verwerpen.

Een $p$-waarde die zo dicht bij de $1$ ligt, betekent dat de geobserveerde dagelijkse geboortecijfers in de praktijk nagenoeg perfect overeenkomen met de theoretisch verwachte waarden. Er is dus totaal geen statistisch bewijs voor de volkswijsheid dat er tijdens bepaalde maanfasen (zoals volle maan) meer kinderen geboren worden. De maanfase en het aantal geboorten zijn volledig onafhankelijk van elkaar.
```

### Lab 4.05
1. Data inlezen en voorbereiden
Opgave
Laad het bestand data/survey.csv. Let op met ontbrekende waarden (NA): zorg ervoor dat de waarde 'None' in de kolom Exer niet per ongeluk als een ontbrekende waarde wordt gezien. Zet daarna Exer en Smoke om naar ordinale variabelen met een logische volgorde.

Oplossing (Code)
```python
# Inlezen van de data met specifieke NA-instellingen
survey = pd.read_csv('data/survey.csv', na_values=['NA'], keep_default_na=False)

# Unieke waarden bekijken (optioneel, om de categorieën te controleren)
# print(survey['Exer'].unique())
# print(survey['Smoke'].unique())

# Definiëren van de ordinale volgorde
exer_type = CategoricalDtype(categories=['None', 'Some', 'Freq'], ordered=True)
smoke_type = CategoricalDtype(categories=['Never', 'Occas', 'Regul', 'Heavy'], ordered=True)

# Typeconversie toepassen
survey['Exer'] = survey['Exer'].astype(exer_type)
survey['Smoke'] = survey['Smoke'].astype(smoke_type)
```

```text
Verklaring
Standaard ziet pandas de string 'None' als een synoniem voor NaN (ontbrekende data). Omdat studenten bij de vraag naar sporten letterlijk "Geen" (None) kunnen antwoorden, moeten we keep_default_na=False gebruiken en expliciet meegeven dat alleen de letterlijke tekst 'NA' een ontbrekende waarde is. Door de kolommen om te zetten naar een geordende CategoricalDtype staan de categorieën straks in een logische volgorde in onze tabellen en grafieken (bijv. van 'Nooit' naar 'Vaak' roken).
```

2. Functie voor Cramér's V
Opgave
Om voor elke combinatie de effectgrootte (Cramér's V) te berekenen, schrijven we eerst een herbruikbare Python-functie.

Oplossing (Code)
```python
def cramers_v(contingency_table):
    chi2 = stats.chi2_contingency(contingency_table)[0]
    n = contingency_table.sum().sum()
    r, k = contingency_table.shape
    return np.sqrt(chi2 / (n * min(r - 1, k - 1)))
```
# 3. Analyse van de Variabelenparen
Voor elk paar voeren we telkens dezelfde stappen uit: een kruistabel maken, de $\chi^2$-onafhankelijkheidstoets uitvoeren en Cramér's V berekenen. De betrouwbaarheid is telkens $\alpha = 0.05$.

Paar 1: Exer (Sporten) vs. Smoke (Roken)
Oplossing (Code)
```python
# 1. Kruistabel (onafhankelijke variabele eerst = rijen)
exer_smoke_table = pd.crosstab(survey['Exer'], survey['Smoke'])
print("--- Frequentietabel Exer/Smoke ---")
print(exer_smoke_table)

# 2. Visualisatie (optioneel via een mozaïekdiagram)
mosaic(survey, ['Exer', 'Smoke'])
plt.show()

# 3. Chi-kwadraat toets
chi2, p, dof, expected = stats.chi2_contingency(exer_smoke_table)
g = stats.chi2.ppf(1 - 0.05, dof)
v = cramers_v(exer_smoke_table)

print(f"Chi-kwadraat (χ²): {chi2:.3f}")
print(f"Kritieke g-waarde: {g:.3f}")
print(f"p-waarde:          {p:.3f}")
print(f"Cramér's V:        {v:.3f}")
```

```text
Verklaring & Conclusie
Resultaat: $\chi^2 \approx 5.489$, $g \approx 12.592$, $p \approx 0.483$, Cramér's $V \approx 0.089$

Analyse: De berekende $\chi^2$-waarde ($5.489$) is veel kleiner dan de kritieke g-grens ($12.592$). Bovendien is de $p$-waarde ($0.483$) veel groter dan $\alpha = 0.05$.

Conclusie: We aanvaarden de nulhypothese ($H_0$). Dit betekent dat er geen statistisch significant verband is tussen hoe vaak een student sport en hoe vaak deze rookt. Cramér's V ligt dicht bij 0, wat duidt op een verwaarloosbaar tot zeer zwak effect.
```

Paar 2: W.Hnd (Dominante hand) vs. Fold (Armen kruisen)
Oplossing (Code)
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

```text
Verklaring & Conclusie
Resultaat: $\chi^2 \approx 1.581$, $g \approx 5.992$, $p \approx 0.454$, Cramér's $V \approx 0.083$

Analyse: De $p$-waarde ($0.454 > 0.05$) toont aan dat het resultaat niet statistisch significant is.

Conclusie: We aanvaarden de nulhypothese ($H_0$). Welke hand dominant is (links of rechts), heeft geen invloed op welke arm bovenop ligt als een student de armen kruist. De twee variabelen zijn onafhankelijk.
```

Paar 3: Sex (Geslacht) vs. Smoke (Roken)
Oplossing (Code)
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

```text
Verklaring & Conclusie
Resultaat: $\chi^2 \approx 3.554$, $g \approx 7.815$, $p \approx 0.314$, Cramér's $V \approx 0.123$

Analyse: Wederom ligt de $\chi^2$-waarde onder de kritieke g-grens en is de $p$-waarde groter dan $0.05$.

Conclusie: We aanvaarden de nulhypothese ($H_0$). Er is geen significant verband tussen het geslacht van de student en het rookgedrag.
```

Paar 4: Sex (Geslacht) vs. W.Hnd (Dominante hand)
Oplossing (Code)
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

```text
Verklaring & Conclusie
Resultaat: $\chi^2 \approx 0.236$, $g \approx 3.842$, $p \approx 0.627$, Cramér's $V \approx 0.032$

Analyse: De extreem lage $\chi^2$-waarde en de zeer hoge $p$-waarde ($0.627$) geven aan dat de geobserveerde tabel nagenoeg perfect overeenkomt met wat je bij onafhankelijkheid mag verwachten.

Conclusie: We aanvaarden de nulhypothese ($H_0$). Of een student man of vrouw is, staat volledig los van het feit of ze links- of rechtshandig zijn. Cramér's V is nagenoeg $0$, wat duidt op de totale afwezigheid van een effect binnen deze steekproef.
```

### Lab 4.06
Hier vind je de volledige opbouw van het labo over de invloed van achtergrondmuziek op het aankoopgedrag van wijn. De structuur bevat telkens de opgave, de bijbehorende Python-code en een heldere verklaring van de resultaten.1. Data inlezen en opschonenOpgaveLees het bestand data/MuziekWijn.csv in. Controleer de kolomnamen en pas ze aan waar nodig om eventuele fouten (zoals spaties of typefouten) te corrigeren.Oplossing (Code)Python# Inlezen van de dataset
df = pd.read_csv('data/MuziekWijn.csv')

# Kolomnamen controleren
print("Oorspronkelijke kolomnamen:", df.columns.tolist())

# Kolomnamen hernoemen (indien er bijvoorbeeld onzichtbare spaties of typefouten in zitten)
df = df.rename(columns={'Muziek': 'Muziek', ' Wijn': 'Wijn'})

# Controle van de aanpassing
print("Aangepaste kolomnamen:", df.columns.tolist())
VerklaringBij het inlezen van externe CSV-bestanden komt het vaak voor dat kolomnamen ongewenste spaties bevatten (bijvoorbeeld ' Wijn' in plaats van 'Wijn'). Door gebruik te maken van .rename(columns=...) trekken we dit recht, zodat de kolommen later in de code foutloos aangeroepen kunnen worden.2. Kruistabel en Marginale Totalen (Vraag 1 & 2)OpgaveStel de juiste kruistabel op waarbij de onafhankelijke variabele (Muziek) de rijen vormt en de afhankelijke variabele (Wijn) de kolommen.Bepaal de marginale totalen (de rijsommen en kolomsommen).Oplossing (Code)Python# 1. Opstellen van de kruistabel (geobserveerde waarden)
kruistabel = pd.crosstab(df['Muziek'], df['Wijn'])
print("--- Kruistabel (Geobserveerde waarden) ---")
print(kruistabel)

# 2. Marginale totalen berekenen door 'margins=True' toe te voegen
kruistabel_margins = pd.crosstab(df['Muziek'], df['Wijn'], margins=True, margins_name="Totaal")
print("\n--- Kruistabel met Marginale Totalen ---")
print(kruistabel_margins)
VerklaringDe kruistabel geeft de werkelijk waargenomen frequenties (geobserveerde waarden) weer. De marginale totalen tonen de sommen per rij (hoeveel flessen er in totaal per muziektype zijn verkocht) en per kolom (hoeveel flessen er in totaal per wijnsoort zijn verkocht). Deze randtotalen zijn essentieel voor het berekenen van de verwachte waarden onder de nulhypothese.3. Verwachte Resultaten & Chi-kwadraat toets (Vraag 3 & 4)OpgaveBepaal de verwachte resultaten (expected values) wanneer er van wordt uitgegaan dat er geen verband is.Bereken de $\chi^2$-toetsstatistiek.Oplossing (Code)Python# Uitvoeren van de Chi-kwadraat onafhankelijkheidstoets
chi2, p_waarde, dof, expected_matrix = stats.chi2_contingency(kruistabel)

# 3. Omzetten van de verwachte resultaten naar een overzichtelijke DataFrame
verwachte_tabel = pd.DataFrame(expected_matrix, index=kruistabel.index, columns=kruistabel.columns)
print("--- Verwachte Resultaten ---")
print(verwachte_tabel.round(3))

# 4. Weergeven van de Chi-kwadraat statistiek en de p-waarde
print(f"\nChi-kwadraat statistiek (χ²): {chi2:.3f}")
print(f"p-waarde:                    {p_waarde:.3f}")
print(f"Vrijheidsgraden (dof):       {dof}")
VerklaringVerwachte resultaten: Dit zijn de frequenties die we zouden verwachten als de achtergrondmuziek totaal geen invloed zou hebben op de wijnkeuze. Ze worden berekend via de formule:$$\text{Verwacht} = \frac{\text{Rijtotaal} \times \text{Kolomtotaal}}{\text{Algemeen Totaal}}$$$\chi^2$-statistiek ($\approx 18.279$): Deze waarde meet hoe sterk de geobserveerde waarden afwijken van de verwachte waarden. Omdat de bijbehorende $p$-waarde ($0.001$) kleiner is dan het standaard significantieniveau $\alpha = 0.05$, verwerpen we de nulhypothese. Er is dus een statistisch significant verband tussen de achtergrondmuziek en het type wijn dat klanten kopen.4. Cramér's V berekenen (Vraag 5)OpgaveBereken Cramér's V en formuleer de eindconclusie over de sterkte van het verband.Oplossing (Code)Python# Functie voor Cramér's V
def bereken_cramers_v(tabel):
    chi2 = stats.chi2_contingency(tabel)[0]
    n = tabel.sum().sum()
    r, k = tabel.shape
    return np.sqrt(chi2 / (n * min(r - 1, k - 1)))

# Cramér's V berekenen voor de wijn-muziek tabel
v = bereken_cramers_v(kruistabel)
print(f"Cramér's V: {v:.3f}")
Verklaring & ConclusieResultaat: Cramér's $V \approx 0.194$.Conclusie: Omdat Cramér's V tussen $0.10$ en $0.30$ ligt, spreken we van een zwak tot matig positief verband. De achtergrondmuziek heeft een aantoonbare invloed op het aankoopgedrag (Franse muziek stimuleert de verkoop van Franse wijn, Italiaanse muziek die van Italiaanse wijn), maar het is niet de enige of allesbepalende factor voor de consument.5. Visualisatie van de DatasetOpgaveMaak de volgende drie grafieken aan om de data visueel in kaart te brengen:Een staafdiagram dat de percentages van de verkochte wijnsoorten toont wanneer er geen muziek speelde.Een geclusterd staafdiagram (clustered bar chart) van de gehele dataset.Een gestapeld staafdiagram (stacked bar chart) van de gehele dataset.Oplossing (Code)Python# --- Grafiek 1: Percentages wijnsoorten bij 'Geen muziek' ---
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


# --- Grafiek 2: Geclusterd staafdiagram (Clustered Bar Chart) ---
kruistabel.plot(kind='bar', figsize=(8, 5), width=0.8)
plt.title("Wijnverkoop per achtergrondmuziek (Geclusterd)")
plt.ylabel("Aantal verkochte flessen")
plt.xlabel("Achtergrondmuziek")
plt.xticks(rotation=0)
plt.legend(title="Wijnsoort")
plt.grid(axis='y', linestyle='--', alpha=0.5)
plt.show()


# --- Grafiek 3: Gestapeld staafdiagram (Stacked Bar Chart) ---
# We zetten de tabel eerst om naar percentages per rij voor een eerlijke vergelijking
kruistabel_relatief = kruistabel.div(kruistabel.sum(axis=1), axis=0) * 100

kruistabel_relatief.plot(kind='bar', stacked=True, figsize=(8, 5), colormap='viridis')
plt.title("Aandeel wijnsoorten per muziekstijl (Gestapeld %)")
plt.ylabel("Percentage (%)")
plt.xlabel("Achtergrondmuziek")
plt.xticks(rotation=0)
plt.legend(title="Wijnsoort", loc='upper left', bbox_to_anchor=(1, 1))
plt.grid(axis='y', linestyle='--', alpha=0.5)
plt.show()

### Lab 4.07
Opgave: Exercise 7 - Digimeter sampleElk jaar voert Imec een studie uit naar het gebruik van digitale technologieën in Vlaanderen: de Digimeter ($n = 2164$). In deze oefening controleren we of de steekproef van de Digimeter representatief is voor de Vlaamse bevolking wat betreft de leeftijdscategorieën van de deelnemers.Er wordt gebruikgemaakt van twee (fictieve) databestanden:data/leeftijden-digimeter.csv: Relatieve frequenties (percentages) van de leeftijd van deelnemers.data/leeftijden-bestat-vl.csv: Absolute frequenties van de verschillende leeftijdscategorieën van de totale Vlaamse bevolking (BelStat).De Deelvragen:De tabel met leeftijdsgegevens voor de Vlaamse bevolking heeft meer categorieën dan die van de Digimeter. Groepeer de BelStat-data zodat je exact dezelfde categorieën krijgt als de Digimeter (15-19, 20-29, 30-39, 40-49, 50-59, 60-64, 65+).Bereken de absolute waargenomen frequenties (observed values) in de Digimeter-steekproef ($n = 2164$) op basis van de gegeven percentages.Bereken de verwachte percentages ($\pi_i$) voor de totale Vlaamse bevolking op basis van de samengevoegde BelStat-data.Voer een chi-kwadraat ($\chi^2$) aanpassingstest (goodness-of-fit test) uit. Is de steekproef representatief voor de Vlaamse bevolking?🛠️ Code & Oplossingen per celStap 0: Inlezen van de dataVoeg eerst de code toe om de datasets correct in te laden en de percentages om te zetten naar bruikbare fracties/getallen.Cel: Read the dataset data/leeftijden-digimeter.csvPython# Inlezen van de digimeter data
digimeter = pd.read_csv('data/leeftijden-digimeter.csv')

# Zorg ervoor dat de percentages echte getallen/fracties worden (indien ze als string of 0-100 staan)
# We gaan ervan uit dat de kolom 'Percentage' heet. Als deze al tussen 0 en 100 staat, delen we door 100.
if digimeter['Percentage'].max() > 1.0:
    digimeter['Percentage'] = digimeter['Percentage'] / 100

print(digimeter)
Cel: Read the dataset leeftijden-bestat-vl.csvPython# Inlezen van de BelStat data
bestat = pd.read_csv('data/leeftijden-bestat-vl.csv')
print(bestat)
Stap 1: Categorieën samenvoegenCel: Code voor vraag 1Python# Groepeer de BelStat data handmatig naar de 7 categorieën van de Digimeter.
# Opmerking: Pas de kolomnamen aan op basis van je exacte CSV-structuur (bijv. 'Leeftijd' en 'Aantal').

# Voorbeeld van een mapping (pas de exacte leeftijdsklassen aan conform je dataset):
mapping = {
    '15-19': ['15-19'],
    '20-29': ['20-24', '25-29'],
    '30-39': ['30-34', '35-39'],
    '40-49': ['40-44', '45-49'],
    '50-59': ['50-54', '55-59'],
    '60-64': ['60-64'],
    '65+':   ['65-69', '70-74', '75-79', '80-84', '85-89', '90+'] # alle oudere categorieën
}

# Transformeer of aggregeer de data op basis van deze nieuwe categorieën
# (Dit is een algemene opzet, afhankelijk van hoe de CSV is opgebouwd):
population_counts = []
categories = ['15-19', '20-29', '30-39', '40-49', '50-59', '60-64', '65+']

# Stel dat 'bestat' kolommen 'Categorie' en 'Aantal' heeft:
for cat in categories:
    allowed_subcats = mapping[cat]
    total_for_cat = bestat[bestat['Categorie'].isin(allowed_subcats)]['Aantal'].sum()
    population_counts.append(total_for_cat)

bestat_grouped = pd.DataFrame({'Categorie': categories, 'Aantal': population_counts})
print(bestat_grouped)
Stap 2: Absolute waargenomen frequenties berekenenCel: Code voor vraag 2Pythonn = 2164  # Steekproefgrootte Digimeter

# Bereken de absolute waargenomen waarden (observed)
# We vermenigvuldigen de percentages uit de digimeter met de totale n
observed = digimeter['Percentage'] * n

print("Observed absolute frequencies:")
print(observed.values)
Oplossing / Output:[142.824 307.288 324.6   352.732 374.372 157.972 502.048]Stap 3: Verwachte percentages ($\pi_i$) berekenenCel: Code voor vraag 3Python# Bereken de totale Vlaamse bevolking in de relevante categorieën
total_population = bestat_grouped['Aantal'].sum()

# Verwachte percentages (pi_i) zijn de deelfrequenties gedeeld door het totaal
expected_percentages = bestat_grouped['Aantal'] / total_population

print("Expected percentages (pi_i):")
print(expected_percentages.values)
Oplossing / Output:[0.06915147 0.1438298  0.15293412 0.17959016 0.16540295 0.07153788 0.21755361]Stap 4: Chi-kwadraat aanpassingstest uitvoerenCel: Code voor vraag 4 (Berekening & Test)Python# Bereken de absolute verwachte waarden (expected) binnen onze steekproefgrootte n
expected = expected_percentages * n

# Voer de chi-squared goodness-of-fit test uit
chi2_stat, p_val = stats.chisquare(f_obs=observed, f_exp=expected)

# Bereken de kritieke waarde g bij een significantieniveau van alpha = 0.05
alpha = 0.05
df = len(observed) - 1
g = stats.chi2.ppf(1 - alpha, df)

print(f"χ² (Chi-kwadraat)  : ≈ {chi2_stat:.3f}")
print(f"Degrees of freedom : {df}")
print(f"g (Kritieke waarde): ≈ {g:.3f}")
print(f"p-waarde           : ≈ {p_val:.3f}")
Cel: Code voor vraag 4 (Conclusie)Python# Evaluatie van de hypothese
if p_val < alpha:
    print("Conclusie: We verwerpen de nulhypothese (H0). De steekproef is NIET representatief.")
else:
    print("Conclusie: We kunnen de nulhypothese (H0) NIET verwerpen. De steekproef is WEL representatief voor de Vlaamse bevolking.")
 Eindconclusie / OplossingUit de statistische berekeningen komen de volgende resultaten naar voren:$$\chi^2 \approx 6.700$$$$df = 6$$$$g \approx 12.592$$$$p \approx 0.350$$Omdat de behaalde teststatistiek ($\chi^2 = 6.700$) kleiner is dan de kritieke waarde ($g = 12.592$) — of omdat de $p$-waarde ($0.350$) groter is dan het significantieniveau ($\alpha = 0.05$) — verwerpen we de nulhypothese niet.Conclusie: Er is geen statistisch significant verschil tussen de leeftijdsverdeling in de steekproef en die van de Vlaamse bevolking. De steekproef van de Digimeter 2016 is dus wel degelijk representatief voor de Vlaamse bevolking op het gebied van leeftijdscategorieën.

### Lab 4.08

 Hier vind je de opgave, de bijbehorende Python-code en de statistische uitwerking voor de oefening Off days.net als bij de vorige oefening gebruiken we hier een Chi-kwadraat aanpassingstest (Goodness-of-Fit test). Als je niemand trekt of bevoordeelt, zou iedereen in theorie immers even vaak vrij moeten krijgen op zaterdag (een uniforme verdeling).📋 Opgave: Exercise 8 - Off daysJe runt een klein bedrijf met vier werknemers: Susan, Jimmy, Albert en Camilla. Omdat je altijd drie werknemers op de werkvloer nodig hebt, kan er telkens maar één werknemer per week vrij krijgen op zaterdag. Iedereen wil uiteraard altijd die zaterdag vrij.Een van de werknemers beschuldigt je ervan dat je bepaalde collega's voortrekt. Om dit te onderzoeken, heb je een lijst getrokken van wie de afgelopen weken op zaterdag vrij had (data/off-days.csv).Doel: Visualiseer de data en voer een geschikte statistische test uit om te bepalen of er sprake is van vriendjespolitiek (voortrekken), of dat de vrije dagen eerlijk verdeeld zijn.🛠️ Code & Oplossingen per celStap 1: Data inlezen en verkennenEerst laden we de dataset in en kijken we hoe vaak elke werknemer daadwerkelijk een zaterdag heeft vrijgekregen.Cel: Code voor het inlezen en tellenPython# 1. Inlezen van de dataset
df = pd.read_csv('data/off-days.csv')

# Bekijk de eerste rijen en de unieke namen om te zien hoe de kolom het (waarschijnlijk 'Employee' of 'Name')
print(df.head())

# 2. Bereken de waargenomen frequenties (observed) per werknemer
# Pas de kolomnaam aan naar de exacte naam in je CSV (bijv. df['Employee'])
observed_counts = df['Employee'].value_counts()
print("\nWaargenomen vrije zaterdagen per werknemer:")
print(observed_counts)

# Totaal aantal geobserveerde weken (n)
n = len(df)
print(f"\nTotaal aantal geanalyseerde weken: {n}")
Stap 2: Visualisatie (Staafdiagram)Een snelle manier om te zien of de verdeling scheef is, is door een bar plot te maken.Cel: Code voor visualisatiePython# Visualiseer de data met Seaborn of Matplotlib
plt.figure(figsize=(8, 5))
sns.countplot(data=df, x='Employee', order=observed_counts.index, palette='viridis')
plt.title('Aantal vrije zaterdagen per werknemer')
plt.xlabel('Werknemer')
plt.ylabel('Aantal keren vrij')
plt.show()
Stap 3: Hypothesetest (Chi-kwadraat aanpassingstest)Als er geen sprake is van voortrekken (de nulhypothese $H_0$), heeft elke werknemer een even grote kans ($p = 0.25$) om vrij te krijgen. De verwachte frequentie voor iedereen is dus:$$\text{Expected} = \frac{\text{Totaal aantal weken (n)}}{4}$$Cel: Code voor de Chi-kwadraat testPython# Aantal categorieën (werknemers)
k = len(observed_counts)

# Omdat we een uniforme verdeling verwachten, delen we het totaal aantal weken door 4
expected_counts = [n / k] * k

# Voer de chi-squared goodness-of-fit test uit
chi2_stat, p_val = stats.chisquare(f_obs=observed_counts, f_exp=expected_counts)

# Bereken de kritieke waarde g bij alpha = 0.05
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
    print("De verschillen zijn statistisch niet significant. De vrije dagen zijn eerlijk (at random) verdeeld.")
📝 Formele Hypotheseproces & ConclusieWanneer je dit notebook straks runt, kun je de conclusie als volgt structureren in Markdown:Hypothesen:$H_0$: De vrije zaterdagen zijn uniform verdeeld over de vier werknemers (er is geen favoritisme).$H_1$: De vrije zaterdagen zijn niet uniform verdeeld (bepaalde werknemers worden voorgetrokken of benadeeld).Significantieniveau: $\alpha = 0.05$Resultaat: (Kijk naar de exacte output van je code voor de exacte getallen van $\chi^2$ en de $p$-waarde).Statistische beslissing: * Als $p < 0.05$: Verwerp $H_0$. De werknemer heeft gelijk; de verdeling is niet eerlijk.Als $p \ge 0.05$: Verwerp $H_0$ niet. De fluctuaties zijn puur op basis van toeval; er is geen hard bewijs voor vriendjespolitiek.
### Lab 4.09
Hier vind je de opgave, de bijbehorende Python-code en de statistische uitwerking voor de oefening Golf balls.Ook bij deze oefening gebruiken we een Chi-kwadraat aanpassingstest (Goodness-of-Fit test). In dit geval toetsen we of de verdeling van kopers die wél of níét een Callaway golfbal zouden kopen, overeenkomt met de verwachte verdeling op basis van de winstgrens van 20%.📋 Opgave: Exercise Golf ballsCallaway overweegt om de golfbalmarkt te betreden. Het bedrijf zal winst maken als het marktaandeel meer dan 20% is. Een marktonderzoek (steekproef) toont aan dat 140 van de 624 kopers van golfballen een Callaway golfbal zouden kopen.Vraag: Is er voldoende bewijs om Callaway te overtuigen de golfbalmarkt te betreden?Verwachte uitkomsten:$$\chi^2 = 2.3141$$$$p\text{-waarde } = 0.1282$$🛠️ Code & Oplossingen per celPlaats de onderstaande codeblokken in de opeenvolgende lege cellen van je Jupyter Notebook.Cel 1: Waargenomen en verwachte waarden definiërenPython# 1. Gegevens uit de opgave definiëren
n_total = 624
observed_callaway = 140
observed_other = n_total - observed_callaway

# Waargenomen frequenties (Observed)
observed_counts = np.array([observed_callaway, observed_other])

# 2. Verwachte percentages op basis van de winstgrens (H0: marktaandeel is 20%)
p_callaway = 0.20
p_other = 1.00 - p_callaway

# Verwachte absolute frequenties (Expected)
expected_counts = np.array([n_total * p_callaway, n_total * p_other])

print("Waargenomen aantallen [Callaway, Overig]:", observed_counts)
print("Verwachte aantallen   [Callaway, Overig]:", expected_counts)
Cel 2: Chi-kwadraat test uitvoerenPython# Voer de chi-squared goodness-of-fit test uit
chi2_stat, p_val = stats.chisquare(f_obs=observed_counts, f_exp=expected_counts)

# Bereken de kritieke waarde g bij alpha = 0.05
# Aantal categorieën = 2 (Callaway vs Overig), dus df = 2 - 1 = 1
alpha = 0.05
df_degrees = len(observed_counts) - 1
g = stats.chi2.ppf(1 - alpha, df_degrees)

print(f"χ² (Chi-kwadraat teststatistiek): ≈ {chi2_stat:.4f}")
print(f"Degrees of freedom (vrijheidsgraden): {df_degrees}")
print(f"g (Kritieke waarde)            : ≈ {g:.4f}")
print(f"p-waarde                       : ≈ {p_val:.4f}")
Cel 3: Evaluatie en ConclusiePythonprint("--- Statistische Evaluatie ---")
if p_val < alpha:
    print(f"p-waarde ({p_val:.4f}) < alpha ({alpha}): We verwerpen de nulhypothese (H0).")
    print("Het marktaandeel wijkt significant af van de 20%.")
else:
    print(f"p-waarde ({p_val:.4f}) >= alpha ({alpha}): We kunnen de nulhypothese (H0) niet verwerpen.")
    print("Het verschil met de verwachte 20% is statistisch niet significant.")
📝 Formele Hypotheseproces & ConclusieAls tekst of toelichting onderaan je notebook kun je de conclusie als volgt formuleren:1. Hypothesen$$H_0$$: Het marktaandeel van Callaway is gelijk aan 20% ($$\pi = 0.20$$).$$H_1$$: Het marktaandeel van Callaway wijkt af van de 20% ($$\pi \neq 0.20$$).2. Resultaten$$\chi^2 \approx 2.3141$$$$df = 1$$$$p\text{-waarde} \approx 0.1282$$3. ConclusieOmdat de $p$-waarde ($0.1282$) groter is dan het standaard significantieniveau ($\alpha = 0.05$), kunnen we de nulhypothese niet verwerpen. De geobserveerde 140 kopers (wat neerkomt op ongeveer 22.4%) wijken weliswaar licht af van de vereiste 20%, maar deze afwijking is statistisch gezien te klein om met zekerheid te zeggen dat het niet op toeval berust.Er is op dit moment niet genoeg statistisch bewijs om Callaway met zekerheid te overtuigen de golfbalmarkt te betreden.


### Example exam questions 4.10
Hier vind je de opgaven, de bijbehorende Python-code en de statistische uitwerkingen voor alle oefeningen uit het voorbeeldoamen van Hoofdstuk 4.📋 Exercise 1: Weather and CrimeCriminologen debatteren al lang over de vraag of er een verband bestaat tussen weersomstandigheden en het aantal gepleegde misdrijven. In 1988 werden 1361 moorden geclassificeerd per seizoen. Is er een associatie tussen het seizoen en het aantal gepleegde moorden?Omdat we hier één kwalitatieve variabele (seizoen) testen tegenover een verwachte gelijke (uniforme) verdeling, gebruiken we de Chi-kwadraat aanpassingstest (Goodness-of-Fit test).🛠️ CodePython# Waargenomen moorden per seizoen
observed_murders = dfmurders['number_of_murders'].values
total_murders = observed_murders.sum()

# Onder de nulhypothese verwachten we dat moorden gelijk verdeeld zijn over de 4 seizoenen
expected_murders = [total_murders / 4] * 4

# Chi-kwadraat aanpassingstest
chi2_stat, p_val = stats.chisquare(f_obs=observed_murders, f_exp=expected_murders)

alpha = 0.05
df = len(observed_murders) - 1
g = stats.chi2.ppf(1 - alpha, df)

print(f"χ² (Chi-kwadraat)  : ≈ {chi2_stat:.4f}")
print(f"Degrees of freedom : {df}")
print(f"g (Kritieke waarde): ≈ {g:.4f}")
print(f"p-waarde           : ≈ {p_val:.4f}")
📝 Uitwerking & ConclusieHypothesen:$H_0$: Er is geen associatie tussen het seizoen en het aantal moorden (de moorden zijn gelijk verdeeld).$H_1$: Er is wel een associatie tussen het seizoen en het aantal moorden.Resultaat: $\chi^2 \approx 4.0132$, $df = 3$, $p \approx 0.2599$, $g \approx 7.8147$.Conclusie: Omdat $p \approx 0.2599 > 0.05$ (en $\chi^2 < g$), kunnen we de nulhypothese niet verwerpen. Er is onvoldoende bewijs dat het seizoen invloed heeft op het aantal moorden.📋 Exercise 2: Education and SpiritualityEr is een steekproef genomen waarbij individuen zijn ingedeeld op basis van hun opleidingsniveau/discipline en hun mate van spiritualiteit. Is er een associatie tussen het opleidingsniveau en het gevoel van spiritualiteit?Omdat we hier te maken hebben met een kruistabel van twee kwalitatieve variabelen, gebruiken we de Chi-kwadraat onafhankelijkheidstest (Test for Independence).🛠️ CodePython# Maak een kruistabel door de tekstkolom als index te zetten
contingency_table = dfspirituality.set_index('sense_of_spirituality')

# Chi-kwadraat onafhankelijkheidstest
chi2_stat, p_val, df, expected = stats.chi2_contingency(contingency_table)

alpha = 0.05
g = stats.chi2.ppf(1 - alpha, df)

print(f"χ² (Chi-kwadraat)  : ≈ {chi2_stat:.4f}")
print(f"Degrees of freedom : {df}")
print(f"g (Kritieke waarde): ≈ {g:.4f}")
print(f"p-waarde           : ≈ {p_val:.4e}")
📝 Uitwerking & ConclusieHypothesen:$H_0$: Opleidingsniveau en spiritualiteit zijn onafhankelijk (er is geen associatie).$H_1$: Opleidingsniveau en spiritualiteit zijn afhankelijk (er is wel een associatie).Resultaat: $\chi^2 \approx 123.7997$, $df = 6$, $p \approx 3.3283 \times 10^{-24}$, $g \approx 12.5916$.Conclusie: De $p$-waarde is extreem klein ($p < 0.05$) en $\chi^2$ is vele malen groter dan de kritieke waarde $g$. We verwerpen de nulhypothese. Er is een zeer sterke, statistisch significante associatie tussen iemands achtergrond/opleiding en diens mate van spiritualiteit.📋 Exercise 4: Drug Experiment and Patient StateIn een experiment kregen drie groepen patiënten een placebo, een halve dosis, of een volledige dosis van een nieuw medicijn. Er is geregistreerd hoe zij zich achteraf voelden (beter, gelijk, slechter). Is er op een 5% significantieniveau bewijs dat er een verschil is in de conditie tussen de drie groepen patiënten?Ook dit is een vergelijking tussen meerdere categorische groepen, dus we gebruiken de Chi-kwadraat onafhankelijkheidstest / homogeniteitstest.🛠️ CodePython# dfexperiment heeft 'state' al als index, dus we kunnen de tabel direct invoeren
chi2_stat, p_val, df, expected = stats.chi2_contingency(dfexperiment)

alpha = 0.05
g = stats.chi2.ppf(1 - alpha, df)

print(f"χ² (Chi-kwadraat)  : ≈ {chi2_stat:.4f}")
print(f"Degrees of freedom : {df}")
print(f"g (Kritieke waarde): ≈ {g:.4f}")
print(f"p-waarde           : ≈ {p_val:.4f}")
📝 Uitwerking & ConclusieHypothesen:$H_0$: Er is geen verschil in de conditie tussen de drie behandelingsgroepen (medicijndosis en effect zijn onafhankelijk).$H_1$: Er is wel een verschil in de conditie tussen de drie behandelingsgroepen.Resultaat: $\chi^2 \approx 1.5187$, $df = 4$, $p \approx 0.8234$, $g \approx 9.4877$.Conclusie: Omdat de $p$-waarde ($0.8234$) aanzienlijk groter is dan $\alpha = 0.05$, kunnen we de nulhypothese niet verwerpen. Er is op basis van dit experiment geen statistisch bewijs dat de verandering in de toestand van de patiënt verschilt tussen de placebo-, halve dosis- of volledige dosisgroep.

