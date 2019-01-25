# Intro to DOM and Events

## Objectives
* Define what DOM stands for and what it refers to
* Select elements from the DOM using selectors
* Add events to elements in the DOM

## What is the DOM?

When the broswer loads a webpage, it takes in all the information about what needs to be displayed on the page, and creates a bunch of Javascript objects that represent the elements on the page. These objects mirror the html hierarchy, where the highest order parent is the _document_ object.

![DOM](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSew-BsBdRn9RadAFC2626myd4j66yaFIWzSd6nkdvN-rbg14NX)

**Document Object Model**: A model of all the html elements on a web page - usually represented by a tree of nodes -  that is created by the browser when a web page is loaded.

[DOM on MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)

### Exercise

Visit [this website](https://taylordarneille.github.io/finished-intro-demo/) and open up the dev tools. Look at the Elements tab. Try expanding the collapsed elements to explore the structure of the document more. Now go to the Console tab and run `document` to see the document object. Notice that this is the same hierarchy from the Elements tab. ***The Elements tab of the console shows us a vertical representation of the DOM***

Now draw out a diagram (tree of nodes) that represents the DOM that is loaded by this webpage.

## DOM manipulation

### What is DOM manipulation?

Using Javascript we can read and/or change the DOM, i.e. make the webpage do something interesting.

**Examples:**
* Change or Remove HTML elements
* Create and Add HTML elements
* Change the CSS styles of HTML elements
* Read & Change attributes of HTML elements (href, src, alt, etc)
* Attach event listeners to HTML elements (click, keypress, submit, etc.)

### Why would we want to manipulate the DOM?

Without DOM manipulation, web pages would just be static visuals with no changes and no interaction with the user.

**Example:**
* Clicking on buttons would do nothing
* You could never type into an input field
* You could never zoom into an image or stop/start a video

### How do we manipulate the dom?: 

The *document* object represents the DOM.

#### Selecting DOM Elements by HTML
* document.getElementById()
* document.getElementsByClassName()
* document.getElementsByTagName()

***html***
```html
<div id="hello" class="square">
  Hello, world!
</div>

<div id="gb" class="square">
  Goodbye, world!
</div>
```

***css***
```css
#hello {
  background: yellow;
}

#gb {
  background: orange;
}

.square {
  height: 100px;
  width: 100px;
}
```

***js***
```js
var myDiv = document.getElementById('hello');

console.log(myDiv);

var theSquares = document.getElementsByClassName("square");

console.log(theSquares[0]);
console.log(theSquares[1]);

var theDivs = document.getElementsByTagName("div")

console.log(theDivs[0]);
console.log(theDivs[1]);

```

#### Selecting DOM Elements by CSS
* document.querySelector()
* document.querySelectorAll()

***js***
```js
var myDiv2 = document.querySelector('#gb');

console.log(myDiv2);

var mySquares2 = document.querySelectorAll('.square');

console.log(mySquares2[0])
console.log(mySquares2[1])

var myDivs2 = document.querySelectorAll('div');

console.log(myDivs2[0])
console.log(myDivs2[1])
```

#### Changing DOM Elements

***changing styles***
```js
myDiv.style.backgroundColor = 'chartreuse';
myDiv2.style.height = "300px";
```

#### Changing Content

***single DOM element changes***
```js
myDiv.textContent = "I love WDI"
myDiv2.innerHTML = "<h2>I love GA</h2>"
```

#### Manipulating Multiple DOM Elements

What if I want to do something to both divs at once?

```js
// this wont work!
theSquares.style.border = "2px dashed black"

// but this will
for(var i = 0; i<theSquares.length; i++) {
  theSquares[i].style.border ="dashed 2px black"
}
```

** Be Careful! **
Multi-element selectors like `querySelectorAll`, `getElementsByTageName`, and `getElementsByClassName` don't actually return an array; they return something called an _HTML collection_. This means that many array methods (iterators, in particular, which we'll learn about later) wont work. If you run into this problem, you can use the [`Array.from`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from) method to convert it to an array.


**Accessing and changing element attributes**

What if I want to change an attribute, like the src on this img?

**html**
```html
<img src="http://www.placekitten.com/200/200">
```

There are 2 ways to get and set attributes of a DOM element. You can access the properties directly or use use get/setAttribute methods. It's important that you know both exist, but generally accessing the properties directly is more consistent across browsers.

```js
var photo = document.querySelector("img");

// print current property value
console.log(photo.src);

// set new property value
photo.src = "https://picsum.photos/200/200";

// print using getAttribute method
photo.getAttribute("src");

// set using setAttribute method
photo.setAttribute("src","https://placebear.com/200/200");
```

**Classes**

Acessing, getting, setting CSS classes is slightly different than other properties.

First you can directly access the class attribute by using the `className` property of a DOM element.

```js
console.log(document.querySelector('div').className);
```

This works fine, but since elements can have multiple classes (separated by spaces) this often leads to needing to do some string parsing, so intead, we often use the `classList` attribute, which gives us "a DOM token list".

```js
console.log(document.querySelector('div').classList);
```

And just like the HTML collection, we can access the values in the classList like an array.

```js
var helloDiv = document.querySelector('div');
console.log(helloDiv.classList[0]);
```
You can add to the classList:

```js
helloDiv.classList.add('yellow');
console.log(helloDiv.classList);
```

You can also check if an item has a class (returns true or false)

```js
console.log(helloDiv.classList.contains('square'))
```

You can remove a class from the classList:

```js
helloDiv.classList.remove('yellow');
console.log(helloDiv.classList.contains('yellow'))
```

So far we've only seen examples of how to change properties of the DOM
and do things like:

- change the `textContent` of an element
- swap an existing image tag with another
- add/remove CSS classes
- modify CSS

We haven't seen the true power of DOM manipulation. The power to create and
destroy DOM elements!!

## Creating DOM Elements

We can add to the DOM! One big part of DOM manipulation is creating and adding new HTML elements to our page.

To create any new element we follow three basic steps:

1. **create** a new element and save it to a variable
2. **modify** any properties of the element
3. **attach** the element to an existing element on the page

Let's look at an example of how to create a new element in JavaScript and add it
to a page.

### Adding A New Link to the Bottom of a Page

```js
var a = document.createElement("a");
a.href = "http://hackertyper.com/";
a.textContent = "Hack!";

document.body.appendChild(a);
```

First, we use `document.createElement` and pass a string representing the tag we want to create. In this case we pass `"a"` to say we want to make a new link anchor tag. Save the result of the function into a variable.

`document.createElement` returns a new anchor tag. The element saved in our var has properties like any other anchor tag on the page, except they're all empty because it was just created. We must manually add `href` and `textContent` to set the link and display text.

Finally, we add the new anchor tag to the `<body>` with `document.body.appendChild`. This adds the anchor to the bottom of the page.

#### .appendChild()
The `appendChild()` function exists for all HTML elements. It's easy to show how to add things to the end of the body of the page because body is a property on the global document variable.

In order to append a new element to any other element, simply obtain a reference to it first. Here's an example showing how to add a new list item to an unordered list of my favorite movies.

```html
<ul id="my-favorite-movies">
  <li>Rushmore</li>
  <li>Star Trek VI: Undiscovered Country</li>
  <li>Anchorman</li>
</ul>
```

```js
// obtain a reference to where we'll add it
var list = document.getElementById("my-favorite-movies");

// CREATE
var newMovie = document.createElement("li");

// MODIFY
newMovie.textContent = "Pirates of Silicon Valley";

// ATTACH
list.appendChild(newMovie);
```

And there you have it!

#### .insertBefore()

It's easy to add things to the end of the body, at the bottom of a div, or at the end of a list. We have to do slightly more work if we want to add a new element at the beginning or in the middle of an existing element.

Use the following syntax:

```js
var insertedNode = parentNode.insertBefore(newNode, referenceNode);
```

* `parentNode` refers to the container (like body, or a div, or a list)
* `newNode` refers to the element we're adding
* `referenceNode` referes to an element that already exists in the `parentNode`

We can obtain a reference to all of the elements attached to a `parentNode` by accessing the `children` property.

Here's what it looks like all together:

```js
var list = document.getElementById("my-favorite-movies");

var newMovie = document.createElement("li");
newMovie.innerText = "Dr. Strangelove";

// get a reference to the second element inside the list
var second = list.children[1];

console.log(second);

// use insertBefore to add newMovie just before the second element.
list.insertBefore(newMovie, second);
```

Of course, you can choose any of the children of an element as a `referenceNode` for `insertBefore`. The new element will simply be added in the spot just before whatever you choose.


## The Case Against `.innerHTML`
It's true that instead of doing any of those fancy DOM manipulation we could just use set `.innerHTML` equal to strings.

**1. Safer** 
  Using `innerHTML` may be a security concern. Someone can sneak malicious content on your page. Using `.textContent` guarantees strings will only appear as text.

  Imagine if someone posted this as their status on Facebook and Facebook rendered it with `.innerHTML`. If status appeared in your page you would be redirected to an evil website!

  ```html
  <script>
  window.location = "http://evil.com";
  </script>
  ```
**2. Easier**
  Using `innerHTML` requires string manipulation when it's mixed with functions and parameters. These lines get long, and it's easy to confuse when to use necessary single-quotes or double-quotes to make attributes in HTML tags render correctly.

  As elements become more complex innerHTML becomes hairier to use and you'll find that creating elements as described here offers more modularity and fine-grain control over how things are added to the page.

  It totally works, but it can get nasty!

**3. Faster**
  A final reason to prefer manual DOM manipulation over `.innerHTML` is that it's much faster. The browser is optimized to make changes to the DOM via the methods described here.

  The browser spends more time manually computing how to interpret string content added via `.innerHTML`.

## Destroying DOM Elements

It's way easier to remove elements than it is to add them. Use this syntax:

```js
// straight up remove an element without remorse.
parent.removeChild(child);

// you may, if desired, save a reference to the element that was removed.
var oldChild = parent.removeChild(child);
```

`.removeChild()` allows you to remove any element you have reference to. If you save a removed element to a variable then you have access to it and can add it to somewhere else on the page.

### Exercise: Moving Elements

Practice saving the return value of `.removeChild()` and passing it to `.appendChild()` to move elements from one list to another.

Hook the functionality up to a button so you move one brunch item to the lunch list every time it is clicked. Prevent any errors from occuring if there's nothing left in the breakfast list.

```html
<button id="brunch">move to brunch</button>

<h1>Breakfast</h1>
<ul id="breakfast-foods">
  <li>Pancakes</li>
  <li>Waffles</li>
  <li>French Toast</li>
</ul>

<h1>Lunch</h1>
<ul id="lunch-foods">
  <li>Hamburger</li>
  <li>Sandwich</li>
</ul>
```
