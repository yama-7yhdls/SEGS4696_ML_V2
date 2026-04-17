Generate or update the model comparison section in SEHS4696_GP.ipynb.

Steps:
1. Read SEHS4696_GP.ipynb to find existing model predictions (lr_pred, rf_pred, gb_pred)
2. Ensure a results DataFrame is built with columns: Model, R-squared (R²), MAE
3. Add a visual bar chart comparing R² scores across all models using seaborn
4. Add a visual bar chart comparing MAE across all models
5. Print a plain-English conclusion cell identifying the best model and why
6. If $ARGUMENTS specifies an additional model name (e.g. "XGBoost"), scaffold the training and evaluation cells for that model too

Models currently in the notebook: Linear Regression, Random Forest, Gradient Boosting.

Output ready-to-insert notebook cell JSON or edit the file directly.
