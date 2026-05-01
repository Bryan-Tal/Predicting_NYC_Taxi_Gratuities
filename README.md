# Predicting_NYC_Taxi_Gratuities
Predicting Taxi Gratuities in NYC

## Project Overview

The goal of this project was to predict whether a New York City yellow-taxi rider would tip generously (≥ 20%). A Random Forest baseline and a tuned XGBoost model were compared on 2017 NYC TLC trip data. The final XGBoost model performed with **83.3% accuracy and 82.3% precision**, beating the Random Forest baseline by **+12.7 F1 points**. The most influential features were VendorID, fare amount, and total trip cost.

The "generous" threshold was defined as `tip_amount / (total_amount - tip_amount) ≥ 0.20`. After preprocessing the class balance was **47.4% non-generous / 52.6% generous** — relatively balanced, so no SMOTE or class-weight handling was needed.

## Business Understanding 

According to salary.com the average salary for a New York Taxi Driver is around $45,000. This salary is significantly low compared to a median rent value of $6,500 per month. It is important to understand what factors encourage riders to leave tips in order to help drivers obtain a livable wage. 

## Data Understanding 

The NYC Taxi and Limousine Commission data came from NYC.gov. The data consisted of approximately 408k unique trips and 18 features. The features included information on trip duration and destination, vendor used, toll information, and payment type. The bar chart below shows the breakdown of how many generous tippers (>20%) versus non-generous tippers that exist in the data set. 
<img width="394" alt="Screenshot 2024-12-31 at 3 01 15 AM" src="https://github.com/user-attachments/assets/5e8b4ce2-0fa8-49fb-aa17-d3b20694439f" />


## Feature Engineering
- Extracted **day of week** and **month** from the pickup timestamp.
- Engineered **four time-of-day binary features**: `am_rush` (06–10), `daytime` (10–16), `pm_rush` (16–20), `nighttime` (20–06).
- One-hot encoded vendor, rate code, and time bins via `pd.get_dummies()`.
- Dropped `tip_amount`, `tip_percent`, `payment_type`, and raw timestamps to prevent target leakage.

## Methodology
- **80/20 train/test split** (`random_state=42`).
- **5-fold GridSearchCV refit on F1** for both Random Forest and XGBoost.
  - Random Forest search: `max_depth ∈ [3, 4, 5]`, `n_estimators ∈ [50, 100]`, plus `max_features` and `max_samples`.
  - XGBoost search: `max_depth ∈ [3, 4, 5]`, `learning_rate ∈ [0.1, 0.2, 0.3]`, `n_estimators ∈ [5, 10, 15]`.
- **Best XGBoost params**: `max_depth=5`, `learning_rate=0.3`, `n_estimators=15`, `min_child_weight=5`.

## Modeling and Evaluation 

XGBoost was the champion, beating the Random Forest baseline across every metric on the held-out test set:

| Model | Accuracy | Precision | Recall | F1 |
|---|---|---|---|---|
| Random Forest (baseline) | 0.7124 | 0.6809 | 0.8254 | 0.7462 |
| **XGBoost (champion)** | **0.8330** | **0.8233** | **0.8581** | **0.8403** |

Feature importance (mean decrease in impurity) ranked **VendorID, fare_amount, and total_amount** as the top three predictors.

<img width="915" alt="Screenshot 2024-12-31 at 3 06 14 AM" src="https://github.com/user-attachments/assets/febb5a57-3300-4dbc-9a27-e3cf23ffb6b9" />

### Error analysis
The confusion matrix shows **more false positives than false negatives** — the model is "optimistically generous" toward drivers, predicting good tips slightly more often than warranted. For a driver-facing app, this Type-I-leaning bias is preferable to the inverse (telling a driver to expect a good tip and being wrong is less harmful than telling them not to expect one and being wrong).

## Tech Stack
Python · Pandas · NumPy · scikit-learn · XGBoost · Matplotlib · Seaborn · Jupyter

## Limitations
- **Cash-payment survivorship bias.** NYC TLC records cash tips as $0, so the notebook drops all cash trips (`payment_type != 1`) before modeling. The model only generalizes to **credit-card riders** — likely a more affluent and tip-engaged subset of the population than NYC taxi riders overall.
- **Single year (2017).** Temporal generalization is untested; tipping behavior likely shifted post-2020.
- **VendorID effect unexplained.** A feature this dominant deserves causal investigation — could be driver assignment, vehicle type, or app UI differences between vendors. Flagged as future work.
- **Black-box champion.** XGBoost predictions aren't directly interpretable. SHAP or LIME analysis would be a natural next step.

## Conclusion

This model can benefit Taxi Drivers by letting them know if they will be tipped generously or not. In the future, adding more information on a rider's past tipping behavior may also be beneficial in helping the stakeholder address their business problem.
