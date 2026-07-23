# F1 Analytics Pipeline on Microsoft Fabric

## Overview

This project implements an end-to-end Formula 1 analytics pipeline using Microsoft Fabric. It ingests raw race data from Azure Storage, processes it through a medallion architecture (Bronze → Silver → Gold), and delivers interactive reports via Power BI.

The goal was to build a production-grade data pipeline that transforms raw data into business-ready insights while handling real-world challenges like path management, cross-layer access, and reporting performance.

## Architecture

## Architecture
┌─────────────────┐

│ Azure Storage │
│ (Raw F1 Data) │

└────────┬────────┘


│

▼


┌─────────────────┐

│ Bronze Lakehouse│ ← Raw ingestion

└────────┬────────┘


│

▼


┌─────────────────┐

│ Silver Lakehouse│ ← Clean & transform

└────────┬────────┘


│

▼


┌─────────────────┐

│ Gold Warehouse │ ← Reporting tables

└────────┬────────┘


│

▼

┌─────────────────┐

│ Power BI Report │ ← Interactive dashboard

└─────────────────┘


## Data Flow

### 1. Bronze Layer (Raw Ingestion)
- Source: Azure Storage (Parquet files)
- Destination: Bronze Lakehouse tables
- Tables: `drivers`, `races`, `results`, `constructors`
- No transformations applied

### 2. Silver Layer (Cleansing & Standardization)
- Drivers: Renamed columns, meaningful names
- Races: Cleaned + standardized date formats
- Results: Casted points/grid to proper numeric types
- Constructors: Standardized team names

### 3. Gold Layer (Reporting Ready)
- Destination: Gold Warehouse
- Final table: `f1_race_results`
- Columns: `race_year`, `race_name`, `race_date`, `driver_name`, `team`, `points`, `grid`
- Created SQL views for reusable logic

### 4. Power BI
- Connection: Import mode to Gold Warehouse
- Created semantic model with measures
- Built interactive dashboards with KPIs, trends, and race-level breakdowns

## Project Structure

├── README.md
├── /notebooks
│ ├── bronze_ingestion.ipynb
│ ├── silver_transformation.ipynb
│ └── gold_warehouse_load.ipynb
├── /sql
│ ├── create_views.sql
│ └── sample_queries.sql
├── /architecture
│ └── pipeline_diagram.png
├── /screenshots
│ ├── bronze_layer.png
│ ├── silver_layer.png
│ ├── gold_warehouse.png
│ └── powerbi_dashboard.png
└── /demo
└── demo_thumbnail.png

## Key Transformations

### Bronze → Silver
- Standardized column names (e.g., `driver_forename` + `driver_surname` → `driver_name`)
- Cast numeric fields (points, grid) to proper types
- Cleaned date formats for race dates
- Removed nulls and duplicates

### Silver → Gold
- Joined multiple tables into a single fact table
- Added business-friendly column names
- Optimized for Power BI reporting

## Challenges & Solutions

| Challenge | Solution |
|-----------|----------|
| Cross-lakehouse read/write paths | Used explicit lakehouse references and separated source/target layers |
| Warehouse write limitations from Spark | Kept Gold in Warehouse for reporting, used notebooks for transformation |
| Column mismatch errors | Applied correct transformations per dataset |
| Reporting performance | Chose Power BI Import mode over DirectQuery |
| Reusability across data sources | Created SQL views for common logic |

## Tools & Technologies

- **Microsoft Fabric** (Lakehouse, Warehouse, Data Engineering, Data Warehouse experiences)
- **Azure Storage** (raw data source)
- **Apache Spark** (PySpark notebooks)
- **SQL** (views, queries)
- **Power BI** (semantic model, reports, dashboards)
- **Parquet** (file format)

## What I Learned

- Production patterns for medallion architecture in Fabric
- How to manage cross-layer data movement efficiently
- Trade-offs between Lakehouse vs Warehouse for reporting
- Best practices for Power BI semantic models and measures
- Importance of clear naming, paths, and documentation

## Demo

[![Watch Demo](demo/demo_thumbnail.png)](https://youtube.com/watch?v=YOUR_VIDEO_ID)


## Screenshots

### Bronze Layer
![Bronze](screenshots/bronze_layer.png)

### Silver Layer
![Silver](screenshots/silver_layer.png)

### Gold Warehouse
![Gold](screenshots/gold_warehouse.png)

### Power BI Dashboard
![Power BI](screenshots/powerbi_dashboard.png)

## How to Run

1. Clone this repository
2. Open Microsoft Fabric workspace
3. Upload notebooks to your Lakehouse
4. Update source paths to match your Azure Storage
5. Run notebooks in order: Bronze → Silver → Gold
6. Execute SQL scripts to create views
7. Connect Power BI to Gold Warehouse
8. Import the semantic model and build reports

## Next Steps

- Add incremental data loads
- Implement data quality checks
- Add more dimensions (e.g., circuits, seasons)
- Deploy to production workspace
- Automate with Data Factory / Fabric pipelines

---

Built with ❤️ using Microsoft Fabric
