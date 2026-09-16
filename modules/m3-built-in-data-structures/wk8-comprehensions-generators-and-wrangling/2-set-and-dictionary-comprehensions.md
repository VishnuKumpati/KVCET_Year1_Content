# Set and Dictionary Comprehensions

The comprehension form is not tied to lists. The same expression, `for` clause and optional condition build a set or a dictionary. What changes is the brackets, and for a dictionary, the expression.

| Brackets | Expression | Produces |
| --- | --- | --- |
| `[ ]` | one value | a list |
| `{ }` | one value | a set |
| `{ }` | `key: value` | a dictionary |

Curly brackets do two jobs, and the colon is what distinguishes them — exactly as it does when writing a set or dictionary out by hand.

## Set Comprehensions

Square brackets swapped for curly ones, and the result is a set: unordered, with duplicates dropped.

```python
words = ["the", "cat", "sat", "on", "the", "mat"]

print([len(word) for word in words])
print({len(word) for word in words})
```

**Output:**

```
[3, 3, 3, 2, 3, 3]
[2, 3]
```

The list reports every length. The set reports the distinct lengths, which is usually the question worth asking.

Filtering and transforming work as before:

```python
numbers = [1, -2, 3, -2, 5, 3]
print({abs(number) for number in numbers})
print({number for number in numbers if number > 0})
```

**Output:**

```
{1, 2, 3, 5}
{1, 3, 5}
```

Since a set has no order, sort when the output order matters:

```python
names = ["anita", "RAVI", "Anita", "ravi"]
print(sorted({name.lower() for name in names}))
```

**Output:**

```
['anita', 'ravi']
```

Four names, two distinct people. A set comprehension normalised and deduplicated in one expression.

This is the usual reason to reach for one: a list comprehension followed by `set()` does the same job, and the set comprehension says up front that duplicates are not wanted.

## Dictionary Comprehensions

A dictionary comprehension has a colon in its expression. The part before the colon becomes the key, the part after becomes the value:

```python
squares = {number: number * number for number in range(1, 6)}
print(squares)
```

**Output:**

```
{1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
```

Written the long way, that is an empty dictionary and an assignment in a loop:

```python
squares = {}
for number in range(1, 6):
    squares[number] = number * number
```

Either half of the expression can be computed. Here the keys are transformed and the values are left alone:

```python
ages = {"anita": 21, "ravi": 22}
print({name.title(): age for name, age in ages.items()})
```

**Output:**

```
{'Anita': 21, 'Ravi': 22}
```

And here the values are transformed:

```python
prices = {"pen": 10, "book": 250}
print({item: price * 1.18 for item, price in prices.items()})
```

**Output:**

```
{'pen': 11.799999999999999, 'book': 295.0}
```

Rounding is worth adding when money is involved:

```python
prices = {"pen": 10, "book": 250}
print({item: round(price * 1.18, 2) for item, price in prices.items()})
```

**Output:**

```
{'pen': 11.8, 'book': 295.0}
```

Filtering a dictionary uses the trailing `if`, exactly as a list comprehension does:

```python
ages = {"Anita": 21, "Ravi": 22, "Meera": 20}
print({name: age for name, age in ages.items() if age >= 21})
```

**Output:**

```
{'Anita': 21, 'Ravi': 22}
```

`items()` and the two loop variables do most of the work in all of these. A dictionary comprehension over `record.items()` is the standard way to produce a changed copy of a dictionary without altering the original.

## Building a Dictionary from Two Lists

Two lists that line up item for item become a dictionary through `zip()`:

```python
names = ["Anita", "Ravi", "Meera"]
ages = [21, 22, 20]

ages_by_name = {name: age for name, age in zip(names, ages)}
print(ages_by_name)
```

**Output:**

```
{'Anita': 21, 'Ravi': 22, 'Meera': 20}
```

`zip()` produces the pairs, the two names unpack each pair, and the expression puts them either side of the colon.

For this exact case — pairs straight in, nothing transformed — `dict()` does it without the comprehension:

```python
print(dict(zip(names, ages)))
```

**Output:**

```
{'Anita': 21, 'Ravi': 22, 'Meera': 20}
```

Use `dict(zip(...))` when nothing needs changing, and the comprehension when a key or value has to be computed or a condition applied:

```python
names = ["anita", "ravi", "meera"]
ages = [21, 22, 20]
print({name.title(): age for name, age in zip(names, ages) if age >= 21})
```

**Output:**

```
{'Anita': 21, 'Ravi': 22}
```

This is also the cure for the pair of parallel lists that a dictionary replaces. Data often arrives as separate columns, and one line turns it into records keyed properly.

## Inverting a Dictionary

Swapping the two sides of the colon turns keys into values and values into keys:

```python
ages = {"Anita": 21, "Ravi": 22, "Meera": 20}
by_age = {age: name for name, age in ages.items()}
print(by_age)
print(by_age[22])
```

**Output:**

```
{21: 'Anita', 22: 'Ravi', 20: 'Meera'}
Ravi
```

Two conditions have to hold for this to be sensible.

The values must be usable as keys, so they must be immutable. Inverting a dictionary whose values are lists fails with `TypeError: unhashable type: 'list'`.

And the values must be unique. A dictionary holds one value per key, so duplicates collapse and the last one wins:

```python
courses = {"Anita": "Maths", "Ravi": "Physics", "Meera": "Maths"}
print({course: name for name, course in courses.items()})
```

**Output:**

```
{'Maths': 'Meera', 'Physics': 'Ravi'}
```

Anita has been lost. Inverting is only safe when the values genuinely identify one thing each — an id, a username, a code.

When values repeat, grouping is what is actually wanted, and that needs a loop with `setdefault()` rather than a comprehension:

```python
courses = {"Anita": "Maths", "Ravi": "Physics", "Meera": "Maths"}

by_course = {}
for name, course in courses.items():
    by_course.setdefault(course, []).append(name)

print(by_course)
```

**Output:**

```
{'Maths': ['Anita', 'Meera'], 'Physics': ['Ravi']}
```

Nobody is lost, because each key now holds a list. A comprehension cannot do this: it produces one value per iteration and has nowhere to accumulate.

## Empty Braces Are Still a Dictionary

`{}` means an empty dictionary, and `set()` is the only way to write an empty set. That rule is unchanged, and it has one consequence worth stating.

A set comprehension is written with braces and no colon, so it is a set even when it produces nothing:

```python
numbers = [1, 2, 3]
result = {number for number in numbers if number > 100}
print(result)
print(type(result))
```

**Output:**

```
set()
<class 'set'>
```

The type is decided by the form of the expression, not by what comes out of it. A comprehension with a colon is a dictionary comprehension however few pairs it yields:

```python
empty = {k: v for k, v in [] }
print(empty)
print(type(empty))
```

**Output:**

```
{}
<class 'dict'>
```

So a set comprehension is the one brace-delimited form that reliably produces a set. When a set has to be built from literal values with no comprehension involved, and it might be empty, `set()` is still the way to start it.

## Further Reading

- **Official Python guide to comprehensions** — https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions
- **Dictionary comprehensions explained** — https://realpython.com/python-dict-comprehension/

The comprehension form builds lists, sets and dictionaries from the same three parts. Next, a way of writing a function small enough to fit inside an expression.
