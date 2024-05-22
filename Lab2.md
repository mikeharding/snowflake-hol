# Cortex ML-Based functions

The original content for this lab is based on a Snowflake Quickstart that can be foud [here](https://quickstarts.snowflake.com/guide/ml_forecasting_ad/index.html?index=..%2F..index#1)

## Step 1
Step 1 has been performed by your administrator.

## Step 2: Creating Objects, Load Data, & Set Up Tables
- Create a new worksheet by clicking on the ‘Worksheets' tab on the left hand side.
- Paste and run the following SQL commands in the worksheet to create the required Snowflake objects, ingest sales data from S3, and update your Search Path to make it easier to work with the ML Functions.

```SQL
-- Use appropriate resources: 
USE DATABASE HOLXXX; -- where XXX is your user number.
USE SCHEMA ml_functions;

USE WAREHOUSE whXXX; -- where XXX is your user number.


-- Create a csv file format to be used to ingest from the stage: 
CREATE OR REPLACE FILE FORMAT csv_ff
    type = 'csv'
    SKIP_HEADER = 1,
    COMPRESSION = AUTO;

-- Create an external stage pointing to AWS S3 for loading sales data: 
CREATE OR REPLACE STAGE s3load 
    COMMENT = 'Quickstart S3 Stage Connection'
    url = 's3://sfquickstarts/frostbyte_tastybytes/mlpf_quickstart/'
    file_format = csv_ff;

-- Define Tasty Byte Sales table
CREATE OR REPLACE TABLE tasty_byte_sales(
  	DATE DATE,
	PRIMARY_CITY VARCHAR(16777216),
	MENU_ITEM_NAME VARCHAR(16777216),
	TOTAL_SOLD NUMBER(17,0)
);

-- Ingest data from S3 into our table
COPY INTO tasty_byte_sales 
    FROM @s3load/ml_functions_quickstart.csv;

-- View a sample of the ingested data: 
SELECT * FROM tasty_byte_sales LIMIT 100;
```
At this point, we have all the data we need to start building models. We will get started with building our first forecasting model.
[Next](Lab3.md)
