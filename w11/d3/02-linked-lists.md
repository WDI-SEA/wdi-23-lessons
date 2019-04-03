# Linked Lists

You've done great with the data structures so far. Now it's time to take it up a notch. So far we've been building our data structures using arrays for data storage. Now we will be building one from scratch, making our own classes for the data elements as well as the overarching data structure.

The Linked List is an array-like data structure in which each data element is linked to the next:

```
  HEAD
    |
    V
--------    --------    --------    --------
| data |    | data |    | data |    | data |
|______|    |______|    |______|    |______|
| next |--->| next |--->| next |--->| next |---> null
|______|    |______|    |______|    |______|
```

Each element in the list is called a **ListNode** or just a **Node**. Every node contains two things:

* A data payload. Each element in your linked list will be stored in a node's data payload.
* A reference to the **next** node.

There are a few styles of linked lists but the most basic usually only maintains a reference or pointer to the first node, called the **head**. Some lists also maintain a **tail** pointer but in many cases the end of the list is found simply by iterating over each node until you find a **next** reference that points at `null`.

## Use Cases

Linked lists are widely used in computer science. Because they are dynamic data structures in which each element is newly instantiated in memory, they are great in native languages where memory management is more important than in web development. They are also used internally in other data structures such as hash tables. In JavaScript, the `Array` already has much of this functionality but, as computer scientists, we must understand how these data structures are created and how they function. They are also very common topics for interview questions.

## Implementation

In this first part of the lesson, you will be building the necessary classes and some basic functionality. You will expand it in the second part to add some more advanced functionality.

You will need two classes:

* **ListNode**: This is the class for the node elements. It needs a **data** member variable and a **next** member variable. When an object is instantiated from this class, you should initially set the **next** pointer to `None` and the **data** variable to whatever value is passed into the constructor.
* **LinkedList**: This is the class for the list itself. It only needs one data member: a member variable named **head** that will initially point at `None` when the list is first created but will be set to the first node when one is added to the list.

In addition to the **head** property, the LinkedList class will need the following methods:

* `add()`: This function should take one parameter which is the data to add to the **end** of the list. It doesn't need to return anything. It should construct a new **node** with the data provided and add this node to the appropriate place in the list. If the **head** is `None` then you should set the **head** to point to the new node. If **head** is not `None` then you must iterate over the list until you find a **node** with its **next** property pointing at `None` and add the node there. It will always add the new **node** at the very end of the list.
* `print()`: This function should **return a string** of the values of the linked list separated by dashes. For example, if your list contains the values 5, 10, and 15, your `print()` function would return the string `5-10-15`. If your list is empty, have it return the string `empty`.

> NOTE: Printing values in this way is not a convention of linked lists. It is only to get a consistent format on which we can run tests.

# Linked Lists, part 2

Great! You now have the beginnings of a solid linked list. Now we will add a bit more functionality.

As you probably realize, adding items onto the end of the list is a fairly simple operation. It is when we start changing things in the middle of the list that it gets a little complicated. Imagine a chain. What would we need to do to add a link in the middle of it? It would involve the breaking of certain links, the insertion of a new element, and then a reconnection of links to accomodate the new element.

You will be writing two methods that perform operations such as this.

* `delete(index)`: The delete function takes one parameter: an index of the item to delete. It need not return anything. Since the linked list maintains no collection of indices or any length, you must iterate over the list until you find the node that would be at the index provided. Also realize that each node only has knowledge of the next node, and nothing else in the list. If you delete a node, the reference it had to the next node will be gone. You will need to save that reference in a variable so that the previous node can be reconnected to it, preserving the continuity of the list. If the node to delete is the **head** node, you must reassign the **head** to the current second node. If the index is not in the list, simply return.
* `insert(data, index)`: The insert function takes two parameters: a data parameter and an index of where you want the new node with the new data inserted. It also need not return anything. The index provided should be the final position of the new node, meaning that if the index passed in is 0, the new inserted node will be inserted before the current **head** node, making it the new head node. If passed an index beyond the length of the list, simply add it at the end.

## Pseudocode

Let's walk through the logic for inserting and deleting:

### Deleting an element

1. Save the **next** pointer of the node you are going to delete.
2. If there is a previous node, connect it's **next** pointer to the node after the deleted one.
3. If the node to delete was the **head**, make sure to reassign the **head** property of the list to the node after the deleted one.
4. Set the deleted node's **data** and **next** properties to `None`.

There is no need to explicitly delete an object in Python. The garbage collection system in the engine will reclaim the memory of any variable that has no references to it so removing all the references is all we need to do.

### Inserting an element

1. Find the node currently at the index of insertion and store a reference to it. Make sure to also keep track of the node before it, if present.
2. If there is a previous element, reassign it's **next** property to the node you are inserting.
3. Assign the node currently at the index of insertion to the new node's **next** property.
4. Make sure to update the list's **head** if necessary.

### Implement it!

Add these new functions to your LinkedList class

### Further Research Resources

* [List List Data Structure on Wikipedia](https://en.wikipedia.org/wiki/Linked_list)
* [Linked List on Geeks For Geeks](https://www.geeksforgeeks.org/data-structures/linked-list/)
