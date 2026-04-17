Generate a complete EDA (Exploratory Data Analysis) section for the notebook targeting the dataset and column specified.

Steps to perform:
1. Read the current state of SEHS4696_GP.ipynb to understand what EDA already exists
2. Add the following EDA cells after the dataset load cell (before preprocessing):
   - `dataset.head(10)` with a markdown cell explaining each column
   - `dataset.describe()` for summary statistics
   - Salary distribution: `sns.histplot(dataset['salary'], bins=50, kde=True)`
   - Correlation heatmap of numerical columns only
   - Bar chart: mean salary grouped by `education_level`
   - Bar chart: mean salary grouped by `industry`
   - Box plot: salary vs `experience_years`

Use the argument `$ARGUMENTS` as the target column to focus analysis on (default: salary).

Output the new notebook cells as properly formatted JSON notebook cell objects ready to insert into the .ipynb file.
