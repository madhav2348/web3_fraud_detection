# web3_fraud_detection
---

# Web3 / Crypto Transaction Anomaly Detection

## Overview

This project detects suspicious or anomalous transactions in synthetic Web3/crypto datasets using **unsupervised machine learning**.
It is designed for **fraud detection** tasks in blockchain ecosystems where labeled fraud data is scarce.
The system uses **Isolation Forest** and **Local Outlier Factor (LOF)** to flag unusual behavior for further investigation.

---

## Features

* **Ingest & preprocess** Web3 transaction data.
* **Feature engineering** from timestamps, amounts, and categorical fields.
* **Two anomaly detection models** (Isolation Forest & LOF) with ensemble scoring.
* **Realtime scoring API** (`process_transaction`) for live or batch transactions.
* **Automatic anomaly logging** to CSV for human review.
* **Artifacts persistence** (encoder, scaler, models) for reproducible inference.
* **Extendable pipeline** to add more models or features.

---

## Dataset Structure

| Column        | Description                             |
| ------------- | --------------------------------------- |
| tx\_hash      | Unique transaction hash                 |
| from\_wallet  | Sender wallet address                   |
| to\_wallet    | Receiver wallet address                 |
| token         | Token symbol (e.g., ETH, USDT)          |
| amount        | Transaction amount                      |
| timestamp     | Date/time of transaction                |
| gas\_fee\_usd | Gas fee paid (USD)                      |
| platform      | Platform used (e.g., Uniswap, OpenSea)  |
| tx\_type      | Transaction type (e.g., swap, transfer) |

---

##  Models & Algorithms

* **Isolation Forest**

  * Tree-based anomaly detector that isolates outliers via random splits.
  * Detects *global* anomalies.
* **Local Outlier Factor (LOF, novelty=True)**

  * Measures local density deviation to catch *local* anomalies.
* **Ensemble Scoring**

  * Combines scores from both models to reduce false positives.

---

##  Solution Architecture

1. **Data Ingestion**
   Load dataset (via Kaggle API) into Pandas DataFrame.

2. **Feature Engineering**

   * Extract `year`, `month`, `day`, `hour` from `timestamp`.
   * Select numerical & categorical columns.

3. **Encoding & Scaling**

   * `OrdinalEncoder` for categorical fields (handles unseen categories).
   * `StandardScaler` for numerical normalization.

4. **Model Training**

   * Train Isolation Forest and LOF.
   * Compute prediction labels and anomaly scores.

5. **Persistence**
   Save encoder, scaler, and both models to disk (`joblib`).

6. **Realtime Scoring Function** (`process_transaction`)

   * Accepts a single transaction as a Python dict.
   * Preprocess → scale → predict.
   * Logs anomalies to CSV for human review.

7. **Human Review & Feedback Loop**

   * Analysts review anomalies.
   * Optionally retrain with confirmed fraud patterns.

---
## Evaluation

Since this is **unsupervised**, evaluation is based on:

* Manual inspection of top anomalies.
* Synthetic injection of fraudulent patterns to test sensitivity.
* Precision\@K for flagged transactions.

---

## Potential Impact

* **Early fraud detection** in blockchain ecosystems.
* **Reduce investigator workload** by focusing on high-risk transactions.
* **Support compliance** with audit-ready anomaly logs.
* **Scalable** to millions of transactions.

---
