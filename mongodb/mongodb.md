#### Start service and enter mongodb
`sudo service mongodb start` will start mongodb and we can enter the cli by doing `mongo`.

#### List dbs
`show dbs`
Thsi will list all dbs running in the mongo service.

#### use [dbname]
This will allow you to select a specific db to be aliased to `db`. If this does not exist, it will be created and switched to this database.

#### [db]
If want to check the database you're working in you can use `db`.

#### Documents
A document is a JSON object which contains key value pairs.

#### db.createUser
We can create a new reference using this function. We can see the details by looking at the online docs: https://docs.mongodb.com/manual/reference/method/db.createUser/

We will allow this user to have the roles readWrite and dbAdmin.

```db.createUser({
  user: "billy",
  pwd: "pwd",
  roles: ["readWrite", "dbAdmin"]
});
```

Running this returns us `Successfully added user: { "user" : "billy", "roles" : [ "readWrite", "dbAdmin" ] }`

#### Creating a collection
Collections are similar to tables in a relational database. They hold records or documents.

We can do this by doing:
`db.createCollection('collName');`
This returns us `{ "ok" : 1 }` to verify this was successful.

#### List Collections
We can also verify which collections exist in our database by doing `show collections`.

#### Adding data to collection
We can use `insert` to insert data:
`db.customers.insert({first_name:"John", last_name: "Doe"});` which returns us with `WriteResult({ "nInserted" : 1 })` to show the data was successfully inserted.

#### Getting data
To get data we use `find`:
`db.customers.find();` returns us: `{ "_id" : ObjectId("5b07e5ae082cbb0b4d1024a7"), "first_name" : "John", "last_name" : "Doe" }`

We get our list of documents back which has our firstname and lastname but also has an id field. MongoDB gives every record it's own unique ID. This is handled by MongoDB which would normally have to be configured manaully in a relational database.

#### Inserting multiple
We ca still do this with the `insert` command. With a relational Database, we would need to specify that the Customers table has specific fields and can't add new ones on the fly. This is not true for MongoDB as we can add any field we like. Here we will add two records, one with a gender field and one without:

`db.customers.insert([{first_name:"Steven", last_name: "Smith"}, {first_name: "Joan", last_name: "johnson", gender: "female"}]);`

Now we can see we have one record with a gender of `female`:

```
> db.customers.insert([{first_name:"Steven", last_name: "Smith"}, {first_name: "Joan", last_name: "johnson", gender: "female"}]);
BulkWriteResult({
	"writeErrors" : [ ],
	"writeConcernErrors" : [ ],
	"nInserted" : 2,
	"nUpserted" : 0,
	"nMatched" : 0,
	"nModified" : 0,
	"nRemoved" : 0,
	"upserted" : [ ]
})
> db.customers.find();
{ "_id" : ObjectId("5b07e5ae082cbb0b4d1024a7"), "first_name" : "John", "last_name" : "Doe" }
{ "_id" : ObjectId("5b07e792082cbb0b4d1024a8"), "first_name" : "Steven", "last_name" : "Smith" }
{ "_id" : ObjectId("5b07e792082cbb0b4d1024a9"), "first_name" : "Joan", "last_name" : "johnson", "gender" : "female" }

```

#### Pretty print
We can pretty print our data when we get it by using `pretty();` on the end of our query to print things nicely in the console.

#### Update
To update data, we will be using `update`. Our first parameter must be a match to tell mongoDB which records will need updating. Below we are changing all records where the `first_name` is `John`. This in reality would be a bad query to change a single record as there could be multiple records with this name. We should normally use the id where possible.

`db.customers.update({first_name:"John"})`
