# Section 2: Understanding the Basics & CRUD Operations

- Database can have multiple collections, collections can have documents
- Databases, collections and documents are created implicitly when we are storing data(created on demand)

        admin   0.000GB -> Users and roles
        config  0.000GB -> Configurations
        local   0.000GB

- Document is always defined with `{}` (JSON document)
- Every document inserted will get a unique ID
- This ID can be used to sort because it has timestamp data
- We can also assign ids if we can make sure it will be unique
`db.flightData.insertOne({     departureAirport: "CMB",    arrivalAirport: "DXB",     "aircraft": "Airbus A380",  _id:"cmb-dxb-1"   })`

MongoDB will convert JSON into BSON(Binary Data) which will be stored

## CRUD Operations

### Create

`insertOne(data, options)`
`insertMany(data, options)` done by passing an array of JSON documents

    db.flightData.insertMany([
    ...   {
    ...     "departureAirport": "MUC",
    ...     "arrivalAirport": "SFO",
    ...     "aircraft": "Airbus A380",
    ...     "distance": 12000,
    ...     "intercontinental": true
    ...   },
    ...   {
    ...     "departureAirport": "LHR",
    ...     "arrivalAirport": "TXL",
    ...     "aircraft": "Airbus A320",
    ...     "distance": 950,
    ...     "intercontinental": false
    ...   }
    ... ])

MongoDB automatically keeps the order,

    {
        "acknowledged" : true,
        "insertedIds" : [
                ObjectId("6026def4ae906db0be2898b2"), >>>> id end with 2
                ObjectId("6026def4ae906db0be2898b3")  >>>> id end with 3
        ]
    }

### Read

`find(filter, options)`
`findOne(filter, options)` gives the 1st matching document

`db.flightData.find({intercontinental: true}).pretty()`

Finds flights with distance greater than 10000
`db.flightData.find({distance: {**$gt**: 10000}}).pretty()`

pretty() is not supported in findOne(), because **pretty() is a method of the cursor object**
`db.flightData.findOne({distance: {$gt: 900}})`

#### find()

- find() does not return all results and it will return a **cursor object** with metadata which allows us to cycle through results
- find() gives the first 20 documents by default

- `it` allows to fetch the next set of data

- `db.collection.find().toArray()` will fetch all documents

- `db.passengers.find().forEach((passenger) => {printjson(passenger)})`
- forEach allows to pass a function so that we can do anything with all the documents
- For insert, update and delete cursors do not exist because the manipulate data and not fetch

#### Projection

- We can only fetch the required fields of a document, so it doesn't impact bandwidth
- Data transformation is done in the MongoDB server before being sent
- When using projection the id will be returned by default

This will fetch all passenger names with the objectId
`db.passengers.find({}, {name: 1}).pretty()`

- First argument is empty(no filter) as we require all passengers
- Second argument allows to project, here we pass which key values we want to get
1 = Include the field in the data that is being returned
0 = Exclude the field

If we want to exclude the object Id we have to explicitly exclude it
`db.passengers.find({}, {name: 1, _id: 0}).pretty()`

### Update

`updateOne(filter, data, options)`
`updateMany(filter, data, options)`

db.flightData.updateOne({"_id" : ObjectId("6026def4ae906db0be2898b2")}, {$set: {delayed:true}})
db.flightData.updateMany({}, {$set: {marker: "toDelete"}})
db.flightData.update({"_id" : ObjectId("6026def4ae906db0be2898b2")}, {$set: {delayed:false}})

- `$` is a reserved operator in MongoDB
- `$set` MongoDB will identify which describes the changes
- `$set` is **required** in **updateOne** and **updateMany**. Using `$set` **patches the existing document**

- update with `$set` and updateMany will update all matching elements
- The difference of update is that when `$set` is not used it will take an object and will replace the existing object/document

        {
                "_id" : ObjectId("6026def4ae906db0be2898b2"),
                "departureAirport" : "MUC",
                "arrivalAirport" : "SFO",
                "aircraft" : "Airbus A380",
                "distance" : 12000,
                "intercontinental" : true,
                "delayed" : false
        }

        db.flightData.update({"_id" : ObjectId("6026def4ae906db0be2898b2")},  {delayed:false})

After update, document will have the **same ID but overrode all the other key value pairs**,

    { "_id" : ObjectId("6026def4ae906db0be2898b2"), "delayed" : false }

In general, it's better to use updateOne and updateMany

#### Replace

If you want to replace it's safer to use replaceOne

    db.flightData.replaceOne({"_id" : ObjectId("6026def4ae906db0be2898b2")}, {
    ...     "departureAirport": "MUC",
    ...     "arrivalAirport": "SFO",
    ...     "aircraft": "Airbus A380",
    ...     "distance": 12000,
    ...     "intercontinental": true
    ...   })

### Delete

`deleteOne(filter, options)`
`deleteMany(filter, options)`

db.flightData.deleteOne({departureAirport: "CMB"})
db.flightData.deleteOne({_id: "cmb-mlb-1"})
db.flightData.deleteMany(({})
db.flightData.deleteMany({marker: 'toDelete'})

## Embedded Documents

- This nesting of documents inside one document
- In MongoDB  there's a hard limit of 100 levels of nesting
- The maximum overall document size is 16mb
- Another JSON document within a wrapping JSON document

`db.flightData.updateMany({}, { $set: {status: {description: 'on-time', lastUpdated: '1 hour ago', details: {responsible: 'Tanushka'} }}})`

## Arrays

- Arrays can hold any type of data
- Can also be arrays of embedded documents
- Any kind of data that can be in a document can also be in an array

`db.passengers.updateOne({name: 'Albert Twostone'},{$set: {hobbies: ['gaming', 'football', 'cooking']} })`

### Accessing Structured data

`db.passengers.findOne({name:'Albert Twostone'}).hobbies`
will return
`[ "gaming", "football", "cooking" ]`

`db.passengers.findOne({hobbies: 'gaming'})` MongoDB can find the object if the array contains
will return

    {
            "_id" : ObjectId("6027e8e1e6bda30bca1c8742"),
            "name" : "Albert Twostone",
            "age" : 68,
            "hobbies" : [
                    "gaming",
                    "football",
                    "cooking"
            ]
    }

When finding nested data we have to use .(dot notation) and when using this we need to wrap in ""(quotes)
`db.flightData.find({'status.description': 'on-time'}).pretty()`
`db.flightData.find({'status.details.responsible': 'Tanushka'}).pretty()`
