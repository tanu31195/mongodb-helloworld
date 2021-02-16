# Section 3: Schemas & Relations: How to Structure Documents

## Schemas

- Schema is the structure of a document(which fields, values and types)
- MongoDB doesn't enforce any schemas
- Can have totally different schemas
- But when developing we have to use some kind of schema which makes it easier and efficient to work
- If we want to have a SQL based schema we can set null for keys/fields
`db.products.insertOne({name: "T-Shirt", price: 20.99, size: "M", details: null})`

## Data Types

- Text: "Tanushka", "Bandara"
- Boolean: true, false
- Number: Integer(int32 > bits): 25, NumberLong(int64)/FloatValue: 10000000000000000, NumberDecimal: 12.99
- ObjectId("sdfsfds") Has a temporal component which guarantees the order when inserting many items
- ISODate: ISODate("2021-02-16")
- Timestamp: Timestamp(12121345) This will also be unique even if two items are inserted at the same time, it will add an ordinal values which give the order
- Embedded Documents: {a: {...}}
- Arrays: {a: [...]}
