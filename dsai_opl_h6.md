# Hoofdstuk 6 - Regressieanalyse

## Lab 6.01

### Exercise 1: Relation between heart weight and body weight in Cats

#### Part 1: All cats

##### Voorbereiding: Package imports

```python
import numpy as np                                  # Scientific computing
import scipy.stats as stats                         # Statistical tests
import pandas as pd                                 # Dataframe
import matplotlib.pyplot as plt                     # Basic visualisation
from statsmodels.graphics.mosaicplot import mosaic  # Mosaic plot
import seaborn as sns                               # Advanced dataviz
from sklearn.linear_model import LinearRegression
```

##### Stap 1: Dataset importeren

Load het `Cats.csv` bestand en bekijk de eerste records:

```python
cats = pd.read_csv('https://raw.githubusercontent.com/HoGentTIN/dsai-labs/main/data/Cats.csv')
cats.head()
```

Dit geeft:

| ID | Sex | Hwt | Bwt |
|:---|:---|:---|:---|
| 1 | F | 2.0 | 7.0 |
| 2 | F | 2.0 | 7.4 |
| 3 | F | 2.0 | 9.5 |
| 4 | F | 2.1 | 7.2 |
| 5 | F | 2.1 | 7.3 |

Perform a linear regression analysis on the variables:
- **Afhankelijke variabele**: Body weight (`Bwt`)
- **Onafhankelijke variabele**: Heart weight (`Hwt`)

##### Stap 2: Scatterplot tekenen

Teken een scatterplot van beide variabelen:

```python
plt.figure(figsize=(8, 6))
plt.scatter(cats['Hwt'], cats['Bwt'], alpha=0.6, edgecolors='k')
plt.xlabel('Heart Weight (g)')
plt.ylabel('Body Weight (kg)')
plt.title('Relationship between Heart Weight and Body Weight in Cats')
plt.grid(True, alpha=0.3)
plt.show()
```

##### Stap 3: Regressielijn berekenen en tekenen

```python
# Voorbereiding: X en y definiëren
X = cats[['Hwt']].values
y = cats['Bwt'].values

# Linear regression model
model = LinearRegression()
model.fit(X, y)

# Parameters
beta_0 = model.intercept_
beta_1 = model.coef_[0]

# Voorspellingen voor de regressielijn
X_line = np.array([[cats['Hwt'].min()], [cats['Hwt'].max()]])
y_line = model.predict(X_line)

# Visualisatie
plt.figure(figsize=(8, 6))
plt.scatter(cats['Hwt'], cats['Bwt'], alpha=0.6, edgecolors='k', label='Data points')
plt.plot(X_line, y_line, color='red', linewidth=2, label='Regression line')
plt.xlabel('Heart Weight (g)')
plt.ylabel('Body Weight (kg)')
plt.title('Linear Regression: Body Weight vs Heart Weight')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()

print(f"Regression line: ŷ = {beta_0} + {beta_1} x")
```

##### Stap 4: Correlatiecoëfficiënt en determinatiecoëfficiënt

```python
# Pearson correlation coefficient
correlation = np.corrcoef(cats['Hwt'], cats['Bwt'])[0, 1]

# Coefficient of determination (R²)
r_squared = correlation ** 2

print(f"Correlation coefficient (R): {correlation}")
print(f"Coefficient of determination (R²): {r_squared}")
```

##### Stap 5: Interpretatie van resultaten

De lineaire regressieanalyse toont aan dat er een sterke positieve correlatie bestaat tussen hartgewicht en lichaamsgewicht van katten. De regressielijn kan worden gebruikt voor voorspellingen.

---

#### Part 2: Regressieanalyse per geslacht

##### Stap 1: Dataset inladen

```python
cats = pd.read_csv('https://raw.githubusercontent.com/HoGentTIN/dsai-labs/main/data/Cats.csv')
cats.head()
```

##### Stap 2: Scatterplot per geslacht

```python
plt.figure(figsize=(10, 6))

# Plot voor males
males = cats[cats['Sex'] == 'M']
females = cats[cats['Sex'] == 'F']

plt.scatter(males['Hwt'], males['Bwt'], alpha=0.6, label='Male', edgecolors='k')
plt.scatter(females['Hwt'], females['Bwt'], alpha=0.6, label='Female', edgecolors='k')

plt.xlabel('Heart Weight (g)')
plt.ylabel('Body Weight (kg)')
plt.title('Heart Weight vs Body Weight by Gender')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

##### Stap 3: Regressielijn per geslacht berekenen en tekenen

```python
# Function to fit regression for a group
def fit_regression(data, group_name):
    X = data[['Hwt']].values
    y = data['Bwt'].values
    
    model = LinearRegression()
    model.fit(X, y)
    
    beta_0 = model.intercept_
    beta_1 = model.coef_[0]
    
    correlation = np.corrcoef(data['Hwt'], data['Bwt'])[0, 1]
    r_squared = correlation ** 2
    
    print(f"\n{group_name}:")
    print(f"Regression line: ŷ = {beta_0} + {beta_1} x")
    print(f"Correlation (R): {correlation}")
    print(f"R²: {r_squared}")
    
    return model, X, y, beta_0, beta_1, correlation, r_squared

# Fit regression for males and females
male_model, male_X, male_y, male_b0, male_b1, male_r, male_r2 = fit_regression(males, "Male")
female_model, female_X, female_y, female_b0, female_b1, female_r, female_r2 = fit_regression(females, "Female")

# Visualisatie met regressielijnen
plt.figure(figsize=(12, 6))

# Males
plt.scatter(males['Hwt'], males['Bwt'], alpha=0.6, label='Male', edgecolors='k')
X_line_m = np.array([[males['Hwt'].min()], [males['Hwt'].max()]])
y_line_m = male_model.predict(X_line_m)
plt.plot(X_line_m, y_line_m, color='blue', linewidth=2)

# Females
plt.scatter(females['Hwt'], females['Bwt'], alpha=0.6, label='Female', edgecolors='k')
X_line_f = np.array([[females['Hwt'].min()], [females['Hwt'].max()]])
y_line_f = female_model.predict(X_line_f)
plt.plot(X_line_f, y_line_f, color='orange', linewidth=2)

plt.xlabel('Heart Weight (g)')
plt.ylabel('Body Weight (kg)')
plt.title('Linear Regression by Gender')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

##### Stap 4: Correlatiecoëfficiënt en determinatiecoëfficiënt per geslacht

De bovenstaande functie berekent al deze waarden.

##### Stap 5: Interpretatie

De regressieanalyse per geslacht toont verschillende relaties: mannelijke katten vertonen een sterkere lineaire relatie dan vrouwelijke katten.

##### Antwoordentabel

| Selection | $\beta_0$ | $\beta_1$ | $R$ | $R^2$ |
| :--- | ---: | ---: | ---: | ---: |
| All | -0.3510784 | 4.0317575 | 0.8041348 | 0.6466328 |
| Male | -1.1768253 | 4.3098189 | 0.7930443 | 0.6289193 |
| Female | 2.9813124 | 2.636414 | 0.5320497 | 0.2830768 |

---

## Lab 6.02

### Exercise 2: Flemish agricultural and horticultural businesses

#### Voorbereiding: Package imports

```python
import numpy as np
import scipy.stats as stats
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.linear_model import LinearRegression
```

#### Stap 1: Dataset inladen

Laad het bestand `agriculture flanders.csv`. Dit bestand bevat gegevens over landbouw- en tuinbouwbedrijven in Vlaanderen:

```python
farms = pd.read_csv(
    'https://raw.githubusercontent.com/HoGentTIN/dsai-labs/main/data/agriculture%20flanders.csv',
    delimiter=";",
    decimal=','
)
farms.head()
```

#### Stap 2: Gegevenstypen controleren

```python
print(farms.dtypes)
print(farms.info())
```

#### Stap 3: Scatterplot aantal bedrijven versus jaar

```python
plt.figure(figsize=(10, 6))
plt.scatter(farms['year'], farms['number_of_farms'], alpha=0.6, edgecolors='k')
plt.xlabel('Year')
plt.ylabel('Number of Farms')
plt.title('Number of Farms Over Time')
plt.grid(True, alpha=0.3)
plt.show()
```

#### Stap 4: Positieve of negatieve relatie?

Is er een positieve/negatieve relatie tussen jaar en aantal_bedrijven?

$$R = -0.9861066349492859$$

Dit geeft aan dat er een sterke **negatieve** correlatie is: het aantal bedrijven daalt aanzienlijk over de jaren heen.

```python
correlation_farms = np.corrcoef(farms['year'], farms['number_of_farms'])[0, 1]
print(f"Correlation: {correlation_farms}")
```

#### Stap 5: Sterke relatie?

Is er een sterke relatie tussen jaar en 'number_of_farms'?

$$R^2 = 0.9724062954910041$$

Ja, $R^2 \approx 0.972$ geeft aan dat ongeveer 97.24% van de variantie in het aantal bedrijven kan worden verklaard door de tijd (jaar).

```python
r_squared_farms = correlation_farms ** 2
print(f"R²: {r_squared_farms}")
```

#### Stap 6: Scatterplot gemiddelde oppervlakte per bedrijf versus jaar

```python
plt.figure(figsize=(10, 6))
plt.scatter(farms['year'], farms['average_area_per_farm_(ha)'], alpha=0.6, edgecolors='k')
plt.xlabel('Year')
plt.ylabel('Average Area per Farm (ha)')
plt.title('Average Farm Area Over Time')
plt.grid(True, alpha=0.3)
plt.show()
```

#### Stap 7: Voorspelling gemiddelde oppervlakte voor 2035

Wat zal de gemiddelde oppervlakte per bedrijf in 2035 zijn?

**Antwoord**: Average area in 2035 = 34.91987804878045 ha

```python
# Fit regression model
X = farms[['year']].values
y = farms['average_area_per_farm_(ha)'].values

model = LinearRegression()
model.fit(X, y)

# Prediction for 2035
year_2035 = np.array([[2035]])
predicted_area_2035 = model.predict(year_2035)[0]

print(f"Predicted average area in 2035: {predicted_area_2035} ha")
```

#### Stap 8: Totale landbouwoppervlakte per jaar

Bereken voor elk jaar de totale oppervlakte. Bereken voor elk jaar de verandering van de totale oppervlakte ten opzichte van 1980. Maak een grafiek:

```python
# Calculate total area
farms['total_area'] = farms['number_of_farms'] * farms['average_area_per_farm_(ha)']

# Calculate change relative to 1980
area_1980 = farms[farms['year'] == 1980]['total_area'].values[0]
farms['change_relative_to_1980'] = farms['total_area'] - area_1980

# Plot
plt.figure(figsize=(10, 6))
plt.plot(farms['year'], farms['change_relative_to_1980'], marker='o', linewidth=2, markersize=6)
plt.axhline(y=0, color='r', linestyle='--', alpha=0.5)
plt.xlabel('Year')
plt.ylabel('Change in Total Area (ha) relative to 1980')
plt.title('Change in Total Agricultural Area Over Time')
plt.grid(True, alpha=0.3)
plt.show()
```

---

## Lab 6.03

### Exercise 3: Movies of 2006 and 2007

#### Voorbereiding: Package imports

```python
import numpy as np
import scipy.stats as stats
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.linear_model import LinearRegression
```

#### Stap 1: Dataset inladen

Het bestand `Movies_2006_2007.csv` bevat informatie over meer dan 200 films die in 2006 en 2007 werden uitgebracht:

```python
movies = pd.read_csv(
    'https://raw.githubusercontent.com/HoGentTIN/dsai-labs/main/data/Movies_2006_2007.csv',
    delimiter=";",
    encoding='cp1252'
)
movies.head()
```

#### Stap 2: Dollars omzetten naar miljoen dollars

```python
cols_with_dollars = [
    '7-day Gross',
    '14-day Gross',
    'Total US Gross',
    'International Gross',
    'US DVD Sales',
    'Budget'
]

# Function to convert dollar strings to floats
def dollar_to_float(dollar_str):
    if isinstance(dollar_str, str):
        # Remove $ and spaces, convert to float
        value = float(dollar_str.replace('$', '').replace(' ', '').replace(',', ''))
        return value / 1_000_000  # Convert to millions
    return dollar_str

# Apply to all columns with dollars
for col in cols_with_dollars:
    movies[col] = movies[col].apply(dollar_to_float)

movies.head()
```

#### Stap 3: Scatterplots

Maak twee scatterplots:
1. Total US Gross (Y) versus 7-day Gross (X)
2. Total US Gross (Y) versus 14-day Gross (X)

```python
fig, axes = plt.subplots(1, 2, figsize=(14, 5))

# Plot 1: 7-day vs Total US Gross
axes[0].scatter(movies['7-day Gross'], movies['Total US Gross'], alpha=0.6, edgecolors='k')
axes[0].set_xlabel('7-day Gross ($ millions)')
axes[0].set_ylabel('Total US Gross ($ millions)')
axes[0].set_title('Total US Gross vs 7-day Gross')
axes[0].grid(True, alpha=0.3)

# Plot 2: 14-day vs Total US Gross
axes[1].scatter(movies['14-day Gross'], movies['Total US Gross'], alpha=0.6, edgecolors='k')
axes[1].set_xlabel('14-day Gross ($ millions)')
axes[1].set_ylabel('Total US Gross ($ millions)')
axes[1].set_title('Total US Gross vs 14-day Gross')
axes[1].grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

#### Stap 4: Correlatiecoëfficiënten en determinatiecoëfficiënten

```python
# Calculate correlations and R²
corr_7day = np.corrcoef(movies['7-day Gross'].dropna(), 
                        movies.loc[movies['7-day Gross'].notna(), 'Total US Gross'])[0, 1]
r2_7day = corr_7day ** 2

corr_14day = np.corrcoef(movies['14-day Gross'].dropna(), 
                         movies.loc[movies['14-day Gross'].notna(), 'Total US Gross'])[0, 1]
r2_14day = corr_14day ** 2

print(f"R (7 Day Gross vs Total US Gross) = {corr_7day}")
print(f"R² (7 Day Gross vs Total US Gross) = {r2_7day}")
print(f"\nR (14 Day Gross vs Total US Gross) = {corr_14day}")
print(f"R² (14 Day Gross vs Total US Gross) = {r2_14day}")
```

**Antwoorden**:
- R (7 Day Gross vs Total US Gross) = 0.959407544414305
- R² (7 Day Gross vs Total US Gross) = 0.9204628362790865
- R (14 Day Gross vs Total US Gross) = 0.9681876849309906
- R² (14 Day Gross vs Total US Gross) = 0.9373471050769301

#### Stap 5: Regressielijnen en interpretatie

```python
# Fit regression models
X_7day = movies[['7-day Gross']].dropna()
y_7day = movies.loc[X_7day.index, 'Total US Gross']

X_14day = movies[['14-day Gross']].dropna()
y_14day = movies.loc[X_14day.index, 'Total US Gross']

model_7day = LinearRegression()
model_7day.fit(X_7day, y_7day)

model_14day = LinearRegression()
model_14day.fit(X_14day, y_14day)

print(f"7 days gross: ŷ = {model_7day.intercept_} + {model_7day.coef_[0]} * x")
print(f"14 days gross: ŷ = {model_14day.intercept_} + {model_14day.coef_[0]} * x")

# Visualize regression lines
fig, axes = plt.subplots(1, 2, figsize=(14, 5))

# Plot 1: 7-day with regression line
axes[0].scatter(movies['7-day Gross'], movies['Total US Gross'], alpha=0.6, edgecolors='k')
X_line_7 = np.array([[movies['7-day Gross'].min()], [movies['7-day Gross'].max()]])
y_line_7 = model_7day.predict(X_line_7)
axes[0].plot(X_line_7, y_line_7, color='red', linewidth=2)
axes[0].set_xlabel('7-day Gross ($ millions)')
axes[0].set_ylabel('Total US Gross ($ millions)')
axes[0].set_title('7-day Gross vs Total US Gross with Regression Line')
axes[0].grid(True, alpha=0.3)

# Plot 2: 14-day with regression line
axes[1].scatter(movies['14-day Gross'], movies['Total US Gross'], alpha=0.6, edgecolors='k')
X_line_14 = np.array([[movies['14-day Gross'].min()], [movies['14-day Gross'].max()]])
y_line_14 = model_14day.predict(X_line_14)
axes[1].plot(X_line_14, y_line_14, color='red', linewidth=2)
axes[1].set_xlabel('14-day Gross ($ millions)')
axes[1].set_ylabel('Total US Gross ($ millions)')
axes[1].set_title('14-day Gross vs Total US Gross with Regression Line')
axes[1].grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

**Antwoorden**:
- 7 days gross: ŷ = 4.590921233818129 + 2.1134671523944885 * x
- 14 days gross: ŷ = -5.555050127408889 + 2.6306629388976386 * x

Interpretatie: De intercept representeert de voorspelde totale bruto wanneer het initiele bruto nul is. De helling toont hoeveel de totale bruto toeneemt voor elke miljoen dollar stijging in het initiele bruto.

#### Stap 6: Uitbijters identificeren en verwijderen

##### 6.1: Boxplots voor uitbijters

```python
fig, axes = plt.subplots(1, 2, figsize=(12, 5))

axes[0].boxplot(movies['7-day Gross'].dropna())
axes[0].set_ylabel('7-day Gross ($ millions)')
axes[0].set_title('Boxplot: 7-day Gross')

axes[1].boxplot(movies['14-day Gross'].dropna())
axes[1].set_ylabel('14-day Gross ($ millions)')
axes[1].set_title('Boxplot: 14-day Gross')

plt.tight_layout()
plt.show()
```

##### 6.2: Berekenen van limietwaarden voor uitbijters

```python
# Calculate IQR and limits
q1_7day = movies['7-day Gross'].quantile(0.25)
q3_7day = movies['7-day Gross'].quantile(0.75)
iqr_7day = q3_7day - q1_7day
limit_7day = q3_7day + 1.5 * iqr_7day

q1_14day = movies['14-day Gross'].quantile(0.25)
q3_14day = movies['14-day Gross'].quantile(0.75)
iqr_14day = q3_14day - q1_14day
limit_14day = q3_14day + 1.5 * iqr_14day

print(f"limit_7_days_gross is {limit_7day}")
print(f"limit_14_days_gross is {limit_14day}")
```

**Antwoorden**:
- limit_7_days_gross is 50.376637875
- limit_14_days_gross is 96.2906745

##### 6.3: Uitbijters identificeren

```python
outliers_7day = movies[movies['7-day Gross'] > limit_7day]
outliers_14day = movies[movies['14-day Gross'] > limit_14day]

print("Outliers for 7-day Gross:")
print(outliers_7day[['Title', '7-day Gross', 'Total US Gross']])

print("\nOutliers for 14-day Gross:")
print(outliers_14day[['Title', '14-day Gross', 'Total US Gross']])
```

##### 6.4: Uitbijters verwijderen

```python
# Remove outliers
movies_clean = movies[
    (movies['7-day Gross'] <= limit_7day) & 
    (movies['14-day Gross'] <= limit_14day)
].copy()

print(f"Original dataset: {len(movies)} rows")
print(f"After removing outliers: {len(movies_clean)} rows")
print(f"Removed: {len(movies) - len(movies_clean)} rows")
```

##### 6.5: Nieuwe waarden voor R en R²

```python
# Recalculate correlations without outliers
corr_7day_clean = np.corrcoef(
    movies_clean['7-day Gross'].dropna(),
    movies_clean.loc[movies_clean['7-day Gross'].notna(), 'Total US Gross']
)[0, 1]
r2_7day_clean = corr_7day_clean ** 2

corr_14day_clean = np.corrcoef(
    movies_clean['14-day Gross'].dropna(),
    movies_clean.loc[movies_clean['14-day Gross'].notna(), 'Total US Gross']
)[0, 1]
r2_14day_clean = corr_14day_clean ** 2

print(f"R (7 Day Gross vs Total US Gross) = {corr_7day_clean}")
print(f"R² (7 Day Gross vs Total US Gross) = {r2_7day_clean}")
print(f"\nR (14 Day Gross vs Total US Gross) = {corr_14day_clean}")
print(f"R² (14 Day Gross vs Total US Gross) = {r2_14day_clean}")
```

**Antwoorden**:
- R (7 Day Gross vs Total US Gross) = 0.9324792650689281
- R² (7 Day Gross vs Total US Gross) = 0.8695175797834882
- R (14 Day Gross vs Total US Gross) = 0.9533836790706848
- R² (14 Day Gross vs Total US Gross) = 0.9089401261522848

##### 6.6: Nieuwe regressieparameters

```python
# Fit new regression models without outliers
X_7day_clean = movies_clean[['7-day Gross']].dropna()
y_7day_clean = movies_clean.loc[X_7day_clean.index, 'Total US Gross']

X_14day_clean = movies_clean[['14-day Gross']].dropna()
y_14day_clean = movies_clean.loc[X_14day_clean.index, 'Total US Gross']

model_7day_clean = LinearRegression()
model_7day_clean.fit(X_7day_clean, y_7day_clean)

model_14day_clean = LinearRegression()
model_14day_clean.fit(X_14day_clean, y_14day_clean)

print(f"7 days gross: ŷ = {model_7day_clean.intercept_} + {model_7day_clean.coef_[0]} * x")
print(f"14 days gross: ŷ = {model_14day_clean.intercept_} + {model_14day_clean.coef_[0]} * x")
```

**Antwoorden**:
- 7 days gross: ŷ = -5.073945979380596 + 2.7456809688135646 * x
- 14 days gross: ŷ = -10.628313843439857 + 2.953646627239584 * x

**Effect van uitbijters**: De uitbijters hebben een aanzienlijk effect op de regressieparameters. Zonder uitbijters zijn de intercepten negatief en de hellingen groter, wat duidt op dat de initiële uitbijters de relatie hebben vervormd.

---

## Lab 6.04

### Exercise 4: Production Cost Analysis

#### Voorbereiding: Package imports

```python
import numpy as np
import scipy.stats as stats
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.linear_model import LinearRegression
```

#### Stap 1: Dataset inladen

Het bestand `production.csv` bevat de productiekosten per geproduceerde eenheid:

```python
df = pd.read_csv(
    'https://raw.githubusercontent.com/HoGentTIN/dsai-labs/main/data/production.csv',
    delimiter=";"
)
df.head(20)
```

De dataset toont dat het produceren van de 100e eenheid €82 kost, terwijl het produceren van de 600e eenheid €34 kost. Dit illustreert het concept van schaaleffecten: naarmate meer eenheden worden geproduceerd, dalen de gemiddelde kosten per eenheid.

#### Volgende stappen

Voer de volgende analyse uit op deze dataset:

1. Visualiseer de relatie tussen eenheden geproduceerd en productiekosten
2. Bereken de lineaire regressie
3. Bepaal de correlatiecoëfficiënten en R²
4. Interpreteer de resultaten
5. Maak voorspellingen voor toekomstige productiewaarden

```python
# Stap 1: Scatterplot
plt.figure(figsize=(10, 6))
plt.scatter(df['units'], df['cost'], alpha=0.6, edgecolors='k')
plt.xlabel('Units Produced')
plt.ylabel('Cost per Unit (€)')
plt.title('Production Cost per Unit')
plt.grid(True, alpha=0.3)
plt.show()

# Stap 2: Fit regression model
X = df[['units']].values
y = df['cost'].values

model = LinearRegression()
model.fit(X, y)

beta_0 = model.intercept_
beta_1 = model.coef_[0]

# Stap 3: Correlation and R²
correlation = np.corrcoef(df['units'], df['cost'])[0, 1]
r_squared = correlation ** 2

print(f"Regression equation: ŷ = {beta_0} + {beta_1} * x")
print(f"Correlation (R): {correlation}")
print(f"R²: {r_squared}")

# Visualize regression line
plt.figure(figsize=(10, 6))
plt.scatter(df['units'], df['cost'], alpha=0.6, edgecolors='k', label='Data')

X_line = np.array([[df['units'].min()], [df['units'].max()]])
y_line = model.predict(X_line)
plt.plot(X_line, y_line, color='red', linewidth=2, label='Regression line')

plt.xlabel('Units Produced')
plt.ylabel('Cost per Unit (€)')
plt.title('Production Cost with Linear Regression')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

---

## Lab 6.05

### Exercise 5: Advertising budget and sales analysis

#### Overzicht

Dit dataset bevat gegevens over de verkoop van een product in relatie tot de reclamebudgetten besteed aan TV, radio en kranten. Alle waarden zijn in duizenden dollars, behalve verkoop die in duizenden eenheden is. Het dataset bevat 200 rijen en 4 variabelen: TV, Radio, Newspaper, en Sales.

Bron: <https://www.geeksforgeeks.org/machine-learning/dataset-for-linear-regression/>

#### Voorbereiding: Package imports

```python
import numpy as np
import scipy.stats as stats
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.linear_model import LinearRegression
```

#### Stap 1: Dataset inladen en visualiseren

Load het dataset en onderzoek de relatie tussen de drie reclamebudgetten (onafhankelijke variabelen) en verkoop (afhankelijke variabele):

```python
advertising = pd.read_csv(
    'https://media.geeksforgeeks.org/wp-content/uploads/20240522145649/advertising.csv'
)
advertising.head()
```

Visualiseer de dataset met pairplots:

```python
# Pairplot to show relationships
sns.pairplot(advertising, kind='scatter', diag_kind='hist')
plt.tight_layout()
plt.show()

# Or create individual scatterplots
fig, axes = plt.subplots(1, 3, figsize=(15, 4))

axes[0].scatter(advertising['TV'], advertising['Sales'], alpha=0.6, edgecolors='k')
axes[0].set_xlabel('TV Budget')
axes[0].set_ylabel('Sales')
axes[0].set_title('Sales vs TV Budget')
axes[0].grid(True, alpha=0.3)

axes[1].scatter(advertising['Radio'], advertising['Sales'], alpha=0.6, edgecolors='k')
axes[1].set_xlabel('Radio Budget')
axes[1].set_ylabel('Sales')
axes[1].set_title('Sales vs Radio Budget')
axes[1].grid(True, alpha=0.3)

axes[2].scatter(advertising['Newspaper'], advertising['Sales'], alpha=0.6, edgecolors='k')
axes[2].set_xlabel('Newspaper Budget')
axes[2].set_ylabel('Sales')
axes[2].set_title('Sales vs Newspaper Budget')
axes[2].grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

**Observatie**: TV lijkt de sterkste relatie met verkoop te hebben.

#### Stap 2: Correlatiecoëfficiënten en determinatiecoëfficiënten

Bereken de correlatiecoëfficiënten en determinatiecoëfficiënten voor elk reclamebudget versus verkoop:

```python
# Calculate correlations and R² for each variable
channels = ['TV', 'Radio', 'Newspaper']

for channel in channels:
    correlation = np.corrcoef(advertising[channel], advertising['Sales'])[0, 1]
    r_squared = correlation ** 2
    
    print(f"\n{channel}:")
    print(f"  Correlation: {correlation:.4f}")
    print(f"  R²: {r_squared:.4f}")
```

**Antwoorden**:
- TV: R = 0.9012, R² = 0.8122
- Radio: R = 0.3496, R² = 0.1222
- Newspaper: R = 0.1580, R² = 0.0250

**Interpretatie**: TV heeft duidelijk de sterkste relatie met verkoop (R² = 0.8122), terwijl Radio en Newspaper veel zwakkere relaties hebben. Dit suggereert dat het primair lonend is om in TV-reclame te investeren.

#### Stap 3: Eenvoudige lineaire regressie voor het beste kanaal

Gebruik alleen het beste performende reclamekanaal (TV) voor voorspellingen:

```python
# Fit regression model for TV
X_tv = advertising[['TV']].values
y = advertising['Sales'].values

model_tv = LinearRegression()
model_tv.fit(X_tv, y)

beta_0 = model_tv.intercept_
beta_1 = model_tv.coef_[0]

print(f"Regression line: Sales = {beta_0} + {beta_1} * TV")

# Predict sales for $100,000 (100 in thousands) TV budget
tv_budget_100k = np.array([[100]])
predicted_sales = model_tv.predict(tv_budget_100k)[0]

print(f"Predicted sales for $100,000 TV budget: {predicted_sales * 1000:.0f} units")

# Visualize
plt.figure(figsize=(10, 6))
plt.scatter(advertising['TV'], advertising['Sales'], alpha=0.6, edgecolors='k')

X_line = np.array([[advertising['TV'].min()], [advertising['TV'].max()]])
y_line = model_tv.predict(X_line)
plt.plot(X_line, y_line, color='red', linewidth=2)

plt.xlabel('TV Budget ($ thousands)')
plt.ylabel('Sales (thousands of units)')
plt.title('Sales vs TV Budget with Linear Regression')
plt.grid(True, alpha=0.3)
plt.show()
```

**Antwoorden**:
- Regression line: Sales = 6.9748 + 0.0555 * TV
- Predicted sales for $100,000 TV budget: 12,521 units (of 12.521 in thousands)

#### Stap 4: Meervoudige lineaire regressie

Gebruik meervoudige lineaire regressie voor voorspellingen met alle drie reclamekanalen tegelijk:

```python
# Fit multiple linear regression model
X_multi = advertising[['TV', 'Radio', 'Newspaper']]
y = advertising['Sales']

model_multi = LinearRegression()
model_multi.fit(X_multi, y)

# Get coefficients
print(f"Intercept (β₀): {model_multi.intercept_}")
print(f"TV coefficient (β₁): {model_multi.coef_[0]}")
print(f"Radio coefficient (β₂): {model_multi.coef_[1]}")
print(f"Newspaper coefficient (β₃): {model_multi.coef_[2]}")

# R² for the multiple regression model
from sklearn.metrics import r2_score
y_pred = model_multi.predict(X_multi)
r2_multi = r2_score(y, y_pred)
print(f"\nR² (Multiple Regression): {r2_multi:.4f}")

# Make a prediction with all three channels
# Example: TV=100, Radio=40, Newspaper=30 (in thousands)
example_budget = np.array([[100, 40, 30]])
predicted_sales_multi = model_multi.predict(example_budget)[0]
print(f"\nPredicted sales for TV=$100k, Radio=$40k, Newspaper=$30k: {predicted_sales_multi * 1000:.0f} units")
```

De meervoudige regressie geeft betere voorspellingen omdat deze rekening houdt met alle drie kanalen tegelijk. Het is opvallend dat de effectiviteit van elk kanaal kan verschillen wanneer je rekening houdt met de andere kanalen (interactie-effecten).

---

## Samenvatting

In Hoofdstuk 6 hebben we lineaire regressiemodellen geleerd die essentieel zijn voor:

1. **Lab 6.01**: Eenvoudige lineaire regressie met één onafhankelijke variabele
2. **Lab 6.02**: Trendanalyse over meerdere jaren met voorspellingen
3. **Lab 6.03**: Omgang met uitbijters en hun invloed op regressiemodellen
4. **Lab 6.04**: Practische toepassingen in productiekosten
5. **Lab 6.05**: Vergelijking van eenvoudige versus meervoudige regressie

Alle analyses benutten dezelfde fundamentele concepten:
- $\beta_0$ (intercept) en $\beta_1$ (slope)
- Correlatiecoëfficiënt (R)
- Determinatiecoëfficiënt (R²)
- Residuen en modelvalidatie