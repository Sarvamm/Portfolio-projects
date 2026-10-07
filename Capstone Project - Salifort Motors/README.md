# Salifort Motors HR Analytics Capstone

This project analyzes employee attrition at Salifort Motors and builds a predictive model to identify employees most likely to leave the company.

## Project objective

The HR department wants to understand which factors are associated with employee departure and what initiatives may reduce turnover. The analysis focuses on the question: what is likely to make an employee leave Salifort Motors?

## Dataset

The project uses a synthetic/structured HR dataset containing employee attributes and a target variable, `left`, where:

- `1` = employee left the company
- `0` = employee stayed

Dataset files:

- `data/raw/HR_capstone_dataset.csv`
- `data/processed/HR_capstone_dataset.csv`

### Features

- `satisfaction_level`
- `last_evaluation`
- `number_project`
- `average_montly_hours`
- `time_spend_company`
- `Work_accident`
- `promotion_last_5years`
- `Department`
- `salary`

### Dataset summary

- 14,999 rows
- 10 columns
- No missing values
- Target distribution: 11,428 stayed, 3,571 left

## Project structure

- `eda.ipynb` — exploratory data analysis and visualizations
- `modelling.ipynb` — hypothesis testing, modeling, validation, and final model training
- `data/raw/HR_capstone_dataset.csv` — original raw dataset
- `data/processed/HR_capstone_dataset.csv` — processed version used in modeling
- `model/model.pkl` — saved trained Random Forest model

## Exploratory data analysis highlights

The EDA notebook investigated relationships between employee exit and variables such as satisfaction, evaluation scores, workload, tenure, promotion, accidents, department, and compensation.

Key findings from the analysis:

- Low employee satisfaction is strongly associated with leaving.
- Employees with very low satisfaction levels are much more likely to exit.
- Employees involved in a high workload pattern — many projects and many monthly hours — are more likely to leave.
- Most employees who leave do so in the 3–5 year tenure range.
- Those with 7+ years at the company are not observed leaving.
- Work accidents and leaving are statistically associated.
- Promotion history is not a major driver of exit for most employees.
- Salary and department show some variation, but satisfaction and workload are the strongest signs.

## Statistical testing

The modeling notebook applies a chi-square test of independence to assess whether the relationship between work accidents and employee departure is statistically significant.

The test rejects the null hypothesis, indicating that work accidents are not independent of employee exit.

## Modeling workflow

The modeling notebook follows a structured machine learning workflow:

1. Load processed data
2. Define target and features
3. Split data into train/validation/test sets
4. Compare candidate models
5. Tune hyperparameters with cross-validation
6. Select the best-performing model based on F1 and recall
7. Evaluate on unseen test data
8. Retrain on the full training data and save the final model

## Selected model

The final selected model is a `RandomForestClassifier` with the following tuned configuration:

- `criterion`: `entropy`
- `random_state`: `0`
- `class_weight`: `balanced`
- `n_estimators`: `175`
- `max_depth`: `11`
- `min_samples_split`: `3`
- `min_samples_leaf`: `1`
- `max_features`: `6`

## Model performance

The final model was evaluated on the test set and achieved strong results:

- Accuracy: 0.98
- Precision (class 1 / left): 0.96
- Recall (class 1 / left): 0.90
- F1-score (class 1 / left): 0.93

This indicates the model has strong predictive power for identifying which employees are likely to leave.

## Business interpretation

The results suggest that employee retention efforts should focus on:

- improving employee satisfaction
- managing workload and overtime
- monitoring attrition risk in the 3–5 year tenure window
- supporting employees in high-stress or high-workload roles
- evaluating retention programs for employees with risk factors such as low satisfaction and heavy workloads


## Tools and libraries

The project uses common Python data-science libraries, including:

- pandas
- NumPy
- matplotlib
- seaborn
- plotly
- scikit-learn
- SciPy

## Outcome

This project delivers both a business-facing understanding of employee churn drivers and a practical predictive model that can support HR decision-making and retention planning at Salifort Motors.
