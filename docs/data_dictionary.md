# Enterprise Data Dictionary — E-Commerce Sales & Logistics Platform

**Project:** Global E-Commerce Sales Analytics & Business Insights  
**Maintainer:** Lead Data Analyst / Analytics Engineering  
**Data Source:** Olist E-Commerce Ecosystem  

---

## 1. `clean_orders.csv` (`olist_orders_dataset.csv`)
* **Description:** Core transactional dataset representing customer order headers and fulfillment lifecycle timestamps.
* **Primary Key:** `order_id`
* **Foreign Keys:** `customer_id` -> `clean_customers.csv`

| Column Name | Description | Datatype | Example | Business Meaning | Possible Issues | Null Values |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `order_id` | Unique identifier of the transaction | String (UUID) | `e481f51cbdc54678b7cc49136f2d6af7` | Single checkout transaction basket | None | 0 (0.00%) |
| `customer_id` | Foreign key to customer entity | String (UUID) | `9ef4001407e4ba1a226302e1b15a133b` | Order-specific customer token | One-time token per order (not unique customer ID) | 0 (0.00%) |
| `order_status` | Current lifecycle state of order | String | `delivered` | Operational state (`delivered`, `canceled`, `invoiced`, `shipped`) | Statuses like `canceled` lack delivery dates | 0 (0.00%) |
| `order_purchase_timestamp` | Order placement timestamp | Datetime | `2017-10-02 10:56:33` | Exact time purchase was authorized | Standard datetime formatting required | 0 (0.00%) |
| `order_approved_at` | Payment approval timestamp | Datetime | `2017-10-02 11:07:15` | Time payment cleared gateway | Latency for offline payment methods (e.g., Boleto) | 160 (0.16%) |
| `order_delivered_carrier_date` | Logistics handoff timestamp | Datetime | `2017-10-04 19:55:00` | Package handed to fulfillment carrier | Null if canceled or pre-carrier transit | 1,783 (1.79%) |
| `order_delivered_customer_date` | Final delivery timestamp | Datetime | `2017-10-10 21:25:13` | Customer receipt timestamp | Primary metric for delivery SLA tracking | 2,965 (2.98%) |
| `order_estimated_delivery_date` | Promised delivery deadline | Datetime | `2017-10-18 00:00:00` | SLA promised at checkout | Overly conservative estimates skew early metrics | 0 (0.00%) |
| `delivery_delay_days` | Feature: Days delayed beyond SLA | Float | ` -7.10` | Variance between actual and promised delivery date | Negative values indicate early delivery | 2,965 (2.98%) |
| `is_delayed` | Feature: Boolean SLA breach indicator | Boolean | `False` | SLA status (`True` if delivered after estimate) | Excludes undelivered/in-transit orders | 2,965 (2.98%) |
| `delivery_time_days` | Feature: Total fulfillment lead time | Float | `8.43` | End-to-end days from order placement to delivery | High variance across remote states | 2,965 (2.98%) |
| `purchase_year` | Feature: Year of purchase | Integer | `2017` | Temporal grouping variable | Partial coverage in 2016 and 2018 | 0 (0.00%) |
| `purchase_month` | Feature: Year-Month string | String | `2017-10` | Monthly revenue and order volume grouping | String sorting requires explicit formatting | 0 (0.00%) |
| `purchase_dayofweek` | Feature: Day name of purchase | String | `Monday` | Day-of-week purchasing behavior variable | Categorical ordering required for charts | 0 (0.00%) |

---

## 2. `clean_order_items.csv` (`olist_order_items_dataset.csv`)
* **Description:** Line-item detail dataset mapping individual products, sellers, and prices within each order.
* **Primary Key:** Compound (`order_id`, `order_item_id`)
* **Foreign Keys:** `order_id` -> `clean_orders.csv`, `product_id` -> `clean_products.csv`, `seller_id` -> `clean_sellers.csv`

| Column Name | Description | Datatype | Example | Business Meaning | Possible Issues | Null Values |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `order_id` | Associated order identifier | String (UUID) | `00010242fe8c5a6d1ba2dd792cb16214` | Link to order header | One order can contain multiple item rows | 0 (0.00%) |
| `order_item_id` | Line item sequential number | Integer | `1` | Sequential index of product within the order | Increments per item in multi-item orders | 0 (0.00%) |
| `product_id` | Unique product identifier | String (UUID) | `4244733e06e7ecb4970a6e2683c13e61` | Specific catalog item sold | Products can belong to multiple categories over time | 0 (0.00%) |
| `seller_id` | Unique seller identifier | String (UUID) | `484364060f20df7121212a8483113be8` | Marketplace seller fulfilling item | Multi-seller orders split fulfillment logistics | 0 (0.00%) |
| `shipping_limit_date` | Seller shipping deadline | Datetime | `2017-09-19 09:45:35` | SLA given to seller to hand off package | Missed seller deadlines cause downstream delays | 0 (0.00%) |
| `price` | Unit price of item (BRL) | Decimal | `58.70` | Gross item revenue before shipping | Excludes discounts and promotional vouchers | 0 (0.00%) |
| `freight_value` | Item shipping fee (BRL) | Decimal | `13.29` | Shipping charge allocated to this line item | Freight cost can exceed product price on cheap items | 0 (0.00%) |

---

## 3. `clean_products.csv` (`olist_products_dataset.csv`)
* **Description:** Physical product attribute dimension dataset including English category translations.
* **Primary Key:** `product_id`

| Column Name | Description | Datatype | Example | Business Meaning | Possible Issues | Null Values |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `product_id` | Unique product identifier | String (UUID) | `1e9ec64a143d45e780a2e2b0717e4d1b` | Catalog item SKU token | None | 0 (0.00%) |
| `product_name_lenght` | Character count of product title | Integer | `40` | Listing title depth | Missing metadata for uncategorized items | 610 (1.85%) |
| `product_description_lenght` | Character count of description | Integer | `287` | Listing detail quality proxy | Missing metadata for uncategorized items | 610 (1.85%) |
| `product_photos_qty` | Gallery photo count | Integer | `1` | Visual listing quality attribute | Zero or null photos impact conversion | 610 (1.85%) |
| `product_weight_g` | Product weight in grams | Integer | `225` | Logistics weight calculation factor | Extreme outliers skew shipping cost models | 2 (0.01%) |
| `product_length_cm` | Product packaging length (cm) | Integer | `16` | Package dimension factor | Outliers in physical package dimensions | 2 (0.01%) |
| `product_height_cm` | Product packaging height (cm) | Integer | `10` | Package dimension factor | Outliers in physical package dimensions | 2 (0.01%) |
| `product_width_cm` | Product packaging width (cm) | Integer | `14` | Package dimension factor | Outliers in physical package dimensions | 2 (0.01%) |
| `product_category_name_english` | English category name translation | String | `health_beauty` | Standardized merchandise category | Imputed with `'unknown'` when translation missing | 0 (0.00%) |

---

## 4. `clean_payments.csv` (`olist_order_payments_dataset.csv`)
* **Description:** Transaction payment split dataset recording payment methods and installment breakdowns per order.
* **Primary Key:** Compound (`order_id`, `payment_sequential`)
* **Foreign Keys:** `order_id` -> `clean_orders.csv`

| Column Name | Description | Datatype | Example | Business Meaning | Possible Issues | Null Values |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `order_id` | Unique order identifier | String (UUID) | `b8102fe3145d4b42389a047323d10b3f` | Link to order header | Single orders can have multiple payment rows | 0 (0.00%) |
| `payment_sequential` | Sequential payment index | Integer | `1` | Index for split-payment orders | Multiple rows when customer uses credit + voucher | 0 (0.00%) |
| `payment_type` | Method of payment | String | `credit_card` | Gateway instrument (`credit_card`, `boleto`, `voucher`, `debit_card`) | Vouchers produce $0 net cash flow items | 0 (0.00%) |
| `payment_installments` | Number of financing installments | Integer | `8` | Deferred payment split count | Credit card orders only; non-credit defaults to 1 | 0 (0.00%) |
| `payment_value` | Amount paid in instrument (BRL) | Decimal | `99.33` | Monetary sum allocated to this method | Sum across sequentials equals order grand total | 0 (0.00%) |

---

## 5. `clean_reviews.csv` (`olist_order_reviews_dataset.csv`)
* **Description:** Customer feedback dataset containing numerical review ratings and textual commentary.
* **Primary Key:** `review_id`
* **Foreign Keys:** `order_id` -> `clean_orders.csv`

| Column Name | Description | Datatype | Example | Business Meaning | Possible Issues | Null Values |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `review_id` | Unique feedback identifier | String (UUID) | `7bc617d130570e30d81c0022938b31a0` | Feedback record token | A small subset of orders have duplicate review prompts | 0 (0.00%) |
| `order_id` | Unique order identifier | String (UUID) | `73fc7af87114b39712e6da79b0a377eb` | Associated customer transaction | Key to evaluate review score against shipping latency | 0 (0.00%) |
| `review_score` | Customer rating (1 to 5) | Integer | `5` | CSAT / Customer Satisfaction Rating | Primary target variable for customer sentiment | 0 (0.00%) |
| `review_comment_title` | Subject header of review | String | `Recomendo` | Headline comment summary | Imputed with `'No Title'` during cleaning | 0 (0.00%) |
| `review_comment_message` | Free text feedback commentary | String | `Produto excelente...` | Detailed feedback explanation | Imputed with `'No Comment Provided'` during cleaning | 0 (0.00%) |
| `review_creation_date` | Date review survey was sent | Datetime | `2018-01-18 00:00:00` | Survey trigger timestamp | Automated trigger sent upon estimated/actual delivery | 0 (0.00%) |
| `review_answer_timestamp` | Date customer filled review | Datetime | `2018-01-18 21:46:59` | Customer response submission time | Delay between creation and answer varies | 0 (0.00%) |

---

## 6. `clean_customers.csv` (`olist_customers_dataset.csv`)
* **Description:** Customer geographic dimension dataset mapping order tokens to physical locations.
* **Primary Key:** `customer_id`

| Column Name | Description | Datatype | Example | Business Meaning | Possible Issues | Null Values |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `customer_id` | Unique order-customer token | String (UUID) | `06bbf393e96e65febe52b6369723524d` | Key matching specific order in `clean_orders` | Generates new token per order (order-level proxy) | 0 (0.00%) |
| `customer_unique_id` | True unique customer entity ID | String (UUID) | `8c84742f49f6b43a98da1d73f3723d8d` | Persistent entity ID for repeat purchase analysis | Required to calculate true customer retention/LTV | 0 (0.00%) |
| `customer_zip_code_prefix` | First 5 digits of postal code | Integer | `14409` | Geographic location zone | Requires padding leading zeros in some regions | 0 (0.00%) |
| `customer_city` | Customer delivery city | String | `franca` | Geographic urban unit | Inconsistent accent marks/casing in raw text | 0 (0.00%) |
| `customer_state` | Customer state code (2-letter) | String | `SP` | Administrative state territory | Critical dimension for regional shipping delay analysis | 0 (0.00%) |

---

## 7. `clean_sellers.csv` (`olist_sellers_dataset.csv`)
* **Description:** Marketplace seller geographic and identification dimension dataset.
* **Primary Key:** `seller_id`

| Column Name | Description | Datatype | Example | Business Meaning | Possible Issues | Null Values |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `seller_id` | Unique seller identifier | String (UUID) | `3504c0cb71d7034d5b9e22a428ba074d` | Merchant marketplace partner token | Joins with `clean_order_items` | 0 (0.00%) |
| `seller_zip_code_prefix` | First 5 digits of postal code | Integer | `13070` | Origin warehouse location zone | Used to compute physical seller-to-customer distance | 0 (0.00%) |
| `seller_city` | Seller warehouse city | String | `campinas` | Origin municipality | Text standardization required | 0 (0.00%) |
| `seller_state` | Seller state code (2-letter) | String | `SP` | Origin state territory | Concentration in SP skews freight costs to remote states | 0 (0.00%) |

---

## 8. `product_category_name_translation.csv`
* **Description:** Static reference translation table mapping Portuguese product category strings to English equivalents.
* **Primary Key:** `product_category_name`

| Column Name | Description | Datatype | Example | Business Meaning | Possible Issues | Null Values |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `product_category_name` | Original Portuguese category | String | `beleza_saude` | Native product category taxonomy | Must match exactly with product table strings | 0 (0.00%) |
| `product_category_name_english` | Translated English category | String | `health_beauty` | Standardized reporting category name | Unmapped new categories require fallback handling | 0 (0.00%) |

---

## 9. `olist_geolocation_dataset.csv`
* **Description:** Spatial coordinate lookup table mapping Brazilian postal zip code prefixes to latitude/longitude.
* **Primary Key:** Non-unique (Multiple coordinates per prefix)

| Column Name | Description | Datatype | Example | Business Meaning | Possible Issues | Null Values |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `geolocation_zip_code_prefix` | Postal code prefix | Integer | `01037` | Geographical lookup key | Contains multiple coordinate entries per prefix | 0 (0.00%) |
| `geolocation_lat` | Latitude coordinate | Decimal | `-23.545621` | Spatial Y-coordinate | Requires spatial aggregation (centroid mean) per prefix | 0 (0.00%) |
| `geolocation_lng` | Longitude coordinate | Decimal | `-46.639292` | Spatial X-coordinate | Outlier coordinates fall outside Brazilian territory | 0 (0.00%) |
| `geolocation_city` | City name | String | `sao paulo` | Spatial municipality label | Spelling variations across duplicate prefixes | 0 (0.00%) |
| `geolocation_state` | State code | String | `SP` | Spatial state boundary | None | 0 (0.00%) |

---

## 10. `olist_marketing_qualified_leads_dataset.csv`
* **Description:** Marketing funnel dataset tracking inbound prospective seller leads before sales qualification.
* **Primary Key:** `mql_id`

| Column Name | Description | Datatype | Example | Business Meaning | Possible Issues | Null Values |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `mql_id` | Unique marketing lead identifier | String (UUID) | `dac32548eff7823d7039d2e973431684` | Top-of-funnel prospective seller token | Joins with closed deals table on conversion | 0 (0.00%) |
| `first_contact_date` | First contact timestamp | Datetime | `2018-02-01 00:00:00` | Inbound lead generation timestamp | Time range differs from primary transactional dataset | 0 (0.00%) |
| `landing_page_id` | Lead capture landing page ID | String (UUID) | `b484fe451c672f7b3e369c7647224d53` | Form/page origin token | High missingness in early campaigns | 0 (0.00%) |
| `origin` | Marketing acquisition channel | String | `organic_search` | Acquisition source (`paid_search`, `social`, `organic`) | Uncategorized traffic labeled as null | 60 (0.75%) |

---

## 11. `olist_closed_deals_dataset.csv`
* **Description:** Sales funnel dataset tracking qualified seller leads converted into active marketplace merchants.
* **Primary Key:** `mql_id`
* **Foreign Keys:** `mql_id` -> `olist_marketing_qualified_leads_dataset.csv`, `seller_id` -> `clean_sellers.csv`

| Column Name | Description | Datatype | Example | Business Meaning | Possible Issues | Null Values |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `mql_id` | Lead identifier | String (UUID) | `5d3781cd12204382c7b5d03b0ce1c934` | Converted lead link token | Only converted leads appear in this dataset | 0 (0.00%) |
| `seller_id` | Generated seller identifier | String (UUID) | `2c430ec47a1669f1049900b6ff0cd76d` | Marketplace merchant ID | Links sales funnel to transaction history | 0 (0.00%) |
| `sdr_id` | Sales Dev Representative ID | String (UUID) | `4b339f9567d05812735d207d9f805e80` | Sales agent qualifying lead | Used to evaluate SDR pipeline velocity | 0 (0.00%) |
| `sr_id` | Sales Representative ID | String (UUID) | `4e53a61c99d86d5d34f2d7a229a1d95d` | Closing sales agent | Used to evaluate representative conversion rate | 0 (0.00%) |
| `won_date` | Deal closing timestamp | Datetime | `2018-02-14 17:24:00` | Merchant onboarding authorization date | Lead-to-win duration = `won_date` minus `first_contact_date` | 0 (0.00%) |
| `business_segment` | Merchant product segment | String | `pet` | Industry sector | Minor nulls for unclassified businesses | 1 (0.12%) |
| `lead_type` | Lead classification tier | String | `online_store` | Business structure (`online_store`, `reseller`) | Minor missing values | 6 (0.71%) |
| `business_type` | Commercial business model | String | `reseller` | Operating model (`manufacturer`, `reseller`) | Minor missing values | 10 (1.19%) |
| `declared_monthly_revenue` | Self-reported monthly revenue | Decimal | `0.0` | Revenue scale declared at sign-up | High null rate due to optional form field | 773 (91.81%) |