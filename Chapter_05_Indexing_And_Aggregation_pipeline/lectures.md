# Chapter- 05- Indexing and Aggregations  Pipelines

## Aggregation

Aggregation operations process multiple documents and return computed results. You can use aggregation operations to:

1. Group values from multiple documents together.
2. Perform operations on the grouped data to return a single result.
3. Analyze data changes over time.

To perform aggregation operations, you can use:

1. Aggregation pipelines, which are the preferred method for performing aggregations.
2. Single purpose aggregation methods, which are simple but lack the capabilities of an aggregation pipeline.
3. Map-reduce operations, which are deprecated starting in MongoDB 5.0. Instead, use an aggregation pipeline.


## Aggregation Pipelines

An aggregation pipeline consists of one or more stages that process documents:

1. Each stage performs an operation on the input documents. For example, a stage can filter documents, group documents, and calculate values.
2. The documents that are output from a stage are passed to the next stage.
3. An aggregation pipeline can return results for groups of documents. For example, return the total, average, maximum, and minimum values.


## Aggregation Pipeline Example

The following aggregation pipeline example contains two stages and returns the total order quantity of medium size pizzas grouped by pizza name:

db.orders.aggregate( [
   // Stage 1: Filter pizza order documents by pizza size
   {
      $match: { size: "medium" }
   },
   // Stage 2: Group remaining documents by pizza name and calculate total quantity
   {
      $group: { _id: "$name", totalQuantity: { $sum: "$quantity" } }
   }
] )



### The $match stage:

1. Filters the pizza order documents to pizzas with a size of medium.
2. Passes the remaining documents to the $group stage.


### The $group stage:

1. Groups the remaining documents by pizza name.
2. Uses $sum to calculate the total order quantity for each pizza name. The total is stored in the totalQuantity field returned by the aggregation pipeline.



Single Purpose Aggregation Methods
You can use the following single purpose aggregation methods to aggregate documents from a single collection:

db.collection.estimatedDocumentCount() -        Returns an approximate count of the documents in a collection or a view.

db.collection.count() -                         Returns a count of the number of documents in a collection or a view.

db.collection.distinct() -                      Returns an array of documents that have distinct values for the specified field.


The single purpose aggregation methods are simple but lack the capabilities of an aggregation pipeline.


### Map-Reduce

Starting in MongoDB 5.0, map-reduce is deprecated:

1. Instead of map-reduce, you should use an aggregation pipeline. Aggregation pipelines provide better performance and usability than map-reduce.
2. You can rewrite map-reduce operations using aggregation pipeline stages, such as $group, $merge, and others.
3. For map-reduce operations that require custom functionality, you can use the $accumulator and $function aggregation operators, available starting in version 4.4. You can use those operators to define custom aggregation expressions in JavaScript.


For examples of aggregation pipeline alternatives to map-reduce, see:

1. Map-Reduce to Aggregation Pipeline
2. Map-Reduce Examples



## Indexes

Indexes support the efficient execution of queries in MongoDB. Without indexes, MongoDB must perform a collection scan, i.e. scan every document in a collection, to select those documents that match the query statement. If an appropriate index exists for a query, MongoDB can use the index to limit the number of documents it must inspect.

Indexes are special data structures [1] that store a small portion of the collection's data set in an easy to traverse form. The index stores the value of a specific field or set of fields, ordered by the value of the field. The ordering of the index entries supports efficient equality matches and range-based query operations. In addition, MongoDB can return sorted results by using the ordering in the index.

The following diagram illustrates a query that selects and orders the matching documents using an index:


https://docs.mongodb.com/manual/images/index-for-sort.bakedsvg.svg



Fundamentally, indexes in MongoDB are similar to indexes in other database systems. MongoDB defines indexes at the collection level and supports indexes on any field or sub-field of the documents in a MongoDB collection.

Default _id Index
MongoDB creates a unique index on the _id field during the creation of a collection. The _id index prevents clients from inserting two documents with the same value for the _id field. You cannot drop this index on the _id field.


## Create an Index

To create an index in the Mongo Shell, use db.collection.createIndex().

db.collection.createIndex( <key and index type specification>, <options> )



The following example creates a single key descending index on the name field:

db.collection.createIndex( { name: -1 } )

The db.collection.createIndex() method only creates an index if an index of the same specification does not already exist.


## Index Names

The default name for an index is the concatenation of the indexed keys and each key's direction in the index ( i.e. 1 or -1) using underscores as a separator. For example, an index created on { item : 1, quantity: -1 } has the name item_1_quantity_-1.

You can create indexes with a custom name, such as one that is more human-readable than the default. For example, consider an application that frequently queries the products collection to populate data on existing inventory. The following createIndex() method creates an index on item and quantity named query for inventory:

db.products.createIndex(
  { item: 1, quantity: -1 } ,
  { name: "query for inventory" }
)


You can view index names using the db.collection.getIndexes() method. You cannot rename an index once created. Instead, you must drop and re-create the index with a new name.

## Index Types
MongoDB provides a number of different index types to support specific types of data and queries.

## Single Field

In addition to the MongoDB-defined _id index, MongoDB supports the creation of user-defined ascending/descending indexes on a single field of a document.

For a single-field index and sort operations, the sort order (i.e. ascending or descending) of the index key does not matter because MongoDB can traverse the index in either direction.    


## Compound Index

MongoDB also supports user-defined indexes on multiple fields, i.e. compound indexes.

The order of fields listed in a compound index has significance. For instance, if a compound index consists of { userid: 1, score: -1 }, the index sorts first by userid and then, within each userid value, sorts by score.

For compound indexes and sort operations, the sort order (i.e. ascending or descending) of the index keys can determine whether the index can support a sort operation. See Sort Order for more information on the impact of index order on results in compound indexes.


## Multikey Index

MongoDB uses multikey indexes to index the content stored in arrays. If you index a field that holds an array value, MongoDB creates separate index entries for every element of the array. These multikey indexes allow queries to select documents that contain arrays by matching on element or elements of the arrays. MongoDB automatically determines whether to create a multikey index if the indexed field contains an array value; you do not need to explicitly specify the multikey type.


## Geospatial Index

To support efficient queries of geospatial coordinate data, MongoDB provides two special indexes: 2d indexes that uses planar geometry when returning results and 2dsphere indexes that use spherical geometry to return results.


## Text Indexes

MongoDB provides a text index type that supports searching for string content in a collection. These text indexes do not store language-specific stop words (e.g. "the", "a", "or") and stem the words in a collection to only store root words.