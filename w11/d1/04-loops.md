# Loops

## But first, some foundational info...

### Ranges - Purpose
<br>

- Although not a general purpose container, **ranges** are a _sequence type_ like _lists_ and _tuples_.

- The **range** type represents an immutable sequence of numbers and is commonly used for looping a specific number of times in `for` loops.

- _Ranges_ have a class (type) of `range`.

---
### Ranges - Basic Syntax
<br>

- **Ranges** can only be created by invoking the `range()` class:

	```python
	for num in range(5):
		print(num)
	> 0
	> 1
	> 2
	> 3
	> 4
	```
	Notice that by default, the sequence starts at `0` and goes up to, but does not including the integer passed in.

---
### Ranges - Basic Syntax
<br>

- _Ranges_ can also generate sequences with a **start** and a **step**:

	```python
	for even in range(2, 10, 2):
		print(even)
	> 2
	> 4
	> 6
	> 8
	```

- When not passed in, the _start_ value defaults to `0` and the _step_ defaults to `1`.

---
### Ranges - Basic Syntax
<br>

- _Ranges_ can also be used to create _lists_ and _tuples_:

	```python
	nums = list(range(10))
	print(nums)
	> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
	odds = tuple(range(1, 10, 2))
	print(odds)
	> (1, 3, 5, 7, 9)
	```

---
### Ranges - Negative Step
<br>

- If the _step_ is a negative integer, the _sequence_ counts down:

	```python
	for num in range(5, 0, -1):
		print(num)
	> 5
	> 4
	> 3
	> 2
	> 1
	```

# Okay, now really: Loops!

## The `for` Loop

`for` loops appear in several computer programming languages and get their name from the fact that they loop (or iterate) **for** a specific number of times: once **for** each item in a list, or once **for** each number in a range.

Let's take a look at the syntax:

```python
names = ["Tom", "Deborah", "Murray", "Axel"]

for name in names:
  print(name)
```

Just like in ES2015, we can use the `for...in` loop construct. It is a loop that will iterate once for each element in the collection:

```python
for item in collection:
  # do something with the item
```

This is the only for-loop construct in Python. The old-school, imperative way of writing out a for-loop...

```js
for (let i = 0; i < 10; i++) {...}
```

...does not exist in Python. You must have a collection to iterate over. But then how do we iterate for a specific number of times? Python has a special kind of collection called a `range` which is perfect for holding a range of numbers, like the kind you might use as an iterator value:

```python
for i in range(10):
  print(i)
```

If we run this we will see that the numbers 0-9 are printed. They came from that range. By declaring it with a 10 as the argument, it contains the numbers 0 through 9. Having an index like this is very useful if we want to modify elements in the collection we are working with. We will learn about ranges this afternoon but let's see if we can iterate over the range of indices in a collection. First, we need to know how to find the length of a collection. Python provides a function called `len()` which will return the length of whatever collection was passed in. Let's make an array (called a List in Python):

```python
names = ["Flint", "John Silver", "Billy Bones", "Israel Hands"]
```

To get a range containing all the valid indices from that array, we can pass the array length into the range function:

```python
range( len(names) ) # Spaces added for readability
```

Now we should be able to iterate over the array using indices:

```python
names = ["Flint", "John Silver", "Billy Bones", "Israel Hands"]

for i in range(len(names)):
  names[i] = names[i].upper()
```

Using the for-loop in this way, we can use the `i` variable to access the actual elements in the array and change them if we want. You cannot do this using the `for item in collection` syntax above because that will only give you a copy of the element. The only way to access the element directly is using the index with the range method above or the `enumerate()` function to pull the index with the element.

## The `while` loop

Python also has a `while` loop construct that will continue to iterate __while a given condition is true__. Let's look at the syntax:

```python
while condition:
  # do some stuff
```

`While` loops are great for when you don't know how many times you will need to iterate. Like in guessing games. Let's use a small function called `input()` to get some user input so that they can take guesses about our secret number. For this guessing game, let's hard-code in a secret number, just for simplicity:

```python
answer = 4
guess = input("Guess what number I'm thinking of (1-10): ")
while guess != str(answer):
  guess = input("Nope, try again: ")
print("You got it!")
```

Run this to see what the `input()` function does. You should see the prompt asking you to enter a guess
between 1 and 10. Input a number and hit `enter`. It stores the number you entered into the `guess` variable, but what gets stored is actually a string and not a number. As a result, when we compare the guess to the answer, we need to use the `str()` function to convert the value in `answer` to a string. (Just like apples and oranges, you can't compare integers to strings. Both things being compared must be of the same type.) If they are not the same, the guess was wrong, so we prompt the user to try again. Each time the user enters a new number, the script checks to see if it matches the answer and loops again if it doesn't. When the guess finally matches the answer, the loop condition will be false and the loop will exit. Finally, the notification that the user guessed correctly is displayed.

## List Comprehensions

---
### List Comprehensions
<br>

- One of the most powerful features in Python are _list comprehensions_.

- _List comprehensions_ provide a concise way to create and work with lists.

- They will probably seem a little confusing as first, but they certainly are a favorite of _Pythonistas_ and you will certainly come across them when googling.

- Also, _comprehensions_ can be used similarly to create _dictionaries_ too.

---
### List Comprehensions<br><small>Numerical Example</small>
<br>

- If we needed to square all of the numbers in a _list_ and put them into a new _list_, we might use a for loop like this:

	```python
	nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
	
	# I want 'n * n' for each 'n' in nums 
	squares = []
	for n in nums:
		squares.append(n * n)
	print(squares)
	> [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
	```

- **What method in JS would we use in this scenario?**

---
### List Comprehensions<br><small>Numerical Example</small>

- A _list comprehension_ can reduce this code:

	```python
	# I want 'n * n' for each 'n' in nums 
	squares = []
	for n in nums:
		squares.append(n * n)
	```
	To this:
	
	```python
	# I want 'n * n' for each 'n' in nums 
	squares = [n * n for n in nums]
	```

- The _comprehension_ is basically an advanced `for` loop within _square brackets_ which, of course, returns a new _list_.

---
### List Comprehensions - Basic Syntax
<br>

- Here's the basic syntax of a _list comprehension_:

	```python
	# [<expression> for <item> in <list>]
	# This reads as: I want <expression> for each <item> in <list>
	```

---
### List Comprehensions - Filtering

- We've seen how _list comprehensions_ are a nice way to map a list, but they can be used for **filtering** too.

- Just like before, let's first look at how we would use a `for` loop to map and filter simultaneously:

	```python
	nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
	
	# I want 'n * n' for each 'n' in nums  if 'n * n' is even
	even_squares = []
	for n in nums:
		square = n * n 
		if square % 2 == 0:
			even_squares.append(square)
	print(even_squares)
	> [4, 16, 36, 64, 100]
	```


---
### List Comprehensions - Filtering

- Again _list comprehensions_ reduce the mapping and filtering from:

	```python
	# I want 'n * n' for each 'n' in nums  if 'n * n' is even
	even_squares = []
	for n in nums:
		square = n * n 
		if square % 2 == 0:
			even_squares.append(square)
	```
	To this one-liner:
	
	```python
	# I want 'n * n' for each 'n' in nums  if 'n * n' is even
	even_squares = [n * n for n in nums if (n * n) % 2 == 0]
	```
	Nice and readable!

---
### List Comprehensions - Review
<br>

- **What characters start and end a _list comprehension_**

- **Do _list comprehensions_ create or loop through lists?**

---
### List Comprehensions - Summary
<br>

- We've only scratched the surface of _list comprehensions_.

- They can even be used to create lists of _tuples_ that would otherwise require nested `for` loops.