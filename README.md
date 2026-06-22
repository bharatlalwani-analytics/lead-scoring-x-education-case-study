# Lead Scoring Model — X Education Case Study

A Logistic Regression model to identify high-probability leads ("Hot Leads") for X Education, an online course provider. The model assigns a Lead Score (0–100) to each incoming lead, enabling the sales team to prioritise outreach and achieve the CEO's target of ~80% conversion rate.

---

## Problem Statement

X Education generates a large volume of leads daily but converts only ~30% of them. The sales team spends equal effort on every lead regardless of conversion likelihood, resulting in wasted resources and missed revenue. The goal was to build a model that scores each lead so the team focuses only on the most promising ones.

---

## Dataset

- 9,240 leads | 37 raw features | 17 final features after selection
- Target variable: `Converted` (1 = enrolled, 0 = not enrolled)
- Baseline conversion rate: **38.5%**

---

## Project Structure

## Approach

### 1. Data Cleaning & EDA
- Removed columns with >35% missing values
- Treated `"Select"` (skipped form fields) as null → replaced with NaN
- Numeric columns imputed with median; categorical with mode
- Dropped identifier columns (Prospect ID, Lead Number)
- Excluded `Lead Quality` and `Tags` to prevent data leakage (assigned post-interaction)

### 2. Feature Engineering
- Mapped 14 Yes/No binary columns to 0/1
- Capped high-cardinality categoricals to top-7 categories + `"Other"` bucket
- One-hot encoded all categorical features with `drop_first=True`

### 3. Feature Selection (3-Stage Pipeline)

| Stage | Method | Features In | Features Out |
|-------|--------|-------------|--------------|
| 1 | L1-regularised Logistic Regression (C=0.1) | 109 | 23 |
| 2 | Iterative p-value pruning (threshold > 0.05) | 23 | 20 |
| 3 | VIF pruning (threshold > 5.0) + p-value check | 20 | 17 |

### 4. Model
- Algorithm: **Logistic Regression** (scikit-learn, C=1.0)
- Train-test split: 70/30 stratified
- Scaling: StandardScaler on numeric features

---

## Results

| Metric | Value |
|--------|-------|
| AUC-ROC | **0.8737** |
| Accuracy @ threshold 0.50 | 81.0% |
| Sensitivity @ threshold 0.30 | **83.8%** |
| Specificity @ threshold 0.30 | 75.4% |

### Lead Score Tiers

| Tier | Score Range | Count | Actual Conversion Rate |
|------|-------------|-------|------------------------|
| 🔥 Hot Lead | 70 – 100 | ~2,300 | ~87% |
| 🌡️ Warm Lead | 40 – 69 | ~2,100 | ~52% |
| ❄️ Cold Lead | 0 – 39 | ~4,840 | ~11% |

---

## Top Predictive Features

| Feature | Odds Ratio | Direction |
|---------|------------|-----------|
| Lead Origin: Lead Add Form | 30.4x | ✅ Highest intent signal |
| Occupation: Working Professional | 15.7x | ✅ Strongest motivation to enrol |
| Lead Source: Welingak Website | 9.8x | ✅ High-quality referral |
| Last Notable Activity: SMS Sent | 7.4x | ✅ Recent engagement |
| Total Time Spent on Website | 3.0x | ✅ Higher interest |
| Do Not Email = Yes | 0.20x | ❌ Opted out |
| Lead Profile: Student of SomeSchool | 0.05x | ❌ Converts very rarely |

---

## Business Recommendations

| Phase | Threshold | Action |
|-------|-----------|--------|
| Normal operations | 0.50 | Call leads with Score ≥ 50 — balanced precision & recall |
| Aggressive (intern phase) | 0.30 | Call leads with Score ≥ 30 — sensitivity rises to 83.8% |
| Conservative (post-target) | 0.55–0.60 | Call only Score ≥ 55 — minimise wasted calls |

- Prioritise **Lead Add Form** as primary acquisition channel
- Target **working professionals** in marketing campaigns
- Follow up on SMS-engaged leads **within 24 hours**

---

## Tech Stack

![Python](https://img.shields.io/badge/Python-3.x-blue)
![Pandas](https://img.shields.io/badge/Pandas-✓-lightgrey)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-✓-orange)
![Statsmodels](https://img.shields.io/badge/Statsmodels-✓-lightgrey)
![Matplotlib](https://img.shields.io/badge/Matplotlib-✓-blue)
![Seaborn](https://img.shields.io/badge/Seaborn-✓-teal)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange)

---

## Author

**Bharat Lalwani**  
[LinkedIn](https://www.linkedin.com/in/bharatlalwani-analytics) • [GitHub](https://github.com/bharatlalwani-analytics)
