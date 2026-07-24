# Business Requirements Document (BRD) — E-Commerce Sales & Logistics Optimization

**Project Title:** Global E-Commerce Sales Analytics & Business Insights  
**Document Owner:** Business Analyst / Lead Analytics Consultant  
**Target Audience:** Executive Leadership, Logistics Operations, Marketplace Operations  

---

## 1. Business Problem
The e-commerce platform experiences operational bottlenecks across logistics, seller onboarding, and customer retention. Specifically:
* **Delivery Delays & Logistics Bottlenecks:** Unpredictable shipping lead times in high-volume regional hubs cause customer dissatisfaction and negative review scores.
* **Funnel Leakage in Seller Acquisition:** A significant percentage of marketing-qualified leads (MQLs) fail to convert into active marketplace sellers due to untracked drop-offs in the seller onboarding pipeline.
* **Revenue Fragmentation:** Management lacks visibility into how multi-seller orders, split payments (credit + voucher), and freight costs affect true net margins.

---

## 2. Business Objectives
* **Optimize Operational Delivery Performance:** Identify top regional shipping delays to improve on-time delivery rates across high-volume states.
* **Enhance Seller Onboarding Efficiency:** Track the seller conversion pipeline from MQL to closed deal to reduce acquisition lead times.
* **Drive Customer Retention & Satisfaction:** Quantify the exact correlation between delivery speed and customer review scores (1–5 scale) to mitigate churn risk.
* **Maximize Order Value:** Analyze cross-category performance and payment preferences to drive higher Average Order Value (AOV).

---

## 3. Key Performance Indicators (KPIs)
* **On-Time Delivery Rate (OTDR %):** Percentage of orders delivered on or before the estimated delivery date shown at checkout.
* **Average Delivery Lead Time (Days):** Time elapsed from order placement (`order_purchase_timestamp`) to customer receipt (`order_delivered_customer_date`).
* **Lead Conversion Rate (LCR %):** Percentage of Marketing Qualified Leads (MQLs) converted into won deals (`olist_closed_deals`).
* **Average Order Value (AOV):** Total order monetary value divided by total number of distinct orders.
* **Customer Review Score (CSAT / Review Score):** Average star rating (1–5) aggregated by product category, state, and shipping delay status.

---

## 4. Stakeholders
* **Chief Executive Officer (CEO) / C-Suite:** Primary consumer of high-level revenue, growth, and retention metrics.
* **Head of Logistics & Operations:** Requires granular visibility into transit delays, carrier bottlenecks, and estimated vs. actual delivery variance.
* **VP of Marketing & Seller Growth:** Uses lead conversion funnel and SDR/SR performance metrics to optimize acquisition spend.
* **Marketplace Operations Lead:** Tracks seller performance, catalog size, and cancellation rates across product categories.

---

## 5. Success Metrics
* Achieve 100% data audit coverage with zero unhandled orphaned foreign keys across transactional tables.
* Identify top 3 bottleneck geographical routes causing > 80% of delivery delays.
* Establish clear correlation threshold mapping delivery delay days to drop in review scores.
* Complete end-to-end data processing pipelines yielding automated reporting outputs for executive review.

---

## 6. Expected Deliverables
1. **Automated Audit & Cleaning Pipeline:** Executable Jupyter Notebooks (`01_data_audit.ipynb`, `02_data_cleaning.ipynb`) with modular python transformations.
2. **Processed Analytics Schema:** Standardized datasets exported to `data/processed/` with standardized timestamps, English category translations, and engineered features.
3. **Data Quality Documentation:** Comprehensive Data Dictionary (`docs/data_dictionary.md`) detailing primary/foreign keys and business logic assumptions.
4. **Executive Dashboard / Visual Analytics:** Persona-based interactive dashboard layouts (Overview, Logistics, Marketing/Sellers).

---

## 7. Acceptance Criteria
* **Data Integrity:** Primary keys (`order_id`, `customer_id`, `mql_id`, etc.) must be unique with zero unexplained duplicate rows.
* **Timestamp Validity:** All timestamp fields must be parsed into proper temporal datatypes (`datetime64`), and non-logical ordering (e.g., delivery before purchase) flagged or filtered.
* **Translation Coverage:** 100% of Portuguese product category names mapped to English equivalents (unmapped categories assigned `'unknown'`).
* **Code Quality:** All transformation logic documented with inline comments and clear Markdown sections.

---

## 8. Professional Assumptions
* **Currency Standard:** All monetary figures (`price`, `freight_value`, `payment_value`) are in Brazilian Real (BRL).
* **Scope Boundary:** Analysis focuses strictly on historic marketplace transactions captured within the dataset timeframe (2016–2018).
* **Missing Timestamp Logic:** Missing `order_delivered_customer_date` values represent undelivered/in-transit/canceled orders and are excluded from historical shipping delay calculation formulas without dropping the base order record.