# Proactive Device Upgrade: A Machine Learning-Driven Retention Strategy for High-Value Telecom Customers

Final assignment — **GCI World 2026** (Data Science & AI Diploma, Matsuo Lab)

Predictive churn model and business retention strategy for a telecom operator in the Colombian market, using a dataset of 100,000 customers.

## 📌 Overview

Colombia's telecom market is dominated by three operators in a near-saturated, competitive landscape, where growth increasingly depends on **customer retention** rather than new subscriber acquisition. This project identifies the strongest driver of voluntary churn in a 100,000-customer dataset — **device ageing** — and translates that finding into a concrete, financially quantified retention program: a **Proactive Device Upgrade** offer targeted at high-value customers with ageing devices.

## 🧠 Business Question

> How can the company reduce voluntary churn among its highest-value customers by addressing the root cause identified in the exploratory analysis — device ageing?

## 📊 Data & Exploratory Analysis

Dataset: 100,000 telecom customers (balanced target — global churn rate of 49.6%).

**Key findings:**
- **Device age is the strongest churn signal.** Customers who churned had devices averaging **421 days old**, vs. **363 days** for retained customers (+58 days). `eqpdays` ranked as the **#1 feature by XGBoost gain**.
- **Usage decline precedes churn.** Churners used **60 fewer minutes/month** on average (483 vs. 543), signaling disengagement before cancellation.
- **No data leakage detected.** The highest feature correlation with churn was only 0.11, confirming the model learns genuine signal rather than a proxy variable.
- **Silent churn dominates.** In Q1 2025, number portability represented only 15.8% of total line withdrawals in Colombia (CRC, 2025) — the remaining 84.2% is disengagement without switching providers, which is exactly what a predictive model can catch early.

## 🎯 Target Segment Definition

Three sequential filters were applied to prioritize retention efforts:

| Filter | Criterion | Result |
|---|---|---|
| Revenue | `rev_Mean` ≥ Q75 (≥ $70.75/month) | High-value customers |
| Device | `eqpdays` > 365 days | Device older than 1 year |
| Action | `churn_proba` ≥ 0.60 | Model-flagged priority targets |

This segment (7,763 customers) shows a **55.2% churn rate**, compared to 49.1% outside the segment, with an average monthly revenue of $107.19 — a small share of the customer base that generates disproportionate revenue.

## 🤖 Model

| Parameter | Value |
|---|---|
| Algorithm | XGBoost Classifier |
| Training set | 70,000 customers (stratified split) |
| Test set | 30,000 customers |
| Estimators / Learning rate / Max depth | 300 / 0.05 / 6 |
| Random state | 42 (reproducible) |
| **Accuracy** | **~76%** (vs. 50% baseline) |
| **AUC-ROC** | **> 0.83** |
| **Precision at cutoff 0.60** | **78.9%** |

At a probability cutoff of 0.60, the model flags **2,798 priority customers**, of which 2,207 are true churners (≈8 out of every 10 flagged customers), balancing targeting precision against program reach.

## 💡 Business Proposal — Proactive Device Upgrade Program

1. **Identify** — the model scores all 100K customers weekly; the top 2,798 high-value customers with `churn_proba` ≥ 0.60 are flagged.
2. **Offer** — the retention team proactively contacts flagged customers with a device upgrade, using their old device as a trade-in.
3. **Finance** — the remaining balance is financed over 12 or 24 monthly installments, with no change to the customer's current plan.
4. **Retain** — the installment commitment creates structural retention for the duration of the financing period.

This approach mirrors standard retention practice among LatAm operators (Claro, Movistar, Tigo), while directly addressing the #1 churn driver identified in the data — and creating a financial lock-in effect beyond a one-time offer.

## 💰 Quantified Business Impact

Conservative assumptions: 2,798 clients targeted · $120 intervention cost/client · 25% success rate · $107.19 avg. monthly revenue.

| Scenario | Clients Retained | Revenue Retained | Program Cost | Net Impact | ROI |
|---|---|---|---|---|---|
| 12-month installment plan | 700 | $899,753 | $335,760 | **$563,993** | **168%** |
| 24-month installment plan | 700 | $1,799,506 | $335,760 | **$1,463,746** | **436%** |

## ⚠️ Limitations

- The 25% success rate is an assumption, not a measured outcome — a pilot would be needed to validate it.
- The model captures voluntary churn only; it does not account for involuntary churn (non-payment, address change).
- No competitor pricing or market-offer data was available, so external "pull" factors aren't captured.
- Substitution risk: a retained customer could downgrade their plan, reducing ARPU even if they don't churn.
- The dataset lacks timestamps for dynamic churn tracking; scores are point-in-time.

## ✅ Recommendation

Launch a pilot targeting the top 500 customers by predicted churn probability, measure the actual success rate, refine assumptions, and then scale to the full 2,798-customer cohort.

## 🛠️ Tech Stack

- **Python** — pandas, scikit-learn, XGBoost, matplotlib

## 📁 Repository Structure

```
├── README.md
├── notebook_churn_analysis.ipynb     # Full analysis: EDA, feature engineering, model training & evaluation
├── presentation.pdf                  # Business-facing slide deck (GCI World 2026 final assignment)
└── data/                             # (optional) dataset or data dictionary, if shareable
```

## 📚 Key References

- Comisión de Regulación de Comunicaciones (CRC). *Portabilidad Numérica Móvil — Primer semestre 2025.* 2025.
- Deloitte Colombia. *La industria de telecomunicaciones en Colombia.* 2023.
- Bancolombia. *Futuro del sector telecomunicaciones y sus retos para 2026.* April 2026.
- GoContact. *10 estrategias prácticas para reducir la tasa de abandono (churn) en las telecomunicaciones.* October 2024.

*(Full reference list available in the presentation.)*

## 👤 Author

**Deivis Alexander Maldonado Rausseo**
Industrial Engineering student, Universidad del Atlántico | Data Science & AI Diploma, GCI (Matsuo Lab)
[LinkedIn](https://www.linkedin.com/in/deivis-maldonado-5b974a3b5/) · deivismaldonado0@gmail.com
