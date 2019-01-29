How to Do a Good Code Review:

Code Reviewers should...

* Review small chunks of code at a time
    * 200 - 400 lines of code (LOC) at a time
    * Not more than 400 LOC per hour - TAKE YOUR TIME!
    * No more than an hour at a time without a break
* Review it all
* Everyone participates so they can learn
* Have a positive attitude
* BE KIND! Remember, coders are proud of their work.
* Use a checklist (like below) so that nothing is missed and nothing unimportant is added

Code Writers should…

* Annotate the code. Comment the hell out of it.
* MINDSET: Look forward to finding out the new and best ways to write code


Checklist:

General

* Does the code work?
* Is the code easily readable?
* Is the location of braces, variable and function names, line length, indentations, formatting, and comments consistent within the code and with company style guides.
* Is there any repeated code?
* Is the code as modular as possible? (Can it be taken and used elsewhere without refactoring.)
* Can any global variables be moved to a narrower scope?
* Is there any commented out code?
* Do loops have a set length and correct termination conditions?
* Have console logging and other debugging statements been removed?

Security (Haven’t really covered)

* Are all data inputs checked (for the correct type, length, format, and range) and encoded?
* Where third-party utilities are used, are returning errors being caught?
* Are output values checked and encoded?
* Are invalid parameter values handled?

Documentation

* Do comments exist and describe the intent of the code?
* Are all functions commented?
* Is any unusual behavior or edge-case handling described?
* Is the use and function of third-party libraries documented?
* Are data structures and units of measurement explained?
* Is there any incomplete code? If so, should it be removed or flagged with a suitable marker like ‘TODO’?

Testing (Haven’t covered yet)

* Is the code testable? i.e. don’t add too many or hide dependencies, unable to initialize objects, test frameworks can use methods etc.
* Do tests exist and are they comprehensive? i.e. has at least your agreed on code coverage.
* Do unit tests actually test that the code is performing the intended functionality?
* Are arrays checked for ‘out-of-bound’ errors?
* Could any test code be replaced with the use of an existing API?


You are responsible for thes items today:

General

* Is the code easily readable?
* Is the location of braces, variable and function names, line length, indentations, formatting, and comments consistent within the code and with company style guides.
* Is there any repeated code?
* Is there any commented out code?
* Have console logging and other debugging statements been removed?

Documentation

* Do comments exist and describe the intent of the code?
* Are all functions commented?
* Are data structures explained?
