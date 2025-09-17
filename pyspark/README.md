
# ⚡ PySpark Quick Reference

## 📌 Setup & Initialization
```python
from pyspark.sql import SparkSession

# Start Spark session
spark = SparkSession.builder \
    .appName("MyApp") \
    .getOrCreate()

# Check Spark version
spark.version
````

---

## 📂 DataFrames

```python
# Create DataFrame from list
data = [("Alice", 25), ("Bob", 30)]
df = spark.createDataFrame(data, ["name", "age"])
df.show()

# Read CSV/Parquet
df = spark.read.csv("file.csv", header=True, inferSchema=True)
df = spark.read.parquet("file.parquet")

# Write DataFrame
df.write.mode("overwrite").parquet("output/")
```

---

## 🔍 Basic Operations

```python
df.printSchema()                     # View schema
df.select("name").show()             # Select column
df.filter(df.age > 25).show()        # Filter rows

from pyspark.sql.functions import col
df = df.withColumn("age_plus_one", col("age") + 1)   # Add column
df = df.drop("age_plus_one")                         # Drop column
```

---

## 📊 Aggregations

```python
# GroupBy
df.groupBy("age").count().show()

# Aggregate functions
from pyspark.sql.functions import avg, max, min, count
df.agg(avg("age"), max("age"), min("age"), count("age")).show()
```

---

## 🔗 Joins

```python
df1.join(df2, df1.id == df2.id, "inner").show()   # Inner Join
df1.join(df2, df1.id == df2.id, "left").show()    # Left Join
df1.join(df2, df1.id == df2.id, "right").show()   # Right Join
df1.join(df2, df1.id == df2.id, "outer").show()   # Full Outer Join
```

---

## 🧹 Useful Actions

* `df.show()` → Display rows
* `df.count()` → Count rows
* `df.collect()` → Return all rows as list (⚠️ large data risk!)
* `df.take(5)` → Return first 5 rows

---

## 📚 References

* [PySpark Documentation](https://spark.apache.org/docs/latest/api/python/)
* [Databricks PySpark Guide](https://docs.databricks.com/)



# ⚡ PySpark Advanced Concepts

## ⚙️ Partitioning & Performance
```python
# Repartition (shuffle-based, more expensive)
df = df.repartition(10)

# Coalesce (reduce partitions, cheaper)
df = df.coalesce(2)

# Inspect partitions
df.rdd.getNumPartitions()
````

👉 Use repartitioning to optimize joins and shuffles.

---

## 🔄 Caching & Persistence

```python
df.cache()         # Store in memory
df.persist()       # Store in memory/disk (default: MEMORY_AND_DISK)
df.unpersist()     # Remove from cache
```

👉 Useful when reusing DataFrames across multiple actions.

---

## 🧮 Window Functions

```python
from pyspark.sql.window import Window
from pyspark.sql.functions import row_number, rank, dense_rank

w = Window.partitionBy("department").orderBy("salary")

df.withColumn("row_num", row_number().over(w)).show()
df.withColumn("rank", rank().over(w)).show()
```

👉 Great for ranking, running totals, moving averages.

---

## 🏎️ Broadcast Joins

```python
from pyspark.sql.functions import broadcast

df_large.join(broadcast(df_small), "id").show()
```

👉 Avoids expensive shuffles when one dataset is small.

---

## 🐍 User-Defined Functions (UDFs)

```python
from pyspark.sql.functions import udf
from pyspark.sql.types import IntegerType

def add_one(x): 
    return x + 1

add_one_udf = udf(add_one, IntegerType())
df.withColumn("age_plus_one", add_one_udf(df.age)).show()
```

⚠️ UDFs can be slow — prefer **pandas UDFs** or built-in functions.

---

## 🐼 Pandas UDFs (Vectorized UDFs)

```python
from pyspark.sql.functions import pandas_udf

@pandas_udf("double")
def multiply_by_two(v):
    return v * 2

df.withColumn("double_salary", multiply_by_two(df.salary)).show()
```

👉 Much faster than normal UDFs, since they use Apache Arrow.

---

## 📤 Writing Optimized Data

```python
# Partitioned write
df.write.partitionBy("year", "month").parquet("output/")

# Bucketed write (requires Hive support)
df.write.bucketBy(8, "id").sortBy("id").saveAsTable("bucketed_table")
```

---

## 🔗 Joins Optimization

* Use **broadcast joins** for small lookup tables.
* Ensure join keys are distributed evenly (avoid skew).
* Repartition on join keys before large joins:

  ```python
  df1 = df1.repartition("id")
  df2 = df2.repartition("id")
  df1.join(df2, "id")
  ```

---

## 🛠️ Common Performance Tips

* Prefer **DataFrame API** over RDD API (optimized via Catalyst).
* Minimize **shuffles** (joins, groupBy, distinct).
* Use `explain()` to view query plan:

  ```python
  df.groupBy("department").count().explain(True)
  ```
* Push filters early (`filter` before `join`).
* Use **Parquet/ORC** instead of CSV for storage (columnar, compressed).

---

## 📚 References

* [Optimizing Spark](https://spark.apache.org/docs/latest/sql-performance-tuning.html)
* [Databricks Performance Guide](https://docs.databricks.com/optimizations/)





