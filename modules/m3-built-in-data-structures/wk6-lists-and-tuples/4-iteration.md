# Iteration

A list contains many values. Iteration lets a program process those values one by one, without writing the same code repeatedly.

> **Iteration is the process of going through a collection of values one at a time.** In this lesson, you will use a `for` loop to iterate over lists. The loop takes each element in turn, from the first to the last.

## Looping Through a List

A `for` loop takes a list after the keyword `in`. On each iteration, the loop variable receives the next element from the list.

**Syntax**

```
for variable in list_name:
    statements
```

**Example**

```python
marks = [87, 72, 95]
for mark in marks:
    print(mark)
```

**Output:**

```
87
72
95
```

The loop ran three times, once for each element. `mark` held `87` on the first iteration, `72` on the second and `95` on the third.

The loop stops on its own at the end of the list. Nothing counts the elements, and the length never appears in the code. On an empty list the loop runs zero times and raises no error.

## Collecting a Result from a List

A variable created before the loop keeps its value across every iteration, which is how a total is built up.

**Example**

```python
marks = [87, 72, 95]
total = 0
for mark in marks:
    total = total + mark
print("Total:", total)
```

**Output:**

```
Total: 254
```

`total` started at `0` and grew on every iteration. It is created before the loop so its value can be carried from one iteration to the next.

## Using a Condition Inside a Loop

An `if` statement inside the loop body decides what happens for each element.

**Example**

```python
marks = [87, 30, 95, 20]
passed = 0
for mark in marks:
    if mark >= 35:
        passed = passed + 1
print("Passed:", passed)
```

**Output:**

```
Passed: 2
```

The loop visited all four elements. `passed` was increased only on the two iterations where the condition was true.

## Looping with the Index

A `for` loop over a list gives you each element directly. If you also need its position, you can loop over `range(len(list_name))` and use each index to access the corresponding element.

**Example**

```python
marks = [87, 72, 95]
for position in range(len(marks)):
    print(position, marks[position])
```

**Output:**

```
0 87
1 72
2 95
```

`range(len(marks))` produced `0`, `1` and `2`, and each was used as an index.

## Changing Elements Inside a Loop

When you need to change elements while iterating, you can use their indexes. Assigning to the loop variable does not change the list.

**Example**

```python
marks = [87, 72, 95]
for mark in marks:
    mark = mark + 5
print(marks)
```

**Output:**

```
[87, 72, 95]
```

Nothing changed. `mark` receives the value of each element. The statement `mark = mark + 5` assigns a new value to `mark`; it does not update the corresponding element in the list.

Assigning through the index changes the list itself.

**Example**

```python
marks = [87, 72, 95]
for position in range(len(marks)):
    marks[position] = marks[position] + 5
print(marks)
```

**Output:**

```
[92, 77, 100]
```

## Looping with enumerate()

`range(len(list_name))` gives the positions, but you then need to use each position to access the corresponding element. The `enumerate()` function gives both at once.

**Syntax**

```
for position, variable in enumerate(list_name):
    statements
```

**Example**

```python
names = ["Asha", "Ravi", "Meera"]
for position, name in enumerate(names):
    print(position, name)
```

**Output:**

```
0 Asha
1 Ravi
2 Meera
```

`enumerate()` gives two values on each iteration: the position and the element. `position` and `name` receive those two values.

## Looping Through Two Lists with zip()

Sometimes two lists hold related values, such as students' names in one list and their marks in another. The `zip()` function goes through both lists together, giving one element from each list on every iteration.

**Syntax**

```
for variable1, variable2 in zip(list1, list2):
    statements
```

**Example**

```python
names = ["Asha", "Ravi", "Meera"]
marks = [87, 72, 95]
for name, mark in zip(names, marks):
    print(name, mark)
```

**Output:**

```
Asha 87
Ravi 72
Meera 95
```

On the first iteration, `name` holds `"Asha"` and `mark` holds `87`, the first element of each list. On the second, they hold the second elements, and so on.

When the lists have different lengths, `zip()` stops at the end of the **shorter** list.

**Example**

```python
names = ["Asha", "Ravi", "Meera"]
marks = [87, 72]
for name, mark in zip(names, marks):
    print(name, mark)
```

**Output:**

```
Asha 87
Ravi 72
```

`marks` has only two elements, so the loop ran twice. `Meera` was skipped because there was no matching mark.

## Removing Elements During a Loop

A list should not be shortened while a loop is going through it. Removing an element moves the later elements along, and the loop skips the one that takes its place.

**Example**

```python
marks = [87, 30, 20, 95]
for mark in marks:
    if mark < 35:
        marks.remove(mark)
print(marks)
```

**Output:**

```
[87, 20, 95]
```

The `20` should have been removed and was not. When `30` was removed, `20` shifted into its position. The loop then moved to the next index, skipping `20`.

The safe approach is to build a new list of the elements to keep.

**Example**

```python
marks = [87, 30, 20, 95]
kept = []
for mark in marks:
    if mark >= 35:
        kept.append(mark)
print(kept)
```

**Output:**

```
[87, 95]
```

The original list is only read, never changed, so nothing is skipped.


## Further Reading

- 📎 **Looping through lists with worked examples** — https://www.programiz.com/python-programming/list
- 📎 **Official Python guide to lists** — https://docs.python.org/3/tutorial/datastructures.html

A loop can now visit every element of a list, whatever its length. But sometimes you need the elements in a different order.

Next, you will learn how to put a list in order.