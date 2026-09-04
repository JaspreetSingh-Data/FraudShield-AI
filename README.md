# FraudShield AI — Credit Card Fraud Detection

An end-to-end machine learning pipeline that detects fraudulent bank transactions, built and evaluated on a dataset of 1,000,000 transactions with realistic class imbalance (5.5% fraud).

## Overview

Fraud detection is a classic imbalanced classification problem — fraud is rare, expensive to miss, and easy to over-flag if a model isn't calibrated carefully. This project builds a full pipeline from raw transaction data to a calibrated, explainable fraud classifier, and evaluates it the way a production system would be evaluated: on data from a time period the model never saw during training.

## Dataset

- 1,000,000 transactions, 26 raw features
- 5.5% fraud rate (55,255 fraudulent transactions)
- Expanded to 109 features after preprocessing and feature engineering
- **Note:** the raw dataset file is not included in this repo due to GitHub's file size limits. See `data/README.md` for details.

## Approach

1. **Data acquisition, cleaning & preprocessing** — handling missing values, encoding, and preparing the raw transaction data
2. **Exploratory data analysis (EDA)** — understanding fraud patterns, transaction distributions, and feature relationships
3. **Feature engineering** — building behavioral and risk-based features from raw transaction data
4. **Temporal train/test split** — trained on 2023 data, evaluated on completely unseen 2024 data, to avoid the data leakage that a random split can hide
5. **Model benchmarking** — compared 7 classifiers:
   - Logistic Regression
   - Decision Tree
   - Random Forest
   - Extra Trees
   - HistGradientBoosting
   - XGBoost
   - **CatBoost** (final model)
6. **Calibration & threshold tuning** — applied Platt scaling to calibrate predicted probabilities, then tuned the decision threshold (0.65) to balance precision and recall
7. **Explainability** — used SHAP for both global and local interpretability, and ran a feature ablation study to validate that the top feature (`behavioral_risk_score`, ~51% importance) was genuinely predictive rather than a modeling artifact

## Results (on unseen 2024 test data)

| Metric | Score |
|---|---|
| ROC-AUC | 0.728 |
| PR-AUC | 0.127 |
| Precision (at threshold 0.65) | 13.8% |
| Recall (at threshold 0.65) | 36.2% |
| F1 | 0.199 |

At the chosen threshold, the model catches **36% of fraud cases** in a dataset where fraud makes up only 5.5% of transactions — a meaningful lift over random flagging, in a setting where perfect recall is not realistic without an unacceptable false-positive rate.

## Tech Stack

Python · pandas · NumPy · scikit-learn · XGBoost · CatBoost · SHAP · Matplotlib · Seaborn

## Repository Structure

\```
FraudShield-AI/
├── fraud_detection_analysis.ipynb   # Full pipeline: EDA → features → modeling → evaluation → SHAP
├── data/
│   └── README.md                    # Where to get the dataset
├── requirements.txt
└── README.md
\```

## Key Learnings

- A random train/test split can look great and still hide leakage in time-series-like data (like transactions); a temporal split gives a more honest estimate of real-world performance.
- With a 5.5% fraud rate, accuracy is a meaningless metric — ROC-AUC, PR-AUC, and recall at a business-relevant threshold matter far more.
- Explainability isn't optional for a fraud model — SHAP and ablation testing confirm the model is learning genuine risk signals, not spurious correlations.

- 
