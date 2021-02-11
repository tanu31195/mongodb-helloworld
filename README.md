# mongodb-helloworld

## What is MongoDB?

<https://docs.mongodb.com/>

- MongoDB is a database solution
- Database server
- Popular for Read/Write heavy applications
- In MySQL there're tables but in MongoDB we have collections
- Inside of a collection there're Documents
- MongoDB is Schemaless
- Documents are in JSON(BSON) format (key value pairs)

        {
            "name": "Tanushka",
            "age": 25,
            "address":
            {
                "city": "Boralesgamuwa"
            },
            "hobbies": [
                {"name": "Gaming"},
                {"name": "Football"}
            ]
        }

- Key names should be in double quotations
- We can store nested data as shown by address (This allows to create complex relations between data and store them in one document and can be easily fetched without any complex joins)
- Hobbies are a list of embedded documents

APPLICATION----------------------------------------------------------------------------------->DATA

Frontend (UI) > Backend (Server)> Drivers(Node.js,Java, Python)<<< Queries >>> MongoDB Server << Communicate >> Storage Engine << File/Data Access >> DataBase files(Slow) || Memory(Fast)

### BSON Data Structure

No schema (Gives a lot of flexibility)
Less relations due to embedded documents
Less collections (tables) no joins or merging required

### MongoDB EcoSystem

#### MongoDB database

- Community/ Enterprise solutions
- Atlas (Cloud solution)
- Mobile
- Compass (GUI)
- BI connectors
- MongoDB charts

#### Stitch

- Serveless backend solution
- Serveless Query API
- Serveless Functions
- Database Triggers
- Real-Time Sync

### Installing MongoDB

<https://www.mongodb.com/try/download/community>

After installation add PATH variable of the MongoDB binaries
`C:\Program Files\MongoDB\Server\4.4\bin`

Create data db path
`C:\data\db`

#### Basic Commands

Run CMD as admin to stop the MongoDB background service in windows
`net stop MongoDB`

To start the service
`mongod`

To connect
`mongo`

Clear screen
`cls`

Show existing DB information
`show dbs`

Create new DB (It will automatically switch to the newly created db)
`use dbname`

Insert document(MacBook Pro) to a collection(products)
`db.products.insertOne({"name": "MacBook Pro", "brand": apple, "price": 1099})`
`db.products.insertOne({"name": "iPhone 12", "brand": "apple", "price": 729, "processor": "A14", "features": { "os": "iOS 14.1", "ram": "4GB"}})`

To view documents in a collection
`db.products.find()`
`db.products.find().pretty()`

To start the server in a different port
`mongod --port 27018`
`mongo --port 27018`
