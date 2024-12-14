# E-Commerce Lakehouse with Medallion Architecture

[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![Delta Lake](https://img.shields.io/badge/Delta_Lake-3.1-00ADD8.svg)](https://delta.io/)
[![Databricks](https://img.shields.io/badge/Databricks-Runtime_14-FF3621.svg)](https://databricks.com/)

## Demo

![Project Demo](screenshots/project-demo.png)

*Medallion architecture flow (Bronze -> Silver -> Gold) with SCD Type 2 tracking, customer segmentation, and pipeline execution status*

## Architecture

```
Source Systems                    Medallion Architecture                     Serving
+----------+                 +---------+ +---------+ +---------+ 
| PostgreSQL|---CDC--------->| BRONZE  |->| SILVER  |->|  GOLD   |---> BI / Analytics
| (Orders) |               |  (Raw)  | |(Cleaned)| | (Marts) |
+----------+                 +---------+ +---------+ +---------+ 
+----------+                     |           |           |
| REST API |---Batch------------>|           |           |
|(Products)|                     |     Schema|     Star  |
+----------+                     |   Enforce |    Schema |
+----------+                     |     SCD   |    Facts  |
|CSV/Parquet|---Upload---------->|    Type 2 |    Dims   |
| (Legacy) |                     |           |           |
+----------+                Unity Catalog Governance Layer
```

## Features

- **Bronze Layer**: Raw ingestion with schema-on-read, append-only
- **Silver Layer**: Schema enforcement, dedup, SCD Type 2, data quality checks
- **Gold Layer**: Star schema with fact/dimension tables for analytics
- **CDC Pipeline**: Debezium-style CDC from PostgreSQL via Delta Lake change data feed
- **Unity Catalog**: Table-level access control and data lineage
- **Auto Loader**: Incremental file ingestion with Auto Loader pattern
- **Time Travel**: Delta Lake versioning for audit and rollback
- **dbt Models**: Full dbt project for Silver -> Gold transformation

## Quick Start

```bash
cp .env.example .env
# Configure Databricks workspace credentials

# Local development with Delta Lake + PySpark
pip install -r requirements.txt
python -m pipelines.bronze_ingestion
python -m pipelines.silver_transform
python -m pipelines.gold_aggregate

# Or run via Databricks notebooks
databricks workspace import-dir ./notebooks /Shared/ecommerce-lakehouse
```

## Project Structure

```
+-- config/                  # Settings and schemas
+-- pipelines/               # ETL pipeline code
|   +-- bronze_ingestion.py  # Raw data landing
|   +-- silver_transform.py  # Cleaning + SCD Type 2
|   +-- gold_aggregate.py    # Star schema materialization
+-- notebooks/               # Databricks notebook equivalents
+-- dbt_project/             # dbt for Gold layer
+-- quality/                 # Data quality checks
+-- unity_catalog/           # Catalog/schema setup SQL
+-- docker-compose.yml       # Local PostgreSQL + data generator
+-- tests/                   # Unit tests
```

## Test Results

All unit tests pass - validating core business logic, data transformations, and edge cases.

![Test Results](screenshots/test-results.png)

**8 tests passed** across 2 test suites:
- `TestSilverTransform` - status normalization, line totals, SCD2 change detection
- `TestGoldAggregation` - value segment classification, RFM scoring

## Maintainer

**Chinmaya Sri Rama Seshu Pasupuleti**
Data Engineer
Email: pramaseshu@outlook.com
GitHub: https://github.com/Ramaseshu0
LinkedIn: https://www.linkedin.com/in/rama-seshu/

I am a Data Engineer with over 4 years of experience building enterprise data pipelines, cloud-based ETL/ELT workflows, and analytical data platforms. This project serves as a comprehensive implementation of a modern data lakehouse using the Medallion Architecture, PySpark, and Delta Lake. My expertise includes Python, SQL, Apache Spark, and Databricks, with a focus on data ingestion, distributed processing, and pipeline orchestration.