# Data Warehouse and Analytics Project

Welcome to the **Data Warehouse and Analytics Project** repository! 🚀  
This project demonstrates a comprehensive data warehousing and analytics solution, from building a data warehouse to generating actionable insights. Designed as a portfolio project, it highlights industry best practices in data engineering and analytics.

---

## 🏗️ Data Architecture

The data architecture for this project follows Medallion Architecture **Bronze**, **Silver**, and **Gold** layers:

![Data Architecture](docs/data_architecture.png)

1. **Bronze Layer**: Stores raw data as-is from the source systems. Data is ingested from CSV Files into SQL Server Database.
2. **Silver Layer**: This layer includes data cleansing, standardization, and normalization processes to prepare data for analysis.
3. **Gold Layer**: Houses business-ready data modeled into a star schema required for reporting and analytics (`dim_customers`, `dim_products`, `fact_sales`).

---

## 📖 Project Overview

This project involves:

1. **Data Architecture**: Designing a Modern Data Warehouse Using Medallion Architecture **Bronze**, **Silver**, and **Gold** layers.
2. **ETL Pipelines**: Extracting, transforming, and loading data from source systems into the warehouse.
3. **Data Modeling**: Developing fact and dimension tables optimized for analytical queries.
4. **Analytics & Reporting**: Creating SQL-based reports and dashboards for actionable insights.

🎯 This repository is an excellent resource for professionals and students looking to showcase expertise in:
- SQL Development
- Data Architect
- Data Engineering  
- ETL Pipeline Developer  
- Data Modeling  
- Data Analytics  

---

## 🛠️ Important Links & Tools

Everything is for Free!

- **[Datasets](datasets/):** Access to the project dataset (csv files).
- **[SQL Server Express](https://www.microsoft.com/en-us/sql-server/sql-server-downloads):** Lightweight server for hosting your SQL database.
- **[SQL Server Management Studio (SSMS)](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16):** GUI for managing and interacting with databases.
- **[Git Repository](https://github.com/):** Set up a GitHub account and repository to manage, version, and collaborate on your code efficiently.
- **[DrawIO](https://www.drawio.com/):** Design data architecture, models, flows, and diagrams.
- **[Notion](https://www.notion.com/):** All-in-one tool for project management and organization.
- **[Notion Project Steps](https://app.notion.com/p/a211c70822e382c1b00101846dee47e7):** Access to All Project Phases and Tasks.

---

## 🚀 Project Requirements

### Building the Data Warehouse (Data Engineering)

#### Objective
Develop a modern data warehouse using SQL Server to consolidate sales data, enabling analytical reporting and informed decision-making.

#### Specifications
- **Data Sources**: Import data from two source systems (ERP and CRM) provided as CSV files.
- **Data Quality**: Cleanse and resolve data quality issues prior to analysis.
- **Integration**: Combine both sources into a single, user-friendly data model designed for analytical queries.
- **Scope**: Focus on the latest dataset only; historization of data is not required.
- **Documentation**: Provide clear documentation of the data model to support both business stakeholders and analytics teams.

---

### BI: Analytics & Reporting (Data Analysis)

#### Objective
Develop SQL-based analytics to deliver detailed insights into:
- **Customer Behavior**
- **Product Performance**
- **Sales Trends**

These insights empower stakeholders with key business metrics, enabling strategic decision-making.

---

## Quick Start

Anyone can run this project locally. Full details are in the [setup guide](docs/setup_guide.md). The short version:

1. **Install** SQL Server (Express is enough) and SQL Server Management Studio (SSMS). See the Important Links & Tools section above.
2. **Clone** this repository.
3. **Get the datasets** and place the CSV files in `datasets/source_crm/` and `datasets/source_erp/`.
4. **Set your local path** in `scripts/bronze/load_bronze.sql` by replacing `<YOUR_BASE_PATH>` with the folder that contains `source_crm` and `source_erp`. If you contribute changes back, do not commit your edited path (see the setup guide).
5. **Run the scripts in this order:**

   | Order | Script | What it does |
   |-------|--------|--------------|
   | 1 | `scripts/01_init_database.sql` | Creates the `DataWarehouse` database and the `bronze`, `silver` and `gold` schemas. **Drops the database if it already exists.** |
   | 2 | `scripts/bronze/02_ddl_bronze.sql` | Creates the 6 empty Bronze tables. |
   | 3 | `scripts/bronze/load_bronze.sql` | Creates the load procedure. |
   | 4 | `EXEC bronze.load_bronze;` | Loads the CSV files into the Bronze tables. |

---

## Documentation Map

| Document | What you will find |
|----------|--------------------|
| [docs/setup_guide.md](docs/setup_guide) | How to get the source files, set your local path, and run the scripts in the right order. Includes troubleshooting. |
| [docs/data_catalog.md](docs/data_catalog) | Every table and column in the Bronze and Gold layers, with data types and descriptions. |
| [docs/naming_conventions.md](docs/naming_conventions) | Rules for naming schemas, tables, columns, procedures and files. |
| `docs/data_architecture.drawio` | The project's architecture diagram. |
| `docs/data_flow.drawio` | The data flow diagram. |
| `docs/data_models.drawio` | The data models (star schema). |
| `docs/etl.drawio` | The different techniques and methods of ETL. |

---

## 📂 Repository Structure

```
data-warehouse-project/
│
├── datasets/                           # Raw datasets used for the project (ERP and CRM data)
│   ├── source_crm/                     #   cust_info.csv, prd_info.csv, sales_details.csv
│   └── source_erp/                     #   loc_a101.csv, cust_az12.csv, px_cat_g1v2.csv
│
├── docs/                               # Project documentation and architecture details
│   ├── setup_guide.md                  # How to set up and run the project
│   ├── etl.drawio                      # Draw.io file shows all different techniques and methods of ETL
│   ├── data_architecture.drawio        # Draw.io file shows the project's architecture
│   ├── data_catalog.md                 # Catalog of datasets, including field descriptions and metadata
│   ├── data_flow.drawio                # Draw.io file for the data flow diagram
│   ├── data_models.drawio              # Draw.io file for data models (star schema)
│   ├── naming_conventions.md           # Consistent naming guidelines for tables, columns, and files
│
├── scripts/                            # SQL scripts for ETL and transformations
│   ├── 01_init_database.sql            # Creates the database and the bronze, silver and gold schemas
│   ├── bronze/                         # Scripts for extracting and loading raw data
│   │   ├── 02_ddl_bronze.sql           #   Creates the Bronze tables
│   │   └── load_bronze.sql             #   Loads the CSV files into the Bronze tables
│   ├── silver/                         # Scripts for cleaning and transforming data
│   ├── gold/                           # Scripts for creating analytical models
│
├── tests/                              # Test scripts and quality files
│
├── README.md                           # Project overview and instructions
├── LICENSE                             # License information for the repository
└── .gitignore                          # Files and directories to be ignored by Git
```

---

## 🛡️ License

This project is licensed under the [MIT License](LICENSE). You are free to use, modify, and share this project with proper attribution.

---

## 🌟 About Me

Hi there! I'm **Fady Emad**, and this is my data warehouse and analytics project.

Let's stay in touch! Feel free to connect with me on LinkedIn:

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/fady-emad-analyst/)

---

## Acknowledgements

This project is based on the SQL Data Warehouse project by **Baraa Khatib Salkini (Data With Baraa)**: [datawithbaraa.com](https://www.datawithbaraa.com). The original project steps are available in his [Notion project guide](https://thankful-pangolin-2ca.notion.site/SQL-Data-Warehouse-Project-16ed041640ef80489667cfe2f380b269?pvs=4).
