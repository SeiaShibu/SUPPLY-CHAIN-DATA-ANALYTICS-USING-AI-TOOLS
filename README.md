# SUPPLY-CHAIN-DATA-ANALYTICS-USING-AI-TOOLS
# 📦 Supply Chain Data Analytics Project

This project leverages **n8n**, **Supabase**, and **Quadratic** to build a low-code data analytics pipeline focused on **Supply Chain KPIs** like OTIF%, order delays, and revenue loss.

---

## 🚀 Overview

- **Objective**: Automate the extraction, processing, and visualization of supply chain data.
- **Tech Stack**:
  - `n8n`: For workflow automation (data ingestion, processing).
  - `Supabase`: PostgreSQL backend to store transformed data.
  - `Quadratic`: No-code BI tool used to generate interactive reports and answer business queries.

---

## 🔄 Workflow Summary

1. **Email Extraction** (`n8n`)
   - Automatically fetches Excel reports from India and USA factories via email.
   - Parses and cleans the raw data.

2. **Data Aggregation & Transformation** (`n8n`)
   - Merges data into 4 tables:
     - `fact_order_line`
     - `fact_aggregate`
     - `dim_customer`
     - `dim_product`

3. **Database Integration** (`Supabase`)
   - Pushes transformed data into a PostgreSQL DB on Supabase.
   - Tables reflect normalized schema design.

4. **Visualization & Insights** (`Quadratic`)
   - KPIs like OTIF%, IF%, OT%, order delays, revenue loss visualized.
   - Business Questions Answered:
     - Revenue loss from undelivered orders.
     - Top customers based on OTIF discrepancy.
     - Product categories with low ‘In Full’ delivery rates.
     - Average delay time for late deliveries.

---

## 📊 Sample KPI Queries

```sql
-- Top 5 customers by order value with OTIF %, IF %, OT %
SELECT
  customer_name,
  customer_id,
  city,
  SUM(order_value) AS total_order_value,
  ROUND(SUM(otif_count)*100.0 / COUNT(*), 2) AS otif_percent,
  ROUND(SUM(if_count)*100.0 / COUNT(*), 2) AS if_percent,
  ROUND(SUM(ot_count)*100.0 / COUNT(*), 2) AS ot_percent
FROM fact_aggregate
JOIN dim_customer USING(customer_id)
GROUP BY customer_id
ORDER BY total_order_value DESC
LIMIT 5;
