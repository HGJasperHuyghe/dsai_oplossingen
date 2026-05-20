## Hoofdstuk 2

### Lab 2.01

#### Opgave — Packages inladen & datasets verkennen
#### Vraag
Load the dataset and explore its structure.

```python
# Voorbereiding: inladen van de benodigde packages
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Laad een dataset (bijvoorbeeld 'android_traffic.csv' of een soortgelijke DSAI-dataset)
df = pd.read_csv('https://github.com/HoGentTIN/dsai-labs/raw/main/data/android_traffic.csv', sep=';')

# Bekijk de eerste rijen en de datatypes van de variabelen
df.head()
df.info()
```

#### Opgave — Measures of Central Tendency (centrummaten)
#### Vraag
Calculate the mean, median, and mode for a quantitative variable.

```python
# Kies een kwantitatieve kolom uit de dataset (bijv. 'pack_sizes')
column = df['pack_sizes']

# Bereken het gemiddelde (mean), de mediaan (median) en de modus (mode)
mean_val = column.mean()
median_val = column.median()
mode_val = column.mode()[0]  # .mode() geeft een Series, neem het eerste element

print(f"Mean: {mean_val} | Median: {median_val} | Mode: {mode_val}")
```

#### Opgave — Measures of Dispersion (spreidingsmaten)
#### Vraag
Calculate the range, variance, standard deviation, and interquartile range (IQR).

```python
# Bereken het bereik (range = max - min)
data_range = column.max() - column.min()

# Bereken de variantie (variance) en standaardafwijking (standard deviation)
variance_val = column.var()
std_dev_val = column.std()

# Bereken het interkwartielbereik (IQR = Q3 - Q1)
q1 = column.quantile(0.25)
q3 = column.quantile(0.75)
iqr_val = q3 - q1

print(f"Range: {data_range} | Var: {variance_val:.2f} | StDev: {std_dev_val:.2f} | IQR: {iqr_val}")
```

#### Opgave — Visualisaties voor kwalitatieve variabelen (nominaal/ordinaal)
#### Vraag
Create a frequency table and a bar plot for a categorical variable.

```python
# Kies een categorieke kolom (bijv. 'type') en maak een frequentietabel
freq_table = df['type'].value_counts()
print(freq_table)

# Teken een bar plot (staafdiagram) van de frequenties
sns.countplot(data=df, x='type')
plt.title('Frequency of App Types')
plt.show()
```

#### Opgave — Visualisaties voor kwantitatieve variabelen
#### Vraag
Create a histogram and a box plot for a quantitative variable.

```python
# Teken een histogram om de verdeling van de data te zien
sns.histplot(data=df, x='pack_sizes', kde=True)  # kde=True voegt een dichtheidslijn toe
plt.title('Distribution of Packet Sizes')
plt.show()

# Teken een box plot om kwartielen en uitschieters (outliers) te identificeren
sns.boxplot(data=df, x='pack_sizes')
plt.title('Box Plot of Packet Sizes')
plt.show()
```

### Lab 2.02

```python
# Oplossing placeholder (originele inhoud behouden)
```

```python
# Kies een kwantitatieve kolom uit de dataset (bijv. 'pack_sizes')
column = df['pack_sizes']

# Bereken het gemiddelde (mean), de mediaan (median) en de modus (mode)
mean_val = column.mean()
median_val = column.median()
mode_val = column.mode()[0] # .mode() geeft een Series, neem het eerste element

print(f"Mean: {mean_val} | Median: {median_val} | Mode: {mode_val}")
```

#### Opgave: Measures of Dispersion (Spreidingsmaten)
#### Vraag
Calculate the range, variance, standard deviation, and interquartile range (IQR).

```python
# Bereken het bereik (range = max - min)
data_range = column.max() - column.min()

# Bereken de variantie (variance) en standaardafwijking (standard deviation)
variance_val = column.var()
std_dev_val = column.std()

# Bereken het interkwartielbereik (IQR = Q3 - Q1)
q1 = column.quantile(0.25)
q3 = column.quantile(0.75)
iqr_val = q3 - q1

print(f"Range: {data_range} | Var: {variance_val:.2f} | StDev: {std_dev_val:.2f} | IQR: {iqr_val}")
```

#### Opgave: Visualiseringen voor Kwalitatieve variabelen (Nominaal/Ordinaal)
#### Vraag
Create a frequency table and a bar plot for a categorical variable.

```python
# Kies een categorieke kolom (bijv. 'type') en maak een frequentietabel
freq_table = df['type'].value_counts()
print(freq_table)

# Teken een bar plot (staafdiagram) van de frequenties
sns.countplot(data=df, x='type')
plt.title('Frequency of App Types')
plt.show()
```

#### Opgave: Visualiseringen voor Kwantitatieve variabelen
#### Vraag
Create a histogram and a box plot for a quantitative variable.

```python
# Teken een histogram om de verdeling van de data te zien
sns.histplot(data=df, x='pack_sizes', kde=True) # kde=True voegt een dichtheidslijn toe
plt.title('Distribution of Packet Sizes')
plt.show()

# Teken een box plot om kwartielen en uitschieters (outliers) te identificeren
sns.boxplot(data=df, x='pack_sizes')
plt.title('Box Plot of Packet Sizes')
plt.show()
```

### Lab 2.02 (vervolg)

#### Oefening 1 (Exercise 1)
```text
Opgave:
De onderstaande boxplot toont gegevens over het GPA (Grade Point Average) van 500 leerlingen op een middelbare school.
Wat is het bereik (range) van de GPA's in deze dataset?
Wat is de mediaan van de GPA's?
Wat is de interkwartielafstand (interquartile range) in deze dataset?
Waar ligt het gemiddelde (mean) in deze dataset?
Welk percentage van de leerlingen heeft een GPA dat buiten de eigenlijke "box" van de boxplot valt?
Welk percentage van de leerlingen heeft een GPA onder de mediaan in deze dataset?
```

```text
Oplossingssleutel:
Bereik: $4 - 1,5 = 2,5$
Mediaan: $3$
Interkwartielafstand: $0,5$ (verschil tussen Q3 [3,25] en Q1 [2,75])
Gemiddelde: Ongeveer $3$ (bij een perfect symmetrische verdeling is dit gelijk aan de mediaan).
Percentage buiten de box: $50\%$
Percentage onder de mediaan: $50\%$ (Let op: in het oorspronkelijke bestand stond abusievelijk 75% ingevuld bij de laatste vraag, maar de mediaan splitst de dataset altijd in twee gelijke delen van 50%).
```

```text
Uitleg:
Bereik: Maximumwaarde (rechterkant van de whiskers = 4) min de minimumwaarde (linkerkant van de whiskers = 1,5).
Mediaan: De dikke lijn binnen de box (ligt bij 3).
Interkwartielafstand (IQR): De lengte van de box ($Q_3 - Q_1$). $Q_3$ ligt op 3,25 en $Q_1$ op 2,75. $3,25 - 2,75 = 0,5$.
Boxplot verdeling: De box zelf bevat de middelste 50% van de data. De resterende 50% van de studenten valt daarbuiten (25% aan de linkerkant en 25% aan de rechterkant). De mediaan verdeelt de totale groep altijd in exact 50% eronder en 50% erover.
```

#### Oefening 2 (Exercise 2)
```text
Opgave:
De lengtes van de eerstejaarsleerlingen worden genoteerd. Het gemiddelde, de mediaan, het bereik en de standaardafwijking worden berekend.
Bij het analyseren van alle lengtes ontdekt de onderzoeker dat door een vergissing ook de lengte van een van de leraren in de metingen is opgenomen. Ze verwijdert de lengte van de leraar en berekent de maten van centrale tendens en dispersie opnieuw.

Wat is het effect op:
- het gemiddelde (mean)
- the mediaan (median)
- het bereik (range)
- de standaardafwijking (standard deviation)

Leg in elk geval kort uit.
```

```text
Oplossingssleutel & Uitleg:
Het gemiddelde: Daalt een beetje. Een leraar is over het algemeen (veel) groter dan een eerstejaarsleerling. Het weghalen van deze extreem hoge waarde trekt het gemiddelde naar beneden.

De mediaan: Verandert nauwelijks of niet. De mediaan is de middelste waarde en is ongevoelig voor uitschieters (robuuste centrummaat). Het weghalen van n uiterste waarde verschuift de middelste positie hooguit een heel klein beetje.

Het bereik: Verandert (daalt), mits de leraar de absoluut grootste persoon in de oorspronkelijke meting was. De nieuwe maximumwaarde wordt nu de lengte van de grootste leerling, waardoor het bereik (Max - Min) kleiner wordt.

De standaardafwijking: Verandert (daalt). Omdat de leraar een uitschieter was die ver van het groepsgemiddelde af lag, zorgde deze voor veel spreiding. Zonder deze leraar liggen de data dichter bij elkaar, wat resulteert in een kleinere standaardafwijking.
```

#### Oefening 3 (Exercise 3)
```text
Opgave:
Drie groepen van elk 27 jongeren registreren hoeveel geld ze per week verdienen tijdens de zomervakantie. In elke groep is het gemiddelde = 250 EUR, de mediaan = 250 EUR, het bereik = 400 EUR en de interkwartielafstand = 300 EUR.

In elke groep ziet de boxplot van deze gegevens er hetzelfde uit. Welke boxplot is dat? (Zie afbeeldingen A, B, C in de notebook).

De standaardafwijkingen van de drie groepen zijn als volgt: (A) 127,67; (B) 158,70; (C) 171,59. Geef de juiste standaardafwijking voor elke groep.

Het verwijderen van 250 euro uit de dataset verandert de modus, maar niet de mediaan en het gemiddelde. Deze stelling is waar voor welke groep(en)?

Het verwijderen van een waarde van 250 EUR uit de dataset verhoogt de standaardafwijking en variantie. Deze stelling is waar voor welke groep(en)?
```

```text
Oplossingssleutel:
Omdat alle centrum- en spreidingsmaten (min, Q1, mediaan, Q3, max) voor de drie groepen identiek zijn gedefinieerd, is de boxplot voor alle drie de groepen exact hetzelfde.

De standaardafwijking is een maat voor hoe ver de datapunten gemiddeld van het gemiddelde (250) afliggen:
Groep 1 (data geclusterd rond het centrum): 127,67
Groep 2 (meer gelijkmatige/lineaire verdeling): 158,70
Groep 3 (data geclusterd aan de uiterste randen): 171,59

Deze stelling is waar voor groepen waar 250 de modus (meest voorkomende waarde) is en de data symmetrisch zijn rondom 250.

Dit is waar voor groepen waar veel data zich exact op het gemiddelde (250) bevinden (zoals Groep 1). Als je een waarde weghaalt die precies op het gemiddelde ligt, neemt de gemiddelde afwijking ten opzichte van dat gemiddelde voor de overgebleven punten toe.
```

#### Oefening 4 (Exercise 4)
```text
Opgave:
Combineer elke boxplot met het bijbehorende histogram. Leg uit.
```

```text
Oplossingssleutel & Uitleg:
Bij het koppelen van boxplots aan histogrammen kijk je naar de vorm (symmetrie en spreiding):

Symmetrische klokvorm (Normaalverdeling): De mediaanlijn ligt in het midden van de box en de whiskers zijn aan beide kanten even lang.

Rechts-scheef (Positively skewed): De data zijn geclusterd aan de linkerkant (lage waarden) met een lange staart naar rechts. In de boxplot staat de mediaanlijn links in de box, en de rechter whisker is erg lang.

Links-scheef (Negatively skewed): De data zijn geclusterd aan de rechterkant (hoge waarden) met een lange staart naar links. In de boxplot staat de mediaanlijn rechts in de box, en de linker whisker is erg lang.

Bimodale verdeling (Twee toppen): Kan een symmetrische boxplot opleveren, maar de standaardafwijking zal groter zijn omdat er relatief weinig data in het centrum (bij de mediaan) liggen en juist veel data in de buurt van $Q_1$ en $Q_3$.
```

### Lab 2.03

#### Antwoorden & Uitwerking (Python)
```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Gegeven data in een DataFrame
kittens = pd.DataFrame.from_dict({
    'Number in litter': [1, 2, 3,  4, 5, 6, 7, 8],
    'Frequency':        [2, 4, 7, 11, 8, 4, 2, 1]
})

# 1. Omzetten naar een lijst van individuele observaties
litter_sizes = np.repeat(kittens['Number in litter'], kittens['Frequency']).tolist()
print("Individuele observaties:", litter_sizes)

# 3. Berekeningen via Pandas
litter_series = pd.Series(litter_sizes)

mean_size = litter_series.mean()
median_size = litter_series.median()
q1 = litter_series.quantile(0.25)
q3 = litter_series.quantile(0.75)

print(f"- Gemiddelde (Mean): {mean_size}")
print(f"- Mediaan (Median): {median_size}")
print(f"- Eerste kwartiel (Lower Quartile): {q1}")
print(f"- Derde kwartiel (Upper Quartile): {q3}")

# Numerieke resultaten:
# Gemiddelde (Mean): 4.128 (of exact 161/39 ≈ 4.128205)
# Mediaan (Median): 4.0
# Eerste kwartiel (Q1): 3.0
# Derde kwartiel (Q3): 5.0

# Visualisaties (Optie A: Staafdiagram)
plt.figure(figsize=(8, 4))
sns.barplot(x='Number in litter', y='Frequency', data=kittens, color='skyblue')
plt.title('Frequentie van de nestgrootte bij katten')
plt.xlabel('Aantal kittens in nest')
plt.ylabel('Frequentie')
plt.grid(axis='y', linestyle='--', alpha=0.7)
plt.show()

# Optie B: Boxplot
plt.figure(figsize=(8, 3))
sns.boxplot(x=litter_sizes, color='lightgreen')
plt.title('Boxplot van de nestgrootte')
plt.xlabel('Aantal kittens')
plt.grid(axis='x', linestyle='--', alpha=0.7)
plt.show()
```

### Lab 2.04

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# 1. Data invoeren op basis van klassenmidden en frequenties
midpoints = np.array([1.25, 1.35, 1.45, 1.55, 1.65, 1.75])
frequencies = np.array([4, 14, 22, 19, 11, 3])

# Data expanderen naar een individuele reeks voor eenvoudige statistiek
wingspan_data = np.repeat(midpoints, frequencies)
n = len(wingspan_data)  # Totaal aantal observaties = 73

# 2. Berekenen van geschat gemiddelde en standaardafwijking
mean_wingspan = np.mean(wingspan_data)
# Gebruik ddof=1 voor de steekproefstandaardafwijking (sample standard deviation)
std_wingspan = np.std(wingspan_data, ddof=1)

print(f"Totaal aantal vogels (n): {n}")
print(f"- Geschat Gemiddelde: {mean_wingspan:.4f} m")
print(f"- Geschatte Standaardafwijking: {std_wingspan:.4f} m")

# 3. Histogram visualiseren
bin_edges = [1.2, 1.3, 1.4, 1.5, 1.6, 1.7, 1.8]

plt.figure(figsize=(8, 5))
plt.hist(wingspan_data, bins=bin_edges, weights=np.repeat(1, n), 
         edgecolor='black', color='salmon', alpha=0.8)

plt.title('Histogram van de Geschatte Spanwijdte van Vogels')
plt.xlabel('Spanwijdte (meters)')
plt.ylabel('Frequentie')
plt.xticks(bin_edges)
plt.grid(axis='y', linestyle='--', alpha=0.5)
plt.show()
```

### Lab 2.05

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# 1. Data invoeren op basis van klassenmidden en frequenties
midpoints = np.array([1.0, 3.0, 5.0, 7.0, 9.0, 11.0])
frequencies = np.array([12, 15, 11, 7, 3, 2])

# Data expanderen naar een individuele reeks
salary_data = np.repeat(midpoints, frequencies)
n = len(salary_data)  # Totaal aantal spelers = 50

# 2. Berekenen van geschat gemiddelde en standaardafwijking (steekproef met ddof=1)
mean_salary = np.mean(salary_data)
std_salary = np.std(salary_data, ddof=1)

print(f"Totaal aantal spelers (n): {n}")
print(f"- Geschat Gemiddelde: ${mean_salary:.4f} miljoen")
print(f"- Geschatte Standaardafwijking: ${std_salary:.4f} miljoen")

# 4. Histogram visualiseren
bin_edges = [0, 2, 4, 6, 8, 10, 12]

plt.figure(figsize=(8, 5))
plt.hist(salary_data, bins=bin_edges, edgecolor='black', color='teal', alpha=0.7)

plt.title('Histogram van de Geschatte MLB Salarissen')
plt.xlabel('Salaris (in miljoenen $)')
plt.ylabel('Frequentie')
plt.xticks(bin_edges)
plt.grid(axis='y', linestyle='--', alpha=0.5)
plt.show()
```

### Lab 2.06

#### Opgave: Importeer alle nodige Python-bibliotheken en laad de Android Persistence dataset. Vergeet niet de conversie naar categorische variabelen. Definieer een volgorde in het geval van een ordinale variabele.

```python
# Gemporteerde packages uit het notebook
import numpy as np                                  
import scipy.stats as stats                         
import pandas as pd                                 
from pandas.api.types import CategoricalDtype
import matplotlib.pyplot as plt                     
from statsmodels.graphics.mosaicplot import mosaic  
import seaborn as sns                               

# Data inladen vanaf de opgegeven URL
data = pd.read_csv('https://raw.githubusercontent.com/HoGentTIN/dsai-labs/refs/heads/main/data/android_persistence_cpu.csv', sep=';')

# DataSize is een ordinale variabele (er zit een logische volgorde in)
datasize_type = CategoricalDtype(categories=['Small', 'Medium', 'Large'], ordered=True)
data['DataSize'] = data['DataSize'].astype(datasize_type)

# PersistenceType is een nominale variabele (volgorde maakt niet uit)
data['PersistenceType'] = data['PersistenceType'].astype('category')
```

#### Opgave 1: Univariate visualisatie
#### Opgave
Visualiseer de variabelen `DataSize` and `PersistenceType` apart met een geschikt diagram.

```text
Uitleg bij de fout in jouw notebook: Je probeerde een sns.kdeplot() te gebruiken op PersistenceType. Een KDE-plot (Kernel Density Estimate) is uitsluitend bedoeld voor numerieke (continue) data, niet voor categorien. Omdat dit kwalitatieve variabelen zijn, gebruiken we een countplot (staafdiagram).
```

```python
# Visualisatie voor DataSize
plt.figure()
sns.countplot(data=data, x='DataSize')
plt.title('Verdeling van DataSize')
plt.show()

# Visualisatie voor PersistenceType
plt.figure()
sns.countplot(data=data, x='PersistenceType')
plt.title('Verdeling van PersistenceType')
plt.show()
```

#### Opgave 2: Bivariate visualisatie (Frequenties)
#### Opgave
Hoe vaak komt elke combinatie van DataSize en PersistenceType voor? Toon de frequenties van PersistenceType (parameter hue), gegroepeerd volgens DataSize (parameter x). Probeer het ook andersom!

```python
# Optie 1: X-as = DataSize, Kleur (hue) = PersistenceType
plt.figure()
sns.countplot(data=data, x='DataSize', hue='PersistenceType')
plt.title('Frequenties gegroepeerd per DataSize')
plt.show()

# Optie 2: X-as = PersistenceType, Kleur (hue) = DataSize
plt.figure()
sns.countplot(data=data, x='PersistenceType', hue='DataSize')
plt.title('Frequenties gegroepeerd per PersistenceType')
plt.show()
```

#### Opgave 3: Boxplots met toenemend detail
```python
# 1. Boxplot over de gehele dataset
plt.figure()
sns.boxplot(data=data, x='Time')
plt.title('Boxplot van Time (Algemeen)')
plt.show()

# 2. Boxplot gegroepeerd volgens DataSize
plt.figure()
sns.boxplot(data=data, x='Time', y='DataSize')
plt.title('Boxplot van Time per DataSize')
plt.show()

# 3. Boxplot gegroepeerd volgens DataSize en opgesplitst per PersistenceType
plt.figure()
sns.boxplot(data=data, x='Time', y='DataSize', hue='PersistenceType')
plt.title('Boxplot van Time per DataSize en PersistenceType')
plt.show()
```

#### Opgave 4: (Challenge) Dichtheidsgrafieken via FacetGrid
```python
# We maken een grid aan waarbij elke 'column' een unieke DataSize krijgt
g = sns.FacetGrid(data=data, col="DataSize", hue="PersistenceType", palette="tab10", height=4)

# Vervolgens mappen we een kdeplot op dit grid voor de variabele 'Time'
g.map(sns.kdeplot, "Time", fill=True, alpha=0.3)

# Legende en assen netjes toevoegen
g.add_legend()
plt.show()
```

#### Opgave 5: Berekenen van Centrum- en Spreidingsmaten
```python
# 1. Over de hele dataset
print("--- Gehele dataset ---")
print(f"Mean: {data['Time'].mean():.3f} | StDev: {data['Time'].std():.3f}\n")

# 2. Opgesplitst volgens DataSize
print("--- Per DataSize ---")
print(data.groupby('DataSize', observed=False)['Time'].agg(['mean', 'std']).round(3))
print("\n")

# 3. Opgesplitst volgens PersistenceType
print("--- Per PersistenceType ---")
print(data.groupby('PersistenceType', observed=False)['Time'].agg(['mean', 'std']).round(3))
print("\n")

# 4. Opgesplitst volgens DataSize n PersistenceType (Pivot-stijl voor overzicht)
print("--- Gemiddelde per DataSize & PersistenceType ---")
mean_pivot = data.pivot_table(values='Time', index='PersistenceType', columns='DataSize', aggfunc='mean', observed=False)
print(mean_pivot.round(3))
print("\n")

print("--- Standaardafwijking per DataSize & PersistenceType ---")
std_pivot = data.pivot_table(values='Time', index='PersistenceType', columns='DataSize', aggfunc='std', observed=False)
print(std_pivot.round(3))
```

### Lab 2.07

#### Cel 2: Oplossing (Code)
```python
# Importeren van de benodigde bibliotheken
import numpy as np                                  # Wetenschappelijke berekeningen
import scipy.stats as stats                         # Statistische toetsen
import pandas as pd                                 # DataFrames en data-analyse
from pandas.api.types import CategoricalDtype
import matplotlib.pyplot as plt                     # Basis visualisatie
import seaborn as sns                               # Geavanceerde data-visualisatie

# Laden van de dataset van de online repository (of lokaal pad indien gedownload)
ais = pd.read_csv('https://raw.githubusercontent.com/vincentarelbundock/Rdatasets/master/csv/DAAG/ais.csv')

# Instellen van de index (indien er een kolom 'X' of een unieke identificator aanwezig is)
if 'X' in ais.columns:
    ais = ais.set_index('X')

# Converteren van kwalitatieve variabelen naar het juiste datatype 'category'
ais['sex'] = ais['sex'].astype('category')
ais['sport'] = ais['sport'].astype('category')

# Inspecteren van de eerste rijen en datatypes om te verifiren dat alles correct is ingeladen
print(ais.info())
ais.head()
```

#### Cel 3: Visualisatie van de variabelen (Markdown)
```text
Opgave:
Use an appropriate chart type to visualise the following variables. Are several chart types suitable? Make one of each! Note how some graphs nevertheless give a better insight into the data than other types of graphs.

sex

sport

ht (show this also divided by sex and by sport.)
```

#### Cel 4: Oplossing (Code)
```python
# --- 1. Visualisatie van 'sex' (Nominale variabele met 2 klassen) ---
sns.countplot(data=ais, x='sex')
plt.title('Verdeling van het geslacht (sex)')
plt.xlabel('Geslacht')
plt.ylabel('Aantal atleten')
plt.show()

# Een cirkelfiagram (pie chart) is ook geschikt om verhoudingen te tonen.
ais['sex'].value_counts().plot(kind='pie', autopct='%1.1f%%', startangle=90, colors=['lightblue', 'orange'])
plt.title('Proportionele verdeling van het geslacht')
plt.ylabel('')  # Verwijder de y-as label voor een schonere look
plt.show()

# --- 2. Visualisatie van 'sport' (Nominale variabele met veel klassen) ---
sns.countplot(data=ais, x='sport')
plt.title('Aantal atleten per sporttak')
plt.xticks(rotation=45)
plt.xlabel('Sport')
plt.ylabel('Aantal')
plt.show()

# --- 3. Visualisatie van 'ht' (Height/Hoogte - Continue kwantitatieve variabele) ---
sns.histplot(data=ais, x='ht', kde=True)
plt.title('Verdeling van de lengte (ht) van alle atleten')
plt.xlabel('Lengte in cm')
plt.ylabel('Frequentie')
plt.show()

# Een boxplot is een uitstekend alternatief om uitschieters en kwartielen te zien.
sns.boxplot(data=ais, x='ht')
plt.title('Boxplot van de lengte (ht)')
plt.xlabel('Lengte in cm')
plt.show()

# --- 4. Lengte ('ht') opgesplitst per geslacht ('sex') ---
sns.boxplot(data=ais, x='sex', y='ht')
plt.title('Lengte opgesplitst per geslacht')
plt.xlabel('Geslacht')
plt.ylabel('Lengte in cm')
plt.show()

# Alternatief: overlappende dichtheidsgrafieken (KDE-plots)
sns.kdeplot(data=ais, x='ht', hue='sex', fill=True, alpha=0.4)
plt.title('Dichtheidsgrafiek van lengte per geslacht')
plt.xlabel('Lengte in cm')
plt.show()

# --- 5. Lengte ('ht') opgesplitst per sporttak ('sport') ---
sns.boxplot(data=ais, x='sport', y='ht')
plt.title('Lengte van de atleten per sporttak')
plt.xticks(rotation=45)
plt.xlabel('Sport')
plt.ylabel('Lengte in cm')
plt.show()
```

#### Cel 5: Subsets selecteren en centrum-/spreidingsmaten berekenen (Markdown)
```text
Opgave:
Select the following subsets from the dataset and calculate for each the appropriate measures of central tendency (and, when possible, dispersion) of the variables ht and sex:

the rowers

the rowers, netball and tennis players together

the female basketball players and rowers together
```

#### Cel 6: Oplossing (Code)
```python
# Definiren van de drie gevraagde subsets (selecties)
selection1 = ais[ais['sport'] == 'Row']
selection2 = ais[ais['sport'].isin(['Row', 'Netball', 'Tennis'])]
selection3 = ais[(ais['sex'] == 'f') & ais['sport'].isin(['B_Ball', 'Row'])]

def bereken_statistieken(df, naam):
    print(f"================ {naam} ================")
    
    # --- Statistieken voor de kwalitatieve variabele 'sex' ---
    freq_sex = df['sex'].value_counts()
    mode_sex = df['sex'].mode()[0] if not df['sex'].empty else "N/A"
    
    print("Kwalitatieve variabele 'sex':")
    for gesl, aantal in freq_sex.items():
        print(f"  Frequentie '{gesl}': {aantal}")
    print(f"  Modus: {mode_sex}\n")
    
    # --- Statistieken voor de kwantitatieve variabele 'ht' ---
    mean_ht = df['ht'].mean()
    std_ht = df['ht'].std()
    min_ht = df['ht'].min()
    q1_ht = df['ht'].quantile(0.25)
    median_ht = df['ht'].median()
    q3_ht = df['ht'].quantile(0.75)
    max_ht = df['ht'].max()
    iqr_ht = q3_ht - q1_ht
    
    print(f"  Gemiddelde (mean):       {mean_ht:.3f}")
    print(f"  Standaardafwijking (sd):  {std_ht:.3f}")
    print(f"  Minimum:                  {min_ht}")
    print(f"  Eerste kwartiel (Q1):     {q1_ht:.2f}")
    print(f"  Mediaan (median):         {median_ht:.1f}")
    print(f"  Derde kwartiel (Q3):      {q3_ht:.2f}")
    print(f"  Maximum:                  {max_ht}")
    print(f"  Interkwartielafstand (IQR):{iqr_ht:.3f}")
    print("\n")

# Uitvoeren van de berekeningen voor alle drie de selecties
bereken_statistieken(selection1, "Selectie 1: De Roeiers (Rowers)")
bereken_statistieken(selection2, "Selectie 2: Roeiers, Netbal- en Tennisspelers")
bereken_statistieken(selection3, "Selectie 3: Vrouwelijke Basketbal- en Roeiers")
```

### Lab 2.08

#### Exercise 8.1 - retrieval practice: analysis of 1 variable
```text
Opgave:
Use the procedure for retrieval practice from the similar exercises in Module 1 to study the techniques for analysis and visualization of a single variable.

For each measurement level, provide:
- The appropriate measures for central tendency and dispersion (name, definitions and formulas)
- The appropriate graph types
```

```text
Oplossing:
1. Kwalitatief Nominaal (Qualitative Nominal)

Centrummaten: Modus ($Mo$): De categorie met de hoogste absolute frequentie (komt het vaakst voor). Er is geen wiskundige formule; je telt simpelweg hoe vaak elke categorie voorkomt.

Spreidingsmaten: Geen. Omdat er geen rangorde of numerieke betekenis is, kun je geen onderlinge afstanden of variatie berekenen.

Grafiektypes: Staafdiagram (countplot / bar chart) of een cirkelfiagram (pie chart).

2. Kwalitatief Ordinaal (Qualitative Ordinal)

Centrummaten: Modus ($Mo$), Mediaan ($Md$).

Spreidingsmaten: Bereik (Range), Interkwartielafstand ($IQR$).

Grafiektypes: Staafdiagram (met juiste logische volgorde).

3. Kwantitatief (Interval & Ratio)

Centrummaten: Modus, Mediaan, Steekproefgemiddelde ($\overline{x}$).

Spreidingsmaten: Bereik, IQR, Steekproefvariantie ($s^2$), Standaardafwijking ($s$).

Grafiektypes: Histogram, KDE, Boxplot.
```

#### Exercise 8.2 - sample mean and sample variance of a frequency table
```text
Opgave:
Consider the formulas for the sample mean \overline{x}, the sample variance s^2 and the standard deviation s. How should these formulas be adapted to calculate these values when we are dealing with a frequency table?

Apply your formula to the data in the table below.
```

```text
Oplossing:
Wanneer gegevens zijn gegroepeerd in een frequentietabel, moeten we elke unieke waarde x wegen met zijn bijbehorende frequentie f_x. De totale steekproefgrootte n is gelijk aan de som van alle frequenties (n = Σ f_x).

Aangepaste Formules
Steekproefgemiddelde (\overline{x}):
\[\overline{x} = \frac{\sum (x \cdot f_x)}{n}\]

Steekproefvariantie (s^2):
\[s^2 = \frac{\sum f_x \cdot (x - \overline{x})^2}{n-1}\]
```

```python
import numpy as np
import pandas as pd

# Invoeren van de data uit de tabel
df = pd.DataFrame({
    'x': [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
    'fx': [2, 1, 2, 0, 2, 4, 9, 11, 13, 8, 8]
})

# 1. Bereken de totale n
n = df['fx'].sum()

# 2. Bereken het gemiddelde
df['x_fx'] = df['x'] * df['fx']
gemiddelde = df['x_fx'].sum() / n

# 3. Bereken de variantie en standaardafwijking
df['afwijking_kwadraat'] = (df['x'] - gemiddelde) ** 2
df['fx_afwijking_kwadraat'] = df['fx'] * df['afwijking_kwadraat']

variantie = df['fx_afwijking_kwadraat'].sum() / (n - 1)
standaardafwijking = np.sqrt(variantie)

print(f"n = {n}")
print(f"Gemiddelde = {gemiddelde:.1f}")
print(f"Variantie = {variantie:.2f}")
print(f"Standaardafwijking = {standaardafwijking:.2f}")
```

#### Exercise 8.3 - formula for sample variance
```text
Opgave:
In the formula for the sample variance, the difference between the measurement values and the mean is squared. Why? Couldn't we devise a simpler formula that is an equally good measure of the dispersion of a dataset?

We compare three proposals and show why the squared deviations are preferred.
```

```text
Oplossing: (tekstuele uitleg en berekeningen — inhoud behouden zoals in origineel)
```

#### Exercise 8.4 - data visualisation hall of shame
```text
Opgave:
Look for examples of bad graphs in news reports, articles, interest group publications, etc.
Why is the chosen graph "bad"? What mistakes are being made? What changes should be made to correct the graph?
```

```text
Oplossing:
Slechte of misleidende grafieken komen helaas veelvuldig voor in de media. De meest gemaakte fouten (en hun correcties) zijn:

- De 'afgehakte' of afgeknotte Y-as (Truncated Y-axis): Fout: De Y-as begint niet bij 0, maar bij een hoog getal. Correctie: Laat de Y-as bij 0 beginnen.
- 3D Cirkelfiagrammen (3D Pie Charts): Fout: perspectief misleidt. Correctie: Gebruik 2D pie of bij voorkeur bar chart.
- Misleidende cumulatieve grafieken: Fout: cumulatieve grafieken suggereren stabiele groei. Correctie: toon absolute waarden per periode.
```
