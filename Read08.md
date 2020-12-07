# List Comprehensions in Python

> It consists of brackets containing an expression followed by a for clause, then
zero or more for or if clauses.

The result will be a new list resulting from evaluating the expression in the
context of the for and if clauses which follow it.

Example:

`[ expression for item in list if conditional ]`

This is equivalent to:

`for item in list:`
    `if conditional:`
        `expression`

By breaking it down we find:

**expression(i):** Expression is based on the variable used for each element in the old list.

**for i in old_list:** The word for followed by the variable name to use, followed by the word in the old list.

**if filter(i):** Apply a filter with an If-statement.

### Examples

***Create a simple list***

`x = [i for i in range(10)]`
`print x`

This will give the output: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

***Lower/Upper case converter***

`[x.lower() for x in ["A","B","C"]]`

result: ['a', 'b', 'c']

***Print numbers only from a given string***

`string = "Hello 12345 World"`
`numbers = [x for x in string if x.isdigit()]`
`print numbers`

result: ['1', '2', '3', '4', '5']

## Conclusion

*It Is As Simple As You Can Imagine*
