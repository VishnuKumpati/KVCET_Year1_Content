# Generator Expressions

A generator function is defined using `def` and contains at least one `yield`. When you only need to calculate a value for each item, you can often do it without writing a separate generator function.

A generator expression creates a generator in a single line.

```python
scores = [87, 72, 95]
bonus = (score + 5 for score in scores)
```

> **A generator expression creates a generator object from a single expression.** It is written like a list comprehension, but with round brackets in place of square brackets. Instead of building a list, it produces values one at a time as they are requested.

## List Comprehension and Generator Expression

The two forms look almost identical. Only the brackets differ.

```python
scores = [87, 72, 95]

bonus_list = [score + 5 for score in scores]
bonus_gen = (score + 5 for score in scores)

print(bonus_list)
print(type(bonus_gen))
```

**Output:**

```
[92, 77, 100]
<class 'generator'>
```

Square brackets created a list with all three values calculated immediately. Round brackets created a generator object; each value is calculated when requested, although the iterable expression is evaluated when the generator expression is created.

## Syntax of a Generator Expression

```
(expression for variable in iterable)
```

The `for` part and the `if` part work exactly as they do in a list comprehension.

```python
scores = [87, 72, 95]
bonus = (score + 5 for score in scores)

for value in bonus:
    print(value)
```

**Output:**

```
92
77
100
```

Each value was calculated at the moment the loop requested it.

A condition filters the values in the same way.

```python
scores = [87, 32, 72, 25, 95]
passed = (score for score in scores if score >= 35)
print(list(passed))
```

**Output:**

```
[87, 72, 95]
```

## Requesting One Value at a Time

`next()` works on a generator expression exactly as it does on a generator function.

```python
scores = [87, 72, 95]
bonus = (score + 5 for score in scores)

print(next(bonus))
print(next(bonus))
```

**Output:**

```
92
77
```

Two values were requested and produced. The third value has not been calculated yet because it has not been requested.

## A Generator Expression Is Consumed

Like any generator object, it can be consumed until it is exhausted. Once that happens, it cannot be restarted.

```python
scores = [87, 72]
bonus = (score + 5 for score in scores)

print(list(bonus))
print(list(bonus))
```

**Output:**

```
[92, 77]
[]
```

The first `list()` collected both values. The second found none remaining.

## No Indexing and No Length

A generator does not provide list-style indexing or `len()`. It produces values as they are requested rather than storing them in a collection.

```python
scores = [87, 72]
bonus = (score + 5 for score in scores)
print(len(bonus))
```

**Output:**

```
TypeError: object of type 'generator' has no len()
```

```python
scores = [87, 72]
bonus = (score + 5 for score in scores)
print(bonus[0])
```

**Output:**

```
TypeError: 'generator' object is not subscriptable
```

A list comprehension is the right choice when the values need to be indexed, counted, or read more than once.

## Passing a Generator Expression to a Function

When a generator expression is the only argument to a function, its round brackets are not written separately.

```python
scores = [87, 72, 95]
print(sum(score + 5 for score in scores))
```

**Output:**

```
269
```

`sum(score + 5 for score in scores)` is the same as `sum((score + 5 for score in scores))`, with one pair of brackets left out.

This is a common use of a generator expression. Functions such as `sum()`, `max()` and `min()` process one value at a time, so building a list first is unnecessary.

## Choosing Between the Two Forms

| List comprehension | Generator expression |
| --- | --- |
| Written with square brackets. | Written with round brackets. |
| Calculates every value immediately. | Calculates values as they are requested. |
| Can be reused, indexed and measured with `len()`. | Is consumed as values are requested; supports neither indexing nor `len()`. |
| Useful when values need to be accessed repeatedly. | Useful when values are passed straight into a loop or a function. |

## Further Reading

- 📎 **Generator expressions with worked examples** — https://www.programiz.com/python-programming/generator
- 📎 **Official Python guide to generator expressions** — https://docs.python.org/3/tutorial/classes.html#generator-expressions

Generator expressions can help process data one value at a time. But real-world data often contains missing, inconsistent, or incorrectly formatted values that need to be cleaned and reshaped.

Next, you will learn about data wrangling.
