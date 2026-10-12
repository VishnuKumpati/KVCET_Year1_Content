# Sorting with a Key

`sorted()` on its own compares the items directly. Numbers sort by value and strings sort alphabetically.

That works until each item contains multiple values. For example, when sorting students represented as dictionaries, should Python compare their names or their scores? The `key` argument lets us choose which field determines the order.

It takes a function, and `sorted()` sorts by whatever that function returns.

**Syntax**

```
sorted(iterable, key=function)
```

## Sorting a List of Dictionaries

The function receives one item at a time and returns the field to sort by.

**Example**

```python
students = [
    {"name": "Asha", "score": 87},
    {"name": "Ravi", "score": 30},
    {"name": "Meera", "score": 95}
]

ordered = sorted(students, key=lambda student: student["score"])

for student in ordered:
    print(student["name"], student["score"])
```

**Output:**

```
Ravi 30
Asha 87
Meera 95
```

The lambda receives one dictionary and returns its score. `sorted()` compares `30`, `87` and `95`, and moves the whole dictionaries into that order, so each name stays with its own score.

## Sorting in Reverse

The `reverse` argument works alongside `key`.

**Example**

```python
students = [
    {"name": "Asha", "score": 87},
    {"name": "Ravi", "score": 30},
    {"name": "Meera", "score": 95}
]

ordered = sorted(students, key=lambda student: student["score"], reverse=True)
print([student["name"] for student in ordered])
```

**Output:**

```
['Meera', 'Asha', 'Ravi']
```

The highest score now comes first.

## Sorting a Dictionary by Value

By default, iterating over a dictionary gives its keys. To sort by value, use `items()` to get its key-value pairs, then pass a `key` function to `sorted()`.

**Example**

```python
results = {"Asha": 87, "Ravi": 30, "Meera": 95}
ordered = sorted(results.items(), key=lambda pair: pair[1])
print(ordered)
```

**Output:**

```
[('Ravi', 30), ('Asha', 87), ('Meera', 95)]
```

Each pair looks like `('Asha', 87)`, so `pair[0]` is the name and `pair[1]` is the score. Returning `pair[1]` sorts by score.

The result is a list of tuples, not a dictionary.

## Sorting by Two Fields

When the key function returns a tuple, Python compares the first values. If they are equal, it compares the second values, and continues if needed.

**Example**

```python
students = [
    {"name": "Ravi", "class": 7, "score": 87},
    {"name": "Asha", "class": 7, "score": 95},
    {"name": "Meera", "class": 6, "score": 72}
]

ordered = sorted(students, key=lambda student: (student["class"], student["name"]))

for student in ordered:
    print(student["class"], student["name"])
```

**Output:**

```
6 Meera
7 Asha
7 Ravi
```

Meera is first because class 6 comes before class 7. Asha and Ravi are both in class 7, so their names decide the order between them.

## Ignoring Capital Letters

Sorting strings puts capital letters before lowercase ones. A `key` can remove that difference.

**Example**

```python
names = ["ravi", "Zoya", "meera"]
print(sorted(names))
print(sorted(names, key=lambda name: name.lower()))
```

**Output:**

```
['Zoya', 'meera', 'ravi']
['meera', 'ravi', 'Zoya']
```

The first sort put `Zoya` first because `Z` is a capital letter. The second sort compared the lowercase versions of the names but kept the original names in the output. This gives a case-insensitive alphabetical order.

## sorted() Returns a New List

`sorted()` returns a new list and leaves the original alone, whatever the `key` does.

**Example**

```python
scores = [87, 30, 95]
ordered = sorted(scores, key=lambda score: -score)
print(ordered)
print(scores)
```

**Output:**

```
[95, 87, 30]
[87, 30, 95]
```

The lambda returned `-87`, `-30` and `-95`, which sorted the values from highest to lowest. The list itself still holds the original positive numbers.

## Further Reading

- 📎 **Sorting with key explained** — https://docs.python.org/3/howto/sorting.html
- 📎 **Official Python reference for sorted()** — https://docs.python.org/3/library/functions.html#sorted

A `key` function tells Python what value to compare for each item. Python also lets us produce values one at a time without creating an entire list in advance.

Next, you will learn about generators.
