# Loops

#### `while`

A **while loop** repeatedly executes a code block as long as a specified condition is true. It is great for when we don't know how many times we need to do something, but must continue to do them until something happens to change the condition.

```js
var i = 0;
while (i < 5) {
  console.log("i is " + i);
  i++;
}

// Will print out:
// > i is 0
// > i is 1
// > i is 2
// > i is 3
// > i is 4
```

**The parts of a while loop**

```
while (CONDITION) {
  // CODE
}
```

#### `for`

A **for loop** is used for when we know exactly how many times we want to loop. This is very useful for repeating sections of code a certain number of times but is also very commonly used when accessing or manipulating elements in an array or similar collection. 

```js
for (var i = 0; i < 5; i++) {
  console.log("i is " + i);
}

// Will _also_ print out:
// >i is 0
// >i is 1
// >i is 2
// >i is 3
// >i is 4
```

**The parts of a for loop**

```
for (ITERATOR DECLARATION; CONDITION; UPDATE) {
  // CODE
}
```

In other words, you declare a variable to be used as the iterator and test to see if that variable passes a condition in order to run the code block. The update statement runs after the code block is executed.

Very commonly, you will use it to loop through an array.

```js
var foods = ["pizza", "tacos", "ice cream"];

for (var i = 0; i < foods.length; i++) {
  console.log("i like " + foods[i]);
}

// Will print out:
// > i like pizza
// > i like tacos
// > i like ice cream
```

#### `for...of`

A **for...of** loop is a newer construct, similar to a **for loop** in other languages like Python. It is specifically used for iterating over an array but the code is easier to read and understand.

```js
var cars = ['SUV', 'Subcompact', 'Economy', 'Performance', 'Pickup'];

for (var type of cars) {
  console.log("My car is a " + type);
}

// Will print out:
// > My car is a SUV
// > My car is a Subcompact
// > My car is a Economy
// > My car is a Performance
// > My car is a Pickup
```

#### `for...in`

A **for...in** loop is similar to a **for...of**, but good for looping through all the key-value pairs in an Object.

```js
var car = {
  wheels: 4,
  doors: 2,
  seats: 5
};
for (var thing in car) {
  console.log("My car has " + car[thing] + " " + thing);
}

// Will print out:
// > My car has 4 wheels
// > My car has 2 doors
// > My car has 5 seats
```
