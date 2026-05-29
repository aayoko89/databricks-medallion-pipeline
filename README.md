# Manufacturing Quality ETL Pipeline — Medallion Architecture on Databricks

End-to-end data engineering pipeline built on Databricks using the Medallion Architecture (Bronze/Silver/Gold) to process manufacturing defect data at scale.

## Architecture

Raw CSV Data → Bronze (Delta) → Silver (Delta) → Gold (Delta) → Analytics

## Tech Stack
- **Platform:** Databricks (Serverless)
- **Processing:** PySpark, Spark SQL
- **Storage:** Delta Lake (Medallion Architecture)
- **Languages:** Python, SQL

## Pipeline Layers

### 🥉 Bronze — Raw Ingestion
- Ingests 1,050 raw manufacturing records into Delta Lake as-is
- Adds metadata columns: `ingested_at`, `source`
- No transformations — preserves raw state for auditability

### 🥈 Silver — Cleansed & Conformed
- Removes 50 duplicate records via `dropDuplicates()`
- Standardizes `shift` and `defect_type` to uppercase using PySpark transforms
- Normalizes null/inconsistent defect values to `NO_DEFECT`
- Imputes null `temperature_c` and `units_produced` with column averages
- Adds `has_defect` binary flag for downstream ML use

### 🥇 Gold — Aggregated KPIs
Three analytics-ready Delta tables:

| Table | Description |
|---|---|
| `gold_machine_kpis` | Defect rate %, total units, avg temp per machine |
| `gold_shift_defects` | Defect type breakdown by shift |
| `gold_summary` | Plant-wide KPIs: 274K units, 5.6% defect rate |

## Key Results
- **274,296** total units processed
- **5.6%** overall defect rate identified
- **MACH-03** flagged as highest defect machine (6.09%)
- **Night shift** identified as highest CRACK defect period

## What This Demonstrates
- Medallion Architecture design on Databricks
- PySpark DataFrame transformations and aggregations
- Delta Lake table creation and management
- Data quality handling: deduplication, null imputation, standardization
- Production-ready pipeline structure with metadata tracking
