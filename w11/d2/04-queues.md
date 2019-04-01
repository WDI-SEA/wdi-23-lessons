# Data Structures: Queue

If we were all from England, we would have no problem understanding this data structure. The Brits use the term "queue" to refer to a line of people waiting for service. It's like the old saying, "First come, first served." The data structure is exactly the same thing. It is another Array-like structure that stores data for processing in a very specific order.

The queue is a **first in, first out** (or FIFO) data structure which means that the first element added to the queue will be the first one removed and processed. It implements only two functions:

* **enqueue**: This function works like `push`. It takes one parameter and it inserts it into the queue in the last place position.
* **dequeue**: This one works like `pop`. It takes no parameters. It removes and returns the element in the first place position.

## Use Cases

It's much easier to think of what a queue would be used for. In any situation where you must process data items in the order they came in, a queue is the perfect solution. It is used in event systems like in our web apps. You may also have heard of a print queue that documents go into to wait their turn to be printed. This is the same sort of structure. It keeps the data organized so that no matter how many new ones come in or how long each one takes to process, they will always be done in the order they came in.

## Implement It!

You will be building a Queue class that has its own `enqueue` and `dequeue` methods. You can use a standard array to hold the data inside the class. Your `enqueue` and `dequeue` methods should operate on the data in that array. You will need to decide which side of your array will be the "high priority" side and then you must only `dequeue` from that side. Likewise, you must only `enqueue` onto the "low priority" side. You may use standard array functions inside your class but the only methods your class will implement that are publically callable will be `enqueue` and `dequeue`.

Your `Queue` class should have a constructor that initializes the class to contain an empty array for holding the data.

## Function Specs

* **enqueue**: Takes one parameter and inserts it onto the queue in the last place position. Returns nothing.
* **dequeue**: Takes no parameters. Removes first place element from the array and returns it. If you call `dequeue()` on an empty queue it should return `undefined`.
* **peek**: An additional function that will be used for testing. Some queues do implement this function though it is less common. It takes no parameters. It returns the value of the first place element **without removing that element**.

### Further Research Resources

* [Queue Data Structure on Wikipedia](https://en.wikipedia.org/wiki/Queue_(abstract_data_type))
* [Queue on Geeks For Geeks](https://www.geeksforgeeks.org/queue-data-structure/)
