# 🍃 MongoDB Quick Reference Guide

## 📌 Basic Commands

```bash
# Show databases
show dbs

# Switch / create database
use mydb

# Show collections
show collections

# Drop current database
db.dropDatabase()
📌 Collection Operations
bash
Copy code
# Create a collection
db.createCollection("users")

# Drop a collection
db.users.drop()
📌 CRUD Operations
bash
Copy code
# Insert one document
db.users.insertOne({ name: "John", age: 30, city: "Boston" })

# Insert many documents
db.users.insertMany([
  { name: "Alice", age: 25, city: "New York" },
  { name: "Bob", age: 28, city: "Chicago" }
])

# Find all documents
db.users.find()

# Find with condition
db.users.find({ city: "Boston" })

# Find with projection (only name, age)
db.users.find({ city: "Boston" }, { name: 1, age: 1, _id: 0 })

# Update one
db.users.updateOne(
  { name: "John" },
  { $set: { city: "New York" } }
)

# Update many
db.users.updateMany(
  { city: "Boston" },
  { $set: { city: "Cambridge" } }
)

# Delete one
db.users.deleteOne({ name: "Bob" })

# Delete many
db.users.deleteMany({ city: "New York" })
📌 Query Operators
bash
Copy code
# Comparison
db.users.find({ age: { $gt: 25 } })        # greater than
db.users.find({ age: { $gte: 25 } })       # greater or equal
db.users.find({ age: { $lt: 30 } })        # less than
db.users.find({ age: { $ne: 25 } })        # not equal

# Logical
db.users.find({ $or: [ { city: "Boston" }, { city: "Chicago" } ] })
db.users.find({ $and: [ { age: { $gt: 25 } }, { city: "Boston" } ] })

# In
db.users.find({ city: { $in: ["Boston", "Chicago"] } })
📌 Sorting & Limiting
bash
Copy code
# Sort by age ascending
db.users.find().sort({ age: 1 })

# Sort by age descending
db.users.find().sort({ age: -1 })

# Limit results
db.users.find().limit(5)

# Skip results
db.users.find().skip(5).limit(5)
📌 Aggregation
bash
Copy code
# Group by city and count users
db.users.aggregate([
  { $group: { _id: "$city", count: { $sum: 1 } } }
])

# Average age by city
db.users.aggregate([
  { $group: { _id: "$city", avgAge: { $avg: "$age" } } }
])

# Match and group
db.users.aggregate([
  { $match: { age: { $gte: 25 } } },
  { $group: { _id: "$city", total: { $sum: 1 } } }
])
📌 Indexing
bash
Copy code
# Create index on name
db.users.createIndex({ name: 1 })

# Create unique index
db.users.createIndex({ email: 1 }, { unique: true })

# List indexes
db.users.getIndexes()
📌 Useful Admin Commands
bash
Copy code
# Current database stats
db.stats()

# Collection stats
db.users.stats()

# Server status
db.serverStatus()
📚 References
MongoDB Manual

MongoDB Aggregation

MongoDB Shell Quick Reference
