# Recursion

## Objectives

* Define recursion
* Identify when to use recursive solutions
* Identify the two parts of a recursive function:
  * Write base case
  * Write recursive case
* Write recursive functions

#### "To understand recursion you must first understand recursion." - Ancient Computer Science Wisdom

## What is it?

Today we're going to explore a topic called **recursion**. According to Wikipedia recursion is "the process of repeating items in a self-similar way." In programming recursion basically means, "a function that calls itself."

Here's some pictures that we could say are **recursive** and exhibit properties of **recursion**:

![Mona Lisa holding her own painting](recursion_mona_lisa.jpg)
![Hulk Hogan's beard growing his own face](recursion_hulk_hogan.jpg)

## Recursion vs Iteration

When do we use recursion? Pretty much any time we can write an iterative solution (i.e. one that loops to get to the solution), we can typically use recursion to write a more elegant solution. In general, any iterative algorithm can be rewritten as a recursive algorithm and vice versa.

So when should you use one or the other? Both have pros and cons. Here are some of the properties of each:

#### Iterative Solutions:
* Are usually harder to read but easier to write
* Have the potential for infinite loops

#### Recursive Solutions:
* Usually more difficult to write but much easier to read
* Have the potential to overflow the function call stack

There are pitfalls to both approaches, but we must prefer recursive solutions in most cases where the iterative solution would be much harder to read and maintain. Certainly, don't rewrite all of your looping algorithms as recursive algorithms but do realize that most standard algorithms like the ones we will learn for searching and sorting are usually written recursively.

## Let's Pretend We Are Each A Recursive Function

How can we count how many people are sitting directly behind one person in this
classroom?

The teacher stands at the front of the room and asks someone how many people are
behind them. That person can do two things:

1. they can say there's no one sitting behind them
2. they can ask the person behind and add one to their answer

Now let's try it. Don't turn around and look at who all is behind you!
You can only communicate with the person who asked you the question,
and the person directly behind you.

This is an example of recursive programming. We could write our instructions
as a function called `count` that calls itself:

```python
def count(person):
  other_person = person.get_person_behind_me()
  if other_person == "no one":
    return 0
  else:
    return 1 + count(other_person)
```

Recursion allows us to write extremely expressive code! We can write a very
small amount of code and have it perform extremely powerful computations.

## A Useless Recursive Function

We know that functions can call other functions. It's not so obvious that functions can actually call themselves too. Let's look at one function that calls itself and consider what it does.

* What will be the output of this function?
* When will this program stop running?

```python
# define the function
def navel_gazer():
  print("hmm...")
  # make a recursive call to the function
  navel_gazer()

# call the function
navel_gazer()
```

This function will theoretically print out "hmm..." forever. It will never stop running. It will keep calling itself forever and ever.

In practice, the function will eventually crash. Your computer will run out of memory and you'll see an error message saying something like, "stack overflow exception" or "maximum call stack exceeded."

This function is only here to prove that it's possible to call a function from inside itself, and to show the danger of a function that calls itself forver.

Recursion gets much better than this useless example. It's possible to write recursive functions in such a way that we can write very robust, expressive code.

Let's look at more recursive functions and see what techniques we can use to make sure our programs do useful things and don't simply call themselves forever.

## Base Cases and Recursive Cases
Recursive functions are comprised of the following parts:

* one or more **base cases**, and
* the **recursive case**.

Think of the base case as where the recursion will stop. If we are writing a function that will add the numbers between 0 and some integer then the case for adding 0 will be our base case. When writing recursive functions, this is usually the first thing we check for. If we find we are handling the base case then we can deal with that and return the value from the function. Else, we recurse (call the function again) with the next successive value.

This function doesn't do anything but it shows you the structure. See how we recurse from n down to 0. Each time we recurse, the function is passed a number one lower than the previous function call until it is finally passed 0 and then it will hit the base case:

```python
def recurse(n):
  # check for base case
  if n <= 0:
    return 0
  else:
    # otherwise do a small amount of work and call the function again
    return recurse(n - 1)
```

## Base Cases
The base case is the simple case. It's the case when the function doesn't call itself. These cases are often deceivingly simple! Think of them as writing what the program should return for the most obvious of examples.

If you're writing a function that computes the sum of numbers from `0` to `n`, then `0` will be your base case:

```python
if n <= 0:
  return 0
```

The base case is very important because it is what stops the recursion. Writing one or more base cases that define the answer for the simplest part of the problem will prevent your program from calling itself indefinitely.

## Recursive Cases
The recursive case is the case when the function performs one small part of the problem and calls itslf recursively to solve the next small part of the problem.

## Sum Problem Practice

Let's write a function called `sum` that accepts a number `n` and computes the sum of numbers from 0 to n.

What is the base case?

```python
if n <= 0:
  return 0
```

> Why less than or equal to 0? I want to make sure that all cases return 0 unless n is actually greater than 0. Just an extra safety precaution.

What is the recursive case?

```python
if n > 0:
  # man, I wish we had a function that computed the sum of 0..N-1
  return n + ???
```

Oh wait!! We've already defined a function that sums all numbers! Take a step and take the leap of faith. Call the function again!

```python
if n > 0:
  # Call sum() again, passing in the next lower number
  return n + sum(n - 1)
```

Putting the whole thing together, we have:

```python
def sum(n):
  if n <= 0:
    return 0
  else:
    return n + sum(n - 1)
```

It is important to understand how this code is called and run. If we pass in a value of 3, we expect that it will return the result of 0 + 1 + 2 + 3 which is 6. When we first call `sum(3)` the conditional will evaluate to false and go to the recursive case and return the value of `n + sum(n - 1)`. But it doesn't know what `sum(n - 1)` will be yet so before it returns anything it must run `sum(n - 1)` where `n` equals 3. The value 2 is passed in and the process begins again. 2 is greater than 0 so we will go to the `else` case (the recursive case) and call `sum(n - 1)` but this time `n` equals 2. Rinse. Repeat. Now, we will call `sum(n - 1)` where `n` equals 1. Then the conditional will branch to the base case and will return `0`. Finally, the previous functions can start returning their values now that the most recent recursive call has completed and returned an actual value (from the base case.) 0 is returned and we add 1 to it and return that. That is returned to the previous call which adds 2. That's returned to the original call which returns 3 plus whatever was returned from the recursive calls (2 + 1 + 0) and we get our expected value of 6.

## Palindrome Practice Problem

Detecting whether a string is a palindrome is an excellent example of a problem that turns out to be extremely elegant when written recursively.

What is a palindrome? A palindrome is a string that is spelled the same backwards and forwards.

Put another way, a palindrome is a string where the first letter is equal to the last letter, and the second letter is equal to the second to last letter and so on. An empty string is considered a palindrome. A one letter string is considered a palindrome.

Write a function called `is_palindrome` that accepts a string and returns `True` if the string is a palindrome, and returns `False` if the string is not.

What are our base case(s)?

* Return true if the string is empty
* Return true if the string is of length 1

Where did these come from? These are the cases where we know what to return without doing any additional work. For us, it is spelled out in the definition of a palindrome: strings of length 1 or 0 are considered palindromes so we can just return `true` in those cases. These are the cases where we don't need to recurse so recursion will stop here. It is important to define the base case(s) so that you do not overflow the call stack.

What is our recursive case?

* compare the first and last letter:
  * if they are equal then recurse on the remaining parts of the string
  * if they are different then return false

Remember your return statements! The final solution should bubble up from the deeper recursive calls!

```js
is_palindrome("")        // true
is_palindrome("a")       // true
is_palindrome("ab")      // false
is_palindrome("abba")    // true
is_palindrome("catdog")  // false
is_palindrome("tacocat") // true
```

```python
def is_palindrome(phrase):
  # Base cases for strings of length 0 or 1
  if phrase.length < 2:
    return True
  else:
    # Get the first and last letters
    first = phrase[0]
    last = phrase[-1]

    # Compare the first and last letters.
    if first !== last:
      # Exit the function right away since if there is ever a mismatch then it ain't a palindrome.
      return False
    else:
      # Slices the middle portion of the string, omitting the first and last letters
      middle = phrase[1:-1]
      # We pass that portion into the recursive call
      return is_palindrome(middle)
```

Eventually, the middle portion that we pass in will have gotten down to the size of 1 or 0 characters (if no mismatches had been detected by that point). At that point, the base case will trigger a return of `true` and the recursive calls will start returning.

## Challenges

### Factorial

Write a recursive function that computes the factorial of a number `num` passed in as an argument. The factorial of a number is the product of that number multipled by every successive integer downward to 1. In mathematics, it is denoted by the `!` symbol (e.g. 5!). So 5! (spoken "five factorial") is equal to 5 * 4 * 3 * 2 * 1 or 120. 1! is equal to 1. Interestingly, 0! is also equal to 1 according to the official definition. You should also return 1 for any negative numbers entered, for the sake of simplicity.

```python
factorial(-1) # 1
factorial(0)  # 1
factorial(1)  # 1
factorial(2)  # 2
factorial(3)  # 6
factorial(4)  # 24
factorial(5)  # 120
```

### Fibonacci

Write a recursive function that accepts a positive number and returns the number at that place in the Fibonacci Sequence. Each term in the Fibonacci Sequence is the sum of the previous two terms. The sequence starts as follows:

```1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89...```

The sequence doesn't really make sense until you have at least two numbers to add so the definition of the Fibonacci Sequence states that the first two numbers are always 1 and 1 (or 0 and 1 by some definitions but we will use the "1, 1" starter definition).

With that in mind, for 0 or any negative argument just return 0. Since the first two positions are always defined to be 1 and 1 so you can set up your function to return 1 when the selected position is either 1 or 2. Any higher position will need to be calculated.

```python
fibonacci(-1) # 0
fibonacci(0) # 0
fibonacci(1) # 1
fibonacci(2) # 1
fibonacci(3) # 2
fibonacci(4) # 3
fibonacci(5) # 5
fibonacci(6) # 8
fibonacci(7) # 13
```

### Reverse String

Write a recursive function called `reverse` that accepts a string and returns
a reversed string.

```python
reverse("") # ""
reverse("a") # "a"
reverse("ab") # "ba"
reverse("computer") "retupmoc"
reverse("abcdefghijklmnopqrstuvwxyz") # "zyxwvutsrqponmlkjihgfedcba"
reverse(reverse("computer")) # "computer"
```

Is `reverse(reverse("computer"))` considered recursive? Why or why not?