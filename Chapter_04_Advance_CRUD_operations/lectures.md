# Chapter - 04 - Advance CRUD Operations

# Query Documents

 The examples on this page use the inventory collection. Populate the inventory collection with the following documents:

[
    { "item": "journal", "qty": 25, "size": { "h": 14, "w": 21, "uom": "cm" }, "status": "A" },
    { "item": "notebook", "qty": 50, "size": { "h": 8.5, "w": 11, "uom": "in" }, "status": "A" },
    { "item": "paper", "qty": 100, "size": { "h": 8.5, "w": 11, "uom": "in" }, "status": "D" },
    { "item": "planner", "qty": 75, "size": { "h": 22.85, "w": 30, "uom": "cm" }, "status": "D" },
    { "item": "postcard", "qty": 45, "size": { "h": 10, "w": 15.25, "uom": "cm" }, "status": "A" }
]


## Select All Documents in a Collection
To select all documents in the collection, pass an empty document as the query filter parameter to the query bar. The query filter parameter determines the select criteria.


This operation corresponds to the following SQL statement:

SELECT * FROM inventory

## Specify Equality Condition
To specify equality conditions, use <field>:<value> expressions in the query filter document:

{ <field1>: <value1>, ... }


The following example selects from the inventory collection all documents where the status equals "D":

Copy the following filter into the Compass query bar and click Find:

{ status: "D" }


This operation corresponds to the following SQL statement:

SELECT * FROM inventory WHERE status = "D"

The MongoDB Compass query bar autocompletes the current query based on the keys in your collection's documents, including keys in embedded sub-documents.


## Specify Conditions Using Query Operators

A query filter document can use the query operators to specify conditions in the following form:

{ <field1>: { <operator1>: <value1> }, ... }



The following example retrieves all documents from the inventory collection where status equals either "A" or "D":

Copy the following filter into the Compass query bar and click Find:

{ status: { $in: [ "A", "D" ] } }


Although you can express this query using the $or operator, use the $in operator rather than the $or operator when performing equality checks on the same field.


The operation corresponds to the following SQL statement:

SELECT * FROM inventory WHERE status in ("A", "D")


## Specify AND Conditions

A compound query can specify conditions for more than one field in the collection's documents. Implicitly, a logical AND conjunction connects the clauses of a compound query so that the query selects the documents in the collection that match all the conditions.

The following example retrieves all documents in the inventory collection where the status equals "A" and qty is less than ($lt) 30:

Copy the following filter into the Compass query bar and click Find:

{ status: "A", qty: { $lt: 30 } }

The operation corresponds to the following SQL statement:

SELECT * FROM inventory WHERE status = "A" AND qty < 30


## Specify OR Conditions

Using the $or operator, you can specify a compound query that joins each clause with a logical OR conjunction so that the query selects the documents in the collection that match at least one condition.

The following example retrieves all documents in the collection where the status equals "A" or qty is less than ($lt) 30:

Copy the following filter into the Compass query bar and click Find:

{ $or: [ { status: "A" }, { qty: { $lt: 30 } } ] }


The operation corresponds to the following SQL statement:

SELECT * FROM inventory WHERE status = "A" OR qty < 30


## Specify AND as well as OR Conditions
In the following example, the compound query document selects all documents in the collection where the status equals "A" and either qty is less than ($lt) 30 or item starts with the character p:

Copy the following filter into the Compass query bar and click Find:

{ status: "A", $or: [ { qty: { $lt: 30 } }, { item: /^p/ } ] }


The operation corresponds to the following SQL statement:

SELECT * FROM inventory WHERE status = "A" AND ( qty < 30 OR item LIKE "p%")



## MongoDB expressive query syntax

One of the objectives of the expressive query syntax is to bring the power of MongoDB’s aggregation expressions to MongoDB’s query language. An interesting use case is the ability to compose dynamic validation rules that compute and compare multiple attribute values at runtime. Using the new $expr operator, it is possible to validate the value of the totalWithVAT attribute with the following validation expression:

$expr: {
   $eq: [
     "$totalWithVAT",
     {$multiply: [
       "$total", 
       {$sum: [1, "$VAT"]}
     ]}
   ]
}



## Query an Array
The examples on this page use the inventory collection. To populate the inventory collection, run the following:

db.inventory.insertMany([
   { item: "journal", qty: 25, tags: ["blank", "red"], dim_cm: [ 14, 21 ] },
   { item: "notebook", qty: 50, tags: ["red", "blank"], dim_cm: [ 14, 21 ] },
   { item: "paper", qty: 100, tags: ["red", "blank", "plain"], dim_cm: [ 14, 21 ] },
   { item: "planner", qty: 75, tags: ["blank", "red"], dim_cm: [ 22.85, 30 ] },
   { item: "postcard", qty: 45, tags: ["blue"], dim_cm: [ 10, 15.25 ] }
]);


## Match an Array
To specify equality condition on an array, use the query document { <field>: <value> } where <value> is the exact array to match, including the order of the elements.

The following example queries for all documents where the field tags value is an array with exactly two elements, "red" and "blank", in the specified order:

db.inventory.find( { tags: ["red", "blank"] } )


If, instead, you wish to find an array that contains both the elements "red" and "blank", without regard to order or other elements in the array, use the $all operator:

db.inventory.find( { tags: { $all: ["red", "blank"] } } )



## Query an Array for an Element
To query if the array field contains at least one element with the specified value, use the filter { <field>: <value> } where <value> is the element value.

The following example queries for all documents where tags is an array that contains the string "red" as one of its elements:

db.inventory.find( { tags: "red" } )



To specify conditions on the elements in the array field, use query operators in the query filter document:

{ <array field>: { <operator1>: <value1>, ... } }


For example, the following operation queries for all documents where the array dim_cm contains at least one element whose value is greater than 25.

db.inventory.find( { dim_cm: { $gt: 25 } } )



## Specify Multiple Conditions for Array Elements

When specifying compound conditions on array elements, you can specify the query such that either a single array element meets these condition or any combination of array elements meets the conditions.

## Query an Array with Compound Filter Conditions on the Array Elements
The following example queries for documents where the dim_cm array contains elements that in some combination satisfy the query conditions; e.g., one element can satisfy the greater than 15 condition and another element can satisfy the less than 20 condition, or a single element can satisfy both:

db.inventory.find( { dim_cm: { $gt: 15, $lt: 20 } } )


## Query for an Element by the Array Index Position
Using dot notation, you can specify query conditions for an element at a particular index or position of the array. The array uses zero-based indexing.


When querying using dot notation, the field and nested field must be inside quotation marks.

The following example queries for all documents where the second element in the array dim_cm is greater than 25:

db.inventory.find( { "dim_cm.1": { $gt: 25 } } )


## Query an Array by Array Length
Use the $size operator to query for arrays by number of elements. For example, the following selects documents where the array tags has 3 elements.

db.inventory.find( { "tags": { $size: 3 } } )



## Query an Array of Embedded Documents
The examples on this page use the inventory collection. To populate the inventory collection, run the following:

db.inventory.insertMany( [
   { item: "journal", instock: [ { warehouse: "A", qty: 5 }, { warehouse: "C", qty: 15 } ] },
   { item: "notebook", instock: [ { warehouse: "C", qty: 5 } ] },
   { item: "paper", instock: [ { warehouse: "A", qty: 60 }, { warehouse: "B", qty: 15 } ] },
   { item: "planner", instock: [ { warehouse: "A", qty: 40 }, { warehouse: "B", qty: 5 } ] },
   { item: "postcard", instock: [ { warehouse: "B", qty: 15 }, { warehouse: "C", qty: 35 } ] }
]);


## Query for a Document Nested in an Array
The following example selects all documents where an element in the instock array matches the specified document:

db.inventory.find( { "instock": { warehouse: "A", qty: 5 } } )

Equality matches on the whole embedded/nested document require an exact match of the specified document, including the field order. For example, the following query does not match any documents in the inventory collection:

db.inventory.find( { "instock": { qty: 5, warehouse: "A" } } )



## Specify a Query Condition on a Field in an Array of Documents

Specify a Query Condition on a Field Embedded in an Array of Documents
If you do not know the index position of the document nested in the array, concatenate the name of the array field, with a dot (.) and the name of the field in the nested document.

The following example selects all documents where the instock array has at least one embedded document that contains the field qty whose value is less than or equal to 20:

db.inventory.find( { 'instock.qty': { $lte: 20 } } )


## Use the Array Index to Query for a Field in the Embedded Document
Using dot notation, you can specify query conditions for field in a document at a particular index or position of the array. The array uses zero-based indexing.


When querying using dot notation, the field and index must be inside quotation marks.

The following example selects all documents where the instock array has as its first element a document that contains the field qty whose value is less than or equal to 20:

db.inventory.find( { 'instock.0.qty': { $lte: 20 } } )


## Specify Multiple Conditions for Array of Documents

When specifying conditions on more than one field nested in an array of documents, you can specify the query such that either a single document meets these condition or any combination of documents (including a single document) in the array meets the conditions.

A Single Nested Document Meets Multiple Query Conditions on Nested Fields
Use $elemMatch operator to specify multiple criteria on an array of embedded documents such that at least one embedded document satisfies all the specified criteria.

The following example queries for documents where the instock array has at least one embedded document that contains both the field qty equal to 5 and the field warehouse equal to A:

db.inventory.find( { "instock": { $elemMatch: { qty: 5, warehouse: "A" } } } )


The following example queries for documents where the instock array has at least one embedded document that contains the field qty that is greater than 10 and less than or equal to 20:

db.inventory.find( { "instock": { $elemMatch: { qty: { $gt: 10, $lte: 20 } } } } )


## Combination of Elements Satisfies the Criteria

If the compound query conditions on an array field do not use the $elemMatch operator, the query selects those documents whose array contains any combination of elements that satisfies the conditions.

For example, the following query matches documents where any document nested in the instock array has the qty field greater than 10 and any document (but not necessarily the same embedded document) in the array has the qty field less than or equal to 20:

db.inventory.find( { "instock.qty": { $gt: 10,  $lte: 20 } } )



The following example queries for documents where the instock array has at least one embedded document that contains the field qty equal to 5 and at least one embedded document (but not necessarily the same embedded document) that contains the field warehouse equal to A:

db.inventory.find( { "instock.qty": 5, "instock.warehouse": "A" } )


