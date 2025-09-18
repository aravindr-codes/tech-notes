# 🔍 Elasticsearch Basic Query Guide

## 📌 Index Operations
```bash
# Create an index
PUT /my_index

# Delete an index
DELETE /my_index

# List all indices
GET /_cat/indices?v
📌 Document Operations
bash
Copy code
# Insert a document
POST /my_index/_doc/1
{
  "name": "John",
  "age": 30,
  "city": "Boston"
}

# Get a document by ID
GET /my_index/_doc/1

# Update a document
POST /my_index/_update/1
{
  "doc": {
    "city": "New York"
  }
}

# Delete a document
DELETE /my_index/_doc/1
📌 Basic Search
bash
Copy code
# Match all documents
GET /my_index/_search
{
  "query": {
    "match_all": {}
  }
}

# Match by field
GET /my_index/_search
{
  "query": {
    "match": {
      "city": "Boston"
    }
  }
}
📌 Filtering
bash
Copy code
# Term filter (exact match)
GET /my_index/_search
{
  "query": {
    "term": {
      "age": 30
    }
  }
}

# Range filter
GET /my_index/_search
{
  "query": {
    "range": {
      "age": {
        "gte": 25,
        "lte": 40
      }
    }
  }
}
📌 Sorting & Pagination
bash
Copy code
# Sort by age descending
GET /my_index/_search
{
  "query": { "match_all": {} },
  "sort": [
    { "age": { "order": "desc" } }
  ]
}

# Pagination (from, size)
GET /my_index/_search
{
  "from": 0,
  "size": 5,
  "query": { "match_all": {} }
}
📌 Aggregations
bash
Copy code
# Count documents by city
GET /my_index/_search
{
  "size": 0,
  "aggs": {
    "city_count": {
      "terms": { "field": "city.keyword" }
    }
  }
}

# Average age
GET /my_index/_search
{
  "size": 0,
  "aggs": {
    "avg_age": { "avg": { "field": "age" } }
  }
}
📌 Useful Commands
bash
Copy code
# Check cluster health
GET /_cluster/health

# Get index mapping
GET /my_index/_mapping

# Get index settings
GET /my_index/_settings