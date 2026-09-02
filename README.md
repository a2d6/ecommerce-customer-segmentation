# E-Commerce Customer Segmentation & Retention Analysis

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat&logo=numpy&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-ffffff?style=flat&logo=matplotlib&logoColor=black)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=flat&logo=jupyter&logoColor=white)

An end-to-end customer analytics project utilizing **RFM (Recency, Frequency, Monetary) modeling** on transactional e-commerce data to identify high-value cohorts, combat churn, and optimize retention spend.

---

## Executive Summary

* **Revenue Concentration:** The top **22.2% of customers generated 65.2% of total revenue** (£5.81M of £8.91M), highlighting extreme Pareto distribution across the customer base.
* **Churn Vulnerability:** Identified **643 At-Risk customers** representing substantial past spend; within this cohort, the top **20% account for 56.2% of total At-Risk revenue**.
* **Cleaned Data Pipeline:** Filtered raw logs down to **397,884 verified positive transactions** across **4,338 unique customer accounts**.

---

## Business Objectives

* Quantify historical customer lifetime value and transaction patterns across accounts.
* Compute empirical **RFM scores (1–5 scale)** to segment customers by recency, purchase frequency, and monetary contribution.
* Isolate churn risks among previously high-spending accounts to prioritize re-engagement campaigns.
* Deliver targeted retention playbooks to maximize marketing return on investment (ROI).

---

## Dataset & Data Integrity

The analysis uses the **Online Retail** transaction dataset from a UK-based non-store retailer:

| Stage | Record Count | Description |
| :--- | :--- | :--- |
| **Raw Ingest** | 541,909 | Initial transactional log |
| **Cancelled Orders** | -9,288 | Removed invoices starting with `C` |
| **Missing IDs** | -135,080 | Excluded non-registered guest transactions lacking `CustomerID` |
| **Data Anomalies** | -457 | Filtered out non-positive entries (`Quantity <= 0` or `UnitPrice <= 0`) |
| **Cleaned Baseline** | **397,884** | **100% complete records across 8 core fields** |

**Engineered Metric:**  
$$\text{Revenue} = \text{Quantity} \times \text{UnitPrice}$$

---

## Methodology

### 1. RFM Metric Engineering

For each of the **4,338 distinct customers**, three core dimensions were calculated relative to the latest snapshot date:
* **Recency ($R$):** Days elapsed since the customer's most recent completed invoice.
* **Frequency ($F$):** Total count of unique order invoices.
* **Monetary ($M$):** Total aggregated gross revenue spend.

### 2. Scoring & Segmentation

Quantile binning was applied to assign scores from **1 (lowest)** to **5 (highest)** across each metric, categorizing the customer base into five strategic segments:

```text
  [ Champions ]          ──> High Recency, High Frequency, High Monetary
  [ Loyal Customers ]    ──> Steady cadence, above-average spend, consistent history
  [ Potential Customers] ──> High Recency, Low Frequency, moderate basket size
  [ At Risk ]            ──> High Monetary/Frequency, but high days since last purchase
  [ Others ]             ──> Low overall engagement or one-off legacy shoppers
