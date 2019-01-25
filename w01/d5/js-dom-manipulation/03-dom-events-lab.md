<img src="https://i.imgur.com/VLwvpbl.jpg" width="900">

## DOM Events - Lab

Let's practice with DOM events now that you know how to add event listeners to listen to events being raised in the browser.

For this lab, create a new pen in [CodePen](https://codepen.io/).

### Goal

The goal is to make something like the following embedded pen that displays the x and y location of the mouse when it is being moved over a `<div>`...

<iframe src="https://codepen.io/jim-clark/full/MbrVPN" height="600" width="90%"></iframe>

### Deliverable

Please slack your instructors the link to your completed pen.

### Requirements

- Add a `<div>` on the page and style it so that has:
	- A width of 300px
	- A height of 200px
	- Its `background-color` set to yellow
	- A border of `border: solid orange 10px;`

- Register an event listener on the `<div>` to listen to the ??? event.  Hint: It's not 'click' :)

- The callback function (the event listener) should display the x and y coordinates inside of the `<div>`, e.g., `x: 165, y: 22`

- Try experimenting with different location properties such as clientX/Y and offsetX/Y.

### Bonus

- Center the x, y display vertically and horizontally within the `<div>`.

