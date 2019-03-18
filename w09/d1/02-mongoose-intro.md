
[click here to view as a presentation](https://presentations.generalassemb.ly/3f360aafb3cc65751dea4c02093322af#/1)

---

# Intro To
<br>
<img src="https://i.imgur.com/cD5R8OG.png" width="900px">

---

# Learning Objectives
<br>

- Describe the use case for Mongoose

- Define a basic Schema for a single Model

- Create and Read documents using a Model

- Define validations in a Schema

---

# Roadmap
<br>

1. Intro to Mongoose
1. Including Mongoose in an app
1. Defining Schemas in Mongoose
1. Built-in Types for Properties
1. Compiling Schemas into Models
1. Use a Model to Create data
1. Use a Model to Read data
1. Defining validations for a Property
1. Essential Questions

---
# Intro to Mongoose
---
## Intro to Mongoose
<br>

- What is Mongoose?

- Sneak peak of some Mongoose code

- The big picture

---
### What is Mongoose?

---
<p>Yes, this guy, but not in the context of MongoDB...</p>

<img src="https://i.imgur.com/Y74xxoD.jpg" width="900">

---
### What is Mongoose?
<br>

- _Mongoose_ is to MongoDB as Sequelize is to a SQL database.

-  But, because it maps code to MongoDB _documents_, it is referred to as an **Object Document Mapper (ODM)** instead of an ORM.

- Mongoose is going to make it easier to perform CRUD using object-oriented JS instead of working directly MongoDB.

---
### What is Mongoose? (cont.)
<br>

- Using the Mongoose ODM is by far the most popular way to perform CRUD on a MongoDB.

- Let's check out the landing page for Mongoose and see what it has to say for itself...

	<a href="http://mongoosejs.com/index.html" target="_blank">Mongoose Homepage</a>

---
### What is Mongoose? (cont.)

- So, Mongoose's homepage says it best:

> "Mongoose provides a straight-forward, schema-based solution to model your application data"

- Wait a second, what's with this "schema" business, isn't MongoDB schema-less?  

- Well, yes it is, however, the vast majority of applications benefit when their data conforms to a defined structure (schema).

- Mongoose allows us to define schemas and ensures that documents conform.

---
### What is Mongoose? (cont.)
<br>

- Mongoose also provides lots of other useful functionality:
	- Default property values
	- Validation
	- Automatic related model population via the `populate` method
	- _Virtual properties_ - create properties like "fullName" that are not persisted in the database
	- Custom _Instance methods_ which operate on the document
	- _Static methods_ which operate on the entire collection 
	- `pre` and `post` event lifecycle hooks (Mongoose "middleware")

---
### Sneak peak of some Mongoose code
<br>

- For a preview of what Mongoose does, let's review the small amount of code shown on the Mongoose homepage...

---

```js
var mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/test');

var Cat = mongoose.model('Cat', { name: String });

var kitty = new Cat({ name: 'Zildjian' });
kitty.save(function (err) {
  if (err) {
    console.log(err);
  } else {
    console.log('meow');
  }
});
```

<p>So, the big picture is...</p>

---
### The Big Picture 
<br>

- Here is the big picture overview of the components we'll be working with:

<img src="https://i.imgur.com/Q6A7KTQ.png" width="900">

---
### Big Picture Example 

- Assuming the following schema:

	```js
	var postSchema = new mongoose.Schema({
		content: String
	});
	```

- It can be compiled into a model and that model exported like this:

	```js
	module.exports = mongoose.model('Post', postSchema);
	```

- The model can then be required and used to perform CRUD on the `posts` collection in the MongoDB:

	```js
	var Post = require('./models/post');
	Post.create({content: 'Amazing post...'});
	```

---
## Review Questions
<br>

- **In your own words, describe the use case for Mongoose (what is it's purpose and when might you choose to use it?).**

- **A Mongoose _________ is compiled into a Mongoose Model.**

- **We use a Mongoose  _________ to perform CRUD operations on a MongoDB.**.

---
# Including Mongoose<br>in an App
---
## Including Mongoose in an App
<br>

- Create an Express app

- Install Mongoose

- Configure Mongoose in a module

- Add event listeners to the Mongoose connection

---
### Create an Express App

### Install Mongoose
<br>

- Installing the Mongoose package is straight forward:

	```sh
	$ npm i mongoose
	```
	Note: `i` is a shortcut for `install`

- Add the code to connect Mongoose to MongoDB

	```js
	var mongoose = require('mongoose');
	// It used to be necessary to set the mongoose library to avoid
	// warnings. Not anymore with ver. 5.x
	// mongoose.Promise = Promise;

	mongoose.connect('mongodb://localhost/movies',
	    {useNewUrlParser: true}
	);
	```

- The `{useNewUrlParser: true}` option only avoids a deprecation warning and is not required.

---
### Configure Mongoose
<br>

- Time to check if our app starts up without errors:

	- Ensure that the MongoDB engine is running in a separate Terminal session:<br>`$ mongod`
	- Start our app:<br>`$ nodemon`
	- Browse to:<br>`localhost:3000`

- No errors?  Great!  However, wouldn't it be nice to know that our connection to our database was successful?  Sure it would...

---
### Adding event listeners to the Mongoose connection
<br>

- The Mongoose connection object inherits from Node's `EventEmitter` which allows us to listen to defined events.

- Let's listen to the `open` and `error` events...

---
### Adding event listeners (cont.)

- Let's modify our _database.js_ module as follows:

	```js
	var mongoose = require('mongoose');
	mongoose.connect('mongodb://localhost/movies');
	
	// shortcut to mongoose.connection object
	var db = mongoose.connection;
	
	db.once('open', function() {
  		console.log(`Connected to MongoDB at ${db.host}:${db.port}`);
	});
	
	db.on('error', function(err) {
  		console.error(`Database error:\n${err}`);
	});
	```

- Now check it out with both the MongoDB engine running and not running (to trigger an error).

---
## Defining Schemas in Mongoose
<br>

- Create a module for the Schema/Model

- Define a basic Schema for a `Movie` model

---
### Create a module for the Schema/Model
<br>

- Now that we are connected to the MongoDB engine, it's time to define our first schema.

- So, where are we going to put our app's schemas and models?  In their own folder - of course!

- The MVC design pattern influences our code organization:

	```sh
	$ mkdir models
	$ touch models/movie.js
	```

---
### Define a basic Schema for a _Movie_ model

- It is customary to have a single file per Mongoose Model where we define and compile its schema.

- In our schema/model files, we will always do this:

	```js
	var mongoose = require('mongoose');
	// optional shortcut to the mongoose.Schema class
	var Schema = mongoose.Schema;
	```

- Creating the shortcut to the `mongoose.Schema` class is optional but convenient when defining complex schemas.

- Now let's define our schema...

---
### Define a basic Schema (cont.)
<br>

- Here's our basic _Movie_ schema:

	```js
	var Schema = mongoose.Schema;
	
	var movieSchema = new Schema({
  		title: String,
  		releaseYear: Number,
  		rating: String,
  		cast: [String]
	});
	```

- Note the `cast` property's type is an Array of Strings.

---
### Define a basic Schema (cont.)
<br>

- Vocab note:
	- A **property** may be referred to as a "**path**", or "**field**".

---
### Define a basic Schema (cont.)
<br>

- What we have defined is a very basic schema. Later we will see much more complex schemas.

- For now, let's take a look at the eight built-in types available...

---
## Built-in Types for Properties
<br>

- The types that we can assign to properties are known as `SchemaTypes`

- There are several built-in types that we can specify for our properties:
	- **String**
	- **Number**
	- **Boolean**
	- **Date**
	- **mongoose.Schema.Types.ObjectId**
	- **Array - []** 

---
## Compiling Schemas into Models

#### **Remember - Models,<br>not schemas are used to perform CRUD**

---
### Compiling Schemas into Models

- Mongoose performs CRUD using a **Model**.

- Compiling a schema into a model is as easy as calling the `mongoose.model` method:

	```js
	var Schema = mongoose.Schema;
		
	var movieSchema = new Schema({
  		title: String,
  		releaseYear: Number,
  		rating: String,
  		cast: [String]
	});
	
	// Compile the schema into a model and export it
	module.exports = mongoose.model('Movie', movieSchema);
	```

---
### Compiling Schemas into Models
<br>

- **There is a one-to-one mapping between Mongoose models and MongoDB collections**.

- By default, the collection will be named as the pluralized version of the model in all lower-case.

- The collection name can be overridden when compiling the model, but it's uncommon to do so.

---
### Use a Model to Create data
<br>

- Now that we have a model, we're ready to perform some CRUD!

- First up is **creating** data.

- We have two ways to create documents:
	- `new <Model>()`, then `<model instance>.save()`
	- `<Model>.create()`

- Let's see how we can `create` a document in Node's REPL...

---
### Use a Model to Create data (cont.)
<br>

```sh
$ node
> require('./config/database')
> var Movie = require('./models/movie')
> Movie.create({
... title: 'Star Wars',
... releaseYear: 1977
... }, function(err, doc) {
... console.log(doc);
... })
```

---
### Use a Model to Create data (cont.)

```js
{ __v: 0,
  title: 'Star Wars',
  releaseYear: 1977,
  _id: 57ea692bab09506a97e969ba,
  cast: [] }
```

- The `__v` field is added by Mongoose to track versioning - ignore it.

- Note that although we did not supply a value for the `cast` property, it was initialized to an array - ready to have cast members pushed into it!

---
### Use a Model to Read data
<br>

- The querying ability of Mongoose is very capable.  For example:

	```js
	Movie.find({rating: 'PG'})
		.where('releaseYear').lt(1970)
		.where('cast').in('John Wayne')
		.sort('-title')
		.limit(3)
		.select('title releaseYear')
		.exec(cb);
	``` 

- But we're going to start with the basics :)

---
### Use a Model to Read data (cont.)

- Here are the useful methods on the model for querying data:
	- `find`: Returns an array of all documents matching the _query object_
		
		```js
		Movie.find({rating: 'PG'}, function(err, movies) {...
		```
		
	- `findById`: Find a document based on it's `_id`
	
		```js
		Movie.findById(req.params.id, function(err, movie) {...
		```

	- `findOne`: Find the first document that matches the _query object_

		```js
		Movie.findOne({releaseYear: 2000}, function(err, movie) {...
		```

---
### Timestamps in Mongoose

- Mongoose will add `createdAt` and add/update `updatedAt` fields if we set the `timestamps` option as follows in the schema:

	```js
	var movieSchema = new mongoose.Schema({
	  title: String,
	  releaseYear: {
  		 type: Number,
  		 default: function() {
  			return new Date().getFullYear();
  		 }
	  },
	  ...
	}, {
	  timestamps: true
	});
	```

---
### Defining validations for a Property
<br>

- Validations are used to prevent bogus data from being saved in the database.

- There are several built-in validators we can use.

- However, endless flexibility is possible with custom asynchronous and synchronous validator functions and/or Mongoose middleware.

- We'll keep it simple :)

---
### Defining validations for a Property (cont.)
<br>

- Movies should not be allowed to be created without a `title`.  Let's make it required:

	```js
	var movieSchema = new mongoose.Schema({
	  title: {
	    type: String,
	    required: true
	  },
	...
	```
- Now, if we try saving a movie without a `title` an error will be set.

---
### Defining validations for a Property (cont.)
<br>

- For properties that are of type _Number_, we can specify<br>a `min` and `max` value:

	```js
	var movieSchema = new mongoose.Schema({
	  ...
	  releaseYear: {
	    type: Number,
	    default: function() {
	      return new Date().getFullYear();
	    },
	    min: 1927
	  },
	  ...
	```

- No more silent movies!

---
### Defining validations for a Property (cont.)
	
- For properties that are of type _String_, we have:
	- **`enum`**: String must be in the provided list
	- **`match`**: String must match the provided regular expression
	- **`maxlength`** and **`minlength`**: Take a guess :)

- Here is how we use the `enum` validator:

	```js
	var movieSchema = new mongoose.Schema({
	  ...
	  rating: {
	    type: String,
	    enum: ['G', 'PG', 'PG-13', 'R']
	  },
	  ...
	```

---
### Summary
<br>

- Mongoose is the go to when it comes to working with a MongoDB.

- We define Mongoose **schemas**, which are then compiled using the `mongoose.model` method into **Models**.

- We use a Model to perform all CRUD for a given MongoDB collection.

---
# Essential Questions
<br>

<p>Take a couple of minutes to review before you get picked</p>

- True or false:  A document's structure is defined in a Mongoose model.
- Can a single Model be used to query more than one MongoDB collection?
- What line of code would compile a `drinkSchema` into a model named `Drink`?
- What do we export from the module that contains the schema definition and the compiled model?

---

# References
<br>

- [Official MongooseJS Documentation](http://mongoosejs.com/)


