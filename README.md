# Airflow + dbt + Snowflake Data Pipeline

## Project Overview

This project demonstrates a production-ready data engineering pipeline that orchestrates dbt transformations in Snowflake using Apache Airflow. It showcases modern data orchestration practices using Astronomer Cosmos to seamlessly integrate dbt models into Airflow DAGs.

## Architecture

**Tech Stack:**
- **Apache Airflow** - Workflow orchestration platform
- **dbt (data build tool)** - SQL-based transformation framework
- **Snowflake** - Cloud data warehouse
- **Astronomer Cosmos** - Library for running dbt projects as Airflow DAGs
- **Astronomer Runtime** - Containerized Airflow deployment

## Project Structure
```
dbt-dag/
├── dags/
│   ├── dbt/
│   │   └── data_pipeline/          # dbt project
│   │       ├── models/
│   │       │   ├── staging/        # Source data staging models
│   │       │   └── marts/          # Business logic & fact tables
│   │       ├── macros/             # Reusable SQL functions
│   │       └── tests/              # Data quality tests
│   └── dbt_dag.py                  # Airflow DAG definition
├── Dockerfile                       # Custom Airflow image with dbt
└── requirements.txt                 # Python dependencies
```

## Key Features

### 1. **dbt Project - TPC-H Data Pipeline**
The dbt project transforms sample TPC-H (Transaction Processing Performance Council) data from Snowflake into analytics-ready models:

**Staging Models:**
- `stg_tpch_orders` - Cleaned and standardized order data
- `stg_tpch_line_items` - Cleaned line item details with surrogate keys

**Intermediate Models:**
- `int_order_items` - Joins orders and line items with discount calculations
- `int_order_items_summary` - Aggregated order metrics

**Mart Models:**
- `fct_orders` - Final fact table combining order details with item-level metrics

**Custom Macros:**
- `discounted_amount()` - Reusable pricing calculation logic

### 2. **Data Quality Testing**
- **Generic tests:** Uniqueness, null checks, referential integrity, accepted values
- **Custom tests:** Date validation, business rule enforcement (discount amounts)
- Tests run automatically as part of the dbt workflow

### 3. **Airflow Orchestration**
The `dbt_dag.py` file creates a self-contained DAG that:
- Automatically generates Airflow tasks from dbt models
- Maintains proper task dependencies based on dbt model relationships
- Runs dbt in a dedicated virtual environment
- Executes daily at scheduled intervals
- Connects to Snowflake using Airflow connections

### 4. **Docker-Based Deployment**
- Custom Dockerfile extending Astronomer Runtime
- Isolated dbt virtual environment for dependency management
- Ready for deployment to Astronomer Cloud or local development

### 5. **CI/CD Testing**
Includes pytest-based tests to validate:
- DAG integrity and parse errors
- Proper tagging and retry configuration
- Import validation across all DAG files

## Configuration Highlights

**Snowflake Integration:**
- Uses Snowflake sample TPC-H dataset (`SNOWFLAKE_SAMPLE_DATA.TPCH_SF1`)
- Outputs to custom database/schema (`dbt_db.dbt_schema`)
- Configurable warehouse for staging vs. marts models

**Materialization Strategy:**
- Staging models → Views (fast, minimal storage)
- Marts models → Tables (optimized for querying)

## Use Cases Demonstrated

✅ Modern data stack integration (Airflow + dbt + Snowflake)  
✅ Incremental data transformation patterns  
✅ Data quality enforcement through automated testing  
✅ Modular SQL with macros and ref() functions  
✅ Production-ready containerization and deployment  
✅ Proper separation of staging, intermediate, and mart layers

## Getting Started

1. **Prerequisites:** Astronomer CLI, Snowflake account with TPC-H sample data
2. **Configure:** Add Snowflake connection (`snowflake_conn`) in Airflow UI
3. **Deploy:** `astro dev start` for local development
4. **Run:** DAG executes daily automatically or trigger manually

---

This project demonstrates proficiency in modern data engineering workflows, infrastructure-as-code practices, and SQL-based analytics engineering.
