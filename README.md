Azure Synapse Analytics Project
This repository contains an Azure Synapse Analytics project for ingesting and preparing sales data for analytics. It demonstrates an end-to-end data engineering workflow: copying CSV data into a Parquet-based bronze layer, applying transformations through a mapping data flow, and using SQL scripts to define silver- and gold-layer warehouse objects.

Overview
The project is configured to process sales data with order, customer, product, and regional fields. Its Synapse workspace artifacts include the source dataset, data lake connections, ingestion and transformation pipelines, mapping data flow, and SQL scripts.

The workflow uses order_day1.csv as its configured source and organizes data into bronze and silver layers, with SQL artifacts for gold-layer analytics objects.

Architecture
The project follows a medallion-style data architecture:

Source: Sales data is read from a CSV file in the source file system.
Bronze: The raw_ingestion pipeline copies the source data to Parquet in the bronze/sales location.
Silver: The Dataflow mapping data flow filters and enriches the bronze data, with output configured for silver/enrichedSales.
Gold: SQL scripts define fact and dimension objects for analytics.
Key Features
CSV-to-Parquet ingestion using an Azure Synapse pipeline
Bronze and silver storage locations configured in Azure Data Lake Storage Gen2
Mapping Data Flow transformations, including filtering USA records and deriving a domain value from CustomerEmail
SQL scripts for schemas, external resources, silver tables, dimensions, and a fact table
Managed integration runtime configuration
Synapse workspace artifacts stored as JSON
Project Components
Pipelines
raw_ingestion — Copies data from the configured CSV dataset to Parquet in the bronze location.
TransformationLayer — Orchestrates data-flow transformation and a SQL script activity for the silver table.
Data Flow
Dataflow — Reads the bronze Parquet dataset, filters out records where Country is USA, derives a domain value from CustomerEmail, and writes Parquet output to the silver dataset.
Datasets
The repository includes dataset definitions for:

ds_rawCSV — The configured order_day1.csv source, with order, customer, product, and regional columns.
ds_Parquet — Parquet output for the raw ingestion pipeline.
ds_Parquet_Bronze — Bronze-layer Parquet input for the data flow.
ds_silver — Silver-layer Parquet output in enrichedSales.
Linked Services and Runtime
The linked services define connections for Azure Data Lake Storage Gen2 and Synapse SQL. The project also includes a managed AutoResolveIntegrationRuntime and a workspace managed-identity credential artifact.

SQL Scripts
The sqlscript/ directory contains Synapse SQL artifacts for:

Creating the silver and gold schemas
Defining external credentials and data resources
Creating the silver table
Defining gold dimension and fact objects
Data Processing Flow
Place the input CSV at the location configured by ds_rawCSV: source/order_day1.csv.
Run raw_ingestion to copy the source data to Parquet in the bronze sales location.
Run the transformation workflow to filter and enrich the bronze data and write the result to the silver enrichedSales location.
Run the relevant SQL scripts to set up external resources and silver/gold warehouse objects.
Business Use Case
This project demonstrates an Azure-based sales data workflow for integrating, transforming, and preparing data for analysis. Its layered storage and SQL objects provide a foundation for downstream analytics and reporting.

Tools and Technologies
Azure Synapse Analytics
Azure Synapse Pipelines
Mapping Data Flows
Azure Data Lake Storage Gen2
Synapse serverless SQL
Parquet
JSON-based Synapse workspace artifacts
Repository Structure
Azure-Synapse-Analytics-Project/
├── credential/         # Workspace managed-identity credential
├── dataflow/           # Bronze-to-silver mapping data flow
├── dataset/            # CSV and Parquet dataset definitions
├── integrationRuntime/ # Managed integration runtime
├── linkedService/      # Storage and SQL connection definitions
├── pipeline/           # Ingestion and transformation pipelines
├── sqlscript/          # Schema, external-resource, and table scripts
├── publish_config.json # Synapse Git publish configuration
└── README.md
Setup
This repository contains Synapse workspace artifacts rather than a standalone application, so it does not have a local run command.

Connect the repository to an Azure Synapse workspace.
Update linked services and datasets for your storage account, file systems, paths, SQL endpoint, and database.
Confirm that the workspace identity has the required access to storage and SQL resources.
Place the source CSV at the location configured in ds_rawCSV.
Validate and publish the artifacts in Synapse Studio, then run the pipelines and SQL scripts for your environment.
Review authentication and credential configuration before publishing, and avoid committing passwords, tokens, or other secrets.the transformation pipeline.
