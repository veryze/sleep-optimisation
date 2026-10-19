# Sleep Behaviour and Fatigue Analysis

> Predicting next-day fatigue from sleep, screen time, and lifestyle factors using linear regression and random forest

---

## Project Overview

This project investigates which behavioural factors have the strongest impact on next-day fatigue when sleep is reduced. Using a dataset of 8,500 individuals with sleep, lifestyle, and fatigue measurements, we build and evaluate linear regression and random forest models to identify actionable insights for minimising fatigue under sleep restriction.

### Research Question

> **What is the optimal behaviour to minimise fatigue with reduced sleep?**

### Hypothesis

Reducing screentime, exercise before sleep, screen brightness, caffeine intake, and increasing sleep hours, and snoozes should help minimise fatigue.

This question is relevant because more and more younger students are having worse sleep, and sleep debt can result in poor cognitive performance, increased sleepiness, poor mood, and a higher risk for accidents. It also significantly affects academics. While we are often given advice on how to sleep better, I wondered which factors have the highest impact on next-day fatigue, as well as how to minimise it.

https://academic.oup.com/sleepadvances/article/4/1/zpac044/6854927

https://pmc.ncbi.nlm.nih.gov/articles/PMC10900033/

---

## Dataset

| Attribute | Details |
|-----------|---------|
| **Source** | https://www.kaggle.com/datasets/samartalwar/sleep-debt-and-screen-time-late-night-phone-habits |
| **Size** | 8,500 samples × 18 features |
| **Target** | `next_day_fatigue_score` (0–10 scale, step = 0.1) |
| **Features** | Sleep metrics, screen time, caffeine, activity |

### Features Used

| Category | Features |
|----------|----------|
| **Sleep Behaviour** | `total_sleep_hours`, `sleep_latency_min`, `deep_sleep_pct`, `rem_sleep_pct`, `morning_alarm_snoozes` |
| **Screen Use** | `bedtime_phone_minutes`, `screen_brightness_pct`, `blue_light_filter_active` |
| **Lifestyle** | `caffeine_post_5pm_mg`, `physical_activity_min` |

Other features such as demographics (age/careers of participants) were included in the dataset but not used for this project.

---

## Methodology

### Preprocessing

- **Normalisation**: Z-score standardisation of all features
- **Train/Test Split**: 60% train / 40% test (random_state=42)

### Models

| Component | Choice |
|-----------|--------|
| **Algorithm 1** | Linear Regression (OLS) |
| **Algorithm 2** | Random Forest Regressor (100 estimators) |
| **Features** | 10 numeric features |
| **Target** | `next_day_fatigue_score` |
| **Metrics** | MSE, R² |

### Evaluation

**Linear Regression:**
- **Test MSE**: 0.6521
- **Test R²**: 0.9095
- **Validation**: Train/test split with visualisation

**Random Forest:**
- **Test MSE**: 0.3637
- **Test R²**: 0.9495
- **Validation**: Train/test split with feature importance analysis

---

## Results

### Model Performance

**Linear Regression:**

| Metric | Value |
|--------|-------|
| **MSE** | 0.6521 |
| **R² Score** | **0.9095** (great fit) |

**Random Forest:**

| Metric | Value |
|--------|-------|
| **MSE** | 0.3637 |
| **R² Score** | **0.9495** (excellent fit) |

**Random Forest outperforms Linear Regression** with lower MSE and higher R².

### Feature Coefficients (Linear Regression - Standardised)

| Feature | Coefficient | Interpretation |
|---------|-------------|----------------|
| `total_sleep_hours` | **−1.390** | **Strongest reducer** of fatigue |
| `morning_alarm_snoozes` | +0.638 | *Contrary to hypothesis: More snoozes → more fatigue* |
| `sleep_latency_min` | +0.541 | Longer time to fall asleep → more fatigue |
| `bedtime_phone_minutes` | +0.206 | More screen time → more fatigue |
| `caffeine_post_5pm_mg` | +0.200 | Evening caffeine → more fatigue |
| `deep_sleep_pct` | −0.201 | More deep sleep → less fatigue |
| `screen_brightness_pct` | +0.107 | Brighter screen → more fatigue |
| `blue_light_filter_active` | −0.072 | Filter ON → less fatigue |
| `rem_sleep_pct` | −0.008 | Near-zero effect |
| `physical_activity_min` | **+0.011** | *Contrary to hypothesis: more activity → slightly more fatigue* |

### Random Forest Feature Importances

| Feature | Importance | Rank |
|---------|------------|------|
| `morning_alarm_snoozes` | 0.481 | 1 |
| `total_sleep_hours` | 0.388 | 2 |
| `sleep_latency_min` | 0.082 | 3 |
| `deep_sleep_pct` | 0.015 | 4 |
| `caffeine_post_5pm_mg` | 0.010 | 5 |
| `screen_brightness_pct` | 0.007 | 6 |
| `bedtime_phone_minutes` | 0.006 | 7 |
| `rem_sleep_pct` | 0.005 | 8 |
| `physical_activity_min` | 0.005 | 9 |
| `blue_light_filter_active` | 0.001 | 10 |

*According to the linear regression model, total sleep hours has the highest importance followed by morning alarm snoozes and sleep latency, but in the random forest model, morning alarm snoozes has the highest importance.*

### Linear Regression Model Equation

```
fatigue = 3.805 (intercept)
        + 0.206 × bedtime_phone_min
        + 0.107 × screen_brightness
        − 0.072 × blue_light_filter
        + 0.200 × caffeine_post_5pm
        + 0.011 × physical_activity
        + 0.541 × sleep_latency
        − 1.390 × total_sleep_hours
        − 0.201 × deep_sleep_pct
        − 0.008 × rem_sleep_pct
        + 0.638 × morning_snoozes
```

---

### kFold Five-fold Cross Validation

| Fold | MSE | R2 Score |
|---------|------------|------|
| 1 | 0.352793 | 0.951030 |
| 2 | 0.363634 | 0.952443 |
| 3 | 0.346484 | 0.951567 |
| 4 | 0.407203 | 0.941751 |
| 5 | 0.378294 | 0.946110 |

**Mean CV MSE**: 0.3697 (+/- 0.0216)
**Mean CV R2 Score**: 0.9486 (+/- 0.0041)

---

## Key Insights and Recommendations

### High-Impact Interventions

| Priority | Action | Expected Fatigue Reduction |
|----------|--------|---------------------------|
| 1 | **Increase total sleep hours** | **High** (−1.39 per hour) |
| 2 | **Reduce snoozing** | **High** (−0.64 per snooze) |
| 3 | **Reduce sleep latency** | **Medium** (−0.54 per minute) |
| 4 | **Increase deep sleep percentage** | **Medium** |
| 5 | **Reduce bedtime phone use** | **Low–Medium** |
| 6 | **Avoid evening caffeine** | **Low–Medium** |
| 7 | **Enable blue light filter** | **Low** |

### Counter-Intuitive Findings

| Factor | Hypothesis | Actual Finding |
|--------|------------|----------------|
| Morning snoozes | Should reduce fatigue | **Increases fatigue** (+0.64/snooze) |
| Physical activity | Should reduce fatigue | **Slightly increases fatigue** (+0.011/min) |

### Trade-off Analysis

Under **optimal conditions** (all other factors optimal):
- To achieve **Fatigue Score 1.0**: Need **5.71 hours** sleep
- To achieve **Fatigue Score 2.0**: Need **4.67 hours** sleep
- To achieve **Fatigue Score 3.0**: Need **3.63 hours** sleep
- To achieve **Fatigue Score 4.0**: Need **3.20 hours** sleep
- **Beyond 4 hours**: Diminishing returns (plateau at ~3.2 hours minimum)

---

### Results

Ultimately, my hypothesis was partially right - reducing screentime, screen brightness, caffeine intake, and increasing sleep hours did help minimise fatigue. However, contrary to my assumption, exercise before sleeping had a positive correlation with fatigue reduction, and snoozing in the morning had a negative correlation, although these correlations were relatively weak. 

---

## Repository Structure

```
sleep-fatigue-analysis/
├── Project.ipynb          # Main analysis notebook
├── bedtime_screentime_sleep_debt.csv  # Dataset
├── README.md                
├── requirements.txt         # Dependencies
```

---

## References

1. [Sleep Advances — Sleep and fatigue](https://academic.oup.com/sleepadvances/article/4/1/zpac044/6854927)
2. [PMC — Sleep and cognitive performance](https://pmc.ncbi.nlm.nih.gov/articles/PMC10900033/)
3. [scikit-learn Linear Regression](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html)
4. [scikit-learn Random Forest](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html)

---

## Author

**Mana Ganesh**  
[GitHub](https://github.com/veryze)

---

## Acknowledgements

- Dataset providers
- scikit-learn, pandas, matplotlib communities
- Prior sleep research cited above
