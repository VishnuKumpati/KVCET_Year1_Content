# Lambda Functions

In comprehensions, you used expressions such as `score + 5` to transform values. Sometimes, another function needs a function as input. You could define that function separately using `def`, but for a simple calculation used in one place, that can mean extra code.

Python's `lambda` keyword lets you write the function directly where it is needed.

```python
add_bonus = lambda score: score + 5
print(add_bonus(87))
```

**Output:**

```
92
```

> **A lambda function is a function defined using the `lambda` keyword.** It can accept multiple parameters but contains only one expression, whose value is returned automatically.

## Syntax of a Lambda Function

```
lambda parameters: expression
```

Every part of the line has a job.

**Example**

```python
square = lambda x: x * x
print(square(5))
```

**Output:**

```
25
```

- `square` is the variable the function is stored in, so it can be called later
- `lambda` is the keyword that defines the function
- `x` is the parameter that receives the input
- `:` separates the parameters from the expression
- `x * x` is the expression whose value is returned

The call `square(5)` passes `5` as the argument, so `x` receives `5`. The expression `x * x` gives `25`, and that value is returned.

The same function can be written with `def`.

```python
def square(x):
    return x * x
```

Both create the same function. The lambda version is shorter, but `def` is generally clearer when a function needs a meaningful name for reuse or more complex logic.

## Lambda with Multiple Parameters

Several parameters are separated by commas, exactly as in a `def` function.

**Syntax**

```
lambda parameter1, parameter2: expression
```

**Example**

```python
total = lambda x, y, z: x + y + z
print(total(10, 20, 30))
```

**Output:**

```
60
```

Each value in the call is matched to a parameter by its position, so `x` receives `10`, `y` receives `20` and `z` receives `30`.

## Single Expression Restriction

A lambda can contain only one expression. If the function needs more logic than can be written in one expression, use a normal `def` function.

A conditional expression still counts as one expression, so it can go inside a lambda.

**Example**

```python
status = lambda score: "Pass" if score >= 35 else "Fail"
print(status(87))
print(status(30))
```

**Output:**

```
Pass
Fail
```

## Passing a Lambda to Another Function

This is where lambdas become especially useful.

`sorted()` takes a `key` argument, which is a function. `sorted()` calls that function on every item and sorts using the value returned by the key function.

```python
names = ["Meera", "Asha", "Ravi"]
print(sorted(names, key=lambda name: len(name)))
```

**Output:**

```
['Asha', 'Ravi', 'Meera']
```

`sorted()` needs a value to use for comparison. The `key` function provides that value.

For each name, the lambda returns its length:

```
"Meera" → 5
"Asha"  → 4
"Ravi"  → 4
```

`sorted()` then sorts the names using those lengths.

`Asha` and `Ravi` both have four letters. When two items compare equal, `sorted()` keeps them in the order they appeared in the original list.

We could define a separate function using `def` and pass its name to `sorted()`. Here, the lambda lets us write the sorting logic directly inside the `key` argument.

## Choosing Between `def` and `lambda`

Use `def` when you need a named function that can be reused or needs more logic.

Use `lambda` when you need a simple function directly inside another expression, such as the `key` argument of `sorted()`.

## Further Reading

- 📎 **Lambda functions with worked examples** — https://www.programiz.com/python-programming/anonymous-function
- 📎 **Official Python guide to lambda expressions** — https://docs.python.org/3/tutorial/controlflow.html#lambda-expressions

A lambda is especially useful when passed to a function that accepts another function. Two built-in functions commonly used this way are `map()`, which applies a function to each item, and `filter()`, which keeps items for which the function returns a truthy value.

Next, you will learn `map()` and `filter()`.
