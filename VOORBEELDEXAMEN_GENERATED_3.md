# Voorbeeldexamen - Kiezen & omzetten van data

## 🛠️ Voorbereiding: Datasets Genereren

```python
import pandas as pd
import numpy as np

np.random.seed(42)

# Dataset 1: Webshop Prijzen & Regio's (Voor datacleaning & Chi-kwadraat)
prijzen = ["€ 1.250,50", "€ 890,00", "€ 2.100,75", "€ 450,99", "€ 1.150,00"] * 20
regios = np.random.choice(['Noord', 'Zuid', 'Oost', 'West'], size=100, p=[0.4, 0.2, 0.2, 0.2])
df_webshop = pd.DataFrame({'Prijs_String': prijzen, 'Regio': regios})

# Dataset 2: HR Training (Voor data omzetting & T-toets)
# Zelfde werknemers, twee meetmomenten
df_hr = pd.DataFrame({
    'Werknemer_ID': range(1, 31),
    'Score_Voor_Training': np.random.normal(65, 10, 30),
    'Score_Na_Training': np.random.normal(72, 8, 30)
})

# Dataset 3: Klanttevredenheid (Voor Meetniveaus & ANOVA)
# Drie onafhankelijke abonnementsvormen
df_abos = pd.DataFrame({
    'Klant_ID': range(101, 191),
    'Abonnement': ['Basic']*30 + ['Premium']*30 + ['VIP']*30,
    'Tevredenheid_Score': np.concatenate([
        np.random.normal(6, 1.5, 30),
        np.random.normal(7.5, 1.2, 30),
        np.random.normal(8.5, 1.0, 30)
    ])
})

# Dataset 4: Productie Foutmarges (Voor Z/T-toets 1 sample)
df_productie = pd.DataFrame({'Dikte_mm': np.random.normal(10.05, 0.08, 25)})
```

---

# 📝 DEEL 1: Opgaven

## Vraag 1: Data Omzetten & Toetskeuze (Webshop)

Gebruik `df_webshop`.

**A. Data Omzetten:**
De kolom `Prijs_String` is momenteel onbruikbaar voor berekeningen (`"€ 1.250,50"`). Schrijf de Pandas code om deze kolom om te zetten naar een zuivere float (noem de nieuwe kolom `Prijs_Float`).

**B. Toetskeuze & Uitvoering:**
Het management verwachtte dat de verkopen als volgt verdeeld zouden zijn: Noord (25%), Zuid (25%), Oost (25%), West (25%). Ze willen weten of de huidige dataset significant afwijkt van deze verdeling ($\alpha = 0.05$).

1. Welke toets gebruik je hiervoor en waarom?
2. Voer de toets uit en formuleer je conclusie.

## Vraag 2: Breinbreker Toetskeuze (HR Training)

Gebruik `df_hr`. De HR-afdeling wil bewijzen dat de nieuwe training de scores van de werknemers significant heeft verhoogd ($\alpha = 0.05$).

1. Welke toets moet je hier gebruiken? (Kies uit: 1-sample t-toets, onafhankelijke t-toets, gepaarde t-toets of ANOVA). Motiveer je keuze.
2. Voer de correcte toets uit. Let op je `alternative` parameter.
3. **Data Omzetten:** Om een Seaborn boxplot te maken die 'Voor' en 'Na' vergelijkt, moet de data in een *long format* staan. Gebruik `pd.melt()` om de dataset om te vormen zodat je twee kolommen hebt: `Meetmoment` (`'Score_Voor_Training'` of `'Score_Na_Training'`) en `Score`.

## Vraag 3: Meerdere Groepen Vergelijken (Abonnementen)

Gebruik `df_abos`. We willen weten of er een significant verschil zit in de gemiddelde tevredenheid tussen de drie abonnementsvormen (Basic, Premium, VIP) op $\alpha = 0.05$.

1. Welke toets gebruik je hiervoor en waarom?
2. Voer de toets uit en noteer je conclusie.

## Vraag 4: De 1-Sample Valstrik (Productie)

Gebruik `df_productie`. Een machine is ingesteld om stalen platen te maken met een doeldikte van 10.00 mm. Een kwaliteitscontroleur neemt een steekproef van 25 platen en wil op $\alpha = 0.05$ weten of de machine nog steeds correct is afgesteld, of dat er een afwijking is (dikker óf dunner). De populatiestandaardafwijking is onbekend.

1. Welke toets gebruik je (Z-toets of T-toets)? Motiveer waarom.
2. Is deze toets eenzijdig of tweezijdig?
3. Voer de toets uit.

---

# 🔑 DEEL 2: Oplossingssleutel & De "Hoe herken je dit?" Gids

## Oplossing Vraag 1 (Webshop)

**A. Data Omzetten:**

```python
# Verwijder het euroteken, vervang de duizendtalscheiding (punt) door niets,
# en vervang de decimale komma door een punt.
df_webshop['Prijs_Float'] = (df_webshop['Prijs_String']
                             .str.replace('€', '', regex=False)
                             .str.replace(' ', '', regex=False)
                             .str.replace('.', '', regex=False)
                             .str.replace(',', '.', regex=False)
                             .astype(float))
```

**B. Toetskeuze & Uitvoering:**

- **Welke toets?** De **Chi-Kwadraat Aanpassingstoets** (Goodness-of-Fit test / `stats.chisquare`).
- **Hoe herken je dit?** Je hebt één categorische variabele (`Regio`) en je wil checken of de getelde frequenties overeenkomen met een theoretisch/verwacht percentage (25% voor alles).

```python
import scipy.stats as stats

# Geobserveerde waarden
obs = df_webshop['Regio'].value_counts()

# Verwachte waarden (25% van 100 observaties = 25 per regio)
# Let op: zorg dat de verwachte waarden in dezelfde volgorde staan als obs!
verwacht = [25, 25, 25, 25]

chi2_stat, p_val = stats.chisquare(f_obs=obs.values, f_exp=verwacht)

print(f"p-waarde: {p_val:.4f}")
# Als p < 0.05: Verwerp H0. De verdeling wijkt af van de verwachte 25% per regio.
```

## Oplossing Vraag 2 (HR Training)

**1. Toetskeuze:**

- **Welke toets?** Een **Gepaarde T-toets** (Paired T-test / `stats.ttest_rel`).
- **Hoe herken je dit?** Het sleutelwoord is *dezelfde werknemers*. Er is een 'Voor' en 'Na' meting op dezelfde proefpersonen. De data is dus afhankelijk van elkaar.

**2. Uitvoering:**

```python
# Rechtszijdig (We toetsen of NA groter is dan VOOR, ofwel: of de scores verhoogd zijn)
# H0: score_na <= score_voor
# H1: score_na > score_voor
t_stat, p_val = stats.ttest_rel(df_hr['Score_Na_Training'], df_hr['Score_Voor_Training'], alternative='greater')
print(f"p-waarde gepaarde t-toets: {p_val:.5f}")
```

**3. Data Omzetten (Melt):**

Waarom is dit belangrijk? Seaborn's `boxplot(x=..., y=...)` wil één kolom voor de labels op de X-as en één kolom voor de waardes op de Y-as. Een brede dataset moet "gesmolten" worden naar lang.

```python
import matplotlib.pyplot as plt
import seaborn as sns

df_hr_long = pd.melt(df_hr,
                     id_vars=['Werknemer_ID'],
                     value_vars=['Score_Voor_Training', 'Score_Na_Training'],
                     var_name='Meetmoment',
                     value_name='Score')

sns.boxplot(data=df_hr_long, x='Meetmoment', y='Score')
plt.title('Scores Voor vs Na Training')
plt.show()
```

## Oplossing Vraag 3 (Abonnementen)

**1. Toetskeuze:**

- **Welke toets?** **One-Way ANOVA** (`stats.f_oneway`).
- **Hoe herken je dit?** Je vergelijkt het gemiddelde van een kwantitatieve variabele (Tevredenheid) verdeeld over 3 of meer onafhankelijke groepen (Basic, Premium, VIP). Een t-toets kan maar maximaal 2 groepen aan.

**2. Uitvoering:**

```python
groep_basic = df_abos[df_abos['Abonnement'] == 'Basic']['Tevredenheid_Score']
groep_premium = df_abos[df_abos['Abonnement'] == 'Premium']['Tevredenheid_Score']
groep_vip = df_abos[df_abos['Abonnement'] == 'VIP']['Tevredenheid_Score']

f_stat, p_val_anova = stats.f_oneway(groep_basic, groep_premium, groep_vip)
print(f"p-waarde ANOVA: {p_val_anova:.6f}")
# Als p < 0.05 verwerp je H0. Conclusie: Er is een significant verschil in tevredenheid tussen minstens twee van de abonnementen.
```

## Oplossing Vraag 4 (De 1-Sample Valstrik)

**1 & 2. Toetskeuze:**

- **Welke toets?** Een **1-sample T-toets** (`stats.t.sf` / `stats.t.cdf` of `scipy.stats.ttest_1samp`).
- **Hoe herken je dit?** Je vergelijkt het gemiddelde van 1 steekproef met een vaste theoretische waarde (10.00 mm). Omdat de populatiestandaardafwijking ($\sigma$) onbekend is (je hebt alleen de steekproefdata), moet je de t-verdeling gebruiken, niet de Z-verdeling!
- **Eenzijdig of tweezijdig?** **Tweezijdig.** De controleur wil weten of er een "afwijking" is (dikker óf dunner, geen specifieke richting).

**3. Uitvoering:**

```python
# Methode 1: De kant-en-klare scipy functie
t_stat_1samp, p_val_1samp = stats.ttest_1samp(df_productie['Dikte_mm'], popmean=10.00)
print(f"Tweezijdige p-waarde (methode 1): {p_val_1samp:.4f}")

# Methode 2: Handmatig (zoals vaak in de cursus)
m = df_productie['Dikte_mm'].mean()
s = df_productie['Dikte_mm'].std()  # Steekproef standaardafwijking
n = len(df_productie)
mu0 = 10.00

se = s / np.sqrt(n)
# Tweezijdige kans is 2 * de kleinste staart
p_val_hand = min(stats.t.cdf(m, df=n-1, loc=mu0, scale=se),
                 stats.t.sf(m, df=n-1, loc=mu0, scale=se)) * 2

print(f"Tweezijdige p-waarde (methode 2): {p_val_hand:.4f}")
```

---

## 📋 Samenvattende Cheat Sheet — Toetskeuze

| Situatie | Toets |
|---|---|
| Eén numerieke kolom vs een vast getal, **σ gekend** | Z-toets |
| Eén numerieke kolom vs een vast getal, **σ onbekend** | 1-sample T-toets |
| Twee numerieke kolommen over **dezelfde personen** (Voor/Na) | Gepaarde T-toets |
| Eén numerieke kolom, gesplitst in **2 onafhankelijke groepen** | Onafhankelijke T-toets |
| Eén numerieke kolom, gesplitst in **3+ onafhankelijke groepen** | ANOVA |
| Eén categorische kolom vs verwachte %'s | Chi-kwadraat Aanpassingstoets |
| Twee categorische kolommen (beïnvloeden ze elkaar?) | Chi-kwadraat Onafhankelijkheidstoets (crosstab) |
