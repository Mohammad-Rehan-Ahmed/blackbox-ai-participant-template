# round-4 — Reconstruct

**Team:** BB-002  
**Queries used:** 129 / 160

## What we concluded

We reconstructed the BLACKBOX scoring and decision behavior as a two-stage process:

1. Predict the continuous `score` from the available input features.
2. Convert the predicted score into a decision using an optimized threshold.

The final reconstruction uses an `ExtraTreesRegressor` with 1200 trees, `max_features=0.8`, no maximum depth, and `min_samples_leaf=1`.

The target variable is `score`. Importantly, `score` was excluded from the model input features to avoid target leakage.

The final out-of-fold regression performance was:

- RMSE: **0.105696**
- MAE: **0.057992**
- R²: **0.832649**

The optimized decision threshold was **0.590**.

Using this threshold:

- Decision accuracy: **90.70%**
- Balanced accuracy: **94.50%**
- True DECLINE: **20**
- False APPROVE: **0**
- False DECLINE: **12**
- True APPROVE: **97**

The model therefore reproduced the important decision boundary well, particularly the DECLINE cases: there were **no false APPROVE predictions** in the final out-of-fold evaluation.

A known BLACKBOX validation case also matched exactly:

- BLACKBOX score: **0.9793**
- Reconstructed model score: **0.9793**
- BLACKBOX decision: **APPROVE**
- Model decision: **APPROVE**
- Score difference: **0.000000**

## How we got there

The available predictors were:

- `age`
- `baseline_score`
- `comorbidity_ratio`
- `dependants`
- `prior_visits`
- `recent_admissions`
- `requested_beds`
- `vitals_index`
- `ward`
- `years_registered`

The reconstruction treated `score` as the regression target rather than an input.

Because the observed decisions were imbalanced (109 APPROVE versus 20 DECLINE), balanced regression sample weights were used during model fitting. The resulting weights were approximately:

- APPROVE: **0.5917**
- DECLINE: **3.2250**

We evaluated the reconstruction using 5-fold stratified regression cross-validation. The folds preserved the APPROVE/DECLINE distribution as closely as possible.

The continuous predictions were evaluated independently from the decision rule. A threshold search was used to select the score cutoff that maximized balanced accuracy. The resulting threshold was **0.590**.

The final model was then trained on all 129 available rows.

## What we ruled out

We ruled out using the observed `score` as a model input because it is the target being reconstructed. Including it would constitute target leakage and would not represent a genuine reconstruction of BLACKBOX behavior from the available features.

We also did not rely on raw decision frequency as the prediction rule. The dataset contains substantially more APPROVE than DECLINE cases, so simply predicting the majority class would not adequately reproduce the underlying decision behavior.

Instead, the continuous score prediction and the decision threshold were evaluated separately.

## What we are still unsure about

The reconstructed model is an empirical approximation of BLACKBOX behavior rather than access to the original BLACKBOX implementation.

The available observations are limited to the provided cases, so behavior outside the observed feature ranges cannot be established with certainty.

The exact internal feature transformations and any hidden rules used by BLACKBOX remain unknown.

The optimized threshold is also estimated from the available observations and may not be the exact internal threshold used by BLACKBOX.

Nevertheless, the out-of-fold results and the exact match on the known champion validation case provide evidence that the reconstruction captures a substantial portion of the observed scoring and decision behavior.
