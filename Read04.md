#

## Classes and Objects

Objects are an encapsulation of variables and functions into a single entity

Objects get their variables and functions from classes.

Classes are essentially a template to create your objects.

Example:

`class MyClass:`
   `variable = "blah"`

   `def function(self):`
        `print("This is a message inside the class.")`

`myobjectx = MyClass()`

***Accessing Object Variables***
To access the variable inside of the newly created object "myobjectx" you would do the add:

`myobjectx.variable`

You can Create Multi Objects using the class 
just assign a new variable with the calling of your class

***Accessing Object Functions***

Same as the one used to access in the variables:

`myobjectx.function()`

## Thinking Recursively in Python

### Recursive Functions in Python

A recursive function is a function defined in terms of itself via self-referential expressions.

1. Decompose the original problem into simpler instances of the same problem

2. As the large problem is broken down into successively less complex ones, those subproblems must eventually become so simple that they can be solved without further subdivision

    - these are called the base casess

### Maintaining State

When doing the recursive functions remember to:

- Thread the state through each recursive call
- Keep the state in global scope

## Python Testing

#### Fixtures

you define fixtures using a combination of the pytest.fixture decorator, along with a function definition. For example, say you have a file that returns a list of lines from a file, in which each line is reversed:

`def reverse_lines(f):`
   `return [one_line.rstrip()[::-1] + '\n'`
           `for one_line in f]`

you'll need to pass it a file-like object.

Here's how that looks in pytest:

`@pytest.fixture`
`def simple_file():`
   `return StringIO('\n'.join(['abc', 'def', 'ghi', 'jkl']))`

fixtures are used differently from global variables.

if you want to include a fixture in one of your tests.
You can mention it in the test's parameter list. Then, inside the test, you can access the fixture by name.

##### Coverage

when software gets larger and more complex, it's not going to be so easy to eyeball it. That where you want to have "code coverage", checking that your tests have run all of the code.

> Now, 100% code coverage doesn't mean that your code is perfect or that it lacks bugs. But it does give you a greater degree of confidence in the code and the fact that it has been run at least once.

there's a package called ((pytest-cov)) on PyPI that you can download and install. Once that's done, you can invoke pytest with the --cov option. If you don't say anything more than that, you'll get a coverage report for every part of the Python library that your program used

`pytest --cov=mymul`

there is alot of extension but here an example:

`coverage html`

This creates a directory called htmlcov

Open the index.html file in this directory using your browser, and you'll get a web-based report showing (in red) where your program still lacks coverage. Sure enough, in this case, it showed that the even-number path wasn't covered. Let's add a test to do this:

`def test_even_numbers():`
  `with pytest.raises(NoEvenNumbersHereException):`
      `only_odd_mul(2,4)`

