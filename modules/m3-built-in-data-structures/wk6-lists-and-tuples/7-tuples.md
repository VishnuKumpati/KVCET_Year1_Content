# Tuples

A list is mutable: it can be changed in place, and every name pointing at it sees the change. Sometimes that is exactly wrong. A pair of map coordinates, a date, a colour as three numbers — none of these should quietly gain a fourth item or have one replaced.

A **tuple** is an ordered sequence, like a list, that is **immutable**. Once built, its contents are fixed.

## Creating a Tuple

A tuple is written with round brackets:

```python
point = (3, 5)
print(point)
print(len(point))
```

**Output:**

```
(3, 5)
2
```

Printing shows the round brackets, which is how you tell a tuple from a list at a glance.

Like a list, a tuple can hold any types, mixed freely:

```python
student = ("Anita", 21, 88.5)
print(student)
```

**Output:**

```
('Anita', 21, 88.5)
```

## The Single-Item Tuple

One case needs care. Round brackets on their own mean grouping, as in arithmetic, so they do not make a tuple:

```python
not_a_tuple = ("a")
print(type(not_a_tuple))
```

**Output:**

```
<class 'str'>
```

What makes a tuple is the **comma**, not the brackets. A one-item tuple needs a trailing comma:

```python
one_item = ("a",)
print(type(one_item))
print(len(one_item))
```

**Output:**

```
<class 'tuple'>
1
```

The comma looks like a typing mistake and is not. Without it, `("a")` is just the string `"a"` in brackets.

## Indexing and Slicing

Everything positional works exactly as it does on a list, because both are ordered sequences:

```python
letters = ("a", "b", "c", "d", "e")
print(letters[0])
print(letters[-1])
print(letters[1:4])
print(letters[::-1])
```

**Output:**

```
a
e
('b', 'c', 'd')
('e', 'd', 'c', 'b', 'a')
```

A slice of a tuple gives a tuple, not a list. Slicing reads without changing anything, so immutability does not interfere with it.

Looping and membership work unchanged too:

```python
letters = ("a", "b", "c")
for letter in letters:
    print(letter)
print("b" in letters)
```

**Output:**

```
a
b
c
True
```

## What a Tuple Cannot Do

Assigning to a position fails:

```python
point = (3, 5)
point[0] = 9
```

**Output:**

```
TypeError: 'tuple' object does not support item assignment
```

So does every operation that would change the contents. A tuple has no `append()`, no `insert()`, no `remove()`, no `pop()`, no `sort()` and no `clear()` — not because they were left out, but because there is nothing for them to do to a value that cannot change.

Two methods survive, and both only read:

```python
votes = ("yes", "no", "yes")
print(votes.count("yes"))
print(votes.index("no"))
```

**Output:**

```
2
1
```

This is the whole trade. A tuple gives up flexibility and gains a guarantee: nothing anywhere in the program can alter it after it is built.

## Packing and Unpacking

The brackets can be left off entirely. A comma-separated list of values on the right of an assignment builds a tuple, which is called **packing**:

```python
point = 3, 5
print(point)
print(type(point))
```

**Output:**

```
(3, 5)
<class 'tuple'>
```

The reverse is **unpacking**: several names on the left, one tuple on the right:

```python
point = (3, 5)
x, y = point
print(x)
print(y)
```

**Output:**

```
3
5
```

The number of names must match the number of items:

```python
point = (3, 5)
x, y, z = point
```

**Output:**

```
ValueError: not enough values to unpack (expected 3, got 2)
```

Unpacking is what the two loop variables of `enumerate()` and `zip()` were doing all along. Each produces a pair, and the pair is unpacked into two names at the top of the loop.

## Swapping Two Variables

Packing and unpacking in the same statement swaps two values in one line:

```python
a = 10
b = 20
a, b = b, a
print(a, b)
```

**Output:**

```
20 10
```

The right-hand side is evaluated first, building the tuple `(20, 10)`. Only then is it unpacked into `a` and `b`, so neither original value is lost on the way. No temporary variable is needed.

## Returning Several Values

A function returns one value. Packing makes that one value a tuple, which is how a function appears to return several:

```python
def min_and_max(numbers):
    return min(numbers), max(numbers)

result = min_and_max([78, 91, 64])
print(result)
print(type(result))
```

**Output:**

```
(64, 91)
<class 'tuple'>
```

At the call, unpack it straight into names:

```python
def min_and_max(numbers):
    return min(numbers), max(numbers)

lowest, highest = min_and_max([78, 91, 64])
print("Lowest:", lowest)
print("Highest:", highest)
```

**Output:**

```
Lowest: 64
Highest: 91
```

This is the standard way a Python function hands back more than one result.

## Choosing Between a Tuple and a List

Both are ordered and both index the same way. The difference is whether the contents should change.

| Use a list when | Use a tuple when |
| --- | --- |
| items are added or removed as the program runs | the set of values is fixed once built |
| the items are many of the same kind of thing | the items are fixed parts of one thing |
| the length can vary | the length is part of the meaning |
| the order may be rearranged or sorted | the position tells you what each item means |

A list of marks is a list: marks get added, and every item means the same kind of thing. A coordinate is a tuple: there are always exactly two numbers, the first is always `x`, and a coordinate that changed underneath you would be a bug.

Immutability is also worth choosing for safety. Passing a tuple into a function guarantees the function cannot alter it, which is a guarantee a list cannot offer.

## Further Reading

- **Official Python guide to data structures** — https://docs.python.org/3/tutorial/datastructures.html
- **Tuples with worked examples** — https://www.programiz.com/python-programming/tuple

A tuple is an ordered sequence whose contents are fixed. Next, you will put lists inside lists and use the result as a table.
