# Lab 6: Association Rule Mining with Apriori and FP-Growth

**Name:** Mohit Gokul Murali  
**Course:** Advanced Big Data and Data Mining (MSCS-634-B01)  
**Date:** Nov 30, 2025  

## Overview

This lab explores **association rule mining** using two classical algorithms: **Apriori** and **FP-Growth**. The goal is to uncover **frequent itemsets** and **association rules** from transactional data, and to analyze these patterns using visualizations. The lab demonstrates how these techniques can provide insights into customer purchasing behavior and how algorithm choice affects efficiency and performance.

The **Online Retail dataset** from the UCI Machine Learning Repository was used, containing transaction-level purchase data for a UK-based online retail store.

---

## Step 1: Data Preparation

### Actions Taken
- Loaded the dataset directly from the UCI repository.
- Cleaned the data by:
  - Removing rows with missing values in `InvoiceNo`, `Description`, or `Quantity`.
  - Removing cancelled transactions (`InvoiceNo` starting with "C").
  - Keeping only positive quantities.
- Created a **transaction matrix** with items as columns and invoices as rows.
- Converted the matrix to **boolean** (`True` if purchased, `False` otherwise) to optimize frequent pattern mining.
- Explored the dataset:
  - Visualized the **top 10 most frequent items**.
  - Identified patterns in purchase frequency.

### Observations
- Some items are purchased significantly more than others.
- Data cleaning was critical to remove cancelled orders and ensure accurate mining.

---

## Step 2: Frequent Itemset Mining Using Apriori

### Actions Taken
- Applied **Apriori algorithm** with `min_support=0.02`.
- Extracted **frequent itemsets** and sorted them by support.
- Visualized the **top 10 frequent itemsets** using a Seaborn barplot.

### Observations
- Frequent combinations include popular items often purchased together.
- Apriori correctly identifies itemsets but can be slower on large datasets due to candidate generation overhead.

---

## Step 3: Frequent Itemset Mining Using FP-Growth

### Actions Taken
- Applied **FP-Growth algorithm** with the same `min_support=0.02`.
- FP-Growth uses a **compact FP-tree** structure to efficiently find frequent patterns.
- Visualized the **top 10 frequent itemsets** with Seaborn.
- Compared **runtime performance** with Apriori.

### Observations
- FP-Growth is **faster than Apriori**, especially for larger datasets.
- Both algorithms produce **similar frequent itemsets** when using the same support threshold.
- Visualizations confirm consistent patterns with Apriori results.

---

## Step 4: Association Rule Generation

### Actions Taken
- Generated **association rules** from frequent itemsets using `confidence >= 0.5`.
- Calculated **support, confidence, and lift** for each rule.
- Visualized rules with a **scatter plot (confidence vs. lift)**, sizing points by support.

### Insights
- High-confidence rules often involve frequently purchased items.
- Lift highlights rules with **strong associations** beyond random chance.
- Rules can inform marketing strategies, promotions, or recommendation systems.

---

## Step 5: Comparative Analysis

| Algorithm   | Runtime (seconds) | Observations |
|------------|-----------------|--------------|
| Apriori    | [Measured Value] | Candidate generation can be slow; easy to interpret. |
| FP-Growth  | [Measured Value] | Faster; handles larger datasets efficiently; same results. |

### Challenges Faced
- Initial errors in barplot visualization due to Seaborn not recognizing column names.  
- Type issues with `InvoiceNo` required conversion to string.  
- Boolean conversion was necessary to remove mlxtend warnings.  

### How Challenges Were Resolved
- Updated barplots to pass a DataFrame (`data=`) and convert itemsets to strings.  
- Cleaned and preprocessed data to ensure compatibility with both algorithms.  
- Converted transaction matrix to boolean type for efficient computation.

---

## Key Insights

1. **Frequent Itemsets:** Most customers tend to purchase popular items in combination (e.g., “WHITE HANGING HEART T-LIGHT HOLDER” appears in many baskets).  
2. **Association Rules:** Rules with high lift and confidence highlight strong, meaningful associations.  
3. **Algorithm Efficiency:** FP-Growth is generally faster than Apriori and scales better for large datasets.  
4. **Visualizations:** Plots help quickly identify important items, frequent itemsets, and the quality of rules.

---

## Conclusion

This lab successfully demonstrates **association rule mining** using Apriori and FP-Growth. It highlights:

- The importance of **data cleaning and preprocessing**.  
- How **algorithm choice affects runtime and efficiency**.  
- How **visualizations can aid interpretation** of mined patterns.  
- How mined rules can provide actionable business insights in retail.

---

## Repository Contents

- `Lab6_Association_Rules.ipynb` – Jupyter Notebook with all code, visualizations, and analysis.  
- `README.md` – Detailed explanation of methodology, insights, and comparative analysis.  
