# Alpha Vantage ETL Project

## Purpose: 
To extract daily stock data of 2 stocks from Alpha Vantage to demonstrate ETL pipeline 

## ETL Architecture: 
Azure ADLS Gen 2 storage -> Azure Databricks Medallion Setup (Bronze -> Silver -> Gold) -> Power BI

## Overview
This project seeks to showcase a simple end-to-end ETL pipeline:
- Raw data is ingested via API call using Databricks connector and stored in AZURE ADLS GEN 2 
- JSON format raw data processed in AZURE DATABRICKS using MEDALLION architecture (Bronze → Silver → Gold)
- Use Power BI to produce report to show insights 

## Architecture Flow
1. BRONZE LAYER
   - Ingest raw OHLCV stock data (NVDA, MSFT) using API call in Databricks and create Dataframes
   - Write raw data using compute in Databricks to AZURE ADLS GEN 2 container for storage
   - Simple sanity checks applied to check for null values and row/column counts 
   - Store as Bronze Delta tables in Databricks

2. SILVER LAYER
   - Clean and ensure schema (date, symbol, open, close, high, low, volume) is populating correctly
   - Drop duplicates, recasting of datatypes, renaming of columns and checking if null values are valid 
   - Enforce no null values for mandatory columns OHLCV, date, symbol
   - Store as Silver Delta table in Databricks

3. GOLD LAYER 
   - Creation of aggregate monthly table (open_month, close_month, high_month, low_month, volume_month)
   - Add additional derived metric columns (pct_change_intraday, pct_change_close)
   - Store as Gold Delta tables for analytics.

4. POWER BI Integration
   - Connect Power BI Desktop to Databricks SQL Warehouse
   - Import 2x Gold tables (gold_daily, gold_monthly)
   - Prepare a simple monthly comparison report (NVDA vs MSFT) showing daily closing prices and % intraday change
     - Include filtering by months 
   - Publish to Power BI Service browser.

## Platforms 
- AZURE ADLS GEN: Raw data storage (bronze folder)
- DATABRICKS: Build ETL pipeline with Medallion architecture 
- POWER BI: Visualization and reporting

## Report Generated (NVDA vs MSFT)
- Line chart: Daily closing price
- Cluster Column chart: Percentage change in intraday price

## Outcomes 
- ETL pipeline successfully implemented
- Report published in Power BI service 
