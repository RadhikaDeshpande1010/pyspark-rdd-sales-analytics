# pyspark-rdd-sales-analytics

> Distributed sales data analysis using Apache PySpark RDD transformations — `map()`, `filter()`, and `reduceByKey()` — on a real-world retail dataset.

---

## Overview

This project demonstrates core **PySpark RDD programming** concepts applied to a retail sales dataset spanning 100 orders across multiple cities, product categories, and customers. Each analytical task is implemented as a functional transformation pipeline without relying on Spark DataFrames or SQL, reinforcing a low-level understanding of distributed computation.

The notebook covers 15 analytical questions including revenue aggregation, category filtering, city-level sales breakdowns, and customer-level spend analysis.

---

## Repository Structure

```
pyspark-rdd-sales-analytics/
│
├── notebook/
│   └── pyspark_rdd_sales_data_analysis.ipynb   # Main analysis notebook
├── sample_data/
│   └── sales_data.txt                      # Raw sales dataset (CSV format)
└── README.md
```

---

## Dataset Schema

The dataset `sales_data.txt` is a comma-separated file with the following fields:

| Index | Field         | Description                        | Example       |
|-------|---------------|------------------------------------|---------------|
| 0     | `order_id`    | Unique order identifier            | `1`           |
| 1     | `customer_id` | Customer identifier                | `C112`        |
| 2     | `product`     | Product name                       | `Printer`     |
| 3     | `category`    | Product category                   | `Electronics` |
| 4     | `city`        | City where the order was placed    | `Chennai`     |
| 5     | `quantity`    | Number of units ordered            | `5`           |
| 6     | `unit_price`  | Price per unit (INR)               | `12000`       |

**Categories:** `Electronics`, `Furniture`  
**Cities covered:** Chennai, Delhi, Patna, Hyderabad, Mumbai, Bangalore, Pune, Kolkata

---

## Analysis Covered

| Q   | Operation                          | RDD Methods Used                    |
|-----|------------------------------------|-------------------------------------|
| Q1  | Parse raw records into lists       | `map()`                             |
| Q2  | Total price per order              | `map()`                             |
| Q3  | Filter Electronics orders          | `map()`, `filter()`                 |
| Q4  | Orders with quantity > 2           | `filter()`                          |
| Q5  | Total sales amount per product     | `map()`, `reduceByKey()`            |
| Q6  | Total quantity sold per product    | `map()`, `reduceByKey()`            |
| Q7  | Orders with unit price > 20,000    | `map()`, `filter()`                 |
| Q8  | Total revenue per city             | `map()`, `reduceByKey()`            |
| Q9  | Furniture orders with quantity > 1 | `map()`, `filter()`                 |
| Q10 | Order count per category           | `map()`, `reduceByKey()`            |
| Q11 | Total revenue per customer         | `map()`, `reduceByKey()`            |
| Q12 | Distinct products sold in Delhi    | `map()`, `filter()`, `distinct()`   |
| Q13 | Total quantity sold per city       | `map()`, `reduceByKey()`            |
| Q14 | Orders with total sales > 50,000   | `map()`, `filter()`                 |
| Q15 | Total revenue per category         | `map()`, `reduceByKey()`            |

---

## Getting Started

### Prerequisites

- Python 3.8+
- Java 8 or Java 11 (required by Spark)
- PySpark

### Installation

```bash
# Clone the repository
git clone https://github.com/<your-username>/pyspark-rdd-sales-analytics.git
cd pyspark-rdd-sales-analytics

# Install PySpark
pip install pyspark

# Launch Jupyter Notebook
jupyter notebook pyspark_rdd_sales_data_analysis.ipynb
```

### Running on Google Colab

1. Upload `pyspark_rdd_sales_data_analysis.ipynb` to [Google Colab](https://colab.research.google.com)
2. Install PySpark in the first cell:
   ```python
   !pip install pyspark
   ```
3. Upload `sales_data.txt` to `/content/sample_data/sales_data.txt`
4. Run all cells

---

## Key Concepts Demonstrated

**RDD Creation**
```python
raw_sales_rdd = spark_context.textFile("sales_data.txt")
parsed_sales_rdd = raw_sales_rdd.map(lambda raw_line: raw_line.strip().split(","))
```

**Transformation Pipeline**
```python
# Total revenue per customer
total_revenue_per_customer_rdd = parsed_sales_rdd \
    .map(lambda record: (record[1], int(record[5]) * int(record[6]))) \
    .reduceByKey(lambda revenue_a, revenue_b: revenue_a + revenue_b)
```

**Chained Filtering**
```python
# Furniture orders with quantity > 1
furniture_bulk_orders_rdd = parsed_sales_rdd \
    .map(lambda record: (record[0], record[3], int(record[5]))) \
    .filter(lambda order_cat_qty: order_cat_qty[1] == 'Furniture' and order_cat_qty[2] > 1) \
    .map(lambda order_cat_qty: (order_cat_qty[0], order_cat_qty[2]))
```

---

## Technologies Used

![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=flat&logo=python&logoColor=white)
![PySpark](https://img.shields.io/badge/PySpark-3.x-E25A1C?style=flat&logo=apache-spark&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=flat&logo=jupyter&logoColor=white)
![Google Colab](https://img.shields.io/badge/Google-Colab-F9AB00?style=flat&logo=googlecolab&logoColor=white)

---

*Built by **Radhika Deshpande** · Distributed Sales Data Analysis Using PySpark RDD Transformations*
