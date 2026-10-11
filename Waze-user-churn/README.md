# Waze User Churn Analysis

This project analyzes Waze user activity and builds classification models to identify users labeled as churned.

## Business goal

The goal is to help Waze understand behavioral patterns associated with churn and explore whether activity data can flag users for timely retention or re-engagement outreach. The models are analytical prototypes, not a deployed customer-targeting system.

## Data

The raw file, `data/raw/waze_dataset.csv`, contains 14,999 user records and activity measures such as sessions, drives, navigation usage, distance, driving time, activity days, driving days, and device. The label has 700 missing values; the modeling workflow removes those records. The processed file, `data/processed/waze_dataset.csv`, adds derived activity measures and device encoding.

In the labeled sample, 17.7% of users are marked churned and 82.3% retained.

## What was done

- Explored the relationship between user activity, device, and churn in [`eda.ipynb`](./eda.ipynb).
- Engineered measures such as kilometers and drives per driving day. In the saved analysis, churned users had a median of about 698 km per driving day and 10 drives per driving day, compared with about 290 km and 4.1 drives for retained users. Retained users were active on more days (median 17 versus 8).
- Added behavioral features, including a “professional driver” indicator, and trained a logistic-regression baseline plus tuned Random Forest and XGBoost models in [`modelling.ipynb`](./modelling.ipynb).
- Used stratified train/validation/test splits, compared precision, recall, F1, and accuracy, and examined a lower XGBoost decision threshold to trade some accuracy for better churn detection.
- Included a serialized model artifact at `model/model.pkl`.

## Model results

On the saved test split, the XGBoost model at its default threshold reported:

| Metric for churned users | Result |
| --- | ---: |
| Precision | 0.38 |
| Recall | 0.18 |
| F1-score | 0.24 |
| Overall accuracy | 0.80 |

Reducing the probability threshold to 0.40 increased churn recall to 0.26 and F1-score to 0.31, while accuracy fell to 0.79. The logistic-regression baseline reached 0.83 accuracy but detected only 0.09 of churned users, illustrating why overall accuracy alone is not an adequate success measure for this imbalanced problem.

## Business outcome and limitations

The analysis identified useful behavioral differences and produced an initial risk-scoring approach that could be tested in retention experiments. However, the default-threshold model misses most churned users, and even the lower-threshold version has modest recall. Use the scores only to guide further investigation and controlled outreach tests, not as an automatic decision rule. The notebook's model-save cell writes `model.pkl` in the project root, whereas the supplied artifact is under `model/model.pkl`; align the path when regenerating the artifact.

## Project files

- `eda.ipynb` — churn patterns and user-activity exploration
- `modelling.ipynb` — feature engineering, model comparison, evaluation, and serialization
- `data/raw/` and `data/processed/` — source and modeling datasets
- `model/model.pkl` — supplied serialized model artifact

The notebooks use Python with pandas, NumPy, Matplotlib, Seaborn, SciPy, and scikit-learn; the modeling notebook also uses XGBoost.
