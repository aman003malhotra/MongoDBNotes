# MongoDB Notes

## COllections
- Collection is a group of MongoDB documents.It is the equivalent of an RDBMS table. A collection exists within a single database.Collections do not enforce a schema. Documents within a collection can have different fields. Typically, all documents in a collection are of similar or related purpose.

## Database is a physical container for collections.
Each database gets its own set of files on the file system. A single MongoDB server typically has multiple databases.

### Some Rules for Database
- The empty string ("") is not a valid database name.
- A database name cannot contain any of these characters: / , \, ., ", *, <, >, :, |, ?, $ (a single space), or \O (the null character). Basically, stick with alphanumeric ASCII.
- Database names are case-insensitive.
- Database names are limited to a maximum of 64 bytes.

- admin
The admin database plays a role in authentication and authorization
- local
This database stores data specific to a single server.

- config
Sharded MongoDB clusters use the config database to store information about each shard.


## Advantages on mongodb

- Schema less - MongoDB is a document database in which one collection holds different documents. Number of fields, content and size of the document can differ from one document to another.
- Structure of a single object is clear
- No complex joins
- Deep query ability. MongoDB supports dynamic queries on documents using a document-based query language that's nearly powerful as SQL.
- Performace Tuning 
- Ease of scale Out - Mongodb is easy to scale.
- Conversion/mapping of application objects to database objects not needed.
- Uses internal memory of storing the (windowed) working set, enabling faster access to data.
- Flexible Schema - unline SQL database, where you must determine and declare a table's schema before inserting data, mongoDB's collection by default, do not require their document to have the same schema.


### Embedded Document 

- Embedded document captures relationships between data by storing related data in a single document structure. MongoDB document make is possible to embed document structure in a field or array within a document.

- These deformalize data models allow applications to retrieve and manipulate related data in a single database operation.

- 100 Levels of Document Nesting is Allowed.

- Maximum Size of a document allowed is 16MB.

```
db.students.find({'idCards.hasPanCard':true})

```

### References 
- References store the relationships between the data by including links or references from one document to another. Applications can resolve these references to access the related data. Broadly, these are normalized data models.

### Mongo Shell Commands
- `show collections` - show all the collections in a particular database.
- `show dbs` - show all the databases.
- `use dbname` - will shift to the database `dbname`.
- `db.collectionName.insertOne({bson data})` - will create a collection if it is already not there and if there is already a collection then will add the new document into the collection.
- `db.collectionName.find()` - will display all the documents for a particular collection.
- `db.collectionName.insertMany([{}, {}, {}])` - in every curly brackets there are documents and the documents will be inserted into the collection.
- `db.collectionName.updateOne({key:value to find the document}, {$set: {updating key: updating Value})` - for updating the information in the document.
-  `db.collectionName.deleteOne({key:value to find the document})` - for deleting a particular document from a collection.
- `db.collectionName.drop` - to delete the whole collection.

### MongoDB Data Types
- String 

```
{"x" : "foobar"}
```

- Number
```
{"X" : 3}
{"x" : NumberInt("3")}
{"x" : NumberLong("3")}
```
- Boolean 
```
{"x" : true}
```
- Double
```
{"x" : 546.43}
```
- Arrays

```
["x" : ["a","b", "c"]]
```

- TimeStamps

```
timestamp{}
```

- Object

Embedded Document

- Date

Date()
new Date()
new ISODate()

- Regular Expression
- new RegExp('regular expression')

### Logical Operators

- `db.collectionName.find({$and: [{"sex":"Male"}, {"class":"VI"}]}).pretty()` - to apply and operations while using find query.
- `db.collectionName.find({$or: [{"sex":"Male"}, {"age":11}]})` -  to apply or operations while using the find query.
- `db.collectionName.find({$nor: [{"sex":"Male"}, {"age":11}]})` - both the conditions should not be true.
- `db.collectionName.find({"age" : {$not:{$lt:12}}})` - age should not be less than 12.

### Comparison Operators

- `$eq` - equal to
```
db.collectionName.find({age:{$eq:5}})
```
- `$gt` - greater than
```
db.collectionName.find({age:{$gt:5}})
```
- `$gte` - greater than or equal to
```
db.collectionName.find({age:{$gte:5}})
```
- `$lt` - less than
```
db.collectionName.find({age:{$lt:5}})
```
- `$lte` - less than or equal to
```
db.collectionName.find({age:{$lte:5}})
```
- `$ne` - not equal
```
db.collectionName.find({age:{$ne:5}})
```
- `$in` - 
```
db.collectionName.find({age:{$in:[5, 11, 12]}})
```
- `$nin` - Not in
db.collectionName.find({age:{$nin:[5, 11, 12]}})



### Sample queries

- `db.pesto.find({age:{$eq : 23}}, {name:1, _id:0})` - find from the collection pesto where age is equal to 23 and don't show _id and show name.
- `db.collectionName.find({skills:{$in:['html', 'css']}})` - find from skills array that have skills html or css.
- 

### Element Query Operator

-`$exists` - 


### Aggregation

- Aggregation operations process data records and return computed results.
- Aggregation operation group values from multiple documents together and can perform a variety of operations on the grouped data to return a single result.
- SQL count and with group by is equivalent of MongoDB aggregation.

- `$match` - db.collenctionName.aggregate([{$match:{country:'Spain', city:'Salamanca'}}]) - match the key value pair as mentioned.
- `$project` - db.collectionName.aggregate([ { $project:{_id:0, country:1, city:1, name:1} }])  - when we want specific fields in the document to be printed.
- `$group` - db.collectionName.aggregate([ {$group: {_id:'$name', totaldocs:{$sum:1} } } ]) - group by the name and give the number of totaldocs for each name.
```
db.teachers.aggregate([{$group:{ _id: "$age", students: { $push:"$$ROOT" }}}])
The $$ROOT value is a reference to the current document being processed in the pipeline, which represents the complete document.

db.teachers.aggregate([{$match:{gender:"male"}}, {$group:{_id:"$age", number:{$sum:1}}}]) 
The value of $sum is 1, which means that for each document in the group, the value of
"number" will be incremented by 1.

db.students.aggregate([{$match:{gender:"male"}}, {$group:{_id:"$age", count:{$sum:1}}, {$sort:count:-1}}])
db.student.aggregate([{$unwind:"$Hobbies"},{$group:{_id:"$age", hobbies:{$push:"$Hobbies"}}}])
Find Hobbies per age group

db.teacher.aggregate([{$group:{_id:null, averageAge:{$age:"$age"}}}])
Find average age of all students

db.students.aggregate([{$unwind:"$Hobbies"}, {$group:{_id:null, count:{$sum:1}}}])
Find the total number of hobbies for all the students in a collection

{ $ifNull: [ <expression>, <repIacementExpression> ] } used if some field is not there

input: Specifies the array expression to filter.
as: Specifies a variable name that can be used inside the
cond expression to reference the current element of the
input array.
cond: Specifies the condition that must be met in order for
an element to be included in the result set. The expression
must return either true or false.

```

- `$count` - Calculates the quantity of documents in the given group.
- `$max` - Displays the maximum value of a document's field in the collection.
- `$min` - Displays the minimum value ofa document's field in the collection.
- `$avg` - Displays the average value of a document's field in the collection.
- `$sum` - Sums up the specified values of all documents in the collection.
- `$push` - Adds extra values into the array of the resulting document.
- `$out` - to add the result of the aggregate pipeline to a new collection.
- `$unwind` - distribute every record of an array and every element will have a different document for it where everything will be same but the array element will be different.

- `$sort` - db.collectionName.aggregate([$sort: {'students.number':-1}]) - -1 will means descending order and 1 will be asending order.
- `$limit` - db.collectionName.aggregate([{$limit: 2}]) - it will only show first 2 results.
- `$skip` - db.collectionName.aggregate([{$skip : 1}]) - skip 1 value.






### What are Indexes
- Indexes support the efficient execution of queries in MongoDB. Without indexes, MongoDB must perform a collection scan
- If an appropriate index exists for a query, MongoDB can use the index to limit the number of documents it must inspect.
- Indexes are special data structures that store a small portion of the collection's data set . The index stores the value of a specific field or set of fields, ordered by the value of the field. The ordering of the index entries supports efficient equality matches and range-based query operations.

#### Single Index

- `db.collectionName.createIndex({age:1})` - create an index for age document field in asending order.
- `db.collectionName.createIndex({age:-1})` - create an index for age document field in descending order. 
- `db.collectionName.dropIndex("index_name_generated")` - drop the index created.
- `db.collectionName.getIndexes()` - to see all the indexes created.
- `db.collectionName.createIndex({age:1},{unique:true})` - onlt unique ages will be saved.

#### Compound Index

- `db.collectionName.createIndex({age:1, gender:1})` - create compound Index for age and then gender document field order matters.
- only the age scan , age and gender scan will be index scan but only gender scan will be collection scan.

#### Multi Key Index
- A multi-key index is an index that can be created on an array field

#### Text Index
- `db.collectionName.createIndex.({bio:"text"})`
- `db.student.find({$text:{$search:"youtuber"}})` 

#### Partial Filters

- `db.teachers.createIndex({age: 1}, {partialFilterExpression:{age:{$gt:22}} } )` - create index only for age greater then 22.

#### In case of multiple Indexes
- MongoDB checks the performanceof index on a sample of documents once the queries is run and set it as Winning plan.
- Then for second query of similar type it doesn't race them again.
- It store that winning plan in cache

#### To not lock other queries
- db.collectionName.createIndex({name:"text"}, {background:true})

### When not to use indexes.
- When the collection is small.
- When the collection is frequently updated.
- When the queries are complex (multiple fields)
- When the collection is large ( make less indices).

### Ordered Option in Insert

```
if there is any error in any one of the document others will be added without any problem.
db.collectionName.insertMany([{},{},{}], {ordered:false});

```
### Schema Validation

```
Add Validation
db.createCollection.(collectionName, {
    validator:{
        $jsonSchema:{
            required:['name', 'price'],
            properties:{
                name:{
                    bsonType:'string',
                    description:'must be a string and required'
                },
                price:{
                    bsonType:'number',
                    description:'must be a number and required'
                }
            }
        }
    },
    validationAction:'error' / 'warn'
})

Modify Validation

db.runCommand({
    collMod:collectionName,
    validator:{
        $jsonSchema:{
            required:['name', 'price'],
            properties:{
                name:{
                    bsonType:'string',
                    description:'must be a string and required'
                },
                price:{
                    bsonType:'number',
                    description:'must be a number and required'
                },
                author:{
                    bsonType:'array',
                    description:'must be an array and is required',
                    items:{
                        bsonType:'object'
                        requires:['name', 'email'],
                        properties:{
                            name:{
                                bsonType:'string',
                            },
                            email:{
                                bsonType:'string'
                            }
                        }
                    }
                }
            }
        }
    }
})
```

### Atomicity in mongodb

- if the insertion is taking place then if the 10 documents are there 5 have been inserted and 5 are left, then its ok 5 will be inserted and will not rollback this means if insertion was taking place then that document will not be inserted because atomicity is achieved at document level.

### $exists and $type operator

- `db.collectionName.find({hasMacbook : {$exists: true, $type: 8}})`

Double                    1 "double"
String                    2 "string"
Object                    3 "object"
Array                     4 "array"
Binary data               5 "binData" 
Objectld                  7 "objectld"
Boolean                   8 "bool"
Date                      9 "date"
Null                      10 "null"
Regular Expression        11 "regex"


### Evaluation Query Operators

- `$expr` - Allows use of aggregation expressions within the query language.
```
db.collectionName.find({
    $expr:{
        $gt:["$field1", "$field2"]
    }
})

This will find all the documents in the collection where the value of field 1 is greater than value of field 2.
```

- `$jsonSchema` - Validate documents against the given JSON Schema.
- `$mod `- Performs a modulo operation on the value of a field and selects documents with a specified result.
```

db.collectionName.find({age: {$mod:[2,0]}})
even age people which when divided by 2 gives 0 remainder
```

- `$regex` - Selects documents where values match a specified regular expression.
- `$text` - Performs text search. To use the $text operator, you create a text index on field(s) yom want to search. text index allows Mongo db search for that specific words and phrases in indexed fields and returns documents that match the search criteria.
- `$where` - Matches documents that satisfy a JavaScript expression.



### Array Query Operator

- `db.products.find({products: {$elemMatch:{ quantity: { $gt:ll } , name: "apple" }}})`

### Advanced Update

- `db.collectionName.update({}, {$inc:{age:1}})` - increase age of all students by 1
- `db.collectionName.updateOne({name:"sita"}, {$max:{age:50}})` - increase age of sita only if it is less than 50.
- `db.collectionName.updateOne({name:"sita"}, {$min:{age:23}})` - decrease to 23 only if more than 23.
- `db.collectionName.updateOne({name:'sita'}, {$mul:{age:2}})` - multiply age of sita by 2.
- `db.collectionName.updateOne({name:'sita'}, {$unset:{age:anynumber}})` - age field will be deleted.
- `db.collectionName.updateOne({name:'sita'}, {$rename:{age:"student_age"}})` - changing the column name.
- `db.collectionName.updateOne({name:'Golu'}, {$set:{age:100}}, {upsert:true})` - if the golu is not found then make one.


### Update nested Arrays

- `db.collectionName.updateMany({experience: {$elemMatch:{duration:{$lte:1}}}}, {$set:{"experience.$.neglect":true}})` - first match will be updated
- `db.collectionName.updateMany({experience: {$elemMatch:{duration:{$lte:1}}}}, {$set:{"experience.$[].neglect":true}}, {arrayFilters:[{"e.duration":{$lte:1}}]})` - all match will be updated
- `db.students.updateOne({name:"Ram"}, {$push:{experience:{compane:"Meta", duration:2}}})` - add new document to the experience array.
- `db.students.updateOne({name:"Ram"}, {$addToSet:{experience:{compane:"Meta", duration:2}}})` - add new document to the experience array.
- `db.collectionName.updateOne({name:"Ram"}, {$pop:{experience:1}})` - pop one element from the last.
- `db.collectionName.updateOne({name:"Ram"}, {$pull:{experience:{compane:"Meta", duration:2}}})` - delete the element matching.