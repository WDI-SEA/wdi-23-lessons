# Async

## Objectives

* Identify situations where the `async` module would be needed
* Utilize `async` control flow functions for asynchronous function calls

## What is Async?

It helps to know the dictionary definitions of synchronous and asynchronous:

_[synchronous](https://www.merriam-webster.com/dictionary/synchronous): happening, existing, or arising at exactly the same time_

_[asynchronous](https://www.merriam-webster.com/dictionary/asynchronous): not simultaneous or concurrent in time_ 

Async is a JavaScript module available for Node.js and the browser. It provides a set of utilities for working with functions asynchronously. We'll be focusing our attention to Async's control flow functions, mainly `series`, `parallel`, `concat`, and `waterfall`.

## Why Use Async?

There are situations where functions will need to be run without conflicting with each other. This is one of the benefits/drawbacks of asynchronous behavior in JavaScript. A great example is data scraping multiple pages. If you were to data scrape pages using a loop, the requests would all run at the same time.

```js
var request = require('request');

var urls = ['http://www.google.com', 'http://www.yelp.com', 'http://www.twitter.com'];
var allData = [];

//this would work correctly to a point, but would be disastrous if there were, say, 1000 URLs in the urls array
urls.forEach(function(url) {
  request(url, function(err, response, data) {
    allData += data;
  });
});

//this would not work correctly
console.log(allData);
```

Using the `async` module will allow us to control the flow of how these functions are run, and when we can access all of the data.

## Using Async

First, let's install the `async` module.

```
npm install async
```

### Series

The `.series` function can pass in an array of functions, along with a callback function to execute once the array of functions is completed. Each function in the array takes in a callback function as a parameter, then executes the callback when the function is finished (passing an error and results). Functions will be executed in order until completed, then the final callback is executed.

```js
const async = require('async');

// functions
function seriesFunc1(callback) {
  console.log(1);
  callback(null,1);
}

function seriesFunc2(callback) {
  console.log(2);
  callback(null,2);
}

function seriesFunc3(callback) {
  console.log(3);
  callback(null,3);
}

let seriesFuncs = [seriesFunc1,seriesFunc2,seriesFunc3]

// call async.series on our array of functions
async.series(seriesFuncs, function(err, results) {
  console.log('SERIES DONE')
  console.log(results);
})


```
___
### Parallel

The `.parallel` function also accepts an array of functions, along with a callback function. However, the functions aren't executed one after another. They start in parallel, and the final callback is executed once the array of functions is done executing. Note that the functions are started, but not executed, in parallel. So if the functions don't take any time to complete, they'll execute in series (just as before).

```js
const async = require('async');

// functions

// 10 second timeout
function paraFunc1(callback) {
  setTimeout(function() {
    console.log(1);
    callback(null, 'HELLO, ');
  }, 10000);
}

// 1 second timeout
function paraFunc2(callback) {
  setTimeout(function() {
    console.log(2);
    callback(null, 'PARA');
  }, 1000);
}

// 5 second timeout
function paraFunc3(callback) {
  setTimeout(function() {
    console.log(3);
    callback(null, 'LLEL');
  }, 5000);
}

let paraFuncs = [paraFunc1,paraFunc2,paraFunc3]

// call async.parallel on our array of functions
async.parallel(paraFuncs, function(err, results) {
  console.log('PARALLEL DONE')
  console.log(results.join(""));
})
```
___
### Waterfall

The `.waterfall` function is similar to `.series`, but allows arguments to be passed from one function to the next. This is ideal if multiple API calls are being made, and the results of one call are required for the next call.

```js
const async = require('async');

//functions
function waterFunc1(callback) {
  var initial = 55;
  console.log('waterFunc1', initial)
  callback(null, initial);
}

function waterFunc2(num1, callback) {
  num1 += 5;
  console.log('waterFunc2:', num1)
  callback(null, num1)
}

function waterFunc3(num1, callback) {
  num1 += 40;
  console.log('waterFunc3:', num1)
  callback(null, num1);
}

let waterFuncs = [waterFunc1,waterFunc2,waterFunc3]

// call async.parallel on our array of functions
async.waterfall(waterFuncs, function(err, results) {
  console.log('WATERFALL DONE')
  console.log(results);
})
```
___
### Concat

Lastly, `.concat` allows you to pass in an array of values and an iterator (instead of passing in an array of functions). This allows functions to be more reusable, as in the instance of making requests. These functions occur in parallel.

```js
var async = require('async');
var request = require('request');

//urls to make requests to
var urlsToGet = ['https://www.reddit.com/search.json?q=politics',
                 'https://www.reddit.com/search.json?q=puppies',
                 'https://www.reddit.com/search.json?q=drake'];

//create an iterator to get the first title for each URL
var getFirstTitle = function(url, callback) {
  request(url, function(err, response, data) {
    var firstTitle = JSON.parse(data).data.children[0].data.title;
    callback(null, firstTitle);
  });
};

//get each title, then print the results
async.concat(urlsToGet, getFirstTitle, function(err, results) {
  console.log(results);
});
```

___

## Practical Parallel Example

Using `request()`, this example consumes data from the `/users` endpoint of the `jsonplaceholder` API. An array of subsequent functions is generated to consume data from the `/albums?userId` endpoint for each of the users returned from the initial API call and format the data from both into a single object for each user.

`async.parallel` is invoked on the array of requests and returns when 

**user data**:
```js
{
  id: 10,
  name: "Clementina DuBuque",
  username: "Moriah.Stanton",
  email: "Rey.Padberg@karina.biz",
  address: {
    street: "Kattie Turnpike",
    suite: "Suite 198",
    city: "Lebsackbury",
    zipcode: "31428-2261",
    geo: {
      lat: "-38.2386",
      lng: "57.2232"
      }
    },
  phone: "024-648-3804",
  website: "ambrose.net",
  company: {
    name: "Hoeger LLC",
    catchPhrase: "Centralized empowering task-force",
    bs: "target end-to-end models"
  }
}
```
**album data**
```js
{
  userId: 1,
  id: 1,
  title: "quidem molestiae enim"
}
```

**resultObject example**
```js
  { 
    name: 'Clementina DuBuque',
    albumTitles: [ 
      'repellendus praesentium debitis officiis',
      'incidunt et et eligendi assumenda soluta quia recusandae',
      'nisi qui dolores perspiciatis',
      'quisquam a dolores et earum vitae',
      'consectetur vel rerum qui aperiam modi eos aspernaturipsa',
      'unde et ut molestiae est molestias voluptatem sint',
      'est quod aut',
      'omnis quia possimus nesciunt deleniti assumenda sedautem',
      'consectetur ut id impedit dolores sit ad ex aut',
      'enim repellat iste' ] } 
```


**example code**
```js
const async = require('async')
const request = require('request')


const apiURL = 'https://jsonplaceholder.typicode.com/' 


// hit api to pull a collection of users
request(apiURL + "users", function(err, response, body){
  let userData = JSON.parse(body);

  // make array of functions to request album endpoint for each user
  let albumRequests = userData.map( function(user) {

    // return function definition for each unique user
    return function(callback) {
      let albumURL = apiURL + `albums?userId=${user.id}`;
      request(albumURL, function(err, response, body) {
        let name = user.name;

        // map titles from each album object into an array
        let albumTitles = JSON.parse(body).map( function(album){
          return album.title;
        });

        // create object in desired format
        let resultObject = {
          name: name,
          albumTitles: albumTitles
        }

        // invoke `callback` with null & resultObject
        callback(null, resultObject)
      })
    }
  })

  // invoke async.parallel to fire all generated functions from albumRequests array
  async.parallel(albumRequests, function(err, results) {
    console.log(results)
    // results are returned when all functions have been resolved
    // db stuff should go here!
  })
})
```
___

## Wrapup

This is just a sampling of the various utilities included in the `async` module. Some other useful functions include `.forever`, `.until`, `.queue`, and `.times`. For usage samples, see the documentation:

https://github.com/caolan/async