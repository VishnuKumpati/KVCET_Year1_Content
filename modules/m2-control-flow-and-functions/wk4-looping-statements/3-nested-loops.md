# Nested Loops

A loop repeats a block of statements, and that block can hold any statement at all, including another loop.

That is what a grid needs. Take a hall with 3 rows and 2 seats in every row. Printing every seat position means going through the rows one at a time, and then, for each of those rows, going through its 2 seats. Two loops, one inside the other.

## Loops Inside Loops

A **nested loop** is a loop written inside the block of another loop. The outer loop controls how many times the inner loop is started, and the inner loop completes all of its own iterations before the outer loop moves on.

```python
for row in range(3):
    for seat in range(2):
        print(row, seat)
```

**Output:**

```
0 0
0 1
1 0
1 1
2 0
2 1
```

Six lines: the outer loop runs 3 times, and the inner loop runs 2 times for each outer iteration. `range()` counts from 0, so the first row and the first seat are both numbered `0`.

```
row = 0
 ├─ seat = 0
 └─ seat = 1

row = 1
 ├─ seat = 0
 └─ seat = 1

row = 2
 ├─ seat = 0
 └─ seat = 1
```

Read the output beside the tree and the pattern is clear. `row` changes slowly and `seat` changes quickly, because `seat` finishes its whole sequence before `row` moves on.

The indentation says which loop a line belongs to. The `print` is inside both loops, so it runs on every iteration of the inner one.

## Nesting a while Loop

Any loop can be nested inside any other. Two `while` loops nest the same way:

```python
row = 1
while row <= 2:
    seat = 1
    while seat <= 2:
        print(row, seat)
        seat += 1
    row += 1
```

**Output:**

```
1 1
1 2
2 1
2 2
```

One detail matters here. The statement `seat = 1` is inside the outer block, not above the outer loop. The inner loop leaves `seat` at `3`, so without that reset the inner condition would be `False` on the second outer iteration and the inner loop would never run again.

A nested `for` loop needs no such reset, because `range()` produces its sequence afresh each time the inner loop starts.

## Further Reading

- **Official Python guide to control flow** — https://docs.python.org/3/tutorial/controlflow.html

Every loop so far has run to its natural end. Next, you will learn how to end a loop early and how to skip a single iteration.
