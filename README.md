
Azure Synapse Analytics Project
This repository contains an Azure Synapse Analytics project for ingesting and preparing sales data for analytics. It demonstrates a data engineering workflow that copies CSV data into a Parquet-based bronze layer, applies a mapping data flow for transformation, and includes SQL scripts for setting up silver- and gold-layer warehouse objects.

Overview
The project is configured to process sales data with order, customer, product, and regional fields. Its Synapse workspace artifacts define the source dataset, data lake connections, ingestion and transformation pipelines, mapping data flow, and SQL scripts.

The current source dataset is configured for a single file, order_day1.csv. The repository does not include that input file or the Azure resources required to run the workflow.

Architecture
The project uses a medallion-style layout:

Source: A CSV file in the source file system.
Bronze: The raw_ingestion pipeline copies the CSV data to Parquet in the bronze/sales location.
Silver: The Dataflow mapping data flow filters and enriches the bronze data, with output configured for silver/enrichedSales.
Gold: SQL scripts define gold-layer fact and dimension objects for analytics.
Key Features
CSV-to-Parquet ingestion using an Azure Synapse pipeline
Bronze and silver storage locations configured in Azure Data Lake Storage Gen2
Mapping Data Flow transformations that exclude rows where Country is USA and derive a domain value from CustomerEmail
SQL scripts for schemas, external resources, silver tables, dimensions, and a fact table
Managed integration runtime configuration
Synapse workspace artifacts stored as JSON
Project Components
Pipelines
raw_ingestion — Copies data from the configured CSV dataset to Parquet in the bronze location.
TransformationLayer — Defines a data-flow activity followed by a SQL script activity for the silver table.
Data Flow
Dataflow — Reads the bronze Parquet dataset, filters out USA records, derives an email domain value, and writes Parquet output to the silver dataset.
Datasets
The repository includes dataset definitions for:

ds_rawCSV — The configured order_day1.csv source, with order, customer, product, and regional columns.
ds_Parquet — Parquet output for the raw ingestion pipeline.
ds_Parquet_Bronze — Bronze-layer Parquet input for the data flow.
ds_silver — Silver-layer Parquet output in enrichedSales.
Linked Services and Runtime
The linked services define connections for Azure Data Lake Storage Gen2 and Synapse SQL. The project also includes a managed AutoResolveIntegrationRuntime and a workspace managed-identity credential artifact.

These definitions contain environment-specific configuration. Review and update them for your own Azure resources before publishing or running the project.

SQL Scripts
The sqlscript/ directory contains Synapse SQL artifacts for:

Creating the silver and gold schemas
Defining external credentials and data resources
Creating the silver table
Defining gold dimension and fact objects
Review the scripts and their prerequisites before executing them in your workspace.

Data Processing Flow
Place the input CSV at the location configured by ds_rawCSV: source/order_day1.csv.
Run raw_ingestion to copy the source data to Parquet in the bronze sales location.
Run the transformation workflow to filter and enrich the bronze data and write the result to the silver enrichedSales location.
Run the relevant SQL scripts to set up external resources and silver/gold warehouse objects.
Business Use Case
This project is a learning example of an Azure-based sales data workflow. It illustrates how source data can be ingested into a data lake, transformed into a curated dataset, and made available through SQL warehouse objects for downstream analytics.

Tools and Technologies
Azure Synapse Analytics
Azure Synapse Pipelines
Mapping Data Flows
Azure Data Lake Storage Gen2
Synapse serverless SQL
Parquet
JSON-based Synapse workspace artifacts
Repository Structure
Text
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
Setup Notes
This repository contains Synapse workspace artifacts rather than a standalone application, so there is no local run command or test suite.

Before using the project:

Connect the repository to an Azure Synapse workspace.
Update linked services and datasets for your storage account, file systems, paths, SQL endpoint, and database.
Confirm that the workspace identity has the required access to storage and SQL resources.
Review authentication and credential configuration. Do not commit passwords, tokens, or other secrets.
Validate and publish the artifacts in Synapse Studio.
