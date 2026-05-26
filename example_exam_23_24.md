# Example Exam 2023/2024 - Solutions

## Question 1

### Exercise 1: Scatter Diagram Estimation

#### Overview

Estimate the correlation coefficient ($R$) based on the appearance of a scatter plot.

#### Answer

**Close to 0.**

#### Reasoning

Looking at the scatter plot, the data points form a wide, dispersed cloud with no tight linear pattern. While the regression line has a very slight downward slope, the variance around the line is massive. A correlation coefficient ($R$) close to 1 or -1 would require the points to be tightly grouped around the line. A value of -0.5 would show a more distinct, visible downward trend. Because the points are so scattered, the correlation is extremely weak, making 0 the best estimate.

---

## Question 2

### Exercise 2: Sampling Methods

#### Overview

Students are sorted by height and the first 10 are selected. Evaluate whether this is a valid sampling method.

#### Step 1: Is this a random sample?

**No.** In a true simple random sample, every individual (and every possible group of 10 students) must have an equal chance of being selected. Because the students are sorted by height and chosen from the front of the line, the shortest students are guaranteed to be selected, while the taller students have a 0% chance of being picked.

#### Step 2: What type of error is being made here?

**Selection Bias** (specifically, non-random sampling error). The sampling method systematically favors a specific trait (being short).

#### Step 3: Is this a good sample?

**No.** Because of the selection bias, this sample is completely unrepresentative of the overall school population regarding height, age, or any other variable that might correlate with height.

---

## Question 3

### Exercise 3: Probability

#### Overview

We start with 7 fruits total (4 Oranges, 3 Lemons).

#### Step 1: P(Orange 1)

$$\frac{4 \text{ Oranges}}{7 \text{ Total}} = \frac{4}{7}$$

#### Step 2: P(Orange 1 AND Orange 2) without replacement

- First pick is Orange: $4/7$
- Second pick is Orange (3 Oranges left, 6 Fruits left): $3/6$

$$P(O_1 \cap O_2) = \frac{4}{7} \times \frac{3}{6} = \frac{12}{42} = \frac{2}{7}$$

#### Step 3: P(Orange 2) without replacement

To get an orange on the second pick, you could have picked (Orange, Orange) OR (Lemon, Orange).

- $P(O_1 \cap O_2) = \frac{12}{42}$
- $P(L_1 \cap O_2) = (\frac{3}{7}) \times (\frac{4}{6}) = \frac{12}{42}$

$$\text{Sum: } \frac{12}{42} + \frac{12}{42} = \frac{24}{42} = \frac{4}{7}$$

#### Step 4: P(Orange 1 | Orange 2)

Using conditional probability: $P(A|B) = \frac{P(A \cap B)}{P(B)}$

$$P(O_1 | O_2) = \frac{P(O_1 \cap O_2)}{P(O_2)} = \frac{12/42}{24/42} = \frac{12}{24} = \frac{1}{2}$$

---

## Question 4

### Exercise 4: Normal Distribution & Hypothesis Testing

#### Overview

This question uses $D \sim Nor(13.4, 0.12)$.

#### Step 1: Proportion meeting tolerance limits

```python
import scipy.stats as stats

mu = 13.4
sigma = 0.12

# P(13.35 <= D <= 13.5)
prop = stats.norm.cdf(13.5, loc=mu, scale=sigma) - stats.norm.cdf(13.35, loc=mu, scale=sigma)
print(f"Proportion meeting limits: {prop:.4f}")
# Expected calculation: ~0.4589 (45.89%)
```

#### Step 2: Probability none of 3 pistons meet the limits

```python
p_fail = 1 - prop
p_all_three_fail = p_fail ** 3
print(f"Probability none meet limits: {p_all_three_fail:.4f}")
# Expected calculation: ~0.1584 (15.84%)
```

#### Step 3: Hypothesis Test at 5% Significance Level

- $H_0$: $\mu = 13.4$ (No modifications)
- $H_1$: $\mu \neq 13.4$ (Modifications made; two-tailed)

```python
import numpy as np

m = 13.43
n = 20
mu0 = 13.4
alpha = 0.05

se = sigma / np.sqrt(n)
p_val = min(stats.norm.cdf(m, loc=mu0, scale=se),
            stats.norm.sf(m, loc=mu0, scale=se)) * 2

print(f"p-value: {p_val:.4f}")
# If p < 0.05: Reject H0. If p >= 0.05: Fail to reject H0.
```

---

## Question 5

### Exercise 5: Measurement Levels

#### Overview

Determine the measurement level of each variable.

#### Step 1: Timestamp (Unix time) → Interval

Time has consistent steps, but the 1970 "zero" is an arbitrary starting point, not an absolute absence of time.

#### Step 2: HTTP request type (GET, PUT...) → Nominal

Categorical with no inherent mathematical ranking.

#### Step 3: Response time (in ms) → Ratio

Numeric with a true absolute zero; 0 ms means no time elapsed.

#### Step 4: Response status (2xx, 4xx, 5xx) → Nominal or Ordinal

Ordinal is acceptable as they group from success to severe server errors, but Nominal is safer as they are discrete categorical buckets.

---

## Question 6

### Exercise 6: Paired T-Test

#### Overview

Because you are measuring the exact same subjects at two different time points (`time1` and `time2`), this requires a **Paired t-test** (`stats.ttest_rel`).

#### Step 1: Visualisation

According to the syllabus, a melted boxplot is required for paired data.

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

df_melted = pd.melt(temperatures[['time1', 'time2']], var_name='Timeline', value_name='Temp')

plt.figure(figsize=(6, 5))
sns.boxplot(data=df_melted, x='Timeline', y='Temp')
plt.title("Comparison: Time 1 vs Time 2")
plt.show()
```

#### Step 2: Statistical Test

A Paired Two-Sample t-test.

#### Step 3: Hypotheses

- $H_0$: $\mu_{diff} = 0$ (There is no significant difference between the time points).
- $H_1$: $\mu_{diff} \neq 0$ (There is a significant difference between the time points).

#### Step 4: p-value and Conclusion

```python
import scipy.stats as stats

stat, p_val = stats.ttest_rel(temperatures['time1'], temperatures['time2'])
print(f"p-value: {p_val:.4f}")
# If p_val < 0.05, reject H0 and conclude the temperatures are significantly different.
```

#### Step 5: Descriptive Statistics for time1

```python
time1 = temperatures['time1']

iqr = time1.quantile(0.75) - time1.quantile(0.25)
kurtosis = time1.kurtosis()
data_range = time1.max() - time1.min()
std_dev = time1.std(ddof=1)

print(f"IQR: {iqr}, Kurtosis: {kurtosis}, Range: {data_range}, STD: {std_dev}")
```

---

## Question 7

### Exercise 7: Time Series

#### Overview

Map each plotted model to its method and choose the appropriate forecasting model.

#### Step 1: Mapping the models

- **A (Blue Dashed) = Simple Moving Average with period 12.** You can tell because it begins much later on the timeline than the other lines (it takes 12 periods of data before it can calculate the first point) and smooths out the data heavily.
- **B (Cyan Dotted) = Simple Exponential Smoothing with $\alpha = 0.1$.** A low alpha means the model relies heavily on past data rather than recent changes, resulting in a very smooth line that reacts slowly to spikes.
- **C (Red Dash-Dot) = Simple Exponential Smoothing with $\alpha = 0.9$.** A high alpha means the model gives massive weight to the most recent observation, causing it to closely mimic the raw data with a slight 1-step lag.

#### Step 2: Which model to use?

You should use **Triple Exponential Smoothing (Holt-Winters)**. The black observation line clearly shows both an overall upward trend over the years and repeating seasonality (peaks happening at consistent intervals). Holt-Winters is the only model among the standard syllabus options equipped to handle both trend and seasonality simultaneously.

---

## Question 8

### Exercise 8: Linear Regression

#### Overview

Analyse the relationship between vehicle mileage and maintenance costs.

#### Step 1: Make the plot

```python
import seaborn as sns
import matplotlib.pyplot as plt

# Using seaborn's regplot to match the requested scatter with regression line
sns.regplot(data=sample_data, x='mileage', y='maintenance costs')
plt.title("Maintenance Costs vs Mileage")
plt.show()
```

#### Step 2: Equation of the line & Correlation

```python
from sklearn.linear_model import LinearRegression
import numpy as np

X = sample_data[['mileage']].values
y = sample_data['maintenance costs'].values

model = LinearRegression()
model.fit(X, y)

beta_0 = model.intercept_
beta_1 = model.coef_[0]
r_value = np.corrcoef(sample_data['mileage'], sample_data['maintenance costs'])[0, 1]

print(f"Equation: ŷ = {beta_0:.4f} + {beta_1:.4f}x")
print(f"Correlation (r): {r_value:.4f}")
```

#### Step 3: Interpretation

Assuming $R$ is close to 0.9 (based on the visual), you would interpret this as a strong positive correlation. As the mileage increases, the maintenance costs increase predictably.

#### Step 4: Maintenance cost for 5000 km

```python
future_x = np.array([[5000]])
prediction = model.predict(future_x)[0]
print(f"Estimated cost for 5000 km: €{prediction:.2f}")
```

#### Step 5: Fixed cost (0 kilometres)

The fixed cost is exactly the value of the intercept ($\beta_0$) calculated in Step 2.

---

## Question 9

### Exercise 9: Goodness-of-Fit Test

#### Overview

Because you are checking the frequencies of 1 categorical variable against an expected distribution, you use the **Chi-Square Goodness-of-Fit Test**.

#### Step 1: Visualisation

```python
import seaborn as sns
import matplotlib.pyplot as plt

sns.countplot(data=products, x='Choice')
plt.title("Product Preferences")
plt.show()
```

**Inference**: Look at the bars. If one bar is drastically higher than the others, you can infer that customers likely have a distinct preference.

#### Step 2: Hypothesis Test

The Chi-Square Goodness-of-Fit Test (`stats.chisquare`).

#### Step 3: Hypotheses

- $H_0$: Customers have no distinct preference (the preference is uniformly distributed; 33.3% for each product).
- $H_1$: Customers have a distinct preference (the proportions deviate from a uniform distribution).

#### Step 4: Test Statistic and p-value

```python
import numpy as np
import scipy.stats as stats

# 1. Get observed counts
observed = products['Choice'].value_counts().values
total_n = observed.sum()

# 2. Expected is equal across all 3 products (1/3 each)
expected_percentages = np.array([1/3, 1/3, 1/3])
expected = expected_percentages * total_n

# 3. Calculate test statistic and p-value
chi2_stat, p_value = stats.chisquare(f_obs=observed, f_exp=expected)

print(f"Chi-square (χ²): {chi2_stat:.4f}")
print(f"p-value: {p_value:.4f}")
```

#### Step 5: Conclusion

Compare your calculated `p_value` to $\alpha = 0.05$. If $p < 0.05$, reject $H_0$ and conclude that customers do indeed have a statistically significant preference for a specific product.

---

## Summary Example Exam 2023/2024

This exam covers the following techniques:

### Distributions & Probability

1. **Normal distribution**: CDF for proportions between limits
2. **Probability rules**: Multiplication for independent/sequential events, conditional probability $P(A|B) = \frac{P(A \cap B)}{P(B)}$

### Hypothesis Tests by Variable Type

1. **Z-test**: Mean against a value when $\sigma$ is known (two-tailed)
2. **Paired t-test** (`stats.ttest_rel`): Same subjects measured twice
3. **Chi-Square Goodness-of-Fit** (`stats.chisquare`): One categorical variable vs. an expected distribution

### Modelling & Description

- **Linear regression**: Equation $\hat{y} = \beta_0 + \beta_1 x$, prediction, and intercept as fixed cost
- **Correlation ($R$)**: Direction and strength; near 0 = weak, near ±1 = strong
- **Time series**: Identifying SMA vs. SES (by $\alpha$) and selecting Holt-Winters for trend + seasonality
- **Measurement levels**: Nominal, ordinal, interval (arbitrary zero), ratio (absolute zero)
- **Sampling**: Recognising selection bias and non-representative samples
