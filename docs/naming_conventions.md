# **Naming Conventions**

This document outlines the naming conventions used for schemas, tables, views, columns, pipelines, and other objects in the Superstore Sales Lakehouse platform.

---

## **Table of Contents**

1. [General Principles](#general-principles)
2. [Schema Naming Conventions](#schema-naming-conventions)
3. [Table Naming Conventions](#table-naming-conventions)
   - [Bronze Rules](#bronze-rules)
   - [Silver Rules](#silver-rules)
   - [Gold Rules](#gold-rules)
4. [Column Naming Conventions](#column-naming-conventions)
   - [Business Keys](#business-keys)
   - [Surrogate Keys](#surrogate-keys)
   - [Technical Columns](#technical-columns)
5. [Pipeline & Job Naming Conventions](#pipeline--job-naming-conventions)

---

## **General Principles**

- **Naming Format**: Use snake_case with lowercase letters and underscores (`_`).
- **Language**: Use English for all object names.
- **Clarity Over Abbreviation**: Prefer descriptive names over short acronyms.
- **Avoid Reserved Words**: Do not use SQL reserved keywords as object names.
- **Environment Independence**: Do not include environment names (`dev`, `test`, `prod`) in table names.

---

## **Schema Naming Conventions**

Schemas represent processing layers within the Lakehouse.

Only the following schemas are permitted:

- `bronze`
- `silver`
- `gold`

Examples:
- `bronze.orders`
- `silver.orders_clean`
- `gold.fact_sales`

Schemas must not include business domains or source system names.

---

## **Table Naming Conventions**

### **Bronze Rules**

- Bronze tables must reflect the source system structure.
- Table names must match the original entity names without business renaming.
- Pattern: <entity_name\>



Example:
- `orders` → Raw orders table ingested from SQL Server.

Bronze tables represent raw, minimally transformed data.

---

### **Silver Rules**

- Silver tables contain standardized and cleansed data.
- Table names must clearly indicate transformation.
- Pattern: <entity_name\>_clean


Example:
- `orders_clean` → Cleansed and validated version of `orders`.

Silver tables apply data quality rules and standardization logic.

---

### **Gold Rules**

- Gold tables must use business-aligned naming.
- Tables must begin with a category prefix.

Pattern: <category\>_<entity\>


Examples:
- `dim_customer` → Customer dimension table.
- `fact_sales` → Sales transaction fact table.

---

#### **Glossary of Category Patterns**

| Pattern     | Meaning                      | Example            |
|-------------|-----------------------------|--------------------|
| `dim_`      | Dimension table             | `dim_customer`     |
| `fact_`     | Fact table                  | `fact_sales`       |
| `bridge_`   | Bridge table                | `bridge_order_product` |
| `report_`   | Aggregated reporting table  | `report_sales_monthly` |

---

## **Column Naming Conventions**

### **Business Keys**

- All natural or business identifiers must use the suffix `_id`.

Pattern: <entity\>_id


Example:
- `customer_id` → Business identifier from source system.

---

### **Surrogate Keys**

- All surrogate keys in dimension tables must use the suffix `_key`.

Pattern: <entity\>_key


Example:
- `customer_key` → Surrogate key in `dim_customer`.

Surrogate keys are used only in the Gold layer.

---

## **Pipeline & Job Naming Conventions**

### **ADF Pipelines**

- The main ingestion pipeline must follow the pattern: pl_<layer\>_orchestrator


Example:
- `pl_bronze_orchestrator`

If reusable child pipelines are used, they must follow the pattern: pl_<layer\>_<purpose\>


Example:
- `pl_bronze_entity_load`

---

### **Databricks Jobs**

- All transformation jobs must follow the pattern: job_<layer\>_<purpose\>


Examples:
- `job_silver_transform`
- `job_gold_publish`

---

## **Summary**

- Schemas represent data processing layers only.
- Bronze mirrors the source system.
- Silver standardizes and validates data.
- Gold models data for analytics.
- Business keys use `_id`.
- Surrogate keys use `_key`.
- All naming must remain consistent, predictable, and environment-independent.
