# Intro to Objects and Classes

## Learning Objectives
* Describe the difference between classes and objects
* Use Python syntax to define a class
* Instantiate an object from a class
* Explain the use of the `__init__` method
* Utilize the `self` keyword in the scope of class variables and methods
* Understand class variables and how they differ from instance variables

# Objects and Classes

Python is an **object oriented programming language**. Object oriented programming is characterized
by the creation and use of collections of data and behavior that act like everyday physical objects.
Every day we interact with objects like chairs, beverages, cars, other people, etc. These objects have
properties that define them and behaviors we can execute to interact with them. What are some examples
of properties? Well, a chair could perhaps be wood or plastic. Cars have different shapes, colors, and
speeds. Beverages could be hot or cold. And behaviors or actions of the objects would include things
like sitting in chairs, driving cars, or drinking and refilling beverages. Over many years,
programmers have found that designing systems to reflect discrete, everyday objects makes the systems
easier to understand, write, test, and maintain.

You will hear people say that "everything is an object" in Python. What this means is that nearly
every variable you declare already has a set of properties and functions that it can use because it
is an object. Every string, number, list, dictionary, etc. has a set of behaviors and properties that
are "baked-in" because they are instances of a class. For example, every list you declare is an
instance of a built-in Python List class and because of that you can use any of the built-in list
functions like append() and pop() on your own list.

So what is a class? A class is the blueprint for an object. Let's think about the car: all cars have
things that make them a car:

* Has an engine
* Has tires
* Has doors
* Ability to drive
* Ability to park

But when we make a car, we vary the values of these properties. Some cars have economy engines, some
have performance engines. Some have 4 doors, others have 2. The blueprint or specification for the car
is the **class** and each car that we make from that blueprint is the **object**.

Making our own classes to organize data in the ways we want is a very common thing in programming.
Python gives us, for example, a string and an integer, but if we want to store a bunch of data where
each item contains a string **and** an integer **and** a function that prints them out nicely, we
can make a class for that and then each object we make from that class will have all that baked-in.

## Codealong: Write a basic class

I like dogs. Here's what a Dog class definition might look like in Python:

```python
class Dog():
  def __init__(self, name="", age=0):
    self.name = name
    self.age = age

  def bark_hello(self):
    print("Woof! I am called", self.name, "and I am", self.age, "human-years old.")

```

The `Dog` class is a collection of **variables** and **methods**. The `__init__` method is a special
method Python executes when a new dog is created. It is short for "initialize" and is used to set
the values of the properties of the object. We will talk about this shortly. The variables in
this class are `self.name` and `self.age`. The method in this class is `bark_hello`.

## Instantiating Objects From Classes

By defining the dog class we now know the structure that each of our dogs will have: each has a name,
an age, and each can bark Hello! But how do we make new dogs? We call our class name as a function:

```python
gracie = Dog("Gracie", 8)
```

This will create a new object according to our Dog class specification. When that happens, Python runs
our `__init__` method to initialize the object. Here, we are telling our `__init__` method to set the
name of this dog to 'Gracie' and set her age to 8 years old.

> So, what's this `self` keyword inside the `__init__` method? `self` can be a strange programming paradigm. Imagine that you are an object. You represent the `self.` If you wanted to access your arm, it would be self.arm. Your name would probably be stored in self.name. This lets each object made from a class keep reference to its own data and function members. Not every "Person" has the same name so we want individual People to maintain their own names. The `self` keyword is each object's identity.

When we make objects from this class, the `self` keyword inside each object refers to that specific object. This allows each object made from a class to maintain its own copies of variables. Any method in our class that will need to use the `self` keyword (like our `bark_hello` method which uses both `self.name` and `self.age`) needs to have the `self` keyword passed in as a parameter to the method. That's why we include `self` in our `__init__` and `bark_hello` methods.

Let's make a couple more dogs so we can see the differences in our dog objects:

```python
spitz = Dog("Spitz", 5)  # lead dog
buck = Dog("Buck", 3)  # upstart newcomer
```

Each dog has its own name and age. Let's have them say hello:

```python
gracie.bark_hello()  # Woof! I am called Gracie and I am 8 human-years old.'
spitz.bark_hello()   # Woof! I am called Spitz and I am 5 human-years old.'
buck.bark_hello()    # Woof! I am called Buck and I am 3 human-years old.'
```

Notice how, even though `self` is listed as a parameter for the `bark_hello` function, we don't
pass it into the function. It happens automatically. Clearly, there is some magic at work here.
Let's add some more code to better see what is going on behind the scenes.

## Code-Along: Write a class that prints a string when instantiated

We are going to extend our Dog class, a bit. Let's give our dogs a color attribute. We need to
add a `self` variable for the color and we need to add a color parameter in the `__init__`
method. Now add a line to print out name and the object address when `__init__` fires. Lastly,
add your dog's color to their hello function. Now your Dog class looks like this:

```python
class Dog():
  def __init__(self, name="", age=0, color=""):
    self.name = name
    self.age = age
    self.color = color
    print(name, "created:", self)

  def bark_hello(self):
    print("Woof! I am called", self.name, "I am", self.color "and I am", self.age, "human-years old.")
```

With our class refactored, we will see new behavior. Add some colors to your dog instantiation
lines and then run the code to see what is output when we create new dog objects.

```python
gracie = Dog("Gracie", 8, "black") # Gracie created: <__builtin__.Dog instance at 0x7fecda802a70>
spitz = Dog("Spitz", 5, "white")  # Spitz created: <__builtin__.Dog instance at 0x7fecda802b48>
buck = Dog("Buck", 3, "brown")  # Buck created: <__builtin__.Dog instance at 0x7f6a23e93b90>

gracie.bark_hello()  # Woof! I am called Gracie I am black and I am 8 human-years old.'
spitz.bark_hello()   # Woof! I am called Spitz I am white and I am 5 human-years old.'
buck.bark_hello()    # Woof! I am called Buck I am brown and I am 3 human-years old.'
```

So we can see that each dog is created at a new address in memory. Each dog maintains its own
name, age and color. And each dog prints its name and memory address when it is created. The
`__init__` method will always execute once and only once when you create a new object from a class.

## Class vs instance members

In our `Dog` class, we have variables attached to the `self` property that exist
independently for each object that's created. These are called **instance variables**.
Each object instance has its own copies of these variables and they can vary across
objects. We can also attach variables to the class itself so that there's one single thing that
exists for an entire class. These are called **class variables**.

Suppose we want to keep a tally of how many dogs we have running around in our app.
We could put a copy of the tally in each dog object but that's not efficient: we would
be duplicating a value in memory multiple times and we would have to update the value in every dog
object in order to keep it accurate. It's much better if we can store it once in the class.
That way, each dog object can access it but we only need to store it and set it in one place.

Let's add some code to our class. To create a class variable, we simply add a variable outside
of any existing functions. Let's add a `totalDogs` variable to the class. Let's also add a
line that increments this value inside our `__init__` method. And just for fun, let's add
another line to our `bark_hello` method that references this total:

```python
class Dog():
  totalDogs = 0
  def __init__(self, name="", age=0 color=""):
    self.name = name
    self.age = age
    self.color = color
    Dog.totalDogs += 1
    print(name, "created:", self)

  def bark_hello(self):
    print("Woof! I am called", self.name, "I am", self.color "and I am", self.age, "human-years old.")
    print("There are", Dog.totalDogs, "dogs in this room!")
```

Now when we create a new dog, the `__init__` method increments the `totalDogs` counter which
is stored in the Dog class itself. We can access the value stored in `Dog.totalDogs` inside our script
and each dog object can access it from their own functions.

## Subclasses and Inheritance

Python also provides a way for you to make a class "inherit" functionality from a different "parent" class. If you had a general class like `Dog` and you decided that you wanted to represent the more specific characteristics of certain breeds, you could create individual breed classes that each inherited all the members of `Dog` like `name`, `age`, `color`, and `bark_hello()`. Or perhaps having `Dog` be a subclass of an even-more-general `Animal` class. When we organize our classes like this we can save ourselves the time of writing code over again. All we need to do to get all the stuff in one class is to inherit it into a child class and then we can add or change anything we want and our changes are specific to this new child class.

We can see examples of inheritance in both JavaScript and Python. In JavaScript, the `HTMLElements` that we get when we call `document.getElementById()` seem like they have an `innerHTML` property but this is an inherited property from its parent class (or super class) `Element`, which itself is a subclass of `Node` which is actually where `textContent` comes from. Because `HTMLElement` is a descendent of these super classes, it can access all of these properties. It has inherited them.

In Python, we have strings and lists. Each one has a grandparent class called `sequence` which is the thing that `len()` works on. Strings and lists inherit this characteristic from `sequence` but each one has a different parent class. Sequence has two child classes: `mutable` and `immutable`. String inherits from `immutable` and List inherits from `mutable`.

Here is how we make it work:

```python
class Parent():
  def __init__(self):
    self.first_name = "Lorelei"
    self.last_name = "Gilmore"
    print("Parent initialized:", self)
  def hello(self):
    print(f"Hey, I'm {self.first_name}. Welcome to the Dragonfly!")

class Child(Parent):
  def __init__(self):
    Parent.__init__(self)
    self.first_name = "Rory"
    print("Child initialized:", self)

mom = Parent()
daughter = Child()

mom.hello()
daughter.hello()

print(daughter.last_name)
```

After examining this code, we see that the `Child` class doesn't actually contain a `hello()` method or a `last_name` property. But we can access those members because they are inherited from the `Parent` class. This shows how we can automatically add the parent's members to our class but it also illustrates a problem: The `mom` and `daughter` objects are maintaining their own `first_name`s but they say the same line when we call `hello()`.

We can change the behavior by `overriding` the function we inherited from the parent. Update `Child` to the following:

```python
class Child(Parent):
  def __init__(self):
    Parent.__init__(self)
    self.first_name = "Rory"
    print("Child initialized:", self)
  def hello(self):
    print(f"I'm {self.first_name}. Can't talk, late for class!")
```

Now the daughter says something more in line with her character. The child class still inherited the `hello()` method but the functionality we put into `Child` overrode what was inherited.

## Exercise: Create Your Own Python Classes

Now that we've seen a dog-based example of Python classes, let's make one a bit more sophisticated.
Create a `BankAccount` class.  

* Bank accounts should be created with the `accountType` property (like "savings" or "checking").
* Bank accounts should have a class-level variable tracking the total amount of money in all account objects.
* Each account should keep track of its own current `balance`.
* Each account should have a `deposit` and a `withdraw` method.
* Each account should start with its `balance` set to zero.

Things to think about:
* Any starting values for variables should be set in the `__init__` method
* Class variables are declared inside the class but outside any methods
* Instance variables are declared inside the `__init__` method
* Does your `__init__` method need to accept any parameters?

Bonus: 

1. Start each account with an additional `overdraftFees` property that starts at zero. If a call
to `withdraw` ends with the `balance` below zero then `overdraftFees` should be incremented by twenty.
2. Make a `ChildBankAccount` class that inherits from `BankAccount`. The `ChildBankAccount` should override the `withdraw` method so that no overdraft fees are applied. Instead, output a message that there are insufficient funds available.


## Closing Thoughts
* A Class is a pre-defined structure that contains attributes and behaviors that are grouped together logically. (All dogs have traits and actions they can do.)
* An Object is an instance of a Class structure.  A class could be thought of as a house blueprint and an object as a house built from the blueprint.
* Classes are defined via a method call. Classes contain an `__init__` method that takes in parameters to be assigned to an Object.
* Instance variables contain data types declared in the class but defined in each object. (Each dog has its own name.)
* Class variables and methods contain data and actions that span across all Objects. (How many dogs are there in total.)
* The `self` keyword lets us distinguish between variables that exist at the class level versus in each object.
* A Class can inherit properties and methods from another Class.