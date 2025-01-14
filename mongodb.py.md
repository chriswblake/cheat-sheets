### Join

```json
{
  "$lookup": {
    "from": "comments", // The additional collection to lookup from
    "let": { "id": "$_id" }, // Saving a field to a variable, the ID from the other first collection.
    "pipeline": [
      { "$match": { "$expr": { "$eq": ["$movie_id", "$$id"] } } } //Querying for the needed data, based on $$id
    ],
    "as": "movie_comments" //Store the results in this field
  }
}
```

### Insert

```python
# Ensure that the write propgates to most servers
# w=0 -> Fire and forget
# w=1 (default)
db.users.with_options(write_concern=WriteConcern(w="majority")).insert_one({
    "name": name,
    "email": email,
    "password": hashedpw
})
```

### Update

```python
# Insert if it doesn't already exist
result = db.sessions.update_one(
            { "user_id": email },
            { "$set": { "jwt": jwt } },
            upsert=True
        )
```

### Connection + Settings

```python
def get_db():
   """
   Configuration method to return db instance
   """
   db = getattr(g, "_database", None)
   MFLIX_DB_URI = current_app.config["MFLIX_DB_URI"]
   MFLIX_DB_NAME = current_app.config["MFLIX_NS"]
   if db is None:
       db = g._database = MongoClient(
       MFLIX_DB_URI,

       # Connectin Pooling Set the maximum connection pool size to 50 active connections.
       maxPoolSize=50,
       # Set the write timeout limit to 2500 milliseconds.
       wTimeout=2500
       )[MFLIX_DB_NAME]
   return db


# Use LocalProxy to read the global db instance with just `db`
db = LocalProxy(get_db)
```

### Change Streams (events)

```python
# Anything changes
try:
    with inventory.watch(full_document="updateLookup") as change_stream_cursor:
        for data_change in change_stream_cursor:
            print(data_change)
except:
    print('Change stream closed because of error.)


# Filtered
low_quantity_pipeline = [
    {"$match": {"fullDocument.quantity": {"$lt": 20 }}}
]
with inventory.watch(pipeline=low_quantity_pipeline, full_document="updateLookup") as change_stream_cursor:
    for data_change in change_stream_cursor:
        current_quantity = data_change["fullDocument"].get("quantity")
        fruit = data_change["fullDocument"].get("type")
        msg = "Therea re only {0} units left of {1}".format(current_quantity, fruit)
        print(msg)
```
