# Two-Dimensional Lists

Nothing says a list's items have to be single values. An item can be another list.

A list of lists is called a **two-dimensional list**, and it models anything arranged as a table: a grid of squares, a seating plan, a spreadsheet, or a class of students each with several marks.

## Creating One

Each inner list is one row, written as an ordinary item of the outer list:

```python
marks = [[78, 91, 64], [88, 72, 95], [60, 85, 70]]
print(marks)
```

**Output:**

```
[[78, 91, 64], [88, 72, 95], [60, 85, 70]]
```

That is hard to read on one line. Python allows a list to be spread across several lines, and laying the rows out one per line makes the table visible:

```python
marks = [
    [78, 91, 64],
    [88, 72, 95],
    [60, 85, 70],
]
```

The trailing comma after the last row is allowed and is good practice: adding another row later becomes a one-line change.

The outer list has three items, and each of them is a list of three:

```python
print(len(marks))
print(len(marks[0]))
```

**Output:**

```
3
3
```

`len()` on the outer list counts rows. `len()` on a row counts the values in that row. The rows do not have to be the same length, though for a table they normally are.

## Double Indexing

One index reaches a row, which is itself a list:

```python
marks = [
    [78, 91, 64],
    [88, 72, 95],
    [60, 85, 70],
]
print(marks[0])
```

**Output:**

```
[78, 91, 64]
```

A second index then reaches into that row:

```python
print(marks[0][1])
print(marks[2][0])
```

**Output:**

```
91
60
```

Read `marks[0][1]` left to right: take row `0`, then take item `1` of it. The first bracket is the row and the second is the column.

```
              column 0   column 1   column 2
   row 0         78         91         64
   row 1         88         72         95
   row 2         60         85         70
```

Both indexes count from `0`, and both raise `IndexError` when they run past the end.

Negative indexing works at either level, so `marks[-1][-1]` is the last value of the last row:

```python
print(marks[-1][-1])
```

**Output:**

```
70
```

## Changing One Cell

Assigning to a double index replaces a single value:

```python
marks = [
    [78, 91, 64],
    [88, 72, 95],
    [60, 85, 70],
]
marks[1][2] = 99
print(marks[1])
print(marks)
```

**Output:**

```
[88, 72, 99]
[[78, 91, 64], [88, 72, 99], [60, 85, 70]]
```

Assigning to a single index replaces a whole row:

```python
marks[0] = [50, 50, 50]
print(marks)
```

**Output:**

```
[[50, 50, 50], [88, 72, 99], [60, 85, 70]]
```

## Visiting Every Cell

A loop inside a loop visits every cell. The outer loop takes one row at a time, and the inner loop takes one value out of that row:

```python
marks = [
    [78, 91, 64],
    [88, 72, 95],
    [60, 85, 70],
]
for row in marks:
    for mark in row:
        print(mark, end=" ")
    print()
```

**Output:**

```
78 91 64
88 72 95
60 85 70
```

The structure is the same one that prints a rectangular pattern: the outer loop controls rows, the inner loop controls what goes across each row, and the empty `print()` after the inner loop ends the line. The only difference is that the values come out of real data rather than being counted.

When the positions are needed, both loops run over ranges instead:

```python
for row_number in range(len(marks)):
    for column_number in range(len(marks[row_number])):
        print(row_number, column_number, marks[row_number][column_number])
```

**Output:**

```
0 0 78
0 1 91
0 2 64
1 0 88
1 1 72
1 2 95
2 0 60
2 1 85
2 2 70
```

`range(len(marks))` gives the row numbers, and `range(len(marks[row_number]))` gives the column numbers for that particular row. Written this way, rows of different lengths are handled correctly.

## Row by Row Against Cell by Cell

Not every job needs both loops. Decide first whether a row is a unit of meaning.

When each row is one student's marks, a single loop over the rows is enough, because the work happens to a whole row at a time. The `sum()` function adds up the numbers in a list, which saves writing an accumulator by hand:

```python
students = ["Anita", "Ravi", "Meera"]
marks = [
    [78, 91, 64],
    [88, 72, 95],
    [60, 85, 70],
]
for name, row in zip(students, marks):
    print(name, "total:", sum(row), "average:", sum(row) / len(row))
```

**Output:**

```
Anita total: 233 average: 77.66666666666667
Ravi total: 255 average: 85.0
Meera total: 215 average: 71.66666666666667
```

When the question is about every value regardless of which row it sits in, the inner loop is needed:

```python
highest = 0
for row in marks:
    for mark in row:
        if mark > highest:
            highest = mark
print("Highest mark:", highest)
```

**Output:**

```
Highest mark: 95
```

One loop for a question about rows, two for a question about individual values.

## Building a Two-Dimensional List

The same shape that built a flat list builds a table, one level deeper. An empty outer list, and an empty inner list created fresh at the top of each outer iteration:

```python
grid = []
for row_number in range(3):
    row = []
    for column_number in range(4):
        row.append(0)
    grid.append(row)
print(grid)
```

**Output:**

```
[[0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0]]
```

Three rows of four zeroes. Note where `row = []` sits: inside the outer loop, so a new row is started each time round, and `grid.append(row)` runs after the inner loop has filled it.

There is a shortcut that looks like it does the same thing and does not:

```python
grid = [[0] * 4] * 3
grid[0][0] = 9
print(grid)
```

**Output:**

```
[[9, 0, 0, 0], [9, 0, 0, 0], [9, 0, 0, 0]]
```

One cell was assigned to and three changed. `* 3` did not make three rows; it made three references to the same single row, so all three names lead to the same list. This is aliasing, and it bites hardest here because the outer structure looks convincing.

Build rows in a loop, as above, and each row is a separate list.

## Further Reading

- **Official Python guide to data structures** — https://docs.python.org/3/tutorial/datastructures.html
- **Two-dimensional lists in Python** — https://www.geeksforgeeks.org/python/python-using-2d-arrays-lists-the-right-way/

A list of lists stores a table, and a loop inside a loop walks it. Next, you will put a list's items into order.
