# List Methods and Mutation

A list created with three elements does not have to stay that way. Elements can be added, removed and replaced at any point while the program runs.

> **Mutability means a value can be changed after it is created.** A list is mutable, so adding or removing an element changes the existing list rather than producing a new one.

Integers, floats, strings and Booleans cannot be changed after they are created. Operations that appear to change them create a new value instead.

So, what does changing a list actually look like? The most common change is adding an element.

## Adding an Element to the End

The `append()` method adds one element to the end of a list.

**Syntax**

```
list_name.append(element)
```

**Example**

```python
marks = [87, 72]
marks.append(95)
print(marks)
```

**Output:**

```
[87, 72, 95]
```

## Adding an Element at a Position

The `insert()` method adds an element at a chosen index. Every element from that position onwards moves one place to the right.

**Syntax**

```
list_name.insert(index, element)
```

**Example**

```python
marks = [87, 72, 95]
marks.insert(1, 80)
print(marks)
```

**Output:**

```
[87, 80, 72, 95]
```

`80` went in at index `1`. The elements that were at `1` and `2` are now at `2` and `3`, so the list grew by one.

## Adding Several Elements

The `extend()` method adds every element of another list to the end.

**Syntax**

```
list_name.extend(other_list)
```

**Example**

```python
marks = [87, 72]
marks.extend([95, 60])
print(marks)
print(len(marks))
```

**Output:**

```
[87, 72, 95, 60]
4
```

The length is `4`, because `extend()` added two separate elements.

## append() and extend() Compared

Both add to the end of a list, and they treat the value differently.

**Example**

```python
first = [87, 72]
first.append([95, 60])
print(first)

second = [87, 72]
second.extend([95, 60])
print(second)
```

**Output:**

```
[87, 72, [95, 60]]
[87, 72, 95, 60]
```

`append()` added the whole list as one element, which is why the output shows square brackets inside the outer ones. `extend()` added the elements of that list one by one.

## Removing an Element by Value

The `remove()` method deletes the first element that matches the value given.

**Syntax**

```
list_name.remove(value)
```

**Example**

```python
marks = [87, 72, 95]
marks.remove(72)
print(marks)
```

**Output:**

```
[87, 95]
```

When the value appears more than once, only the first occurrence is removed.

**Example**

```python
marks = [87, 72, 87]
marks.remove(87)
print(marks)
```

**Output:**

```
[72, 87]
```

The `87` at index `0` was removed. The one at the end is still there.

If the value is not in the list, Python raises a `ValueError`.

**Example**

```python
marks = [87, 72, 95]
marks.remove(60)
```

**Output:**

```
ValueError: list.remove(x): x not in list
```

## Removing an Element by Position

The `pop()` method removes the element at a given index and returns it.

**Syntax**

```
list_name.pop(index)
```

**Example**

```python
marks = [87, 72, 95]
removed = marks.pop(1)
print(removed)
print(marks)
```

**Output:**

```
72
[87, 95]
```

`pop()` differs from `remove()` in two ways. It takes a position rather than a value, and it gives back the element it removed, so the value can still be used.

Called with no index at all, `pop()` removes the last element.

**Example**

```python
marks = [87, 72, 95]
last = marks.pop()
print(last)
print(marks)
```

**Output:**

```
95
[87, 72]
```

## Deleting an Element with del

The `del` statement removes the element at a given index. Unlike `pop()`, it returns nothing.

**Syntax**

```
del list_name[index]
```

**Example**

```python
marks = [87, 72, 95]
del marks[0]
print(marks)
```

**Output:**

```
[72, 95]
```

`del` is a statement, not a method, so it is written before the list rather than after a dot.

## Emptying a List

The `clear()` method removes every element, leaving an empty list.

**Syntax**

```
list_name.clear()
```

**Example**

```python
marks = [87, 72, 95]
marks.clear()
print(marks)
print(len(marks))
```

**Output:**

```
[]
0
```

The variable still holds a list. That list now has no elements.

## Finding and Counting Elements

Two methods report on the contents of a list without changing it. `index()` gives the position of the first matching element, and `count()` gives the number of matching elements.

**Syntax**

```
list_name.index(value)
list_name.count(value)
```

**Example**

```python
marks = [87, 72, 87]
print(marks.index(87))
print(marks.count(87))
```

**Output:**

```
0
2
```

`index()` returned `0`, the position of the first `87`. `count()` returned `2`, because the value appears twice.

If the value is not in the list, `index()` raises a `ValueError`.

## List Assignment and Shared Lists

Mutability has a consequence worth understanding. Assigning a list to a second variable does not make a second list. Both names refer to the same list, so a change made through one name is visible through the other.

**Example**

```python
first = [87, 72]
second = first
second.append(95)
print(first)
print(second)
```

**Output:**

```
[87, 72, 95]
[87, 72, 95]
```

Only `second` was changed, and `first` shows the change as well. There is one list here, with two names pointing at it.

## Copying a List

The `copy()` method makes a separate list with the same elements. Changing one then leaves the other alone.

**Syntax**

```
list_name.copy()
```

**Example**

```python
first = [87, 72]
second = first.copy()
second.append(95)
print(first)
print(second)
```

**Output:**

```
[87, 72]
[87, 72, 95]
```

There are two lists now. `second` grew and `first` did not.

## Changing a List Inside a Function

When a list is passed to a function, the function receives access to the same list. Any change it makes is visible to the caller.

**Example**

```python
def add_bonus(scores):
    scores.append(100)

marks = [87, 72]
add_bonus(marks)
print(marks)
```

**Output:**

```
[87, 72, 100]
```

The function returned nothing, and `marks` changed anyway. This is the same behaviour as two names for one list, with the parameter acting as the second name.

If a function should leave the caller's list unchanged, it can work on a copy.

## Summary

The key points about list methods and mutation:

- A list is mutable, which means it can be changed after it is created.
- `append()` adds one element to the end of a list.
- `insert()` adds an element at a chosen index and moves the later elements along.
- `extend()` adds every element of another list to the end.
- `append()` adds a list as one element, while `extend()` adds its elements separately.
- `remove()` deletes the first element matching a value, and raises a `ValueError` if the value is absent.
- `pop()` removes the element at an index and returns it, or removes the last element if no index is given.
- `del` removes the element at an index and returns nothing.
- `clear()` removes every element, leaving an empty list.
- `index()` gives the position of the first matching element, and `count()` gives how many match.
- Assigning a list to another variable creates a second name for the same list, not a second list.
- `copy()` creates a separate list with the same elements.
- A list passed to a function can be changed by that function.

## Further Reading

- 📎 **List methods with worked examples** — https://www.programiz.com/python-programming/methods/list
- 📎 **Official Python guide to lists** — https://docs.python.org/3/tutorial/datastructures.html

A list can now be built, read, changed and emptied. But so far, the examples have added, removed or changed elements one operation at a time.

Next, you will learn how to work through every element of a list in turn.
