# List Comprehensions

One shape of loop appears over and over: create an empty list, walk a sequence, append something to the list each time.

```python
squares = []
for number in range(1, 6):
    squares.append(number * number)
print(squares)
```

**Output:**

```
[1, 4, 9, 16, 25]
```

Four lines, three of which are scaffolding. The single interesting part is `number * number`.

A **list comprehension** writes the same thing as one expression:

```python
squares = [number * number for number in range(1, 6)]
print(squares)
```

**Output:**

```
[1, 4, 9, 16, 25]
```

Same result. The empty list, the `append()` call and the loop body have all gone, because the form implies them.

## The Three Parts

A comprehension has an expression, a `for` clause, and an optional `if` clause, in that order, inside square brackets:

```
[  number * number    for number in range(1, 6)    if number % 2 == 0  ]
    expression            for clause                   condition
```

- The **expression** is what goes into the new list. It usually mentions the loop variable.
- The **for clause** names the loop variable and the sequence, written exactly as in an ordinary `for` line without the colon.
- The **condition** decides which items are used at all. Leave it out and every item is used.

The brackets matter: square brackets produce a list.

A comprehension always builds a **new** list. It never changes the sequence it reads.

## Reading One Aloud

The parts are written expression-first and execute for-clause-first, which is what makes a comprehension awkward to read at the start. Read it in execution order:

```python
squares = [number * number for number in range(1, 6)]
```

*For each number in the range 1 to 6, give number times number.*

With a condition, the condition comes second and the expression last:

```python
even_squares = [number * number for number in range(1, 11) if number % 2 == 0]
print(even_squares)
```

**Output:**

```
[4, 16, 36, 64, 100]
```

*For each number in the range 1 to 11, if the number is even, give number times number.*

Reading right to left and then jumping back to the front is the habit to build. Once it is automatic, the expression sitting first is an advantage: the thing being collected is the first thing you see.

## Filtering

Without an expression that transforms anything, a comprehension filters:

```python
numbers = [4, -2, 7, 0, -9, 3]
positives = [number for number in numbers if number > 0]
print(positives)
```

**Output:**

```
[4, 7, 3]
```

The expression is just `number`, so each item that passes the test goes through unchanged.

The condition can be any expression that yields `True` or `False`, including method calls and membership tests:

```python
words = ["python", "is", "readable", "and", "popular"]
long_words = [word for word in words if len(word) > 3]
print(long_words)

names = ["anita", "Ravi", "meera"]
capitalised = [name for name in names if name[0].isupper()]
print(capitalised)
```

**Output:**

```
['python', 'readable', 'popular']
['Ravi']
```

## Transforming

Without a condition, a comprehension transforms every item:

```python
names = ["anita", "ravi", "meera"]
print([name.title() for name in names])

marks = [78, 91, 64]
print([mark + 5 for mark in marks])

print([str(mark) for mark in marks])
```

**Output:**

```
['Anita', 'Ravi', 'Meera']
[83, 96, 69]
['78', '91', '64']
```

The last one is the tidy way to prepare numbers for `join()`.

Pulling one field out of a list of records is the same thing:

```python
students = [
    {"name": "Anita", "age": 21},
    {"name": "Ravi", "age": 22},
    {"name": "Meera", "age": 20},
]
print([student["name"] for student in students])
print([student["name"] for student in students if student["age"] >= 21])
```

**Output:**

```
['Anita', 'Ravi', 'Meera']
['Anita', 'Ravi']
```

Filtering and transforming together is the common case: select the records that matter, then keep only the field that matters.

## A Conditional Expression Inside

The `if` at the end **removes** items. Sometimes every item should stay and only the value should differ. That needs a different construct in a different place — a conditional expression, which chooses between two values:

```python
marks = [78, 45, 91, 32]
results = ["pass" if mark >= 50 else "fail" for mark in marks]
print(results)
```

**Output:**

```
['pass', 'fail', 'pass', 'fail']
```

Four marks in, four results out. Nothing was filtered.

The form is `value_if_true if condition else value_if_false`, and it is an expression, so it fits where the comprehension's expression goes — before the `for`.

Position is the whole difference between the two uses of `if`:

```python
marks = [78, 45, 91, 32]

print(["pass" if mark >= 50 else "fail" for mark in marks])
print([mark for mark in marks if mark >= 50])
```

**Output:**

```
['pass', 'fail', 'pass', 'fail']
[78, 91]
```

| Where the `if` sits | What it does | `else` |
| --- | --- | --- |
| before the `for` | chooses between two values | required |
| after the `for` | keeps or discards the item | not allowed |

The `else` requirement is the giveaway. An expression must produce a value in every case, so it needs both branches. A filter has nothing to produce when the test fails, so an `else` would be meaningless.

## Nested Comprehensions

A comprehension whose expression is itself a comprehension builds a list of lists:

```python
grid = [[0 for column in range(4)] for row in range(3)]
print(grid)
```

**Output:**

```
[[0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0]]
```

Read it from the outside: *for each row in the range 0 to 3, give a list made of a zero for each column in the range 0 to 4.* The inner comprehension runs once per outer iteration, producing a fresh list each time.

That last point matters. Each row is separately built, so this does not suffer the aliasing problem that `[[0] * 4] * 3` has:

```python
grid = [[0 for column in range(4)] for row in range(3)]
grid[0][0] = 9
print(grid)
```

**Output:**

```
[[9, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0]]
```

One cell changed, as intended.

A comprehension can also carry two `for` clauses, which flattens rather than nests:

```python
matrix = [[1, 2], [3, 4], [5, 6]]
flat = [value for row in matrix for value in row]
print(flat)
```

**Output:**

```
[1, 2, 3, 4, 5, 6]
```

One list came out, not three. The two `for` clauses appear in the same order as the equivalent nested loop:

```python
flat = []
for row in matrix:
    for value in row:
        flat.append(value)
```

Left to right in the comprehension is outer to inner in the loop. This is the one place where reading strictly left to right after the expression is correct, and getting the two clauses the wrong way round is a common error.

## When a Comprehension Is the Wrong Choice

A comprehension is clearer than a loop when it builds one list from one sequence with a simple expression and at most one condition. That covers most cases, and it is worth preferring because the reader can see at a glance that a list is being built and nothing else is happening.

It is the wrong choice in four situations.

**When the body does more than produce a value.** A comprehension holds one expression. Anything needing several statements — a `try`, a running total, a print — needs a loop.

**When the expression is long.** Compare:

```python
results = ["distinction" if m >= 85 else "pass" if m >= 50 else "fail" for m in marks]
```

with the same logic as an `if`/`elif`/`else` in a loop. The comprehension is shorter and the loop is comprehensible. Chained conditional expressions are where comprehensions stop paying.

**When the nesting is more than two levels.** Two `for` clauses are readable. Three are not.

**When the result is not used.** A comprehension written for its side effects builds a list that gets thrown away:

```python
[print(name) for name in names]
```

This prints, and it also constructs a list of `None` values for no reason. Write the loop.

The test to apply: if the comprehension needs a comment to explain it, a loop probably did not.

## Further Reading

- **Official Python guide to list comprehensions** — https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions
- **Comprehensions in depth** — https://realpython.com/list-comprehension-python/

A list comprehension builds a list from a sequence in one expression, filtering with a trailing `if` and choosing values with a conditional expression. Next, the same three parts with different brackets.
