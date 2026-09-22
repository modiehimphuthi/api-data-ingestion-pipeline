# API Data Ingestion Pipeline

A small end-to-end pipeline that pulls user data from a public REST API, cleans and reshapes it with pandas, and loads it into a SQL Server database — then verifies the load by querying it back.

## What it does

1. **Extract** — Calls the [JSONPlaceholder](https://jsonplaceholder.typicode.com/users) API and pulls a nested JSON response of user records.
2. **Transform**
   - Flattens the nested JSON (`address`, `company` sub-objects) into a tabular structure with `pandas.json_normalize`.
   - Selects only the relevant columns for downstream use (`id`, `name`, `username`, `email`, `city`, `postal_code`, `phone`, `website`, `company_name`).
   - Renames columns to clean, consistent, snake_case names.
3. **Load**
   - Connects to a local SQL Server instance via `SQLAlchemy` + `pyodbc` (Windows trusted authentication).
   - Writes the cleaned DataFrame to a SQL table with `df.to_sql()`.
4. **Validate**
   - Runs a `SELECT * FROM` query against the new table to confirm the load.
   - Reads the table back into a DataFrame with `pandas.read_sql()` as a final sanity check.

## Tech stack

- **Python** — `requests`, `pandas`, `sqlalchemy`, `pyodbc`
- **SQL Server** (local instance, ODBC Driver 17)
- **Jupyter Notebook** for development

## Why this project

This was built as a hands-on exercise in the core pattern behind most data engineering pipelines: **extract → transform → load → validate**. It specifically practices:

- Working with nested/semi-structured JSON from a REST API
- Flattening and reshaping data with pandas
- Writing to and reading from a relational database programmatically
- Confirming data integrity after a load, not just assuming it worked

## Possible next steps

- Parameterize the API endpoint and DB connection details (currently hardcoded)
- Add error handling for failed API calls or DB writes
- Add data quality checks (nulls, duplicates, type validation) before the load step
- Schedule the pipeline to run on a cadence (e.g. with a task scheduler or Airflow)

## Getting started

```bash
pip install requests pandas sqlalchemy pyodbc
```

Update the `server` and `database` variables in the notebook to point to your own SQL Server instance, then run the cells in order.
