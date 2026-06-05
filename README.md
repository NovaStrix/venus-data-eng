# Venus Brand Data Engineering Pipeline

A documented end-to-end data engineering pipeline that cleans, standardizes, enriches, and loads business data into Supabase PostgreSQL for analytics and BI reporting.[1][2]

## Project Overview

This project transforms raw operational data from a single multi-sheet Excel source into trusted warehouse tables and reporting-ready data marts.[1][2] The pipeline is designed to improve data accuracy, consistency, and reporting speed so business users can analyze sales, stock, and market share with confidence.[2]

The workflow covers raw data ingestion, data quality checks, cleaning, enrichment, warehouse loading, and mart creation for downstream dashboards and executive reporting.[1][2]

## Architecture

The pipeline follows a simple ETL-style flow:

1. Extract raw data from a multi-sheet Excel workbook.[1][2]
2. Clean and standardize each source table.[1][2]
3. Enrich transactional data using the product master table.[1]
4. Load curated tables into Supabase PostgreSQL.[1][2]
5. Build reporting marts for sales, stock, and market share analytics.[1][2]

## Source Data

The pipeline uses five input domains from one multi-sheet Excel file:[1]

| Source | Description | Granularity |
|---|---|---|
| Sell-In (A, B, C) | Shipments from distributors to stores.[1] | Daily.[1] |
| Sell-Out | Product sales from stores to customers.[1] | Daily.[1] |
| Stock | Inventory at the beginning of the day.[1] | Daily snapshot.[1] |
| Market Share | External brand performance data.[1] | Weekly snapshot.[1] |
| Product Master | Product and SKU reference data.[1] | Dimension table.[1] |

## Data Cleaning and Transformation

Key preparation steps include:[1][2]

- Missing value and duplicate checks across raw sheets.[1]
- Product master correction for duplicate SKU pricing issues, including removal of invalid rows for SKU22 and SKU25 after validating against sell-out transactions.[1]
- Market share formatting fixes, including date conversion and removal of the `brand:` prefix.[1][2]
- Stock standardization by converting negative values to 0 because the source system treats negative stock as zero.[1][2]
- Enrichment of sell-in, sell-out, and stock tables with product attributes from `product_master`.[1]

## Warehouse Tables

The pipeline creates clean core tables in Supabase PostgreSQL.[1][2]

| Table | Purpose |
|---|---|
| `product_master` | Product lookup table containing SKU, brand, format, size, and price.[1] |
| `total_sell_ins` | Unified shipment table combining all distributor sell-in data.[1] |
| `stock` | Daily inventory movement table with beginning stock, stock in, stock out, and end stock.[1] |
| `market_share` | Weekly market share table with cleaned brand and date fields.[1] |
| `sell_out` | Transaction-level sales table with unit price, total value, and product details.[1] |

## Reporting Data Marts

To support BI tools and dashboards, the project builds three reporting marts.[1][2]

| Data Mart | Purpose |
|---|---|
| `looker_sales_data_mart` | Tracks sales performance by product, store, and time with derived calendar dimensions.[1][2] |
| `looker_stock_data_mart` | Supports inventory analysis with stock movement metrics and value of stock sold.[1][2] |
| `looker_market_share_data_mart` | Supports weekly brand comparison and market share trend analysis.[1][2] |

## Business Value

This pipeline creates one consistent version of the truth for reporting and analysis.[2] It reduces confusion caused by inconsistent raw data, speeds up executive reporting, and improves visibility across sales, stock, and market position.[2]

In practice, the project helps stakeholders answer questions such as:

- Are sales growing across stores and products?[2]
- Are inventory levels healthy and operationally efficient?[2]
- Is the brand gaining or losing market share over time?[2]

## Example Transformation Logic

Some of the main business rules implemented in the pipeline include:[1]

- `end_stock = beginning_stock - stock_out + stock_in` for daily stock calculation.[1]
- `sell_out.value = qty * price` for transaction value calculation.[1]
- ISO week, month, and day attributes added to marts for easier time-based reporting.[1]
- `value_stock_out_weekly = stock_out * product_master.price` in the stock reporting mart.[1]

## Suggested Repository Structure

```text
.
├── data/
│   └── raw/
├── notebooks/
│   └── pipeline.ipynb
├── sql/
│   └── marts/
├── docs/
│   └── pipeline-guide.pdf
├── src/
│   ├── extract.py
│   ├── transform.py
│   └── load.py
└── README.md
```

## Tech Stack

- Python and pandas for ETL and transformation logic.[2]
- Supabase PostgreSQL as the warehouse layer.[1][2]
- BI-ready data marts for dashboarding and executive reporting.[1][2]

## Future Improvements

Potential next steps identified in the project materials include:[2]

- Improve handling and validation of negative stock records.[2]
- Integrate the pipeline into a more automated company workflow.[2]
- Add automated alerts to support faster operational action.[2]

## Repository Description

End-to-end data engineering pipeline for Venus brand sales, stock, and market share data using Python and Supabase, with cleaned warehouse tables and BI-ready reporting marts.[1][2]
