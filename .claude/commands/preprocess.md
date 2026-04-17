Audit and improve the preprocessing section of SEHS4696_GP.ipynb.

Steps:
1. Read SEHS4696_GP.ipynb
2. Identify and report:
   - Missing value counts per column
   - Columns dropped and whether dropping is justified
   - Categorical columns and how they are encoded
   - Whether numerical features are scaled (and whether scaling is needed for the models used)
   - Train/test split ratio and whether it is appropriate for the dataset size
3. Suggest and optionally apply improvements:
   - Add `StandardScaler` for Linear Regression if not present
   - Ensure `drive.mount()` cell comes before `read_csv`
   - Document each preprocessing step with a one-line comment explaining WHY

Target column (label): $ARGUMENTS (default: salary)
