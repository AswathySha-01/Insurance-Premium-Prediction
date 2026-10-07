# Insurance Premium Prediction

A machine learning project for predicting **Annual Premium Amount** using customer demographic, income, insurance, and related features.

## Project Overview

This project covers the complete workflow used in the uploaded Colab notebook:

1. Load the insurance premium dataset from Excel.
2. Inspect the dataset and identify missing values and duplicates.
3. Handle missing categorical values using the mode.
4. Correct invalid dependent-count values.
5. Detect and treat selected outliers in `Age` and `Income_Lakhs`.
6. Explore numerical and categorical features using visualizations.
7. Create a normalized medical-risk score from the `Medical History` field.
8. Encode categorical features into numerical form.
9. Analyze feature relationships using a correlation heatmap.
10. Separate features (`X`) and the target (`y`).
11. Apply Min-Max feature scaling to selected numerical/encoded features.
12. Check multicollinearity using VIF (Variance Inflation Factor).
13. Remove `Income_Level` from the modeling features after the VIF analysis.
14. Split the data into training and testing sets.
15. Train and evaluate:
    - Linear Regression
    - Ridge Regression
    - XGBoost Regression
16. Tune XGBoost hyperparameters using `RandomizedSearchCV`.
17. Use the best XGBoost model for prediction.
18. Analyze prediction errors and extreme-error cases.
19. Visualize feature coefficients/importance and residual-error distributions.

## Target Variable

The model predicts:

```text
Annual_Premium_Amount
```

## Main Features

The notebook works with features including:

- Age
- Number Of Dependants
- Income_Lakhs
- Income_Level
- Insurance_Plan
- Gender
- Region
- Marital_status
- BMI_Category
- Smoking_Status
- Employment_Status
- Normalized medical risk score
- One-hot encoded categorical features

The final modeling dataset removes the original medical-history fields and uses encoded/numerical features.

## Data Preprocessing

### Missing Values

Missing values in:

- `Smoking_Status`
- `Employment_Status`
- `Income_Level`

are filled using the mode of the respective column.

### Invalid Values

Negative values in `Number Of Dependants` are converted using absolute values.

### Outlier Treatment

The notebook:

- removes records where `Age > 100`
- calculates the IQR for `Income_Lakhs`
- uses the 99.9th percentile as the selected income threshold
- removes records above that threshold

### Feature Engineering

Medical history is split into two disease fields and mapped to predefined risk scores. These scores are combined into:

```text
total_risk_score
normalized_risk_score
```

`Insurance_Plan` and `Income_Level` are also mapped to numerical values.

Categorical variables such as gender, region, marital status, BMI category, smoking status, and employment status are one-hot encoded.

## Feature Scaling

`MinMaxScaler` is applied to:

```text
Age
Number Of Dependants
Income_Level
Income_Lakhs
Insurance_Plan
```

The scaled features are used for subsequent modeling and VIF analysis.

## Multicollinearity

VIF (Variance Inflation Factor) is calculated to identify multicollinearity among input features.

After the VIF check, `Income_Level` is removed from the modeling feature set:

```python
X_reduced = X.drop('Income_Level', axis='columns')
```

## Machine Learning Models

### Linear Regression

A Linear Regression model is trained and evaluated using:

- R² score
- Mean Squared Error (MSE)
- Root Mean Squared Error (RMSE)

The notebook also visualizes Linear Regression coefficients.

### Ridge Regression

Ridge Regression is trained with:

```python
Ridge(alpha=1)
```

Its training/test scores and RMSE are evaluated.

### XGBoost Regression

An `XGBRegressor` is trained and evaluated.

The notebook then performs hyperparameter tuning using `RandomizedSearchCV` with 3-fold cross-validation.

The search includes:

```text
n_estimators: [20, 40, 50]
learning_rate: [0.01, 0.1, 0.2]
max_depth: [3, 4, 5]
```

The scoring metric used for the randomized search is:

```text
R² (r2)
```

The best estimator is then used for test-set predictions.

## Model Error Analysis

For the selected XGBoost model, the notebook creates a results table containing:

- `actual`
- `predicted`
- `diff`
- `diff_pct`

It also:

- plots the distribution of percentage differences
- identifies predictions with an absolute percentage difference greater than 10%
- calculates the percentage of test cases with these extreme errors
- investigates feature distributions for extreme-error cases

## Visualizations

The notebook includes visualizations such as:

- Box plots for numerical features
- Histograms with KDE
- Scatter plots against annual premium
- Categorical percentage-distribution charts
- Income-level vs insurance-plan bar chart
- Income-level vs insurance-plan heatmap
- Correlation heatmap
- Linear Regression coefficient chart
- XGBoost feature-importance chart
- Residual/error distribution
- Extreme-error feature distribution comparisons

## Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Statsmodels
- XGBoost
- OpenPyXL
- Google Colab / Jupyter Notebook

## Project Structure

```text
Insurance-Premium-Prediction/
│
├── Premium.ipynb
├── requirements.txt
├── premiums (1).xlsx
└── README.md
```

> If the dataset is private, restricted, or not licensed for redistribution, do not upload the Excel file to a public GitHub repository. In that case, provide instructions for obtaining the dataset separately.

## Installation

Clone the repository:

```bash
git clone <your-repository-url>
cd Insurance-Premium-Prediction
```

Install the required libraries:

```bash
pip install -r requirements.txt
```

## Running the Project

### Using Jupyter Notebook

```bash
jupyter notebook Premium.ipynb
```

### Using Google Colab

Upload `Premium.ipynb` to Google Colab and update the dataset path if necessary.

The original notebook loads the Excel file from:

```python
/content/sample_data/premiums (1).xlsx
```

If the dataset is stored in the same project folder, update the path accordingly, for example:

```python
pd.read_excel('premiums (1).xlsx')
```

## Important Notes

- The notebook was developed in Google Colab.
- The dataset path in the original notebook is Colab-specific and may need to be changed when running locally or from GitHub.
- Model performance depends on the dataset and preprocessing used in the notebook.
- The notebook contains exploratory analysis, preprocessing, model comparison, hyperparameter tuning, and error analysis.

## Author

**Aswathy**

B.Sc. Computer Science | Data Science & AI

Interested in Data Analytics, Data Science, Machine Learning, and AI.

---

⭐ If you found this project useful, consider giving the repository a star.
