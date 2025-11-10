# MSCS_634_Lab_2 – Classification Using KNN and Radius Neighbors (RNN) on Wine Dataset

**Author:** Mohit Gokul Murali  
**Course:** Advanced Big Data and Data Mining (MSCS-634-B01)  
**Date:** November 2025  

---

## Objective

Implement and compare **K-Nearest Neighbors (KNN)** and **Radius Neighbors Classifier (RNN)** on the **Wine Dataset** from `sklearn`.  
Evaluate the impact of hyper-parameters (`k` and `radius`) on classification accuracy after **feature scaling**, and determine:
- Which model performs better?
- How sensitive is each model to its hyper-parameter?
- When should one prefer KNN vs. RNN?

---

## Dataset Overview

| Property | Details |
|--------|--------|
| **Source** | `sklearn.datasets.load_wine()` |
| **Samples** | 178 |
| **Features** | 13 (chemical properties: alcohol, malic_acid, proline, etc.) |
| **Classes** | 3 types of wine (class_0: 59, class_1: 71, class_2: 48) |
| **Task** | Multi-class classification |

> **Note**: The dataset is **clean**, **balanced**, and **highly separable** after scaling.

---

## Methodology

### 1. **Data Preparation**
```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, stratify=y, random_state=42)
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```
- **Stratified split** preserves class distribution.
- **StandardScaler** ensures all features contribute equally to Euclidean distance.

---

### 2. **KNN Implementation**
```python
k_values = [1, 5, 11, 15, 21]
```
- Trained `KNeighborsClassifier` with **Euclidean distance**.
- Evaluated accuracy on test set.

---

### 3. **RNN Implementation**
```python
radius_values = [2.0, 2.5, 3.0, 3.5, 4.0, 4.5]
```
- Used `RadiusNeighborsClassifier` with `outlier_label='most_frequent'`.
- **Radius tuned to scaled data** (average distance ≈ 3.0–4.0).

> **Critical Insight**: Original lab radii (350–600) were **invalid** after scaling → caused fallback to majority class → ~0.39 accuracy.

---

### 4. **Evaluation & Visualization**
- Accuracy recorded for each hyper-parameter.
- Three plots: KNN trend, RNN trend, combined comparison.
- Summary table with side-by-side results.

---

## Key Results

### Accuracy Summary Table

| KNN_k | KNN_Accuracy | RNN_Radius | RNN_Accuracy |
|-------|--------------|------------|--------------|
| 1     | 0.9722       | 2.0        | 0.8611       |
| 5     | 0.9722       | 2.5        | **0.9722**   |
| 11    | **1.0000**   | 3.0        | **0.9722**   |
| 15    | **1.0000**   | 3.5        | **1.0000**   |
| 21    | **1.0000**   | 4.0        | 0.9444       |
| -     | -            | 4.5        | 0.9167       |

> **Best KNN**: `k = 11, 15, 21` → **100% accuracy**  
> **Best RNN**: `radius = 3.5` → **100% accuracy**

---

## Hyper-parameter Sensitivity

### KNN
- **Robust**: Accuracy stays high from `k=5` onward.
- `k=1` → slight drop (overfitting risk).
- `k≥11` → **perfect classification**.

### RNN
- **Highly sensitive**:
  - `radius < 2.0` → too few neighbors → fallback to majority class.
  - `radius > 4.0` → too many neighbors → under-fitting.
  - **Sweet spot**: `2.5 ≤ radius ≤ 3.5`

---

## Model Comparison

| Criterion | KNN | RNN |
|--------|-----|-----|
| **Ease of Tuning** | 5/5 (integer `k`) | 2/5 (radius depends on scale) |
| **Interpretability** | Nearest points | Fixed-distance ball |
| **Best Accuracy** | 100% | 100% |
| **Robustness** | High | Low (if radius mischosen) |
| **Use Case** | General tabular data | When fixed distance threshold is meaningful (e.g., geospatial) |

> **Winner**: **KNN** – simpler, more robust, same peak performance.

---

## Visualizations

1. **KNN Accuracy vs. k**  
   ![KNN Plot](https://via.placeholder.com/600x400?text=KNN+Accuracy+vs+k)  
   → Plateau at 100% for `k ≥ 11`.

2. **RNN Accuracy vs. Radius**  
   ![RNN Plot](https://via.placeholder.com/600x400?text=RNN+Accuracy+vs+Radius)  
   → Bell-shaped curve, peak at `radius = 3.5`.

3. **Combined Comparison**  
   ![Combined](https://via.placeholder.com/600x400?text=KNN+vs+RNN)  
   → Both reach 100%, but KNN is flatter.

> *(Screenshots of plots are included in the notebook. GitHub renders them inline.)*

---

## Key Insights & Recommendations

1. **Feature scaling is non-negotiable** for distance-based models.
2. **Hyper-parameter search space must be defined in the scaled feature space**.
3. **KNN is the default choice** for small-to-medium tabular classification.
4. **Use RNN only when**:
   - A natural distance threshold exists (e.g., "within 5 km").
   - You want to control neighborhood size explicitly.
5. **Always inspect a few pairwise distances** after scaling:
   ```python
   np.linalg.norm(X_train_scaled[0] - X_train_scaled[1])  # ~3.5
   ```

---

## Challenges Faced & Solutions

| Challenge | Solution |
|---------|----------|
| RNN gave ~0.39 accuracy with `radius=350–600` | Realized radii were **invalid after scaling** → used distances to pick `2.0–4.5` |
| `outlier_label` triggered too often | Increased radius until most points had neighbors |
| Summary table had mismatched lengths | Used `pd.concat([df1, df2], axis=1)` instead of list padding |
| Syntax error in observations | Escaped code block with triple single quotes `'''` |

---

**Conclusion**: Both KNN and RNN can achieve **perfect classification** on the Wine dataset, but **KNN is superior in practice** due to ease of use and robustness.

