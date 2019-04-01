# Python Functions 

### Learning Objectives
*After this lesson, you will be able to:*
- Understand the differences in functions between JS and Python.
- Create and call functions in Python.
- Understand and use `*args*` and `**kwargs` parameters
- Return a value from a function.

## Introduction: What Are Functions?

The definition of a function is simple — it's a reusable piece of code. We've seen them in JavaScript and we've used them for organizing our own code as well as passing into other functions as `callbacks`. We saw that they can have names or they can be anonymous. However, many of these features are not present in Python so it is very important for us to remember what the differences are.

### Declaring and Defining functions

Let's take a look at the basic function syntax in Python:

```python
def function_name(param1, param2, ...):
    # Code to execute when the function is
    # called goes here.
```

The top line starts with the `def` keyword. This defines a function. The next word is the identifier or name of the function. Following that is a parameter list inside parentheses. And the line ends with a colon. We can see the Python block form that we are beginning to be familiar with: the block starts with that colon and ends when the lines become unindented.

This brings us to the first important distinction between JS functions and Python functions:

```
1. Function expressions do not exist in Python
```

You can think of the code above as a function declaration and this is the only way to do it in Python. You cannot assign a function to a variable through expressive assignment like you can in JS.

That leads to the second distinction:

```
2. Functions are always named in Python
```

You cannot make an anonymous function in Python. When you declare them, you must give them names. There is a concept similar to an anonymous function in Python called a `lambda` but this is not really a function. It is merely a single expression that can be repurposed anonymously into other functions. Lambdas are fairly advanced so please feel free to look into them post-cohort.

We may as well mention the third difference while discussing anonymous functions:

```
3. Functions cannot be passed into other functions in Python
```

This is a feature of JavaScript, not Python. We will see how asynchronous code is dealt with once we start looking into Django.

### Calling functions

Good news! Calling functions is done in exactly the same way in Python as in JavaScript:

```python
function_name()
```

We simply write the function's name and include the parentheses and our script will call that function. We can also pass in arguments just as in JavaScript:

```python
function_name(arg1, arg2)

# Examples:
sum = add(num1, num2)
print_user(name)
merge_lists([1,3,5], [2,4,6])
parse_input('Some text')
```

In the same way as JS, we pass arguments into our function's parameter list (in the parentheses). These can be variable names or can be literal values.

## Parameters

Parameters are the placeholders for the data that is passed in as arguments into our functions. We define what the parameter names are and how many of them there will be when we first declare our function. They are largely done exactly the same as in JavaScript, as you can see above, but Python has a few special parameter options that are worth knowing about.

### The `*args` parameter

This is a special parameter that gives you the ability to pass in more arguments than there are parameters to hold them. By using the `*args` parameter, you tell Python that an argument list of arbitrary length will be passed in. Python, in turn, provides an `args` list inside the function which you can iterate over to gather all the extra arguments. Let's look at an adding example. Here's a function that can add two numbers:

```python
def add(num1, num2):
    return num1 + num2
```

Nice and sleek but not super scalable. What if we want to add more than two numbers? We'd need to rewrite it or write an additional function to deal with three params, one to deal with four, etc. That would suck. Let's use `*args` to make our lives easier:

```python
def add(*args):
    sum = 0
    for arg in args:
        sum += arg
    return sum
```

Now, we can pass in any amount of arguments to add and Python will loop through them all, adding them to `sum`, and finally returning that sum. You can mix normal and *args parameters also:

```python
def deep_func(arg1, *args):
    print(f"{arg1} is the first argument...")
    for arg in args:
        print(f"{arg} is also an argument.")
```

How do we call functions with these parameters? We can either pass in multiple parameters explicitly:

```python
add(2, 2, 5, 7, 9, 1, 13)
deep_func('foo', 'farge', 'meemaw')
```

Or, if you already have a list containing the arguments you'd like to pass in, you can do this:

```python
nums = [2, 2, 5, 7, 9, 1, 13]

add(*nums) # Note the asterisk we put before the list's name
```

That causes each element in the list to be passed directly into the `args` collection that you can use in the function.

### The `**kwargs` parameter

`**kwargs` is very similar to `*args` but it gives you the ability to pass in an indeterminate amount of key-word pair arguments, hence the name `kwargs`. We define the function in the same way as above but swap in `**kwargs` in the parameter list. The double-star prefix is specific to this parameter, just as the single-star is specific to the `*args` parameter:

```python
def we_need_the_func(**kwargs):
    for key, value in kwargs.items():
        print( f"{key} = {value}" )
```

We use the syntax `**kwargs` as our parameter in the function declaration. Then inside the function, we call the `items()` function on our kwargs. This let's us iterate over the object grabbing both `key` and `value` from the `items()` call. Then we print each pair.

When we call this, it looks like so:

```python
we_need_the_func(state='hungry', name='Del', food='Taco', quantity=10)
```

We put in however many key-value pairs we would like. Each one has a key name (not in quotes), then the equals symbol, then the value.

## Return Statements

When we are done with the function or if we want it to send a value back to the calling section of your script, we can call `return` just like in JS:

```python
def add(num1, num2):
    return num1 + num2
```

Normally, functions return one value but, just as in JS, if you need to return more values or a complex structure, you can choose to return a list, tuple, or dictionary.

Of course, just using the `return` statement by itself will exit the function without returning any value. This is useful for breaking out of the function early:

```python
saturday_night = True
we_in_the_spot = True

def uptown_func():
    if saturday_night and we_in_the_spot:
        print("Don't believe me just watch!")
        return
    print("Gotta kiss myself, I'm so pretty.)

uptown_func()
# prints "Don't believe me just watch!"
```

## Quick Knowledge Check

Let's test your knowledge of what we've learned this lesson.

1. Look at the following code. Which is the last line in the function that will execute if `x` is `10`?

```python
def categorize(x):
    if (x < 8):
        return 8
    x += 3
    if (x < 15):
        return x
    return 100
```

<details>
  <summary><strong>Click to Expand Answer</strong></summary>

  > Because `x` is greater than `8`, the first `if` condition is false. After adding `3` to `x` with `x += 3`, `x` will be `12`, which is less than `15`. This means that the second `if` statement condition is true, and Python will run the line `return x` and then stop running.
</details>
<br />

2. Look at this `adder` function:

```Python
def adder(number1, number2):
    return number1 + number2
```

Which of the following statements will result in an error?

A. `adder(10, 100.)` <br>
B. `adder(10, '10')` <br>
C. `adder(100)` <br>
D. `adder('abc', 'def')` <br>
E. `adder(10, 20, 30)` <br>

<details>
  <summary><strong>Click to Expand Answer</strong></summary>

> B, C, and E cause errors: In B, `adder(10, '10')` is incorrect because it tries to combine a string and an integer. In C, `adder(100)` will result in an error because it only provides one value, while in E, `adder(10, 20, 30)` provides too many.
</details>
<br />

3. How can we modify the `adder` function so that the statement on line E won't throw an error?

<details>
  <summary><strong>Click to Expand Answer</strong></summary>

```python
def adder(*args*):
    sum = 0
    for arg in args:
        sum += arg
    return sum
```
</details>
<br />