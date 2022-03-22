# Chapter 5: Indexing and Aggregation Pipeline

## Lab: Aggregation Framework
## Problem:

To complete this exercise connect to your Atlas cluster using the in-browser IDE space at the end of this chapter.

What room types are present in the sample_airbnb.listingsAndReviews collection?

## Answer:
Private room

Shared room 

Entire home/apt


## Quiz: Aggregation Framework
## Problem:

What are the differences between using aggregate() and find()?

## Answer:

aggregate() can do what find() can and more.

aggregate() allows us to compute and reshape data in the cursor.


## Quiz 1: sort() and limit()
## Problem:

Which of the following commands will return the name and founding year for the 5 oldest companies in the sample_training.companies collection?

## Answer:
db.companies.find({ "founded_year": { "$ne": null }},
                  { "name": 1, "founded_year": 1 }
                 ).sort({ "founded_year": 1 }).limit(5)


db.companies.find({ "founded_year": { "$ne": null }},
                  { "name": 1, "founded_year": 1 }
                 ).limit(5).sort({ "founded_year": 1 })



## Quiz 2: sort() and limit()
## Problem:

To complete this exercise connect to your Atlas cluster using the in-browser IDE space at the end of this chapter.

In what year was the youngest bike rider from the sample_training.trips collection born?

## Answer:
1999


## Quiz: Introduction to Indexes
## Problem:
 
Jameela often queries the sample_training.routes collection by the src_airport field like this:

db.routes.find({ "src_airport": "MUC" }).pretty()
Which command will create an index that will support this query?

## Answer:
db.routes.createIndex({ "src_airport": -1 })


## Quiz: Introduction to Data Modeling
## Problem:

What is data modeling?


## Answer:


a way to organize fields in a document to support your application performance and querying capabilities


## Quiz: Upsert
## Problem:

How does the upsert option work?


## Answer:

By default upsert is set to false.

When upsert is set to true and the query predicate returns an empty cursor, the update operation creates a new document using the directive from the query predicate and the update predicate.

When upsert is set to false and the query predicate returns an empty cursor then there will be no updated documents as a result of this operation.