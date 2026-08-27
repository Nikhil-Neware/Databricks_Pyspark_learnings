# PySpark Learning Series

A hands-on collection of three Jupyter notebooks covering fundamental to intermediate PySpark DataFrame operations. The notebooks use practical examples with CSV, JSON, and manually created DataFrames to demonstrate common data engineering and data transformation workflows.

## 📚 Repository Structure

```text
.
├── Pyspark_101.ipynb   # Data reading, schemas, DataFrame transformations
├── Pyspark_102.ipynb   # Functions, aggregations, null handling, joins
└── Pyspark_103.ipynb   # Window functions, data writing, tables, Spark SQL
```

## 🛠️ Technologies Used

- Python
- PySpark
- Spark SQL
- PySpark SQL Functions
- PySpark SQL Types
- Window Functions
- CSV and JSON data formats
- Parquet
- Delta tables

## 📖 Notebook Overview

### 1. `Pyspark_101.ipynb` — PySpark Fundamentals

This notebook introduces the core concepts required to work with data using PySpark DataFrames.

#### Topics Covered

**Data Reading**
- Reading CSV files using `spark.read.format("csv")`
- Using `inferSchema`
- Reading files with headers
- Reading JSON files
- Using the `multiLine` option

**Schema Management**
- Inspecting schemas using `printSchema()`
- Creating schemas using DDL strings
- Creating explicit schemas with `StructType`
- Defining columns using `StructField`
- Using Spark SQL data types such as `StringType`, `IntegerType`, and `DoubleType`

**Column Operations**
- Selecting columns using `select()`
- Using `col()`
- Renaming selected columns with `alias()`
- Renaming DataFrame columns with `withColumnRenamed()`
- Creating new columns with `withColumn()`
- Creating literal values using `lit()`
- Performing arithmetic operations between columns

**Filtering and Data Cleaning**
- Filtering records using `filter()`
- Combining conditions using `&`
- Filtering values using `isin()`
- Identifying null values using `isNull()`
- Finding distinct values using `distinct()`
- Removing columns using `drop()`
- Removing duplicate records using `dropDuplicates()`

**String Transformation and Type Casting**
- Replacing values using `regexp_replace()`
- Creating boolean columns from conditions
- Casting columns using `cast()`

**Sorting and Limiting**
- Sorting using `sort()`
- Ascending and descending ordering
- Sorting by multiple columns
- Limiting output using `limit()`

---

### 2. `Pyspark_102.ipynb` — Transformations and Data Analysis

This notebook builds on the fundamentals and focuses on commonly used PySpark functions for transforming and analyzing data.

#### Topics Covered

**DataFrame Creation**
- Creating DataFrames using `spark.createDataFrame()`
- Defining schemas using DDL syntax

**Union Operations**
- Combining DataFrames using `union()`
- Combining DataFrames by column name using `unionByName()`
- Understanding the importance of column order and schema compatibility

**String Functions**
- Converting text to lowercase using `lower()`
- Examples of other string transformations such as capitalization and uppercase conversion

**Date Functions**
- Creating date columns using `current_date()`
- Adding days using `date_add()`
- Subtracting days using negative offsets
- Calculating date differences using `date_diff()`
- Formatting dates using `date_format()`

**Null Handling**
- Removing null records using `dropna()`
- Removing records based on selected columns
- Replacing null values using `fillna()`

**Array and String Processing**
- Splitting strings into arrays using `split()`
- Accessing array elements by index
- Converting array elements into rows using `explode()`
- Checking values in arrays using `array_contains()`

**Aggregations**
- Grouping data using `groupBy()`
- Calculating `sum()` and `avg()`
- Grouping by multiple columns
- Renaming aggregated columns using `alias()`

**Collection and Pivot Operations**
- Aggregating values into arrays using `collect_list()`
- Transforming rows into columns using `pivot()`

**Conditional Logic**
- Implementing SQL-like `CASE WHEN` logic using `when()` and `otherwise()`
- Creating multi-condition classification columns

**Joins**
- Inner join
- Left join
- Right join
- Anti join

---

### 3. `Pyspark_103.ipynb` — Window Functions, Data Writing and Spark SQL

This notebook focuses on intermediate PySpark concepts used in analytics and data engineering pipelines.

#### Topics Covered

**Window Functions**
- Importing and defining windows using `Window`
- Partitioning data using `partitionBy()`
- Ordering data using `orderBy()`
- Generating sequential row numbers using `row_number()`
- Ranking records using `rank()`
- Generating dense ranks using `dense_rank()`
- Calculating cumulative sums using `sum().over()`
- Defining window frames with `rowsBetween()`
- Using `Window.unboundedPreceding`
- Using `Window.currentRow`

**User-Defined Functions**
- Creating a Python function for custom transformations
- Applying custom logic to DataFrame columns

**Writing Data**
- Writing DataFrames in CSV format
- Converting Spark DataFrames to Pandas using `toPandas()`
- Writing data using Pandas `to_csv()`
- Specifying output paths
- Appending data to existing output locations

**Write Modes**
- `append` — adds new data to existing data
- `overwrite` — replaces existing data
- `error` — raises an error when data already exists
- `ignore` — skips the write when data already exists

**Parquet and Tables**
- Writing DataFrames in Parquet format
- Writing managed tables using `saveAsTable()`
- Working with Delta table format

**Spark SQL**
- Creating temporary views using `createTempView()`
- Querying views using SQL
- Running SQL from PySpark using `spark.sql()`

## 🚀 Getting Started

### Prerequisites

You should have access to a Spark environment such as:

- Databricks
- Apache Spark
- A local PySpark installation

Install PySpark locally:

```bash
pip install pyspark
```

### Required Imports

Most notebook examples use the following imports:

```python
from pyspark.sql.functions import *
from pyspark.sql.types import *
```

Window function examples additionally use:

```python
from pyspark.sql.window import Window
```

### Example: Reading a CSV File

```python
df = (
    spark.read
    .format("csv")
    .option("inferSchema", True)
    .option("header", True)
    .load("path/to/file.csv")
)
```

### Example: Filtering Data

```python
df.filter(
    (col("Item_Type") == "Soft Drinks") &
    (col("Item_Weight") < 10)
).display()
```

### Example: Aggregation

```python
df.groupBy("Item_Type").agg(
    sum("Item_MRP").alias("Total_Amount"),
    avg("Item_MRP").alias("Average_Amount")
).display()
```

### Example: Window Function

```python
df.withColumn(
    "row_number",
    row_number().over(
        Window.partitionBy("Item_Identifier")
              .orderBy("Item_Identifier")
    )
).display()
```

## 🎯 Learning Path

The notebooks are organized as a progressive PySpark learning series:

1. **Pyspark_101** → Learn how to read data, define schemas, and perform basic DataFrame transformations.
2. **Pyspark_102** → Learn transformations, built-in functions, null handling, aggregations, pivots, and joins.
3. **Pyspark_103** → Learn window functions, writing data, working with tables, and executing Spark SQL.

## 📊 Dataset Context

Several examples use a **BigMart Sales** CSV dataset containing item and outlet-related columns such as:

- Item identifier
- Item weight
- Item fat content
- Item visibility
- Item type
- Item MRP
- Outlet identifier
- Outlet establishment year
- Outlet size
- Outlet location type
- Outlet type
- Item outlet sales

A JSON dataset is also used to demonstrate JSON file ingestion.

## 💡 Key Concepts Demonstrated

This repository provides practical examples of the following PySpark workflow:

```text
Read Data
   ↓
Define / Inspect Schema
   ↓
Select and Transform Columns
   ↓
Filter and Clean Data
   ↓
Handle Nulls and Duplicates
   ↓
Aggregate and Analyze
   ↓
Join and Reshape Data
   ↓
Apply Window Functions
   ↓
Write Data / Create Tables
   ↓
Query with Spark SQL
```

## 📌 Notes

- The notebooks contain hands-on examples intended for learning and experimentation.
- Several examples use Databricks-specific paths and display functionality.
- Update dataset paths according to your local Spark or Databricks environment.
- Schema inference is convenient for exploration, while explicitly defining schemas is generally useful when stronger control over data types is required.

## 👨‍💻 Author

A hands-on PySpark learning repository covering DataFrame operations, transformations, analytics functions, window functions, data writing, and Spark SQL.
