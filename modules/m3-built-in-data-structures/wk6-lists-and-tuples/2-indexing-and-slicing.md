# Indexing and Slicing

A list keeps its values in order, so each value has a position. That position is called its **index**, and it is how you reach one value out of the many.

## Position Numbering

Positions are numbered from `0`, not from `1`:

```
 letters:    "a"    "b"    "c"    "d"    "e"
 index:       0      1      2      3      4
```

The reason is that an index is a **distance from the start**, not a count. The first item is zero steps from the start, the second is one step, and so on. Counting this way is what makes the arithmetic in the rest of this topic work out evenly.

The consequence to remember: a list of five items has indexes `0` to `4`. The last index is always `len(items) - 1`.

## Reading One Item

Write the index in square brackets after the list name:

```python
letters = ["a", "b", "c", "d", "e"]
print(letters[0])
print(letters[3])
```

**Output:**

```
a
d
```

`letters[0]` is a single value, not a list, so it prints without brackets. It can be used anywhere a value can be used:

```python
marks = [78, 91, 64]
total = marks[0] + marks[1] + marks[2]
print("Total:", total)
```

**Output:**

```
Total: 233
```

The last item is at `len(items) - 1`, which is a common thing to need:

```python
marks = [78, 91, 64]
print(marks[len(marks) - 1])
```

**Output:**

```
64
```

## IndexError

Asking for a position that does not exist is an error:

```python
letters = ["a", "b", "c"]
print(letters[3])
```

**Output:**

```
IndexError: list index out of range
```

This list has three items, so its valid indexes are `0`, `1` and `2`. Index `3` would be the fourth item, and there isn't one.

`IndexError` is one of the most common errors in list code, and it almost always means the same thing: a position was calculated as if counting from `1`, or a loop ran one iteration too many.

## Negative Indexing

Counting from the end is also allowed, using negative numbers:

```
 letters:    "a"    "b"    "c"    "d"    "e"
 index:       0      1      2      3      4
 negative:   -5     -4     -3     -2     -1
```

```python
letters = ["a", "b", "c", "d", "e"]
print(letters[-1])
print(letters[-2])
```

**Output:**

```
e
d
```

`-1` is the last item, `-2` is the one before it. There is no `-0`, because `0` already means the first item.

Negative indexing is why `marks[-1]` is preferred over `marks[len(marks) - 1]`. Both give the last item, and the first is shorter and harder to get wrong.

## Changing One Item

An index can be assigned to, which replaces the value at that position:

```python
marks = [78, 91, 64]
marks[1] = 95
print(marks)
```

**Output:**

```
[78, 95, 64]
```

The list is the same list, with one value replaced. Its length has not changed, and the other items were untouched.

This works with negative indexes too:

```python
marks = [78, 91, 64]
marks[-1] = 70
print(marks)
```

**Output:**

```
[78, 91, 70]
```

Assigning to a position that does not exist raises `IndexError` for the same reason reading it does. Assignment replaces an item; it cannot create one.

## Slicing

An index takes one item. A **slice** takes a range of items, and gives back a new list:

```python
letters = ["a", "b", "c", "d", "e"]
print(letters[1:4])
```

**Output:**

```
['b', 'c', 'd']
```

The form is `[start:stop]`. The slice begins at `start` and ends **before** `stop`, so the item at `stop` is not included. That is the same rule `range()` follows, and for the same reason: `stop - start` is then the number of items taken. Here `4 - 1` is `3`, and three items came back.

A slice always produces a list, even a slice of one item:

```python
letters = ["a", "b", "c", "d", "e"]
print(letters[2:3])
print(letters[2])
```

**Output:**

```
['c']
c
```

Unlike an index, a slice does not raise an error when it runs past the end. It simply stops at the end of the list:

```python
letters = ["a", "b", "c", "d", "e"]
print(letters[3:99])
```

**Output:**

```
['d', 'e']
```

## The Three Shorthand Forms

Either side of the colon may be left out, and Python fills in the obvious value:

```python
letters = ["a", "b", "c", "d", "e"]
print(letters[:3])
print(letters[2:])
print(letters[:])
```

**Output:**

```
['a', 'b', 'c']
['c', 'd', 'e']
['a', 'b', 'c', 'd', 'e']
```

| Form | Meaning |
| --- | --- |
| `items[:n]` | from the start up to but not including `n` |
| `items[n:]` | from `n` to the end |
| `items[:]` | every item, as a new list |

The first two split a list at a chosen point. The third looks pointless, since it appears to give back the same list, but it is the standard way to make a **copy** of a list.

## Slicing with a Step

A third number sets the step, exactly as it does in `range()`:

```python
letters = ["a", "b", "c", "d", "e"]
print(letters[0:5:2])
print(letters[::2])
```

**Output:**

```
['a', 'c', 'e']
['a', 'c', 'e']
```

The slice took every second item. With `start` and `stop` left out, `[::2]` reads as "every second item of the whole list".

A negative step walks backwards, which reverses the list:

```python
letters = ["a", "b", "c", "d", "e"]
print(letters[::-1])
```

**Output:**

```
['e', 'd', 'c', 'b', 'a']
```

`[::-1]` is worth recognising on sight. It appears constantly in real code and it always means the same thing: the whole sequence, reversed, as a new list.

## Further Reading

- **Official Python guide to data structures** — https://docs.python.org/3/tutorial/datastructures.html
- **List slicing explained** — https://www.geeksforgeeks.org/python/python-list-slicing/

Indexing reaches one item and slicing reaches a range of them. Next, you will change the list itself: adding items, removing them, and dealing with what that means.
