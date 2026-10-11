# TikTok Claim and Opinion Classification

This project analyzes TikTok videos labeled as claims or opinions and develops a text-based classifier to distinguish the two categories.

## Business goal

The prototype is intended to help a content-review workflow prioritize videos that make factual claims for closer review. It classifies the supplied labels; it does **not** verify whether a claim is true or false, and it is not a substitute for moderation or fact-checking decisions.

## Data

The source dataset is `Data/Raw/tiktok_dataset.csv`. It includes each video's claim/opinion label, transcription, duration, engagement counts, and the creator's verification and moderation statuses. After rows with missing values are removed, the classification notebook reports a nearly balanced target: about 50.3% claims and 49.7% opinions.

## What was done

- Explored label balance, video engagement, author moderation status, and engagement rates in [`eda.ipynb`](./eda.ipynb).
- Found a large difference in median view counts: 501,555 for claims versus 4,953 for opinions in this dataset. The analysis also found higher engagement rates for claim-labeled videos; these are observed associations, not evidence that the label caused the engagement.
- Added transcription length and encoded creator-status categories as features.
- Converted the transcriptions into a limited set of the 15 most frequent two- and three-word n-grams, then compared a tuned Random Forest and XGBoost classifier using five-fold cross-validation.
- Selected Random Forest as the notebook's final claim/opinion model and assessed it alongside XGBoost in [`claim_opinion_classification.ipynb`](./claim_opinion_classification.ipynb).
- Built a separate logistic-regression analysis of whether an account is verified in [`reg_analysis.ipynb`](./reg_analysis.ipynb). That notebook balances the verification classes by upsampling and reports 67% accuracy on its test split; this is a separate, exploratory target from claim/opinion classification.

## Results and interpretation

The saved validation report rounds the Random Forest's precision, recall, F1, and accuracy to 1.00; XGBoost's corresponding validation metrics round to 0.99. These results indicate strong separation on the notebook's split, but should be treated as preliminary. The final Random Forest test classification report calls `classification_report(y_pred, y_test)` with the arguments reversed, so its reported class-level precision and recall are not a correctly oriented test evaluation. Re-run that evaluation with the true labels first before using or presenting test-set metrics.

The companion verification-status notebook upsamples the minority class **before** splitting into train and test sets. Duplicate examples can therefore appear in both sets, making that notebook's test metrics potentially optimistic. It should be treated as an exploratory baseline, not evidence of reliable account-status prediction.

## Business outcome and limitations

The project delivers an exploratory claim/opinion triage model and analysis of how labels relate to video engagement and creator status. The results can inform what features or workflows to investigate next, but the classifier has no saved model artifact or deployment interface in this folder. Before operational use, validate performance on a fresh, representative, leakage-free holdout set, check class-specific errors, and assess moderation impacts.

## Project files

- `eda.ipynb` — exploratory analysis of labels, moderation status, and engagement
- `claim_opinion_classification.ipynb` — text feature engineering and claim/opinion models
- `reg_analysis.ipynb` — separate exploratory model of creator verification status
- `Data/Raw/tiktok_dataset.csv` — source dataset

The notebooks use Python with pandas, NumPy, Matplotlib, Seaborn, and scikit-learn; the claim/opinion notebook also uses XGBoost.
