
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




