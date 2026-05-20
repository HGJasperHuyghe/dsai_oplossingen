# Hoofdstuk 7 - Tijdreeksanalyse

## Lab 7.01

### Exercise 1: House sales

#### Overzicht

Het bestand `House Sales.csv` bevat maandgegevens van het aantal nieuwe éénmalige woningen verkocht in de Verenigde Staten (in duizenden) van januari 1991 tot december 2011. Huizenbverkoop steeg constant tot ongeveer begin 2006, waarna de huizenmarkt instortte. Uiteindelijk begon de verkoop opnieuw stijgen.

#### Voorbereiding: Package imports

```python
import numpy as np
import pandas as pd
import scipy.stats as stats
import matplotlib.pyplot as plt
import seaborn as sns

from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_absolute_error, mean_squared_error

from datetime import datetime
from statsmodels.tsa.api import Holt
from statsmodels.tsa.holtwinters import ExponentialSmoothing
from statsmodels.tsa.seasonal import seasonal_decompose
```

#### Stap 1: Dataset inladen

```python
data = pd.read_csv(
    'https://raw.githubusercontent.com/HoGentTIN/dsai-labs/main/data/Monthly%20House%20Sales.csv',
    delimiter=";",
    parse_dates=['Month']
).set_index(['Month'])

data.head()
```

Dit geeft:

| Month | Houses Sold |
|:---|---:|
| 1991-01-01 | 401 |
| 1991-02-01 | 482 |
| 1991-03-01 | 507 |
| 1991-04-01 | 508 |
| 1991-05-01 | 517 |

#### Stap 2: Tijdreeksgrafiek maken

Creëer een grafiek van de huizenbverkoop over de tijd:

```python
plt.figure(figsize=(14, 6))
plt.plot(data.index, data['Houses Sold'], linewidth=2, color='steelblue')
plt.title('Monthly House Sales (1991-2011)', fontsize=14)
plt.xlabel('Date', fontsize=12)
plt.ylabel('Houses Sold (thousands)', fontsize=12)
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.show()
```

**Observatie**: De grafiek toont een stijgende trend tot 2006, gevolgd door een scherpe daling, en daarna een herstel.

#### Stap 3: Simple Moving Average (SMA)

Voeg voorspellingen toe voor SMA met span van 3, 6 en 12 maanden:

```python
# Calculate Simple Moving Averages
data['SMA3'] = data['Houses Sold'].rolling(window=3).mean()
data['SMA6'] = data['Houses Sold'].rolling(window=6).mean()
data['SMA12'] = data['Houses Sold'].rolling(window=12).mean()

# Display the dataframe
print(data.head(15))
```

#### Stap 4: Simple Exponential Smoothing (SES)

```python
# Fit Simple Exponential Smoothing model
from statsmodels.tsa.api import SimpleExpSmoothing

model_ses = SimpleExpSmoothing(data['Houses Sold'])
fit_ses = model_ses.fit(optimized=True)

# Get fitted values
data['SES'] = fit_ses.fittedvalues

print(f"SES smoothing parameter (alpha): {fit_ses.params['smoothing_level']:.4f}")
```

#### Stap 5: Double Exponential Smoothing (DES)

```python
# Fit Double Exponential Smoothing (Holt's method)
model_des = Holt(data['Houses Sold'])
fit_des = model_des.fit(optimized=True)

# Get fitted values
data['DES'] = fit_des.fittedvalues

print(f"DES alpha: {fit_des.params['smoothing_level']:.4f}")
print(f"DES beta: {fit_des.params['smoothing_trend']:.4f}")
```

#### Stap 6: Visualisatie van alle modellen

```python
plt.figure(figsize=(16, 6))

plt.plot(data.index, data['Houses Sold'], label='Actual', linewidth=2, color='black')
plt.plot(data.index, data['SMA3'], label='SMA3', alpha=0.7)
plt.plot(data.index, data['SMA6'], label='SMA6', alpha=0.7)
plt.plot(data.index, data['SMA12'], label='SMA12', alpha=0.7)
plt.plot(data.index, data['SES'], label='SES', alpha=0.7)
plt.plot(data.index, data['DES'], label='DES (Holt)', alpha=0.7)

plt.title('House Sales with Different Smoothing Models', fontsize=14)
plt.xlabel('Date', fontsize=12)
plt.ylabel('Houses Sold (thousands)', fontsize=12)
plt.legend(loc='best')
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.show()
```

#### Stap 7: MAE-berekening voor model evaluatie

```python
# Drop rows with NaN values
data_clean = data.dropna()

# Calculate MAE for each model
columns = ['SMA3', 'SMA6', 'SMA12', 'SES', 'DES']
mae_values = {}

for col in columns:
    mae = mean_absolute_error(data_clean['Houses Sold'], data_clean[col])
    mae_values[col] = mae
    print(f"MAE for {col}: {mae:.2f}")

# Find the best model
best_model = min(mae_values, key=mae_values.get)
print(f"\nBest model: {best_model} with MAE: {mae_values[best_model]:.2f}")
```

---

## Lab 7.02

### Exercise 2: Aircraft engines

#### Overzicht

Je bent aangesteld om het aantal vliegtuigmotoren te voorspellen dat elke maand door een motorfabrikant wordt besteld. Aan het einde van februari is de voorspelling dat 100 motoren in april zullen worden besteld. In maart worden daadwerkelijk 120 motoren besteld.

Gebruikmaking van $\alpha = 0.3$, bepaal een voorspelling (aan het einde van maart) voor het aantal bestellingen in april en mei. Gebruik Simple Exponential Smoothing.

#### Relevante formules

De Simple Exponential Smoothing-formules zijn:

$$X_t = \alpha x_t + (1 - \alpha) X_{t-1}$$

$$F_{t+m} = X_t$$

Waarbij:
- $X_t$ = geëxtrapoleerde waarde op moment $t$
- $\alpha$ = smoothing parameter (0 < $\alpha$ < 1)
- $x_t$ = werkelijke waarde op moment $t$
- $F_{t+m}$ = voorspelling voor moment $t+m$

#### Berekening

```python
# Gegeven gegevens
alpha = 0.3
X_feb = 100  # Voorspelling aan het einde van februari voor april
x_mar = 120  # Werkelijke bestellingen in maart

# Stap 1: Bereken X_mar (geëxtrapoleerde waarde einde maart)
X_mar = alpha * x_mar + (1 - alpha) * X_feb
X_mar = 0.3 * 120 + 0.7 * 100
X_mar = 36 + 70
X_mar = 106

print(f"Smoothed level at end of March (X_mar): {X_mar}")

# Stap 2: Voorspelling voor april (F_Apr)
# Deze is gelijk aan de geëxtrapoleerde waarde op einde maart
F_Apr = X_mar
F_Apr = 106

print(f"Forecast for April (at end of March): {F_Apr}")

# Stap 3: Voorspelling voor mei (F_May)
# Deze is ook gelijk aan X_mar (geen trend component)
F_May = X_mar
F_May = 106

print(f"Forecast for May (at end of March): {F_May}")
```

#### Resultaten

- **Voorspelling voor april**: 106 motoren
- **Voorspelling voor mei**: 106 motoren

Deze voorspellingen zijn gebaseerd op een geëxtrapoleerde waarde die rekening houdt met zowel de werkelijke meting van maart (120) als de eerdere voorspelling van februari (100), gewogen met $\alpha = 0.3$.

---

## Lab 7.03

### Exercise 3: Car sales

#### Overzicht

Een autodealer gebruikt Holt's methode (Double Exponential Smoothing) om wekelijkse autoverkopers te voorspellen. Het huidige niveau wordt geschat op 50 auto's per week, en de trend wordt geschat op 6 auto's per week. In de huige week worden 30 auto's verkocht.

Na waarneming van de verkoop van deze week, voorspel het aantal auto's drie weken later. Gebruik $\alpha = \beta = 0.3$.

#### Relevante formules

Holt's methode (Double Exponential Smoothing) gebruikt twee vergelijkingen:

$$X_t = \alpha x_t + (1-\alpha)(X_{t-1} + b_{t-1})$$

$$b_t = \beta(X_t - X_{t-1}) + (1-\beta)b_{t-1}$$

$$F_{t+m} = X_t + mb_t$$

Waarbij:
- $X_t$ = geëxtrapoleerde niveau op moment $t$
- $b_t$ = trend op moment $t$
- $\alpha$ = smoothing parameter voor niveau
- $\beta$ = smoothing parameter voor trend
- $F_{t+m}$ = voorspelling voor $m$ perioden vooruit

#### Gegeven informatie

```python
# Gegeven parameters
alpha = 0.3
beta = 0.3
X_prev = 50      # Geschat niveau vorige week
b_prev = 6       # Geschatte trend vorige week
x_current = 30   # Werkelijke verkoop hutige week
m = 3            # Voorspel 3 weken vooruit
```

#### Berekening

```python
# Stap 1: Bereken het nieuwe niveau (X_t)
X_current = alpha * x_current + (1 - alpha) * (X_prev + b_prev)
X_current = 0.3 * 30 + 0.7 * (50 + 6)
X_current = 9 + 0.7 * 56
X_current = 9 + 39.2
X_current = 48.2

print(f"New level (X_current): {X_current}")

# Stap 2: Bereken de nieuwe trend (b_t)
b_current = beta * (X_current - X_prev) + (1 - beta) * b_prev
b_current = 0.3 * (48.2 - 50) + 0.7 * 6
b_current = 0.3 * (-1.8) + 4.2
b_current = -0.54 + 4.2
b_current = 3.66

print(f"New trend (b_current): {b_current}")

# Stap 3: Voorspel de verkoop 3 weken later
F_3weeks = X_current + m * b_current
F_3weeks = 48.2 + 3 * 3.66
F_3weeks = 48.2 + 10.98
F_3weeks = 59.18

print(f"Forecast for 3 weeks from now: {F_3weeks:.2f} cars")
```

#### Resultaat

**Voorspelling voor autoverkopers 3 weken later**: 59.18 auto's (afgerond: 59 auto's)

Dit toont aan dat ondanks de lage verkoop van deze week (30 auto's), de voorspelling stijgt boven het huide niveau door de positieve trend die nog steeds aanwezig is.

---

## Lab 7.04

### Exercise 4: Airline ticket data

#### Overzicht

Analyseer vliegtuigkaartjesgegevens om tijdreekspatronen te identificeren en voorspellingen te doen.

#### Voorbereiding: Package imports

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from statsmodels.tsa.holtwinters import ExponentialSmoothing
from sklearn.metrics import mean_absolute_error
```

#### Stap 1: Dataset inladen

```python
data = pd.read_csv(
    'https://raw.githubusercontent.com/HoGentTIN/dsai-labs/main/data/airline%20ticket%20data.csv',
    delimiter=";",
    parse_dates=['Month']
).set_index(['Month'])

data.head()
```

#### Stap 2: Tijdreeksgrafiek en model selectie

```python
# Visualiseer de gegevens
plt.figure(figsize=(14, 6))
plt.plot(data.index, data['Tickets'], linewidth=2, color='steelblue', marker='o', markersize=4)
plt.title('Airline Ticket Sales Over Time', fontsize=14)
plt.xlabel('Date', fontsize=12)
plt.ylabel('Number of Tickets', fontsize=12)
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.show()
```

**Model selectie**: Op basis van de grafiek zou je kunnen zien of er:
- Alleen een trend is (DES/Holt) → gebruik DES
- Trend EN seizoenaliteit (DES+seizoen) → gebruik ExponentialSmoothing met seizoenale component
- Alleen niveau variaties → gebruik SES

#### Stap 3: Model creëren en trainen

```python
# Fit the Exponential Smoothing model
# Adjust seasonal parameter if seasonality is present (e.g., seasonal='add' for additive)
model = ExponentialSmoothing(
    data['Tickets'],
    trend='add',           # additive or multiplicative trend
    seasonal=None,         # 'add'/'mul' if seasonal, None otherwise
    seasonal_periods=12    # if seasonal, specify period (e.g., 12 for monthly)
)

fit = model.fit(optimized=True)

# Get fitted values
data['Fitted'] = fit.fittedvalues

print(fit.summary())
```

#### Stap 4: Voorspellingen voor volgende 12 maanden

```python
# Predict next 12 months
forecast_steps = 12
forecast = fit.get_forecast(steps=forecast_steps)
forecast_values = forecast.predicted_mean

print(f"Forecasted values for next 12 months:")
print(forecast_values)
```

#### Stap 5: Visualisatie met voorspellingen

```python
plt.figure(figsize=(16, 6))

# Plot actual data
plt.plot(data.index, data['Tickets'], label='Actual', linewidth=2, color='steelblue', marker='o')

# Plot fitted values
plt.plot(data.index, data['Fitted'], label='Fitted', linewidth=2, color='orange', alpha=0.7)

# Plot forecast
forecast_index = pd.date_range(start=data.index[-1], periods=forecast_steps+1, freq='MS')[1:]
plt.plot(forecast_index, forecast_values, label='Forecast', linewidth=2, color='red', marker='s', linestyle='--')

plt.title('Airline Ticket Sales: Actual, Fitted, and Forecast', fontsize=14)
plt.xlabel('Date', fontsize=12)
plt.ylabel('Number of Tickets', fontsize=12)
plt.legend(loc='best')
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.show()
```

---

## Lab 7.05

### Exercise 5: Alcoholic beverages sales

#### Overzicht

Het bestand `US Retail.csv` bevat maandelijkse detailhandelverkoop van bier, wijn en sterke drank in Amerikaanse slijterijen.

#### Stap 1: Dataset inladen en onderzoeken

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from statsmodels.tsa.seasonal import seasonal_decompose
from statsmodels.tsa.holtwinters import ExponentialSmoothing
from sklearn.metrics import mean_absolute_error

data = pd.read_csv(
    'https://raw.githubusercontent.com/HoGentTIN/dsai-labs/main/data/US%20Retail.csv',
    delimiter=";",
    parse_dates=['Month']
).set_index(['Month'])

data.head()
```

#### Stap 2: Controle op seizoenaliteit

```python
# Plot the time series
plt.figure(figsize=(14, 6))
plt.plot(data.index, data['Sales'], linewidth=2, color='steelblue')
plt.title('US Retail Alcohol Sales Over Time', fontsize=14)
plt.xlabel('Date', fontsize=12)
plt.ylabel('Sales ($ millions)', fontsize=12)
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.show()

# Decompose the time series
decomposition = seasonal_decompose(data['Sales'], model='multiplicative', period=12)
decomposition.plot()
plt.tight_layout()
plt.show()

print("Decomposition components:")
print(f"Trend: {decomposition.trend}")
print(f"Seasonal: {decomposition.seasonal}")
print(f"Residual: {decomposition.resid}")
```

**Seizoenaliteit**: Ja, duidelijk zichtbaar (hogere verkoop rond de feestdagen).

#### Stap 3: Train/test split

```python
# Split into train and test set
train_data = data['1992-01-01':'2008-12-01']
test_data = data['2009-01-01':'2009-12-01']

print(f"Train set size: {len(train_data)}")
print(f"Test set size: {len(test_data)}")
```

#### Stap 4: Model creëren en trainen

```python
# Fit Exponential Smoothing model with seasonality
model = ExponentialSmoothing(
    train_data['Sales'],
    trend='add',
    seasonal='mul',
    seasonal_periods=12
)

fit = model.fit(optimized=True)

# Get fitted values for train set
train_data_copy = train_data.copy()
train_data_copy['Fitted'] = fit.fittedvalues

print(fit.summary())
```

#### Stap 5: Voorspellingen voor 2009

```python
# Forecast for 2009
forecast = fit.get_forecast(steps=12)
forecast_2009 = forecast.predicted_mean

print("Forecasted values for 2009:")
print(forecast_2009)
```

#### Stap 6: Visualisatie

```python
plt.figure(figsize=(16, 6))

# Plot train data
plt.plot(train_data.index, train_data['Sales'], label='Train Data', linewidth=2, color='steelblue')

# Plot fitted values
plt.plot(train_data_copy.index, train_data_copy['Fitted'], label='Fitted Values', linewidth=2, color='orange', alpha=0.7)

# Plot test data
plt.plot(test_data.index, test_data['Sales'], label='Test Data (Actual)', linewidth=2, color='green', marker='o')

# Plot forecast
forecast_index = pd.date_range(start='2009-01-01', periods=12, freq='MS')
plt.plot(forecast_index, forecast_2009, label='Forecast', linewidth=2, color='red', marker='s', linestyle='--')

plt.title('US Alcohol Retail Sales: Train, Test, and Forecast', fontsize=14)
plt.xlabel('Date', fontsize=12)
plt.ylabel('Sales ($ millions)', fontsize=12)
plt.legend(loc='best')
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.show()
```

#### Stap 7: MAE-berekening

```python
# Calculate MAE on test set
mae = mean_absolute_error(test_data['Sales'], forecast_2009)
print(f"Mean Absolute Error (MAE) on test set: ${mae:.2f} million")
```

---

## Lab 7.06

### Exercise 6: COVID-19 data

#### Overzicht

In dit lab gebruiken we de COVID-19 dataset van [Our World in Data](https://ourworldindata.org/) om tijdreeksanalyse toe te passen op pandemiegegevens.

#### Voorbereiding

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from statsmodels.tsa.seasonal import seasonal_decompose
from statsmodels.tsa.holtwinters import ExponentialSmoothing
```

#### Stap 1: Dataset importeren

```python
# Import the COVID-19 dataset
# Tip: The CSV file is rather large, so you might want to use chunksize
data = pd.read_csv(
    'https://covid.ourworldindata.org/data/owid-covid-data.csv',
    parse_dates=['date']
).set_index(['date'])

# Filter for a specific country if needed (e.g., Belgium)
country = 'Belgium'
country_data = data[data['location'] == country].copy()

# Select relevant columns (e.g., new_cases, total_deaths)
country_data = country_data[['new_cases', 'total_deaths', 'total_cases']]

print(country_data.head())
```

#### Stap 2: Exploratieve analyse

```python
# Visualize COVID-19 time series
fig, axes = plt.subplots(3, 1, figsize=(14, 10))

axes[0].plot(country_data.index, country_data['new_cases'], linewidth=1, color='steelblue')
axes[0].set_title(f'{country}: Daily New Cases', fontsize=12)
axes[0].set_ylabel('Cases')
axes[0].grid(True, alpha=0.3)

axes[1].plot(country_data.index, country_data['total_cases'], linewidth=1, color='orange')
axes[1].set_title(f'{country}: Cumulative Total Cases', fontsize=12)
axes[1].set_ylabel('Cases')
axes[1].grid(True, alpha=0.3)

axes[2].plot(country_data.index, country_data['total_deaths'], linewidth=1, color='red')
axes[2].set_title(f'{country}: Cumulative Total Deaths', fontsize=12)
axes[2].set_ylabel('Deaths')
axes[2].set_xlabel('Date')
axes[2].grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

#### Stap 3: Seizoenaliteit en decomposition

```python
# Check for seasonality
# Note: COVID-19 data might not have clear seasonal patterns like traditional economic data

# Try to decompose (this might require interpolation for missing values)
data_clean = country_data['new_cases'].fillna(method='ffill')

try:
    decomposition = seasonal_decompose(data_clean, model='additive', period=365)
    decomposition.plot()
    plt.tight_layout()
    plt.show()
except Exception as e:
    print(f"Decomposition failed: {e}")
```

#### Stap 4: Voorspellingen

```python
# Prepare data for modeling
data_for_model = country_data['new_cases'].dropna()

# Fit an exponential smoothing model
model = ExponentialSmoothing(
    data_for_model,
    trend='add',
    seasonal=None
)

fit = model.fit(optimized=True)

# Forecast next 30 days
forecast_30days = fit.get_forecast(steps=30).predicted_mean

# Visualize
plt.figure(figsize=(14, 6))
plt.plot(data_for_model.index, data_for_model, label='Actual', linewidth=2, color='steelblue')
plt.plot(data_for_model.index, fit.fittedvalues, label='Fitted', linewidth=2, color='orange', alpha=0.7)

forecast_index = pd.date_range(start=data_for_model.index[-1], periods=31, freq='D')[1:]
plt.plot(forecast_index, forecast_30days, label='Forecast (30 days)', linewidth=2, color='red', linestyle='--')

plt.title(f'{country}: COVID-19 Cases with Forecast', fontsize=14)
plt.xlabel('Date', fontsize=12)
plt.ylabel('Daily New Cases', fontsize=12)
plt.legend(loc='best')
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.show()
```

---

## Lab 7.07

### Exercise 7: Golden Cross

#### Overzicht

Een **Golden Cross** is een patroon dat gebruikt wordt in technische analyse van aandeelprijzen. Een Golden Cross treedt op wanneer een korte-termijn voortschrijdend gemiddelde een lange-termijn voortschrijdend gemiddelde van onderaf doorbreekt. Dit is een indicator voor het potentieel van een grote rally op de aandelen (*bull market*). Het lange-termijn voortschrijdend gemiddelde wordt dan beschouwd als een "weerstandsniveau".

#### Stap 1: Dataset inladen en voorbereiding

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# Import S&P 500 data
data = pd.read_csv(
    'https://raw.githubusercontent.com/HoGentTIN/dsai-labs/main/data/SP500.csv',
    parse_dates=['Date']
).set_index(['Date'])

# Keep only the Close column
data = data[['Close']].copy()

print(data.head())
```

#### Stap 2: Visualiseer de tijdreeks

```python
plt.figure(figsize=(16, 6))
plt.plot(data.index, data['Close'], linewidth=1.5, color='steelblue', label='S&P 500 Close Price')
plt.title('S&P 500 Index Over Time', fontsize=14)
plt.xlabel('Date', fontsize=12)
plt.ylabel('Price', fontsize=12)
plt.grid(True, alpha=0.3)
plt.legend()
plt.tight_layout()
plt.show()
```

#### Stap 3: Berekenen van voortschrijdende gemiddelden

Traders gebruiken voortschrijdende gemiddelden vaak bij het analyseren van aandeelprijzen:
- **50-day moving average**: Het gemiddelde van de afgelopen 10 handelsweken (veel gebruikt steunniveau)
- **200-day moving average**: Het gemiddelde van de afgelopen 40 weken (wordt gebruikt om aan te geven dat een prijs relatief goedkoop is vergeleken met het prijsbereik van het afgelopen jaar)

```python
# Calculate moving averages
data['MA50'] = data['Close'].rolling(window=50).mean()
data['MA200'] = data['Close'].rolling(window=200).mean()

print(data.head(250))
```

#### Stap 4: Visualisatie met voortschrijdende gemiddelden

```python
plt.figure(figsize=(16, 7))

plt.plot(data.index, data['Close'], label='S&P 500 Close', linewidth=1, color='steelblue', alpha=0.7)
plt.plot(data.index, data['MA50'], label='50-day MA', linewidth=2, color='orange')
plt.plot(data.index, data['MA200'], label='200-day MA', linewidth=2, color='red')

plt.title('S&P 500 with 50-day and 200-day Moving Averages', fontsize=14)
plt.xlabel('Date', fontsize=12)
plt.ylabel('Price', fontsize=12)
plt.legend(loc='best', fontsize=10)
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.show()
```

#### Stap 5: Identificeren van belangrijke events

**Augustus 2011 stock market crash**: Zoek naar de scherpe daling in de grafiek veroorzaakt door de Europese schuldcrisis.

```python
# Focus on 2011-2015 to see the Golden Crosses after the 2011 crash
start_date = '2010-01-01'
end_date = '2015-12-31'
subset = data[start_date:end_date].copy()

plt.figure(figsize=(16, 7))

plt.plot(subset.index, subset['Close'], label='S&P 500 Close', linewidth=1, color='steelblue', alpha=0.7)
plt.plot(subset.index, subset['MA50'], label='50-day MA', linewidth=2, color='orange')
plt.plot(subset.index, subset['MA200'], label='200-day MA', linewidth=2, color='red')

# Mark August 2011 crash
plt.axvline(x=pd.Timestamp('2011-08-01'), color='black', linestyle='--', alpha=0.5, label='Aug 2011 Crash')

plt.title('S&P 500: Golden Cross Opportunities (2010-2015)', fontsize=14)
plt.xlabel('Date', fontsize=12)
plt.ylabel('Price', fontsize=12)
plt.legend(loc='best', fontsize=10)
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.show()
```

#### Stap 6: Identificeren van Golden Crosses

```python
# Identify Golden Crosses (when MA50 crosses above MA200)
data['MA_Diff'] = data['MA50'] - data['MA200']
data['Signal'] = np.where(data['MA_Diff'] > 0, 1, 0)
data['Golden_Cross'] = data['Signal'].diff()

# Golden Crosses occur when Golden_Cross == 1
golden_crosses = data[data['Golden_Cross'] == 1].copy()

print("Golden Cross dates:")
print(golden_crosses[['Close', 'MA50', 'MA200']])
```

#### Stap 7: Analyse van bull markets

```python
# Analyze the bull markets following each Golden Cross
# Find when MA50 drops below MA200 again (end of bull market)

bull_markets = []
current_bull = None

for idx, row in data.iterrows():
    if row['Golden_Cross'] == 1:  # Golden Cross detected
        current_bull = {'start': idx, 'start_price': row['Close']}
    elif current_bull is not None and row['Signal'] == 0 and row['MA_Diff'] < 0:  # MA50 drops below MA200
        current_bull['end'] = idx
        current_bull['end_price'] = row['Close']
        bull_markets.append(current_bull)
        current_bull = None

print("Bull Markets Following Golden Crosses:")
for market in bull_markets:
    duration = (market['end'] - market['start']).days
    price_change = market['end_price'] - market['start_price']
    pct_change = (price_change / market['start_price']) * 100
    
    print(f"Start: {market['start'].date()} ({market['start_price']:.2f})")
    print(f"End: {market['end'].date()} ({market['end_price']:.2f})")
    print(f"Duration: {duration} days")
    print(f"Price change: {price_change:.2f} ({pct_change:.1f}%)\n")
```

#### Stap 8: Support level analyse

```python
# Check if MA200 acts as a support level
# Plot with focus on periods where price touches MA200

plt.figure(figsize=(16, 7))

plt.fill_between(data.index, data['MA200'], data['Close'], 
                 where=(data['Close'] >= data['MA200']), 
                 alpha=0.2, color='green', label='Price > MA200')
plt.fill_between(data.index, data['MA200'], data['Close'], 
                 where=(data['Close'] < data['MA200']), 
                 alpha=0.2, color='red', label='Price < MA200')

plt.plot(data.index, data['Close'], label='S&P 500 Close', linewidth=1, color='steelblue')
plt.plot(data.index, data['MA200'], label='200-day MA (Support)', linewidth=2, color='red', linestyle='--')

plt.title('S&P 500: MA200 as Support Level', fontsize=14)
plt.xlabel('Date', fontsize=12)
plt.ylabel('Price', fontsize=12)
plt.legend(loc='best')
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.show()

# Print statistics
times_touched = (abs(data['Close'] - data['MA200']) < 10).sum()
print(f"Number of trading days where price was within $10 of MA200: {times_touched}")
```

---

## Lab 7.08

### Exercise 8: Walmart Sales

#### Overzicht

Analyse van **Walmart's kwartaalverkopen** (uitgedrukt in miljarden dollars).

#### Stap 1: Dataset voorbereiding

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from datetime import datetime

# Walmart quarterly sales data
data = {
    'startdate': ['Q1 2015', 'Q2 2015', 'Q3 2015', 'Q4 2015', 
                  'Q1 2016', 'Q2 2016', 'Q3 2016', 'Q4 2016',
                  'Q1 2017', 'Q2 2017', 'Q3 2017', 'Q4 2017',
                  'Q1 2018', 'Q2 2018', 'Q3 2018', 'Q4 2018'],
    'sales': [119.76, 116.18, 116.66, 131.63,
              122.29, 118.10, 119.45, 135.02,
              123.70, 119.35, 120.52, 138.81,
              126.75, 122.38, 123.90, 143.04]
}

df = pd.DataFrame(data)

# Convert startdate to datetime
def parse_quarter(quarter_str):
    parts = quarter_str.split()
    quarter = int(parts[0][1])
    year = int(parts[1])
    month = (quarter - 1) * 3 + 1
    return pd.Timestamp(year=year, month=month, day=1)

df['Date'] = df['startdate'].apply(parse_quarter)
df = df.set_index('Date')

print(df)
```

#### Stap 2: Tijdreeksgrafiek

```python
plt.figure(figsize=(12, 6))
plt.plot(df.index, df['sales'], marker='o', linewidth=2, markersize=8, color='steelblue')
plt.title("Walmart Quarterly Sales", fontsize=14)
plt.xlabel("Quarter", fontsize=12)
plt.ylabel("Sales ($ billions)", fontsize=12)
plt.grid(True, alpha=0.3)
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

**Observaties**:
- Opwaartse trend over de tijd
- Seizoenaliteit: hogere verkopen in Q4 (feestdagen)

#### Stap 3: Trend- en seizoenanalyse

```python
from statsmodels.tsa.seasonal import seasonal_decompose

# Decompose the time series
decomposition = seasonal_decompose(df['sales'], model='multiplicative', period=4)
decomposition.plot()
plt.tight_layout()
plt.show()
```

#### Stap 4: Exponentiële afvlakking

```python
from statsmodels.tsa.holtwinters import ExponentialSmoothing
from sklearn.metrics import mean_absolute_error, mean_squared_error

# Fit Triple Exponential Smoothing (Holt-Winters) with seasonality
model = ExponentialSmoothing(
    df['sales'],
    trend='add',
    seasonal='mul',
    seasonal_periods=4
)

fit = model.fit(optimized=True)

# Get fitted values
df['Fitted'] = fit.fittedvalues

print(fit.summary())
```

#### Stap 5: Voorspelling

```python
# Forecast the next 4 quarters
forecast = fit.get_forecast(steps=4)
forecast_values = forecast.predicted_mean

print("Forecast for next 4 quarters:")
print(forecast_values)
```

#### Stap 6: Visualisatie met voorspellingen

```python
plt.figure(figsize=(14, 6))

# Plot actual data
plt.plot(df.index, df['sales'], marker='o', label='Actual Sales', linewidth=2, color='steelblue')

# Plot fitted values
plt.plot(df.index, df['Fitted'], label='Fitted Values', linewidth=2, color='orange', alpha=0.7)

# Plot forecast
forecast_index = pd.date_range(start=df.index[-1], periods=5, freq='QS')[1:]
plt.plot(forecast_index, forecast_values, marker='s', label='Forecast', linewidth=2, 
         color='red', linestyle='--', markersize=6)

plt.title('Walmart Sales: Actual, Fitted, and Forecast', fontsize=14)
plt.xlabel('Quarter', fontsize=12)
plt.ylabel('Sales ($ billions)', fontsize=12)
plt.legend(loc='best')
plt.grid(True, alpha=0.3)
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

#### Stap 7: Model evaluatie

```python
# Calculate model accuracy metrics
mae = mean_absolute_error(df['sales'], df['Fitted'])
rmse = np.sqrt(mean_squared_error(df['sales'], df['Fitted']))

print(f"Mean Absolute Error (MAE): ${mae:.2f}B")
print(f"Root Mean Squared Error (RMSE): ${rmse:.2f}B")
```

---

## Samenvatting Hoofdstuk 7

In Hoofdstuk 7 hebben we belangrijke technieken geleerd voor **tijdreeksanalyse**:

### Concepten

- **Trend**: Langetermijn beweging in gegevens
- **Seizoenaliteit**: Regelmatige patronen die herhalen in vaste intervallen
- **Cycliciteit**: Langere-termijn oscillaties

### Voorspellingsmethoden

1. **Simple Moving Average (SMA)**: Gemiddelde van afgelopen n perioden
2. **Simple Exponential Smoothing (SES)**: Geëxtrapoleerde waarde met gewichten
3. **Double Exponential Smoothing (DES/Holt)**: SES + trend component
4. **Triple Exponential Smoothing**: Holt-Winters voor seizoenaliteit

### Evaluatiemetrieken

- **MAE** (Mean Absolute Error): Gemiddelde absolute afwijking
- **RMSE** (Root Mean Squared Error): Wortel van gemiddelde gekwadrateerde fouten

### Toepassingen

- Huizenprijzen
- Luchtvaartgegevens
- Aandeelprijzen
- Verkoopcijfers
- COVID-19 trends