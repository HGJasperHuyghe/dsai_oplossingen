# Voorbeeldexamen (1°

## Voorbereiding: Datasets genereren

Voer eerst deze cel uit in je notebook om alle data in te laden.

```python
import pandas as pd
import numpy as np

np.random.seed(42)

# Dataset 1: Erasmus Wenen (Chi-kwadraat toets)
destinations = np.random.choice(['Praag', 'Boedapest', 'Bratislava'], size=150, p=[0.4, 0.4, 0.2])
transports = []
for d in destinations:
    if d == 'Bratislava':
        transports.append(np.random.choice(['Trein', 'Bus'], p=[0.8, 0.2]))
    else:
        transports.append(np.random.choice(['Trein', 'Bus'], p=[0.4, 0.6]))
df_erasmus = pd.DataFrame({'Bestemming': destinations, 'Transport': transports})

# Dataset 2: ETF Investeringen 2026 (T-toets)
semi_returns = np.random.normal(loc=1.2, scale=2.5, size=40)
uranium_returns = np.random.normal(loc=0.5, scale=3.0, size=40)
df_etf = pd.DataFrame({
    'Sector': ['Semiconductors']*40 + ['Uranium']*40,
    'Maandrendement': np.concatenate([semi_returns, uranium_returns])
})

# Dataset 3: SAP AI Engineering (Lineaire regressie)
backend_time = np.random.uniform(50, 300, 100)
frontend_time = 20 + 0.6 * backend_time + np.random.normal(0, 15, 100)
df_sap = pd.DataFrame({'Backend_Time_ms': backend_time, 'Frontend_Time_ms': frontend_time})

# Dataset 4: Bezoekersaantallen KLJ (Tijdreeksanalyse)
dates = pd.date_range(start='2023-01-01', periods=36, freq='ME')
trend = np.linspace(100, 300, 36)
seasonality = 50 * np.sin(np.linspace(0, 3 * 2 * np.pi, 36))
visitors = trend + seasonality + np.random.normal(0, 10, 36)
df_bezoekers = pd.DataFrame({'Datum': dates, 'Bezoekers': visitors.astype(int)})
df_bezoekers.set_index('Datum', inplace=True)
```

---

## Vraag 1: Normale Verdeling (Groq AI Bestelsysteem)

### Overzicht

Voor het komende kamp ontwikkelde je een vernieuwd AI-bestelsysteem. Uit de logbestanden blijkt dat de responstijd per bestelling normaal verdeeld is met een gemiddelde $\mu = 1.2$ seconden en een standaardafwijking $\sigma = 0.15$ seconden.

### Opgave

1. Bereken de kans dat een willekeurige bestelling er langer dan 1.4 seconden over doet om te verwerken.
2. Je wil een badge uitreiken voor de absolute recordsnelheden. Onder welke tijd (in seconden) vallen de 5% allersnelst verwerkte bestellingen?

---

## Vraag 2: Bivariate Analyse (Erasmus Wenen)

### Overzicht

Je analyseert het reisgedrag van medestudenten tijdens hun Erasmus in Wenen. Je wil onderzoeken of de keuze van hun transportmiddel (Trein of Bus) afhankelijk is van de gekozen weekendbestemming (Praag, Boedapest of Bratislava). Gebruik `df_erasmus`.

### Opgave

1. Maak een kruistabel (contingency table) en een gestapeld staafdiagram van de data.
2. Voer een geschikte onafhankelijkheidstoets uit (significantieniveau 5%). Formuleer je $H_0$ en $H_1$, en geef duidelijk je besluit of de bestemming en het transportmiddel elkaar beïnvloeden.

---

## Vraag 3: T-toetsen (Kwantitatief)

### Overzicht

Om je beleggingsstrategie te optimaliseren, vergelijk je de historische maandrendementen van twee accumulating sectoren: Semiconductor ETFs en Uranium ETFs. Je theorie is dat de semiconductor-sector een significant hoger gemiddeld rendement oplevert. Gebruik `df_etf`.

### Opgave

1. Voer de correcte hypothesetoets uit om de gemiddelden te vergelijken. (Kies tussen een gepaarde of onafhankelijke t-toets en bepaal de juiste `alternative` parameter).
2. Wat is de p-waarde en wat besluit je hieruit ($\alpha = 0.05$)?

---

## Vraag 4: Lineaire Regressie (SAP Systeemanalyse)

### Overzicht

Tijdens een performance review onderzoek je de laadtijden van een applicatie. Je bekijkt de relatie tussen de backend OData-responstijd (`Backend_Time_ms`) en de frontend Fiori-rendertijd (`Frontend_Time_ms`). Gebruik `df_sap`.

### Opgave

1. Maak een scatterplot met de regressielijn en print de vergelijking van het model ($\hat{y} = \beta_0 + \beta_1 x$).
2. Bereken de determinatiecoëfficiënt ($R^2$). Verklaart de backend-tijd het grootste deel van de variatie in de frontend-tijd? (Tip: $R^2 > 0.70$ betekent een sterke fit).
3. Voorspel de frontend-tijd wanneer de backend-tijd piekt op 350 ms.

---

## Vraag 5: Tijdreeksanalyse

### Overzicht

Je houdt de websitebezoekers van de KLJ-site al drie jaar lang maandelijks bij. De data (`df_bezoekers`) vertoont zowel een structurele groei als seizoensgebonden pieken door de jaarlijkse evenementen.

### Opgave

1. Maak een tijdreeksgrafiek van de historische bezoekersaantallen.
2. Pas Triple Exponential Smoothing (Holt-Winters) toe. Kies de juiste parameters voor trend en seizoenaliteit ($period = 12$).
3. Voorspel de aantallen voor de komende 4 maanden.

---

# Oplossingssleutel

Hieronder vind je de Python-code en tekstuele interpretaties, netjes geformatteerd zoals in de DSAI-cursus.

## Oplossing Vraag 1

```python
import scipy.stats as stats

mu = 1.2
sigma = 0.15

# 1. Kans op een responstijd > 1.4s (sf = survival function / rechtszijdige kans)
p_lang = stats.norm.sf(1.4, loc=mu, scale=sigma)

# 2. De snelste 5% (dus aan de extreem linkerkant van de verdeling)
grens_snelst = stats.norm.ppf(0.05, loc=mu, scale=sigma)

print(f"Kans op responstijd > 1.4s: {p_lang:.4f}")
print(f"De grens voor de 5% snelste bestellingen ligt op: {grens_snelst:.3f} seconden")
```

**Uitleg:**

1. $P(X > 1.4) \approx 0.0912$ (ofwel 9.12%). Er is iets meer dan 9% kans dat een AI-call langer duurt dan 1.4 seconden.
2. Door de inverse CDF (PPF) te gebruiken op 0.05, vinden we dat alles onder 0.953 seconden tot de absolute top 5% snelste verwerkingen behoort.

## Oplossing Vraag 2

```python
import pandas as pd
import matplotlib.pyplot as plt
import scipy.stats as stats

# 1. Kruistabel en Visualisatie
crosstab = pd.crosstab(df_erasmus['Bestemming'], df_erasmus['Transport'])
crosstab.plot(kind='bar', stacked=True, figsize=(8, 5), color=['orange', 'steelblue'])
plt.title('Transportkeuze per Bestemming')
plt.ylabel('Aantal Studenten')
plt.show()

# 2. Chi-kwadraat toets
chi2_stat, p_value, dof, expected_matrix = stats.chi2_contingency(crosstab)
print(f"Chi-kwadraat: {chi2_stat:.4f}")
print(f"p-waarde: {p_value:.4f}")
```

**Hypotheses:**

- $H_0$: Bestemming en Transportmiddel zijn onafhankelijk.
- $H_1$: Bestemming en Transportmiddel zijn afhankelijk van elkaar.

**Besluit:**

De berekende p-waarde (0.0033) is kleiner dan alpha (0.05). We verwerpen de nulhypothese. Er is een significant verband tussen de bestemming en het gekozen transportmiddel (zo is bijvoorbeeld de trein duidelijk oververtegenwoordigd bij Bratislava vergeleken met de andere steden).

## Oplossing Vraag 3

```python
import scipy.stats as stats

# 1. Data splitsen
semi = df_etf[df_etf['Sector'] == 'Semiconductors']['Maandrendement']
uranium = df_etf[df_etf['Sector'] == 'Uranium']['Maandrendement']

# 2. Onafhankelijke t-toets (Rechtszijdig: Semi > Uranium)
# We gebruiken equal_var=False (Welch's t-test) als best practice
t_stat, p_val = stats.ttest_ind(semi, uranium, equal_var=False, alternative='greater')

print(f"t-statistiek: {t_stat:.4f}")
print(f"p-waarde: {p_val:.4f}")
```

**Hypotheses:**

- $H_0$: $\mu_{\text{Semi}} \leq \mu_{\text{Uranium}}$
- $H_1$: $\mu_{\text{Semi}} > \mu_{\text{Uranium}}$

**Besluit:**

Omdat we de ETFs testen als twee onafhankelijke entiteiten (het zijn geen metingen van eenzelfde groep op twee verschillende tijdstippen), is een independent t-test correct. De p-waarde ligt rond 0.17. Dit is groter dan 0.05. We kunnen $H_0$ dus niet verwerpen; er is op basis van deze steekproef onvoldoende statistisch bewijs om te claimen dat Semiconductors significant beter presteren dan Uranium ETFs.

## Oplossing Vraag 4

```python
from sklearn.linear_model import LinearRegression
import numpy as np

X = df_sap[['Backend_Time_ms']].values
y = df_sap['Frontend_Time_ms'].values

# 1. Regressiemodel fitten
model = LinearRegression()
model.fit(X, y)
beta_0 = model.intercept_
beta_1 = model.coef_[0]
print(f"1. Vergelijking: ŷ = {beta_0:.4f} + {beta_1:.4f}x")

# Scatterplot met lijn
plt.scatter(X, y, color='blue', alpha=0.5, label='Meetpunten')
plt.plot(X, model.predict(X), color='red', linewidth=2, label='Regressielijn')
plt.title('Frontend Rendertijd in functie van Backend Responstijd')
plt.xlabel('Backend_Time_ms')
plt.ylabel('Frontend_Time_ms')
plt.legend()
plt.show()

# 2. R-Kwadraat
r_squared = model.score(X, y)
print(f"2. Determinatiecoëfficiënt (R²): {r_squared:.4f}")

# 3. Voorspelling
future_x = np.array([[350]])
pred = model.predict(future_x)[0]
print(f"3. Geschatte frontend-tijd bij 350ms backend: {pred:.2f} ms")
```

**Interpretatie:**

De $R^2$ is enorm hoog (rond de 0.88), wat aantoont dat de backend verwerkingstijd een zeer robuuste en betrouwbare verklarende variabele is voor de fluctuaties in het front-end renderen.

## Oplossing Vraag 5

```python
from statsmodels.tsa.holtwinters import ExponentialSmoothing

# 1 & 2. Holt-Winters model trainen (Triple Exponential Smoothing)
# De data vereist een additieve trend en seizoenscomponent met periode 12.
model = ExponentialSmoothing(
    df_bezoekers['Bezoekers'],
    trend='add',
    seasonal='add',
    seasonal_periods=12
)
fit_model = model.fit(optimized=True)

# 3. Voorspellen voor de volgende 4 maanden
forecast = fit_model.forecast(steps=4)

# Visualisatie
plt.figure(figsize=(10, 5))
plt.plot(df_bezoekers.index, df_bezoekers['Bezoekers'], label='Actuele Bezoekers', marker='o')
plt.plot(forecast.index, forecast, label='Voorspelling (4 maanden)', color='red', linestyle='--', marker='s')
plt.title('KLJ Bezoekersaantallen - Historiek en Voorspelling')
plt.xlabel('Datum')
plt.ylabel('Aantal Bezoekers')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

**Uitleg:**

Doordat het websiteverkeer structureel groeit én gebonden is aan jaarlijkse piekmomenten (zoals de jaarlijkse evenementen), is een Triple Exponential Smoothing (Holt-Winters) model met zowel een additieve trend- als seizoenscomponent de geschikte keuze. Het model vangt zo zowel de structurele groei als de terugkerende seizoenspieken op in zijn voorspellingen.

> *Opmerking: in het originele document was deze laatste alinea afgekapt; de slotzin is hier logisch aangevuld.*
