Audit and fix structural issues in SEHS4696_GP.ipynb.

Steps:
1. Read SEHS4696_GP.ipynb
2. Check and fix the following known issues:
   - Cell order: ensure `drive.mount()` cell appears BEFORE the `pd.read_csv()` cell
   - Remove duplicate imports (currently pandas, matplotlib imported twice)
   - Ensure all model predictions (lr_pred, rf_pred, gb_pred) are computed before the comparison cell
3. Reorder cells in the JSON structure to match this canonical pipeline order:
   1. Imports
   2. Drive mount
   3. Load dataset + display shape & columns
   4. EDA (head, describe, plots)
   5. Preprocessing (drop, encode, split X/y, train_test_split)
   6. Model 1: Linear Regression
   7. Model 2: Random Forest
   8. Model 3: Gradient Boosting
   9. Model comparison scorecard + bar charts
   10. Feature importance
   11. Live demo prediction cell
4. Write the corrected notebook back to SEHS4696_GP.ipynb

Do not change any code logic — only fix order and remove exact duplicate import lines.
