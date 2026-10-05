# Tuples

A list can be changed at any time, and that is what makes it useful for data that grows and shrinks. Not all data is like that. A date of birth, a pair of coordinates and a set of fixed settings are all meant to stay the same for as long as the program runs.

For values of that kind, Python provides a second kind of sequence.

> **A tuple is an ordered collection of values that cannot be changed after it is created.** Like a list, it holds any number of values of any type, and each value keeps its position. Unlike a list, no value can be added, removed or replaced.

> **Immutability means a value cannot be changed after it is created.** A tuple is immutable, so its elements cannot be added, removed or replaced after the tuple is created.

## Creating a Tuple

A tuple is written with round brackets, and its elements are separated by commas.

**Syntax**

```
variable = (element1, element2, element3)
```

**Example**

```python
marks = (87, 72, 95)
print(marks)
print(len(marks))
print(type(marks))
```

**Output:**

```
(87, 72, 95)
3
<class 'tuple'>
```

`tuple` is a built-in data type, like `list`. The output shows round brackets instead of square ones.

## Accessing Elements

Indexing and slicing work exactly as they do on a list.

**Example**

```python
marks = (87, 72, 95)
print(marks[0])
print(marks[-1])
print(marks[0:2])
```

**Output:**

```
87
95
(87, 72)
```

Positions start at `0`, negative indexes count from the end, and a slice returns a new tuple rather than a list.

## Tuples Cannot Be Changed

Assigning to an index raises an error.

**Example**

```python
marks = (87, 72, 95)
marks[0] = 90
```

**Output:**

```
TypeError: 'tuple' object does not support item assignment
```

A tuple also has no methods for adding or removing elements.

**Example**

```python
marks = (87, 72, 95)
marks.append(60)
```

**Output:**

```
AttributeError: 'tuple' object has no attribute 'append'
```

`append()`, `insert()`, `remove()`, `pop()`, `sort()` and `clear()` all belong to lists. A tuple has none of them, because every one of them would change the tuple.

## Creating a Tuple with One Element

A single value in round brackets does not make a tuple. A trailing comma is required.

**Example**

```python
one = (87,)
not_tuple = (87)
print(type(one))
print(type(not_tuple))
```

**Output:**

```
<class 'tuple'>
<class 'int'>
```

In `(87)`, the brackets are the same brackets used in arithmetic, where they group part of a calculation. They do not create a tuple, so `not_tuple` simply holds the integer `87`. The comma in `(87,)` is what makes the difference.

## Creating a Tuple Without Brackets

The commas are what create a tuple. The brackets only make it clearer to read.

**Example**

```python
marks = 87, 72, 95
print(marks)
print(type(marks))
```

**Output:**

```
(87, 72, 95)
<class 'tuple'>
```

Python shows the brackets when printing, even though they were not written.

## Creating an Empty Tuple

An empty tuple is a pair of round brackets with nothing inside.

**Example**

```python
empty = ()
print(empty)
print(len(empty))
```

**Output:**

```
()
0
```

An empty tuple is permanently empty, because nothing can be added to it.

## Unpacking a Tuple

The elements of a tuple can be assigned to separate variables in one statement. This is called unpacking.

**Syntax**

```
variable1, variable2, variable3 = tuple_name
```

**Example**

```python
student = ("Asha", 87, "Class 7")
name, mark, section = student
print(name)
print(mark)
print(section)
```

**Output:**

```
Asha
87
Class 7
```

The number of variables must match the number of elements.

**Example**

```python
student = ("Asha", 87, "Class 7")
name, mark = student
```

**Output:**

```
ValueError: too many values to unpack (expected 2)
```

Unpacking makes swapping two variables a single line, because the right side forms a tuple before anything is assigned.

**Example**

```python
first = 10
second = 20
first, second = second, first
print(first, second)
```

**Output:**

```
20 10
```

## Returning Several Values from a Function

A `return` statement with several values separated by commas returns a tuple.

**Example**

```python
def get_stats(numbers):
    return min(numbers), max(numbers)

result = get_stats([87, 72, 95])
print(result)
print(type(result))

lowest, highest = get_stats([87, 72, 95])
print(lowest, highest)
```

**Output:**

```
(72, 95)
<class 'tuple'>
72 95
```

The function returned one value, a tuple of two elements. Unpacking it at the call gives two separate variables, which is how a function appears to return more than one result.

## Tuple Methods

A tuple has only two built-in methods for working with its elements: `count()` and `index()`. The other common list methods would change the collection, so tuples do not provide them.

**Example**

```python
marks = (87, 72, 87)
print(marks.count(87))
print(marks.index(72))
```

**Output:**

```
2
1
```

## Looping Through a Tuple

A `for` loop works on a tuple exactly as it does on a list.

**Example**

```python
marks = (87, 72, 95)
for mark in marks:
    print(mark)
```

**Output:**

```
87
72
95
```

## Converting Between Lists and Tuples

`list()` makes a list from a tuple, and `tuple()` makes a tuple from a list. Neither changes the original.

**Example**

```python
marks = (87, 72, 95)
as_list = list(marks)
as_list.append(60)
print(as_list)
print(tuple(as_list))
```

**Output:**

```
[87, 72, 95, 60]
(87, 72, 95, 60)
```

The original tuple is not changed. A new tuple is created from the modified list.

## Choosing Between a List and a Tuple

Both hold an ordered collection of values, and the choice comes down to whether the values should change.

Use a list when elements will be added, removed or replaced, such as a list of marks being collected.

Use a tuple when the values are fixed, such as a pair of coordinates or a set of configuration values. The immutability then protects the data, because any line that tries to change it raises an error instead of changing it quietly.

## Summary

The key points about tuples:

- A tuple is an ordered collection of values that cannot be changed after it is created.
- A tuple is written with round brackets, and the elements are separated by commas.
- `tuple` is a built-in data type, like `list`.
- Indexing and slicing work exactly as they do on a list.
- Assigning to an index raises a `TypeError`.
- A tuple has no methods that would change it, so `append()` raises an `AttributeError`.
- A tuple with one element needs a trailing comma.
- The commas create a tuple, and the brackets are optional.
- Unpacking assigns the elements of a tuple to separate variables in one statement.
- A function that returns several values separated by commas returns a tuple.
- A tuple has only two methods, `count()` and `index()`.
- `list()` and `tuple()` convert between the two types.

## Further Reading

- 📎 **Tuples with worked examples** — https://www.programiz.com/python-programming/tuple
- 📎 **Official Python guide to tuples** — https://docs.python.org/3/tutorial/datastructures.html

That completes lists and tuples. You can now store many values in one variable, reach any of them by position, change a list, keep a tuple fixed, and work through a collection with a loop.

Next, you will learn about strings.
