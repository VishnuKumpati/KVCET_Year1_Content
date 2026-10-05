# Sorting

A list keeps its elements in the order they were put in. Sorting arranges the elements of a list in a defined order, such as smallest to largest for numbers or alphabetical order for strings.

> **Sorting is the process of arranging the elements of a list in a particular order.** Python provides two ways to do it: the `sort()` method, which rearranges the existing list, and the `sorted()` function, which produces a new list and leaves the original alone.

## Sorting a List in Place

The `sort()` method rearranges the elements of the list it is called on.

**Syntax**

```
list_name.sort()
```

**Example**

```python
marks = [87, 72, 95, 60]
marks.sort()
print(marks)
```

**Output:**

```
[60, 72, 87, 95]
```

The list now holds the same four elements in a different order. No new list was created.

By default, Python sorts numbers in ascending order, from smallest to largest.

## The Return Value of sort()

`sort()` returns `None`, so assigning its result to a variable does not give you the sorted list.

**Example**

```python
marks = [87, 72, 95]
result = marks.sort()
print(result)
print(marks)
```

**Output:**

```
None
[72, 87, 95]
```

`result` holds `None`, while the list `marks` holds the sorted elements. Writing `marks = marks.sort()` would replace the list with `None`, which is a common mistake.

## Sorting into a New List

The `sorted()` function returns a sorted list and leaves the original unchanged.

**Syntax**

```
sorted(list_name)
```

**Example**

```python
marks = [87, 72, 95, 60]
ordered = sorted(marks)
print(ordered)
print(marks)
```

**Output:**

```
[60, 72, 87, 95]
[87, 72, 95, 60]
```

Two lists exist now. The list `ordered` is sorted, while the list `marks` keeps its original order.

Use `sort()` when the original order is no longer needed. Use `sorted()` when it is.

## Sorting in Descending Order

The `sort()` method and the `sorted()` function can both take `reverse=True` to arrange the elements from largest to smallest.

**Syntax**

```
list_name.sort(reverse=True)
sorted(list_name, reverse=True)
```

**Example**

```python
marks = [87, 72, 95, 60]
marks.sort(reverse=True)
print(marks)
```

**Output:**

```
[95, 87, 72, 60]
```

`reverse=True` changes the order to descending, from largest to smallest.

## Sorting Strings

Sorting is not limited to numbers. Strings are sorted alphabetically.

**Example**

```python
names = ["Ravi", "Asha", "Meera"]
names.sort()
print(names)
```

**Output:**

```
['Asha', 'Meera', 'Ravi']
```

Capital letters come before lower-case letters, so a list with both does not sort the way a dictionary would.

**Example**

```python
names = ["ravi", "Asha", "meera"]
names.sort()
print(names)
```

**Output:**

```
['Asha', 'meera', 'ravi']
```

`Asha` came first because it begins with a capital letter, not because of the letter `A`.

## Reversing a List Without Sorting

The `reverse()` method reverses the current order of the elements. It does not sort them.

**Syntax**

```
list_name.reverse()
```

**Example**

```python
marks = [87, 72, 95]
marks.reverse()
print(marks)
```

**Output:**

```
[95, 72, 87]
```

The elements are in the opposite order to how they were stored. Sorting would have given `[95, 87, 72]`.

## Sorting Lists with Mixed Types

Sorting compares elements with each other, so every element must be comparable with the rest.

**Example**

```python
values = [87, "Asha", 95]
values.sort()
```

**Output:**

```
TypeError: '<' not supported between instances of 'str' and 'int'
```

Python cannot compare a string with an integer, so sorting raises a `TypeError`.

## Summary

The key points about sorting:

- Sorting arranges the elements of a list in a particular order.
- By default, sorting is in ascending order, from smallest to largest.
- `sort()` rearranges the existing list and returns `None`.
- Assigning the result of `sort()` to a variable does not give you the sorted list.
- `sorted()` returns a new sorted list and leaves the original unchanged.
- `reverse=True` makes both `sort()` and `sorted()` arrange the elements from largest to smallest.
- Strings sort alphabetically, with capital letters before lower-case letters.
- `reverse()` reverses the current order of the elements without sorting them.
- Sorting a list whose elements cannot be compared raises a `TypeError`.

## Further Reading

- 📎 **Sorting lists with worked examples** — https://www.programiz.com/python-programming/methods/list/sort
- 📎 **Official Python guide to sorting** — https://docs.python.org/3/howto/sorting.html

Every list so far has held single values. But a list can also hold other lists, which is how a program stores a table of data.

Next, you will learn how to work with a list of lists.
