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

# ⚡ Elasticsearch Advanced Query Guide

## 📌 Full-Text Search
```bash
# Match with relevance scoring
GET /my_index/_search
{
  "query": {
    "match": {
      "description": "machine learning"
    }
  }
}

# Multi-match across multiple fields
GET /my_index/_search
{
  "query": {
    "multi_match": {
      "query": "AI engineer",
      "fields": ["title", "description"]
    }
  }
}
👉 Great for unstructured text search.

📌 Bool Queries (AND, OR, NOT)
bash
Copy code
GET /my_index/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "city": "Boston" } }
      ],
      "must_not": [
        { "term": { "age": 20 } }
      ],
      "should": [
        { "match": { "name": "John" } }
      ]
    }
  }
}
👉 Combine multiple conditions flexibly.

📌 Nested Documents
bash
Copy code
# Example: Search inside nested "skills" array
GET /my_index/_search
{
  "query": {
    "nested": {
      "path": "skills",
      "query": {
        "bool": {
          "must": [
            { "match": { "skills.name": "Python" } }
          ]
        }
      }
    }
  }
}
👉 Needed when querying arrays of objects.

📌 Highlighting (Keyword Snippets)
bash
Copy code
GET /my_index/_search
{
  "query": {
    "match": {
      "description": "cloud"
    }
  },
  "highlight": {
    "fields": {
      "description": {}
    }
  }
}
👉 Useful for search engines and UIs.

📌 Advanced Aggregations
bash
Copy code
# Date histogram (group by month)
GET /my_index/_search
{
  "size": 0,
  "aggs": {
    "sales_per_month": {
      "date_histogram": {
        "field": "date",
        "calendar_interval": "month"
      }
    }
  }
}

# Nested aggregations
GET /my_index/_search
{
  "size": 0,
  "aggs": {
    "by_city": {
      "terms": { "field": "city.keyword" },
      "aggs": {
        "avg_age": {
          "avg": { "field": "age" }
        }
      }
    }
  }
}
📌 Analyzers & Text Analysis
bash
Copy code
# Analyze text with a standard analyzer
POST /_analyze
{
  "analyzer": "standard",
  "text": "Elasticsearch makes search easy!"
}

# Custom analyzer (example in settings)
PUT /my_index
{
  "settings": {
    "analysis": {
      "analyzer": {
        "custom_english": {
          "type": "standard",
          "stopwords": "_english_"
        }
      }
    }
  }
}
👉 Helps control tokenization, stemming, and stop words.

📌 Fuzzy & Wildcard Queries
bash
Copy code
# Fuzzy search (typo tolerant)
GET /my_index/_search
{
  "query": {
    "fuzzy": {
      "name": {
        "value": "jon",
        "fuzziness": 1
      }
    }
  }
}

# Wildcard search
GET /my_index/_search
{
  "query": {
    "wildcard": {
      "city": "Bost*"
    }
  }
}
📌 Scroll & Search After (Large Results)
bash
Copy code
# Scroll API (deprecated in favor of search_after)
GET /my_index/_search?scroll=1m
{
  "size": 100,
  "query": { "match_all": {} }
}

# Search After (for deep pagination)
GET /my_index/_search
{
  "size": 100,
  "query": { "match_all": {} },
  "sort": [{ "age": "asc" }, { "_id": "asc" }],
  "search_after": [30, "doc_id_123"]
}
📌 Useful Tools
explain — shows scoring details:

bash
Copy code
GET /my_index/_explain/1
{
  "query": { "match": { "name": "John" } }
}
_validate/query — test queries without running them:

bash
Copy code
GET /my_index/_validate/query?explain=true
{
  "query": { "match": { "city": "Boston" } }
}
📚 References
Elasticsearch Query DSL : https://www.elastic.co/docs/explore-analyze/query-filter/languages/querydsl

Aggregations Guide : https://www.elastic.co/docs/explore-analyze/query-filter/aggregations