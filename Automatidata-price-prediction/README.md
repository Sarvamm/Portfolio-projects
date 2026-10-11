# Automatidata Taxi Fare Prediction

This project explores New York City yellow-taxi trips and builds a regression prototype to estimate the metered fare from trip characteristics.

## Business goal

Automatidata can use fare estimates to support trip-price planning and identify fare patterns that merit further investigation. This analysis demonstrates a first-pass estimate for the cleaned trips in the 2017 sample; it is not a production pricing service and should not be applied to every fare type without further validation.

## Data

The raw input is `data/raw/2017_Yellow_Taxi_Trip_Data.csv` (22,699 trips). The exploratory notebook checks timestamp order, passenger and distance values, fare anomalies, rate codes, payment types, and whether the reported total reconciles with its listed components. It derives trip duration, average speed, and pickup-time fields, then removes records outside the modeling scope.

The resulting `data/processed/data.csv` contains 21,771 rows and these columns:

- `trip_distance`
- `RatecodeID`
- `fare_amount` (prediction target)
- `trip_duration` (retained for analysis, not used as a model feature)
- `pickup_hr`

## What was done

- Explored trip, payment, rate-code, location, and time-of-day patterns in [`eda.ipynb`](./eda.ipynb).
- Removed implausible trips and fare outliers. JFK flat-rate trips (`RatecodeID` 2) and negotiated fares (`RatecodeID` 5) are excluded from the modeled sample.
- Engineered trip duration and pickup-hour indicators. Trip distance and duration were strongly correlated, so duration was excluded from the regression features.
- Compared linear regression, ElasticNet, and Ridge models using a held-out test set. ElasticNet and Ridge hyperparameters were tuned with five-fold cross-validation in [`modelling.ipynb`](./modelling.ipynb).
- Saved a fitted estimator at `models/model.pkl`.

## Results

On the saved notebook's test split, the linear regression model reported:

| Metric | Result |
| --- | ---: |
| Mean absolute error (MAE) | 1.75 |
| Root mean squared error (RMSE) | 2.53 |
| R-squared | 0.900 |

The three evaluated models were close: ElasticNet reached an R-squared of 0.898 and Ridge reached 0.900. Trip distance had the largest positive linear-regression coefficient among the included features.

## Business outcome and limitations

The analysis produced a fare-estimation baseline and documented which trip characteristics carry useful signal in the cleaned data. Its performance is promising for that restricted sample, but it does not establish accuracy for the excluded JFK and negotiated-fare trips, other years, or future trips. The notebook also labels Linear Regression as selected while its final save cell serializes the fitted ElasticNet model (`alpha=0.01`, `l1_ratio=0.9`); confirm which estimator is intended before using the artifact.

## Project files

- `eda.ipynb` — data checks, cleaning, and exploratory analysis
- `modelling.ipynb` — regression comparison, evaluation, and model serialization
- `data/raw/` and `data/processed/` — source and modeling datasets
- `models/model.pkl` — serialized fitted estimator

The notebooks use Python with pandas, NumPy, Matplotlib, Seaborn, SciPy, and scikit-learn.
