# Iteration

A list holds many values, and most useful work visits all of them. A `for` loop walks a sequence one item at a time, and a list is a sequence, so the two fit together directly.

## Looping over a List

The loop variable holds one item per iteration:

```python
names = ["Anita", "Ravi", "Meera"]
for name in names:
    print("Welcome,", name)
```

**Output:**

```
Welcome, Anita
Welcome, Ravi
Welcome, Meera
```

The loop ran three times because the list has three items, and it took them in order. Nothing had to state the length, and the same two lines work for a list of three hundred.

The loop variable holds the **value**, not the position. This is what you want most of the time:

```python
marks = [78, 91, 64]
total = 0
for mark in marks:
    total = total + mark
print("Total:", total)
print("Average:", total / len(marks))
```

**Output:**

```
Total: 233
Average: 77.66666666666667
```

## Membership with in and not in

`in` tests whether a value is somewhere in a list, and gives back `True` or `False`:

```python
names = ["Anita", "Ravi", "Meera"]
print("Ravi" in names)
print("Sunil" in names)
```

**Output:**

```
True
False
```

`not in` is the opposite test, and reads better than negating the whole expression:

```python
names = ["Anita", "Ravi"]
if "Sunil" not in names:
    names.append("Sunil")
print(names)
```

**Output:**

```
['Anita', 'Ravi', 'Sunil']
```

`in` compares whole items, not parts of them. A list of names contains `"Ravi"`; it does not contain `"Rav"`.

This is also the safe way to use `index()` and `remove()`, both of which raise `ValueError` on a missing value:

```python
names = ["Anita", "Ravi", "Meera"]
if "Ravi" in names:
    print(names.index("Ravi"))
```

**Output:**

```
1
```

## Looping with the Index

Sometimes the position is needed as well as the value. `range(len(items))` produces every valid index:

```python
names = ["Anita", "Ravi", "Meera"]
for position in range(len(names)):
    print(position, names[position])
```

**Output:**

```
0 Anita
1 Ravi
2 Meera
```

`len(names)` is `3`, so `range(3)` gives `0`, `1`, `2` — exactly the valid indexes, and never one too many. This is also why `range()` excluding its stop value is convenient rather than awkward.

The positions are what make this form necessary. Changing items in place needs an index, because assigning to the loop variable changes only the variable:

```python
marks = [78, 91, 64]
for position in range(len(marks)):
    marks[position] = marks[position] + 5
print(marks)
```

**Output:**

```
[83, 96, 69]
```

## enumerate

Numbering output by hand with `range(len(items))` works, but it reaches back into the list for every value. `enumerate()` hands over the position and the value together:

```python
names = ["Anita", "Ravi", "Meera"]
for position, name in enumerate(names):
    print(position, name)
```

**Output:**

```
0 Anita
1 Ravi
2 Meera
```

Two loop variables appear, separated by a comma, because `enumerate()` produces a pair each time round.

Counting from `0` is rarely what a reader wants to see, so `enumerate()` takes a starting number:

```python
names = ["Anita", "Ravi", "Meera"]
for number, name in enumerate(names, start=1):
    print(number, name)
```

**Output:**

```
1 Anita
2 Ravi
3 Meera
```

The `start` value changes only the number reported. It does not change which items are visited or in what order.

Choose between the two forms by what the loop does. `enumerate()` when the position is only being displayed or reported; `range(len(items))` when items are being assigned to by index.

## zip

Two lists that line up item for item can be walked together with `zip()`:

```python
names = ["Anita", "Ravi", "Meera"]
marks = [78, 91, 64]
for name, mark in zip(names, marks):
    print(name, "scored", mark)
```

**Output:**

```
Anita scored 78
Ravi scored 91
Meera scored 64
```

`zip()` pairs the first item of each list, then the second, and so on. It does the same job as looping over `range(len(names))` and indexing both lists, with less to get wrong.

When the lists are different lengths, `zip()` stops at the end of the shorter one:

```python
names = ["Anita", "Ravi", "Meera"]
marks = [78, 91]
for name, mark in zip(names, marks):
    print(name, mark)
```

**Output:**

```
Anita 78
Ravi 91
```

`"Meera"` was silently dropped. No error is raised, so check the lengths yourself when they are meant to match.

`zip()` takes more than two lists if needed, producing one item from each per iteration.

## Do Not Change a List While Looping over It

A `for` loop tracks its position in the list as it goes. Adding or removing items moves the remaining items, and the loop does not know about it.

Here is a loop that should remove every even number:

```python
numbers = [2, 4, 6]
for number in numbers:
    if number % 2 == 0:
        numbers.remove(number)
print(numbers)
```

**Output:**

```
[4]
```

The list should be empty. What happened is that removing `2` shifted `4` and `6` down one position each, and the loop had already moved on to position `1` — which now held `6`, not `4`. `4` was skipped entirely, and the loop ran out of positions before reaching it again.

Adding inside the loop is worse: a loop that appends to the list it is walking never finishes, because the list grows as fast as the loop advances.

The rule is simple: do not add to or remove from a list while looping over it.

There are two straightforward ways round it. Loop over a copy and change the original:

```python
numbers = [2, 4, 6]
for number in numbers.copy():
    if number % 2 == 0:
        numbers.remove(number)
print(numbers)
```

**Output:**

```
[]
```

Or build a new list of the items you want to keep, which avoids the problem instead of working around it:

```python
numbers = [2, 3, 4, 5, 6]
odd_numbers = []
for number in numbers:
    if number % 2 != 0:
        odd_numbers.append(number)
print(odd_numbers)
```

**Output:**

```
[3, 5]
```

The second form is usually clearer, and it leaves the original list intact for anything else that needs it.

## Further Reading

- **Official Python guide to control flow** — https://docs.python.org/3/tutorial/controlflow.html
- **enumerate and zip in practice** — https://realpython.com/python-enumerate/

You can now visit every item of a list, with or without its position, and walk two lists side by side. Next, you will meet a sequence that indexes and slices in exactly the same way but cannot be changed at all.
