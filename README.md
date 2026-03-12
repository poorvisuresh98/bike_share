````markdown
# 🚲 Bike Share Data Analysis (SQL)

## 📌 Project Overview
This project analyzes bike-sharing usage data to understand rider patterns, revenue generation, and profitability. The analysis was implemented in **SQL using SQL Server Management Studio (SSMS)** by combining multi-year datasets and integrating pricing/cost information to compute key business metrics such as **revenue** and **profit**.  
Full project report is included as `Bike Data Analysis.pdf`. :contentReference[oaicite:0]{index=0}

## 🎯 Problem Statement
Bike-sharing services collect large volumes of rental data across seasons, rider types, days, and hours. Without systematic analysis, it is difficult to understand demand patterns, assess revenue streams, and measure profitability. This project merges multi-year rental data with pricing and cost information to calculate revenue and profit and to identify trends by season, weekday, hour, and rider type to support data-driven decisions.

## 📂 Dataset
The repository contains the following tables/files:

- `bike_share_yr_0.csv` — Year 0 rental records  
  Columns (example): `dteday`, `season`, `yr`, `weekday`, `hr`, `rider_type`, `riders`, ...
- `bike_share_yr_1.csv` — Year 1 rental records (same structure as year 0)
- `cost_table.csv` — Pricing and cost info  
  Columns: `yr`, `price`, `COGS` (cost of goods sold per ride)

*(A PDF report with visuals and detailed findings is included in the repo.)* :contentReference[oaicite:1]{index=1}

## 🛠 Tools
- SQL (T-SQL)
- SQL Server Management Studio (SSMS)
- CSV (for data import/export)

## ⚙️ Core Query (Data Preparation & Calculations)
A Common Table Expression (CTE) is used to combine the two yearly tables, and a LEFT JOIN attaches price and cost to compute revenue and profit per record.

```sql
WITH cte AS (
  SELECT * FROM bike_share_yr_0
  UNION ALL
  SELECT * FROM bike_share_yr_1
)

SELECT
  dteday,
  season,
  a.yr,
  weekday,
  rider_type,
  riders,
  price,
  COGS,
  hr,
  riders * price AS revenue,
  riders * price - COGS AS profit
FROM cte a
LEFT JOIN cost_table b
  ON a.yr = b.yr;
````

## 📊 Key Metrics Calculated

* **Revenue (per record)** = `riders * price`
* **Profit (per record)** = `riders * price - COGS`
* Aggregations derived from the above: total riders, total revenue, total profit by `season`, `weekday`, `hr`, and `rider_type`.

## 📈 Example Aggregation Queries

Total revenue and profit by season:

```sql
-- Use the core query as a derived table or temp table, then aggregate:
SELECT
  season,
  SUM(revenue) AS total_revenue,
  SUM(profit)  AS total_profit,
  SUM(riders)  AS total_riders
FROM (
  -- paste the core query here
) t
GROUP BY season
ORDER BY total_revenue DESC;
```

Hourly peak analysis (example):

```sql
SELECT hr, SUM(riders) AS total_riders, SUM(revenue) AS total_revenue
FROM (
  -- paste the core query here
) t
GROUP BY hr
ORDER BY total_riders DESC;
```

## 📈 Business Insights (high-level)

* Seasonal demand varies significantly; some seasons show much higher rentals.
* Peak hours reflect commute/leisure usage and indicate when fleet/resources are most needed.
* Rider-type analysis reveals which segments contribute most to usage and revenue.
* Combining rider counts with pricing/cost data allows evaluation of profitability by segment and time.

## 🗂 Repository Structure

```
Bike-Share-Analysis/
│
├── bike_share_yr_0.csv
├── bike_share_yr_1.csv
├── cost_table.csv
├── SQL_queries.sql
├── Bike Data Analysis.pdf
└── README.md
```

## ✅ How to Reproduce

1. Import the CSVs into your SQL Server database (create tables matching column names).
2. Open SSMS and run the `SQL_queries.sql` (or paste the CTE & SELECT shown above).
3. Use `GROUP BY` and aggregation (`SUM`, `COUNT`) on `revenue`, `profit`, and `riders` to create summary tables and charts.
4. Export aggregated results to CSV or connect to a BI tool (Power BI / Excel) for visualization.



```
```
