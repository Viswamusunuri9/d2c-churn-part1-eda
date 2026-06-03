# D2C Customer Churn Intelligence & Retention
## Part 1 – Data Audit, EDA & Business Understanding

---

## Project Overview

This repository contains the exploratory data analysis (EDA), data-quality assessment, and business understanding phase of a customer churn analytics project for a Direct-to-Consumer (D2C) personal-care brand.

The objective of this phase is to:

- understand customer behavior,
- assess data quality,
- identify churn-risk patterns,
- generate business hypotheses,
- provide recommendations for future retention initiatives.

This work serves as the foundation for customer segmentation, churn prediction modeling, and retention strategy development in subsequent project phases.

---

## Business Context

The company wants to reduce customer churn without offering discounts indiscriminately.

A data-driven understanding of customer purchasing behavior, engagement patterns, support interactions, and campaign history is required before designing retention interventions.

---

## Repository Structure

```text
.
├── charts/
│   ├── churn_distribution.png
│   ├── frequency_vs_churn.png
│   ├── loyalty_vs_churn.png
│   ├── recency_vs_churn.png
│   └── sessions_vs_churn.png
│
├── notebooks/
│   └── eda_audit.ipynb
│
├── reports/
│   ├── data_quality_report.md
│   └── business_memo.md
│
├── requirements.txt
└── README.md
```

---

## Dataset

The analysis uses the following datasets:

- customers.csv
- orders.csv
- support_tickets.csv
- web_events_snapshot.csv
- churn_labels.csv
- intervention_history.csv
- rfm_modeling_snapshot.csv

### Snapshot Date

```text
2025-09-30
```

### Important Leakage Rule

Only data available on or before the snapshot date is used for behavioral analysis.

Orders occurring after the snapshot date were identified and excluded from customer behavior analysis because they belong to the target window used to define churn.

---

## Key Data Quality Findings

### Missing Values

| Column | Missing Count |
|----------|----------:|
| loyalty_tier | 1,386 |
| skin_type | 401 |
| rating | 80 |

### Duplicate-Like Records

- 12 duplicate-like order records identified.

### Post-Snapshot Orders

| Category | Count |
|----------|----------:|
| Pre-Snapshot Orders | 8,137 |
| Post-Snapshot Orders | 1,872 |

### Join Integrity

- No unmatched customer IDs were identified across datasets.

---

## Key Business Findings

### Finding 1

Customers who churned exhibited significantly higher recency compared to retained customers.

### Finding 2

Customers with lower purchase frequency were substantially more likely to churn.

### Finding 3

Retained customers generated higher monetary value than churned customers.

### Finding 4

Lower web/app engagement was associated with increased churn risk.

### Finding 5

Support ticket volume alone was not a reliable indicator of churn risk.

---

## Churn Risk Hypotheses

The analysis produced the following churn hypotheses:

1. Customers with longer periods since their last purchase are more likely to churn.
2. Lower purchase frequency is associated with increased churn risk.
3. Lower customer spend is associated with increased churn risk.
4. Reduced digital engagement is associated with increased churn risk.
5. Customer engagement may be more important than support-ticket volume alone.

---

## How to Run

### 1. Clone Repository

```bash
git clone <repository-url>
cd d2c-churn-part1-eda
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Dataset Placement

Place all CSV files inside the shared project data folder:

```text
../../data/
```

or update the notebook paths as required.

### 4. Launch Jupyter Notebook

```bash
jupyter lab
```

Open:

```text
notebooks/eda_audit.ipynb
```

and execute all cells.

---

## Outputs

### Notebook

- eda_audit.ipynb

### Reports

- data_quality_report.md
- business_memo.md

### Visualizations

- churn_distribution.png
- loyalty_vs_churn.png
- recency_vs_churn.png
- frequency_vs_churn.png
- sessions_vs_churn.png

---

## Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Jupyter Notebook

---

## Conclusion

The analysis indicates that churn is primarily associated with declining purchasing behavior and reduced customer engagement. Customers showing increasing purchase recency, declining order frequency, and lower digital activity should be prioritized for retention efforts before they become inactive.
