# Chapter 4: Advanced CRUD Operations

## Lab 1: Comparison Operators
## Problem:

To complete this exercise connect to your Atlas cluster using the in-browser IDE space at the end of this chapter.

How many documents in the sample_training.zips collection have fewer than 1000 people listed in the pop field?

Copy/paste the exact numeric value (without double quotes) of the result that you get into the response field.

## Answer:
8065


## Lab 2: Comparison Operators
## Problem:

To complete this exercise connect to your Atlas cluster using the in-browser IDE space at the end of this chapter.

What is the difference between the number of people born in 1998 and the number of people born after 1998 in the sample_training.trips collection?

Enter the exact numeric value of the result that you get into the response field.

## Answer:
6

## Lab 3: Comparison Operators
## Problem:

To complete this exercise connect to your Atlas cluster using the in-browser IDE space at the end of this chapter.

Using the sample_training.routes collection find out which of the following statements will return all routes that have at least one stop in them?

## Answer:

db.routes.find({ "stops": { "$ne": 0 }}).pretty()

db.routes.find({ "stops": { "$gt": 0 }}).pretty()


## Quiz 1: Logic Operators
## Problem:

To complete this exercise connect to your Atlas cluster using the in-browser IDE space at the end of this chapter.

How many businesses in the sample_training.inspections dataset have the inspection result "Out of Business" and belong to the "Home Improvement Contractor - 100" sector?

Enter the exact numeric value of the result that you get into the response field.

## Answer:
4

## Quiz 2: Logic Operators
## Problem:

Which is the most succinct query to return all documents from the sample_training.inspections collection where the inspection date is either "Feb 20 2015", or "Feb 21 2015" and the company is not part of the "Cigarette Retail Dealer - 127" sector?

## Answer:

db.inspections.find(
  { "$or": [ { "date": "Feb 20 2015" },
             { "date": "Feb 21 2015" } ],
    "sector": { "$ne": "Cigarette Retail Dealer - 127" }}).pretty()




## Lab 1: Logic Operators
## Problem:

To complete this exercise connect to your Atlas cluster using the in-browser IDE space at the end of this chapter.

Before solving this exercise, make sure to undo some of the changes that we made to the zips collection earlier in the course by running the following command:


db.zips.updateMany({ "city": "HUDSON" }, { "$inc": { "pop": -10 } })
How many zips in the sample_training.zips dataset are neither over-populated nor under-populated?

In this case, we consider population of more than 1,000,000 to be over- populated and less than 5,000 to be under-populated.

Copy/paste the exact numeric value (without double quotes) of the result that you get into the response field.


## Answer:
11193


## Lab 2: Logic Operators
## Problem:

To complete this exercise connect to your Atlas cluster using the in-browser IDE space at the end of this chapter.

How many companies in the sample_training.companies dataset were

either founded in 2004

[and] either have the social category_code [or] web category_code,
[or] were founded in the month of October

[and] also either have the social category_code [or] web category_code?
Copy/paste the exact numeric value (without double quotes) of the result that you get into the response field.

## Answer:
149


## Quiz 1: $expr
##Problem:

What are some of the uses for the $ sign in MQL?


## Answer:
$ signifies that you are looking at the value of that field rather than the field name.

$ denotes an operator.


## Quiz 2: $expr
## Problem:

Which of the following statements will find all the companies that have more employees than the year in which they were founded?


## Answer:

db.companies.find(
    { "$expr": { "$gt": [ "$number_of_employees", "$founded_year" ]} }
  ).count()

db.companies.find(
    { "$expr": { "$lt": [ "$founded_year", "$number_of_employees" ] } }
  ).count()



## Lab: $expr
## Problem:

To complete this exercise connect to your Atlas cluster using the in-browser IDE space at the end of this chapter.

How many companies in the sample_training.companies collection have the same permalink as their twitter_username?

## Answer:
1299


## Lab 1: Array Operators
## Problem:

To complete this exercise connect to your Atlas cluster using the in-browser IDE space at the end of this chapter.

What is the name of the listing in the sample_airbnb.listingsAndReviews dataset that accommodates more than 6 people and has exactly 50 reviews?

Copy/Paste the value of the "name" field into the response field without quotation marks.


## Answer:
Sunset Beach Lodge Retreat


## Lab 2: Array Operators
## Problem:

To complete this exercise connect to your Atlas cluster using the in-browser IDE space at the end of this chapter.

Using the sample_airbnb.listingsAndReviews collection find out how many documents have the "property_type" "House", and include "Changing table" as one of the "amenities"?

Enter the number of results to the response field.

## Answer:
11


## Quiz: Array Operators
## Problem:

Which of the following queries will return all listings that have "Free parking on premises", "Air conditioning", and "Wifi" as part of their amenities, and have at least 2 bedrooms in the sample_airbnb.listingsAndReviews collection?


## Answer:

db.listingsAndReviews.find(
  { "amenities":
      { "$all": [ "Free parking on premises", "Wifi", "Air
        conditioning" ] }, "bedrooms": { "$gte":  2 } } ).pretty()




## Lab: Array Operators and Projection
## Problem:

To complete this exercise connect to your Atlas cluster using the in-browser IDE space at the end of this chapter.

How many companies in the sample_training.companies collection have offices in the city of Seattle?

Copy/paste your answer to the response field.


## Answer:
117


## Quiz: Array Operators and Projection
## Problem:

Which of the following queries will return only the names of companies from the sample_training.companies collection that had exactly 8 funding rounds?


## Answer:


db.companies.find({ "funding_rounds": { "$size": 8 } },
                  { "name": 1, "_id": 0 })



## Lab 1: Querying Arrays and Sub-Documents
## Problem:

To complete this exercise connect to your Atlas cluster using the in-browser IDE space at the end of this chapter.

How many trips in the sample_training.trips collection started at stations that are to the west of the -74 longitude coordinate?

Longitude decreases in value as you move west.

Note: We always list the longitude first and then latitude in the coordinate pairs; i.e.

<field_name>: [ <longitude>, <latitude> ]

## Answer:
1928


## Lab 2: Querying Arrays and Sub-Documents
## Problem:

To complete this exercise connect to your Atlas cluster using the in-browser IDE space at the end of this chapter.

How many inspections from the sample_training.inspections collection were conducted in the city of NEW YORK?


## Answer:
18279


## Quiz: Querying Arrays and Sub-documents
## Problem:

Which of the following queries will return the names and addresses of all listings from the sample_airbnb.listingsAndReviews collection where the first amenity in the list is "Internet"?


## Answer:

db.listingsAndReviews.find({ "amenities.0": "Internet" },
                           { "name": 1, "address": 1 }).pretty()    
