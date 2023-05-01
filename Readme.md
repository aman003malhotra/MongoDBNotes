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

### References 
- References store the relationships between the data by including links or references from one document to another. Applications can resolve these references to access the related data. Broadly, these are normalized data models.

