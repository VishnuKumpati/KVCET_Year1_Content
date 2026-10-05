# Indexing and Slicing

A list keeps its elements in order, and each element has a position in that order. A program can use that position to access a particular element.

> **An index is the position of an element in a list.** Indexing is the use of that position to read or change a single element. Slicing is the use of a range of positions to take several elements at once.

## Index Numbering

The positions in a list start at `0`, not at `1`. The first element is at index `0`, the second at index `1`, and so on.

```
marks     =   [ 87 , 72 , 95 ]
index         [  0 ,  1 ,  2 ]
```

A list of three elements therefore has indexes `0`, `1` and `2`. For a non-empty list, the last index is one less than its length.

## Accessing an Element

An element is read by writing its index inside square brackets after the list name.

**Syntax**

```
list_name[index]
```

**Example**

```python
marks = [87, 72, 95]
print(marks[0])
print(marks[1])
print(marks[2])
```

**Output:**

```
87
72
95
```

Each index gives access to one element. `marks[0]` gave `87`, the first element, because numbering begins at zero.

## Index Out of Range

An index that does not exist in the list causes an error.

**Example**

```python
marks = [87, 72, 95]
print(marks[3])
```

**Output:**

```
IndexError: list index out of range
```

The list has three elements, so its highest index is `2`. Index `3` would be the fourth element, and there is none.

When a program tries to access an index that does not exist, Python raises an `IndexError`.

## Negative Indexing

An index can also be negative. A negative index counts from the end of the list, with `-1` for the last element, so the last element can be reached without knowing the length.

```
marks     =   [ 87 , 72 , 95 ]
index         [  0 ,  1 ,  2 ]
negative      [ -3 , -2 , -1 ]
```

**Example**

```python
marks = [87, 72, 95]
print(marks[-1])
print(marks[-2])
print(marks[-3])
```

**Output:**

```
95
72
87
```

`marks[-1]` gives the last element without needing to know the length of the list.

## Changing an Element

An index on the left of an `=` replaces the element at that position.

**Syntax**

```
list_name[index] = new_value
```

**Example**

```python
marks = [87, 72, 95]
marks[1] = 80
print(marks)
```

**Output:**

```
[87, 80, 95]
```

The element at index `1` changed from `72` to `80`. The list still has three elements, and the other two are untouched.

## Slicing a List

A slice takes a range of elements and returns them as a new list. The range is written as a start index and a stop index, separated by a colon.

**Syntax**

```
list_name[start:stop]
```

**Example**

```python
marks = [87, 72, 95, 60, 48]
print(marks[1:4])
```

**Output:**

```
[72, 95, 60]
```

The slice began at index `1` and stopped before index `4`. It returned the elements at indexes `1`, `2` and `3`.

The stop index is not included, just like the stop value in `range()`. `marks[1:4]` therefore gives three elements, not four.

## Omitting the Start or Stop

The start or stop can be left out. Python then uses the beginning or the end of the list.

**Example**

```python
marks = [87, 72, 95, 60, 48]
print(marks[:3])
print(marks[2:])
print(marks[:])
```

**Output:**

```
[87, 72, 95]
[95, 60, 48]
[87, 72, 95, 60, 48]
```

`marks[:3]` started at the beginning and stopped before index `3`. `marks[2:]` started at index `2` and ran to the end. `marks[:]` left out both and returned every element.

## Slicing with a Step

A third value sets the step, which is how far the slice moves each time.

**Syntax**

```
list_name[start:stop:step]
```

**Example**

```python
marks = [87, 72, 95, 60, 48]
print(marks[::2])
```

**Output:**

```
[87, 95, 48]
```

The slice moved through the list in steps of `2`, so it selected every other element.

A negative step moves through the list backwards. `marks[::-1]` therefore returns the elements in reverse order.

**Example**

```python
marks = [87, 72, 95]
print(marks[::-1])
```

**Output:**

```
[95, 72, 87]
```

## Slices Beyond the End of the List

An index out of range raises `IndexError`, but a slice out of range does not.

**Example**

```python
marks = [87, 72, 95]
print(marks[1:10])
print(marks[5:8])
```

**Output:**

```
[72, 95]
[]
```

The first slice went up to index `10`, but only the elements that existed were returned. The second asked for elements that do not exist at all and received an empty list.

A slice simply returns the elements that exist in the requested range. It does not raise an error when the range goes past the end.

## Indexing vs Slicing

The two look similar but produce different results.

**Example**

```python
marks = [87, 72, 95]
print(type(marks[0]))
print(type(marks[0:1]))
```

**Output:**

```
<class 'int'>
<class 'list'>
```

Indexing gives one element, so the type here is `int`. Slicing gives a list, even when that list contains only one element.

A slice also leaves the original list unchanged.

**Example**

```python
marks = [87, 72, 95, 60]
part = marks[1:3]
print(part)
print(marks)
```

**Output:**

```
[72, 95]
[87, 72, 95, 60]
```

`part` is a new list containing those two elements. `marks` still holds all four.



## Further Reading

- 📎 **Indexing and slicing with worked examples** — https://www.programiz.com/python-programming/list
- 📎 **Official Python guide to lists** — https://docs.python.org/3/tutorial/datastructures.html

You can now access one element or a range of elements, and replace a single element with a new value. But a list can also grow and shrink after it is created.

Next, you will learn the methods that add and remove elements, and what it means for a list to be changeable.
