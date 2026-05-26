
# Voorbeeldexamen 2026 - Oplossingen

## Vraag 1

### Exercise 1: Normale verdeling & Hypothesetoetsing

#### Overzicht

De diameter van een zuiger is normaal verdeeld met gemiddelde $\mu = 13.4$ cm en standaardafwijking $\sigma = 0.12$ cm. We onderzoeken de proportie binnen de tolerantiegrenzen, de kans dat meerdere zuigers falen, en we voeren een hypothesetoets uit.

#### Voorbereiding: Package imports

```python
import numpy as np
import scipy.stats as stats
```

#### Stap 1: Proportie binnen de tolerantiegrenzen

Om de kans te vinden dat een zuiger tussen 13.35 cm en 13.5 cm valt, bereken je de oppervlakte tussen deze twee grenzen met de cumulatieve verdelingsfunctie (CDF).

```python
mu = 13.4
sigma = 0.12

# P(13.35 < D < 13.5) = cdf(bovengrens) - cdf(ondergrens)
prop_within_tolerance = stats.norm.cdf(13.5, loc=mu, scale=sigma) - stats.norm.cdf(13.35, loc=mu, scale=sigma)
print(f"Proportie binnen de grenzen: {prop_within_tolerance:.4f}")
# Verwachte output: ~0.4589 of 45.89%
```

#### Stap 2: Kans dat geen van de 3 zuigers binnen de grenzen valt

Bepaal eerst de kans dat één zuiger faalt. Gebruik vervolgens de basisregels van kansrekening (vermenigvuldiging voor onafhankelijke gebeurtenissen).

```python
# Kans dat ÉÉN zuiger faalt
p_fail = 1 - prop_within_tolerance

# Kans dat DRIE zuigers na elkaar falen
p_all_three_fail = p_fail ** 3
print(f"Kans dat geen voldoet aan de grenzen: {p_all_three_fail:.4f}")
# Verwachte output: ~0.1584 of 15.84%
```

#### Stap 3: Hypothesetoets op 5% significantieniveau

Omdat de populatiestandaardafwijking ($\sigma$) expliciet gegeven is, gebruiken we de standaard normale Z-verdeling voor de toets. Omdat we testen of "er aanpassingen" zijn gebeurd (het gemiddelde kan dus hoger óf lager zijn), is dit een tweezijdige toets.

```python
import numpy as np

m = 13.43         # Steekproefgemiddelde
sigma = 0.12      # Populatiestandaardafwijking
n = 20            # Steekproefgrootte
mu0 = 13.4        # Verwacht populatiegemiddelde (Nulhypothese)
alpha = 0.05      # Significantieniveau

# 1. Standaardfout
se = sigma / np.sqrt(n)

# 2. Tweezijdige p-waarde
p_val = min(stats.norm.cdf(m, loc=mu0, scale=se),
            stats.norm.sf(m, loc=mu0, scale=se)) * 2

print(f"p-waarde: {p_val:.4f}")
# Verwachte output: ~0.2635

if p_val < alpha:
    print("Verwerp H0: Er zijn aanpassingen gebeurd.")
else:
    print("Verwerp H0 niet: Onvoldoende bewijs dat er aanpassingen gebeurd zijn.")
```

#### Hypotheses

- $H_0$: $\mu = 13.4$ (er zijn geen aanpassingen gebeurd)
- $H_1$: $\mu \neq 13.4$ (er zijn aanpassingen gebeurd)

#### Conclusie

De p-waarde (~0.2635) is groter dan $\alpha = 0.05$. We verwerpen $H_0$ niet: er is onvoldoende bewijs om te besluiten dat er aanpassingen aan het productieproces zijn gebeurd.

---

## Vraag 2

### Exercise 2: Pandas & Tijdreeksanalyse

#### Overzicht

Analyse van een dataset `dfemployees` met een datumkolom en het aantal werknemers over de tijd. We onderzoeken de datatypes, converteren de datum, maken een tijdreeksgrafiek en voorspellen toekomstige waarden.

#### Stap 1: Datatypes van beide kolommen

```python
print(dfemployees.dtypes)
```

#### Stap 2: Converteer de 'date' kolom naar datetime-type

```python
import pandas as pd

dfemployees['recording_date'] = pd.to_datetime(dfemployees['recording_date'])
```

#### Stap 3: Maak de grafiek

Voor een tijdreeks met een Datetime-index en een kwantitatieve variabele is een standaard lijngrafiek vereist.

```python
import matplotlib.pyplot as plt

plt.figure(figsize=(10, 5))
plt.plot(dfemployees['recording_date'], dfemployees['number'], linewidth=2)
plt.title('Aantal werknemers over de tijd')
plt.xlabel('Datum')
plt.ylabel('Werknemers')
plt.show()
```

#### Stap 4: Voorspellingsmethode en motivatie

**Methode**: Triple Exponential Smoothing (Holt-Winters).

**Motivatie**: Je kiest Holt-Winters (TES) omdat deze specifieke methode het best geschikt is om zowel onderliggende trends als terugkerende seizoenscycli in de gegevens vast te leggen.

#### Stap 5: Maak een grafiek inclusief de voorspelling

```python
from statsmodels.tsa.holtwinters import ExponentialSmoothing

# Fit het model (met 12 maanden als seizoensperiode)
model = ExponentialSmoothing(dfemployees['number'], trend='add', seasonal='add', seasonal_periods=12)
fit_model = model.fit(optimized=True)

# Voorspel 4 stappen vooruit
forecast = fit_model.forecast(steps=4)

# Plot werkelijke data + voorspelling
plt.figure(figsize=(10, 5))
plt.plot(dfemployees.index, dfemployees['number'], label='Actual')
plt.plot(forecast.index, forecast, label='Forecast (Next 4 Months)', color='red', linestyle='--')
plt.legend()
plt.show()
```

#### Stap 6.1: Gemiddeld aantal werknemers in 2020

```python
avg_2020 = dfemployees[dfemployees['recording_date'].dt.year == 2020]['number'].mean()
print(f"Gemiddelde in 2020: {avg_2020}")
```

#### Stap 6.2: Aantal maanden met meer dan 400 werknemers

```python
months_over_400 = (dfemployees['number'] > 400).sum()
print(f"Maanden > 400: {months_over_400}")
```

---

## Vraag 3

### Exercise 3: Bivariate analyse (Categorisch vs Categorisch)

#### Overzicht

Analyse van de Flanders Travel Behavior Survey waarbij twee categorische variabelen (inkomenscategorie vs. rijbewijsbezit) onderzocht worden op onderlinge afhankelijkheid.

> **Opmerking**: De eigenlijke numerieke tabel uit de Flanders Travel Behavior Survey ontbrak in de opgave. De onderstaande stappen beschrijven precies hoe je de antwoorden bekomt zodra je de gegevens in een Pandas crosstab invoert.

#### Stap 1: Welke hypothesetoets pas je toe?

De **Chi-kwadraat onafhankelijkheidstoets** (`stats.chi2_contingency`). Je vergelijkt twee kwalitatieve/categorische variabelen (inkomenscategorie vs. rijbewijsbezit) om na te gaan of ze elkaar beïnvloeden.

#### Stap 2: Formuleer de hypotheses

- $H_0$: Er is geen verband tussen inkomen en het hebben van een rijbewijs (ze zijn onafhankelijk).
- $H_1$: Er is een verband tussen inkomen en het hebben van een rijbewijs (ze zijn afhankelijk).

#### Stap 3: Teststatistiek en p-waarde

```python
# Ervan uitgaande dat je crosstab als volgt opgebouwd is:
# crosstab = pd.crosstab(df['Income'], df['Has_License'])

chi2_stat, p_value, dof, expected_matrix = stats.chi2_contingency(crosstab)

print(f"Teststatistiek (χ²): {chi2_stat:.4f}")
print(f"p-waarde: {p_value:.4f}")
```

#### Stap 4: Conclusie

Vergelijk je `p_value` met $\alpha = 0.05$. Als $p < 0.05$, verwerp $H_0$ en besluit dat er een statistisch significant verband is tussen het netto-inkomen en het bezitten van een rijbewijs. Als $p \ge 0.05$, aanvaard je $H_0$.

#### Stap 5: Percentage zonder rijbewijs

Om dit te vinden, neem je de totale som van personen zonder rijbewijs en deel je die door het algemeen totaal van de enquête.

```python
# no_license_percent = (total_no_license / grand_total) * 100
```

---

## Vraag 4

### Exercise 4: Lineaire regressie & Onafhankelijke T-toets

#### Overzicht

Analyse van een dataset `dfrestaurants` met scores voor eten, decor en service, prijs en locatie. We berekenen een totaalscore, maken een spreidingsdiagram, voeren lineaire regressie uit en vergelijken de prijzen tussen twee locaties met een t-toets.

#### Stap 1: Bereken de totaalscore

```python
dfrestaurants['total_score'] = dfrestaurants['food'] + dfrestaurants['decor'] + dfrestaurants['service']
```

#### Stap 2: Maak een spreidingsdiagram

Voor twee kwantitatieve variabelen gebruiken we een spreidingsdiagram (scatterplot).

```python
import seaborn as sns

sns.scatterplot(data=dfrestaurants, x='total_score', y='price')
# Of met een regressielijn: sns.lmplot(data=dfrestaurants, x='total_score', y='price')
plt.show()
```

#### Stap 3: Lineaire regressievergelijking

```python
from sklearn.linear_model import LinearRegression

X = dfrestaurants[['total_score']].values
y = dfrestaurants['price'].values

model = LinearRegression()
model.fit(X, y)

beta_0 = model.intercept_
beta_1 = model.coef_[0]

print(f"Vergelijking: ŷ = {beta_0:.4f} + {beta_1:.4f}x")
```

#### Stap 4: Geschatte prijs voor totaalscore = 70

```python
future_x = np.array([[70]])
prediction = model.predict(future_x)[0]
print(f"Geschatte prijs: ${prediction:.2f}")
```

#### Stap 5: Correlatiecoëfficiënt

```python
# Het symbool is r (Pearson's R)
r_value = np.corrcoef(dfrestaurants['total_score'], dfrestaurants['price'])[0, 1]
print(f"Correlatie (r): {r_value:.4f}")
```

#### Stap 6: Interpretatie van de correlatie

Op basis van de `r_value` bepaal je de richting en sterkte. Een waarde dichter bij 1 wijst op een sterke positieve correlatie, terwijl een waarde dichter bij 0 wijst op een zwakke correlatie.

#### Stap 7: Beschrijvende statistieken

```python
print(dfrestaurants.describe())
```

#### Stap 8: Variatiecoëfficiënt voor de eten-score

```python
cv_food = dfrestaurants['food'].std() / dfrestaurants['food'].mean()
print(f"Variatiecoëfficiënt (eten): {cv_food:.4f}")
```

#### Stap 9: NYC-restaurants met prijs > $65

```python
expensive_nyc = len(dfrestaurants[(dfrestaurants['location'] == 'NYC') & (dfrestaurants['price'] > 65)])
print(f"NYC-restaurants > $65: {expensive_nyc}")
```

#### Stap 10: T-toets — Zijn de prijzen in NYC significant hoger?

**Toets**: Onafhankelijke tweesteekproef t-toets (`stats.ttest_ind`).

**Hypotheses**:

- $H_0$: $\mu_{NYC} = \mu_{LI}$ (de prijzen zijn gelijk op beide locaties)
- $H_1$: $\mu_{NYC} > \mu_{LI}$ (de prijzen in NYC zijn hoger). Dit is een rechtszijdige toets.

```python
nyc_prices = dfrestaurants[dfrestaurants['location'] == 'NYC']['price']
li_prices = dfrestaurants[dfrestaurants['location'] == 'LI']['price']

# We voegen 'alternative="greater"' toe omdat we nagaan of groep A (NYC) groter is dan B (LI)
# We gebruiken equal_var=False als moderne statistische best practice (Welch's T-toets)
stat, p_val_ttest = stats.ttest_ind(nyc_prices, li_prices, equal_var=False, alternative='greater')

print(f"p-waarde: {p_val_ttest:.4f}")

if p_val_ttest < 0.05:
    print("Verwerp H0: De prijzen in NYC zijn significant hoger dan op Long Island.")
else:
    print("Verwerp H0 niet: Onvoldoende bewijs dat de prijzen in NYC hoger zijn.")
```

---

## Samenvatting Voorbeeldexamen

In dit voorbeeldexamen komen de belangrijkste technieken uit de cursus aan bod:

### Concepten

- **Normale verdeling**: Kansberekening met de CDF en toetsing met de Z-verdeling
- **Hypothesetoetsing**: Een- en tweezijdige toetsen, p-waarde vs. significantieniveau $\alpha$
- **Tijdreeksanalyse**: Trend en seizoenaliteit met Holt-Winters
- **Bivariate analyse**: Verband tussen variabelen onderzoeken

### Toetsen per variabeletype

1. **Z-toets**: Gemiddelde toetsen wanneer $\sigma$ gekend is
2. **Chi-kwadraat toets**: Categorisch vs. categorisch (onafhankelijkheid)
3. **Onafhankelijke t-toets**: Kwantitatief gemiddelde vergelijken tussen twee groepen
4. **Lineaire regressie & correlatie**: Verband tussen twee kwantitatieve variabelen

### Kerngetallen

- **r** (Pearson's correlatiecoëfficiënt): Richting en sterkte van lineair verband
- **Variatiecoëfficiënt**: Relatieve spreiding (std / gemiddelde)
- **p-waarde**: Beslissingsgrond bij hypothesetoetsing
