# Star Patterns

Conditions decide whether a block runs. Loops decide how many times it runs. A nested loop puts one loop inside another, so the inner loop runs completely for every iteration of the outer loop.

Printing shapes is a clear way to practise this relationship. A shape has rows going down and characters going across, so the outer loop controls the rows and the inner loop controls the characters in each row. If the outer loop runs the wrong number of times, the shape has the wrong number of rows. If the inner loop runs the wrong number of times, the rows have the wrong width.

Because the result appears directly on the screen, you can see how changing either loop changes the shape.

Each pattern below changes one part of that relationship.

## Printing Without a Line Break

One detail comes first. `print()` ends every call with a line break, so stars printed in a loop land one to a line:

```python
for star in range(3):
    print("*")
```

**Output:**

```
*
*
*
```

The `end` setting controls what `print()` puts at the end of its output:

```python
for star in range(3):
    print("*", end="")
print()
```

**Output:**

```
***
```

`end=""` replaces the line break with nothing, so each `print()` carries on where the last one stopped. The empty `print()` afterwards supplies the line break that closes the row.

That gives the structure every pattern uses. The inner loop prints characters with `end=""`, staying on one line, and an empty `print()` after the inner loop ends the row.

## The Square

Four rows, four stars in each:

```python
for row in range(4):
    for star in range(4):
        print("*", end="")
    print()
```

**Output:**

```
****
****
****
****
```

The outer loop starts the inner loop four times, and the inner loop completes all four of its iterations before the outer loop moves on.

The `print()` is indented to the outer loop, not the inner one, so it runs once per row. Move it one level deeper and it would run after every star, giving sixteen lines of one star each.

## The Right-Angled Triangle

In the square, the inner loop count never changed. Here it does, and the value comes from the outer loop variable:

```python
for row in range(1, 5):
    for star in range(row):
        print("*", end="")
    print()
```

**Output:**

```
*
**
***
****
```

`range(row)` instead of `range(4)` is the only change.

| `row` | Stars printed |
| --- | --- |
| 1 | `*` |
| 2 | `**` |
| 3 | `***` |
| 4 | `****` |

The outer loop starts at `1` rather than `0`, because `range(0)` produces no values and the first row would be empty.

## The Inverted Right-Angled Triangle

Counting down instead of up turns the triangle over:

```python
for row in range(4, 0, -1):
    for star in range(row):
        print("*", end="")
    print()
```

**Output:**

```
****
***
**
*
```

`range(4, 0, -1)` uses the third value, the step. Here it is `-1`, so the values run `4`, `3`, `2`, `1`, stopping before `0` exactly as a stop value is always excluded.

The inner loop is untouched. Only the direction of the outer loop changed.

## The Mirrored Right-Angled Triangle

To push a row to the right, print spaces before the stars. A block can hold as many statements as you need, so a second inner loop goes above the first:

```python
rows = 4
for row in range(1, rows + 1):
    for space in range(rows - row):
        print(" ", end="")
    for star in range(row):
        print("*", end="")
    print()
```

**Output:**

```
   *
  **
 ***
****
```

| `row` | Spaces, `rows - row` | Stars, `row` |
| --- | --- | --- |
| 1 | 3 | 1 |
| 2 | 2 | 2 |
| 3 | 1 | 3 |
| 4 | 0 | 4 |

The spaces shrink as the stars grow, so the right edge stays in one place while the left edge moves outwards.

Both inner loops sit at the same indentation level, so they run one after the other within a single row. The `print()` comes after both.

## The Pyramid

A pyramid is centred, so each row needs spaces on the left and an odd number of stars:

```python
rows = 4
for row in range(1, rows + 1):
    for space in range(rows - row):
        print(" ", end="")
    for star in range(2 * row - 1):
        print("*", end="")
    print()
```

**Output:**

```
   *
  ***
 *****
*******
```

| `row` | Spaces, `rows - row` | Stars, `2 * row - 1` |
| --- | --- | --- |
| 1 | 3 | 1 |
| 2 | 2 | 3 |
| 3 | 1 | 5 |
| 4 | 0 | 7 |

Both counts are arithmetic expressions built from `row`. The spaces shrink by one each row, which moves the row leftwards. The stars grow by two each row, one added to each side, which keeps the shape symmetrical.

## The Inverted Pyramid

The same two expressions, with the outer loop counting down:

```python
rows = 4
for row in range(rows, 0, -1):
    for space in range(rows - row):
        print(" ", end="")
    for star in range(2 * row - 1):
        print("*", end="")
    print()
```

**Output:**

```
*******
 *****
  ***
   *
```

`row` now runs `4`, `3`, `2`, `1`. The widest row comes first, and the spaces grow as the stars shrink.

## The Diamond

A diamond is a pyramid with an inverted pyramid below it, so it takes two outer loops, one after the other:

```python
rows = 4
for row in range(1, rows + 1):
    for space in range(rows - row):
        print(" ", end="")
    for star in range(2 * row - 1):
        print("*", end="")
    print()
for row in range(rows - 1, 0, -1):
    for space in range(rows - row):
        print(" ", end="")
    for star in range(2 * row - 1):
        print("*", end="")
    print()
```

**Output:**

```
   *
  ***
 *****
*******
 *****
  ***
   *
```

The second loop starts at `rows - 1`, not at `rows`. Starting at `rows` would print the widest row a second time and split the diamond across its middle.

## Reading Any Pattern

Every pattern here was built by answering three questions:

- How many rows are there? That sets the outer loop.
- How many characters go in each row? That sets the inner loop.
- Does the count change from row to row? If it does, the inner loop count is worked out from the outer loop variable.

Given an unfamiliar pattern, count the characters in each row and write those counts down beside the row numbers. The relationship between the two columns is the expression the inner loop needs.

## Further Reading

- **Official Python guide to control flow** — https://docs.python.org/3/tutorial/controlflow.html

That is the end of loops. Next, you will give a block of code a name, so you can run that same work again without writing the instructions from scratch.
