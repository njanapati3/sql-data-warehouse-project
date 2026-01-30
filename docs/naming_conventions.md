# 📏 Data Warehouse Naming Conventions

This document defines the standards for naming schemas, tables, views, and columns within the Data Warehouse. Adhering to these rules ensures consistency, improves readability, and simplifies the automation of ETL pipelines.

---

## 🏗️ 1. General Principles

* **Case Style**: Use `snake_case` (all lowercase with underscores).
* **Language**: All object names must be in English.
* **Reserved Words**: Avoid SQL reserved keywords (e.g., `ORDER`, `GROUP`, `TABLE`).
* **Pluralization**: Dimension tables and Fact tables should use plural nouns (e.g., `dim_customers` instead of `dim_customer`).

---

## 📂 2. Schema Naming Conventions

The warehouse follows the **Medallion Architecture**, organized into three distinct schemas:

| Schema | Purpose | Data State |
| :--- | :--- | :--- |
| `bronze` | Raw Ingestion | Original format from source (CRM/ERP). |
| `silver` | Cleansing & Standardizing | Deduplicated, trimmed, and type-cast data. |
| `gold` | Analytical Reporting | Star Schema (Dimensions & Facts) for BI. |



---

## 📊 3. Table Naming Conventions

### **Bronze & Silver Layers**
Rules for these layers focus on **traceability**. Table names must mirror the source system to maintain clear lineage.
* **Pattern**: `<source_system>_<original_entity_name>`
* **Examples**:
    * `crm_cust_info` (CRM Customer data)
    * `erp_loc_a101` (ERP Location data)

### **Gold Layer**
Rules for this layer focus on **business usability**. Names should be descriptive and align with the business domain.
* **Pattern**: `<category>_<entity>`
* **Categories**:
    * `dim_` : Dimension table (Descriptive data).
    * `fact_` : Fact table (Quantitative/Transactional data).
    * `report_` : Specialized views for specific dashboards.
* **Examples**:
    * `dim_products`
    * `fact_sales`

---

## 🆔 4. Column Naming Conventions

### **Surrogate Keys**
Every dimension table in the Gold layer must have a surrogate key to decouple the warehouse from source system changes.
* **Pattern**: `<entity_singular>_key`
* **Example**: `customer_key`, `product_key`.

### **Technical & Metadata Columns**
System-generated columns used for auditing and data lineage must be easily distinguishable.
* **Pattern**: `dwh_<column_name>`
* **Examples**:
    * `dwh_load_date`: Timestamp of when the record entered the warehouse.
    * `dwh_process_id`: Identifier for the specific ETL batch run.
    * `dwh_active_flag`: Used for tracking current vs historical records.

---

## ⚙️ 5. Stored Procedure Naming

Stored procedures are the "engines" of our ETL process. Their names must clearly indicate which layer they are responsible for loading.
* **Pattern**: `load_<layer>`
* **Examples**:
    * `load_bronze`: Ingests raw files into the Bronze schema.
    * `load_silver`: Transforms and cleanses data into the Silver schema.
    * `load_gold`: Populates the final Star Schema.

---

## 💡 Why We Use These Standards
1.  **Scalability**: New developers can understand the structure of the warehouse in minutes.
2.  **Automation**: Consistent naming allows for dynamic SQL scripts and automated testing.
3.  **Governance**: Metadata columns (`dwh_`) allow us to track data quality and freshness across the entire platform.

---
*Created by Narendra Janapati | Data Engineering Portfolio*
