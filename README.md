# SQL Query Latency Prediction under Workload Drift

This project builds a learned cost model that predicts SQL query latency from structural and cardinality-based features. Inspired by the LIMAO paper (SIGMOD 2023), it explores how machine learning can generalize query performance prediction under shifting workloads — a core challenge in adaptive database systems.

---

## Summary

- Generated 1,000+ synthetic SQL queries over two SQLite tables (`Customers`, `Orders`)
- Extracted 19 structural features per query, including joins, filters, estimated rows, and selectivity
- Trained and compared multiple regression models (Random Forest, XGBoost, Gradient Boosting) to predict execution latency
- Simulated workload drift across three phases (Simple → Medium → Complex)
- Implemented an adaptive learning strategy that retrains the model when error spikes

---

## Key Results

| Model               | R² Score | MAE (ms) | RMSE (ms) |
|--------------------|----------|----------|-----------|
| Random Forest       | 0.775    | 4.84     | 12.185    |
| XGBoost             | 0.798    | 4.45     | 11.548    |
| Gradient Boosting   | 0.778    | 4.97     | 12.092    |

- **Top predictive features:** query length (33.9%), IN-clause usage (16.3%)
- **Workload drift impact:** error increased by 25–130% across phases
- **Adaptive retraining** reduced performance degradation by learning from incoming queries dynamically

---

This project is implemented in a Google Colab notebook:  
[Open Colab Notebook](https://colab.research.google.com/drive/1ZO-pRIh2Vl7ypN_hAHESHUpmBnw8WnD3?usp=sharing)

---

## Reference

Zhang et al. *LIMAO: A Framework for Lifelong Modular Learned Query Optimization*. SIGMOD 2023.
