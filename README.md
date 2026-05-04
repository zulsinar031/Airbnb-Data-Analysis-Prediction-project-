# Airbnb Market Analysis & Prediction

> Large-scale analysis of 49,000+ Australian Airbnb listings using PySpark on a YARN cluster — covering classification, regression, clustering, and association rule mining.

![PySpark](https://img.shields.io/badge/PySpark-3.3.2-E25A1C)
![Python](https://img.shields.io/badge/Python-3.11-yellow)
![Docker](https://img.shields.io/badge/Docker-containerized-2496ED)
![GCS](https://img.shields.io/badge/GCS-Cloud%20Storage-4285F4)
![YARN](https://img.shields.io/badge/YARN-Cluster-orange)

---

## Overview

This project applies distributed machine learning to real-world Airbnb listing data across Australian cities. All processing runs on a YARN cluster with data stored in Google Cloud Storage — reflecting a production-style big data pipeline rather than a local notebook.

Four analytical tasks cover the core ML problem types: classification, regression, unsupervised learning, and pattern mining.

---

## Dataset

| Detail | Value |
|---|---|
| Source | Inside Airbnb — Australian listings |
| Raw records | 49,079 listings + 48,874 reviews |
| After cleaning | 48,597 records |
| Storage | Google Cloud Storage |
| Processing | PySpark 3.3.2 on YARN (2 executor cores, 2.8GB memory) |

**Features used:** price, room type, neighbourhood, minimum nights, reviews, availability, latitude/longitude

---

## Analytical Tasks & Results

### Task 1 — Classification: Room Type Prediction

Predicts room type (Entire home, Private room, Shared room) from listing features.

| Model | Accuracy |
|---|---|
| Logistic Regression | 0.8033 |
| **Random Forest** | **0.8465** ✅ |

**Key insight:** Price is the dominant feature (0.75 importance), followed by minimum nights and host listing count.

---

### Task 2 — Regression: Price Prediction

Predicts nightly price from listing attributes.

| Model | RMSE | R² |
|---|---|---|
| Linear Regression | $101.63 | 0.0994 |
| **Gradient Boosting** | **$88.91** | **0.3108** ✅ |

**Key insight:** GBT outperforms linear regression significantly. Low R² reflects the inherent noise in price data — location and availability are the strongest signals.

---

### Task 3 — Clustering: Market Segmentation

K-Means (k=5) segments listings into distinct market tiers.

| Cluster | Avg Price | Avg Reviews | Size | Common Type |
|---|---|---|---|---|
| 0 | $107 | 51 | 3,568 | Private room |
| 1 | $551 | 14 | 845 | Entire home |
| 2 | $135 | 30 | 5,050 | Entire home |
| 3 | $241 | 14 | 3,710 | Entire home |
| 4 | $89 | 16 | 11,147 | Private room |

**Key insight:** Cluster 1 is a clear luxury segment (~$550 avg, low reviews). Cluster 4 is the dominant budget segment with the most listings.

---

### Task 4 — Association Rule Mining: Pattern Discovery

FP-Growth identifies relationships between room type, neighbourhood, price tier, and popularity.

| Rule | Confidence | Lift |
|---|---|---|
| Brooklyn + Private room → Budget | 0.86 | 1.91 |
| Brooklyn + Private room + New → Budget | 0.86 | 1.90 |
| Manhattan + Budget → Private room | 0.79 | 1.73 |
| Private room → Budget | 0.77 | 1.71 |

**Key insight:** Private rooms strongly associate with budget pricing regardless of neighbourhood. Manhattan listings skew toward private rooms even in the budget tier.

---

## Tech Stack

| Tool | Purpose |
|---|---|
| PySpark 3.3.2 | Distributed data processing & ML |
| YARN | Cluster resource management |
| Google Cloud Storage | Data lake for raw & processed data |
| Spark MLlib | ML pipelines (classification, regression, clustering, FP-Growth) |
| Matplotlib / Seaborn | Visualisation |
| Docker | Containerised environment |



## Key Findings

- **Random Forest** is the best classifier for room type prediction (84.65% accuracy)
- **Gradient Boosting** outperforms linear regression for price prediction (RMSE $88.91)
- **5 distinct market segments** exist — from budget private rooms (~$89) to luxury entire homes (~$551)
- **Private rooms strongly correlate with budget pricing** across all neighbourhoods (lift: 1.91)
- Price is the single most important feature for room type classification (75% importance)
