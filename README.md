# SQL Query Latency Prediction under Workload Drift (v2)

The goal of this project is to implement a machine learning-based cost model that predicts SQL query latency under evolving workloads. Inspired by the [LIMAO (SIGMOD 2023)](https://dl.acm.org/doi/10.1145/3589317), it investigates how learned models can generalize latency prediction across query complexity shifts.

---

## Summary

- Generated **1,000+ synthetic SQL queries** over two SQLite tables: `Customers`, `Orders`
- Extracted **19 structural and cardinality-based features**, including joins, estimated rows, filter complexity, and selectivity
- Simulated **workload drift** by phasing the query set:  
  - **Phase 1**: Simple filters  
  - **Phase 2**: Joins and nested conditions  
  - **Phase 3**: Complex aggregations and subqueries
- Evaluated multiple models including **Random Forest**, **Gradient Boosting**, and **XGBoost**
- Introduced an **adaptive retraining strategy** triggered by performance degradation during drift

---

## Key Results

| Model             | R² Score | MAE (ms) | RMSE (ms) |
|------------------|----------|----------|-----------|
| XGBoost (Final)  | **0.925**  | **2.87**   | **7.59**    |
| Random Forest     | 0.899    | 3.21     | 8.45      |
| Gradient Boosting | 0.903    | 3.10     | 8.12      |

- **Top predictive features:**  
  - Estimated row count  
  - Number of filter conditions  
  - Selectivity estimate
- **Workload drift impact:**  
  - XGBoost's prediction error increased by up to **40%** across phases without retraining  
  - Adaptive retraining strategy reduced variance and improved R² by **12–18%** over static models

---

## Improvements in v2

- Higher accuracy (R² 0.925)
- Better feature engineering (selectivity, cardinality-aware filters)
- More realistic simulation of workload drift
- Adaptive learning based on error thresholding
- Modular codebase for feature extraction, model training, and evaluation

---

## Try It Yourself

This project is implemented as a Google Colab notebook:  
[▶ Open in Google Colab](https://colab.research.google.com/drive/1ZO-pRIh2Vl7ypN_hAHESHUpmBnw8WnD3?usp=sharing)

---

## Reference

Zhang et al. *LIMAO: A Framework for Lifelong Modular Learned Query Optimization*. SIGMOD 2023.  
[Read the Paper](https://dl.acm.org/doi/10.1145/3589317)
