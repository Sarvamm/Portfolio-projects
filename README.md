# Portfolio Projects

A collection of data analytics and machine-learning projects focused on business questions including customer retention, employee attrition, content review, and fare estimation. Each project folder includes its own README with the dataset, workflow, findings, and relevant caveats.

## Projects

| Project | Summary | Business goal |
| --- | --- | --- |
| [Automatidata Taxi Fare Prediction](./Automatidata-price-prediction/README.md) | Explores 2017 NYC yellow-taxi trips, cleans anomalous records, and compares linear regression, ElasticNet, and Ridge fare estimates. The notebook reports a test R-squared of about 0.90 on its restricted sample. | Provide a baseline fare estimate and identify trip characteristics associated with metered fares. |
| [Salifort Motors HR Analytics](./Salifort-motors-attrition/README.md) | Analyzes employee satisfaction, workload, tenure, and other HR factors, then trains a Random Forest attrition model. Achieves 0.98 test accuracy and 0.90 recall for employees who left. | Help HR understand employee departure patterns and prioritize retention efforts. |
| [Telco Customer Churn Prediction](./Telco-customer-churn/README.md) | Cleans and engineers customer, service, contract, and billing features; compares churn classifiers; and provides a Streamlit prediction app. Achieves a Random Forest test recall of about 0.80 for churn. | Identify telecom customers at risk of leaving and support retention outreach. |
| [TikTok Claim and Opinion Classification](./TikTok-claim-classification/README.md) | Examines video text, engagement, and creator status, then compares text-based classifiers for claim/opinion labels. Includes a separate exploratory analysis of account verification status. | Prototype a triage aid for prioritizing claim-labeled videos for human review; it does not verify factual accuracy. |
| [Waze User Churn Analysis](./Waze-user-churn/README.md) | Studies navigation-app activity patterns and compares logistic regression, Random Forest, and XGBoost churn models. | Explore behavioral signals for user churn and inform potential re-engagement experiments. |

