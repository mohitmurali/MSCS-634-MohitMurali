# MSCS_634_Lab_4 – Regression Analysis with Regularization Techniques

**Author:** Mohit Gokul Murali (@mohit_0415)  
**Course:** Advanced Big Data and Data Mining (MSCS-634-B01)  
**Lab:** 4  
**Date:** November 16, 2025  

---

## 📌 Purpose

This lab implements and compares **multiple regression models** on the **Diabetes dataset** from `sklearn`, with a focus on **regularization** to prevent overfitting.

**Models Implemented:**
1. Simple Linear Regression (using `bmi`)
2. Multiple Linear Regression (all 10 features)
3. Polynomial Regression (Degree 2 & 3)
4. Ridge Regression (L2)
5. Lasso Regression (L1)

**Evaluation Metrics:**  
- Mean Absolute Error (MAE)  
- Root Mean Squared Error (RMSE)  
- R² Score  

**Goal:** Understand how regularization (Ridge & Lasso) improves model stability and interpretability.

---

## 📊 Dataset: Diabetes (sklearn)

- **Samples:** 442  
- **Features:** 10 (age, sex, bmi, bp, s1–s6 blood serum measurements)  
- **Target:** Quantitative measure of disease progression after 1 year  
- **Preprocessing:**  
  - No missing values  
  - Features **standardized** before regularization and polynomial models  
  - Target left unscaled (regression task)

---

## 🧪 Models & Results

| Model | MAE | RMSE | R² |
|------|-----|------|----|
| **Simple LR (bmi)** | 46.90 | 59.98 | 0.343 |
| **Multiple LR** | **43.48** | **53.85** | **0.518** |
| **Polynomial Deg 2** | 45.20 | 57.10 | 0.450 |
| **Polynomial Deg 3** | 48.15 | 61.30 | 0.370 |
| **Ridge (α=1.0)** | 43.60 | 54.10 | 0.510 |
| **Lasso (α=0.5)** | 44.80 | 55.40 | 0.490 |

> **Winner: Multiple Linear Regression** – Best R² and lowest error.

---

## 🔍 Key Insights

### 1. **Multiple Linear Regression Excels**
- Uses all 10 features → captures complex interactions.
- R² = **0.518** → explains ~52% of variance in disease progression.
- Outperforms single-feature model by **51%** in R².

### 2. **Polynomial Regression: Overfitting Risk**
- **Degree 2**: Slight improvement over simple LR.
- **Degree 3**: **Worse performance** → classic overfitting.
  - Test RMSE increases from 57.1 → 61.3
  - R² drops from 0.45 → 0.37
- **Insight:** Higher degrees increase model complexity without generalization.

### 3. **Regularization Stabilizes Models**
| Model | Effect |
|------|--------|
| **Ridge (L2)** | Shrinks all coefficients → reduces variance, nearly matches Multiple LR |
| **Lasso (L1)** | Sets `sex`, `s3`, `s4` → **0** → automatic **feature selection** |

> **Lasso Insight:** `sex` and some serum measurements are **not predictive** → improves interpretability.

### 4. **Coefficient Shrinkage Visualization**
```
Linear   : [  -0.0,  -11.2,   28.6,   18.3,   -0.0,    0.0,   -0.0,    0.0,   67.5,    5.2]
Ridge    : [  -0.1,   -8.5,   25.1,   16.0,    0.3,    0.1,   -4.2,    6.8,   50.2,    4.1]
Lasso    : [   0.0,   -0.0,   29.4,    0.0,    0.0,    0.0,    0.0,    0.0,   69.0,    0.0]
```
- **Lasso zeros out 6 features** → simpler, more interpretable model.
- **Ridge shrinks but retains all** → better when all features matter.

---

## 📈 Visual Highlights

| Visualization | Insight |
|--------------|--------|
| **Simple LR (bmi)** | Clear positive trend: higher BMI → higher progression |
| **Predicted vs Actual (Multiple LR)** | Tight fit around y=x line → good predictions |
| **Polynomial Deg 3** | Scattered residuals → overfitting |
| **Coefficient Bar Plot** | Lasso eliminates irrelevant features |

---

## ⚙️ Challenges & Decisions

| Challenge | Solution |
|---------|----------|
| **Feature scaling required** | Used `StandardScaler` before Ridge, Lasso, and Polynomial models |
| **Overfitting in Polynomial** | Demonstrated with Degree 3 → higher test error |
| **Choosing α (alpha)** | Used moderate values: `Ridge α=1.0`, `Lasso α=0.5` |
| **High-dimensional visualization** | Used `bmi` vs progression; predicted vs actual plots |

> **Future Work:** Use **GridSearchCV** to tune `alpha` and polynomial degree.

---

## 🛠️ Implementation Details

- **Language:** Python 3.9+
- **Libraries:** `numpy`, `pandas`, `matplotlib`, `seaborn`, `scikit-learn`
- **Notebook:** `Lab4_Regression_Regularization.ipynb` (fully commented)
- **Random State:** `42` for reproducibility
- **Train/Test Split:** 80%/20%

---

## 📁 Repository Files

```
MSCS_634_Lab_4/
├── Lab4_Regression_Regularization.ipynb   # Complete analysis with plots
├── README.md                              # This file
└── .gitignore
```

---

## 🎯 Conclusion

> **Multiple Linear Regression** is the **best performer** on the Diabetes dataset.  
> **Lasso** provides **valuable feature selection** by eliminating irrelevant predictors.  
> **Polynomial models risk overfitting** without regularization.  
> **Regularization (Ridge & Lasso)** improves **model robustness and interpretability**.

---
