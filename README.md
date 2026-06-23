# Retail Data Platform

An end to end data engineering and analytics project that integrates multisource retail data  **Stores**, **Products**, and **Transactions** (relational/SQL) alongside **Customers** (semi-structured/JSON) into a unified pipeline on **Azure**, processed with **Databricks**, and visualized in **Power BI**.



##  Overview

Retail businesses rarely have all their data living in one place. This project simulates a realistic scenario where:

- **Stores, Products, and Transactions** data live in a **SQL database** (structured, relational).
- **Customer** data is delivered as **JSON** files (semi-structured, from a CRM and web service export.

The goal is to build a pipeline that ingests both formats, harmonizes them into a single data model, and produces a clean dataset ready for business intelligence and reporting.



## Architecture


┌─────────────┐     ┌─────────────┐
│  SQL Source  │     │ JSON Source │
│ Stores       │     │ Customers   │
│ Products     │     │             │
│ Transactions │     │             │
└──────┬───────┘     └──────┬──────┘
       │                    │
       └─────────┬──────────┘
                  ▼
         ┌─────────────────┐
         │   Azure (ADLS /  │
         │   Data Factory)  │
         └────────┬─────────┘
                  ▼
         ┌─────────────────┐
         │    Databricks    │
         │  (Bronze → Silver│
         │   → Gold layers) │
         └────────┬─────────┘
                  ▼
         ┌─────────────────┐
         │     Power BI     │
         │   Dashboards &   │
         │     Reports      │
         └─────────────────┘


**Layers (Medallion Architecture):**

| Layer | Purpose |
|-------|---------|
| **Bronze** | Raw ingestion of SQL tables and JSON files, no transformation |
| **Silver** | Cleaned, validated, and standardized data (schema enforcement, deduplication, type casting) |
| **Gold** | Business-ready, aggregated tables (e.g. sales by store, customer lifetime value) optimized for reporting |



## Data Sources

| Source | Format | Description |
|--------|--------|-------------|
| **Stores** | SQL | Store metadata — location, region, store type, opening date |
| **Products** | SQL | Product catalog — SKU, category, price, supplier |
| **Transactions** | SQL | Sales transactions — store ID, product ID, customer ID, quantity, amount, timestamp |
| **Customers** | JSON | Customer profiles — demographics, contact info, loyalty status |



##  Tech Stack

- **Azure** — Cloud infrastructure (storage, orchestration, and ingestion)
- **Azure Data Lake Storage (ADLS Gen2)** Centralized storage for raw and processed data
- **Azure Data Factory** *(if used for orchestration) Pipeline scheduling and data movement
- **Databricks**  Data transformation, cleaning, and modeling (PySpark / Spark SQL)
- **Delta Lake**  Reliable, versioned storage format for the Medallion layers
- **Power BI**  Final reporting layer and interactive dashboards


## Pipeline Workflow

1. Ingestion
   - SQL tables (Stores, Products, Transactions) are extracted and loaded into the Bronze layer.
   - JSON customer files are ingested and flattened into a tabular structure.

2. Transformation (Databricks)
   - Schema validation and data type enforcement.
   - Deduplication and null handling.
   - Joining Transactions with Stores, Products, and Customers to build a unified fact table.
   - Aggregations for key business metrics (e.g. revenue per store, top products, customer segments).

3. Serving
   - Gold layer tables are exposed to Power BI via a Databricks SQL endpoint / connector.
   - Power BI dashboards refresh on a scheduled basis.



## Power BI Dashboard

The final dashboard includes:
- Sales performance by store and region
- Top-selling products and categories
- Customer segmentation and loyalty insights
- Revenue trends over time


## Getting Started

### Prerequisites
- Azure subscription with access to Data Lake Storage
- Databricks workspace (Azure Databricks recommended for native integration)
- Power BI Desktop / Power BI Service

### Setup
1. Clone this repository:
   bash
   git clone https://github.com/<your-username>/retail-data-platform.git
   
2. Configure your Azure storage and Databricks connection details in the config file.
3. Run the ingestion notebooks/scripts to load Stores, Products, Transactions, and Customers data.
4. Run the transformation notebooks to build the Bronze → Silver → Gold layers.
5. Connect Power BI to the Gold layer tables and refresh the dashboard.


##  Project Structure

retail-data-platform/
├── data/
│   ├── raw/
│   │   ├── sql/            # Stores, Products, Transactions
│   │   └── json/           # Customers
├── notebooks/
│   ├── bronze_ingestion/
│   ├── silver_transformation/
│   └── gold_aggregation/
├── powerbi/
│   └── retail_dashboard.pbix
├── docs/
│   └── dashboard_preview.png
└── README.md


## Future Improvements
- Add automated orchestration (e.g. Azure Data Factory or Prefect) for scheduled pipeline runs.
- Implement data quality checks and alerting.
- Add CI/CD for notebook deployment.
- Expand to include real-time streaming for transactions.

---

##  Author

**Oscar** — Data Enginneer & AI Automation Specialist
 Johannesburg, South Africa

Feel free to connect or reach out with questions about this project.
