# 📖 Data Catalog: Gold Layer (Analytical Reporting)

The **Gold Layer** represents the final, curated state of the data warehouse. It is architected using a **Star Schema** to ensure optimal performance for Business Intelligence (BI) tools and complex analytical queries.



---

## 👥 1. gold.dim_customers
**Purpose:** Provides a unified, 360-degree view of the customer by reconciling data from CRM and ERP systems. This dimension is the primary source for demographic and geographic analysis.

| Column Name | Data Type | Key Type | Description |
| :--- | :--- | :--- | :--- |
| **customer_key** | `INT` | **Surrogate Key** | Primary internal identifier for the dimension (auto-generated). |
| **customer_id** | `INT` | Natural Key | The unique numerical identifier from the source CRM system. |
| **customer_number** | `NVARCHAR(50)` | Business Key | Alphanumeric identifier (e.g., AW00011000) used for tracking. |
| **first_name** | `NVARCHAR(50)` | Attribute | The customer's first name, cleansed and formatted. |
| **last_name** | `NVARCHAR(50)` | Attribute | The customer's last name or family name. |
| **country** | `NVARCHAR(50)` | Geography | Country of residence, sourced from ERP location data. |
| **marital_status** | `NVARCHAR(50)` | Demographic | Marital status (e.g., 'Married', 'Single'). |
| **gender** | `NVARCHAR(50)` | Demographic | Gender (Enriched: CRM primary, ERP as fallback). |
| **birthdate** | `DATE` | Attribute | The customer's date of birth (YYYY-MM-DD). |
| **create_date** | `DATE` | Metadata | The date the customer record was first created in the system. |

---

## 📦 2. gold.dim_products
**Purpose:** Houses the company's product catalog. It flattens product hierarchies to allow for easy filtering by category and subcategory.

| Column Name | Data Type | Key Type | Description |
| :--- | :--- | :--- | :--- |
| **product_key** | `INT` | **Surrogate Key** | Primary internal identifier for the product dimension. |
| **product_id** | `INT` | Natural Key | Unique internal tracking ID from the CRM. |
| **product_number** | `NVARCHAR(50)` | Business Key | Structured alphanumeric code (SKU) for the product. |
| **product_name** | `NVARCHAR(50)` | Attribute | Full descriptive name of the product. |
| **category_id** | `NVARCHAR(50)` | Link | Identifier for the product's high-level classification. |
| **category** | `NVARCHAR(50)` | Hierarchy | Broad classification (e.g., Bikes, Accessories). |
| **subcategory** | `NVARCHAR(50)` | Hierarchy | Detailed classification (e.g., Road Bikes, Helmets). |
| **maintenance_required**| `NVARCHAR(50)` | Attribute | Flag indicating if the product requires recurring service. |
| **cost** | `INT` | Metric | The standard production or acquisition cost per unit. |
| **product_line** | `NVARCHAR(50)` | Attribute | The specific model line or series (e.g., Mountain, Touring). |
| **start_date** | `DATE` | Metadata | The date the product was officially introduced. |

---

## 💰 3. gold.fact_sales
**Purpose:** The central transaction table recording all sales activities. It connects to dimensions via foreign keys to enable multi-dimensional analysis of revenue and volume.

| Column Name | Data Type | Key Type | Description |
| :--- | :--- | :--- | :--- |
| **order_number** | `NVARCHAR(50)` | Business Key | Unique transaction identifier (e.g., 'SO54496'). |
| **product_key** | `INT` | **Foreign Key** | Reference link to `gold.dim_products`. |
| **customer_key** | `INT` | **Foreign Key** | Reference link to `gold.dim_customers`. |
| **order_date** | `DATE` | Time | The date the purchase was made. |
| **shipping_date** | `DATE` | Time | The date the product was dispatched. |
| **due_date** | `DATE` | Time | The scheduled deadline for payment. |
| **sales_amount** | `INT` | **Metric** | Total gross revenue generated from the transaction line. |
| **quantity** | `INT` | **Metric** | Number of units sold in the transaction. |
| **price** | `INT` | **Metric** | Unit selling price at the time of order. |

---
