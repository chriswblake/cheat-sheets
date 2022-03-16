
### Connect
```bash
# Shell
mongo "mongodb+srv://unique-db-id.mongodb.net/myFirstDatabase" --username <user>

# Connection String
mongodb+srv://user:pass@unique-db-id.mongodb.net/myFirstDatabase
```




# MongoDB - Shell commands

### Find and Open DB
```bash
# List databases
show dbs
# Open DB
use <db name>
# List collections
show collections
```

### Query
```bash
# Search docs
db.<collection name>.find({"field1":"value1", "field2":"value2"}).pretty()

# Get just one doc
db.<collection name>.findOne({})

# Sub documents
db.<collection_name>.find({"rootdoc_field.subdoc_field":"search_value"})
db.companies.find({ "relationships.0.person.last_name": "blake" })

# Regex
db.companies.find({ "relationships.0.person.first_name": "Mark", "relationships.0.title": { "$regex": "CEO" } })
> More about [Regex](https://docs.mongodb.com/manual/reference/operator/query/regex/)
```

### Query + Projection
```bash
# Show only some fields in results (1-show, 0-hide)
db.<collection name>.find(
  {"first_name":"John", "family_name":"smith"},
  {"address":1, "age":1}
).pretty()

# Arrays - Show only values of an array
db.grades.find(
  { "class_id": 431 },
  { "scores": { "$elemMatch": { "score": { "$gt": 85 } } }
}).pretty()

# Arrays - Show documents wher element in array meets criteria
db.grades.find({ "scores": { "$elemMatch": { "type": "extra credit" } } 
}).pretty() 

# Sort
db.zips.find().sort({ "pop": 1 })

# Limit
db.zips.find().sort({ "pop": 1, "city":-1 }).limit(10)          
```

### Aggregation
```bash
# Step 1: Find matching documents
# Step 2: Only return some of the fields
db.listingsAndReviews.aggregate([
                                  { "$match": { "field1": "value1" } },
                                  { "$project": { "price": 1, "address": 1, "_id": 0 } }
                                ])

# Grouping content
db.listingsAndReviews.aggregate([ { "$project": { "address": 1, "_id": 0 }},
                                  { "$group": { "_id": "$address.country" }}])

# Count in each group
db.listingsAndReviews.aggregate([
                                  { "$project": { "address": 1, "_id": 0 }},
                                  { "$group": { "_id": "$address.country",
                                                "count": { "$sum": 1 }
                                              }
                                  }
                                ])

# Avg/Min/Max
db.listingsAndReviews.aggregate([
                                  { "$project": { "address": 1, "weekly_price":1, "_id": 0 }},
                                  { "$group": { "_id": "$address.country",
                                                "avg_weekly_price": { "$avg": "$weekly_price" },
                                                "min_weekly_price": { "$min": "$weekly_price" },
                                                "max_weekly_price": { "$max": "$weekly_price" }
                                              }
                                  }
                                ])
```

### Insert
```bash
# Insert One Doc
db.inspections.insert({
      "field1" : 9278806,
      "field2" : "ATLIXCO DELI GROCERY INC.",
      "subdoc1" : {
              "field1" : "RIDGEWOOD",
              "field2" : 11385,
         }
  })

# Insert Multiple Docs
db.<collection>.insert([ { "test": 1 }, { "test": 2 }, { "test": 3 } ])

# Insert Multiple Docs, ignoring order in array (doesn't halt if error for a doc)
db.<collection>.insert([{ "_id": 1, "test": 1 }, { "_id": 1, "test": 2 }, { "_id": 3, "test": 3 }], { "ordered": false })
```

### Modify / Update
[MQL Update Operators](https://docs.mongodb.com/manual/reference/operator/update/#id1)

```bash

# Set new value for field
db.<collection>.updateOne({ "search_field": "search_value" }, { "$set": { "field1": "value1" } })
db.<collection>.updateMany({ "search_field": "search_value" }, { "$set": { "field1": "value1" } })

# Numeric increment
db.<collection>.updateOne({ "search_field": "search_value" }, { "$inc": { "numeric_field": 10 } })
db.<collection>.updateMany({ "search_field": "search_value" }, { "$inc": { "numeric_field": 10 } })

# Add element to array
db.<collection>.updateOne({ "search_field": "search_value"}, { "$push": { "my_array": "array_value" }})
```

### Delete
```bash

# Field
db.<collection>.updateMany({ "search_field": "search_value" }, { "$unset": {"field1": ""} })

# Document(s)
db.inspections.deleteOne({ "search_field": "search_value" })
db.inspections.deleteMany({ "search_field": 1 })

# Collection
db.<collection name>.drop()
```

### Summarize
```bash
db.<collection>.find({"field": "value"}).count()
```

### Export Data
```bash
# Leave in binary (BSON)
mongodump --uri "<Atlas Cluster URI>"
ls dump/

# As plain text (JSON)
mongoexport --uri "Atlas Cluster URI>" --collection=<collection name> --out=<filename>.json
less <filename>.json
```

### Import Data
```bash
# As binary data (BSON)
mongorestore --uri "<Atlas Cluster URI>" --drop <dump folder>

# As plain text (JSON)
mongoimport --uri "<Atlas Cluster URI>" --drop=<filename>.json
```
> [--drop] - drops the collection before importing.


## MongoDB - Web Collection Browser

### Query (operators)
```json
// Simple exact search
{"state": "NY", "city": "ALBANY"}

// Less than or equal
{"seconds": {"$lte": 60} }

// Not equal
{"field_name": {"$ne": "value"} }

// Equal (default if ommitted)
{"field_name": {"$eq": "value"} }
{"field_name": "value" }

// Simple and search
{"state": "NY", "pop": {"$lte": 1000}}
{"state": "NY", "pop": {"$gte": 1000, "$lte":1050}}

// Logic Operators
{"$or": [{"state": "NY"}, {"state": "AL"}]}
{"$nor": [{"state": "NY"}, {"state": "AL"}]}
{"$and": [ {"state": "NY"}, {"pop": {"$lte":1000}} ]}
{"state": {"$not": {"$eq": "NY"}}}

// Arrays
{"amenities": "Wifi"} // The array includes 'Wifi'.
{"amenities": {"$all": ["Wifi", "Kitchen"]}} // The array includes both
{"amenities": {"$size": 20}}
{"amenities": {"$size": 20, "$all": ["Wifi"]}}

// Example 1
{"$and":[
  {"$or":[
    {"dst_airport": "KZN"},
    {"src_airport": "KZN" }
  ]},
  {"$or":[
      {"airplane": "CR2" },
      {"airplane": "A81" }
  ]}
]}

// Example 2
{"$or":[
  {"founded_year": 2004, "$or":[{"category_code":"social"}, {"category_code":"web"}]},
  {"founded_month": 10, "$or":[{"category_code":"social"}, {"category_code":"web"}]}
]}
```

### Query (using expressions)
```json

// Two fields have the same value
{ "$expr": { "$eq": [ "$end station id", "$start station id"] } }

// Combined comparisons
{ "$expr": { 
    "$and": [
      {"$eq": [ "$end station id", "$start station id"]},
      {"$lte": ["$tripduration", 1000]}
    ]
  }
}
```