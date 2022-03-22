# Chapter 2: Importing, Exporting, and Querying Data

## How does MongoDB store data?

JSON and BSON are close cousins, as their nearly identical names imply, but you wouldn’t know it by looking at them side-by-side. JSON, or JavaScript Object Notation, is the wildly popular standard for data interchange on the web, on which BSON (Binary JSON) is based. We’ll take a look at each, and hopefully throw some light on the JSON vs BSON mystery: what’s the difference, and why does it matter?

What is JSON?

The MongoDB JSON Connection

MongoDB: JSON vs BSON



## What is JSON?

JavaScript Object Notation, more commonly known as JSON, was defined as part of the JavaScript language in the early 2000s by JavaScript creator Douglas Crockford, though it wasn’t until 2013 that the format was officially specified.

JavaScript objects are simple associative containers, wherein a string key is mapped to a value (which can be a number, string, function, or even another object). This simple language trait allowed JavaScript objects to be represented remarkably simply in text:


{

  "_id": 1,

  "name" : { "first" : "John", "last" : "Backus" },

  "contribs" : [ "Fortran", "ALGOL", "Backus-Naur Form", "FP" ],

  "awards" : [

    {
      
      "award" : "W.W. McDowell Award",

      "year" : 1967,

      "by" : "IEEE Computer Society"


    },
     {

      "award" : "Draper Prize",

      "year" : 1993,

      "by" : "National Academy of Engineering"


    }

  ]

}

As JavaScript became the default language of client-side web development, JSON began to take on a life of its own. By virtue of being both human- and machine-readable, and comparatively simple to implement support for in other languages, JSON quickly moved beyond the web page, and into software everywhere.

JSON shows up in many different cases:

1. APIs
2. Configuration files
3. Log messages
4. Database storage

JSON quickly overtook XML, is more difficult for a human to read, significantly more verbose, and less ideally suited to representing object structures used in modern programming languages.


## The MongoDB JSON Connection

MongoDB was designed from its inception to be the ultimate data platform for modern application development. JSON’s ubiquity made it the obvious choice for representing data structures in MongoDB’s innovative document data model.

However, there are several issues that make JSON less than ideal for usage inside of a database.

1. JSON is a text-based format, and text parsing is very slow

2. JSON’s readable format is far from space-efficient, another database concern

3. JSON only supports a limited number of basic data types

In order to make MongoDB JSON-first, but still high-performance and general-purpose, BSON was invented to bridge the gap: a binary representation to store data in JSON format, optimized for speed, space, and flexibility. It’s not dissimilar from other interchange formats like protocol buffers, or thrift, in terms of approach.


## What is BSON?

BSON simply stands for “Binary JSON,” and that’s exactly what it was invented to be. BSON’s binary structure encodes type and length information, which allows it to be parsed much more quickly.

Since its initial formulation, BSON has been extended to add some optional non-JSON-native data types, like dates and binary data, without which MongoDB would have been missing some valuable support.

Languages that support any kind of complex mathematics typically have different sized integers (ints vs longs) or various levels of decimal precision (float, double, decimal128, etc.).

Not only is it helpful to be able to represent those distinctions in data stored in MongoDB, it also allows for comparisons and calculations to happen directly on data in ways that simplify consuming application code.

## Does MongoDB use BSON, or JSON?

MongoDB stores data in BSON format both internally, and over the network, but that doesn’t mean you can’t think of MongoDB as a JSON database. Anything you can represent in JSON can be natively stored in MongoDB, and retrieved just as easily in JSON.
Unlike systems that simply store JSON as string-encoded values, or binary-encoded blobs, MongoDB uses BSON to offer the industry’s most powerful indexing and querying features on top of the web’s most usable data format.

For example, MongoDB allows developers to query and manipulate objects by specific keys inside the JSON/BSON document, even in nested documents many layers deep into a record, and create high performance indexes on those same keys and values.

When using a MongoDB driver in your language of choice, it’s still important to know that you’re accessing BSON data through the abstractions available in that language.

Firstly, BSON objects may contain Date or Binary objects that are not natively representable in pure JSON. Second, each programming language has its own object semantics. JSON objects have ordered keys, for instance, while Python dictionaries (the closest native data structure that’s analogous to JavaScript Objects) are unordered, while differences in numeric and string data types can also come into play. Third, BSON supports a variety of numeric types that are not native to JSON, and each language will represent these differently.

## JSON vs BSON


### JSON 


#### Encoding : -    UTF-8 String

#### Data Support: - String, Boolean, Number, Array


#### Readability: -  Human and Machine 



### BSON


#### Encoding : -    Binary

#### Data Support: - String, Boolean, Number (Integer, Float, Long, Decimal128...),Array, Date, Raw Binary


#### Readability: -  Machine Only 

JSON and BSON are indeed close cousins by design. BSON is designed as a binary representation of JSON data, with specific extensions for broader applications, and optimized for data storage and retrieval.

One particular way in which BSON differs from JSON is in its support for some more advanced types of data. JavaScript does not, for instance, differentiate between integers (which are round numbers), and floating-point numbers (which have decimal precision to various degrees).

Most server-side programming languages have more sophisticated numeric types (standards include integer, regular precision floating point number aka “float”, double-precision floating point aka “double”, and boolean values), each with its own optimal usage for efficient mathematical operations.





SRV connection string - a specific format used to establish a connection between your application and a MongoDB instance.

Code used in this lecture:

mongodump --uri "mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies"

mongoexport --uri="mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies" --collection=sales --out=sales.json

mongorestore --uri "mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies"  --drop dump

mongoimport --uri="mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies" --drop sales.json



## Data Explorer

Atlas UI provides us with data explorer so that we can query data using the GUI(Graphical User Interface)


Namespace - The concatenation of the database name and collection name is called a namespace.

We looked at the sample_training.zips collection and issued the following queries:

{"state": "NY"}
{"state": "NY", "city": "ALBANY"}



# Find() Command

show dbs            (show the databases)

use sample_training

show collections                (show collection or files inside the specific database)

db.zips.find({"state": "NY"})

it iterates through the cursor.


db.zips.find({"state": "NY"}).count()

db.zips.find({"state": "NY", "city": "ALBANY"})

db.zips.find({"state": "NY", "city": "ALBANY"}).pretty()




