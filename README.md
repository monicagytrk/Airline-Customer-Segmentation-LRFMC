# Airline Customer Segmentation — LRFMC
 
Customer segmentation on airline loyalty data using LRFMC model and K-Means clustering to identify 5 distinct groups based on customer transaction behavior and provide data-driven marketing strategy recommendations.
 
---

## Skills & Tools

Python 3: pandas, numpy, matplotlib, seaborn, scikit-learn (KMeans, PCA, StandardScaler, silhouette_score) 

Concepts: Unsupervised Learning, Clustering, LRFMC Framework, EDA, Feature Engineering, Dimensionality Reduction, CLTV

---
## Methodology
 
This study presents a comprehensive customer segmentation analysis utilizing a 2014 airline loyalty dataset of nearly 63,000 records. The methodology begins with rigorous data preparation, encompassing the imputation of missing values, removal of implausible entries, IQR-based outlier winsorization, and the engineering of five core LRFMC features (Length, Recency, Frequency, Monetary, and Discount). Following exploratory data analysis to assess feature distributions and multicollinearity, the data was standardized and processed through a K-Means++ clustering algorithm. An optimal four-cluster solution (K=4) was established using the Elbow method and Silhouette scores (achieving an overall score of 0.270), while Principal Component Analysis (PCA) was applied to map 67% of the variance for 2D visualization. Finally, to evaluate the financial viability of these segments, a relative Proxy Customer Lifetime Value (CLTV) score was computed for each member by integrating revenue proxies, frequency rates, and recency weights.

**Key Finding:** Cluster 3 (Champion Traveler), while the smallest segment (19.7%), holds the highest Proxy CLTV per member at 12,877 — making it the highest-priority segment for retention investment.

## Results & Business Recommendations

<img src="00. Asset/05. Insight 1.jpg" width="1000" alt="Alt text">

<img src="00. Asset/06. Insight 2.jpg" width="1000" alt="Alt text">

A relative CLTV score was computed per member using three behavioral dimensions:
 
```
Revenue Proxy  = SEG_KM_SUM × (1 - avg_discount)
Frequency Rate = FLIGHT_COUNT / Length of membership
Recency Weight = 1 / (1 + log(LAST_TO_END))
Proxy CLTV     = Revenue Proxy × Frequency Rate × Recency Weight × 12
```
 
> Note: Proxy CLTV is a relative score for cluster comparison — not an absolute monetary value.
 
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
 
*For questions or feedback, feel free to open an issue or reach out via GitHub.*
