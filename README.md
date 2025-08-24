
SQL BASED - E-COMMERCE SALES ANALYTICS WAREHOUSE

📌 Project Overview
This project demonstrates a complete data pipeline and analytics workflow using PostgreSQL.
It follows the Medallion Architecture (Bronze → Silver → Gold) to transform raw data into clean, analytics-ready data, and finally into insights for dashboards.
The goal is to showcase data cleaning, modeling, and business intelligence skills with SQL.


⚡ Tech Stack
- Database: PostgreSQL (pgAdmin for queries)
- Architecture: Medallion (Bronze, Silver, Gold)
- Languages: SQL
- Visualization: Power BI

🏗️ Architecture

1. Bronze Layer (Raw Data)
   - Stores raw unprocessed data (customers, products, orders, order_items, shipments).
   - Minimal cleaning (loading as-is).

2. Silver Layer (Cleaned & Standardized Data)
   - Standardizes formats (dates, text casing, null handling).
   - Validates fields (e.g., numeric prices, positive quantities).
   - Removes invalid or missing records.

3. Gold Layer (Analytics & Facts/Dimensions)
   - Star schema with fact tables (orders, order_items, shipments) and dimension tables (customers, products, date).
   - Designed for business intelligence and reporting.

📂 Data Model

🟦 Dimension Tables
- dim_customers → Customer info (name, country, signup date)
- dim_products → Product catalog (product, category, price)
- dim_date → Calendar table (date, year, month, day)

🟨 Fact Tables
- fact_orders → Orders with total amounts
- fact_order_items → Order details with product quantities & revenue
- fact_shipments → Delivery times & shipment status

📊 Analytics Queries

1. Sales Trends
SELECT DATE_TRUNC('month', order_date)::DATE AS month,
       SUM(total_amount) AS monthly_sales
FROM gold.fact_orders
GROUP BY month
ORDER BY month;
Insight: Monthly sales performance over time

2. Top Products by Revenue
SELECT p.product_name, 
       SUM(oi.total_price) AS revenue
FROM gold.fact_order_items oi
JOIN gold.dim_products p ON oi.product_id = p.product_id
GROUP BY p.product_name
ORDER BY revenue DESC
LIMIT 10;
Insight: Best-performing products

3. Customer Lifetime Value (CLV)
SELECT c.name, 
       SUM(o.total_amount) AS lifetime_value
FROM gold.fact_orders o
JOIN gold.dim_customers c ON o.customer_id = c.customer_id
GROUP BY c.name
ORDER BY lifetime_value DESC;
Insight: Top customers by total spend

4. Delivery Performance
SELECT AVG(delivery_days) AS avg_delivery_time,
       SUM(CASE WHEN delivery_days > 7 THEN 1 ELSE 0 END)::FLOAT / COUNT(*) * 100 AS pct_delayed
FROM gold.fact_shipments;
Insight: Average delivery speed & % delayed shipments

5. Repeat Customers %
SELECT COUNT(DISTINCT customer_id) FILTER (WHERE total_orders > 1)::FLOAT / 
       COUNT(DISTINCT customer_id) * 100 AS repeat_customer_pct
FROM (
    SELECT customer_id, COUNT(order_id) AS total_orders
    FROM gold.fact_orders
    GROUP BY customer_id
) AS customer_orders;
Insight: Share of repeat customers

📈 Dashboard Ideas
- Sales Overview → Monthly revenue trend
- Top Customers → CLV leaderboard
- Top Products → Revenue by product
- Client Retention → Avg % of clients with repeat purchases
- Delivery Insights → Avg delivery time, % delayed shipments

🚀 How to Run
1. Clone this repo
2. Load raw data into Bronze schema
3. Run transformation SQL scripts for Silver & Gold layers
4. Run analytics queries
5. Build dashboard in Power BI

🎯 Key Learnings
- Hands-on practice with Medallion architecture
- Data cleaning & transformation using SQL
- Data modeling with facts & dimensions
- Running business analytics queries
- Building a portfolio-ready dashboard

👤 Author
Neelum Rana – Aspiring Data Engineer
📧 Contact: neelumsami@yahoo.com
🔗 LinkedIn: www.linkedin.com/in/neelum-rana-413318374


END $$;
