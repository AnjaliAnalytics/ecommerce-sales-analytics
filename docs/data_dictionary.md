# Comprehensive Data Dictionary — Olist Brazilian E-Commerce Dataset

## 1. `olist_orders_dataset.csv`
* **Description:** Core operational entity tracking order states and timestamp milestones.
* **Primary Key:** `order_id`
* **Foreign Key:** `customer_id` -> `olist_customers_dataset.csv`

| Column | Data Type | Description |
| :--- | :--- | :--- |
| `order_id` | String | Unique identifier of the order |
| `customer_id` | String | Foreign key linking to customer per-order profile |
| `order_status` | String | Status (`delivered`, `shipped`, `canceled`, etc.) |
| `order_purchase_timestamp` | Timestamp | Order placement timestamp |
| `order_approved_at` | Timestamp | Payment approval timestamp |
| `order_delivered_carrier_date` | Timestamp | Carrier dispatch timestamp |
| `order_delivered_customer_date` | Timestamp | Actual delivery timestamp to customer |
| `order_estimated_delivery_date` | Timestamp | Promised delivery date shown at checkout |

---

## 2. `olist_customers_dataset.csv`
* **Description:** Customer geographic details and unique individual mapping.
* **Primary Key:** `customer_id`

| Column | Data Type | Description |
| :--- | :--- | :--- |
| `customer_id` | String | Key assigned per individual order |
| `customer_unique_id` | String | True unique identifier of the customer |
| `customer_zip_code_prefix` | String | First 5 digits of customer postal code |
| `customer_city` | String | Customer city |
| `customer_state` | String | Customer state code |

---

## 3. `olist_order_items_dataset.csv`
* **Description:** Line items associated with each order, linking items to sellers and products.
* **Primary Key:** Composite (`order_id`, `order_item_id`)
* **Foreign Keys:** `order_id` -> `olist_orders_dataset.csv`, `product_id` -> `olist_products_dataset.csv`, `seller_id` -> `olist_sellers_dataset.csv`

| Column | Data Type | Description |
| :--- | :--- | :--- |
| `order_id` | String | Unique order identifier |
| `order_item_id` | Integer | Sequential item number within the order |
| `product_id` | String | Product identifier |
| `seller_id` | String | Seller identifier |
| `shipping_limit_date` | Timestamp | Seller shipping deadline |
| `price` | Decimal | Item price (BRL) |
| `freight_value` | Decimal | Freight charges (BRL) |

---

## 4. `olist_products_dataset.csv`
* **Description:** Physical attributes and categorization of products.
* **Primary Key:** `product_id`

| Column | Data Type | Description |
| :--- | :--- | :--- |
| `product_id` | String | Product SKU ID |
| `product_category_name` | String | Category name in Portuguese |
| `product_name_lenght` | Integer | Character length of product title |
| `product_description_lenght` | Integer | Character length of product description |
| `product_photos_qty` | Integer | Number of published photos |
| `product_weight_g` | Integer | Weight in grams |
| `product_length_cm` | Integer | Length dimension in cm |
| `product_height_cm` | Integer | Height dimension in cm |
| `product_width_cm` | Integer | Width dimension in cm |

---

## 5. `olist_sellers_dataset.csv`
* **Description:** Merchant profiles fulfilling orders on the marketplace platform.
* **Primary Key:** `seller_id`

| Column | Data Type | Description |
| :--- | :--- | :--- |
| `seller_id` | String | Unique seller identifier |
| `seller_zip_code_prefix` | String | First 5 digits of seller postal code |
| `seller_city` | String | Seller city |
| `seller_state` | String | Seller state code |

---

## 6. `olist_order_payments_dataset.csv`
* **Description:** Payment breakdown per order, supporting split methods and installments.
* **Primary Key:** Composite (`order_id`, `payment_sequential`)
* **Foreign Key:** `order_id` -> `olist_orders_dataset.csv`

| Column | Data Type | Description |
| :--- | :--- | :--- |
| `order_id` | String | Unique order identifier |
| `payment_sequential` | Integer | Sequence index for split transactions |
| `payment_type` | String | Payment method (`credit_card`, `boleto`, `voucher`, `debit_card`) |
| `payment_installments` | Integer | Number of installments selected |
| `payment_value` | Decimal | Monetary value paid (BRL) |

---

## 7. `olist_order_reviews_dataset.csv`
* **Description:** Satisfaction survey scores and text feedback provided by buyers.
* **Primary Key:** `review_id`
* **Foreign Key:** `order_id` -> `olist_orders_dataset.csv`

| Column | Data Type | Description |
| :--- | :--- | :--- |
| `review_id` | String | Unique review identifier |
| `order_id` | String | Order identifier |
| `review_score` | Integer | Score rating scale from 1 to 5 |
| `review_comment_title` | String | Title header of feedback comment |
| `review_comment_message` | String | Detailed comment text body |
| `review_creation_date` | Timestamp | Survey dispatch date |
| `review_answer_timestamp` | Timestamp | Response submission timestamp |

---

## 8. `olist_geolocation_dataset.csv`
* **Description:** Spatial location mapping between Brazilian zip code prefixes and lat/long coordinates.

| Column | Data Type | Description |
| :--- | :--- | :--- |
| `geolocation_zip_code_prefix` | String | Zip code prefix (5 digits) |
| `geolocation_lat` | Decimal | Geographic latitude |
| `geolocation_lng` | Decimal | Geographic longitude |
| `geolocation_city` | String | City name |
| `geolocation_state` | String | State abbreviation |

---

## 9. `product_category_name_translation.csv`
* **Description:** English translation lookup table for Portuguese product category names.
* **Primary Key:** `product_category_name`

| Column | Data Type | Description |
| :--- | :--- | :--- |
| `product_category_name` | String | Portuguese category name |
| `product_category_name_english` | String | English translated category name |

---

## 10. `olist_marketing_qualified_leads_dataset.csv`
* **Description:** Optional Marketing Funnel Data — Tracks marketing-qualified leads (MQLs) originating from origin channels before seller sign-up.
* **Primary Key:** `mql_id`

| Column | Data Type | Description |
| :--- | :--- | :--- |
| `mql_id` | String | Unique lead identifier |
| `first_contact_date` | Timestamp | First contact date with Olist marketing |
| `landing_page_id` | String | Landing page form source |
| `origin` | String | Acquisition channel (e.g., search, organic, social) |

---

## 11. `olist_closed_deals_dataset.csv`
* **Description:** Optional Marketing Funnel Data — Tracks leads converted into signed marketplace sellers.
* **Primary Key:** `mql_id`
* **Foreign Keys:** `mql_id` -> `olist_marketing_qualified_leads_dataset.csv`, `seller_id` -> `olist_sellers_dataset.csv`

| Column | Data Type | Description |
| :--- | :--- | :--- |
| `mql_id` | String | Lead identifier |
| `seller_id` | String | Unique seller ID generated upon deal closing |
| `sdr_id` | String | Sales Development Representative ID |
| `sr_id` | String | Sales Representative ID |
| `won_date` | Timestamp | Date deal was closed |
| `business_segment` | String | Seller business industry |
| `lead_type` | String | Lead categorization |
| `declared_monthly_revenue` | Decimal | Self-reported monthly revenue at sign-up |