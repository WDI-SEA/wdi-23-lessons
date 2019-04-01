# Data Structures: Stack

We move now into studying a few different common data structures in programming. What is a **data structure**? We have been using arrays the whole course and they are a perfect example. A **data structure** is a structured collection of data that adhere to certain constraints in the way data is created and accessed.

We build data structures to be efficient at a certain kind of task. For example, arrays provide linear lookup time as long as you know the index. Trees provide extremely fast searching because each time you choose one branch to search in over another, you eliminate half of the elements that needed to be searched. Data structures can seem rather limited after getting to use the JavaScript `Array` class (which has a huge amount of functionality built-in) but the limited ways that we can use data structures actually serve to make our code easier to maintain.

## A little history

Another reason to use data structures is because, quite often, we can dynamically allocate additional memory for any element that we need to insert. In the early days of programming, and even still today with some languages, arrays were not things to which you could freely add as many things as you wanted. You needed to allocate a specific amount of memory for the array. If you found that you needed to add an element and there was no more room, you needed to allocate additional space (usually double what you allocated previously), copy the existing array into the new space, delete the old memory holding the old array, and then finally you would have the space to add a new element.

The `Array` class built-into JavaScript is much more dynamic and full-featured: We can add as many elements as we want and the JavaScript engine deals with the extra memory allocation, if needed. So the memory management benefits of data structures are not quite as relevant in JavaScript. However, we can still learn how these structures work and what they are used for. It's is a good idea to know them also because they are frequently used as interview questions.

## What is a Stack?

A **Stack** is an array-like structure. You can think of it as a vertical stack with elements piled on top of each other. It has only two operations built into it:

* **push**: This function adds the argument passed in to the top of the stack.
* **pop**: This function removes the top element from the stack and returns it.

**Note:** You may recall that these two methods exist on any JavaScript array. They originated from the Stack data struct but because an array and a stack are so close structure-wise, JavaScript decided to build those onto its Array so you could use them as stacks.

That's it! There is only one end of the stack that we ever do anything with: the top. Because of this, we call it a `Last In, First Out` data structure, or `LIFO`. If you want a real world example, think of the trays or plates in the wells at a buffet. You can only ever place new trays or plates on the top and if you take one, it can only be the top one that you take.

## Use Cases

What is this good for, apart from simulating a buffet line? It turns out that stacks are fantastic at keeping track of when things start and stop and what happens in between. The relationship of the items in our stack is very similar to how elements are nested in HTML tags, or how function calls in recursive algorithms nest inside each other. Because of this, many programming language compilers and interpreters use stacks for parsing language syntax, specifically for detecting when you open a code block and then when that code block is closed. We call this "bracket matching" and it is probably the biggest use case for stacks. But, of course, you can use a stack for anything where new things must be dealt with before older things can be resolved.

## Implement It!

You will be building a Stack class that has its own `push` and `pop` methods. You can use a standard array to hold the data inside the class. Your `push` and `pop` methods should operate on the data in that array. You will need to decide which side of your array will be the "top" and then you must only add things onto or remove things from that side. You may use standard array functions inside your class but the only methods your class will implement that are publically callable will be `push` and `pop`.

Your `Stack` class should have a constructor that initializes the class to contain an empty array for holding the data.

## Function Specs

* **push**: Takes one parameter and inserts it onto the top of the stack. Returns nothing.
* **pop**: Takes no parameters. Removes top element from the array and returns it. If you call `pop()` on an empty stack it should return `undefined`.
* **peek**: An additional function that will be used for testing. Some stacks do implement this function though it is less common. It takes no parameters. It returns the value of the top element **without removing that element**.

### Further Research Resources

* [Stack Data Structure on Wikipedia](https://en.wikipedia.org/wiki/Stack_(abstract_data_type))
* [Stack on Geeks For Geeks](https://www.geeksforgeeks.org/stack-data-structure/)
