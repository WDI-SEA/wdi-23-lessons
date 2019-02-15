# Views and Templates

### Views

First, we cannot keep using `res.send` to send a response. Ultimately, we'll want to send HTML files back to the client. It would be much more efficient to store them in files. Let's make a folder, `/views`, and create an `index.html` page inside.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Testing a View</title>
  </head>
  <body>
    <h1>Hello world!</h1>
  </body>
</html>
```

Let's modify the `index.js` to send this file via `.sendFile`. The `sendFile()` function takes a full path from the root of the project.

**index.js**
```js
var express = require('express');
var path = require('path');
var app = express();

app.get('/', function(req, res) {
  // use sendFile to render the index page
  res.sendFile(__dirname + '/views/index.html');
});

app.listen(3000);
```

Now, if we want this HTML file to be able to access a stylesheet, images, or extra JavaScript files, we need to set up the "express.static" setting.

```js
// sets a static folder where CSS, JS, and images can be found
app.use(express.static(path.join(__dirname, 'static')));
```

Then we just place our static files into subfolders of "static" and when the HTML file needs to access them, it can reference them relative to the root of the "static" directory.

### Templating with ejs

The downside to this method is that we are only sending HTML files, and what if we want to customize what's on the page? On the front-end, we could manipulate the page using JavaScript. But on the back-end, we can inject values into the HTML using template engines. So we're going to set up a template engine called **ejs** (embedded JavaScript) and use that instead.

We need to do a couple steps to get the template engine working.

First, install `ejs` by running `npm install ejs` in the command line.

Then, add the following line:

```js
app.set('view engine', 'ejs');
```

Also, rename the .html file to a .ejs file. We'll see that the `.ejs` extension is optional in the route, but necessary in the file's actual name.

### Templating with Variables

Templating with variables means we can pass in an object to the `.render` function and access those variables inside the ejs template.

**server.js**
```js
var express = require('express');
var app = express();

app.set('view engine', 'ejs');

app.get('/', function(req, res) {
  res.render('index', {name: "Sterling Archer"});
});

app.listen(3000);
```

then we need to update our `index.ejs` to use a templating variable.

**index.ejs**
```html
<!DOCTYPE html>
<html>
  <head>
    <title>Testing a View</title>
  </head>
  <body>
    <h1>Hello, <%= name %>!</h1>
  </body>
</html>
```

The JavaScript being embedded is enclosed by the `<% %>` tags. The addition of the `=` sign on the opening tag means that a value will be printed to the screen. We can also use the following signs to tell EJS to parse code in different ways:

* `<%- name %>` like the `=` symbol, this will print out the enclosed expression but **without escaping HTML**
  * If the name was `"<span>Sterling Archer</span>"`, then the `<span>` elements won't be escaped.
* `<% name %>` will not print out the expression, but it will execute it
  * Handy for `if` statements and loops

This doesn't only apply to primitive variables. We can even include variable declarations and iterators using ejs.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Testing a View</title>
  </head>
  <body>
    <h1>Hello, <%= name %>!</h1>

    <% var obsessions = ['spying', 'sarcasm', 'Kenny Loggins']; %>

    <ul>
    <% obsessions.forEach(function(item) { %>
      <li><%= item %></li>
    <% }); %>
    </ul>
  </body>
</html>
```

### Partials

Partials can be used to modularize views and reduce repetition. A common pattern is to move the header and footer of a page into separate views, or partials, then render them on each page.

#### Example

**partials/header.ejs**
```html
<!DOCTYPE html>
<html>
<head>
  <title>My Site</title>
</head>
<body>
```

**partials/footer.ejs**
```html
</body>
</html>
```

**index.ejs**
```html
<% include ../partials/header.ejs %>

<h1>Welcome to my site!</h1>

<% include ../partials/footer.ejs %>
```
