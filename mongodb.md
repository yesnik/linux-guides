# MongoDb

## Installation

See [docs](https://www.mongodb.com/docs/manual/installation/)

## Console Commands

Run MongoDb CLI in the terminal:

```
mongosh
```

Commands:

- `show databases` / `show dbs` - show available databases
- `use local` - switch to `local` DB
- `use mydb` - create `mydb` database. MongoDB only creates the database when you first store data in that database.
- `db.students.insertOne({name: "Kenny", age: 18})` - insert document in the `students` collection
- `db` - show the name of the current database
- `show collections` / `show tables` - show collections for current database

## Collections

*Collections* in MongoDB are like tables in a SQL database, but they are groups of documents rather than groups of records.

Collections can be created:

- by writing a document to them
- with `createCollection` command.

`WriteResult({ "nInserted" : 1 })` indicates that the document was added to the collection.
