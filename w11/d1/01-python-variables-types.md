# Variables

### Learning Objectives
*After this lesson, you will be able to:*
- Create comments.
- Create and use variables.
- Explain Python data types
- Create and format Strings
- Use common String methods

## Python History
The Python programming language started in **December 1989** as a hobby project for Dutch programmer Guido van Rossum. He was looking for a programming project to keep himself occupied during his Christmas break. It was first released in **1991**.

Ten years later on **October 16th 2000, Python 2.0** was introduced with many major new features and it started to gather a community dedicated to it's development.

**Python 3.0 was released on December 3rd 2008.** Version 3.0 introduced many enhancements and unified more of the design of the language at the cost of making some old 2.0 code incompatible.

Python, like JavaScript, is a high-level interpreted programming language. "High level" means that programmers don't have to worry about things like managing memory, the langauge takes care of that for us. Programming languages are either *interpreted* or *compiled*. Compiled languages have to be run through a compiler that changes source code into compiled code before the program can be run. Interpreted languages are run on the fly. Interpreted languages are generally more playful and easier to experiment with because we can take small pieces of code, paste it into a console that interprets it and immediately evaluates it.

The bulk of JavaScript was built by one person in 10 days.

Python has been built by a community of programmers over many years. Many people have put good thought into the design of the language. Python is a general purpose language that exists on your computer outside the browser. Python is built to handle a wide variety of tasks beyond web development. Python
comes with a large standard library of functions that you can use to accomplish most any programming task you set out to do.

The creator of Python, **Guido van Rossum**, still plays an active role in the development of the language. He has been given the title, "Benevolent Dictator for Life" by the Python community. He plays a central role in deciding the direction of Python.

The Python community is continually developing the language and adding new features. One formal way the community drives development is through PEPs, Python Enhancement Proposals. These proposals are documented on <http://python.org> and you can read a full list of them here: <https://www.python.org/dev/peps/>.

# The Python Language

## Comments in Python

Python has comments just like other languages. You make a comment using the `#` symbol. 

```python
# This is a comment! Python will ignore it.

# The next line is not a comment...
total_guitars = 7

# I have to put a new # on every new line that I want to be a comment - every time I hit return or enter.
```

Just like in every other programming language, comments are vital for documentation of code and reducing debugging time. Make liberal use of them.

## Python Variables

Variables in Python work in much the same way that variables work in JavaScript. They are meant to hold bits of data we need throughout our app's lifecycle. Here is how we declare a variable in Python:

```python
my_number = 15
```

Notice that there is no `var`, `let`, or `const` keyword in Python. We only need the name of the variable and then we can assign to it. Just know that you cannot *only* declare a variable in Python: it must be declared and defined in one line. Undefined variables are not allowed in Python:

```python
# illegal syntax: undefined variable...
my_variable
# NameError: name 'my_variable' is not defined
```

### Reassigning Variables

Just like in JavaScript, we can freely assign a new value to a variable after it is declared.

```python
my_number = 15
print(my_number)
my_number = -4
print(my_number)
```

We can reassign our variable as many times as we want. However, only the most recent value of a variable will be retained. Once a variable is reassigned, its original value is lost forever, just like in JS.

### Naming Guidelines (Snake_Case)

Variables are case sensitive. The names `my_number` and `My_Number` aren't the same.

When you have a variable name with multiple words, you can combine all of the words together into one long variable name with the words separated by underscores. This is called "snake case" and is the conventional way we name identifiers in Python.

```js
// In JavaScript we use camelCase...
var myNumber = 10;
```
```python
# In Python we use snake_case...
ny_number = 10
```

# Data Types

Just like in JavaScript, variables have different types based on the values we assign to them. Python's data types are a little different than JavaScript's but you should see some similarities.

## Integers

Python provides the *integer* data type for storing whole numbers, or numbers without a fractional portion (no decimal point). All we need to do to make an integer variable is declare one:

```python
some_int = 25
```

We can force numbers to act as integers in some matematical operations, as we will see. Doing this can force an integer result which is usually the same as calling `Math.floor()` in JavaScript.

## Floating-point Numbers

Numbers with a decimal point are stored in variables with with floating-point numeric data type, usually just called `float`.

```python
some_float = 3.14159
```

Unlike JavaScript, Python actually has separate types for integers and floats which results in more reliability and precision when doing mathematical computations.

## Complex numbers

We won't get into this but Python has a data type for complex numbers: that is numbers with an "imaginary" component usually obtained by taking the square root of a negative number. The imaginary portion is represented by the letter 'j':

```python
cmplx_num = 3+4j
```

## Boolean types

Named after George Boole, these are the logical data types used in conditional expressions. Just like in JS, we have `true` and `false` but they are spelled a little differently:

```python
my_bool = True
my_other_bool = False
```

Notice how they start with capital letters! You must start them with capital letters in Python or they will not reflect the boolean values. We will play with these more in the Control Flow lesson.

## Nothingness

As JS has the `null` value to represent nothingness, Python has a similar value:

```python
my_nothing = None
```

The word `None` with a capital N provides the same meaning in Python.

## Math operations

Python has the normal math operators that you are used to from JS. Addition `(+)`, subtraction `(-)`, multiplcation `(*)`, division `(/)`, and modulus `(%)` all work as you would expect. However, there few other things worth mentioning:

### Exponentiation

While you must use `Math.pow()` in JS to raise a number to a power, Python includes an operator to do this:

```python
result = 4 ** 2
print(result)
# 16 is printed
```

The code above raises 4 to the power of 2. The result is 16. Raising something to any power works like this.

### Integer or Floor division

Earlier I said you could force operands to act like integers to get an integer result. This is specifically enabled for division:

```python
quotient = 5 // 2
print(quotient)
# 2 is printed, because the decimal ".5" is truncated
```

Using 2 slashes (`//`) forces integer division which will effectively chop off the decimal, reducing the result to the next lowest integer. Keep in mind that this is not rounding where the number sometimes rounds up and sometimes down based on the decimal value. This just removes the decimal portion of the result.

### Shortcut assignment operators

As we saw in JS, the operation of adding some number to another number is so common that there are a number of shortcut operators in the language that make it faster to write. Python has some of these but not all. Specifically, it has the shortcut for the following forms:

```python
# This line of code...
num = num + 1
# ...can be written with this shortcut operator:
num += 1

# It also works for any of the other math operations:
num = num / 5
# Rewrite like this:
num /= 5

# And this...
num = num * 3
# Can be written as this...
num *= 3
# And so on with the other operators.
```

That shortcut exists for all standard math operators. However, if you remember the JavaScript increment or decrement operator:

```js
num++;
// or...
num--;
```

THESE DO NOT EXIST IN PYTHON!!! Don't use them.

## Converting Between Data Types

This wasn't a big issue in JavaScript because JS does a lot of automatic conversion between types for us. However, in Python we cannot do this. With few exceptions, variables must be the same type to perform an operation on them. Doing math operations between integers and floats is allowed but can result in loss of precision if you aren't careful. Other than that, you must explicitly convert one of the variables to be the type of the other before carrying out operations such as addition or, more importantly, comparison.

Luckily, Python provides a bunch of global functions that we can use to convert to any type:

```python
int(item, base)  # Converts the provided item to an integer with the provided base
float(item)      # Converts the item to a floating-point number
hex(int)         # Converts an integer to a hexadecimal STRING
oct(int)         # Converts an integer to an octal STRING
tuple(item)      # Converts item to a tuple
list(item)       # Converts item to a list
dict(item)       # Converts item to a dictionary
str(item)        # Converts item to a string
```

## Strings

Python also has the `string` type for holding text, just like JavaScript:

```python
my_string = "A double quoted string"
your_string = 'A single quoted string'
```

You can also do some neat multi-line strings by using a triple quote (single or double):

```python
multiline_string = """ This is my string that
                        goes on multiple lines
                        for whatever reason """
```

### Concatenating strings

One or more strings can be combined into a single string in the same way we do it in JS: with the `+` operator.

```python
little_string = "bad"
medium_string = "super"
long_string = medium_string + little_string
print(long_string)
# prints "superbad"
```

### String interpolation with F-strings

One fancy thing that Python has had much longer than JavaScript is nice syntax for string interpolation, or injecting variables into your strings. While we can always use the concatenation operators above, these get ugly when too many of them appear in a string. Instead, we can use syntax similar to what was introduced in ES6 with string template literals. You just need to remember to add an "f" before the string:

```python
state = "Washington State"
year = 1889
n = 42
my_message = f"{state} was the {n}th state to join the union in {year}."
```

When the "f" is placed directly before the opening quotes of the string, it makes a formatted string or an `F-string` for short. Once we do this, we can put variable names into curly braces to "inject" that variable's value into our string.

F-strings are awesome! There's also a `format()` function on all strings. You can specify empty curly braces and pass values as parameters and the parameter values will fall in line in the order the curly braces appear.

```python
template = "My name is {} and I like {}"
template.format("Steve", "Cheese")
# prints 'My name is Steve and I like Cheese'
```

### String Methods
Just like JS, Python has a number of string methods that we can use for easy, reliable string manipulation. Some are familiar, like `split()` but others have different names:

```python
"ace of spades".split(" ")
# => ['ace', 'of', 'spades']

"abcde".split("")
# => ['a', 'b', 'c', 'd', 'e']

"qqxzzz".index("x")
# => 2

"boo".upper()
# => "BOO"

"WHY???".lower()
# => "why???"

"then I went to the store I like".replace("I", "you")
# => 'then you went to the store you like'
```

Want to search a string for a substring? You don't even need a function for that. You can use the special `in` keyword to quickly find out if one string appears in another.

```python
"eggs" in "green eggs and ham"
# => True
```

Use the `len()` operator on a string to find it's length.

```python
len("Hawaii")
# => 6
```

**Note** that in Python, `len()` is not a method of the string, it is a built-in function that exists in global scope, so you call it and pass in the thing you want to find the length of.

But hey wait - Doesn't `len()` return the length of a List (array)? Close! It actually returns the length of whatever kind of `sequence` you pass in. A List is a sequence of elements. In a similar way, a string is a sequence of characters. So when we pass a string into `len()` it will tell us how many characters it contains.

Because a string is a `sequence`, we can also use the square brackets that we use for List (array) access to access the characters in the string:

```python
my_name = 'steve'
print( my_name[0] )
# => Prints 's'

last_letter = my_name[4]
print(last_letter)
# => Prints 'e'
```

We will learn more about the cool things we can do with the square brackets `[]` later today, including passing in a negative index. What do you think this code will print?

```python
name = "Jason"
print( name[-1] )
```

### Additional Reading
- [Python For Beginners](http://www.pythonforbeginners.com/basics/python-variables)
- [Python Programming Tutorial: Variables](https://www.youtube.com/watch?v=vKqVnr0BEJQ)
- [Variables in Python](https://www.guru99.com/variables-in-python.html)
- [How To Use Variables in Python 3](https://www.digitalocean.com/community/tutorials/how-to-use-variables-in-python-3)
- [How To Do Math in Python 3 With Operators](https://www.digitalocean.com/community/tutorials/how-to-do-math-in-python-3-with-operators)
- [Modulus Operator](https://www.youtube.com/watch?v=MrTtsX2Wg9Q)
- [The Assignment Operator](https://www.youtube.com/watch?v=x6FiPx4Xl6A)
- [Python-Strings](https://www.tutorialspoint.com/python/python_strings.htm)
- [String Concatenation and Formatting](http://www.pythonforbeginners.com/concatenation/string-concatenation-and-formatting-in-python)
- [String Concatenation and Formatting - Video](https://www.youtube.com/watch?v=jA5LW3bR0Us)
