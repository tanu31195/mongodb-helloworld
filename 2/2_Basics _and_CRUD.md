# Section 2: Understanding the Basics & CRUD Operations

Database can have multiple collections, collections can have documents
Databases, collections and documents are created implicitly when we are storing data(created on demand)

admin   0.000GB -> Users and roles
config  0.000GB -> Configurations
local   0.000GB

Document is always defined with `{}` (JSON document)

Every document inserted will get a unique ID
This ID can be used to sort because it has timestamp data
We can also assign ids if we can make sure it will be unique
`db.flightData.insertOne({     departureAirport: "CMB",    arrivalAirport: "DXB",     "aircraft": "Airbus A380",  _id:"cmb-dxb-1"   })`

MongoDB will convert JSON into BSON(Binary Data) which will be stored

`$` is a reserved operator in MongoDB
`$set` MongoDB will identify which describes the changes

## CRUD Operations

### Create

`insertOne(data, options)`
`insertMany(data, options)` done by passing an array of objects

### Read

`find(filter, options)`
`findOne(filter, options)` gives the 1st matching document

### Update

`updateOne(filter, data, options)`
`updateMany(filter, data, options)`

### Delete

`deleteOne(filter, options)`
`deleteMany(filter, options)`

db.flightData.deleteOne({departureAirport: "CMB"})
db.flightData.deleteOne({_id: "cmb-mlb-1"})
db.flightData.deleteMany(({})
db.flightData.deleteMany({marker: 'toDelete'})

db.flightData.updateOne({distance:12000}, {$set:{marker: "delete"}})
db.flightData.updateMany({}, {$set: {marker: "toDelete"}})

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
