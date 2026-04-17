# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Context

This is a SEHS4696 (Machine Learning for Data Mining) group project at Hong Kong Polytechnic University. The goal is to predict job salaries using ML regression models on `job_salary_prediction_dataset.csv` (250,000 rows). The primary deliverable is `SEHS4696_GP.ipynb`, designed to run in **Google Colab**.

## Running the Notebook

This notebook is intended for Google Colab, not local execution. The first runnable cell must be:

```python
from google.colab import drive
drive.mount('/content/drive')
```

The dataset path in `read_csv` must point to Google Drive: `/content/drive/MyDrive/job_salary_prediction_dataset.csv`.

To run locally for testing, replace the drive mount and path with a local path to `job_salary_prediction_dataset.csv`.

## Architecture & Flow

The notebook follows a strict linear pipeline — cells must be run top to bottom:

1. **Imports** — all libraries loaded once at the top
2. **Drive mount + data load** — mount must precede read_csv (current bug: they are swapped)
3. **EDA** — exploration before any transformation
4. **Preprocessing** — drop columns → null check → one-hot encode categorical features (`job_title`, `education_level`, `industry`, `company_size`, `location`) → split X/y → train_test_split (60/40)
5. **Model training** — three models trained on the same `X_train`/`y_train`: Linear Regression, Random Forest (100 estimators), Gradient Boosting (100 estimators, lr=0.1)
6. **Evaluation** — R² and MAE for each model; comparison scorecard DataFrame
7. **Feature importance** — from best model (Gradient Boosting)
8. **Live demo cell** — predict salary for a manually constructed sample input

## Key Design Decisions

- `remote_work` column is dropped before encoding (currently undocumented — add justification if kept or removed)
- One-hot encoding uses `drop_first=True` to avoid multicollinearity
- Train/test split is 60/40 (not the typical 80/20) — justified by the large dataset size (100k test samples is still substantial)
- Evaluation metrics: R² (explained variance) and MAE (interpretable dollar error)
- No `StandardScaler` applied — tree models don't require it; Linear Regression could benefit

## Known Issues

- **Cell order bug**: `drive.mount()` cell is positioned after the `read_csv` cell — must be moved to execute first
- **Missing cells**: EDA visualizations (salary distribution, correlation heatmap) and a new-sample prediction demo cell are not yet implemented (see `CodePlan.md`)

## Project Files

| File | Purpose |
|------|---------|
| `SEHS4696_GP.ipynb` | Main notebook — all code here |
| `job_salary_prediction_dataset.csv` | Source dataset (local copy; upload to Google Drive for Colab) |
| `GroupProject_Tasks.md` | Full task checklist derived from project instructions |
| `CodePlan.md` | Detailed per-cell implementation plan with gaps and enhancements |
| `GroupProject_Instructions_updated.pdf` | Original assignment brief |

## Submission

Submit `SEHS4696_GP.ipynb` and the PPT to Blackboard by **30 April 2026**.
