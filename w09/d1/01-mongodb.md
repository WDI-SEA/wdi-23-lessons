![](https://i.imgur.com/vzWk7s4.png)

# Intro to MongoDB

| Learning Objectives |
| :--- |
| Describe how MongoDB is Different from a RDBMS |
| Start and Stop the MongoDB Engine |
| Save and Retrieve MongoDB Documents using the Mongo Shell |
| Describe Embedding & Referencing |

## Roadmap
- MongoDB vs. Relational SQL Databases
- More About MongoDB
- Installing and Starting MongoDB
- Creating a Database and Inserting Documents
- Data Modeling in MongoDB
- Querying Data
- Updating Data
- Removing Data

## MongoDB vs. Relational SQL Databases

### Terminology

<img src="https://i.imgur.com/XdV3hSs.png" style="width:900px">

As diagramed above, there is a one-to-one mapping of the key concepts of a database.

### Key Differences of MongoDB

#### Schema-less
As you saw when using PostgreSQL, **every** row in a given table has the same number of columns, each with the same datatype.

However, documents (think relational rows) within the same MongoDB collection (think SQL table) can have a varying number of fields (think relational columns).

The fields in a document don't have to be named the same, nor do they need to contain similar data. In fact, a field with the same name can hold different types of data - not that it would be recommended to do this.

#### No Table Joins

In a relational DB, we break up related data into separate tables. These separate tables are later "joined" as necessary using SQL.

In MongoDB, there are no joins. Related data, for example, a _post_ has many _comments_, is often _embedded_ in a single document (example later).  We'll also see another way of modeling associations between data using _referencing_.

The supporters of MongoDB highlight the lack of table joins as a performance advantage since joins are expensive in terms of computer processing.

> Note: MongoDB version 3.2 has introduced a `$lookup` operator for performing aggregations that works a lot like a join.

## More About MongoDB

#### What is MongoDB?

MongoDB is the [dominant player](http://db-engines.com/en/ranking) in the world of data stores known as _NoSQL databases_, also called _document databases_. NoSQL databases do not model or store data in the tabular relations we became familiar with in unit two.

MongoDB puts the "M" in the MEAN/MERN Stack, a technology stack that emphasizes the use of JavaScript in both the front-end and back-end.

Instead of _SQL_ (_Structured Query Language_), MongoDB uses JavaScript as it's native language for database operations.

You're going to see that working with **data** in MongoDB is like working with JavaScript objects.

Although MongoDB is ideal when working in the world of full-stack JavaScript, it can be used with a multitude of programming languages and frameworks, including Python and Django, and Ruby on Rails.

**Why is there no need for migrations in MongoDB?**

#### Hu-**mongo**-us Use Case

NoSQL databases are heavily used in big data and social media applications.  

They can store and retrieve vast amounts of data more easily than a relational DB can.

They are also very useful in applications where the data is "unstructured" or object oriented. 

Also, MongoDB is often used to prototype applications because an application's database can more easily adapt as the application's requirements change. 

However, as great as NoSQL databases are for real-time big and unstructured data, they are not ideal for applications that require a high level of transactional support. For example:

> Consider what happens when a stock is bought/sold at a stock exchange. The buy/sell event would impact several tables in the DB that are used to track the transaction, customer accounts/balances, the equity's ownership, etc.

**What could happen if all the information related to the purchase/sale of a stock was not updated at the _exact_ same time or if there was a hardware failure before all of the tables could be updated?**

Ensuring this point-in-time consistency across multiple data tables is a cornerstone of a RDBMS like PostgreSQL.

MongoDB is designed to maintain consistency at the _document_ level only. If we needed to ensure that more than one document, such as the documents representing the stock, the buyer's account and the seller's account, are updated simultaneously, it would take a lot of code and some hoops to jump through to make this happen in MongoDB.  Whereas this "transactional" support is the bread and butter of a RDBMS.

#### Review Questions

- **Explain the differences between an RDBMS and a NoSQL database?**
- **A MongoDB database consists of _____________.**
- **A MongoDB document consists of _____________.**
- **A MongoDB collection consists of _____________.**

### Data Format

Okay, so in MongoDB, we retrieve _documents_ from a _collection_. 

Lets take a look of what a MongoDB _document_ might look like:

```js
{
    _id: ObjectId("5099803df3f4948bd2f98391"),
    name: { first: "Alan", last: "Turing" },
    birth: ISODate("1912-06-23T00:00:00Z"),
    death: ISODate("1954-06-07T00:00:00Z"),
    contribs: [ "Turing machine", "Turing test", "Turingery" ],
    views: 1250000
}
```

__What does this data structure look very similar to?__

<br>

A MongoDB _document_ is very much like JSON, except it is stored in the database in a format known as _BSON_ (_Binary JSON_).

_BSON_ basically extends _JSON_ with additional data types, such as __ObjectID__ and __Date__ shown above.

### The Document `_id`

The `_id` is a special field represents the document's _primary key_.

It must always be unique.

We _could_ explicitly set the `_id` like this:

```js
{
	_id: "B123",
	desc: "5 Gallon Bucket",
	price: 7.99
}
```

However, it's much more common to allow MongoDB to create it implicitly for us.

MongoDB uses the _ObjectID_ data type for the value of `_id`.  This data type in the database is stored as the binary representation of a string.  See the Alan Turing document above.

A few review questions before we move on:

1. **What is the name of the field that serves as a document's primary key?**
2. **Would it be possible to store the following two documents in the _same_  collection?**<br>

	```js
	{
		_id: ObjectId("5099803df3f4948bd2f23456"),
		make: "Toyota",
		model: "Prius",
		year: 2016
	}
	```
	
	```js
	{
		_id: 123456,
		item: "Hamburger",
		cheese: true
	}
	```

## Installing and Starting MongoDB

### Installation

You may already have MongoDB installed on your system, lets check in terminal `$ mongod` (note the lack of a "b" at the end").

If you receive an error, lets use _Homebrew_ to install MongoDB:

1. Update Homebrew's database (this might take a bit of time)<br>`$ brew update`
2. Then install MongoDB<br>`$ brew install mongodb`
3. Now create the directory where your data will be stored<br>`$ mkdir -p /data/db`<br>be sure you include the leading slash.
4. Get your username<br>`$ whoami`
5. Grant permission<br>`$ sudo chown -R <your username> /data/db`

### Updating MongoDB

If you do have MongoDB installed, it can be upgraded to its latest version:

1. Update Homebrew's database (this might take a bit of time)<br>`$ brew update`
2. Then upgrade MongoDB<br>`$ brew upgrade mongodb`

### Start Your Engine!

`mongod` is the name of the actual process for the database engine. The installation of MongoDB does not set the mongoDB engine to start automatically when our systems boot up.

A common source of errors when starting to work with MongoDB is forgetting to start the database engine. So, be sure to check that you have the engine running in a terminal window if you get an error.

To start the database engine, type `mongod` in terminal.

MongoDB's default port is 27017.

### Stopping the MongoDB Engine

Press `control-c` to stop the engine.

It's important to stop Mongo's database engine **before** closing the terminal window.  Otherwise, you will need to manually shut down the process with this command:

```
$ pgrep mongo
31132
$ kill 31132
```

## Creating a Database and Inserting Documents

### Before we Start

In this lesson, we are going to be working directly with MongoDB to create and modify data using the _Mongo Shell_ application in a Terminal window. This is similar to how you were introduced to PostgreSQL via the _psql shell_.

You will find the Mongo Shell handy for examining and modifying data while developing your application.

However, within your application, you will be using software called _Mongoose_, which will make connecting to, and working with data in MongoDB much easier - similar to how _ActiveRecord_ made working with PostgreSQL much easier.

### The Mongo Shell

MongoDB installs with a client app, a JavaScript-based shell, that allows us to interact with MongoDB directly.

Make sure that the MongoDB engine (`mongod`) is running then...

Start the app in terminal by typing `mongo`

The app will load and change the prompt will change to `>`

List the shell's commands available: `> help`

Show the list of databases: `> show dbs`

Show the name of the currently active database: `> db`

Switch to a different database: `> use [name of database to switch to]`

Let's switch to the `local` database: `> use local`

Show the collections of the current database `> show collections`

### Creating a new Database

To create a new database in the Mongo Shell, we simply have to _use_ the database.  Lets create a database named _myDB_:

```
> use myDB
```

### Inserting Data into a Collection

This is how we can create and insert a document into a collection named _people_:

```
> db.people.insert({
... name: "Fred",	// Don't type the dots, they are from the 
... age: 21			// shell, indicating multi-line input mode
})
```
Using a collection for the first time creates it!

__YOU DO: Let's add another person to the _people_ collection. But this time, add an additional field called _birthDate_ and assign it a date value with something like this: *birthDate: new Date('3/21/1981')*__

Remember, documents in Mongo are schema-less. Each document within a collection can contain **completely** different data.  This is in direct contrast to SQL databases which have schemas where contains the same type of data.

To list all documents in a collection, we can use the _find_ method on the collection without any arguments:

```
> db.people.find()
```

We can also provide an optional _query object_ to specify criteria.  If we provide an empty query object, `find` will once again return all documents:

```
> db.people.find({})
```

#### Plant the Seed and Watch your Data Grow

To practice querying our database, here are few more documents to put in your _people_ collection.

In addition to a single document, we can also provide an __array__ to the _insert_ method and it will create a document for each object in the array.

```js
db.people.insert(
	[
		{
			"name": "Emma",
			"age": 20
		},
		{
			"name": "Ray",
			"age": 45
		},
		{
			"name": "Celeste",
			"age": 33
		},
		{
			"name": "Stacy",
			"age": 53
		},
		{
			"name": "Katie",
			"age": 12
		},
		{
			"name": "Adrian",
			"age": 47
		}
	]
)
```

Be sure to include the closing paren of the _insert_ method.

#### Review Questions

- **How do we "create" a new collection?**

- **What method adds documents to a collection?**

## Data Modeling in MongoDB

There are two ways to model related data in MongoDB:

- via __embedding__ (entire "document", aka sub-document, is contained within the related document)
- via __referencing__ (a document contains the related document's `ObjectId` only)

Both approaches can be used simultaneously in the same document.

### Embedded Documents

In MongoDB, by design, it is common to __embed__ related data in the parent document.

Modeling data with the __embedded__ approach is different than what we've seen in a relational DB where we spread our data across multiple tables.

However, MongoDB works best when related data is embedded. Embedded data allows MongoDB to read and return large amounts of data far more quickly than a SQL database that requires join operations.

To demonstrate __embedding__, we will add another person to our _people_ collection, but this time we want to include contact info. A person may have several ways to contact them, so we will be modeling a typical one-to-many relationship: A Person has many Contacts / A Contact belongs to a Person

Let's walk through this command by entering it together:

```
> db.people.insert({
... name: "Manny",
... age: 33,
... contacts: [
... {
... type: "email",
... contact: "manny@domain.com"
... },
... {
... type: "mobile",
... contact: "(555) 555-5555"
... }
... ]
... })
```

The embedded data objects, like we see above, are called _subdocuments_. When we start working with Mongoose, these subdocuments will automatically have their own `_id`s!

The above approach of embedding "contact" data provides a great deal of flexibility in what types and how many contacts a person may have.

Another example of a data model where embedding works well is the typical a _post_ has many _comments_ scenario. Embedding comments within a _post_ document works great because those comments don't make sense without the post that they belong to.

### Referencing Documents (linking)

We can model data relationships using a __references__ approach where related data is stored in separate documents. These documents, due to the fact that they hold different types of data, are likely be stored in separate collections for organizational purposes (MongoDB doesn't care, but we might).

Think of a _bankAccounts_ collection to demonstrate the __references__ approach. Referencing a _bankAccount_ from a _person_ document may make more sense than embedding it because often more than one person can have access to the same _bankAccount_. If the _bankAccount_ were embedded, any update to the _bankAccount_, such as it's balance, would have to be repeated on every _person_ document with that _bankAccount_.

Consider also that documents can reference more than a single document with the use of arrays of ObjectIds.  For example, we could have an _accounts_ field that is an array on the person documents.

Again, because there are no "joins" in MongoDB, retrieving a person's bank account information would require a separate query on the _bankAccounts_ collection.

You can put the linking field on the _bankAccount_ documents, the _person_ documents, or both! It's your call!  **How is this different than what we saw in PostgreSQL?**

**What is the downside to including the references on both sides of the relationship?**

These decisions depend upon the design and functionality of your application and they are not always black-and-white. There are plenty of tutorials/videos discussing the pros and cons of embedding vs. referencing...

### Data Modeling Best Practices

MongoDB was designed from the ground up with application development in mind. More specifically, what can and can't be done in regards to data is enforced in your application, not the database itself (like in a SQL database).

Here are a few things to keep in mind:

- For performance and simplicity reasons, lean toward _embedding_ over _referencing_.
- Prefer the _reference_ approach when the amount of child data is unbounded and there is a danger of exceeding the 16MB size limit for a document - an uncommon situation however - the entire body of work of Shakespeare can be stored in 5 megabytes!
- Prefer the _reference_ approach when multiple parent documents access the same child document and that child's data changes frequently. This avoids having to update redundant data in multiple locations.
- Obtaining _referenced_ documents requires multiple queries by your application instead of a single query when using _embedding_ - this is why _embedding_ is much more performant.
- In the _references_ approach, depending upon your application's needs, you may choose to maintain links to the related document's *_id* in either document, or both.

Also, if your data model calls for using _referenced_ data, _Mongoose_ will make it easier to implement using the _populate_ method.

For more details regarding data modeling in MongoDB, start with [this section of mongoDB's documentation ](http://docs.mongodb.org/manual/core/data-modeling-introduction/) or this [hour long YouTube video](https://www.youtube.com/watch?v=PIWVFUtBV1Q)

## Querying Data

>PSA: We are going to take a look at MongoDB's native query methods.  However, as fun as it is, in our applications, we will be using a very popular ODM, _Mongoose_.  Many of Mongoose's methods are similar to Mongo's, so it's still worth paying attention :)
<br>

We've seen how to retrieve all of the documents in a collection using the `find()` method.

We can also use the `find()` method to query the collection by passing in an argument containing our query criteria as an JS object:

```
> db.people.find( {name: "Manny"} )
```

Here's how we can use MongoDB's `$gt` query operator to return all _people_ documents with an age greater than 20:

```
> db.people.find( {age: { $gt: 20 } } )
```

MongoDB comes with a slew of built-in [query operators](http://docs.mongodb.org/manual/reference/operator/query/#query-selectors) we can use to write complex queries.

__YOU DO:  Retrieve people that are less than or equal to age 30?__

In addition to selecting which data is returned, we can modify how that data is returned by limiting the number of documents returned, sorting the documents, and by projecting which fields are returned.

This sorts our age query and sorts by _name_:

```
> db.people.find( {age: { $gt: 20 } } ).sort( {name: 1} )
```
The "1" indicates ascending order. 

[This documentation](http://docs.mongodb.org/manual/core/read-operations-introduction/) provides more detail about reading data.

>Notice that the `query object`, is just that, a JS object with key:value pairs.

## Updating Data

In MongoDB, we use the `update()` method on a collection.

We will need to specify the _update criteria_ (like we did with `find()`), and use the `$set` action to set the new value.

```
> db.people.update( { name: "Manny" }, { $set: { age: 99 } })
```

By default `update()` will only modify a single document. However, with the `multi` option, we can update all of the documents that match the query.

```
> db.people.update( { name: { $lt: "M" } }, { $inc: { age: 10 } }, { multi: true } )
```
We used the `$inc` update operator to increase the existing value.

Here is the [list of Update Operators](http://docs.mongodb.org/manual/reference/operator/update/) available.

__YOU DO: add another Contact to our person document named "Manny". Hint: you will need to use the link above to discover the Array Update Operator to use for this__

## Removing Data

We use the `remove()` method to data from collections.

If you want to completely remove a collection, including all of its indexes, use `[name of the collection].drop()`.

Call `remove({})` on the collection to remove **all** docs from a collection.

Otherwise, specify a criteria to remove all documents that match it:

```
>db.people.remove( { age: { $lt: 16 } } )
```
__YOU DO: Choose a person document by name and remove them from the collection.__

## Exercise

Create a file named `mongo.js` and type in a JS object that represents a MondoDB document that would model the following scenario using embedding:

- A `hotel` has `name` and `location` attributes.
- The `hotel` also has a `rooms` attribute that is an array of room objects.
- Embed at least 3 room objects, each with the following attributes: `roomNo`, `rate` and `vacant`.

We'll review in 10 minutes.

## Essential Questions

**SQL Tables are represented in MongoDB with ______?**

**While in MongoDB's shell, what command would we enter to retrieve all of the documents from a collection named _books_?**

**Assuming a document is referencing other documents, and the fact that MongoDB does not have SQL joins, what would we have to do to retrieve those referenced documents?** 

## References

[MongoDB homepage](https://www.mongodb.org/)

[mLab - MongoDB Cloud Hosting](https://mlab.com/)

[MongooseJS - ODM](http://mongoosejs.com/)

