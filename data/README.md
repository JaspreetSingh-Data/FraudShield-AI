# Dataset

The raw dataset (`bank_fraud.csv`, ~1,000,000 transactions, ~150MB) is **not included** in this repository — it exceeds GitHub's 100MB file size limit for standard repos.

## To reproduce this project:

1. Obtain a bank/credit card transaction fraud dataset with transaction-level features and a binary `is_fraud` label. Kaggle hosts several public datasets of this kind (search "bank transaction fraud detection" or "credit card fraud detection").
2. Place the CSV in this `data/` folder as `bank_fraud.csv`.
3. Run `fraud_detection_analysis.ipynb` from the top — all preprocessing and feature engineering steps are included in the notebook.

## Dataset summary (for reference)

- **Rows:** 1,000,000 transactions
- **Raw columns:** 26
- **Target:** `is_fraud` (binary)
- **Class balance:** 94.5% legitimate, 5.5% fraudulent
- **Time span:** transactions from 2023–2024, used for a temporal train/test split
