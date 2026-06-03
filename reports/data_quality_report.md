# Data Quality Report

## D2C Customer Churn Intelligence Project

### Objective

The purpose of this report is to evaluate the quality of the raw datasets before performing customer analysis, segmentation, and churn modeling. The assessment focuses on missing values, duplicate-like records, outliers, join consistency, and potential data leakage risks.

---

# 1. Dataset Overview

The project contains the following datasets:

| Dataset | Rows | Description |
|----------|----------:|----------|
| customers | 2,400 | Customer profile information |
| orders | 10,009 | Order transaction history |
| support_tickets | 1,921 | Customer support interactions |
| web_events_snapshot | 2,400 | Customer web/app activity metrics |
| churn_labels | 2,400 | Churn target labels and data splits |
| intervention_history | 2,400 | Retention campaign history |
| rfm_modeling_snapshot | 2,400 | Pre-engineered modeling features |

No major loading issues were identified during ingestion.

---

# 2. Missing Value Assessment

The following missing values were identified:

| Dataset | Column | Missing Count | Business Interpretation |
|----------|----------|----------:|----------|
| customers | loyalty_tier | 1,386 | Customer is not enrolled in loyalty program |
| customers | skin_type | 401 | Customer did not provide profile information |
| orders | rating | 80 | Customer did not submit a product rating |

### Assessment

The missing values appear to be business-driven rather than system errors.

### Recommended Treatment

- Treat missing `loyalty_tier` as **"Not Enrolled"**.
- Treat missing `skin_type` as **"Unknown"**.
- Preserve missing order ratings rather than imputing arbitrary values.

---

# 3. Duplicate-Like Records

A review of order identifiers identified:

**12 duplicate-like order records**

These records are identifiable through the `_DUP` suffix in the `order_id` field.

### Assessment

The duplicates appear to be intentionally introduced for data-quality evaluation and may represent common real-world deduplication challenges.

### Recommended Treatment

- Investigate duplicate business keys before modeling.
- Exclude confirmed duplicates from future production pipelines if they represent replicated transactions.
- Document duplicate-handling logic to ensure reproducibility.

---

# 4. Outlier Assessment

Order values (`gross_amount`) were analyzed using distribution plots and boxplots.

### Findings

- Most orders fall within a reasonable spending range.
- A small number of unusually high-value transactions are present.
- Maximum order values are substantially larger than the median order value.

### Assessment

These observations may represent:

- legitimate high-value purchases,
- bulk purchases,
- premium product combinations,
- potential data-entry anomalies.

### Recommended Treatment

- Retain outliers during exploratory analysis.
- Evaluate winsorization or log transformation during predictive modeling if required.
- Review extreme values individually before removal.

---

# 5. Join Integrity Validation

Customer identifiers were validated across all datasets.

### Findings

| Dataset | Unmatched Customer IDs |
|----------|----------:|
| orders | 0 |
| support_tickets | 0 |
| web_events_snapshot | 0 |
| churn_labels | 0 |
| intervention_history | 0 |
| rfm_modeling_snapshot | 0 |

### Assessment

Referential integrity is fully preserved across all provided datasets.

### Recommended Treatment

No corrective action is required.

---

# 6. Date Consistency Review

The project uses a customer snapshot date of:

**2025-09-30**

Order history contains both:

- pre-snapshot transactions,
- post-snapshot transactions.

### Findings

| Category | Count |
|----------|----------:|
| Pre-snapshot orders | 8,137 |
| Post-snapshot orders | 1,872 |

### Assessment

Post-snapshot transactions exist solely to support churn-label generation and must not be used as predictive features.

### Recommended Treatment

All behavioral features should be generated using only records on or before the snapshot date.

---

# 7. Leakage Risk Assessment

### Identified Risk

Orders occurring after the snapshot date contain information from the churn target window.

Using these transactions as model features would expose future information to the model and artificially inflate model performance.

### Recommended Control

For all feature engineering, segmentation, and predictive modeling:

> Use only records dated on or before **2025-09-30**.

This control should be enforced throughout Parts 2, 3, and 4 of the project.

---

# 8. Overall Data Quality Assessment

| Area | Assessment |
|----------|----------|
| Missing Values | Acceptable |
| Duplicate-Like Records | Minor Issue |
| Outliers | Moderate |
| Join Integrity | Excellent |
| Date Consistency | Good |
| Leakage Risk | High if Unmanaged |

---

# Final Conclusion

The datasets are generally suitable for customer analytics, retention analysis, and churn prediction. Most data-quality issues are manageable and reflect realistic business scenarios rather than severe data corruption.

The most important risk identified is the presence of post-snapshot transactions, which could introduce target leakage if used incorrectly. Aside from this risk, the datasets demonstrate strong referential integrity, reasonable completeness, and sufficient quality for advanced customer analytics.

### Key Recommendations

1. Enforce snapshot-date filtering before any feature engineering.
2. Explicitly handle missing loyalty and profile attributes.
3. Investigate duplicate-like order records before production deployment.
4. Review extreme order values before modeling.
5. Maintain documented data-validation checks in future analytical workflows.