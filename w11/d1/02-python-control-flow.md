# Control Flow in Python

You've learned how to use statements like `if`, `else`, and `switch` to create conditional branches in your code. You've used `loops` to repeat chunks of code and used `functions` to separate and organize reusable blocks of code. The good news is that many programming languages share these same logical constructs. Typically, learning a new language amounts to nothing more that learning a new syntax and a few idiosyncracies about it. In the abstract, these constructs and statements work in the same way across lanaguages so once you know how they are used, picking up these concepts in a new language is usually much easier.

## Logic in Python

All of this control flow stuff is based on logical values and logical operators so let's review a bit of that.

### Boolean Values

Python has two logical values: `True` and `False` (case-sensitive). Most logical operations result in one of these two values (or truthy or falsey values). The work exactly the same as in JS but are always written with a starting capital letter in Python.

### Logical operators

Python has all the same less-than, greater-than, and equality operators as JavaScript: `(<, >, <=, >=, ==, !=)`. The one big difference is that *there is no non-strict equality operator*. Every comparison that we do will compare the type **and** the value of the variable just like the triple-equals `===` in JavaScript.

```python
8 > 8
# => False — 8 is not greater than 8.

8 >= 8
# => True — This checks if 8 is greater than or equal to 8, and they are equal.

8 < 8
# => False — 8 is not less than 8.

7 == 7
# => True — 7 is equal to 7.

7 == "7"
# => False — One is a number and the other is a string.

7 != 7
# => False — This checks if they aren't equal. Because does 7 equal 7, it's `False`.

6 != 7
# => True — 6 is not equal to 7.

# Note that in addition to the != operator, you can also use this for inequality
6 <> 7
# => True - 6 is less than or greater than 7.
```

### Truthy & Falsey in Python

Like JS, Python has some values and objects that always evaluate to `False` or `True`. These are called "truthy" or "falsey" values and they work the same way that they do in JS. Here are the falsey values in Python:

1. Constants defined to be False: `None` and `False`
2. Zero in any numeric type: `0`, `0.0`, `(0j)`
3. Empty sequences or collections: `''`, `[]`, '()', '{}', range(0)

### OR and AND

We saw in JavaScript that we could connect conditional expressions by using the OR `||` operator or the AND `&&` operator. If two expressions are connected with OR then one OR the other may be true for the whole expression to be true. If connected with AND then both expressions need to be true.

Thankfully, those weird operators are not present in Python. We just use the normal English words `and` and `or`:

```python
7 == 7 or 8 > 12
# => True, the left side is true but not the right

7 != 7 or 18 < 1
# => False, neither side is true

True and 12 > 6
# => True, both sides are true

True and 12 < 6
# => False, both sides need to be true.
```

# Control Flow

## The `if` Statement

Just like in JavaScript, we have the ability for our programs to do different things based on some condition. We can make Python do one thing **if** some condition is true and do a different thing (or nothing) **if** the condition is false. Here is the Python `if` statement:

```python
floor = "sticky"
walls = "clean"
if floor == "sticky":
  print("Clean the floor! It's sticky!")
if walls == "sticky":
  print("Clean the walls! They're sticky!")
```

## INDENTATION AND WHITESPACE!!!

Now is a fantastic time to talk about indentation in Python, or more specifically, whitespace. Doubtless, all of you remember that we begin and end code blocks in JavaScript with curly braces. With few exceptions, `if` statements, `loops`, and `functions` in JS all start and end with a curly bracket. Not so in Python! Python instead uses indentation, or the amount of whitespace before the first character on a line, to determine what block or scope you are in. It uses the colon at the end of a line (`:`) to start the block, and every line that is indented to the same level after that colon is part of that code block. Once the lines cease to be indented after the colon, the block has ended.

With that in mind, here is the syntax of a Python `if` statement:

```python
if condition:
  # Run these lines if condition is True
  # Every line indented to the same level is part of this "if"
```

Notice how we do not need parentheses around the conditional expression. Immediately after the expression, we put a colon which starts the block. Each line indented under the top line of the `if` is in the block and will be executed if True or skipped if False.

## The `else` Statement

Using the `if` statement like above gives us a situation where the script will do something if the condition is true but it will do nothing if the condition is false. What if we want it to do one thing if it's true and a completely different thing if it is false? Python gives us the `else` statement. It has this basic structure:

```python
if condition:
  # do something
else:
  # do something else
```

This works exactly the same as a simple `if` statement except that it adds `else` and another line that will be executed only if the condition evaluates to False.

## The `elif` Statement

Yep, Python has an `else if` also. However, they got really terse with it and called it `elif`. However, it has the syntax you probably expect:

```python
if condition1:
  # do something
elif condition2:
  # do something else
elif condition3 and condition4:
  # do another thing entirely
else:
  # else do this
```
