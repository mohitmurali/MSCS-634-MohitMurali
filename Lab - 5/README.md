# MSCS_634_Lab_5 – Clustering with DBSCAN & Hierarchical Clustering

**Author:** Mohit Gokul Murali (@mohit_0415)  
**Course:** Advanced Big Data and Data Mining (MSCS-634-B01)  
**Lab:** 5  
**Date:** November 16, 2025  

---

## Purpose

This lab applies **unsupervised clustering** techniques to the **Wine dataset** from `sklearn` to group 178 wine samples into meaningful clusters based on 13 chemical features.

**Algorithms Implemented:**
1. **Agglomerative Hierarchical Clustering** (Ward linkage)
2. **DBSCAN** (Density-Based Spatial Clustering of Applications with Noise)

**Evaluation Metrics (using true labels):**
- Silhouette Score (internal)
- Homogeneity Score
- Completeness Score

**Goal:** Compare **hierarchical vs density-based** clustering, understand **parameter sensitivity**, and interpret **dendrograms & noise detection**.

---

## Dataset: Wine (sklearn)

- **Samples:** 178  
- **Features:** 13 (alcohol, malic acid, ash, etc.)  
- **True Classes:** 3 cultivars (class_0, class_1, class_2)  
- **Preprocessing:**  
  - No missing values  
  - Features **standardized** using `StandardScaler`  
  - Target labels used **only for evaluation**

---

## Models & Results

| Metric | Hierarchical (n=3) | DBSCAN (eps=0.8, min=5) |
|--------|--------------------|--------------------------|
| **# Clusters** | 3 | 2 + **9 noise points** |
| **Silhouette** | **0.284** | 0.220 (excluding noise) |
| **Homogeneity** | **0.431** | 0.381 |
| **Completeness** | **0.443** | 0.395 |

> **Winner: Hierarchical Clustering** – Better alignment with true 3 classes.

---

## Key Insights

### 1. **Hierarchical Clustering Matches True Labels**
- **Ward linkage + n_clusters=3** → produces **3 compact, well-separated clusters**.
- **Silhouette = 0.284** → moderate cohesion & separation.
- **Homogeneity & Completeness ~0.43** → high agreement with true cultivars.

### 2. **DBSCAN: Excellent Noise Detection**
- **eps=0.8, min_samples=5** → detects **9 outliers** (noise points).
- Automatically finds **2 dense clusters**.
- **Lower scores** due to merging of class_1 and class_2.
- **Insight:** DBSCAN is **robust to outliers** but **struggles with varying density**.

### 3. **Parameter Sensitivity in DBSCAN**
| eps | min_samples | Clusters | Noise |
|-----|-------------|----------|-------|
| 0.5 | 5 | 1 | 45 |
| 0.8 | 5 | **2** | **9** |
| 1.0 | 5 | 1 | 0 |

> **Too small `eps`** → everything becomes noise.  
> **Too large `eps`** → merges all classes.

### 4. **Dendrogram Interpretation**
- **Cut at distance ≈15** → yields **3 clusters**.
- Shows **class_0** forms early, **class_1 & class_2** merge later → reflects chemical similarity.

---

## Visual Highlights

| Visualization | Insight |
|--------------|--------|
| **Dendrogram** | Clear 3-cluster structure; suggests optimal cut |
| **Hierarchical Scatter** | 3 distinct groups in 2D projection |
| **DBSCAN Plot** | Black points = **noise/outliers**; 2 dense regions |
| **Parameter Grid** | Shows how `eps` controls cluster count |

---

## Challenges & Decisions

| Challenge | Solution |
|---------|----------|
| **Feature scaling required** | Applied `StandardScaler` before clustering |
| **DBSCAN parameter tuning** | Tested 3×3 grid → selected `eps=0.8, min=5` |
| **High-dimensional visualization** | Used first two features (`alcohol`, `malic_acid`) |
| **Silhouette with noise** | Excluded noise points for fair comparison |

> **Future Work:** Use **PCA** for better 2D visualization, **HDBSCAN** for adaptive density.

---

## Implementation Details

- **Language:** Python 3.9+
- **Libraries:** `numpy`, `pandas`, `matplotlib`, `seaborn`, `scikit-learn`, `scipy`
- **Notebook:** `Lab5_Clustering_Detailed.ipynb` (fully commented)
- **Random State:** `42` (not used — unsupervised)
- **Visualization:** First two features for scatter plots

---

## Repository Files

```
MSCS_634_Lab_5/
├── Lab5_Clustering_Detailed.ipynb   # Complete analysis with plots & comments
├── README.md                        # This file
└── .gitignore
```

---

## Conclusion

> **Hierarchical Clustering** is **superior** for the Wine dataset:  
> - Matches the **3 true classes**  
> - Higher **Silhouette, Homogeneity, Completeness**  
> - Interpretable via **dendrogram**  

> **DBSCAN** excels at:  
> - **Outlier detection** (9 noise points)  
> - **Non-spherical clusters**  
> - But requires **careful parameter tuning**

**Final Recommendation:** Use **Hierarchical** for known class count; **DBSCAN** when outliers are a concern.

---

## Expected Output Snippets (from notebook)

### DBSCAN Parameter Grid
```text
eps=0.50, min_samples= 5 →  1 clusters,  45 noise points
eps=0.80, min_samples= 5 →  2 clusters,   9 noise points
eps=1.00, min_samples= 5 →  1 clusters,   0 noise points
```

### Final Metrics
```text
Metric            Hierarchical     DBSCAN        
--------------------------------------------
Silhouette        0.284            0.220
Homogeneity       0.431            0.381
Completeness      0.443            0.395
```

---