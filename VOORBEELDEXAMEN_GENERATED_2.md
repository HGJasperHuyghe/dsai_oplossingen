# Voorbeeldexamen - moeilijke labo's

## 🛠️ Voorbereiding: Datasets Genereren

```python
import pandas as pd
import numpy as np

np.random.seed(42)

# Dataset 1: Smart City E-steps (Labs 1.04, 2.06, 2.07)
df_steps = pd.DataFrame({
    'Afstand_km': np.random.uniform(0.5, 15.0, 500),
    'Gebruiker_Type': np.random.choice(['Abonnee', 'Toerist', 'Student'], 500),
    'Step_Model': np.random.choice(['Eco', 'Sport', 'Pro'], 500),
    'Batterij_Start': np.random.uniform(20, 100, 500)
})

# Dataset 2: Batterij Kwaliteitscontrole (Labs 3.03, 3.04)
# Geclaimde levensduur is 500 cycli. We genereren data die nét iets lager ligt.
df_batteries = pd.DataFrame({'Cycli': np.random.normal(loc=492, scale=18, size=45)})

# Dataset 3: Esports Reactietijden (Labs 5.01, 5.04)
reactie_pre = np.random.normal(250, 20, 30)
reactie_post = reactie_pre - np.random.normal(15, 5, 30)  # Energy drink effect
df_esports_paired = pd.DataFrame({'Speler': range(1, 31), 'Pre_Drink_ms': reactie_pre, 'Post_Drink_ms': reactie_post})

muzen = np.concatenate([np.random.normal(240, 15, 20), np.random.normal(220, 10, 20), np.random.normal(235, 12, 20)])
df_esports_muis = pd.DataFrame({
    'Muis_Merk': ['Logitech']*20 + ['Razer']*20 + ['Corsair']*20,
    'Reactie_ms': muzen
})

# Dataset 4: Streamingdienst Genres (Labs 4.07, 4.08)
df_streaming_dagen = pd.DataFrame({'Dag': ['Ma', 'Di', 'Wo', 'Do', 'Vr', 'Za', 'Zo'],
                                   'Kijkers': [1050, 980, 1020, 1010, 1100, 1060, 990]})
leeftijden = np.random.normal(35, 15, 1000).astype(int)
leeftijden = leeftijden[(leeftijden >= 10) & (leeftijden <= 80)]
df_streaming_leeftijd = pd.DataFrame({'Leeftijd': leeftijden})

# Dataset 5: Vastgoedprijzen (Lab 6.01)
oppervlakte = np.random.uniform(50, 250, 60)
prijs = 50000 + (3000 * oppervlakte) + np.random.normal(0, 40000, 60)
df_vastgoed = pd.DataFrame({'Oppervlakte_m2': oppervlakte, 'Prijs_EUR': prijs})

# Dataset 6: E-commerce Sales & Crypto (Labs 7.04, 7.07, 7.08)
# Dagelijkse data voor Golden Cross
df_crypto = pd.DataFrame({'Datum': pd.date_range('2022-01-01', '2023-12-31'),
                          'Prijs': np.cumsum(np.random.normal(0.5, 2, 730)) + 100}).set_index('Datum')

# Kwartaaldata (Let op het string formaat!)
quarters = [f"Q{q} {y}" for y in range(2018, 2024) for q in range(1, 5)]
sales = [100 + (i*2) + (30 if i%4==3 else 0) + np.random.normal(0,5) for i in range(len(quarters))]
df_quarterly = pd.DataFrame({'Kwartaal': quarters, 'Omzet_Miljoen': sales})
```

---

# 📝 DEEL 1: Opgaven

## Vraag 1: Data Manipulatie & Visualisatie (Smart City E-steps)

*Gerelateerd aan labs 1.04, 2.06, 2.07.* Gebruik `df_steps`.

1. **Conversie (2.07):** Zet de kolommen `Gebruiker_Type` en `Step_Model` om naar categorische variabelen. Zorg dat `Step_Model` een ordinale volgorde heeft: `'Eco' < 'Sport' < 'Pro'`.
2. **Bivariate Visualisatie (2.06):** Maak een geclusterd staafdiagram (countplot) dat toont hoe vaak elk `Step_Model` gebruikt wordt, opgesplitst per `Gebruiker_Type` (kleur/hue). Maak daarnaast een boxplot van de `Afstand_km` per `Step_Model`.
3. **Interpolatie & Cumulatieve som (1.04):** We willen weten bij welke afgelegde afstand de helft van alle ritten voltooid is. Sorteer de dataset op `Afstand_km`. Bereken de cumulatieve som van het aantal ritten. Zet dit om naar percentages (0–100%). Gebruik `scipy.interpolate.interp1d` om exact te berekenen bij welke afstand 50% van de ritten bereikt is.

## Vraag 2: Kansrekening & CLT (Cloud Servers)

*Gerelateerd aan labs 3.01, 3.02.*

1. **Theoretische kansen (3.01 – Basis & Verwachte Waarde):** Een cloudprovider heeft 3 servers. $P(\text{Server A faalt}) = 0.10$, $P(\text{Server B faalt}) = 0.05$. Ze falen onafhankelijk van elkaar.
   - Wat is de kans dat ze beide tegelijk falen?
   - Wat is de kans dat minstens één van de twee faalt?
   - Een downtime kost €5000 als 1 server faalt, en €25000 als ze beide falen. Wat is de verwachte kost $E(X)$ van downtime?
2. **Kansrekening (3.01 – Moeilijk / Combinatoriek):** Er staan 12 servers in een rack. 3 daarvan zijn defect. Een technieker trekt willekeurig 2 servers uit het rack om te testen (zonder terugleggen). Wat is de kans dat hij exact 1 defecte en 1 werkende server trekt?
3. **CLT (3.02):** De responstijd van een server is onbekend verdeeld, maar heeft een gemiddelde van $\mu = 45$ ms en een standaardafwijking van $\sigma = 12$ ms. We nemen een steekproef van $n = 64$ requests. Wat is de kans dat het steekproefgemiddelde $\bar{x}$ groter is dan 48 ms?

## 🚨 Vraag 3: Hypothesetoetsen & Betrouwbaarheidsintervallen (Batterijen)

*Gerelateerd aan labs 3.03, 3.04 (Extra Aandacht!).* Gebruik `df_batteries`. Een fabrikant claimt dat hun batterijen gemiddeld 500 laadcycli meegaan. Een onafhankelijk bureau test 45 batterijen.

1. **Betrouwbaarheidsinterval (3.03):** Bereken het 95% betrouwbaarheidsinterval voor de gemiddelde levensduur op basis van de steekproef. (Kies bewust tussen Z of T!).
2. **Hypothesetoets (3.04):** Het bureau vermoedt dat de batterijen in werkelijkheid minder lang meegaan dan de geclaimde 500 cycli. Toets dit op een significantieniveau van $\alpha = 0.01$.
   - **Stap A:** Formuleer $H_0$ en $H_1$. Is dit linkszijdig, rechtszijdig of tweezijdig?
   - **Stap B:** Welke toets gebruik je en waarom? (Z-toets of T-toets?)
   - **Stap C:** Bereken de test-statistiek en de p-waarde.
   - **Stap D:** Formuleer een duidelijke, zakelijke conclusie.

## Vraag 4: Chi-Kwadraat Toetsen (Streaming)

*Gerelateerd aan labs 4.07, 4.08.*

1. **Uniforme verdeling (4.08 herwerkt):** Gebruik `df_streaming_dagen`. Men beweert dat er elke dag van de week evenveel gekeken wordt. Voer een $\chi^2$-aanpassingstoets uit om dit te controleren ($\alpha = 0.05$). Is er een voorkeursdag?
2. **Met groepering (4.07):** Gebruik `df_streaming_leeftijd`. Deel de leeftijden op in de volgende bins: `[0-20]`, `[21-40]`, `[41-60]`, `[61-80]`. Volgens marktonderzoek zou de verdeling van de kijkers als volgt moeten zijn: 15% jongeren (0–20), 40% volwassenen (21–40), 30% middelbaar (41–60), 15% ouderen (61–80). Is jouw steekproef representatief voor dit marktonderzoek?

## Vraag 5: T-toetsen voor groepen (Esports)

*Gerelateerd aan labs 5.01, 5.04.*

1. **Gepaarde T-toets (5.01):** Gebruik `df_esports_paired`. We testen dezelfde spelers vóór en ná het drinken van een energiedrank. Heeft de drank de reactietijd significant verlaagd (verbeterd)? Bereken ook Cohen's $d$ voor de effectgrootte.
2. **Meerdere groepen (5.04):** Gebruik `df_esports_muis`. We testen 3 verschillende merken muizen. Voer een One-Way ANOVA uit. Is er een significant verschil in reactietijd tussen de merken ($\alpha = 0.05$)?

## Vraag 6: Lineaire Regressie (Vastgoed)

*Gerelateerd aan lab 6.01.* Gebruik `df_vastgoed`.

1. Maak een scatterplot en voeg de regressielijn toe.
2. Bereken de regressievergelijking ($\hat{y} = \beta_0 + \beta_1 x$).
3. Wat zijn de correlatiecoëfficiënt ($R$) en de determinatiecoëfficiënt ($R^2$)? Verklaar de betekenis van $R^2$ in deze context.

## 🚨 Vraag 7: Tijdreeksanalyse (Sales & Crypto)

*Gerelateerd aan labs 7.04, 7.07, 7.08 (Extra Aandacht!).*

1. **Golden Cross (7.07):** Gebruik `df_crypto`. Bereken de 50-dagen (SMA50) en 200-dagen (SMA200) Moving Averages. Plot de prijs samen met deze MA's. Bepaal via code op welke datums een "Golden Cross" plaatsvindt (waar SMA50 de SMA200 naar boven kruist).
2. **Kwartaaldata & Holt-Winters (7.08 herwerkt):** Gebruik `df_quarterly`.
   - **Stap A (Cruciaal 7.08):** De kolom `'Kwartaal'` bevat strings zoals `"Q1 2018"`. Schrijf een parser-functie en gebruik `pd.to_datetime` (of vergelijkbaar) om dit om te zetten naar een échte `DatetimeIndex` die telkens start op de eerste dag van de eerste maand van dat kwartaal (bv. `2018-01-01`).
   - **Stap B (7.04):** Pas Holt-Winters (Triple Exponential Smoothing) toe. Omdat er sprake is van groei en een vaste kwartaalpiek (Q4), gebruik je een additieve trend en additieve seizoenscomponent ($period = 4$).
   - **Stap C:** Voorspel de omzet voor het volledige jaar 2024 (4 kwartalen) en plot het resultaat.

---

# 🔑 DEEL 2: Oplossingssleutel & Uitleg

## Oplossing Vraag 1 (Pandas Manipulatie)

```python
import matplotlib.pyplot as plt
import seaborn as sns
from pandas.api.types import CategoricalDtype
from scipy.interpolate import interp1d
import numpy as np

# 1. Conversie
df_steps['Gebruiker_Type'] = df_steps['Gebruiker_Type'].astype('category')
step_type = CategoricalDtype(categories=['Eco', 'Sport', 'Pro'], ordered=True)
df_steps['Step_Model'] = df_steps['Step_Model'].astype(step_type)

# 2. Bivariate Plots
fig, axes = plt.subplots(1, 2, figsize=(14, 5))
sns.countplot(data=df_steps, x='Step_Model', hue='Gebruiker_Type', ax=axes[0])
axes[0].set_title('Aantal ritten per Step Model & Gebruiker')

sns.boxplot(data=df_steps, x='Step_Model', y='Afstand_km', ax=axes[1])
axes[1].set_title('Afstand per Step Model')
plt.show()

# 3. Cumulatieve som & Interpolatie (Lab 1.04 style)
# Sorteer afstanden oplopend
afstanden = df_steps['Afstand_km'].sort_values().values
# Cumulatieve array maken (1, 2, 3... 500)
aantal_ritten = np.arange(1, len(afstanden) + 1)
# Omzetten naar %
percentages = (aantal_ritten / len(afstanden)) * 100

# Interpolatie functie (Y is percentage, we zoeken X afstand)
calc_afstand = interp1d(percentages, afstanden, kind='linear')
p50_afstand = calc_afstand(50)

print(f"50% van alle ritten is korter dan of gelijk aan {p50_afstand:.2f} km.")
```

## Oplossing Vraag 2 (Kansrekening & CLT)

```python
import scipy.stats as stats

# 1. Theoretische kansen
pA = 0.10
pB = 0.05
p_beide = pA * pB
p_minstens_een = pA + pB - p_beide

# Verwachte kost: E(X) = som(kost_i * P_i)
# P(exact 1 faalt) = P(minstens 1) - P(beide)
p_exact_een = p_minstens_een - p_beide
verwachte_kost = (p_exact_een * 5000) + (p_beide * 25000) + ((1 - p_minstens_een) * 0)

print(f"P(Beide falen): {p_beide:.4f}")
print(f"P(Minstens één): {p_minstens_een:.4f}")
print(f"Verwachte downtime kost: €{verwachte_kost:.2f}")

# 2. Combinatoriek (Hypergeometrische kans of vaasmodel)
# Totaal: 12. Defect: 3. Werkend: 9. Trek: 2. Kans op: 1 defect, 1 werkend.
# P = (Kies 1 uit 3) * (Kies 1 uit 9) / (Kies 2 uit 12)
import math
kans_1d_1w = (math.comb(3, 1) * math.comb(9, 1)) / math.comb(12, 2)
print(f"Kans op 1 defect en 1 werkend: {kans_1d_1w:.4f}")

# 3. CLT (Verdeling van het steekproefgemiddelde)
mu_clt = 45
sigma_clt = 12
n_clt = 64
se = sigma_clt / np.sqrt(n_clt)  # 12 / 8 = 1.5
p_groter_48 = stats.norm.sf(48, loc=mu_clt, scale=se)
print(f"P(x̄ > 48) volgens CLT: {p_groter_48:.4f}")
```

## 🚨 Oplossing Vraag 3 (Hypothesetoetsen 1-sample)

> Dit lab is cruciaal. Lees de stappen aandachtig.

```python
n_bat = len(df_batteries)
xbar_bat = df_batteries['Cycli'].mean()
s_bat = df_batteries['Cycli'].std()  # Steekproef standaardafwijking!

# 1. Betrouwbaarheidsinterval (Omdat sigma_populatie onbekend is, MOET je de T-verdeling gebruiken)
ci_95 = stats.t.interval(0.95, df=n_bat-1, loc=xbar_bat, scale=s_bat/np.sqrt(n_bat))
print(f"1. 95% BI voor levensduur: [{ci_95[0]:.2f}, {ci_95[1]:.2f}]")

# 2. Hypothesetoets (Lab 3.04)
# Stap A:
# H0: mu = 500
# H1: mu < 500 (We vermoeden dat ze MINDER lang meegaan, dus Linkszijdig!)
mu0 = 500
alpha = 0.01

# Stap B: We gebruiken de T-toets (stats.t.cdf) omdat we 's' (uit de steekproef) gebruiken en niet 'sigma'.
se_bat = s_bat / np.sqrt(n_bat)
t_stat = (xbar_bat - mu0) / se_bat

# Stap C: P-waarde berekenen (Linkszijdig = cdf)
p_val_bat = stats.t.cdf(t_stat, df=n_bat-1)

print(f"2. Steekproefgemiddelde: {xbar_bat:.2f}")
print(f"2. T-statistiek: {t_stat:.4f}")
print(f"2. p-waarde: {p_val_bat:.5f}")

# Stap D: Conclusie
if p_val_bat < alpha:
    print("Conclusie: Verwerp H0. Er is op 1% niveau significant bewijs dat de batterijen MINDER dan 500 cycli meegaan.")
else:
    print("Conclusie: Verwerp H0 niet. Er is onvoldoende bewijs om te stellen dat ze minder dan 500 cycli meegaan.")
```

**Toelichting bij Stap A & B:**

- $H_0$: $\mu = 500$
- $H_1$: $\mu < 500$ → **linkszijdig** (we vermoeden dat ze minder lang meegaan).
- We gebruiken een **T-toets**, omdat de populatiestandaardafwijking $\sigma$ onbekend is en we de steekproefstandaardafwijking $s$ moeten gebruiken.

## Oplossing Vraag 4 (Chi-Kwadraat)

```python
# 1. Uniforme verdeling (Wordt elke dag evenveel gekeken?)
obs_dagen = df_streaming_dagen['Kijkers'].values
verwacht_per_dag = obs_dagen.sum() / len(obs_dagen)
exp_dagen = [verwacht_per_dag] * len(obs_dagen)

chi2_dagen, p_dagen = stats.chisquare(f_obs=obs_dagen, f_exp=exp_dagen)
print(f"Uniformiteit: p-waarde = {p_dagen:.4f}. Is het uniform? {'Nee' if p_dagen < 0.05 else 'Ja'}.")

# 2. Met groepering (Lab 4.07)
# Bin de leeftijden
bins = [0, 20, 40, 60, 80]
labels = ['0-20', '21-40', '41-60', '61-80']
df_streaming_leeftijd['Groep'] = pd.cut(df_streaming_leeftijd['Leeftijd'], bins=bins, labels=labels)

obs_groepen = df_streaming_leeftijd['Groep'].value_counts()[labels].values
totaal_kijkers = len(df_streaming_leeftijd)

# De percentages uit het marktonderzoek:
exp_percentages = np.array([0.15, 0.40, 0.30, 0.15])
exp_groepen = exp_percentages * totaal_kijkers

chi2_groep, p_groep = stats.chisquare(f_obs=obs_groepen, f_exp=exp_groepen)
print(f"Representativiteit: p-waarde = {p_groep:.4f}. Representatief? {'Nee (verwerp H0)' if p_groep < 0.05 else 'Ja (behoud H0)'}.")
```

## Oplossing Vraag 5 (T-toetsen Groepen)

```python
# 1. Gepaarde T-toets (Pre vs Post op DEZELFDE spelers)
# H0: mu_pre <= mu_post
# H1: mu_pre > mu_post (Reactietijd DAALT, pre is dus groter dan post)
t_stat_paired, p_val_paired = stats.ttest_rel(df_esports_paired['Pre_Drink_ms'], df_esports_paired['Post_Drink_ms'], alternative='greater')

# Cohen's d (Verschil in gemiddelden gedeeld door de standaardafwijking van de verschillen)
verschillen = df_esports_paired['Pre_Drink_ms'] - df_esports_paired['Post_Drink_ms']
cohen_d = verschillen.mean() / verschillen.std()

print(f"Gepaarde T-toets p-waarde: {p_val_paired:.6f}")
print(f"Cohen's d: {cohen_d:.2f}")

# 2. ANOVA (3 groepen vergelijken)
# H0: mu_logitech = mu_razer = mu_corsair
g1 = df_esports_muis[df_esports_muis['Muis_Merk'] == 'Logitech']['Reactie_ms']
g2 = df_esports_muis[df_esports_muis['Muis_Merk'] == 'Razer']['Reactie_ms']
g3 = df_esports_muis[df_esports_muis['Muis_Merk'] == 'Corsair']['Reactie_ms']

f_stat, p_anova = stats.f_oneway(g1, g2, g3)
print(f"ANOVA p-waarde: {p_anova:.6f}. Significant verschil tussen muizen? {'Ja' if p_anova < 0.05 else 'Nee'}.")
```

## Oplossing Vraag 6 (Regressie)

```python
from sklearn.linear_model import LinearRegression

X = df_vastgoed[['Oppervlakte_m2']].values
y = df_vastgoed['Prijs_EUR'].values

model = LinearRegression()
model.fit(X, y)

beta_0 = model.intercept_
beta_1 = model.coef_[0]

r_val = np.corrcoef(df_vastgoed['Oppervlakte_m2'], y)[0, 1]
r_sq = model.score(X, y)  # of r_val**2

print(f"Regressievergelijking: Prijs = {beta_0:.2f} + {beta_1:.2f} * Oppervlakte")
print(f"Correlatie (R): {r_val:.4f}")
print(f"Determinatiecoëfficiënt (R²): {r_sq:.4f}")
# Betekenis R²: "[r_sq * 100]% van de variantie in de huisprijzen kan verklaard worden door de oppervlakte."

plt.scatter(df_vastgoed['Oppervlakte_m2'], y, color='grey')
plt.plot(df_vastgoed['Oppervlakte_m2'], model.predict(X), color='red')
plt.title('Huisprijs vs Oppervlakte')
plt.show()
```

## 🚨 Oplossing Vraag 7 (Tijdreeksanalyse: Quarters & Holt-Winters)

> Let in stap 2 goed op hoe je kwartaal-strings omzet. Dit is de kernmoeilijkheid van Lab 7.08.

```python
# --- Deel 1: Golden Cross (7.07) ---
df_crypto['SMA50'] = df_crypto['Prijs'].rolling(window=50).mean()
df_crypto['SMA200'] = df_crypto['Prijs'].rolling(window=200).mean()

# Een cross vindt plaats wanneer SMA50 > SMA200 wordt (verschil gaat van negatief naar positief)
df_crypto['Verschil'] = df_crypto['SMA50'] - df_crypto['SMA200']
df_crypto['Signaal'] = np.where(df_crypto['Verschil'] > 0, 1, 0)
df_crypto['Golden_Cross'] = df_crypto['Signaal'].diff()

cross_dates = df_crypto[df_crypto['Golden_Cross'] == 1].index
print("Datums Golden Cross:", cross_dates.date)

# --- Deel 2: Kwartaaldata parseren & Holt-Winters (7.08) ---
from statsmodels.tsa.holtwinters import ExponentialSmoothing

# STAP A: Custom parser voor "Q1 2018" strings
def parse_kwartaal(q_str):
    parts = q_str.split()
    kwartaal = int(parts[0][1])      # Haal de '1' uit 'Q1'
    jaar = int(parts[1])
    maand = (kwartaal - 1) * 3 + 1   # Q1->Jan(1), Q2->Apr(4), etc.
    return pd.Timestamp(year=jaar, month=maand, day=1)

# Toepassen en instellen als index
df_quarterly['Datum'] = df_quarterly['Kwartaal'].apply(parse_kwartaal)
df_quarterly.set_index('Datum', inplace=True)
# Belangrijk: Geef de frequentie mee als kwartaalstart (QS) zodat statsmodels dit snapt
df_quarterly.index.freq = 'QS-JAN'

# STAP B: Holt-Winters trainen
model_hw = ExponentialSmoothing(
    df_quarterly['Omzet_Miljoen'],
    trend='add',
    seasonal='add',
    seasonal_periods=4  # Omdat er 4 kwartalen in een jaar zitten
)
fit_hw = model_hw.fit()

# STAP C: Voorspellen
forecast_hw = fit_hw.forecast(steps=4)

plt.figure(figsize=(10, 5))
plt.plot(df_quarterly.index, df_quarterly['Omzet_Miljoen'], label='Historisch', marker='o')
plt.plot(forecast_hw.index, forecast_hw, label='Voorspelling 2024', color='red', linestyle='--', marker='s')
plt.title('Kwartaalomzet met Holt-Winters voorspelling')
plt.legend()
plt.show()
```
