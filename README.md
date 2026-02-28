# ⚙ Supply Chain Intelligence Platform

<div align="center">

![Dashboard Preview](https://img.shields.io/badge/🔴_LIVE_DASHBOARD-Click_to_View-blue?style=for-the-badge)

**[▶ VIEW LIVE INTERACTIVE DASHBOARD](https://anke-jimrison.github.io/supply-chain-intelligence/dashboard.html)**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white)
![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![Excel](https://img.shields.io/badge/Excel-217346?style=flat&logo=microsoft-excel&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white)

</div>

---

## 🎯 Business Problem

**GlobalManufacture Pvt. Ltd.** — a ₹500Cr+ FMCG manufacturer — had zero visibility into:
- Which of their 85 suppliers were high-risk and why
- Whether procurement prices were being manipulated or anomalous
- Which warehouses were unhealthy with dead stock or stockouts
- Where the biggest cost savings opportunities were hidden

This platform answers all four questions with automated Python pipelines, statistical methods, and interactive dashboards.

---

## 📊 Key Business Insights Found

| Metric | Finding | Business Impact |
|--------|---------|----------------|
| On-Time Delivery | 77.5% vs 95% benchmark | **17.5pp gap causing stockouts** |
| Anomalous POs | 94 orders flagged (Z-score >2.5) | **₹43.3Cr at financial risk** |
| Critical Risk Suppliers | 24 suppliers (28% of base) | **Immediate action required** |
| Procurement Savings | Median price benchmarking | **₹27.75Cr savings opportunity** |
| Dead Stock | Slow-moving inventory identified | **₹138.9Cr working capital blocked** |
| Quality Rejection | 2.31% average | **0.81pp above 1.5% target** |

---

## 🏗 Project Architecture

```
supply-chain-intelligence/
│
├── 📊 dashboard.html          ← LIVE interactive dashboard (open in browser)
├── 🐍 pipeline.py             ← Full Python data pipeline
├── 📋 dashboard.xlsx          ← Excel workbook with 4 analysis sheets
├── 📄 mysql_queries.sql       ← Advanced SQL with CTEs & window functions
│
├── data/
│   ├── supplier_master.csv
│   ├── product_master.csv
│   ├── warehouse_master.csv
│   ├── purchase_orders.csv       ← 5,000 PO records
│   ├── supplier_scorecards.csv   ← 85 suppliers with composite risk scores
│   ├── demand_supply_gap.csv
│   └── warehouse_health.csv
│
└── README.md
```

---

## 🔬 Methodology

### Supplier Risk Scoring (Composite 0–100)
5-factor weighted model:
- **OTD Gap** (max 30 pts): How far below 95% benchmark
- **Quality Rejection** (max 25 pts): Rejection rate above 1.5% target
- **Price Volatility** (max 20 pts): Coefficient of variation of unit prices
- **Single-Source Risk** (10 pts): Binary flag for sole-source suppliers
- **Anomaly Frequency** (max 15 pts): Past anomalous order rate

### Statistical Anomaly Detection
- **Z-Score Method**: Flags orders where unit_price deviates >2.5 standard deviations from SKU mean
- **IQR Method**: Identifies quantity outliers (Q1 - 1.5×IQR or Q3 + 1.5×IQR)
- **Severity Tiers**: CRITICAL (Z>3.5), HIGH (Z 2.5–3.5), combined flags

### Demand-Supply Gap Analysis
- Monthly gap = Demand Units - Supply Units per SKU per Warehouse
- Dead stock = Inventory where gap is persistently negative >3 months
- Working capital impact = Dead stock units × unit cost

---

## 🛢 Advanced MySQL Queries

Highlights used in analysis:
```sql
-- Supplier Risk Scoring with CTEs and Window Functions
WITH otd_stats AS (
  SELECT supplier_id,
         AVG(CASE WHEN on_time_delivery='Y' THEN 1.0 ELSE 0.0 END)*100 AS otd_pct,
         STDDEV(unit_price) / AVG(unit_price) AS price_cv,
         RANK() OVER (ORDER BY SUM(total_po_value) DESC) AS spend_rank
  FROM purchase_orders
  GROUP BY supplier_id
)
SELECT s.supplier_name,
       CASE WHEN score >= 70 THEN 'CRITICAL RISK'
            WHEN score >= 50 THEN 'HIGH RISK'
            WHEN score >= 30 THEN 'MEDIUM RISK'
            ELSE 'LOW RISK' END AS risk_tier
FROM supplier_scorecard_cte s;
```

---

## 📈 Dashboard Pages (Power BI)

| Page | Key Visuals |
|------|-------------|
| **Command Center** | 6 KPI cards, Supplier spend bar (risk-colored), OTD trend line |
| **Supplier Risk Intel** | Scatter (OTD% vs Rejection%), Risk tier matrix, Critical list |
| **Anomaly & Savings** | Anomaly table with Z-scores, Savings waterfall by supplier |
| **Inventory Intelligence** | Warehouse health map, Demand vs Supply, Dead stock trend |

---

## 🛠 Tech Stack

- **Python**: Pandas, NumPy — data generation, cleaning, statistical analysis
- **MySQL**: CTEs, Window functions (RANK, LAG, NTILE, SUM OVER), STDDEV, PERCENTILE_CONT
- **Excel**: 4-sheet workbook — KPI cards, scorecard table, queries reference, Power BI guide
- **Power BI**: Star schema data model, DAX measures, cross-filtering slicers, dark professional theme

---

## 👤 Author

**Anke Jimrison** | Aspiring Data Analyst | Hyderabad, India

[![GitHub](https://img.shields.io/badge/GitHub-anke--jimrison-181717?style=flat&logo=github)](https://github.com/anke-jimrison)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0077B5?style=flat&logo=linkedin)](https://linkedin.com/in/anke-jimrison)

*Open to Data Analyst / Business Analyst roles — Hyderabad & Remote*
