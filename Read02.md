# Testing and Modules

## In Tests We Trust — TDD with Python

TDD means Test-driven development which means the kind of development were you stick lots of step by step tests called unit tests

unit test looks like:

 `def function():`

   `detector = the_functions_tested()`

   `expected_value = detector.run('argument/s')`

   `assert expected_value == 'value_expected_to_be_returned'`

These tests are used in order to check your code

Units test is really important in order to metain your coding pase and keep on checking the result so you want face problems end start fixing bugs you could avoid just by check your inputs and outputs

It is always better to keep the tests code in a seperated folder from production code

Other thing to care about is the structure.convention widely used is the AAA: Arrange, Act and Assert:

**Arrange:** you need to organize the data needed to execute that piece of code (input)

**Act:** here you will execute the code being tested (exercise the behaviour)

**Assert:** after executing the code, you will check if the result (output) is the same as you were expecting.

An important thing about TDD is the cycle.

The cycle is made by three steps:

- Write a unit test and make it fail (it needs to fail because the feature isn’t there, right? If this test passes, call the Ghostbusters, really)
- Write the feature and make the test pass! (you can dance after that)
- Refactor the code — the first version doesn’t need to be the beautiful one (don’t be shy)

> TDD is not about the money/tests

## Recursion

![image](assets/Read02.png)

### What is Recursion

> The process in which a function calls itself directly or indirectly

### base condition in recursion

the solution to the base case is provided and the solution of the bigger problem is expressed in terms of smaller problems.

### particular problem is solved using recursion

represent a problem in smaller problems, and add conditions that stop the recursion.

### Stack Overflow error occurs in recursion

in case the base case never reached

### The difference between direct and indirect recursion

It is called direct when the function calls itself within it

And it is called indirect when the function calles another new functions that calls the same function again

> The recursive program has greater space requirements than iterative program as all functions will remain in the stack until the base case is reached. It also has greater time requirements because of function calls and returns overhead.

> Recursion provides a clean and simple way to write code.
