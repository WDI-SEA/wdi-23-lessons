# Testing with Mocha and Chai

## Learning Objectives

1. Explain the benefits of testing and test driven development (TDD).
2. Explain what the Mocha and Chai libraries do in the context of testing.
3. Integrate Mocha and Chai into a Node project.
4. Write and run unit tests using Mocha and Chai.

**"If you haven't tested it, it doesn't work."** - Ancient Computer Science Wisdom

**"If you aren't checking it, it's broken."** - Important Corollary to Ancient Wisdom

## Why Testing?

Software testing is required to identify defects. We really have only two ways we can find out about defects: We can test the software to see if it behaves as designed, or we can deploy it and wait to see what breaks. Clearly, the first method is preferable.

Testing also ensures that our product meets the specifications of those paying for it, our stakeholders. It is very important that we identify any divergence from the customer's order before delivery.

It can also let us know about performance issues and issues that might pop up down the line while we maintain the software. In the software industry, it is absolutely crucial to thoroughly test our apps if we want our business to stay afloat.

## Test-Driven Development

Test-Driven Development or TDD is a relatively new software development paradigm where the tests for the software are written **before** the actual software. Every test should initially fail and our job is to write only enough code as is necessary to make each test pass. By following this methodology, we end up with the minimal amount of code needed to meet all required functionality for our app. It helps reduce code bloat by only including the bare minimum, and this also makes maintenance more efficient by reducing the number of lines that must be maintained.

## Testing Framework

Before we start writing tests, we must choose a testing framework to help us. There are a couple very popular frameworks out there that we could use:

* **Mocha:** A testing framework that integrates nicely with Node projects.
* **Jest:** A testing framework gaining in popularity that has particular strength in testing React components.

We will be learning Mocha today but if you are interested in also learning Jest, you can read more about it [here](https://jestjs.io/).

## Assertion Library

So what do we use Chai for? Chai is an assertion library that basically gives us a bunch of very English-like functions that we can use to write extremely readable tests. While we don't really need it, it does make tests incredibly easy to read even for non-techies.

## Integrating a Test Framework

We are going to make a simple node project with a function to test. We will come up with the criteria that determines whether the function is working correctly and, using this criteria, we will write some tests. Then we will write the code necessary to make all the tests pass.

Go into your unit4 folder and make a new folder named `mocha-testing`. Go into it and run:

```bash
npm init -y
```

With a node project initialized, we will need to install Mocha both globally and in our new project:

```bash
npm i -g mocha
```
```bash
npm i mocha --save-dev
```

(Since mocha is a testing library, we do not include it as a production node module. By saving it as a dev dependency, it will be excluded from the deployment.)

We can also install chai now:
```bash
npm i chai --save-dev
```

Mocha looks for test files by default in a subfolder named `test` inside our project. Let's make that now:

```bash
mkdir test
```

Let's get all of this open in our code editor. Once we see the file structure, create a new file inside the `test` folder named `test.js` and put this code in it:

```js
var assert = require('assert');

describe('Our First Mocha Test', function() {
  it('length prop should be the length of the string', function() {
    assert.equal("javascript".length, 5);
  });
  it('charAt(0) should return first character of the string', function() {
    assert.equal("javascript".charAt(0), 'j');
  });
});
```

Before we run this test, let's take a look at the syntax. We have three new important keywords here:

* **assert** - This is a function that we use to check whether things are the way they are supposed to be. We assert that something should be equal to some value, or should exist, or should return some value, etc. If that assertion is true then the test passes. Otherwise, it fails.
* **describe** - This is the organization function that groups tests together. The first parameter to `describe` is a string describing **which aspect of the software we are testing**. The second parameter is a function containing our tests.
* **it** - This is the actual test function. Each `it` receives a string parameter describing **what the result should be**, and then a function that can run the actual test.

So in our tests above, we are running tests on built-in JavaScript strings. These tests are all inside the `describe` function which is collecting all of my first tests. Inside the `describe` we have two tests, each starting with `it`.

The first test is asserting that a given string has an accurate length value. (If you are paying attention, you will note that this test will definitely fail. Can you see why?)

The second test asserts that the first character of the string will be returned when we call `charAt(0)` on the string, and that this character will be a 'j'.

## Running Tests

To run these tests, we basically just run `mocha`. However, it integrates nicely into node and our npm scripts so we can set up an `npm test` command that will run our tests for us. Open your package.json and update the scripts section:

```js
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1"
},
```

Update the test line to be:

```js
"test": "mocha"
```

Now, at our command prompt, we can run `npm test` and see what happens:

```bash
  Our First Mocha Test
    1) length prop should be the length of the string
    ✓ charAt(0) should return first character of the string

  1 passing (8ms)
  1 failing

  1) Our First Mocha Test
       length prop should be the length of the string:

      AssertionError [ERR_ASSERTION]: 10 == 5
      + expected - actual

      -10
      +5
      
      at Context.<anonymous> (test/test.js:5:12)

  npm ERR! Test failed.  See above for more details.
```

Up at the top of this output, we see a summary of our tests. The first one failed and the second one passed. Past the summary, we see more details about the tests that failed. We see that we were asserting that the length was 5 but it was in fact 10. As a result, the test failed. The second one passed because the `charAt(0)` function returned the correct character.

Now, if we were trying to make these tests pass, we would update our code only as much as necessary in order to make the test pass. But because this is testing built-in JavaScript functions, I can't do much to edit the code.

## Writing an Actual Test

Let's make some tests for a function that translates a string into Pig Latin. Let's jam on some ideas for what tests to make. The best place to start would be to look at what defines Pig Latin so we can adhere to the spec.

> From WikiHow: "To speak Pig Latin, move the consonant cluster from the start of the word to the end of the word; when words begin on a vowel, simply add "-yay", "-way", or "-ay" to the end instead."

We should decide on one of those endings for the purposes of our script. I'm partial to adding "-yay" onto the end. Having decided on that part of the specification, here are some general ideas:

1. It should return a string.
1. It should return a string that ends in "ay".
1. It should return a string that starts with a vowel.

Maybe we should more specific tests as well:

1. Given a string with multiple consonants at the start, it should return a string with all of those consonants at the end, followed by "ay".
1. Given a string that starts with a vowel, it should return a string with "yay" at the end.

That seems like a good start. Let's clear out our test function inside `test.js` and put in the new `describe` call:

```js
var assert = require('assert');
var pigify = require('../pigify');

describe('Pig Latin Translator', function() {
  
});
```

Great, that provides a grouping of all of our tests. Every test we put into this `describe` block should have something to do with our Pig Latin Translator. We also imported something called `pigify`. This will be the name of our function that we will test. The test file will need to "require in" the function so that it can run it for the tests.

Let's try implementing that first test for checking for a string type:

```js
var assert = require('assert');
var pigify = require('../pigify');

describe('Pig Latin Translator', function() {
  it('should return a string', function() {
    assert.equal(typeof pigify('test'), 'string');
  });
});
```

Our first test is checking to make sure the function returns a string. We can use the JavaScript `typeof` keyword to check the type of a variable. We assert that it will be equal to 'string' when we check. If it is not of type `string` then the test will fail. If we run it now, it will fail because we have not yet written the function it is testing.

Add the remaining two basic tests:

```js
var assert = require('assert');
var pigify = require('../pigify');

describe('Pig Latin Translator', function() {
  it('should return a string', function() {
    assert.equal(typeof pigify('test'), 'string');
  });

  it('should return a string ending in "ay"', function() {
    var result = pigify('test');
    assert.equal( result.slice(result.length - 2), 'ay' )
  });

  it('should return a string that starts with a vowel', function() {
    var vowels = ['a', 'e', 'i', 'o', 'u'];
    assert.equal( vowels.includes(pigify('test')[0]), true )
  });
});
```

By looking at these, you can see that the only new syntax with tests are the `describe` and `it` functions. When we are inside a test function, we are using normal JavaScript to run the function and compare its result with a value we are expecting.

## Introducing Chai

So far this is pretty straightforward and the syntax makes a good amount of sense. However, we can make it even better by bringing in Chai which makes our assertions much more readable. At the top of the `test.js` file after the other requires, add these import lines:

```js
var expect = require('chai').expect;
var should = require('chai').should();
```

These give us two of the behavior driven development or BDD tools built into Chai. BDD is very much like TDD but it is typified by much more readable assertions and usually involves testing more than just functionality. Let's see how we can rewrite each of our tests to use the new Chai expressions:

```js
var assert = require('assert');
var pigify = require('../pigify');
var expect = require('chai').expect;
var should = require('chai').should();

describe('Pig Latin Translator', function() {
  it('should return a string', function() {
    // assert.equal(typeof pigify('test'), 'string');
    expect(pigify(test)).to.be.a('string');
  });

  it('should return a string ending in "ay"', function() {
    var result = pigify('test');
    // assert.equal( result.slice(result.length - 2), 'ay' );
    result.slice(result.length - 2).should.equal('ay');
  });

  it('should return a string that starts with a vowel', function() {
    var vowels = ['a', 'e', 'i', 'o', 'u'];
    // assert.equal( vowels.includes(pigify('test')[0]), true );
    expect(vowels).to.include( pigify('test')[0] );
  });
});
```

That's just an example. There are so many little chaining words in Chai that you could rewrite those almost any way you like. For complete Chai documentation with lists and examples of the chains, check out their [website](https://www.chaijs.com/).

Alright, let's think about those last two tests. We could write a very sophisticated algorithm to check those two characteristics. That would take some time. Or we could simply pass in different test data into the function in the tests and assert what the result should be.

We need to make sure that multiple-starting-consonant words have all the consonants moved to the end. We also need to make sure starting-vowel words simply have 'yay' added to the end. Let's think of some words that will help us test that:

Multiple Consonants:
- glove, stamp, schedule

Vowel Starters:
- echo, absent, orange

Now we can use these to test out those conditions. We can even nest another `describe` inside to group these tests together.

```js
var assert = require('assert');
var pigify = require('../pigify');
var expect = require('chai').expect;
var should = require('chai').should();

describe('Pig Latin Translator', function() {
  it('should return a string', function() {
    // assert.equal(typeof pigify('test'), 'string');
    expect(pigify(test)).to.be.a('string');
  });

  it('should return a string ending in "ay"', function() {
    var result = pigify('test');
    // assert.equal( result.slice(result.length - 2), 'ay' );
    result.slice(result.length - 2).should.equal('ay');
  });

  it('should return a string that starts with a vowel', function() {
    var vowels = ['a', 'e', 'i', 'o', 'u'];
    // assert.equal( vowels.includes(pigify('test')[0]), true );
    expect(vowels).to.include( pigify('test')[0] );
  });

  describe('Starting consonant clusters', function() {
    it('should turn "glove" into "oveglay"', function() {
      expect(pigify('glove')).to.equal('oveglay');
    });
    it('should turn "stamp" into "ampstay"', function() {
      expect(pigify('stamp')).to.equal('ampstay');
    });
    it('should turn "schedule" into "eduleschay"', function() {
      expect(pigify('schedule')).to.equal('eduleschay');
    });
  });

  describe('Starting vowels', function() {
    it('should turn "echo" into "echoyay"', function() {
      expect(pigify('echo')).to.equal('echoyay');
    });
    it('should turn "absent" into "absentyay"', function() {
      expect(pigify('absent')).to.equal('absentyay');
    });
    it('should turn "orange" into "orangeyay"', function() {
      expect(pigify('orange')).to.equal('orangeyay');
    });
  });
});
```

## Your Turn

Now that we have all these tests written, let's engage in a little TDD! Make sure everything is saved, go to the terminal and run the tests.

TDD is like a game: You only try to solve one failing test at a time. Write only what you need to make the test pass. Then go on to the next one. It is tempting to try to solve the whole thing in one go, but that starts to minimize the benefits of TDD. By strictly only writing the code to solve one test at a time, you keep it lean and minimalist.

It is okay if you happen to solve more than one test with a single update or even solve one futher down the test list. We aren't trying to prevent tests from passing. Just make sure you only actively work on solving one at a time.

## Types of Testing

What we have done above is called **unit testing**. When we test a single class or function, it is called unit testing and we are writing unit tests. You may also hear about integration testing, end-to-end testing, regressing testing, etc. There are many ways to test an application both in web development and in UX design.

### Route Testing

It is also possible to test the routes we write in Express. For that we can install a node module called `supertest`. It works inside of Mocha tests and looks like this:

```js
var request = require('supertest');

describe('Auth Controller', function() {
  describe('GET /auth/signup', function() {
    it('should return a 200 response', function(done) {
      request(app).get('/auth/signup').expect(200, done);
    });
  });

  describe('POST /auth/signup', function() {
    it('should redirect to / on success', function(done) {
      request(app).post('/auth/signup')
      .set('Content-Type', 'application/x-www-form-urlencoded')
      .send({
        email: 'new@new.co',
        name: 'Brian',
        password: 'password'
      })
      .expect('Location', '/')
      .expect(302, done);
    });
  });
});
```

As we see, we can use supertest to make HTTP requests to any route in our app and assert the status codes that come back. This is handy for testing APIs.

### Testing React

The preferred tools for testing React are `Jest` and `Enzyme`. Jest is another unit testing framework like Mocha. It also works very well for testing React components, seeing if they mount, contain things, etc. Enzyme provides test DOM access so you can effectively test elements in your components, tweak state attributes, simulate user actions, etc. We won't have time to cover these but we highly recommend you look into them post-cohort to expand your testing expertise