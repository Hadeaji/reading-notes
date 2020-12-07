# Python Scope

### Modules: The Global Scope

when you first start coding you are coding on the Global Scope

Python turns your program’s main script into a module called `__main__` to hold the main program’s execution.
Whenever you run a Python program or an interactive session like in the above code, the interpreter executes the code in the module or script that serves as an entry point to your program.

To inspect the names within your main global scope, you can use `dir()`

If you call `dir()` without arguments, then you’ll get the list of names that live in your current global scope.

You can access or reference the value of any global name from any place in your code.

If you try to assign a value to a global name inside a function, then you’ll be creating that name in the function’s local scope, shadowing or overriding the global name. This means that you won’t be able to change most variables that have been defined outside the function from within the function.

Modifying global names is generally considered bad programming practice because it can lead to code that is:

- **Difficult to debug:** Almost any statement in the program can change the value of a global name.
- **Hard to understand:** You need to be aware of all the statements that access and modify global names.
- **Impossible to reuse:** The code is dependent on global names that are specific to a concrete program.

Good programming practice recommends using local names rather than global names. Here are some tips:

- ***Write*** self-contained functions that rely on local names rather than global ones.
- ***Try*** to use unique objects names, no matter what scope you’re in.
- ***Avoid*** global name modifications throughout your programs.
- ***Avoid*** cross-module name modifications.
- ***Use*** global names as constants that don’t change during your program’s execution.

## Modifying the Behavior of a Python Scope

### The global Statement

when you try to assign a value to a global name inside a function, you create a new local name in the function scope.
To modify this behavior, you can use a **global statement**

In **global statment** you can define a list of names that are going to be treated as global names.

Example:

`counter = 0`
`def update_counter():`
    `global counter`
    `counter = counter + 1`

`update_counter()`
`counter`
1

`update_counter()`
`counter`
2

`update_counter()`
`counter`
3

You can also use a global statement to create lazy global names by declaring them inside a function.Ex:

`def create_lazy_name():`
    `global lazy`
    `lazy = 100`
    `return lazy`

`create_lazy_name()`
100

`lazy`
100

`dir()`
['__annotations__', '__builtins__',..., 'create_lazy_name', 'lazy']

### The nonlocal Statement

nonlocal names can be accessed from inner functions, but not assigned or updated.

If you want to modify them, then you need to use a nonlocal statement.

The following example shows how you can use nonlocal to modify a variable defined in the enclosing or nonlocal scope:

`def func():`
    `var = 100  # A nonlocal variable`
    `def nested():`
        `nonlocal var  # Declare var as nonlocal`
        `var += 100`
    `nested()`
    `print(var)`
`func()`

200

Unlike global, you can’t use nonlocal outside of a nested or enclosed function.
Since nonlocal only works inside an inner or nested function, you get a SyntaxError telling you that you can’t use nonlocal in a module scope.

you can’t use nonlocal to create lazy nonlocal names. Names must already exist in the enclosing Python scope if you want to use them as nonlocal names. This means that you can’t create nonlocal names by declaring them in a nonlocal statement in a nested function.
