# Home Sales Analysis with PySpark

## Overview

This project uses Apache Spark (PySpark) and SparkSQL to analyze a home sales dataset sourced from AWS S3. The notebook explores average home prices across various dimensions — including number of bedrooms, bathrooms, floors, and square footage — and benchmarks query performance with and without caching, and using Parquet partitioning.

---

## Dataset

- **Source:** AWS S3 — `home_sales_revised.csv` (loaded via SparkFiles)
- **Key fields:** `id`, `date`, `date_built`, `price`, `bedrooms`, `bathrooms`, `sqft_living`, `sqft_lot`, `floors`, `waterfront`, `view`
- **Schema modified:** `price` cast to `FloatType`; `sqft_living`, `sqft_lot`, `bedrooms`, `bathrooms`, `floors` cast to `IntegerType`; `date` and `date_built` parsed to `DateType`

---

## Analysis

The notebook addresses the following analytical questions using SparkSQL:

1. **Average price of 4-bedroom homes** — grouped by year of sale
2. **Average price of 3-bed / 3-bath homes** — grouped by year built
3. **Average price of 3-bed / 3-bath / 2-floor homes ≥ 2,000 sq ft** — grouped by year built
4. **Average price by view rating** — filtered to homes priced ≥ $350,000; runtime benchmarked

---

## Performance Benchmarking

The notebook benchmarks three query execution modes for question 4:

- **Uncached DataFrame** — baseline query on the Spark temporary view
- **Cached table** — same query on `CACHE TABLE home_sales`; faster due to in-memory storage
- **Parquet with partitioning** — data partitioned by `date_built` and written to Parquet format, then queried via a new temporary view

The cached query ran faster than the uncached version. The Parquet-partitioned query offers additional scalability for larger datasets.

---

## Tools and Technologies

- Apache Spark (PySpark) via `findspark`
- SparkSQL for all analytical queries
- AWS S3 for data loading
- Python standard libraries: `time` for runtime benchmarking

---

## File Structure

```
HomeSales/
├── Home_Sales.ipynb    # Main analysis notebook
└── README.md
```

---

## How to Run

1. Ensure Apache Spark and `findspark` are installed and configured.
2. Open `Home_Sales.ipynb` in Jupyter.
3. Run all cells sequentially. The dataset is fetched automatically from AWS S3.

> **Note:** Parquet output (`home_sales_delayed/`) is written to the working directory during execution.
