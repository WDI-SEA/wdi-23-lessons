### Forms, Labels, Input Types

#### Forms

One of the most common ways to send data to a server is by using a form. A form has two essential attributes, action and method.

* **Action** - This specifies a route where you are going to. For example an action of '/test' will take you to the /test route (something you have probably configured in your server side code)

* **Method** - The HTTP Verb that this form will be using (HTML only knows GET and POST, but there are ways to override this default which we will see when we use Node and Rails. The default method is GET so if you are making a GET request you can leave this empty.

#### Labels

Labels are text you place before/after inputs to tell the user what the input is for. The for attribute is for screen readers and if the ID of the input matches the ID of the for attribute then you can click on the label and have it automatically focus/check the input.

#### Input Types

By default, input elements will allow users to type in text. There's also a plethora of different input types, specified by a `type` attribute. Take a look at the Codepen below for some examples.

<p data-height="268" data-theme-id="0" data-slug-hash="xZygWo" data-default-tab="result" data-user="bhague1281" class='codepen'>See the Pen <a href='http://codepen.io/bhague1281/pen/xZygWo/'>Form Elements</a> by Brian Hague (<a href='http://codepen.io/bhague1281'>@bhague1281</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

Documentation on input types: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input

### User Input Practice

Clone the following repo and open instructions.html in your browser. Your goal is to work to turn the basic skeleton.html page into a webpage meeting the requirements described in the instructions that looks like the picture shown in solution.png. Good luck!

https://github.com/WDI-SEA/html_user_inputs