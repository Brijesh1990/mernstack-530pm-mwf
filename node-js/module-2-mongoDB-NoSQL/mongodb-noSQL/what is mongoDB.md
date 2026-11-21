# What is MongoDB?

1. MongoDB is a document-oriented NoSQL database.
2. It stores data as BSON documents (a binary representation of JSON) inside collections, rather than storing rows in tables.

```text
Database or database schema
└── Collection (similar to a table) or (table)
    └── Document (similar to a row)
        └── Fields (similar to columns)
```

Documents in one collection can have different fields, which is useful when records have flexible or evolving shapes. MongoDB also supports indexes, validation rules, transactions, and aggregation.

## What is MongoDB Compass?

MongoDB Compass is MongoDB's graphical user interface (GUI). It lets you connect to a MongoDB deployment, browse and edit documents, filter and sort results, build aggregation pipelines, inspect schemas, and manage indexes without typing every command.

Compass is a client, not a database server: you still need a MongoDB deployment, such as a local MongoDB Community Server or a MongoDB Atlas cluster.

## Connect with Compass

1. Install and start MongoDB Community Server locally, or create a MongoDB Atlas cluster.
2. Open Compass and enter the connection string for your deployment.
   - A typical local connection string is `mongodb://localhost:27017`.
   - For Atlas, copy the connection string from Atlas and replace its username, password, and database placeholders. Keep credentials private.
3. Select **Connect**.
4. Create a database named `sample_store` and a collection named `products`. In Compass, use **Create Database** and enter both names. A MongoDB database is persisted once it has a collection or data.

## Sample documents

In Compass, open `sample_store.products`, choose **Add Data** > **Insert Document**, and insert each example document separately:

```json
{
  "name": "Notebook",
  "category": "stationery",
  "price": 4.5,
  "stock": 120,
  "tags": ["paper", "school"],
  "active": true,
  "details": {
    "color": "blue",
    "pages": 200
  }
}
```

```json
{
  "name": "Pen",
  "category": "stationery",
  "price": 1.25,
  "stock": 300,
  "tags": ["ink", "school"],
  "active": true,
  "details": {
    "color": "black",
    "type": "ballpoint"
  }
}
```

```json
{
  "name": "Desk lamp",
  "category": "electronics",
  "price": 24.99,
  "stock": 15,
  "tags": ["light", "desk"],
  "active": false,
  "details": {
    "color": "white",
    "power": "LED"
  }
}
```

MongoDB adds an `_id` field to each document if you do not provide one. It is a unique identifier, commonly stored as an `ObjectId`.

## Find and filter documents in Compass

In the **Documents** tab, enter a filter in the **Filter** box and select **Find**. The examples below use MongoDB query-filter syntax.

| Goal | Filter |
| --- | --- |
| Find all documents | `{}` |
| Find one category | `{ "category": "stationery" }` |
| Find a price greater than 5 | `{ "price": { "$gt": 5 } }` |
| Find prices from 2 through 25 | `{ "price": { "$gte": 2, "$lte": 25 } }` |
| Find either of two categories | `{ "category": { "$in": ["stationery", "electronics"] } }` |
| Find active products that have stock | `{ "active": true, "stock": { "$gt": 0 } }` |
| Match a nested field | `{ "details.color": "blue" }` |
| Find documents containing a tag | `{ "tags": "school" }` |
| Match either condition | `{ "$or": [{ "category": "electronics" }, { "price": { "$lt": 2 } }] }` |

Use the **Project** box to choose returned fields:

```json
{ "name": 1, "price": 1, "_id": 0 }
```

Use the **Sort** box to sort by price ascending (`1`) or descending (`-1`):

```json
{ "price": 1 }
```

Use **Limit** to cap the number of results, for example `5`.

## CRUD: create, read, update, and delete

CRUD describes the four common operations on data:

- **Create:** In Compass, use **Add Data** > **Insert Document** and enter a document like the samples above.
- **Read:** Use the **Filter**, **Project**, **Sort**, and **Limit** boxes in the **Documents** tab, as shown above.
- **Update:** Select a document and use Compass's edit action to change its fields. For update commands, the examples below can be run in `mongosh` (including Compass's embedded shell, if available).
- **Delete:** Select a document and use its delete action. For delete commands, see the shell examples below.

### Equivalent `mongosh` examples

These commands use the same database and collection. In a terminal, start `mongosh` and select the database:

```javascript
use sample_store
```

Insert one document:

```javascript
db.products.insertOne({
  name: "Ruler",
  category: "stationery",
  price: 2,
  stock: 50,
  tags: ["school"],
  active: true
})
```

Read documents:

```javascript
db.products.find({ category: "stationery" })
db.products.findOne({ name: "Pen" })
```

Update one document, setting its stock to 250:

```javascript
db.products.updateOne(
  { name: "Pen" },
  { $set: { stock: 250 } }
)
```

Increase the price of all stationery products by 0.25:

```javascript
db.products.updateMany(
  { category: "stationery" },
  { $inc: { price: 0.25 } }
)
```

Delete one matching document:

```javascript
db.products.deleteOne({ name: "Ruler" })
```

> **Caution:** `deleteMany({})` deletes every document in the collection. Always verify a delete filter before running it.

## More useful queries

Count products in a category:

```javascript
db.products.countDocuments({ category: "stationery" })
```

Sort and limit results:

```javascript
db.products.find({ active: true }).sort({ price: -1 }).limit(2)
```

Find documents where a field exists:

```javascript
db.products.find({ "details.color": { $exists: true } })
```

## Aggregation in Compass

An aggregation pipeline processes documents through stages. In Compass, open the **Aggregations** tab, add these stages, and select **Run**:

```javascript
{ $match: { active: true } }
{ $group: { _id: "$category", productCount: { $sum: 1 }, averagePrice: { $avg: "$price" } } }
{ $sort: { averagePrice: -1 } }
```

The `$match` stage filters first; `$group` calculates a count and average price per category; `$sort` orders those results by average price, highest first.

Equivalent `mongosh` pipeline:

```javascript
db.products.aggregate([
  { $match: { active: true } },
  {
    $group: {
      _id: "$category",
      productCount: { $sum: 1 },
      averagePrice: { $avg: "$price" }
    }
  },
  { $sort: { averagePrice: -1 } }
])
```

## Index example

An index can make frequent searches on a field faster, at the cost of extra storage and slower writes. In Compass, open the **Indexes** tab and create an ascending index on `category`. The equivalent shell command is:

```javascript
db.products.createIndex({ category: 1 })
```

## MongoDB compared with a relational database

| Relational database | MongoDB |
| --- | --- |
| Database table | Collection |
| Row | Document |
| Column | Field |
| Fixed table schema | Flexible document shape, optionally enforced with validation |
| SQL query | MongoDB query filter or aggregation pipeline |

MongoDB is a good fit for many applications with document-shaped data and flexible schemas. A relational database may be a better fit when the application depends heavily on relational joins and strict table structures. Choose based on the data and access patterns, not only because one format is newer.
