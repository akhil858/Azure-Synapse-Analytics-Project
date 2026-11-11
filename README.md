# Azure Synapse Analytics Project

This repository contains an Azure Synapse Analytics project for ingesting and preparing sales data for analytics. It demonstrates a data engineering workflow that copies CSV data into a Parquet-based bronze layer, applies a mapping data flow for transformation, and includes SQL scripts for setting up silver- and gold-layer warehouse objects.

## Overview

The project is configured to process sales data with order, customer, product, and regional fields. Its Synapse workspace artifacts define the source dataset, data lake connections, ingestion and transformation pipelines, mapping data flow, and SQL scripts.

The current source dataset is configured for a single file, `order_day1.csv`. The repository does not include that input file or the Azure resources required to run the workflow.

## Architecture

The project uses a medallion-style layout:

- **Source:** A CSV file in the `source` file system.
- **Bronze:** The `raw_ingestion` pipeline copies the CSV data to Parquet in the `bronze/sales` location.
- **Silver:** The `Dataflow` mapping data flow filters and enriches the bronze data, with output configured for `silver/enrichedSales`.
- **Gold:** SQL scripts define gold-layer fact and dimension objects for analytics.

## Key Features

- CSV-to-Parquet ingestion using an Azure Synapse pipeline
- Bronze and silver storage locations configured in Azure Data Lake Storage Gen2
- Mapping Data Flow transformations that exclude rows where
