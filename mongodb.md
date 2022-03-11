
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
db.<collection name>.findOne();
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

### Filter (Search)
```json
# Simple exact search
{"state": "NY", "city": "ALBANY"}
```