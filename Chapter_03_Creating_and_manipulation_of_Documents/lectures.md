# Chapter-03- Creating and Manipulating Documents

## Insert Documents


If the collection does not currently exist, insert operations will create the collection.

### Insert a Single Document

db.collection.insertOne() inserts a single document into a collection.

The following example inserts a new document into the inventory collection. If the document does not specify an _id field, MongoDB adds the _id field with an ObjectId value to the new document.

db.inventory.insertOne(
   { item: "canvas", qty: 100, tags: ["cotton"], size: { h: 28, w: 35.5, uom: "cm" } }
)


insertOne() returns a document that includes the newly inserted document's _id field value. For an example of a return document, see db.collection.insertOne() reference.

To retrieve the document that you just inserted, query the collection:

db.inventory.find( { item: "canvas" } )

### Insert Multiple Documents

db.collection.insertMany() can insert multiple documents into a collection. Pass an array of documents to the method.

The following example inserts three new documents into the inventory collection. If the documents do not specify an _id field, MongoDB adds the _id field with an ObjectId value to each document. 

db.inventory.insertMany([
   { item: "journal", qty: 25, tags: ["blank", "red"], size: { h: 14, w: 21, uom: "cm" } },
   { item: "mat", qty: 85, tags: ["gray"], size: { h: 27.9, w: 35.5, uom: "cm" } },
   { item: "mousepad", qty: 25, tags: ["gel", "blue"], size: { h: 19, w: 22.85, uom: "cm" } }
])

insertMany() returns a document that includes the newly inserted documents _id field values. See the reference for an example.

## Insert Behavior

Collection Creation
If the collection does not currently exist, insert operations will create the collection.

_id Field
In MongoDB, each document stored in a collection requires a unique _id field that acts as a primary key. If an inserted document omits the _id field, the MongoDB driver automatically generates an ObjectId for the _id field.

This also applies to documents inserted through update operations with upsert: true.

## ObjectId

ObjectId(<hexadecimal>)

Returns a new ObjectId value. The 12-byte ObjectId value consists of:

a 4-byte timestamp value, representing the ObjectId's creation, measured in seconds since the Unix epoch
a 5-byte random value generated once per process. This random value is unique to the machine and process.
a 3-byte incrementing counter, initialized to a random value
While the BSON format itself is little-endian, the timestamp and counter values are big-endian, with the most significant bytes appearing first in the byte sequence.


db.inspections.insert({
      "_id" : ObjectId("56d61033a378eccde8a8354f"),
      "id" : "10021-2015-ENFO",
      "certificate_number" : 9278806,
      "business_name" : "ATLIXCO DELI GROCERY INC.",
      "date" : "Feb 20 2015",
      "result" : "No Violation Issued",
      "sector" : "Cigarette Retail Dealer - 127",
      "address" : {
              "city" : "RIDGEWOOD",
              "zip" : 11385,
              "street" : "MENAHAN ST",
              "number" : 1712
         }
  })

db.inspections.insert({
      "id" : "10021-2015-ENFO",
      "certificate_number" : 9278806,
      "business_name" : "ATLIXCO DELI GROCERY INC.",
      "date" : "Feb 20 2015",
      "result" : "No Violation Issued",
      "sector" : "Cigarette Retail Dealer - 127",
      "address" : {
              "city" : "RIDGEWOOD",
              "zip" : 11385,
              "street" : "MENAHAN ST",
              "number" : 1712
         }
  })

db.inspections.find({"id" : "10021-2015-ENFO", "certificate_number" : 9278806}).pretty()



Insert three test documents:
db.inspections.insert([ { "test": 1 }, { "test": 2 }, { "test": 3 } ])

Insert three test documents but specify the _id values:
db.inspections.insert([{ "_id": 1, "test": 1 },{ "_id": 1, "test": 2 },
                       { "_id": 3, "test": 3 }])

Find the documents with _id: 1
db.inspections.find({ "_id": 1 })

Insert multiple documents specifying the _id values, and using the "ordered": false option.
db.inspections.insert([{ "_id": 1, "test": 1 },{ "_id": 1, "test": 2 },
                       { "_id": 3, "test": 3 }],{ "ordered": false })

Insert multiple documents with _id: 1 with the default "ordered": true setting
db.inspection.insert([{ "_id": 1, "test": 1 },{ "_id": 3, "test": 3 }])

View collections in the active db
show collections

Switch the active db to training
use training

View all available databases
show dbs



## Update Documents

db.collection.updateOne(<filter>, <update>, <options>)
db.collection.updateMany(<filter>, <update>, <options>)
db.collection.replaceOne(<filter>, <update>, <options>)


db.inventory.insertMany( [
   { item: "canvas", qty: 100, size: { h: 28, w: 35.5, uom: "cm" }, status: "A" },
   { item: "journal", qty: 25, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "mat", qty: 85, size: { h: 27.9, w: 35.5, uom: "cm" }, status: "A" },
   { item: "mousepad", qty: 25, size: { h: 19, w: 22.85, uom: "cm" }, status: "P" },
   { item: "notebook", qty: 50, size: { h: 8.5, w: 11, uom: "in" }, status: "P" },
   { item: "paper", qty: 100, size: { h: 8.5, w: 11, uom: "in" }, status: "D" },
   { item: "planner", qty: 75, size: { h: 22.85, w: 30, uom: "cm" }, status: "D" },
   { item: "postcard", qty: 45, size: { h: 10, w: 15.25, uom: "cm" }, status: "A" },
   { item: "sketchbook", qty: 80, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "sketch pad", qty: 95, size: { h: 22.85, w: 30.5, uom: "cm" }, status: "A" }
] );


Update Documents in a Collection
To update a document, MongoDB provides update operators, such as $set, to modify field values.

To use the update operators, pass to the update methods an update document of the form:
{
  <update operator>: { <field1>: <value1>, ... },
  <update operator>: { <field2>: <value2>, ... },
  ...
}

### Update a Single Document
The following example uses the db.collection.updateOne() method on the inventory collection to update the first document where item equals "paper":

db.inventory.updateOne(
   { item: "paper" },
   {
     $set: { "size.uom": "cm", status: "P" },
     $currentDate: { lastModified: true }
   }
)

The update operation:

uses the $set operator to update the value of the size.uom field to "cm" and the value of the status field to "P",
uses the $currentDate operator to update the value of the lastModified field to the current date. If lastModified field does not exist, $currentDate will create the field. See $currentDate for details.

db.inventory.updateMany(
   { "qty": { $lt: 50 } },
   {
     $set: { "size.uom": "in", status: "P" },
     $currentDate: { lastModified: true }
   }
)


The update operation:

uses the $set operator to update the value of the size.uom field to "in" and the value of the status field to "P",
uses the $currentDate operator to update the value of the lastModified field to the current date. If lastModified field does not exist, $currentDate will create the field. See $currentDate for details.

db.inventory.replaceOne(
   { item: "paper" },
   { item: "paper", instock: [ { warehouse: "A", qty: 60 }, { warehouse: "B", qty: 40 } ] }
)



## Delete Documents

db.collection.deleteMany()
db.collection.deleteOne()
The examples on this page use the inventory collection. To populate the inventory collection, run the following:

db.inventory.insertMany( [
   { item: "journal", qty: 25, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "notebook", qty: 50, size: { h: 8.5, w: 11, uom: "in" }, status: "P" },
   { item: "paper", qty: 100, size: { h: 8.5, w: 11, uom: "in" }, status: "D" },
   { item: "planner", qty: 75, size: { h: 22.85, w: 30, uom: "cm" }, status: "D" },
   { item: "postcard", qty: 45, size: { h: 10, w: 15.25, uom: "cm" }, status: "A" },
] );


### Delete All Documents

To delete all documents from a collection, pass an empty filter document {} to the db.collection.deleteMany() method.

The following example deletes all documents from the inventory collection:

db.inventory.deleteMany({})

The method returns a document with the status of the operation. For more information and examples, see deleteMany().


### Delete All Documents that Match a Condition
You can specify criteria, or filters, that identify the documents to delete. The filters use the same syntax as read operations.

To specify equality conditions, use <field>:<value> expressions in the query filter document:

{ <field1>: <value1>, ... }

A query filter document can use the query operators to specify conditions in the following form:

{ <field1>: { <operator1>: <value1> }, ... }


To delete all documents that match a deletion criteria, pass a filter parameter to the deleteMany() method.

The following example removes all documents from the inventory collection where the status field equals "A":

db.inventory.deleteMany({ status : "A" })


The method returns a document with the status of the operation. For more information and examples, see deleteMany().

### Delete Only One Document that Matches a Condition

To delete at most a single document that matches a specified filter (even though multiple documents may match the specified filter) use the db.collection.deleteOne() method.

The following example deletes the first document where status is "D":

db.inventory.deleteOne( { status: "D" } )

