# Code File Plan — Job Salary Prediction
**Dataset:** `job_salary_prediction_dataset.csv`  
**Task Type:** Regression (predict salary to support a recruitment agency's salary range advisory service)  
**Notebook:** `SEHS4696_GP.ipynb` (Google Colab)

**Real-world goal:** Help a recruitment agency quote accurate salary ranges per industry, for both employers (budgeting) and job seekers (negotiation benchmarks). The model output should be presented as a range (predicted ± MAE), not a single number.

---

## Dataset Overview

| Property | Detail |
|----------|--------|
| Rows | 250,000 |
| Target | `salary` (continuous) |
| Categorical features | `job_title`, `education_level`, `industry`, `company_size`, `location`, `remote_work` |
| Numerical features | `experience_years`, `skills_count`, `certifications` |

---

## Notebook Structure Plan

### Cell 1 — Imports
**Status: Done**
- pandas, numpy, matplotlib, seaborn
- sklearn: LinearRegression, RandomForestRegressor, GradientBoostingRegressor, train_test_split, metrics

---

### Cell 2 — Mount Google Drive & Load Dataset
**Status: Needs fix** — drive mount cell must come BEFORE the read_csv cell.

```
Fix: Move drive.mount() to the top (before pd.read_csv)
```

---

### Cell 3 — Dataset Exploration (EDA)
**Status: Partially done** — only prints columns and shape. Missing:

- [ ] `dataset.head(10)` — show sample rows for PPT
- [ ] `dataset.describe()` — summary statistics
- [ ] `dataset.dtypes` — data types
- [ ] `dataset.isnull().sum()` — null check (already done, keep)
- [ ] Salary distribution plot (`sns.histplot`)
- [ ] Correlation heatmap of numerical features
- [ ] Bar chart: **average salary by `industry`** — primary dimension for the recruitment agency use case
- [ ] Bar chart: average salary by `education_level`
- [ ] Bar chart: average salary by `remote_work` — shows impact of work arrangement on salary (key insight for the agency)

---

### Cell 4 — Preprocessing
**Status: Partially done** — incorrectly drops `remote_work` and one-hot encodes remaining columns. Missing:

- [ ] **Restore `remote_work`** — remote/hybrid/on-site directly affects salary and is a key variable for the recruitment agency use case. Do NOT drop it.
- [ ] One-hot encoding for all categorical columns including `remote_work` — update after restoring it
- [ ] Check for and handle any remaining nulls after encoding
- [ ] (Optional) `StandardScaler` on numerical columns to improve Linear Regression performance

```python
# Suggested addition for Linear Regression:
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

---

### Cell 5 — Train/Test Split
**Status: Done**
- 60% train / 40% test
- Consider noting why 60/40 was chosen (large dataset, so 40% test is still 100k samples)

---

### Cell 6 — Model 1: Linear Regression
**Status: Done**
- Train, predict, scatter plot (Actual vs Predicted)
- R² and MAE reported
- Top 10 coefficients shown

---

### Cell 7 — Model 2: Random Forest Regressor
**Status: Done**
- 100 estimators
- Actual vs Predicted + Residual plot
- R² and MAE reported

---

### Cell 8 — Model 3: Gradient Boosting Regressor
**Status: Done**
- 100 estimators, learning_rate=0.1
- Actual vs Predicted + Residual plot
- R² and MAE reported

---

### Cell 9 — Model Comparison
**Status: Done**
- Scorecard DataFrame ranked by R² and MAE

**Suggested addition:**
- [ ] Bar chart comparing R² across all 3 models visually

```python
plt.figure(figsize=(8, 5))
sns.barplot(x='Model', y='R-squared (R²)', data=results_df, palette='Blues_d')
plt.title('Model Comparison — R² Score')
plt.ylim(0, 1)
plt.show()
```

---

### Cell 10 — Feature Importance (Best Model)
**Status: Done**
- Top 10 features bar chart from Gradient Boosting

---

### Cell 11 — Predict on New Sample (MISSING — Add This)
**Status: Not implemented**

Demonstrate the model with a custom input — important for the live demo. Output must be a **salary range** to match the recruitment agency use case (not just a single number).

```python
# Example: recruitment agency queries salary range for a candidate profile
new_sample = pd.DataFrame([{
    'experience_years': 5,
    'skills_count': 10,
    'certifications': 1,
    'job_title_Data Analyst': 1,
    'education_level_Master': 1,
    'industry_Finance': 1,
    'company_size_Large': 1,
    'location_USA': 1,
    'remote_work_Hybrid': 1,
    # ... all other one-hot columns set to 0
}])
new_sample = new_sample.reindex(columns=X.columns, fill_value=0)
predicted_salary = best_model.predict(new_sample)[0]

# Present as a range using MAE as margin — matches agency's advisory use case
print(f"Predicted Salary Range: ${predicted_salary - gb_mae:,.0f} – ${predicted_salary + gb_mae:,.0f}")
print(f"(Point estimate: ${predicted_salary:,.0f}, margin ±${gb_mae:,.0f} MAE)")
```

---

## Summary of Actions Required

| Priority | Task | Status |
|----------|------|--------|
| High | Fix cell order: move drive.mount() before read_csv | Bug |
| High | Restore `remote_work` column — do NOT drop it | Bug |
| High | Add EDA plots: salary by industry, by remote_work, distribution | Missing |
| High | Add salary range prediction cell (point ± MAE) for live demo | Missing |
| Medium | Add model comparison bar chart | Enhancement |
| Medium | Add StandardScaler for Linear Regression | Enhancement |
| Low | Add inline comments explaining each preprocessing step | Polish |

---

## Models Chosen & Justification (for PPT)

| Model | Why |
|-------|-----|
| **Linear Regression** | Baseline — simple, interpretable; coefficients directly show salary drivers per feature (useful for agency to explain to clients) |
| **Random Forest** | Handles non-linearity across industries; robust to outliers in salary data; no scaling needed |
| **Gradient Boosting** | Highest accuracy on tabular data; captures complex interactions (e.g. industry × experience × remote_work) |

**Expected best model:** Gradient Boosting — salary is driven by complex interactions between industry, role, location, and work arrangement, which boosting handles well.

**Output framing for agency use:** Present final predictions as `predicted ± MAE` to give employers a budgeting range and job seekers a negotiation band.

---

## Evaluation Metrics

| Metric | What it measures |
|--------|-----------------|
| **R² (R-squared)** | % of salary variance explained by the model (higher = better, max 1.0) |
| **MAE (Mean Absolute Error)** | Average dollar error in predictions (lower = better) |
