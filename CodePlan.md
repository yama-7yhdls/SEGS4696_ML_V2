# Code File Plan — Job Salary Prediction
**Dataset:** `job_salary_prediction_dataset.csv`  
**Task Type:** Regression (predict continuous salary value)  
**Notebook:** `SEHS4696_GP.ipynb` (Google Colab)

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
- [ ] Bar chart: average salary by `job_title` / `education_level` / `industry`

---

### Cell 4 — Preprocessing
**Status: Partially done** — only drops `remote_work` and one-hot encodes. Missing:

- [ ] **Justify** dropping `remote_work` (or reconsider keeping it — it is a valid feature)
- [ ] One-hot encoding for all categorical columns — Done
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

Demonstrate the model with a custom input — important for the live demo.

```python
# Example: predict salary for a new job profile
new_sample = pd.DataFrame([{
    'experience_years': 5,
    'skills_count': 10,
    'certifications': 1,
    'job_title_Data Analyst': 1,
    'education_level_Master': 1,
    'industry_Finance': 1,
    'company_size_Large': 1,
    'location_USA': 1,
    # ... all other one-hot columns set to 0
}])
new_sample = new_sample.reindex(columns=X.columns, fill_value=0)
predicted_salary = best_model.predict(new_sample)
print(f"Predicted Salary: ${predicted_salary[0]:,.2f}")
```

---

## Summary of Actions Required

| Priority | Task | Status |
|----------|------|--------|
| High | Fix cell order: move drive.mount() before read_csv | Bug |
| High | Add EDA plots (salary distribution, correlation heatmap) | Missing |
| High | Add sample prediction cell for live demo | Missing |
| Medium | Justify dropping `remote_work` or keep it as a feature | Incomplete |
| Medium | Add model comparison bar chart | Enhancement |
| Medium | Add StandardScaler for Linear Regression | Enhancement |
| Low | Add inline comments explaining each preprocessing step | Polish |

---

## Models Chosen & Justification (for PPT)

| Model | Why |
|-------|-----|
| **Linear Regression** | Baseline — simple, interpretable, fast |
| **Random Forest** | Handles non-linearity, robust to outliers, no scaling needed |
| **Gradient Boosting** | Usually highest accuracy on tabular data, sequential error correction |

**Expected best model:** Gradient Boosting (generally outperforms on structured/tabular data)

---

## Evaluation Metrics

| Metric | What it measures |
|--------|-----------------|
| **R² (R-squared)** | % of salary variance explained by the model (higher = better, max 1.0) |
| **MAE (Mean Absolute Error)** | Average dollar error in predictions (lower = better) |
