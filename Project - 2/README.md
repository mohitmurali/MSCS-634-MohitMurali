**Student:** Mohit Gokul Murali  
**Course:** Advanced Big Data and Data Mining (MSCS-634-B01)  
**Instructor:** Satish (s-pen-uc)  
**Date:** November 16, 2025  

---

# MSCS 634 – Project Deliverable 2  
**Regression Modeling and Performance Evaluation**

---

## Dataset Summary
**Name:** Adult Income Dataset  
**Source:** UCI Machine Learning Repository  
**Size:** 48,842 records × 14 attributes  
**Target Variable:** `income_numeric` → **0 (`<=50K`), 1 (`>50K`)**  
**Objective:** Predict **probability of earning >$50K** using linear regression models

---

## Modeling Process Overview

| Step | Description |
|------|-----------|
| 1 | Load and clean raw data (from Deliverable 1 pipeline) |
| 2 | Feature engineering guided by EDA insights |
| 3 | Train **Ridge** and **Lasso** regression models |
| 4 | Perform **5-fold cross-validation** |
| 5 | Evaluate using **R², MSE, RMSE** |
| 6 | Compare performance and interpret coefficients |

---

## Feature Engineering (Driven by EDA Insights)

| Feature | Transformation | Rationale |
|-------|----------------|---------|
| `education_num` | Retained as-is | Strongest linear predictor |
| `age`, `hours_per_week` | Added `age²`, `hours²`, `age × hours` | Capture non-linear and interaction effects |
| `capital_gain`, `capital_loss` | Created `has_capital_gain`, `log_cap_gain`, `log_cap_loss` | Handle zero-inflation and skew |
| `occupation` | One-hot encoded top 6; rest → `Other` | Reduce cardinality, retain predictive power |
| `native_country_grouped` | One-hot (6 groups) | Group rare countries (<1%) |
| `sex`, `marital_status` | One-hot encoded | Binary/low-cardinality |
| `workclass` | One-hot top 4 + `Other` | Balance granularity and stability |

> **Total Engineered Features:** **28**

---

## Models Developed

| Model | Regularization | Best α (via GridSearchCV) |
|-------|----------------|----------------------------|
| **Ridge Regression** | L2 | **1.0** |
| **Lasso Regression** | L1 | **0.0005** |

---

## Model Performance (5-Fold Cross-Validation)

| Model | R² (mean) | MSE (mean) | RMSE (mean) |
|-------|-----------|------------|-------------|
| **Ridge** | **0.412** | 0.134 | **0.366** |
| Lasso | 0.408 | 0.136 | 0.369 |

> **Ridge is the superior model**, achieving higher R² and lower error.

---

## Key Insights & Observations

1. **Ridge outperforms Lasso** due to **multicollinearity** among interaction terms (`age`, `hours`, `age×hours`) — L2 regularization stabilizes coefficients.  
2. **Top predictors (Ridge coefficients):**  
   - `education_num` → **+0.28** (strongest driver)  
   - `has_capital_gain` → **+0.19**  
   - `occ_Exec-managerial` → **+0.14**  
3. **Interaction term `age × hours_per_week`** improves R² by **~0.03** — validates EDA hypothesis.  
4. **Lasso performs automatic feature selection**, zeroing out 4 weak predictors.  
5. **Model explains ~41% of variance** in income probability; RMSE ≈ 0.366 indicates good generalization.  
6. **Probability outputs** from regression will serve as strong features for classification in Deliverable 3.

---

## Challenges & Solutions

| Challenge | Impact | Solution |
|---------|--------|----------|
| High multicollinearity in polynomial & interaction terms | Unstable coefficients | **Ridge (L2)** regularization |
| Risk of high-dimensional one-hot encoding | Overfitting | **Top-k + Other** grouping |
| Binary target in regression context | Non-normal residuals | Interpreted as **probability prediction** (valid for logistic-like use) |
| Hyperparameter selection | Model instability | **GridSearchCV** with 5-fold CV |

---

**Deliverable 2 is complete, self-contained, and fully reproducible.**  
All code, visualizations, and results are documented in the accompanying Jupyter notebook.

---
