# map and filter

Two built-in functions take a function as an argument and apply it across a sequence. `map()` transforms every item. `filter()` keeps some of them.

Both do work that a comprehension also does, and the comparison at the end of this topic is worth making honestly.

## map

`map()` takes a function and a sequence, and applies the function to every item:

```python
numbers = [1, 2, 3, 4]
squares = map(lambda number: number * number, numbers)
print(list(squares))
```

**Output:**

```
[1, 4, 9, 16]
```

The function comes first and the sequence second. The function is passed **without brackets** — `map(square, numbers)`, never `map(square(), numbers)`. Brackets would call it, and what `map()` needs is the function itself, to call later.

A named function works exactly the same way:

```python
def to_fahrenheit(celsius):
    return celsius * 9 / 5 + 32

temperatures = [0, 25, 100]
print(list(map(to_fahrenheit, temperatures)))
```

**Output:**

```
[32.0, 77.0, 212.0]
```

Built-in functions and methods can be passed too, which is where `map()` reads best:

```python
pieces = "10 20 30".split()
print(pieces)
print(list(map(int, pieces)))

names = ["anita", "ravi"]
print(list(map(str.upper, names)))
```

**Output:**

```
['10', '20', '30']
[10, 20, 30]
['ANITA', 'RAVI']
```

No lambda was needed in either case, because a function that does the job already exists.

`map()` also accepts more than one sequence, calling the function with one item from each:

```python
prices = [100, 250, 40]
quantities = [2, 1, 5]
print(list(map(lambda price, quantity: price * quantity, prices, quantities)))
```

**Output:**

```
[200, 250, 200]
```

Like `zip()`, it stops at the end of the shortest sequence.

## filter

`filter()` takes a function and a sequence, and keeps the items for which the function returns `True`:

```python
numbers = [4, -2, 7, 0, -9, 3]
print(list(filter(lambda number: number > 0, numbers)))
```

**Output:**

```
[4, 7, 3]
```

The items are unchanged. `filter()` only decides which ones survive, which is the difference from `map()`: `map()` always returns as many items as it was given, `filter()` returns at most that many.

A named function is often clearer, because its name states the test:

```python
def is_passing(mark):
    return mark >= 50

marks = [78, 45, 91, 32]
print(list(filter(is_passing, marks)))
```

**Output:**

```
[78, 91]
```

Filtering records reads the same way:

```python
students = [
    {"name": "Anita", "age": 21},
    {"name": "Ravi", "age": 17},
    {"name": "Meera", "age": 20},
]
adults = list(filter(lambda student: student["age"] >= 18, students))
print([student["name"] for student in adults])
```

**Output:**

```
['Anita', 'Meera']
```

## Both Return an Iterator

`list()` has appeared around every result so far, and it is not decoration. Neither function returns a list:

```python
numbers = [1, 2, 3]
result = map(lambda n: n * 2, numbers)
print(type(result))
```

**Output:**

```
<class 'map'>
```

Printing one directly shows the object rather than the values:

```python
print(result)
```

**Output:**

```
<map object at 0x000001B2C3D40A90>
```

The hexadecimal number is a memory address and differs every run. What it tells you is that nothing has been computed yet.

A `map` object is an **iterator**: it produces its values one at a time, on demand, and holds none of them. That is useful — a `map()` over a million items uses no more memory than one over ten — and it has a consequence that catches everyone once.

An iterator can only be walked through once:

```python
numbers = [1, 2, 3]
doubled = map(lambda n: n * 2, numbers)

print(list(doubled))
print(list(doubled))
```

**Output:**

```
[2, 4, 6]
[]
```

The first `list()` consumed every value. The second found nothing left. The same happens with a `for` loop: looping over the result twice runs the body only on the first pass.

So convert once and keep the list if the values are needed more than once:

```python
numbers = [1, 2, 3]
doubled = list(map(lambda n: n * 2, numbers))
print(doubled)
print(doubled)
```

**Output:**

```
[2, 4, 6]
[2, 4, 6]
```

If the result is being consumed immediately — looped over once, or handed to `sum()` or `max()` — the conversion is unnecessary:

```python
marks = [78, 45, 91]
print(sum(map(lambda mark: mark + 5, marks)))
```

**Output:**

```
229
```

`filter()` behaves identically, returning a `filter` object with the same one-pass rule.

## The Same Work as a Comprehension

Every `map()` and `filter()` has a comprehension that does the same thing:

| Task | With map/filter | With a comprehension |
| --- | --- | --- |
| Square each item | `list(map(lambda n: n * n, nums))` | `[n * n for n in nums]` |
| Keep the positives | `list(filter(lambda n: n > 0, nums))` | `[n for n in nums if n > 0]` |
| Both | `list(map(lambda n: n * n, filter(lambda n: n > 0, nums)))` | `[n * n for n in nums if n > 0]` |

The first two rows are close. The third is not:

```python
numbers = [4, -2, 7, 0, -9, 3]

print(list(map(lambda n: n * n, filter(lambda n: n > 0, numbers))))
print([n * n for n in numbers if n > 0])
```

**Output:**

```
[16, 49, 9]
[16, 49, 9]
```

The comprehension reads in one pass. The nested version has to be read inside out, and the `filter()` that runs first is written last.

The practical guidance:

**Prefer a comprehension** when a lambda would be needed, and whenever filtering and transforming happen together. It is the form most Python programmers reach for, and it stays readable as the expression grows.

**`map()` reads well** when a function already exists and no lambda is required. `map(int, pieces)` and `map(str.strip, lines)` are shorter and no less clear than the comprehensions that match them.

**Recognise both** regardless of which you write. `map()` and `filter()` are common in existing code, and in other languages they are the standard way to express this work.

## Further Reading

- **Official reference for `map()` and `filter()`** — https://docs.python.org/3/library/functions.html#map
- **map, filter and comprehensions compared** — https://realpython.com/python-map-function/

`map()` transforms and `filter()` selects, both returning single-use iterators. Next, a third function that takes a function as an argument — and the one that is genuinely without a comprehension equivalent.
