Add a live demo prediction cell to SEHS4696_GP.ipynb for use during the presentation.

Steps:
1. Read SEHS4696_GP.ipynb to get the full list of one-hot encoded feature columns in X
2. Create a new notebook cell at the end that:
   - Defines a plain-English job profile dict (e.g. job_title, experience_years, education_level, etc.)
   - Converts it into a one-hot encoded DataFrame aligned to X.columns using reindex(fill_value=0)
   - Runs `best_model.predict()` on it
   - Prints the result formatted as: "Predicted Salary: $XX,XXX"
3. The sample profile should use $ARGUMENTS if provided (e.g. "Data Scientist, 5 years, Master, Finance")
   Otherwise default to: AI Engineer, 8 years experience, Master's degree, Tech industry, Large company

This cell is intended to be executed live during the 20-25 minute presentation demo.
