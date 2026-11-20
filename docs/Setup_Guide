# Setup Guide

How to get the project running on your own machine. Follow the steps in order.

## Table of Contents

1. [Requirements](#requirements)
2. [Step 1: Get the source files](#step-1-get-the-source-files)
3. [Step 2: Make the files readable by SQL Server](#step-2-make-the-files-readable-by-sql-server)
4. [Step 3: Set your local path](#step-3-set-your-local-path)
5. [Step 4: Run the scripts](#step-4-run-the-scripts)
6. [Step 5: Verify](#step-5-verify)
7. [Troubleshooting](#troubleshooting)

---

## Requirements

- Microsoft SQL Server (Express or Developer edition is enough).
- SQL Server Management Studio (SSMS) or Azure Data Studio.
- A login that can create and drop databases (for example `sysadmin` or `dbcreator`).
- The 6 source CSV files (Step 1).

> **Warning:** `scripts/01_init_database.sql` drops the entire `DataWarehouse` database if it exists. Run it only on your own development server, never on a shared one.

---

## Step 1: Get the source files

The project uses 6 CSV files from two source systems:

| Source system | Folder | Files |
|---------------|--------|-------|
| CRM (Customer Relationship Management) | `source_crm/` | `cust_info.csv`, `prd_info.csv`, `sales_details.csv` |
| ERP (Enterprise Resource Planning) | `source_erp/` | `loc_a101.csv`, `cust_az12.csv`, `px_cat_g1v2.csv` |

Put them in the repository's `datasets/` folder (or any folder you like), keeping this layout:

```
datasets/
├── source_crm/
│   ├── cust_info.csv
│   ├── prd_info.csv
│   └── sales_details.csv
└── source_erp/
    ├── loc_a101.csv
    ├── cust_az12.csv
    └── px_cat_g1v2.csv
```

Do not rename the files or folders. The load script expects these exact names.

---

## Step 2: Make the files readable by SQL Server

`BULK INSERT` runs **on the SQL Server machine**, not in your query tool. The path must be valid from the server's point of view.

- **SQL Server installed on your own PC:** any local folder works.
- **SQL Server in Docker, a VM or a remote server:** copy the files onto that machine or into the container first.
- The SQL Server service account needs **read permission** on the folder.

---

## Step 3: Set your local path

`scripts/bronze/load_bronze.sql` contains the placeholder `<YOUR_BASE_PATH>` in every file path, for example:

```sql
FROM '<YOUR_BASE_PATH>\source_crm\cust_info.csv'
```

Replace `<YOUR_BASE_PATH>` with the folder that **contains** `source_crm` and `source_erp`. Use Find & Replace (Ctrl + H).

| | Value |
|---|-------|
| Replace | `<YOUR_BASE_PATH>` |
| With | `C:\my_project\datasets` |
| Result | `C:\my_project\datasets\source_crm\cust_info.csv` |

Rules: keep the backslashes, do not add a trailing backslash to the base path, and keep the quotes.

### Do not commit your path

Everyone who clones the project has a different path, so committing your edited script would break it for anyone else. If you push changes to the repository, use one of these two safe options:

1. **Work on a local copy.** Save your edited version as `load_bronze.local.sql` and add this line to `.gitignore`:

   ```
   *.local.sql
   ```

2. **Or** edit the original, run it, and discard the change before committing (`git restore scripts/bronze/load_bronze.sql`).

---

## Step 4: Run the scripts

Run them in this order. Each script depends on the one before it.

| Order | Script | What it does |
|-------|--------|--------------|
| 1 | `scripts/01_init_database.sql` | Drops and recreates the `DataWarehouse` database, then creates the `bronze`, `silver` and `gold` schemas. |
| 2 | `scripts/bronze/02_ddl_bronze.sql` | Drops and recreates the 6 empty Bronze tables. |
| 3 | `scripts/bronze/load_bronze.sql` | Creates (or updates) the `bronze.load_bronze` stored procedure. |
| 4 | `EXEC bronze.load_bronze;` | Runs the procedure: truncates each Bronze table and bulk-loads its CSV file. It prints progress and load times. |

Make sure the `DataWarehouse` database is selected (`USE DataWarehouse;`) before running scripts 2 to 4.

---

## Step 5: Verify

Check that the database and schemas exist:

```sql
SELECT name FROM sys.databases WHERE name = 'DataWarehouse';

SELECT name FROM DataWarehouse.sys.schemas
WHERE name IN ('bronze', 'silver', 'gold');
```

Check that the 6 Bronze tables exist:

```sql
SELECT TABLE_SCHEMA, TABLE_NAME
FROM INFORMATION_SCHEMA.TABLES
WHERE TABLE_SCHEMA = 'bronze';
```

Check that data was loaded:

```sql
SELECT COUNT(*) FROM bronze.crm_cust_info;
```

---

## Troubleshooting

| Error | Likely cause | Fix |
|-------|--------------|-----|
| `Cannot bulk load. The file does not exist or you don't have file access rights` | Wrong path, file is not on the SQL Server machine, or no read permission. | Re-check [Step 2](#step-2-make-the-files-readable-by-sql-server) and [Step 3](#step-3-set-your-local-path). |
| `Operating system error code 5 (Access is denied)` | The SQL Server service account cannot read the folder. | Move the files to a simple folder such as `C:\sql\...` and grant read permission. |
| `Invalid object name 'bronze.xxx'` | The tables or schema were not created. | Run scripts 1 and 2 first, with `DataWarehouse` selected. |
| `Invalid schema name 'bronze'` | Script 1 was not run, or the wrong database is active. | Run `01_init_database.sql`, then `USE DataWarehouse;`. |
| `Bulk load data conversion error` | The CSV columns do not match the table columns (count or order), or the file was edited or saved with a different delimiter. | Compare the file with the table in [data_catalog.md](data_catalog.md). Re-download the original file. |
| `CREATE DATABASE permission denied in database 'master'` | Your login cannot create databases. | Use a login with `dbcreator` or `sysadmin`. |
| `Database 'DataWarehouse' is in use` | Another session is connected. | Run script 1 from the `master` database and close other connections. |
