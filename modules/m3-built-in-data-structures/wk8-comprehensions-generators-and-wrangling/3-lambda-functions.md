# Lambda Functions

A function defined with `def` has a name, and the name is how it is called later. Some functions are never called later. They exist to be handed to something else, used once, and forgotten.

A **lambda** is a function written as a single expression, with no name.

## The Syntax

```
lambda parameters: expression
```

The keyword `lambda`, the parameters, a colon, and one expression. Compare the two forms of the same function:

```python
def square(number):
    return number * number

print(square(5))
print((lambda number: number * number)(5))
```

**Output:**

```
25
25
```

The brackets around the lambda group it so that `(5)` calls it. That is an unusual thing to write, and it is shown here only to make the point that a lambda is a function like any other.

Several parameters are separated by commas, and no parameters at all is allowed:

```python
add = lambda a, b: a + b
print(add(3, 4))

always_zero = lambda: 0
print(always_zero())
```

**Output:**

```
7
0
```

## There Is No return

A lambda's expression **is** its return value. Writing `return` inside one is a syntax error:

```python
double = lambda n: return n * 2
```

**Output:**

```
SyntaxError: invalid syntax
```

Remove the keyword and it works, because returning the expression is what a lambda does:

```python
double = lambda n: n * 2
print(double(21))
```

**Output:**

```
42
```

This is also why a lambda always returns something. There is no way to write one that returns `None` by falling off the end, as a `def` function can.

## What a Lambda Cannot Do

A lambda holds **one expression**. Everything that is a statement rather than an expression is unavailable:

- no assignment
- no `if` statement, `for` loop or `while` loop
- no `try`
- no `return`, `raise` or `pass`
- no docstring
- no type annotations

A conditional expression is allowed, because it is an expression:

```python
result = lambda mark: "pass" if mark >= 50 else "fail"
print(result(78))
print(result(32))
```

**Output:**

```
pass
fail
```

That is the limit of decision-making inside a lambda, and it is already close to the point where a named function reads better.

| | `def` | `lambda` |
| --- | --- | --- |
| Has a name | yes | no |
| Body | any number of statements | one expression |
| `return` | written explicitly | implied |
| Docstring | yes | no |
| Appears in a traceback as | its name | `<lambda>` |

The traceback row matters more than it looks. When a lambda raises, the error names `<lambda>`, so nothing in the message says which one:

```python
divide = lambda a, b: a / b
print(divide(1, 0))
```

**Output:**

```
ZeroDivisionError: division by zero
```

The traceback above that line says `in <lambda>`, and in a program with several, that is no help at all.

## Why Lambdas Exist

A function can be passed to another function, the same way a number or a list can. Here is one that takes a function as an argument and applies it to every item:

```python
def apply_to_all(items, action):
    return [action(item) for item in items]

print(apply_to_all([1, 2, 3, 4], lambda number: number * number))
print(apply_to_all(["anita", "ravi"], lambda name: name.title()))
```

**Output:**

```
[1, 4, 9, 16]
['Anita', 'Ravi']
```

Each lambda was written where it was needed and existed only for that call. Defining `square` and `titlecase` with `def` first would work and would put two names into the program that nothing else ever mentions.

This is the whole case for lambdas: a small piece of behaviour, supplied as an argument, used once. Several built-in functions are designed to take a function this way, and that is where lambdas are almost always found.

## Do Not Assign a Lambda to a Name

Every example above that names a lambda was written that way to demonstrate the syntax. In real code, do not:

```python
double = lambda n: n * 2
```

```python
def double(n):
    return n * 2
```

The second is the same length, gives the function a real name for tracebacks, allows a docstring, and is what the style guide calls for. A named lambda has all of `def`'s obligations and none of its advantages.

The rule follows from what a lambda is for. If the function needs a name, it is being kept, and anything being kept should be a `def`.

## Choosing Between Them

Use a lambda when all of these hold:

- the function is one short expression
- it is being passed as an argument
- it will not be used anywhere else
- what it does is obvious from reading it

Use `def` when any of them fails. In particular, a lambda that needs a comment, or that a reader has to work through twice, has already failed the last test:

```python
records = sorted(records, key=lambda r: (r["dept"], -r["salary"], r["name"].lower()))
```

That works, and it will be read many more times than it was written. Give it a name and a line of its own instead:

```python
def sort_key(record):
    """Sort by department, then highest salary, then name."""
    return (record["dept"], -record["salary"], record["name"].lower())

records = sorted(records, key=sort_key)
```

Both lines say what they do. Only the second says why.

## Further Reading

- **Official Python reference for lambda expressions** — https://docs.python.org/3/reference/expressions.html#lambda
- **Lambda functions in practice** — https://realpython.com/python-lambda/

A lambda is a nameless one-expression function, made for passing as an argument. Next, the two built-in functions that were designed to receive one.
