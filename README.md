# SQL Query Latency Prediction using Workload Drift

This project simulates a learned query cost model that predicts SQL query latency from structural features and data-driven signals. Inspired by the LIMAO paper (Lifelong Modular Learned Query Optimization), it explores how machine learning can be used to estimate query runtime and handle workload drift — a critical challenge in intelligent database systems.

---

## Motivation

Traditional query optimizers rely on static heuristics and struggle with dynamic or unseen workloads. Recent work like LIMAO proposes lifelong learning systems to adapt query planning over time. This project builds a simplified pipeline to:

- Predict query latency using ML models
- Analyze model performance under synthetic workload drift
- Investigate the role of feature signals like structure, estimated rows, and selectivity

---

## Dataset

We use synthetic data generated via SQLite with two tables:
- `Customers` (10,000 rows)
- `Orders` (20,000 rows)

100 SQL queries were generated across two phases:
- **Phase A**: Simple filters and selections
- **Phase B**: Joins, aggregations, and complex filters

Each query was executed and labeled with its **actual latency** using Python's `time()`.

---

## Features Extracted

Each query was parsed into the following features:

- `num_joins`
- `num_filters`
- `uses_group_by`
- `uses_aggregation`
- `query_length`
- `estimated_rows` (from rewriting query as `SELECT COUNT(*)`)
- `selectivity = estimated_rows / total_rows`

---

## Methodology

We trained a `RandomForestRegressor` from scikit-learn to predict latency based on extracted features. The model was evaluated under two conditions:
- In-distribution (train and test on Phase A)
- Out-of-distribution (train on Phase A, test on Phase B)

---

## Results

| Evaluation | R²     | MAE (ms) | RMSE (ms) |
|------------|--------|----------|-----------|
| Train/Test Split (100 queries) | -0.33 | 3.02     | 4.32      |
| Phase A (In-sample)            | ...  | ...      | ...       |
| Phase B (Out-of-distribution)  | ...  | ...      | ...       |

**Inference Example:**
- Predicted Latency: 3.36 ms
- Actual Latency: 9.58 ms

Adding `estimated_rows` improved the error by ~57%.

---

## Insights

- Structural features alone are not enough — row cardinality significantly improves latency estimates.
- Surprisingly, complex queries (Phase B) often yielded **lower prediction error** due to richer signal.
- Models trained only on simple queries underperformed on unseen complex workloads — mirroring the challenges addressed by LIMAO.

---

## Future Work

- Integrate cardinality estimators or EXPLAIN plan features
- Experiment with lifelong modular fine-tuning (e.g. per-phase adapters)
- Replace hand-crafted features with learned embeddings (SQL-BERT, etc.)

---

## How to Run

This project is implemented in a Google Colab notebook.  
Run it here: [Colab Link](https://colab.research.google.com/drive/1ZO-pRIh2Vl7ypN_hAHESHUpmBnw8WnD3?usp=sharing)

---

## 📚 References

- Zhang et al. *LIMAO: A Framework for Lifelong Modular Learned Query Optimization* (SIGMOD 2023)
