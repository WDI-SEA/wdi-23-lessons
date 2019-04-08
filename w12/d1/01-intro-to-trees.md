# Trees

## Learning Objectives

* Understand the place of trees in computers
* Describe different types of trees
* Describe the differences between in-order, pre-order, and post-order traversals
* Describe the difference between depth-first search and breadth-first search.

## What are Trees?

Trees are data structures that are similar to linked lists, but rely on nodes linking to many nodes in a tree-like fashion.

Trees consist of a top node, called the **root** of the tree, linking to **children** nodes below it.

![A tree](http://poj.org/images/1330_1.jpg)

So what's the point? Trees are used in many areas of computer science, including syntax parsers (determines if your parentheses and functions are correct). They're also used in practical applications, such as nested comments and files/directories.

## Implementation

Trees are built out of two classes: a `Tree` class and a `TreeNode` class.
This is similar to how Linked Lists are built. Linked Lists have a `ListNode` with
a propery for `data` and a property `next` to point to the next node in the list.

A `TreeNode` has a property to store `data`, and `left` and `right` properties to
store references to branches off the current node off to the left and right.

The `Tree` class defines one node called the `root` node which represents the top of the tree.
The `root` property points to an instance of a `TreeNode`. The `Tree` class also houses useful
methods for adding nodes, removing nodes, and printing out the entire tree.

## Traversing Trees

We can use for loops to iterate over arrays. Arrays have a definite length and
all of the indexes are sequential.

We can iterate Linked Lists using while loops. We can set a pointer to the first
node and keep stepping to the next `next` node until there's no next node left.
Linked Lists are still a linear data structure.

Trees are harder to iterate over. Each Tree Node splits into two more, on it's
left and right side. There's no one straight path through a tree. How could we
iterate through a tree?

The answer. Is Recursion. For each Tree Node we can print out it's `data`, then
print out the data for it's left and right side. Traversing a tree looks like this:

```python
def in_order_traverse(node):
  if node != None:
    print(node.data)
    in_order_traverse(node.left)
    in_order_traverse(node.right)
```

Note that there are different orders we can traverse the tree in.

* In-order traverse: recurse left, print node.data, recurse right.

* Pre-order traverse: print node.data, recurse left, recurse right.

* Post-order traverse: recurse left, recurse right, print node.data.

## Practice

Practice writing down the in-order, pre-order and post-order traversals for this tree:

```txt
       4
      / \
     1   8
        / \
       6   12
```

## Practice Problems

1. write a function that counts the total number of nodes in the tree.
2. write a function that finds the minimum value in a tree.

### Binary Search Trees

The most common tree structure is known as a binary search tree. Each node has
at most two children, and they provide a way to order elements. When inserting values,
the value goes to the left child if less than the root node, and to the right child
if greater than the root node.

![BST](http://cramster-image.s3.amazonaws.com/definitions/computerscience-5-img-1.png)

Note the **mathematical benefits** of a binary tree. As we add another "level" to
the tree, the number of nodes at that level doubles. Therefore, this can be represented
with the equation

```
2^x = n
```

where `x` is the number of levels, and `n` is the number of nodes. Since it's likely
that we know the number of nodes vs. of the number of levels, we can represent this
equation with logarithms:

```
x = log(n)
```

where `x` is the number of levels, and `n` is the number of nodes.

Due to the mathematical benefits above, insertion, deletion, and search take an average of O(log(n)) with binary search trees. But there's a worst case scenario.

![BST worst case](http://interactivepython.org/runestone/static/pythonds/_images/skewedTree.png)

We just made a bloated linked list! There has to be a better way, and thanks to research, there is.

### Self-Balancing Trees

To avoid the worst case scenario of a binary search tree, there are methods to create balanced trees, where we are **guaranteed** average and worst case runtimes of O(log(n)). A couple methods of doing this include rebalancing the tree based on subtree size and/or categorizing nodes.

These trees usually maintain a **balance factor** internally which grows or shrinks as the tree has items added to it. If the balance factor gets too high, the tree will rebalance itself using one of several node rotation algorithms which reorganize the nodes to bring the tree back into balance.

Examples of self-balancing trees include the [Red-Black Tree](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree) and the [AVL Tree](https://en.wikipedia.org/wiki/AVL_tree)

### Tries

[Tries](https://en.wikipedia.org/wiki/Trie) are a special type of tree, usually implemented with strings as the data, where child nodes share a common prefix (as represented by the parent node).

![Trie](https://upload.wikimedia.org/wikipedia/commons/thumb/b/be/Trie_example.svg/250px-Trie_example.svg.png)

They're usually used for quick lookups, generally autocomplete fields and IP address lookups. Assuming a trie was balanced, what would be the average lookup time for a value?

### Sets

A **set** is a data type that is often implemented with a tree, but can be implemented with various other data structures. Sets are collections of items that are **unordered** and **contain no duplicates.** In JavaScript, sets are included as the `Set` class.

## Graphs

If we remove even more restrictions from trees, we end up with graphs, which are interconnected sets of nodes. Each connection is called an **edge**.

![Undirected graph](https://computersciencesource.files.wordpress.com/2010/05/dfs_1.png)
![Directed graph](https://upload.wikimedia.org/wikipedia/commons/thumb/0/03/Directed_acyclic_graph_2.svg/305px-Directed_acyclic_graph_2.svg.png)

Graphs can be directed (a path that has direction) or undirected. Graphs are essential in applications such as Facebook or Twitter, where showing relationships between users and groups is essential across large applications. Other applications include maze solving and mutual friend finding, which can be implemented via the following graph searches.

Graphs are more than just nodes and edges though. Graph theory categorizes graphs by different properties, such as **bipartite**, **directed**, **undirected**, and more.

Note that we can modify a linked list to be a graph. Instead of having one `next` pointer, we could have either a list or matrix that shows which edges the graph connects to.

### Searching

There's a couple ways to search graphs.

#### Depth-First Search

Exploring the graph as far as possible, then backtracking.

##### Pseudocode

```
function DFS(node)
    visit node
    Loop through node.children
        if child node is not visited already
            recursive call to DFS for the child node
        end if
    end loop
end function
```

#### Breadth-First Search

Exploring the graph close to the starting point, then slowly growing out.

## Memory

### The Stack and Heap

* Out of Memory
* Stack overflow
* Heap overflow

The above are messages that you may encounter or have already encountered when working with programs. So what do they mean?

Whenever a program initialized (let's say, a Python program), the computer allocates a block of memory for the program to run in.

![Memory](http://www.cs.cornell.edu/courses/cs312/2004fa/lectures/memory%20layout.jpg)

The block of memory is separated into 3 sections: the stack, the heap, and the code.
The code is required to run the program, and the stack/heap are for allocating new values. Note that the stack grows in one direction, while the heap grows in the other direction. If one of these encroaches on the other, we run out of memory!

Ideally, this shouldn't happen, so we need to make sure that we only allocate memory when we absolutely need it. Web programmers occasionally encounter these problems, so it's good to have a general idea of what's going on under the hood.

Also note that linked lists, graphs, trees, and any other data structure that relies on **pointers** (variables that aren't values, but memory addresses) will usually store data in the heap.

### Memory Hierarchy

![Memory Hierarchy](http://tjliu.myweb.hinet.net/COA_CH_6.files/image007.jpg)
