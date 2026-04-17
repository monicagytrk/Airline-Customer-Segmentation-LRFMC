# Airline Customer Segmentation — LRFMC
 
Customer segmentation on airline loyalty data using LRFMC model and K-Means clustering to identify 5 distinct groups based on customer transaction behavior and provide data-driven marketing strategy recommendations.
 
---

## Skills & Tools

Python 3: `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn` (KMeans, PCA, StandardScaler, silhouette_score) 

Concepts: Unsupervised Learning, Clustering, LRFMC Framework, EDA, Feature Engineering, Dimensionality Reduction, CLTV

---
## Executive Summary
 
This project performs customer segmentation on an airline loyalty program dataset using the **LRFMC model** (Length, Recency, Frequency, Monetary, Coefficient of Fare Discount) and **K-Means clustering**. By analyzing 62,727 customer records across 5 behavioral dimensions, the model identifies **4 distinct customer segments**, each with unique characteristics and actionable business strategies.
 
The analysis goes beyond simple clustering — it incorporates **Proxy CLTV (Customer Lifetime Value)** scoring to rank segments by estimated business value, enabling the marketing team to prioritize budget allocation with a data-driven foundation.
 
**Key Finding:** Cluster 3 (Champion Traveler), while the smallest segment (19.7%), holds the highest Proxy CLTV per member at 12,877 — making it the highest-priority segment for retention investment.
 
---
# Business Problem
 
Airline loyalty programs generate large volumes of customer behavioral data, but without proper segmentation, marketing efforts tend to be one-size-fits-all — inefficient and costly.
 
**Core questions this project addresses:**
- Which customers are most valuable and should be retained at all costs?
- Which customers are at risk of churning and need win-back campaigns?
- How should marketing budget be allocated across different customer segments?
- What does each segment need in terms of engagement strategy?
Standard RFM segmentation is insufficient for airlines because it does not account for **flight distance (mileage)** and **fare discount behavior** — two dimensions critical to understanding airline customer value. The LRFMC framework was chosen specifically to address this gap.
 
---
## Methodology
 
### 1. Data Understanding
- Dataset: Airline customer loyalty records from 2014
- Raw data: 62,988 instances × 23 features
- Features cover personal info, flight history, and loyalty program activity
### 2. Data Preparation
- **Missing value handling:** Features with <20% missing rate replaced with median/mode; no features exceeded the 20% drop threshold
- **Duplicate handling:** No duplicates found
- **Implausible value removal:** Removed customers with age <10 or >100, flight count = 0, mileage = 0, and zero ticket purchase history
- **Outlier handling:** IQR-based capping (Winsorization) applied to all numerical features
- **Feature engineering:** 23 raw features transformed into 5 LRFMC features:

| Feature | Column | Description |
|---|---|---|
| L — Length | `LOAD_TIME` - `FFP_DATE` | Membership duration in months |
| R — Recency | `LAST_TO_END` | Days since last flight |
| F — Frequency | `FLIGHT_COUNT` | Total flights during membership |
| M — Monetary | `SEG_KM_SUM` | Total km flown |
| C — Discount | `avg_discount` | Average fare discount coefficient |
 
### 3. Data Exploration (EDA)
- **Distribution analysis:** Histogram/KDE plots revealed right-skewed distributions for L, R, F, M — confirming the need for StandardScaler before clustering
- **Outlier confirmation:** Boxplots validated IQR-capped boundaries per feature
- **Correlation analysis:** Heatmap identified high multicollinearity between F and M (r = 0.89)
- **Scatter plots:** Key feature pair relationships confirmed meaningful behavioral patterns
### 4. Model Development
- **Preprocessing:** StandardScaler applied to all 5 LRFMC features before clustering
- **Optimal K selection:** Elbow Method (inertia) + Silhouette Score evaluated for K = 2–10
- **Final model:** K-Means++ with K=4, chosen because silhouette score at K=4 (0.270) is higher than K=5 (0.247) and K=4 marks the point where inertia reduction begins to plateau
- **Dimensionality reduction:** PCA (2 components, 67% variance retained) used for scatter plot visualization only — not applied before clustering
### 5. Model Validation
- Overall Silhouette Score: **0.270** (cluster structure present, some overlap expected for real-world customer data)
- Per-cluster silhouette scores evaluated with mean, std, and negative sample percentage
- PCA variance plot confirmed PC1 (46.7%) + PC2 (20.3%) = 67% of total variance captured in 2D visualization
### 6. Proxy CLTV Scoring
A relative CLTV score was computed per member using three behavioral dimensions:
 
```
Revenue Proxy  = SEG_KM_SUM × (1 - avg_discount)
Frequency Rate = FLIGHT_COUNT / Length of membership
Recency Weight = 1 / (1 + log(LAST_TO_END))
Proxy CLTV     = Revenue Proxy × Frequency Rate × Recency Weight × 12
```
 
> Note: Proxy CLTV is a relative score for cluster comparison — not an absolute monetary value.

## Results & Business Recommendations
 
### Cluster Overview

<img src="00. Asset/Fig 1. PCA Scater Plot.png" width="1100" alt="Alt text">
 
| Cluster | Label | Members | Share | Proxy CLTV/Member |
|---|---|---|---|---|
| C1 | At-Risk of Churn | 12,905 | 20.6% | 160 |
| C2 | Loyal Veteran | 14,593 | 23.3% | 428 |
| C3 | Champion Traveler | 12,328 | 19.7% | **12,878** |
| C4 | Price-Driven New Member | 22,901 | 36.5% | 1,458 |

### Priority Matrix

<img src="00. Asset/Fig 2. Proxy CLTV per Cluster.png" width="800" alt="Alt text">

```
Priority 1 (Retain):    C3 — Champion Traveler    → Highest CLTV, highest revenue impact
Priority 2 (Grow):      C4 — Price-Driven New     → Largest segment, strong growth potential
Priority 3 (Re-engage): C2 — Loyal Veteran        → Valuable history, declining engagement
Priority 4 (Win-back):  C1 — At-Risk of Churn     → Lowest CLTV, budget reallocation if unresponsive
```
 
---
## Next Steps
 
1. **Investigate overlap between C2 and C4** — both clusters show similar F and M values (~8 flights, ~10–11k km). Adding features like route type or cabin class could improve separation.
2. **Feature engineering** — Create derived features such as `flight_rate` (F/L = flights per month) and `recency_ratio` (R/L) to better differentiate active veterans from dormant members.

---
 
*For questions or feedback, feel free to open an issue or reach out via GitHub.*
