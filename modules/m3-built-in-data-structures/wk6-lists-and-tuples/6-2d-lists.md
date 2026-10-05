# 2D Lists

A list can hold individual values, but data often comes as a table instead, with rows and columns, such as the marks of several students across several subjects.

> **A 2D list is a list of lists arranged to represent data in rows and columns.** The outer list holds the rows, and each inner list holds the values of one row. Python has no separate table type, so a list of lists is how a table is stored.

## Creating a 2D List

A 2D list is written as a list of lists, with each row in its own square brackets.

**Example**

```python
marks = [[87, 72, 90], [95, 60, 70], [48, 55, 65]]
print(marks)
print(len(marks))
```

**Output:**

```
[[87, 72, 90], [95, 60, 70], [48, 55, 65]]
3
```

The length is `3`, not `9`. The outer list has three elements, and each of those elements is a row of three marks.

Writing each row on its own line makes the table shape visible.

**Example**

```python
marks = [
    [87, 72, 90],
    [95, 60, 70],
    [48, 55, 65]
]
print(marks[0])
```

**Output:**

```
[87, 72, 90]
```

Each row has the same number of values, so the values line up into columns.

## Accessing a Row

A single index gives one element of the outer list, which is a whole row.

**Example**

```python
marks = [[87, 72, 90], [95, 60, 70], [48, 55, 65]]
print(marks[0])
print(type(marks[0]))
```

**Output:**

```
[87, 72, 90]
<class 'list'>
```

`marks[0]` gave the first row, and `type()` confirms that the row is itself a list.

## Accessing a Single Value

Two indexes are needed to reach one value. The first selects the row, and the second selects the column within that row.

**Syntax**

```
list_name[row][column]
```

**Example**

```python
marks = [[87, 72, 90], [95, 60, 70], [48, 55, 65]]
print(marks[0][1])
print(marks[2][0])
```

**Output:**

```
72
48
```

`marks[0][1]` selected row `0`, then column `1` of that row. `marks[2][0]` selected row `2`, then its first column.

Both indexes start at `0`, exactly as in any other list.

## Changing a Value

Two indexes on the left of an `=` replace a single value.

**Example**

```python
marks = [[87, 72, 90], [95, 60, 70]]
marks[0][1] = 80
print(marks)
```

**Output:**

```
[[87, 80, 90], [95, 60, 70]]
```

The value at row `0`, column `1` changed from `72` to `80`. Everything else is untouched.

## Counting Rows and Columns

`len()` on the 2D list gives the number of rows. `len()` on one row gives the number of columns.

**Example**

```python
marks = [[87, 72, 90], [95, 60, 70]]
print(len(marks))
print(len(marks[0]))
```

**Output:**

```
2
3
```

The table has two rows and three columns.

> **Note:** A 2D list is a list of lists arranged like a table, where each row has the same number of values. A list of lists can also have rows of different lengths, which is often called a jagged list.

## Looping Through the Rows

A `for` loop over a 2D list gives one whole row on each iteration.

**Example**

```python
marks = [[87, 72, 90], [95, 60, 70], [48, 55, 65]]
for row in marks:
    print(row)
```

**Output:**

```
[87, 72, 90]
[95, 60, 70]
[48, 55, 65]
```

`row` held a list on every iteration, not a single mark.

## Looping Through Every Value

Reaching the individual values needs a loop inside a loop. The outer loop takes each row, and the inner loop takes each column of that row.

**Example**

```python
marks = [[87, 72, 90], [95, 60, 70]]
for row in marks:
    for mark in row:
        print(mark, end=" ")
    print()
```

**Output:**

```
87 72 90 
95 60 70 
```

The inner loop runs completely for each row processed by the outer loop. The `print()` after the inner loop ends each row, which is why the output keeps its table shape.

## Calculating a Row Total

A calculation placed between the two loops applies to each row separately.

**Example**

```python
marks = [[87, 72, 90], [95, 60, 70], [48, 55, 65]]
for row in marks:
    total = 0
    for mark in row:
        total = total + mark
    print(row, "Total:", total)
```

**Output:**

```
[87, 72, 90] Total: 249
[95, 60, 70] Total: 225
[48, 55, 65] Total: 168
```

`total` is created inside the outer loop, so it starts again at `0` for every row.

## Building a 2D List

A 2D list is built with `append()`, giving it a whole row on each call.

**Example**

```python
marks = []
marks.append([87, 72, 90])
marks.append([95, 60, 70])
print(marks)
```

**Output:**

```
[[87, 72, 90], [95, 60, 70]]
```

Each call added one row, so the outer list grew by one element each time.

## Summary

The key points about 2D lists:

- A 2D list is a list of lists arranged to represent data in rows and columns.
- `len()` on a 2D list gives the number of rows, and `len()` on one row gives the number of columns.
- A single index gives a whole row, which is itself a list.
- Two indexes give one value, with the first selecting the row and the second the column.
- Both indexes start at `0`.
- Assigning through two indexes changes a single value.
- A `for` loop over a 2D list gives one row on each iteration.
- A loop inside a loop reaches every individual value.
- `append()` adds a whole row to a 2D list.

## Further Reading

- 📎 **Official Python guide to lists** — https://docs.python.org/3/tutorial/datastructures.html

You now know how to use lists to store, access and process collections of data, including table-shaped data with 2D lists. But lists are changeable, and sometimes you want a collection whose values must stay fixed.

Next, you will learn about tuples, a sequence that cannot be changed after it is created.
