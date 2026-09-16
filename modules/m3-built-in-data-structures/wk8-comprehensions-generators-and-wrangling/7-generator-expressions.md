# Generator Expressions

A generator function is written with `def` and `yield`, which is a lot of structure for a simple rule. When the rule fits in one expression, the comprehension form supplies it.

Swap a comprehension's square brackets for round ones and it produces a generator instead of a list.

## The Syntax

```python
squares_list = [number * number for number in range(5)]
squares_gen = (number * number for number in range(5))

print(squares_list)
print(squares_gen)
```

**Output:**

```
[0, 1, 4, 9, 16]
<generator object <genexpr> at 0x000001F4A2C60C80>
```

The list shows its values because it has them. The generator shows an object, because nothing has been computed yet. The hexadecimal number is a memory address and differs every run.

The values arrive when asked for:

```python
squares_gen = (number * number for number in range(5))
print(list(squares_gen))
```

**Output:**

```
[0, 1, 4, 9, 16]
```

Everything else about the comprehension form is unchanged. The expression, the `for` clause and the optional condition work exactly as they do in a list comprehension:

```python
numbers = [4, -2, 7, 0, -9, 3]
positives = (number for number in numbers if number > 0)
print(list(positives))
```

**Output:**

```
[4, 7, 3]
```

And the one-pass rule applies, because a generator expression produces an iterator:

```python
squares = (number * number for number in range(5))
print(list(squares))
print(list(squares))
```

**Output:**

```
[0, 1, 4, 9, 16]
[]
```

## The Memory Difference

The difference is not small. A million squares as a list, and as a generator expression:

```python
import sys

print(sys.getsizeof([number * number for number in range(1000000)]))
print(sys.getsizeof((number * number for number in range(1000000))))
```

**Output:**

```
8448728
200
```

Eight and a half megabytes against two hundred bytes.

The generator's size does not depend on how many values it will produce, because it holds none of them — only the paused expression and its position. A generator over ten items and one over ten billion are the same size.

There is a time difference too. The list comprehension computes a million multiplications before the next line runs. The generator expression computes nothing until something asks.

## Passing One Straight into a Function

A generator expression that is the only argument to a function needs no brackets of its own. The function's brackets serve:

```python
numbers = [1, 2, 3, 4, 5]
print(sum(number * number for number in numbers))
print(max(number * number for number in numbers))
print(sorted(number * number for number in [3, 1, 2]))
print(", ".join(str(number) for number in numbers))
```

**Output:**

```
55
25
[1, 4, 9]
1, 2, 3, 4, 5
```

Each of these would work with a list comprehension, and each would build a list that is consumed once and discarded. The generator form does the same work without the list.

When there is a second argument, the generator needs its own brackets:

```python
numbers = [1, 2, 3]
print(sum((number for number in numbers), 100))
```

**Output:**

```
106
```

Without them, Python cannot tell where the generator expression ends and the next argument begins.

`", ".join(str(n) for n in numbers)` is worth recognising. Converting a list of numbers for joining was a three-line loop earlier; this is the form it usually takes.

## any and all

Two built-in functions pair particularly well with generator expressions. `any()` returns `True` if at least one item is true, and `all()` returns `True` only if every item is:

```python
marks = [78, 45, 91]
print(any(mark < 50 for mark in marks))
print(all(mark >= 40 for mark in marks))
print(all(mark >= 50 for mark in marks))
```

**Output:**

```
True
True
False
```

These read almost as English: *is any mark below fifty*, *are all marks at least forty*.

Both stop as soon as the answer is settled, and with a generator that means the remaining values are never computed at all:

```python
def noisy(values):
    for value in values:
        print("checking", value)
        yield value

print(any(value > 2 for value in noisy([1, 2, 3, 4, 5])))
```

**Output:**

```
checking 1
checking 2
checking 3
True
True
```

Five values were available and three were examined. `4` and `5` were never produced.

With a list comprehension in place of the generator, all five would be computed first and then the same answer reached. When each value is expensive — a file read, a network call — that difference is the whole point.

## Choosing Between a List and a Generator

| | List comprehension | Generator expression |
| --- | --- | --- |
| Brackets | `[ ]` | `( )` |
| When values are computed | immediately, all of them | on request, one at a time |
| Memory | grows with the number of items | constant |
| Reusable | yes, any number of times | no, single pass |
| Supports `len()`, indexing, slicing | yes | no |

Use a **list** when the values are needed more than once, when indexing, slicing, sorting or `len()` is required, or when the collection is small enough that none of this matters. Most of the time, this is the right answer.

Use a **generator** when the values are passed through once, when there are many of them, or when computing a value that may never be needed is wasteful.

The clearest signal is the shape of the code. A comprehension written directly inside `sum()`, `max()`, `any()`, `all()`, `join()` or a `for` loop is consumed once, and the round brackets are free:

```python
students = [
    {"name": "Anita", "mark": 78},
    {"name": "Ravi", "mark": 45},
    {"name": "Meera", "mark": 91},
]

print(sum(student["mark"] for student in students) / len(students))
print(any(student["mark"] < 50 for student in students))
print(max(student["mark"] for student in students))
```

**Output:**

```
71.33333333333333
True
91
```

A comprehension assigned to a name and used again below should be a list, because it will be read more than once and a generator would be empty the second time.

## Further Reading

- **Official Python reference for generator expressions** — https://docs.python.org/3/reference/expressions.html#generator-expressions
- **Generator expressions in practice** — https://realpython.com/introduction-to-python-generators/

A generator expression is a comprehension in round brackets, producing values on demand instead of building a list. Next, every tool in this module put to work on one set of records.
