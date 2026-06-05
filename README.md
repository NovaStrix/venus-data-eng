# Venus Brand Data Engineering Pipeline

A documented end-to-end data engineering pipeline that cleans, standardizes, enriches, and loads business data into Supabase PostgreSQL for analytics and BI reporting.

## Project Overview

This project transforms raw operational data from a single multi-sheet Excel source into trusted warehouse tables and reporting-ready data marts. The pipeline is designed to improve data accuracy, consistency, and reporting speed so business users can analyze sales, stock, and market share with confidence.

The workflow covers raw data ingestion, data quality checks, cleaning, enrichment, warehouse loading, and mart creation for downstream dashboards and executive reporting.

## Architecture

The pipeline follows a simple ETL-style flow:

1. Extract raw data from a multi-sheet Excel workbook.
2. Clean and standardize each source table.
3. Enrich transactional data using the product master table.
4. Load curated tables into Supabase PostgreSQL.
5. Build reporting marts for sales, stock, and market share analytics.

## Source Data

The pipeline uses five input domains from one multi-sheet Excel file:

| Source | Description | Granularity |
|---|---|---|
| Sell-In (A, B, C) | Shipments from distributors to stores.| Daily.|
| Sell-Out | Product sales from stores to customers. | Daily. |
| Stock | Inventory at the beginning of the day. | Daily snapshot. |
| Market Share | External brand performance data. | Weekly snapshot. |
| Product Master | Product and SKU reference data. | Dimension table. |

## Data Cleaning and Transformation

Key preparation steps include:

- Missing value and duplicate checks across raw sheets.
- Product master correction for duplicate SKU pricing issues, including removal of invalid rows for SKU22 and SKU25 after validating against sell-out transactions.
- Market share formatting fixes, including date conversion and removal of the `brand:` prefix.
- Stock standardization by converting negative values to 0 because the source system treats negative stock as zero.
- Enrichment of sell-in, sell-out, and stock tables with product attributes from `product_master`.

## Warehouse Tables

The pipeline creates clean core tables in Supabase PostgreSQL.

| Table | Purpose |
|---|---|
| `product_master` | Product lookup table containing SKU, brand, format, size, and price. |
| `total_sell_ins` | Unified shipment table combining all distributor sell-in data. |
| `stock` | Daily inventory movement table with beginning stock, stock in, stock out, and end stock. |
| `market_share` | Weekly market share table with cleaned brand and date fields. |
| `sell_out` | Transaction-level sales table with unit price, total value, and product details. |

## Reporting Data Marts

To support BI tools and dashboards, the project builds three reporting marts.

| Data Mart | Purpose |
|---|---|
| `looker_sales_data_mart` | Tracks sales performance by product, store, and time with derived calendar dimensions. |
| `looker_stock_data_mart` | Supports inventory analysis with stock movement metrics and value of stock sold. |
| `looker_market_share_data_mart` | Supports weekly brand comparison and market share trend analysis. |

## Business Value

This pipeline creates one consistent version of the truth for reporting and analysis. It reduces confusion caused by inconsistent raw data, speeds up executive reporting, and improves visibility across sales, stock, and market position.

In practice, the project helps stakeholders answer questions such as:

- Are sales growing across stores and products?
- Are inventory levels healthy and operationally efficient?
- Is the brand gaining or losing market share over time?

## Example Transformation Logic

Some of the main business rules implemented in the pipeline include:

- `end_stock = beginning_stock - stock_out + stock_in` for daily stock calculation.
- `sell_out.value = qty * price` for transaction value calculation.
- ISO week, month, and day attributes added to marts for easier time-based reporting.
- `value_stock_out_weekly = stock_out * product_master.price` in the stock reporting mart.

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

- Python and pandas for ETL and transformation logic.
- Supabase PostgreSQL as the warehouse layer.
- BI-ready data marts for dashboarding and executive reporting.

## Future Improvements

Potential next steps identified in the project materials include:

- Improve handling and validation of negative stock records.
- Integrate the pipeline into a more automated company workflow.
- Add automated alerts to support faster operational action.

## Repository Description

End-to-end data engineering pipeline for Venus brand sales, stock, and market share data using Python and Supabase, with cleaned warehouse tables and BI-ready reporting marts.
