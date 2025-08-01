# Credit Risk Modeling (PD Model – IFRS-9 Aligned) | Logistic Regression | Lending Club Data

This project builds a Probability of Default (PD) model aligned with IFRS-9 guidelines using a real-world Lending Club dataset (~100,000 loans). The model applies logistic regression and incorporates advanced data preprocessing, Weight of Evidence (WoE), and Information Value (IV) techniques.

## Project Summary

This end-to-end credit risk modeling pipeline estimates the likelihood that a borrower will default on a loan. Starting from raw Lending Club data, the process involves data cleaning, variable transformation, model training with logistic regression, and validation using 5-fold cross-validation. The model outputs borrower-level default probabilities in line with IFRS-9 requirements.

## Dataset

- Source: Lending Club loan dataset
- Size: ~100,000 records
- Features: 50+
- Target Variable: `loan_status` (converted into binary: 1 = Default, 0 = Fully Paid)

## Methodology

### 1. Data Cleaning

- Removed duplicates and columns with excessive missing values
- Filtered `loan_status` to include only "Fully Paid" and "Charged Off"
- Created binary target: 1 = Bad loan, 0 = Good loan

### 2. Feature Engineering

- Binned continuous variables (e.g., loan amount, income) using quantiles
- Grouped rare categories for high-cardinality categorical variables
- Converted variables into suitable formats for modeling (e.g., datetime to tenure)

### 3. WoE and IV Transformation

- Applied Weight of Evidence (WoE) transformation on binned variables
- Selected variables with Information Value (IV) > 0.02
- Verified monotonicity of WoE values to ensure regulatory interpretability

### 4. Model Training – Logistic Regression

- Model: Logistic Regression using scikit-learn
- Target: Probability of default (PD)
- Validation: 5-Fold Cross-Validation
- Performance Metrics:
  - Confusion Matrix
  - F1 Score
  - ROC-AUC Score
  - Precision & Recall

### 5. Model Interpretation

- Extracted coefficients from logistic regression model
- Calculated odds ratios for interpretability
- Sorted predictors based on predictive power and business relevance

## IFRS-9 Alignment

- Estimated point-in-time Probability of Default (PD) required for ECL calculation
- Used monotonic, interpretable variables in line with risk governance expectations
- Prepared outputs suitable for integration into Expected Credit Loss (ECL) formula:




- The model is structured to be auditable and explainable for regulatory use

## Cross-Validation Approach

- 5-Fold Cross Validation:
- Data is split into 5 parts
- Model is trained on 4 parts and validated on the remaining part
- Repeated 5 times with each fold serving as the validation set once
- Final performance is the average across all folds

## Output Interpretation

| Variable         | Coefficient | Odds Ratio | Interpretation                                  |
|------------------|-------------|------------|--------------------------------------------------|
| loan_amnt_bin    | 0.95        | 2.59       | Higher loan amount increases default likelihood |
| emp_length_bin   | -0.40       | 0.67       | Longer employment reduces risk                  |
| grade_bin        | 1.10        | 3.00       | Lower grade triples the risk of default         |

Final PD scores can be categorized into risk buckets and used in downstream lending strategy or ECL computation.

## Challenges Faced

| Challenge                      | Solution                                                         |
|-------------------------------|------------------------------------------------------------------|
| Large dataset (~100K records) | Used memory-efficient data types and cleaned early              |
| Class imbalance               | Used F1 and ROC-AUC instead of accuracy                         |
| WoE non-monotonic bins        | Manually re-binned variables to ensure monotonic transformation |
| High-cardinality variables    | Grouped sparse categories and aggregated state-level features   |
| Interpretability requirements | Chose logistic regression with WoE instead of black-box models  |

## Tools & Libraries

- Python (pandas, numpy, scikit-learn, matplotlib, seaborn)
- WoE/IV Calculation (custom logic)
- Logistic Regression (scikit-learn)
- Jupyter Notebook for analysis and modeling

## Future Enhancements

- Add Loss Given Default (LGD) and Exposure at Default (EAD) models
- Extend to Lifetime PD using survival analysis or macroeconomic overlays
- Introduce alternative models (e.g., Random Forest, XGBoost) for benchmarking
- Build interactive dashboards for credit risk segmentation and reporting

