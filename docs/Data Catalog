# Data Catalog

This catalog describes the tables in the data warehouse: what each table is for, its columns and data types, and where the data comes from. Keep it up to date whenever a table or column changes.

## Table of Contents

1. [Overview](#overview)
2. [Gold Layer](#gold-layer)
   - [gold.dim_customers](#golddim_customers)
   - [gold.dim_products](#golddim_products)
   - [gold.fact_sales](#goldfact_sales)
3. [Silver Layer](#silver-layer)
4. [Bronze Layer](#bronze-layer)

---

## Overview

| Layer | Purpose | Documented here |
|-------|---------|-----------------|
| **Gold** | Business-level data for analytics and reporting: dimension tables and a fact table in a star schema. | Yes |
| **Silver** | Cleaned and standardised data. | Not yet (see [Silver Layer](#silver-layer)) |
| **Bronze** | Raw data, loaded as-is from the source CSV files. | Yes |

Naming rules for all of these objects are in [naming_conventions.md](naming_conventions.md).

---

## Gold Layer

The Gold layer is the business-level representation of the data, structured for analytical and reporting use cases. It consists of **dimension tables** and a **fact table** for specific business metrics.

### Relationships

`gold.fact_sales` is the center of the star schema:

| Fact column | Links to |
|-------------|----------|
| `fact_sales.customer_key` | `dim_customers.customer_key` |
| `fact_sales.product_key` | `dim_products.product_key` |

### Where the data comes from

| Gold table | Built from (Bronze/Silver tables) |
|------------|-----------------------------------|
| `dim_customers` | `crm_cust_info`, `erp_cust_az12`, `erp_loc_a101` |
| `dim_products` | `crm_prd_info`, `erp_px_cat_g1v2` |
| `fact_sales` | `crm_sales_details` (joined to the two dimensions) |

> Confirm this table against the Gold scripts once they are in the repository.

---

### gold.dim_customers

- **Purpose:** stores customer details enriched with demographic and geographic data.
- **Columns:**

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `customer_key` | INT | Surrogate key uniquely identifying each customer record in the dimension table. |
| `customer_id` | INT | Unique numerical identifier assigned to each customer. |
| `customer_number` | NVARCHAR(50) | Alphanumeric identifier representing the customer, used for tracking and referencing. |
| `first_name` | NVARCHAR(50) | The customer's first name, as recorded in the system. |
| `last_name` | NVARCHAR(50) | The customer's last name or family name. |
| `country` | NVARCHAR(50) | The country of residence for the customer (e.g., 'Australia'). |
| `marital_status` | NVARCHAR(50) | The marital status of the customer (e.g., 'Married', 'Single'). |
| `gender` | NVARCHAR(50) | The gender of the customer (e.g., 'Male', 'Female', 'n/a'). |
| `birthdate` | DATE | The date of birth of the customer, formatted as YYYY-MM-DD (e.g., 1971-10-06). |
| `create_date` | DATE | The date when the customer record was created in the system. |

---

### gold.dim_products

- **Purpose:** provides information about the products and their attributes.
- **Columns:**

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `product_key` | INT | Surrogate key uniquely identifying each product record in the product dimension table. |
| `product_id` | INT | A unique identifier assigned to the product for internal tracking and referencing. |
| `product_number` | NVARCHAR(50) | A structured alphanumeric code representing the product, often used for categorization or inventory. |
| `product_name` | NVARCHAR(50) | Descriptive name of the product, including key details such as type, color, and size. |
| `category_id` | NVARCHAR(50) | A unique identifier for the product's category, linking to its high-level classification. |
| `category` | NVARCHAR(50) | The broader classification of the product (e.g., Bikes, Components) to group related items. |
| `subcategory` | NVARCHAR(50) | A more detailed classification of the product within the category, such as product type. |
| `maintenance_required` | NVARCHAR(50) | Indicates whether the product requires maintenance (e.g., 'Yes', 'No'). |
| `cost` | INT | The cost or base price of the product, measured in monetary units. |
| `product_line` | NVARCHAR(50) | The specific product line or series to which the product belongs (e.g., Road, Mountain). |
| `start_date` | DATE | The date when the product became available for sale or use. |

---

### gold.fact_sales

- **Purpose:** stores transactional sales data for analytical purposes.
- **Columns:**

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `order_number` | NVARCHAR(50) | A unique alphanumeric identifier for each sales order (e.g., 'SO54496'). |
| `product_key` | INT | Surrogate key linking the order to the product dimension table. |
| `customer_key` | INT | Surrogate key linking the order to the customer dimension table. |
| `order_date` | DATE | The date when the order was placed. |
| `shipping_date` | DATE | The date when the order was shipped to the customer. |
| `due_date` | DATE | The date when the order payment was due. |
| `sales_amount` | INT | The total monetary value of the sale for the line item, in whole currency units (e.g., 25). |
| `quantity` | INT | The number of units of the product ordered for the line item (e.g., 1). |
| `price` | INT | The price per unit of the product for the line item, in whole currency units (e.g., 25). |

---

## Silver Layer

Not documented yet. Add one section per Silver table here when the layer is built, using the same format as the other layers (purpose, columns, data types, descriptions).

Silver tables follow the same naming rule as Bronze (`<sourcesystem>_<entity>`), so they will mirror the Bronze tables below with cleaned and standardised data.

---

## Bronze Layer

Raw data loaded as-is from the source CSV files. No cleaning or transformation is applied, so column names and values are exactly as they arrive from the source. Tables are emptied and reloaded each time `bronze.load_bronze` runs.

| Bronze table | Source system | Source file | Content |
|--------------|---------------|-------------|---------|
| `bronze.crm_cust_info` | CRM | `source_crm/cust_info.csv` | Customer master data |
| `bronze.crm_prd_info` | CRM | `source_crm/prd_info.csv` | Product information |
| `bronze.crm_sales_details` | CRM | `source_crm/sales_details.csv` | Sales transactions |
| `bronze.erp_loc_a101` | ERP | `source_erp/loc_a101.csv` | Customer country / location |
| `bronze.erp_cust_az12` | ERP | `source_erp/cust_az12.csv` | Additional customer data (birth date, gender) |
| `bronze.erp_px_cat_g1v2` | ERP | `source_erp/px_cat_g1v2.csv` | Product categories |

### bronze.crm_cust_info

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `cst_id` | INT | Numerical customer identifier. |
| `cst_key` | NVARCHAR(50) | Alphanumeric customer key. |
| `cst_firstname` | NVARCHAR(50) | Customer's first name. |
| `cst_lastname` | NVARCHAR(50) | Customer's last name. |
| `cst_marital_status` | NVARCHAR(50) | Marital status, as coded in the source. |
| `cst_gndr` | NVARCHAR(50) | Gender, as coded in the source. |
| `cst_create_date` | DATE | Date the customer record was created. |

### bronze.crm_prd_info

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `prd_id` | INT | Numerical product identifier. |
| `prd_key` | NVARCHAR(50) | Product key. |
| `prd_nm` | NVARCHAR(50) | Product name. |
| `prd_cost` | INT | Product cost. |
| `prd_line` | NVARCHAR(50) | Product line, as coded in the source. |
| `prd_start_dt` | DATETIME | Start date of the product record. |
| `prd_end_dt` | DATETIME | End date of the product record. |

### bronze.crm_sales_details

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `sls_ord_num` | NVARCHAR(50) | Sales order number. |
| `sls_prd_key` | NVARCHAR(50) | Product key of the item sold. |
| `sls_cust_id` | INT | Customer identifier. |
| `sls_order_dt` | INT | Order date. Stored as a number in the raw data and converted to DATE in later layers. |
| `sls_ship_dt` | INT | Shipping date. Stored as a number in the raw data and converted to DATE in later layers. |
| `sls_due_dt` | INT | Due date. Stored as a number in the raw data and converted to DATE in later layers. |
| `sls_sales` | INT | Total sales amount of the line item. |
| `sls_quantity` | INT | Quantity sold. |
| `sls_price` | INT | Price per unit. |

### bronze.erp_loc_a101

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `cid` | NVARCHAR(50) | Customer identifier. |
| `cntry` | NVARCHAR(50) | Country of the customer. |

### bronze.erp_cust_az12

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `cid` | NVARCHAR(50) | Customer identifier. |
| `bdate` | DATE | Customer's birth date. |
| `gen` | NVARCHAR(50) | Gender, as coded in the source. |

### bronze.erp_px_cat_g1v2

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `id` | NVARCHAR(50) | Category identifier. |
| `cat` | NVARCHAR(50) | Product category. |
| `subcat` | NVARCHAR(50) | Product subcategory. |
| `maintenance` | NVARCHAR(50) | Whether the product requires maintenance. |
