# eu-covid19-adf

## Overview
This project orchestrates the end-to-end data integration of Europe COVID-19 datasets using Azure Data Factory. It automates the ingestion, validation, transformation, and loading of COVID-19 case, death, testing, and population data into a structured format.

## Table of Contents
1. [Overview](#Overview)
2. [Architecture](#Architecture)
3. [Project Structure](#ProjectStructure)
4. [Data Source](#DataSource)

## Architecture
<img width="1308" alt="image" src="https://github.com/user-attachments/assets/86c509fb-63dd-42e6-8d1d-e08ba2524e30" />

## Project Structure
Directory structure:
```└── itsannhienjoy-eu-covid19-adf/ 
    ├── README.md
    ├── publish_config.json
    ├── dataflow/        --  # Contains all data flow definitions
    ├── dataset/         --  # Contains dataset definition
    ├── factory/         --  # Factory configuration
    ├── linkedService/   --  # Linked service connections
    ├── pipeline/        --  # Pipeline definitions
    └── trigger/         --  # Pipeline triggers
```

## Data Source
[Europe COVID-19 datasets were collected from the European Centre for Disease Prevention and Control (ECDC).](https://www.ecdc.europa.eu/en)

## Technology Stack
1. Azure Data Factory

Pipelines

📄 Pipeline: pl_ingest_ecdc
Automates the ingestion of raw case and death data from ECDC stored in ADLS Gen2. It retrieves a list of files and iterates over them using a ForEach activity, copying each into the destination for downstream processing.
![image](https://github.com/user-attachments/assets/4749c8fd-7db4-4eb4-8f4d-e03b86380a4d)

📄 Pipeline: pl_ingest_population
Validates and ingests population data files. It checks for file existence, verifies schema integrity using metadata, and conditionally copies valid files into the data lake. Invalid files can be logged or flagged for review.
![image](https://github.com/user-attachments/assets/a129cb59-6a92-4a30-b955-8f8f363b2a40)

📄 Pipeline: pl_process_casesDeaths
Processes raw case and death data using the df_transform_casesDeaths data flow, which cleanses and reshapes the data for analytics.
![image](https://github.com/user-attachments/assets/0a1e7e75-603f-4528-8af1-356f623e1460)

Data flows
🔄 Data Flow: df_transform_hospitalAdmissions
Transforms raw hospital admissions data by joining with lookup tables, aggregating by date and country, and creating weekly and daily outputs.
![image](https://github.com/user-attachments/assets/37b5b87f-97a5-471c-8ceb-dc372138f7c6)


🔄 Data Flow: df_transform_casesDeaths
Filters for European countries, pivots COVID-19 case/death indicators, enriches with metadata, and exports structured results.
![image](https://github.com/user-attachments/assets/aaebb0f1-6811-46c9-9ee3-1fb69885e330)


🔄 Data Flow: df_transform_testing
Processes raw testing data by integrating date and country dimensions, performing weekly aggregation, and exporting the results.
![image](https://github.com/user-attachments/assets/2b74ac0a-586a-4a5d-9d13-7182390f2916)


2. Databricks

Executes a Databricks notebook to transform raw population data. It filters to 2019, cleans percentage values, pivots by age group, enriches with country metadata, and saves the processed output to ADLS.
![image](https://github.com/user-attachments/assets/85b73a5e-f506-47d9-8658-7c85894b8c60)

![image](https://github.com/user-attachments/assets/38cc78c8-308d-40bf-ab22-05a02ab78967)

3. Azure Data Lake Gen 2

Used for lookup, raw and processed data storage.
![image](https://github.com/user-attachments/assets/73735056-4068-42ac-84d5-e12af47ee09a)

4. Azure SQL DB

Azure SQL DB is used as a final destination for curated COVID-19 datasets. Data from processed pipelines (e.g., testing or hospital admissions) can be written to SQL tables for easy querying, reporting, or BI consumption.

![image](https://github.com/user-attachments/assets/f6c213fc-4714-453f-8580-55d98f22b417)

![image](https://github.com/user-attachments/assets/5401d8e4-4503-442a-9203-a0f108ab9d0a)

![image](https://github.com/user-attachments/assets/e6526950-9f90-41f6-927a-133971af7ced)


