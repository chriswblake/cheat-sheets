# Connect

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

# Count
db.<collection>.find({"field": "value"}).count()
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

# Rename a field
db.trips.find(
  { "start station id": 531 },
  {
    "_id": 0,
    "birth-year": "$birth year",
  }
)

```

### Join / Reference

```bash
# Find all stores with one of these ids
db.stores.find({_id: {$in: ["store1", "store2"]}})
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

# Update if existing, insert if new
db.<collection>.updateOne({ "sensor": r.sensor,
                            "date": r.date,
                            "valcount": { "$lt": 48 } # Will only update the doc if less than 48 values. Otherwise, it wills start a new doc.
                          },
                          { "$push": { "readings": { "v": r.value, "t": r.time } }, # Saves sensor data to value/time array
                            "$inc": { "valcount": 1, "total": r.value } # Increases array length counter by 1
                          },
                          { "upsert": true })

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

### Bulk Write

```bash
db.stock.bulkWrite([
  {updateOne: {"filter": {"item":"apple"},  "update": {"$inc":{"quantity":50 }}}},
  {updateOne: {"filter": {"item":"butter"}, "update": {"$inc":{"quantity":5 }}}},
  {updateOne: {"filter": {"item":"bread"},  "update": {"$inc":{"quantity":30 }}}},
  {updateOne: {"filter": {"item":"celery"}, "update": {"$inc":{"quantity":40 }}}}
], {ordered: false}) # optionally, specify that the operations are sequentially dependent and processed serially (true, default) or independent of each other and processed parallel (false)
```

# MongoDB - Web Collection Browser

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

# MongoDB - DB Management

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

### Indexes

```bash
# Single key
db.trips.createIndex({ "birth year": 1 })

# Compound key
db.trips.createIndex({ "start station id": 1, "birth year": 1 })

# Unique
db.shipments.createIndex({"truck_id":1}, {"unique": true})
```

### Schema

[Valid BSON Types](https://docs.mongodb.com/manual/reference/bson-types/)

`.\validate_m320.exe example --file answer_schema.json --verbose`

```json
# Example (answer_schema.json)
{
  "_id": "<objectId>",
  "title": "<string>",
  "artist": "<string>",
  "room": "<string>",
  "spot": "<string>",
  "on_display": "<bool>",
  "in_house": "<bool>",
  "events": [{
    "k": "<string>",
    "v": "<date>"
  }]
}
```

### Attribute Pattern

Convert the extra fields of a collection into an "additional_specs" array. This new array enables easier indexing on the fields that will be unique to several documents.

```json
// From this
{
  "manufacture": "China",
  "brand": "MongoDB",
  "sub_brand": "University",
  ...
  "input": "5V/1300 mA",
  "output": "5V/1A",
  "capacity": "4200mAh",
}
// To this
{
  "manufacture": "China",
  "brand": "MongoDB",
  "sub_brand": "University",
  "additional_specs": [
    {"k": "input", "v": "5V/1300 mA"},
    {"k": "output", "v": "5V/1A"},
    {"k": "capacity", "v": "4200", "u": "mAH"},
  ]
}
```

> Alternately, look into the [wildcard index](https://docs.mongodb.com/manual/core/index-wildcard/).

```bash
# To create the index
db.products.createIndex({"additional_specs.k":1, "additional_specs.v":1})

# To query using that index
dp.products.find({"additional_specs":{"$elemMatch":{"k":"capacity", "v":"4200"}}})
```

### Graphs

```bash
# Find the chain of parents
db.categories.aggregate([
    { $graphLookup: {
        from: 'categories',
        startWith: '$name',
        connectFromField: 'parent',
        connectToField: 'name',
        as: 'ancestors'
      }
    }
])

# Update all children to have a new parent
db.categories.updateMany(
  {parent: N},
  {$set: { parent: P }}
)

```

Materialized Path Approach

```json
{
  "name": "Books",
  "ancestors": ".Swag.Office"
}
```

```bash
# Immediate ancestor of Y
db.categories.find({ancestors: "/\.Y$/"})

# If it descends from X and Z
db.categories.find({ancestors: "/^\.X.*Y/i"})
```
