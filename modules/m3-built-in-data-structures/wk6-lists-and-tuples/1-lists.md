# Lists

Three test marks need storing:

```python
mark_one = 78
mark_two = 91
mark_three = 64
```

Three values, three names. A class of forty needs forty names, and every calculation has to mention each one by hand. Worse, the number of names has to be decided while the program is being written, so the same program cannot handle a class of thirty-nine.

A **list** solves this. It is one name holding many values, kept in order.

## Creating a List

A list is written as values separated by commas, inside square brackets:

```python
marks = [78, 91, 64]
print(marks)
```

**Output:**

```
[78, 91, 64]
```

One name now holds all three values.

Printing a list shows the brackets and the commas exactly as they were typed. That is how Python displays a list, and it is a quick way to see the whole thing at once.

The values are held **in order**. `78` is first because it was written first, and that order does not change on its own.

## What a List Can Hold

A list can hold values of any type:

```python
names = ["Anita", "Ravi", "Meera"]
prices = [19.99, 5.50, 120.00]
answers = [True, False, True]
```

The values do not have to be the same type as each other. A single list can mix them:

```python
record = ["Anita", 21, 88.5, True]
print(record)
```

**Output:**

```
['Anita', 21, 88.5, True]
```

Notice that the string appears in quotes here. Printing a list prints each value the way you would type it in code, so strings keep their quotes. Printing a string on its own does not show them.

Mixing types is allowed, but a list is usually clearest when every item means the same kind of thing: all marks, all names, all prices.

## The Empty List

A list can start with nothing in it:

```python
scores = []
print(scores)
```

**Output:**

```
[]
```

An empty list is not an error and it is not `None`. It is a real list that happens to contain no values, which is exactly what you want before a program has collected anything.

## Counting the Items

The `len()` function reports how many items a list holds:

```python
marks = [78, 91, 64]
print(len(marks))

scores = []
print(len(scores))
```

**Output:**

```
3
0
```

`len()` counts items, not characters. A list of three names has a length of `3` no matter how long the names are:

```python
names = ["Anita", "Ravi", "Meera"]
print(len(names))
```

**Output:**

```
3
```

## Adding an Item with append

The `append()` method adds one value to the end of a list:

```python
scores = []
scores.append(78)
scores.append(91)
print(scores)
print(len(scores))
```

**Output:**

```
[78, 91]
2
```

Two points matter here.

`append()` adds exactly one item, and always at the end. The list keeps its existing order and grows by one.

`append()` changes the list itself. It does not produce a new list, and it does not need assigning to anything. Writing `scores = scores.append(78)` is a common early mistake and it destroys the list, because `append()` hands back nothing:

```python
scores = [78, 91]
result = scores.append(64)
print(result)
```

**Output:**

```
None
```

Call it on its own line and let it do its work.

## Building a List in a Loop

Together, the empty list and `append()` let a program build up a list as it goes:

```python
squares = []
for number in range(1, 6):
    squares.append(number * number)
print(squares)
```

**Output:**

```
[1, 4, 9, 16, 25]
```

The list started empty and gained one item per iteration. This is the standard shape for collecting results: create an empty list before the loop, append inside it, use the finished list after it.

The same shape collects input:

```python
names = []
for count in range(3):
    name = input("Enter a name: ")
    names.append(name)
print(names)
print("Names collected:", len(names))
```

**Output:**

```
Enter a name: Anita
Enter a name: Ravi
Enter a name: Meera
['Anita', 'Ravi', 'Meera']
Names collected: 3
```

## Further Reading

- **Official Python guide to data structures** — https://docs.python.org/3/tutorial/datastructures.html
- **Lists with worked examples** — https://www.programiz.com/python-programming/list

A list holds many values under one name and keeps them in order. Next, you will reach into that order and pull out a single value.
