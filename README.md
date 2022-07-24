### Database
Database is a physical container for collections. Each database gets its own set of files on the file system. A single MongoDB server typically has multiple databases.

### Collection
Collection is a group of MongoDB documents. It is the equivalent of an RDBMS table. A collection exists within a single database. Collections do not enforce a schema. Documents within a collection can have different fields. Typically, all documents in a collection are of similar or related purpose.

### Document
A document is a set of key-value pairs. Documents have dynamic schema. Dynamic schema means that documents in the same collection do not need to have the same set of fields or structure, and common fields in a collection's documents may hold different types of data.  

The following table shows the relationship of RDBMS terminology with MongoDB.

| RDBMS | MongoDB |
| --- | --- |
| Database | Database |
| Table | Collection |
| Tuple/Row | Document |
| column | Field |
| Table Join | Embedded Documents |
| Primary Key | Primary Key (Default key _id provided by MongoDB itself) |
| Database Server and Client |     |
| mysqld/Oracle | mongod |
| mysql/sqlplus | mongo |

### Sample Document
Following example shows the document structure of a blog site, which is simply a comma separated key value pair.

```json
{
   _id: ObjectId(7df78ad8902c)
   title: 'MongoDB Overview', 
   description: 'MongoDB is no sql database',
   by: 'tutorials point',
   url: 'http://www.tutorialspoint.com',
   tags: ['mongodb', 'database', 'NoSQL'],
   likes: 100, 
   comments: [	
      {
         user:'user1',
         message: 'My first comment',
         dateCreated: new Date(2011,1,20,2,15),
         like: 0 
      },
      {
         user:'user2',
         message: 'My second comments',
         dateCreated: new Date(2011,1,25,7,45),
         like: 5
      }
   ]
}
```

#### MongoDB Statistics
To get stats about MongoDB server, type the command db.stats() in MongoDB client. This will show the database name, number of collection and documents in the database. 
```sh
db.stats()
```

Create or select database
```sh
use DATABASE_NAME
```
To check your currently selected database
```sh
db
```
Check databases list
```sh
show dbs
```
To display database, you need to insert at least one document into it.
```sh
db.users.insert({"name":"daghan"})
show dbs
```
Drop a existing database
```sh
db.dropDatabase()
```
This will delete the selected database. If you have not selected any database, then it will delete default 'test' database.  

Creates a new collection
```sh
db.createCollection("empDetails")
```
Check the created collection
```sh
show collections
```
Drop a collection from the database
```sh
db.COLLECTION_NAME.drop()
```
Insert only one document into a collection
```sh
db.COLLECTION_NAME.insertOne(document)
```
Insert multiple documents
```sh
db.COLLECTION_NAME.insertMany(documents)
```
Retrieves all the documents
```sh
db.COLLECTION_NAME.find(query, projection)
```
To display the results in a formatted way
```sh
db.COLLECTION_NAME.find(query, projection).pretty()
```
Return only one document
```sh
db.COLLECTION_NAME.findOne(query, projection)
db.COLLECTION_NAME.findOne({"name":"Tom Benzamin"},{"address":1})
db.COLLECTION_NAME.find().limit(NUMBER)
db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)
db.COLLECTION_NAME.find().sort({KEY:1})
```
### RDBMS Where Clause Equivalents in MongoDB
To query the document on the basis of some condition, you can use following operations.

| Operation | Syntax | Example | RDBMS Equivalent |
| --- | --- | --- | --- |
| Equality | {&lt;key&gt;:{$eg;&lt;value&gt;}} | db.mycol.find({"by":"tutorials point"}).pretty() | where by = 'tutorials point' |
| Less Than | {&lt;key&gt;:{$lt:&lt;value&gt;}} | db.mycol.find({"likes":{$lt:50}}).pretty() | where likes < 50 |
| Less Than Equals | {&lt;key&gt;:{$lte:&lt;value&gt;}} | db.mycol.find({"likes":{$lte:50}}).pretty() | where likes <= 50 |
| Greater Than | {&lt;key&gt;:{$gt:&lt;value&gt;}} | db.mycol.find({"likes":{$gt:50}}).pretty() | where likes > 50 |
| Greater Than Equals | {&lt;key&gt;:{$gte:&lt;value&gt;}} | db.mycol.find({"likes":{$gte:50}}).pretty() | where likes >= 50 |
| Not Equals | {&lt;key&gt;:{$ne:&lt;value&gt;}} | db.mycol.find({"likes":{$ne:50}}).pretty() | where likes != 50 |
| Values in an array | {&lt;key&gt;:{$in:\[&lt;value1&gt;, &lt;value2&gt;,……&lt;valueN&gt;\]}} | db.mycol.find({"name":{$in:\["Raj", "Ram", "Raghu"\]}}).pretty() | Where name matches any of the value in :\["Raj", "Ram", "Raghu"\] |
| Values not in an array | {&lt;key&gt;:{$nin:&lt;value&gt;}} | db.mycol.find({"name":{$nin:\["Ramu", "Raghav"\]}}).pretty() | Where name values is not in the array :\["Ramu", "Raghav"\] or, doesn’t exist at all |


To query documents based on the AND condition
```sh
db.COLLECTIONNAME.find({ $and: [ {"key":"value"}, {"key":"value"} ] })
```
To query documents based on the OR condition
```sh
db.COLLECTIONNAME.find({$or: [{"key": "value"}, {"key":"value"}]}).pretty()
```
Using AND and OR Together
```sh
db.COLLECTIONNAME.find({"key": {$gt:10}, $or: [{"key":"value"},{"key":"value"}]}).pretty()
```
To query documents based on the NOT condition
```sh
db.COLLECTION_NAME.find({$not: [{"key": "value"}, {"key":"value"}]})
```
Update the values in the existing document
```sh
db.COLLECTION_NAME.update(SELECTION_CRITERIA, UPDATED_DATA)
db.COLLECTION_NAME.update({"key":"value"},{$set:{"key":"value"}})
db.COLLECTION_NAME.update({"key":"value"},{$set:{"key":"value"}},{multi:true})
```
Replace the existing document with the new document passed
```sh
db.COLLECTION_NAME.save({_id:ObjectId(),NEW_DATA})
db.COLLECTION_NAME.save({"_id" : ObjectId("id"), "key":"value","key":"value"})
```
Update the values in the existing document
```sh
db.COLLECTION_NAME.findOneAndUpdate(SELECTIOIN_CRITERIA, UPDATED_DATA)
db.COLLECTION_NAME.findOneAndUpdate({"key": "value"},{ $set: { "key": "value", "key": "value"}})
```
Update a single document which matches the given filter
```sh
db.COLLECTION_NAME.updateOne(<filter>, <update>)
```
Update all the documents that matches the given filter
```sh
db.COLLECTION_NAME.update(<filter>, <update>)
db.COLLECTION_NAME.updateMany({key:{ $gt: "value" }},{ $set: { key: 'value'}})
```
Remove a document from the collection
```sh
db.COLLECTION_NAME.remove(DELLETION_CRITTERIA)
db.COLLECTION_NAME.remove({'key':'value'})
```
Delete only the first record
```sh
db.COLLECTION_NAME.remove(DELETION_CRITERIA,1)
```
Remove All Documents
```sh
db.mycol.remove({})
```
To create an index
```sh
db.COLLECTION_NAME.createIndex({KEY:1})
```
Drop a particular index
```sh
db.COLLECTION_NAME.dropIndex({KEY:1})
```
Delete multiple (specified) indexes on a collection
```sh
db.COLLECTION_NAME.dropIndexes()
```
Return the description of all the indexes int the collection
```sh
db.COLLECTION_NAME.getIndexes()
```
Aggregations operations process data records and return computed results
```sh
db.COLLECTION_NAME.aggregate(AGGREGATE_OPERATION)
db.COLLECTION_NAME.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : 1}}}])
```
Following is a list of available aggregation expressions.

| Expression | Description | Example |
| --- | --- | --- |
| $sum | Sums up the defined value from all documents in the collection. | db.mycol.aggregate(\[{$group : {\_id : "$by\_user", num_tutorial : {$sum : "$likes"}}}\]) |
| $avg | Calculates the average of all given values from all documents in the collection. | db.mycol.aggregate(\[{$group : {\_id : "$by\_user", num_tutorial : {$avg : "$likes"}}}\]) |
| $min | Gets the minimum of the corresponding values from all documents in the collection. | db.mycol.aggregate(\[{$group : {\_id : "$by\_user", num_tutorial : {$min : "$likes"}}}\]) |
| $max | Gets the maximum of the corresponding values from all documents in the collection. | db.mycol.aggregate(\[{$group : {\_id : "$by\_user", num_tutorial : {$max : "$likes"}}}\]) |
| $push | Inserts the value to an array in the resulting document. | db.mycol.aggregate(\[{$group : {\_id : "$by\_user", url : {$push: "$url"}}}\]) |
| $addToSet | Inserts the value to an array in the resulting document but does not create duplicates. | db.mycol.aggregate(\[{$group : {\_id : "$by\_user", url : {$addToSet : "$url"}}}\]) |
| $first | Gets the first document from the source documents according to the grouping. Typically this makes only sense together with some previously applied “$sort”-stage. | db.mycol.aggregate(\[{$group : {\_id : "$by\_user", first_url : {$first : "$url"}}}\]) |
| $last | Gets the last document from the source documents according to the grouping. Typically this makes only sense together with some previously applied “$sort”-stage. | db.mycol.aggregate(\[{$group : {\_id : "$by\_user", last_url : {$last : "$url"}}}\]) |

Creating a user administrator
```sh
db.createUser({	user: "admin",	pwd: "password",roles:[{role: "userAdminAnyDatabase" , db:"admin"}]})
```
Modeling Referenced Relationships
```sh
var result = db.COLLECTION_NAME.findOne({"key":"value"},{"key":1})
var result = db.COLLECTION_NAME.find({"_id":{"$in":result["key"]}})
```
Using DBRefs
```sh
>var user = db.users.findOne({"key":"value"})
>var dbRef = user.<key>
>db[dbRef.$ref].findOne({"_id":(dbRef.$id)})
```




