# What is Mongodb?
MongoDB is a source-available cross-platform document-oriented database program. Classified as a NoSQL database program, MongoDB uses JSON-like documents with optional schemas. MongoDB is developed by MongoDB Inc. and licensed under the Server Side Public License (SSPL).MongoDB is a document database with the scalability and flexibility that you want with the querying and indexing that you need.

1. MongoDB stores data in flexible, JSON-like documents, meaning fields can vary from document to document and data structure can be changed over time

2. The document model maps to the objects in your application code, making data easy to work with

3. Ad hoc queries, indexing, and real time aggregation provide powerful ways to access and analyze your data

4. MongoDB is a distributed database at its core, so high availability, horizontal scaling, and geographic distribution are built in and easy to use.


## WHAT YOU CAN DO
### Work with your data in MongoDB?


1. Store and query your data
2. Transform data with Aggregations.
3. Sexure access to your data
4. Deploy and scale your database.

## What is MongoDB Database?
MongoDB database is structured NoSQL database. In this, data are not stored in the table, rows and coloumn, instead it is stored in documents.

## Document Structure

Document - a way to organize and store data as a set of field-value pairs.

Field - a unique identifier for a datapoint.

Value - data related to a given identifier.

MongoDB documents are composed of field-and-value pairs and have the following structure:


{
   field1: value1,

   field2: value2,

   field3: value3,

   ...

   fieldN: valueN

}

The value of a field can be any of the BSON data types, including other documents, arrays, and arrays of documents. For example, the following document contains values of varying types:

var mydoc =

 {
               _id: ObjectId("5099803df3f4948bd2f98391"),

               name: { first: "Alan", last: "Turing" },

               birth: new Date('Jun 23, 1912'),

               death: new Date('Jun 07, 1954'),

               contribs: [ "Turing machine", "Turing test", "Turingery" ],

               views : NumberLong(1250000)

   }


The above fields have the following data types:

_id holds an ObjectId.
name holds an embedded document that contains the fields first and last.
birth and death hold values of the Date type.
contribs holds an array of strings.
views holds a value of the NumberLong type.
Field Names
Field names are strings.

Documents have the following restrictions on field names:

The field name _id is reserved for use as a primary key; its value must be unique in the collection, is immutable, and may be of any type other than an array. If the _id contains subfields, the subfield names cannot begin with a  ("$")  symbol.



Field names cannot contain the null character.


The server permits storage of field names that contain dots (".") and dollar signs ("$").


MongodB 5.0 adds improved support for the use of ("$") and (".") in field names.


## Collection

Collection - an organized store of documents in MongoDB usually with common fields between documents. There can be many collections per database and many documents per collection.


# What is MongoDB Atlas?

Mongodb Atlas is cloud database and is Fully Managed Database Service, build for wide range of applications and mongodb at its core.
MongoDB Atlas takes care of time-consuming and costly administration tasks so you can get the database resources you need, when you need them.


## Automated Deployments
Infrastructure provisioning, setup, and deployment is fully automated with MongoDB Atlas. Select a cloud provider, region, instance size, memory, and additional configurations in the Cluster Builder or via the API and be on your way.

## Simple Configuration Changes
When requirements and workloads change, MongoDB Atlas makes it easy to make post-deployment modifications to the database. Scale up, add more storage, configure cross-region clusters, create read-only or analytics nodes, and more with the click of a button.

##    Continuous Improvements
Patches and minor version upgrades are applied automatically so you can take advantage of the latest updates and features. For major version upgrades, you choose when you want them to happen. Atlas makes it easy to spin up environments to test compatibility or try out new features.

Atlas will analyze , visulize , export and build application with your data. It has many different services and tools available to it, all of which are used to storage and retrieval of data within mongodb.

Atlas can deploy the clusters. Clusters are group of servers that store your data. This is in turn configure to replica set. 
Replica Set - a few connected machines that store the same data to ensure that if something happens to one of the machines the data will remain intact. Comes from the word replicate - to copy something.

Instance - a single machine locally or in the cloud, running a certain software, in our case it is the MongoDB database.

Cluster - group of servers that store your data.


## Services of Atlas:

1. Manage Cluster Cretion.
2. Run and maintain database deployment.
3. Use Cloud service provider of your choice.
4. Experiment With new tools and features.







