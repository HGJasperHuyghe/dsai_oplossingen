# Hoofdstuk 5

## Lab 5.01

### Exercise 1 - Soft-drink cans

#### Overzicht

To determine if consumers prefer the new soft-drink can style over the traditional one, we need to analyze paired data. Since the same group of consumers rated both the old and new styles, their scores are dependent. The appropriate statistical test here is a Paired $t$-test (or a Wilcoxon signed-rank test if normality assumptions are heavily violated, though a paired $t$-test is standard for this type of directional hypothesis layout).

#### Visualisatie van de data (Boxplots)

Before running the tests, boxplots help us visually check the distribution shifts between the old and new styles.

#### Python Code Implementatie

```python
import numpy as np
import pandas as pd
import scipy.stats as stats
import matplotlib.pyplot as plt
import seaborn as sns

# Assuming your data is stored in a pandas DataFrame called 'df'
# with columns: 'AO', 'AN', 'WBO', 'WBN'

# 1. Generate the Boxplots
fig, axes = plt.subplots(1, 2, figsize=(12, 5))

# Attractiveness Boxplot
df_A = pd.melt(df[['AO', 'AN']], var_name='Can Style', value_name='Rating')
sns.boxplot(x='Can Style', y='Rating', data=df_A, ax=axes[0], palette='Set2')
axes[0].set_title('Attractiveness: Old (AO) vs New (AN)')
axes[0].set_ylim(0.5, 7.5)

# Likelihood to Buy Boxplot
df_WB = pd.melt(df[['WBO', 'WBN']], var_name='Can Style', value_name='Rating')
sns.boxplot(x='Can Style', y='Rating', data=df_WB, ax=axes[1], palette='Set2')
axes[1].set_title('Likelihood to Buy: Old (WBO) vs New (WBN)')
axes[1].set_ylim(0.5, 7.5)

plt.tight_layout()
plt.show()

# 2. Run the Paired t-tests (Alternative='greater' tests if New > Old)
# Note: stats.ttest_rel(a, b) calculates value of (a - b)
test_attractiveness = stats.ttest_rel(df['AN'], df['AO'], alternative='greater')
test_buy = stats.ttest_rel(df['WBN'], df['WBO'], alternative='greater')

print(f"AO vs AN p-value: {test_attractiveness.pvalue}")
print(f"WBO vs WBN p-value: {test_buy.pvalue}")
```

#### Statistische Conclusies

Hypothesestelling:

- **Nulhypothese** ($H_0$): There is no difference in consumer ratings between the traditional style and the new style ($\mu_{\text{New}} \le \mu_{\text{Old}}$).
- **Alternatieve hypothese** ($H_a$): Consumers rate the new style higher than the traditional style ($\mu_{\text{New}} > \mu_{\text{Old}}$).

Op basis van de testresultaten:

- **Attractiveness (AO vs AN)**: With a $p$-value of $1.32 \times 10^{-7}$ (which is far below the standard significance level of $\alpha = 0.05$), we reject the null hypothesis. The attractiveness rating for the new-style can is significantly higher than the traditional can.

- **Likelihood to Buy (WBO vs WBN)**: With a $p$-value of $2.01 \times 10^{-6}$ (also far below $0.05$), we reject the null hypothesis. The consumer likelihood to purchase the product with the new-style can is significantly higher than with the traditional can.

**Business Takeaway**: The focus group data strongly supports making the design shift; the new style is both visually superior to consumers and metrics indicate it will positively drive intent to purchase.

---

## Lab 5.02

### Exercise 2 - Exercise & Productivity

#### Stap 1: Boxplot van beoordelingen per trainingsstatus

First, let's load the data from the provided URL, separate the groups into Exercisers (29 employees) and Non-Exercisers (51 employees), and generate the box plot to visualize their productivity ratings.

```python
import numpy as np
import pandas as pd
import scipy.stats as stats
import matplotlib.pyplot as plt
import seaborn as sns

# Load dataset
url = 'https://raw.githubusercontent.com/HoGentTIN/dsai-labs/main/data/Exercise%20%26%20Productivity.csv'
exercise_facilities = pd.read_csv(url, delimiter=';')

# Create the Box Plot
plt.figure(figsize=(6, 5))
sns.boxplot(x='Exerciser', y='Productivity', data=exercise_facilities, palette='Set2')
plt.title('Worker Productivity: Exercisers vs. Non-Exercisers')
plt.xlabel('Regular Exerciser')
plt.ylabel('Supervisor Rating (1-25)')
plt.grid(axis='y', linestyle='--', alpha=0.7)
plt.show()
```

#### Stap 2: Statistisch testen (Onafhankelijke $t$-test)

Because the 80 employees are distinct individuals divided into two mutually exclusive, independent categories (Exercisers vs. Non-Exercisers), an Independent Two-Sample $t$-test (two-tailed or one-tailed directional) is performed.

Hypothesestelling:

- **Nulhypothese** ($H_0$): Regular exercise does not increase worker productivity ($\mu_{\text{Exerciser}} \le \mu_{\text{Non-Exerciser}}$).
- **Alternatieve hypothese** ($H_a$): Regular exercise increases worker productivity ($\mu_{\text{Exerciser}} > \mu_{\text{Non-Exerciser}}$).

```python
# Separate into the two target groups
exercisers = exercise_facilities[exercise_facilities['Exerciser'] == 'Yes']['Productivity']
non_exercisers = exercise_facilities[exercise_facilities['Exerciser'] == 'No']['Productivity']

# Perform two-sample independent t-test (assuming equal variances)
# We use alternative='greater' because the question asks if exercise *increases* productivity
t_stat, p_val = stats.ttest_ind(exercisers, non_exercisers, alternative='greater')

print(f"t-statistic: {t_stat:.4f}")
print(f"p-value: {p_val:.9f}")
```

**Statistische conclusie**: Since the calculated $p$-value $\approx 0.0063$, which is significantly lower than the standard significance threshold ($\alpha = 0.05$), we safely reject the null hypothesis ($H_0$). There is strong statistical evidence to conclude that the productivity of regular exercisers is significantly higher than that of non-exercisers.

#### Stap 3: Cohen's d berekening (effectgrootte)

While a $p$-value tells us if a significant difference exists, Cohen's $d$ tells us how large the real-world magnitude of that difference is. We compute it using the pooled standard deviation formula:

$$d = \frac{\bar{X}_1 - \bar{X}_2}{s_{\text{pooled}}}$$

Waarbij:

$$s_{\text{pooled}} = \sqrt{\frac{(n_1 - 1)s_1^2 + (n_2 - 1)s_2^2}{n_1 + n_2 - 2}}$$

```python
# Function to calculate Cohen's d
def cohen_d(group1, group2):
    n1, n2 = len(group1), len(group2)
    var1, var2 = np.var(group1, ddof=1), np.var(group2, ddof=1)
    
    # Calculate pooled standard deviation
    pooled_se = np.sqrt(((n1 - 1) * var1 + (n2 - 1) * var2) / (n1 + n2 - 2))
    
    # Calculate d
    return (np.mean(group1) - np.mean(group2)) / pooled_se

# Calculate effect size
d_value = cohen_d(exercisers, non_exercisers)
print(f"Cohen's d: {d_value:.5f}")
```

**Interpretatie van Cohen's $d$**: The calculated value of $0.55509$ falls squarely within the standard conventions for effect interpretation:

- Small effect size: $\approx 0.2$
- Medium/Average effect size: $\approx 0.5$
- Large effect size: $\approx 0.8$

This tells management that installing physical wellness infrastructure produces a clear, moderate-sized practical improvement in average workplace performance metrics.

---

## Lab 5.03

### Exercise - Computer Skills Training

#### Stap 1: Visualisatie & Gepaarde $t$-test

Since the same group of 40 employees took the computer skills test both before and after the training program, the samples are dependent (paired).

Hypothesestelling:

- **Nulhypothese** ($H_0$): The training program does not increase test scores ($\mu_{\text{Post}} \le \mu_{\text{Pre}}$).
- **Alternatieve hypothese** ($H_a$): The training program increases test scores ($\mu_{\text{Post}} > \mu_{\text{Pre}}$).

```python
import numpy as np
import pandas as pd
import scipy.stats as stats
import matplotlib.pyplot as plt
import seaborn as sns

# Load data
url = 'https://raw.githubusercontent.com/HoGentTIN/dsai-labs/main/data/Computer%20Skills.csv'
computer_skills = pd.read_csv(url, delimiter=';')

# 1. Create a Long-format DataFrame for Seaborn's boxplot
df_melted = pd.melt(computer_skills[['Pre', 'Post']], var_name='Test Time', value_name='Score')

plt.figure(figsize=(6, 5))
sns.boxplot(x='Test Time', y='Score', data=df_melted, palette='Pastel1')
plt.title("Computer Skills Test Scores: Before vs. After Training")
plt.xlabel("Timeline")
plt.ylabel("Test Score")
plt.grid(axis='y', linestyle='--', alpha=0.7)
plt.show()

# 2. Perform a Paired Sample t-test (Directional: Post > Pre)
t_stat, p_value = stats.ttest_rel(computer_skills['Post'], computer_skills['Pre'], alternative='greater')

print(f"t-statistic: {t_stat:.4f}")
print(f"p-value: {p_value:.4e}")
```

**Statistische conclusie**: With a calculated $p$-value of approximately $2.2653 \times 10^{-9}$, which is significantly lower than the $5\%$ level of significance ($\alpha = 0.05$), $H_0$ is rejected. The sample metrics confidently back the claim that the intensive training program successfully elevates the computing knowledge base of new personnel.

#### Stap 2: Cohen's $d$ berekening

Cohen's $d$ captures the degree of shift normalized against the total shared variability. Because this is a paired design, Cohen's $d$ for dependent samples is calculated directly as:

$$d = \frac{\overline{x}_{\text{Post}} - \overline{x}_{\text{Pre}}}{s_{\text{diff}}}$$

Waarbij $s_{\text{diff}}$ de standaardafwijking van de individuele verschillen ($x_{\text{Post}} - x_{\text{Pre}}$) is.

```python
# Compute differences
differences = computer_skills['Post'] - computer_skills['Pre']

# Calculate mean of differences and sample standard deviation of differences
mean_diff = differences.mean()
std_diff = differences.std(ddof=1)

cohen_d = mean_diff / std_diff
print(f"Cohen's d (Paired): {cohen_d:.3f}")
```

**Resultaat**: 1.619

**Interpretatie**: A value of $1.619$ is substantially above the benchmark for a "large" effect size ($0.8$). It highlights an exceptionally large practical benefit, meaning the typical employee saw a vast leap up the performance curve relative to random baseline drift.

#### Stap 3: Glass's Delta ($\Delta$) berekening

Glass's Delta is specifically selected when an experimental process changes the variance of the group after treatment. It standardizes the mean shift exclusively by the standard deviation of the base control state (the Pretraining conditions in this scenario):

$$\Delta = \frac{|\overline{x}_{\text{Post}} - \overline{x}_{\text{Pre}}|}{s_{\text{Pre}}}$$

Waarbij $s_{\text{Pre}}$ de steekproefstandaardafwijking van de basismeting voor interventie is.

```python
# Compute means
mean_pre = computer_skills['Pre'].mean()
mean_post = computer_skills['Post'].mean()

# Compute sample standard deviation of the control group (Pre)
std_pre = computer_skills['Pre'].std(ddof=1)

# Calculate Glass's Delta
glass_delta = abs(mean_post - mean_pre) / std_pre
print(f"Glass's delta: {glass_delta:.3f}")
```

**Resultaat**: 1.241

**Interpretatie**: Glass's delta indicates that the training group's post-test mean is more than $1.2$ control standard deviations higher than the baseline control mean, cross-verifying a highly resilient improvement index independent of any variance distortion introduced during training.

---

## Lab 5.04

### Exercise - Test Results Analysis (Multi-session Comparison)

#### Stap 1: Beschrijvende statistieken en verkennende data-analyse

First, we must load the data, filter out absent students (missing values), and compute the summary parameters (mean, median, standard deviation, and variance) for the entire pool as well as per session.

```python
import numpy as np
import pandas as pd
import scipy.stats as stats
import matplotlib.pyplot as plt
import seaborn as sns

# Load dataset and clean missing values (absent students)
df = pd.read_csv('test-results.csv').dropna(subset=['score'])

# Overall descriptive statistics
print("--- Overall Statistics ---")
print(f"Mean: {df['score'].mean():.2f}")
print(f"Median: {df['score'].median():.2f}")
print(f"Standard Deviation: {df['score'].std():.2f}\n")

# Per session statistics
print("--- Per Session Statistics ---")
session_stats = df.groupby('session')['score'].agg(['mean', 'median', 'std', 'count'])
print(session_stats)
```

#### Stap 2: Staafdiagram met foutenstaven

This visualization plots the mean of each session alongside an error bar denoting $\pm 1$ standard deviation ($s$), showing the dispersion across groups.

```python
plt.figure(figsize=(9, 5))
# Calculate means and standard deviations per session group
means = df.groupby('session')['score'].mean()
stds = df.groupby('session')['score'].std()

plt.bar(means.index, means.values, yerr=stds.values, capsize=5, color='skyblue', edgecolor='black', alpha=0.8)
plt.title('Average Test Score per Session (with ±1 SD Error Bars)')
plt.xlabel('Session ID')
plt.ylabel('Average Score')
plt.ylim(0, 40)
plt.grid(axis='y', linestyle='--', alpha=0.5)
plt.show()
```

#### Stap 3 & 4: Boxplots en twee-steekproeven $t$-testen

To verify whether later groups systematically outperform earlier groups, we plot a boxplot broken down by session and calculate one-sided independent two-sample $t$-tests.

```python
# Create Boxplot
plt.figure(figsize=(10, 6))
sns.boxplot(x='session', y='score', data=df, order=sorted(df['session'].unique()), palette='Set3')
plt.title('Distribution of Test Scores Divided per Session Group')
plt.xlabel('Session')
plt.ylabel('Score (Max 40)')
plt.grid(axis='y', linestyle='--', alpha=0.5)
plt.show()

# Function to run directional independent t-tests (H0: mean1 >= mean2 vs Ha: mean1 < mean2)
def run_one_sided_ttest(g1, g2, label1, label2):
    scores1 = df[df['session'] == g1]['score']
    scores2 = df[df['session'] == g2]['score']
    # stats.ttest_ind computes t-stat for (scores1 - scores2)
    t_stat, p_two_tailed = stats.ttest_ind(scores1, scores2, equal_var=False)
    
    # Transform to a one-sided p-value for Alternative Hypothesis: mean1 < mean2
    if t_stat < 0:
        p_one_sided = p_two_tailed / 2
    else:
        p_one_sided = 1 - (p_two_tailed / 2)
        
    print(f"Test {label1} < {label2}: p-value = {p_one_sided:.5f}")

print("--- One-sided Two-Sample t-test Results ---")
run_one_sided_ttest('A', 'B', 'μ_A', 'μ_B')
run_one_sided_ttest('C', 'D', 'μ_C', 'μ_D')
run_one_sided_ttest('E', 'D', 'μ_E', 'μ_D')
run_one_sided_ttest('F', 'H', 'μ_F', 'μ_H')
run_one_sided_ttest('G', 'H', 'μ_G', 'μ_H')
run_one_sided_ttest('C', 'H', 'μ_C', 'μ_H')
run_one_sided_ttest('A', 'H', 'μ_A', 'μ_H')
```

**Analytische conclusies**:

- **Sessions A vs B**: The observed mean increases from $13.1$ to $17.2$, but with a $p$-value of $\approx 0.0536 > 0.05$, the difference is not statistically significant at the $5\%$ level.

- **Sessions C, D, and E**: Session D ($22.5$) is significantly higher than C ($18.8$) and E ($18.9$). However, because session E took place after session D, the decrease in score rules out information leakage as the driver of the variance.

- **Sessions F, G, and H**: The differences across Day 3 are minor and statistically insignificant ($p > 0.05$).

- **Sessions C vs H**: These represent the absolute first and last sessions on the same main campus. Even with max time available for leaking test material, the variance yields a $p$-value of $0.1463$, indicating no significant leakage effect.

- **Sessions A vs H**: This contrast is highly significant ($p = 0.0003$). However, since they were hosted on entirely different campuses, the disparity is far more likely caused by group skill composition or environment differences rather than information routing.

#### Stap 5: Vergelijking van multi-groepvariaties (ANOVA)

When expanding our assessment to evaluate differences among more than two groups concurrently without inflating our Type I error rate (the false positive rate), running multiple $t$-tests becomes problematic. The appropriate overarching statistical framework is a One-Way Analysis of Variance (ANOVA).

**Core Assumptions of One-way ANOVA**:

- **Independence**: Observations within and between groups are completely independent.
- **Normality**: The dependent variable residuals are normally distributed within each category.
- **Homoscedasticity**: Variances across all group populations are approximately equal.

```python
# Extract scores for each individual session group dynamically
session_groups = [df[df['session'] == s]['score'] for s in sorted(df['session'].unique())]

# Perform One-Way ANOVA
f_stat, anova_p = stats.f_oneway(*session_groups)

print("--- One-Way ANOVA Evaluation ---")
print(f"F-statistic: {f_stat:.4f}")
print(f"p-value: {anova_p:.8e}")
```

**Interpretatie**: The ANOVA tests the null hypothesis ($H_0: \mu_A = \mu_B = ... = \mu_H$) against the alternative hypothesis that at least one session's true performance mean stands apart. Given that our individual pairwise tests yielded an exceptionally low $p$-value elsewhere (such as A vs H), the global ANOVA $p$-value will drop well below $\alpha = 0.05$.

This rejects $H_0$ globally, proving that statistically significant differences do exist across the timeline, though our sequential structural breakdowns indicate these variance trends do not map back cleanly to chronological information leak dependencies.