## Lab 4.01

Opgave 1: Age vs PreferenceOpgave: Onderzoek met een chi-kwadraat ($\chi^2$) onafhankelijkheidstoets of de drankvoorkeur (Preference) onafhankelijk is van de leeftijdscategorie (Age) van de consumenten.Oplossing (Python Code)Python# 1. Kruistabel (contingency table) maken
cross_age = pd.crosstab(softdrinks["Age"], softdrinks["Preference"])
print("Kruistabel Age vs Preference:")
print(cross_age)
print("-" * 50)

# 2. Chi-kwadraat onafhankelijkheidstoets uitvoeren
chi2_age, p_age, dof_age, expected_age = stats.chi2_contingency(cross_age)

print(f"Chi-kwadraat statistiek (χ²): {chi2_age:.4f}")
print(f"p-waarde: {p_age:.4f}")
print(f"Vrijheidsgraden: {dof_age}")
UitlegNulhypothese ($H_0$): Drankvoorkeur en leeftijd zijn onafhankelijk van elkaar.Alternatieve hypothese ($H_1$): Drankvoorkeur is afhankelijk van de leeftijd.Interpretatie: De berekende p-waarde is 0.2771. Omdat deze waarde ruim groter is dan het standaard significantieniveau ($\alpha = 0.05$), kunnen we $H_0$ niet verwerpen. Er is dus geen statistisch significant bewijs dat drankvoorkeur verschilt per leeftijdscategorie.Opgave 2: Gender vs PreferenceOpgave: Onderzoek met een chi-kwadraat ($\chi^2$) onafhankelijkheidstoets of de drankvoorkeur (Preference) onafhankelijk is van het geslacht (Gender) van de consumenten.Let op: Omdat dit een $2 \times 2$-tabel is, past scipy standaard de Yates-continuïteitscorrectie toe. Om exact de resultaten uit de opgave te krijgen, zetten we correction=False.Oplossing (Python Code)Python# 1. Kruistabel (contingency table) maken
cross_gender = pd.crosstab(softdrinks["Gender"], softdrinks["Preference"])
print("Kruistabel Gender vs Preference:")
print(cross_gender)
print("-" * 50)

# 2. Chi-kwadraat onafhankelijkheidstoets uitvoeren (zonder Yates-correctie)
chi2_gender, p_gender, dof_gender, expected_gender = stats.chi2_contingency(
    cross_gender, correction=False
)

print(f"Chi-kwadraat statistiek (χ²): {chi2_gender:.4f}")
print(f"p-waarde: {p_gender:.4f}")
print(f"Vrijheidsgraden: {dof_gender}")
UitlegNulhypothese ($H_0$): Drankvoorkeur en geslacht zijn onafhankelijk van elkaar.Alternatieve hypothese ($H_1$): Drankvoorkeur is afhankelijk van het geslacht.Interpretatie: De berekende p-waarde is 0.2354 (of $0.1834$ met Yates-correctie). In beide gevallen is de p-waarde groter dan $\alpha = 0.05$. We verwerpen $H_0$ niet. Dit betekent dat we er (statistisch gezien) van uit mogen gaan dat mannen en vrouwen geen verschillende voorkeur hebben voor het eigen merk of dat van de concurrent.

OpgaveOnderzoek op basis van het bestand NBA.csv (met de jaarsalarissen van alle NBA-spelers uit het seizoen 2008–2009) of er een verband is (onafhankelijkheid) tussen de positie van een speler en zijn salaris.Hiervoor moeten de volgende databewerkingen worden uitgevoerd:Positie opschonen: Pas samengestelde, afgestreepte posities (zoals C-F) aan naar de eerst genoemde positie (C). Dit is de primaire positie van de speler.Salaris opschonen: Verwijder het dollarteken ($) en de punten (.) uit de kolom Annual Salary en zet deze om naar een numerieke waarde.Categoriseren van salaris: Maak van het salaris een categorische variabele met vier categorieën gebaseerd op de kwartielen:Categorie 1: Alles onder het 1e kwartiel ($Q_1$).Categorie 2: Vanaf het 1e kwartiel tot de mediaan ($Q_2$).Categorie 3: Vanaf de mediaan tot het 3e kwartiel ($Q_3$).Categorie 4: Alles boven het 3e kwartiel.Visualisatie & Toetsing: Maak een visualisatie (mozaïekdiagram) en voer een $\chi^2$-onafhankelijkheidstoets uit om de hypothesen te testen.Antwoord & CodeStap 1: Data inladen en de fout herstellenIn cel 4 van je notebook zie je een AttributeError. De foutmelding "Can only use .str accessor with string values, not integer" betekent dat Python de kolom Annual Salary stiekem al als getal heeft ingelezen (waarschijnlijk omdat de data elders al bewerkt was of automatisch herkend werd), waardoor .str niet meer werkt.Om dit robuust op te lossen, dwingen we de kolom eerst naar een string (.astype(str)) vóórdat we de tekens vervangen.Pythonimport numpy as np
import scipy.stats as stats
import pandas as pd
import matplotlib.pyplot as plt
from statsmodels.graphics.mosaicplot import mosaic
import seaborn as sns
## Labo 4.02
# Data inlezen
nba = pd.read_csv('https://raw.githubusercontent.com/HoGentTIN/dsai-labs/main/data/NBA.csv', sep=";")

# 1. Salaris opschonen (veilig omzetten naar string en dan tekens vervangen)
nba["Annual Salary"] = (
    nba["Annual Salary"]
    .astype(str)
    .str.replace("$", "", regex=False)
    .str.replace(".", "", regex=False)
)
nba["Annual Salary"] = nba["Annual Salary"].astype(int)
Uitleg: Door .astype(str) toe te voegen, voorkom je de crash. Vervolgens haalt .str.replace de symbolen weg, en .astype(int) zorgt ervoor dat we er hiermee wiskundige berekeningen (zoals kwartielen bepalen) op kunnen loslaten.Stap 2: Posities opschonenWe splitsen de posities op het koppelteken (-) en houden alleen het eerste deel over.Python# 2. Positie aanpassen (bijv. C-F wordt C)
nba["Position"] = nba["Position"].str.split("-").str[0]
Uitleg: De methode .str.split("-") knipt een waarde zoals C-F op in een lijstje: ['C', 'F']. Met .str[0] selecteren we telkens het allereerste element uit dat lijstje (in dit geval C).Stap 3: Salaris opdelen in 4 categorieën (Kwartielen)We gebruiken pd.qcut om de numerieke salarissen automatisch in 4 even grote groepen (kwartielen) te verdelen.Python# 3. Kwartielen berekenen en categorisch maken
nba["Salary_Class"] = pd.qcut(nba["Annual Salary"], q=4, labels=["Q1", "Q2", "Q3", "Q4"])
Uitleg: pd.qcut staat voor quantile-based discretization. De parameter q=4 zorgt ervoor dat de data netjes verdeeld wordt in vier groepen die elk 25% van de spelers bevatten. De labels geven we mee om de groepen overzichtelijk te benoemen.Stap 4: Visualisatie (Mozaïekdiagram)Om de verdeling visueel te inspecteren maken we een mozaïekdiagram.Python# 4. Plot maken
plt.figure(figsize=(10,6))
mosaic(nba, ["Position", "Salary_Class"], title="Mozaïekdiagram Positie vs Salaris")
plt.show()
Uitleg: Een mozaïekdiagram laat de kruistabel visueel zien. De breedte van de kolommen toont hoeveel spelers er in een bepaalde positie spelen, en de hoogte van de vakken toont de verdeling van de salarisklassen binnen die positie. Als alle kolommen er min of meer hetzelfde uitzien, wijst dit op onafhankelijkheid.Stap 5: De $\chi^2$-onafhankelijkheidstoets (Chi-kwadraat)Nu voeren we de daadwerkelijke statistische toets uit. Eerst maken we een kruistabel (contingency table) en die stoppen we in de toets.Python# Contingency table (kruistabel) maken
kruistabel = pd.crosstab(nba["Position"], nba["Salary_Class"])

# Chi-kwadraat toets uitvoeren
chi2, p_value, dof, expected = stats.chi2_contingency(kruistabel)

print(f"χ²-waarde: {chi2:.4f}")
print(f"p-waarde: {p_value:.4f}")
Extra uitleg bij de resultatenDe uitkomst van de berekening geeft (zoals ook al in de opgave aangegeven):$\chi^2$ (Chi-kwadraat) $\approx$ 3.0344$p$-waarde = 0.8045Wat betekent dit nu concreet?De Hypotheses:$H_0$ (Nulhypothese): Er is geen verband tussen de positie van een speler en zijn salarisklasse (ze zijn onafhankelijk).$H_1$ (Alternatieve hypothese): Er is wel een verband tussen de positie en het salaris (ze zijn afhankelijk).De p-waarde interpreteren:De $p$-waarde is ontzettend hoog ($0.8045$ oftewel $80,45\%$). Dit is ruim groter dan het standaard significantieniveau van $\alpha = 0.05$ ($5\%$).Een hoge $p$-waarde betekent dat de minieme verschillen die we in de steekproef zien, puur op toeval berusten. Er is dus absoluut geen reden om aan te nemen dat de positie invloed heeft op hoeveel een speler verdient.Conclusie:We verwerpen $H_0$ niet. We concluderen dat de positie waarin een NBA-speler speelt en de hoogte van zijn salaris in het seizoen 2008-2009 onafhankelijk van elkaar zijn. Dus of je nu een Guard, Forward of Center bent, je kansen op een top- of bodemsalaris zijn statistisch gezien gelijk.

## Lab 4.03 
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
antwoord: 
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
Statistische InterpretatieToetsing: Omdat de berekende $\chi^2$ ($3.0027$) kleiner is dan de kritieke waarde ($3.8415$) én de $p$-waarde ($0.0831$) groter is dan het significantieniveau $\alpha = 0.05$, kunnen we $H_0$ niet verwerpen. Er is net niet genoeg statistisch bewijs om harde discriminatie of significante ondervertegenwoordiging aan te tonen op basis van deze steekproef alleen.Residu: Het gestandaardiseerde residu voor de Afro-Amerikaanse leraren is $-1.7328$. Omdat deze waarde groter is dan $-2$ (dus minder extreem dan 2 standaardafwijkingen onder het verwachte getal), valt de afwijking binnen wat we nog aan "toevallige variatie" kunnen toeschrijven.

## Lab 4.04
A popular wisdom states that more children are born during certain phases of the lunar cycle, especially during the full moon. A classification of the number of births according to the lunar cycle was done in 2005.
A sample of the number of births during different lunar cycles is given below.  
Is there a relationship between the lunar phase and the number of births?
dfmoon = pd.DataFrame(data={'lunar_phase': ['new moon', 'young crescent', 'first quarter', 'waxing moon', 'full moon', 'waning moon', 'last quarter', 'ashen moon'],
                            'number_of_days': [24, 152, 24, 149, 24, 150, 24, 152],
                            'number_of_births': [7680, 48442, 7579, 47814, 7711, 47595, 7733, 48230]})

dfmoon.head(10)

**Answers:**

- chi-square statistic $\chi^2 \approx$ 0.1129
- $p \approx$ 0.999996485

Antwoord:
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
Stapsgewijze Uitleg1. Waarom corrigeren voor het aantal dagen?De verschillende fasen van de maanfase duren in de dataset niet allemaal even lang (sommige duren 24 dagen, andere rond de 150 dagen). Het is logisch dat er in een fase van 152 dagen meer kinderen worden geboren dan in een fase van 24 dagen. Daarom moeten we eerst kijken naar het gemiddeld aantal geboorten per dag binnen elke fase:$$\text{Geboorten per dag} = \frac{\text{number\_of\_births}}{\text{number\_of\_days}}$$2. De Hypothesen opstellen$H_0$ (Nulhypothese): Er is geen verband tussen de maanfase en het aantal geboorten. Het gemiddeld aantal geboorten per dag is in elke fase gelijk.$H_1$ (Alternatieve hypothese): Er is wel een verband; in bepaalde maanfasen (zoals bij volle maan) worden er gemiddeld per dag meer of minder kinderen geboren.3. De Verwachte Waarde bepalenOnder de nulhypothese ($H_0$) verwachten we dat het aantal geboorten per dag voor elke fase precies gelijk is aan het totale gemiddelde van alle fasen. Dit gemiddelde is $\approx 319.19$ geboorten per dag. Dit getal dient als de verwachte waarde ($E$) voor alle 8 de categorieën.4. De Chi-kwadraat aanpassingstoetsMet de functie stats.chisquare() vergelijken we de geobserveerde geboorten per dag met de verwachte geboorten per dag. De formule die op de achtergrond wordt uitgevoerd is:$$\chi^2 = \sum \frac{(O - E)^2}{E}$$Dit levert een extreem lage $\chi^2$-waarde op van $0.1129$.Statistische Interpretatie en ConclusieDe $p$-waarde: De berekende $p$-waarde is $0.999996485$ (nagenoeg $1$ of $100\%$).Conclusie: Omdat de $p$-waarde vele malen groter is dan het standaard significantieniveau ($\alpha = 0.05$), kunnen we $H_0$ absoluut niet verwerpen.Een $p$-waarde die zo dicht bij de $1$ ligt, betekent dat de geobserveerde dagelijkse geboortecijfers in de praktijk nagenoeg perfect overeenkomen met de theoretisch verwachte waarden. Er is dus totaal geen statistisch bewijs voor de volkswijsheid dat er tijdens bepaalde maanfasen (zoals volle maan) meer kinderen geboren worden. De maanfase en het aantal geboorten zijn volledig onafhankelijk van elkaar.

## Lab 4.05
1. Data inlezen en voorbereidenOpgaveLaad het bestand data/survey.csv. Let op met ontbrekende waarden (NA): zorg ervoor dat de waarde 'None' in de kolom Exer niet per ongeluk als een ontbrekende waarde wordt gezien. Zet daarna Exer en Smoke om naar ordinale variabelen met een logische volgorde.Oplossing (Code)Python# Inlezen van de data met specifieke NA-instellingen
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
VerklaringStandaard ziet pandas de string 'None' als een synoniem voor NaN (ontbrekende data). Omdat studenten bij de vraag naar sporten letterlijk "Geen" (None) kunnen antwoorden, moeten we keep_default_na=False gebruiken en expliciet meegeven dat alleen de letterlijke tekst 'NA' een ontbrekende waarde is. Door de kolommen om te zetten naar een geordende CategoricalDtype staan de categorieën straks in een logische volgorde in onze tabellen en grafieken (bijv. van 'Nooit' naar 'Vaak' roken).2. Functie voor Cramér's VOpgaveOm voor elke combinatie de effectgrootte (Cramér's V) te berekenen, schrijven we eerst een herbruikbare Python-functie.Oplossing (Code)Pythondef cramers_v(contingency_table):
    chi2 = stats.chi2_contingency(contingency_table)[0]
    n = contingency_table.sum().sum()
    r, k = contingency_table.shape
    return np.sqrt(chi2 / (n * min(r - 1, k - 1)))
3. Analyse van de VariabelenparenVoor elk paar voeren we telkens dezelfde stappen uit: een kruistabel maken, de $\chi^2$-onafhankelijkheidstoets uitvoeren en Cramér's V berekenen. De betrouwbaarheid is telkens $\alpha = 0.05$.Paar 1: Exer (Sporten) vs. Smoke (Roken)Oplossing (Code)Python# 1. Kruistabel (onafhankelijke variabele eerst = rijen)
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
Verklaring & ConclusieResultaat: $\chi^2 \approx 5.489$, $g \approx 12.592$, $p \approx 0.483$, Cramér's $V \approx 0.089$Analyse: De berekende $\chi^2$-waarde ($5.489$) is veel kleiner dan de kritieke g-grens ($12.592$). Bovendien is de $p$-waarde ($0.483$) veel groter dan $\alpha = 0.05$.Conclusie: We aanvaarden de nulhypothese ($H_0$). Dit betekent dat er geen statistisch significant verband is tussen hoe vaak een student sport en hoe vaak deze rookt. Cramér's V ligt dicht bij 0, wat duidt op een verwaarloosbaar tot zeer zwak effect.Paar 2: W.Hnd (Dominante hand) vs. Fold (Armen kruisen)Oplossing (Code)Pythonw_fold_table = pd.crosstab(survey['W.Hnd'], survey['Fold'])
print("\n--- Frequentietabel W.Hnd/Fold ---")
print(w_fold_table)

chi2, p, dof, expected = stats.chi2_contingency(w_fold_table)
g = stats.chi2.ppf(1 - 0.05, dof)
v = cramers_v(w_fold_table)

print(f"Chi-kwadraat (χ²): {chi2:.3f}")
print(f"Kritieke g-waarde: {g:.3f}")
print(f"p-waarde:          {p:.3f}")
print(f"Cramér's V:        {v:.3f}")
Verklaring & ConclusieResultaat: $\chi^2 \approx 1.581$, $g \approx 5.992$, $p \approx 0.454$, Cramér's $V \approx 0.083$Analyse: De $p$-waarde ($0.454 > 0.05$) toont aan dat het resultaat niet statistisch significant is.Conclusie: We aanvaarden de nulhypothese ($H_0$). Welke hand dominant is (links of rechts), heeft geen invloed op welke arm bovenop ligt als een student de armen kruist. De twee variabelen zijn onafhankelijk.Paar 3: Sex (Geslacht) vs. Smoke (Roken)Oplossing (Code)Pythonsex_smoke_table = pd.crosstab(survey['Sex'], survey['Smoke'])
print("\n--- Frequentietabel Sex/Smoke ---")
print(sex_smoke_table)

chi2, p, dof, expected = stats.chi2_contingency(sex_smoke_table)
g = stats.chi2.ppf(1 - 0.05, dof)
v = cramers_v(sex_smoke_table)

print(f"Chi-kwadraat (χ²): {chi2:.3f}")
print(f"Kritieke g-waarde: {g:.3f}")
print(f"p-waarde:          {p:.3f}")
print(f"Cramér's V:        {v:.3f}")
Verklaring & ConclusieResultaat: $\chi^2 \approx 3.554$, $g \approx 7.815$, $p \approx 0.314$, Cramér's $V \approx 0.123$Analyse: Wederom ligt de $\chi^2$-waarde onder de kritieke g-grens en is de $p$-waarde groter dan $0.05$.Conclusie: We aanvaarden de nulhypothese ($H_0$). Er is geen significant verband tussen het geslacht van de student en het rookgedrag.Paar 4: Sex (Geslacht) vs. W.Hnd (Dominante hand)Oplossing (Code)Pythonsex_hand_table = pd.crosstab(survey['Sex'], survey['W.Hnd'])
print("\n--- Frequentietabel Sex/W.Hnd ---")
print(sex_hand_table)

chi2, p, dof, expected = stats.chi2_contingency(sex_hand_table)
g = stats.chi2.ppf(1 - 0.05, dof)
v = cramers_v(sex_hand_table)

print(f"Chi-kwadraat (χ²): {chi2:.3f}")
print(f"Kritieke g-waarde: {g:.3f}")
print(f"p-waarde:          {p:.3f}")
print(f"Cramér's V:        {v:.3f}")
Verklaring & ConclusieResultaat: $\chi^2 \approx 0.236$, $g \approx 3.842$, $p \approx 0.627$, Cramér's $V \approx 0.032$Analyse: De extreem lage $\chi^2$-waarde en de zeer hoge $p$-waarde ($0.627$) geven aan dat de geobserveerde tabel nagenoeg perfect overeenkomt met wat je bij onafhankelijkheid mag verwachten.Conclusie: We aanvaarden de nulhypothese ($H_0$). Of een student man of vrouw is, staat volledig los van het feit of ze links- of rechtshandig zijn. Cramér's V is nagenoeg $0$, wat duidt op de totale afwezigheid van een effect binnen deze steekproef.

## Lab 4.06
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

