# Chapter 3: Creating and Manipulating Documents

## Quiz: ObjectId
## Problem:

How does the value of _id get assigned to a document?

## Answer:
You can select a non ObjectId type value when inserting a new document, as long as that value is unique to this collection.

It is automatically generated as an ObjectId type value.



## Quiz: Insert Errors
## Problem:

Select all true statements from the following list: 

## Answer:

If a document is inserted without a provided _id value, then the _id field and value will be automatically generated for the inserted document before insertion.

MongoDB can store duplicate documents in the same collection, as long as their _id values are different.

## Quiz: Insert Order
## Problem:

Which of the following commands will successfully insert 3 new documents into an empty pets collection?

## Answer:

db.pets.insert([{ "_id": 1, "pet": "cat" },
                { "_id": 1, "pet": "dog" },
                { "_id": 3, "pet": "fish" },
                { "_id": 4, "pet": "snake" }], { "ordered": false })


db.pets.insert([{ "_id": 1, "pet": "cat" },
                { "_id": 2, "pet": "dog" },
                { "_id": 3, "pet": "fish" },
                { "_id": 3, "pet": "snake" }])


db.pets.insert([{ "pet": "cat" }, { "pet": "dog" },
                { "pet": "fish" }])



## Quiz: Updating Documents
## Problem:

MongoDB has a flexible data model, which means that you can have fields that contain documents, or arrays as their values.

Select any invalid MongoDB documents from the given choices:


## Answer:
None of the above.


## Quiz: Updating Documents in the shell
## Problem:

Given a pets collection where each document has the following structure and fields:


{
 "_id": ObjectId("5ec414e5e722bb1f65a25451"),
 "pet": "wolf",
 "domestic?": false,
 "diet": "carnivorous",
 "climate": ["polar", "equatorial", "continental", "mountain"]
}
Which of the following commands will add new fields to the updated documents?


## Answer:

db.pets.updateMany({ "pet": "cat" },
                   { "$push": { "climate": "continental",
                                "look": "adorable" } }) 

db.pets.updateMany({ "pet": "cat" },
                   { "$set": { "type": "dangerous",
                               "look": "adorable" }})




## Quiz 1: Deleting Documents
## Problem:

The sample dataset contains a few databases that we will not use in this course. Clean up your Atlas cluster and get rid of all the collections in these databases:

sample_analytics
sample_geospatial
sample_weatherdata
Does removing all collections in a database also remove the database?

## Answer:
yes


## Quiz 2: Deleting Documents
## Problem:

Which of the following commands will delete a collection named villains?


## Answer:
db.villains.drop()