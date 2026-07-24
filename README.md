#  Global E-Commerce Sales Analytics & Business Insights

[![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=power-bi&logoColor=black)](https://powerbi.microsoft.com/)
[![SQL](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![GitHub Repo Status](https://img.shields.io/badge/Status-Complete-success?style=for-the-badge)]()

An end-to-end, enterprise-grade business intelligence and data analytics solution built to decode global e-commerce performance, optimize supply chain logistics, maximize customer lifetime value (LTV), and drive data-informed executive decision-making.

---

##  Table of Contents
1. [Project Overview](#-project-overview)
2. [Business Problem](#-business-problem)
3. [Objectives](#-objectives)
4. [Dataset Architecture](#-dataset-architecture)
5. [Tech Stack](#-tech-stack)
6. [Folder Structure](#-folder-structure)
7. [Methodology & Workflow](#-methodology--workflow)
   - [Data Cleaning & Transformation](#data-cleaning--transformation)
   - [SQL Processing](#sql-processing)
   - [Python Analytics](#python-analytics)
   - [Power BI Modeling & DAX](#power-bi-modeling--dax)
8. [Dashboard Architecture & Screenshots](#-dashboard-architecture--screenshots)
9. [Key Business Insights](#-key-business-insights)
10. [Strategic Recommendations](#-strategic-recommendations)
11. [Future Improvements](#-future-improvements)

---

##  1. Project Overview
This repository houses a comprehensive, multi-page Power BI dashboard suite backed by a robust star-schema relational model, automated ETL scripts, and advanced DAX measures. Designed from the perspective of a Senior Business Analyst, this project transforms siloed transactional, customer, and logistics logs into a unified executive cockpit.

---

##  2. Business Problem
How can an international e-commerce retailer optimize revenue, profit margins, and customer retention across global markets while addressing hidden operational friction points? Specifically, the enterprise faced:
* **The Revenue Illusion:** High gross revenue figures masking poor net profitability due to compounding freight costs and delivery delays in remote regions.
* **Customer Churn:** Treating the customer base as transactional rather than relational, resulting in low repeat-purchase rates.
* **Inventory Misalignment:** Working capital trapped in low-velocity product categories while high-demand items face stock pressure.
* **Operational Silos:** Lack of cross-functional visibility connecting marketing acquisition campaigns directly to fulfillment capacity.

---

## 3. Objectives
* Build a scalable, star-schema data model capable of handling complex cross-table calculations without performance degradation.
* Implement robust DAX measures to track profitability, on-time delivery rates (OTDR %), average order value (AOV), and customer retention.
* Design an executive-ready, interactive Power BI report suite with dynamic navigation, custom tooltips, and deep-dive drill-throughs.
* Translate raw operational metrics into concrete, actionable business recommendations for senior leadership.

---

##  4. Dataset
The project utilizes a comprehensive relational e-commerce dataset comprising:
* **`fact_orders` / `order_items`:** Transaction-level data including pricing, freight values, seller IDs, and timestamps.
* **`dim_customers`:** Geographic and demographic data mapping customer locations across states and cities.
* **`dim_products`:** Product catalog attributes, category name translations, and physical dimensions.
* **`dim_sellers`:** Logistics origin points and vendor profiles.
* **`dim_calendar`:** Time-intelligence backbone supporting month-over-month and year-over-year analytics.

---

##  5. Tech Stack
* **Database & Querying:** PostgreSQL / SQL (Data wrangling, aggregations, and CTEs).
* **Data Engineering & Analysis:** Python (Pandas, NumPy for data cleaning and exploratory data analysis).
* **Business Intelligence & Visualization:** Power BI Desktop (Star-schema modeling, DAX engine, interactive UI/UX design).
* **Version Control:** Git & GitHub.

---

##  6. Folder Structure
```text
 ecommerce-sales-analytics
├── 📂 data/                    # Raw and cleaned CSV datasets
├── 📂 docs/                    # Data dictionaries and BRD documentation
├── 📂 notebooks/               # Python Jupyter notebooks for EDA and cleaning
├── 📂 reports/                 # Executive summaries and exported reports
├── 📂 sql/                     # SQL extraction, transformation, and view scripts
├── 📂 powerbi/                 # .pbix enterprise dashboard file
├── 📂 screenshots/             # High-resolution dashboard page previews
└── README.md                   # Comprehensive project documentation


7. Methodology & Workflow
Data Cleaning
Handled missing values in customer geolocation and product weight dimensions using imputation and median filling.

Standardized date-time formats across disparate logs to ensure seamless calendar table integration.

Removed duplicate transaction records and filtered out anomalous zero-price test orders.

SQL
Engineered optimized SQL queries utilizing Common Table Expressions (CTEs) and window functions (ROW_NUMBER(), RANK()) to compute preliminary customer lifetime values and delivery lead times.

Created indexed database views to accelerate local query performance prior to Power BI ingestion.

Python
Leveraged pandas to perform exploratory data analysis (EDA), identifying seasonal purchasing trends and verifying distribution normality for delivery delay metrics.

Generated correlation matrices to examine the relationship between freight cost burdens and cart abandonment/profit erosion.

Power BI
Star Schema Implementation: Enforced a clean star-schema with one-to-many (1:*) relationships connecting dim_calendar, dim_customers, and dim_products to fact_orders.

Advanced DAX Measures: Developed sophisticated measures including:
Total Revenue = SUM('fact_orders'[price])
Total Freight Cost = SUM('fact_orders'[freight_value])
Total Profit = [Total Revenue] - [Total Freight Cost]
Profit Margin % = DIVIDE([Total Profit], [Total Revenue], 0)
OTDR % = DIVIDE(CALCULATE(COUNTROWS(fact_orders), fact_orders[delivery_status] = "On Time"), COUNTROWS(fact_orders), 0)

8. Dashboard Architecture & Screenshots
### Executive Overview
High-level macro KPIs, revenue trends, and regional performance summaries.
![Executive Overview](https://raw.githubusercontent.com/AnjaliAnalytics/ecommerce-sales-analytics/main/executive_overview.png)

### Customer Dashboard
Cohort analysis, customer acquisition trends, and repeat purchase tracking.
![Customer Dashboard](https://raw.githubusercontent.com/AnjaliAnalytics/ecommerce-sales-analytics/main/customer_dashboard.png)

### Sales & Products Dashboard
Category performance, Pareto distribution of top SKUs, and volume velocity.
![Sales & Products Dashboard](https://raw.githubusercontent.com/AnjaliAnalytics/ecommerce-sales-analytics/main/sales_products.png)

### Logistics & Delivery Dashboard
Geographic heatmaps of shipping delays, carrier performance, and freight cost variance.
![Logistics & Delivery Dashboard](https://raw.githubusercontent.com/AnjaliAnalytics/ecommerce-sales-analytics/main/logistics_delivery.png)

### Profit Dashboard
Net profit margins, cost-to-serve analysis, and SKU-level profitability breakdowns.
![Profit Dashboard](https://raw.githubusercontent.com/AnjaliAnalytics/ecommerce-sales-analytics/main/profit_dashboard.png)

### Recommendations Dashboard
Strategic executive summary combining data visuals with prioritized action items.
![Recommendations Dashboard](https://raw.githubusercontent.com/AnjaliAnalytics/ecommerce-sales-analytics/main/recommendations.png)


9. Key Business Insights
The Revenue Illusion: High gross revenue figures heavily mask regional profitability leaks caused by disproportionate freight costs in remote states.

Transactional Lifecycle: The business operates as a transactional engine with low repeat customer rates, failing to capture full Customer Lifetime Value (LTV).

Inventory Skew: A small fraction of product categories drive the majority of net profit, while long-tail inventory ties up working capital.

Logistics Bottlenecks: Delivery delays cluster heavily in specific geographic pockets due to carrier reliance, directly impacting customer satisfaction scores.


10. Strategic Recommendations
Implement Minimum Order Values (MOV): Institute free shipping thresholds to protect profit margins from being eroded by lightweight, high-freight orders.

Automate CRM Lifecycle Campaigns: Deploy trigger-based post-purchase email workflows at 30, 60, and 90-day intervals to boost repeat customer retention.

Rationalize Long-Tail Inventory: Reallocate digital merchandising budget and ad spend toward high-velocity, high-margin product categories.

Diversify 3PL Partnerships: Onboard secondary regional logistics providers in high-delay states to improve On-Time Delivery Rates (OTDR %) and negotiate competitive freight rates.

11. Future Improvements
Integrate machine learning-based customer churn prediction models directly into the data pipeline.

Build automated predictive forecasting models for seasonal inventory demand planning.

Set up real-time executive alerting within Power BI for threshold breaches in regional OTDR or profit margins.
