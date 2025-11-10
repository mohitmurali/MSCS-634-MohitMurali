# MSCS_634_Lab_3 - Clustering Analysis on Wine Dataset  
**K-Means vs K-Medoids Comparison**

**Author**: Mohit Gokul Murali  
**Course**: Advanced Big Data and Data Mining (MSCS-634-B01)  
**Lab**: Lab 3 – Clustering Analysis Using K-Means and K-Medoids  
**Date**: November 09, 2025  

---

## Purpose

This lab explores **unsupervised clustering** using the **Wine Dataset** from `scikit-learn`. The goal is to:

1. Apply **K-Means** and **K-Medoids** clustering with `k=3` (matching the 3 true wine cultivars).
2. Evaluate cluster quality using:
   - **Silhouette Score** (internal cohesion & separation)
   - **Adjusted Rand Index (ARI)** (agreement with ground truth labels)
3. Visualize clusters in 2D using **PCA**
4. Compare algorithm performance and interpret results

---

## Dataset Overview

| Attribute | Details |
|---------|--------|
| **Source** | `sklearn.datasets.load_wine()` |
| **Samples** | 178 |
| **Features** | 13 (chemical properties: alcohol, malic acid, etc.) |
| **Classes** | 3 (cultivars: class_0, class_1, class_2) |
| **Class Distribution** | `[59, 71, 48]` |
| **Missing Values** | None |
| **Preprocessing** | Z-score standardization (zero mean, unit variance) |

> **Why standardize?**  
> Distance-based algorithms (K-Means, K-Medoids) are sensitive to feature scales. Standardization ensures equal contribution.

---

## Methodology

### Step 1: Data Preparation
```python
StandardScaler() → X_scaled
```
- No missing values or outliers requiring removal
- Confirmed via `.info()`, `.describe()`, and `.isnull().sum()`

### Step 2: K-Means Clustering
```python
KMeans(n_clusters=3, random_state=42)
```
- Minimizes **within-cluster variance** (squared Euclidean distance)
- Centroids may lie outside data points

### Step 3: K-Medoids Clustering
```python
Custom PAM-style implementation (no sklearn_extra)
```
- Uses **Manhattan distance** and selects **actual data points** as medoids
- More robust to outliers
- Avoids NumPy 2.0 compatibility issues

### Step 4: Evaluation & Visualization
- **PCA** → 2D projection
- Side-by-side scatter plots with:
  - Cluster assignments (color)
  - Centroids (K-Means: red circles)
  - Medoids (K-Medoids: red X's)

---

## Results Summary

| Algorithm   | Silhouette Score | ARI       |
|-------------|------------------|-----------|
| **K-Means**     | **0.2849**           | **0.8975** |
| K-Medoids   | 0.2660           | 0.7263    |

> **Interpretation**:
> - **ARI near 0.9** → K-Means **nearly perfectly recovers** the 3 true wine classes.
> - **Silhouette ~0.28** → Clusters are **detectable but overlapping** (expected in real chemical data).
> - K-Means **outperforms** K-Medoids on **both metrics**.

---

## Visualizations

![Clustering Comparison](images/clustering_comparison.png)  
*(Generated via PCA; saved in `images/` folder)*

### Key Observations from Plot:
- **K-Means**: Forms **compact, spherical clusters** with centroids near geometric centers.
- **K-Medoids**: Medoids are **real data points**, sometimes on cluster edges.
- One cluster (likely class_1) shows **slight fragmentation** in K-Medoids → explains lower ARI.

---

## Detailed Comparison

| Criterion                  | K-Means           | K-Medoids           |
|---------------------------|-------------------|---------------------|
| **Objective**             | Minimize variance | Minimize median distance |
| **Cluster Representative**| Mean (can be virtual) | Actual data point |
| **Outlier Robustness**    | Low               | High                |
| **Speed**                 | Faster (Lloyd’s)  | Slower (PAM-style)  |
| **Performance on Wine**   | **Superior**      | Inferior            |

### When to Use Each:
| Use Case                            | Recommended Algorithm |
|-------------------------------------|------------------------|
| Clean, Gaussian-like data           | **K-Means**            |
| Outliers or noisy features          | **K-Medoids**          |
| Need interpretable cluster centers  | **K-Medoids**          |
| Large datasets                      | **K-Means**            |

---

## Key Insights

1. **ARI > Silhouette for labeled data**:  
   Even with modest Silhouette (~0.28), **ARI = 0.90** proves excellent recovery of true structure.

2. **K-Means excels on Wine dataset**:  
   Clusters are roughly spherical and outlier-free → ideal for variance minimization.

3. **K-Medoids is more conservative**:  
   Prioritizes robustness over tight fit → splits or merges clusters suboptimally here.

4. **PCA reveals overlap**:  
   First two components explain ~55% variance → some natural mixing between classes.

---

## Challenges Faced & Solutions

| Challenge | Solution |
|---------|----------|
| `sklearn_extra` failed with **NumPy 2.0** | Implemented **custom K-Medoids** using `pairwise_distances(..., metric='manhattan')` |
| Reproducibility | Set `random_state=42` in all algorithms |
| Visualization clarity | Used PCA, distinct colormaps (`viridis` vs `plasma`), and marked centroids/medoids |
| Low Silhouette confusion | Explained via **ARI priority** and **real-world data overlap** |

---

## Requirements

```bash
pip install scikit-learn matplotlib seaborn pandas numpy jupyter
```

> **Note**: No `scikit-learn-extra` required — custom K-Medoids avoids dependency issues.

---

## Conclusion

**K-Means is the clear winner** on the Wine dataset due to:
- Higher **Silhouette Score**
- Near-perfect **ARI (0.90)**
- Faster computation
- Better visual cluster separation

**K-Medoids remains valuable** in noisy or outlier-heavy datasets, but **not necessary here**.

---