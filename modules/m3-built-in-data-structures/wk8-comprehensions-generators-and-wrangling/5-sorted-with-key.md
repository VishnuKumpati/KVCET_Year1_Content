# Sorting with a Key

Python's `sorted()` function arranges items in ascending order by default. When working with structured data, however, the order may need to depend on a specific field rather than the item itself.

The `key` argument lets you define what value Python should use for sorting. It accepts a function, including a `lambda`, that receives each item and returns the value to compare.

**Syntax**

```
sorted(iterable, key=function)
```

## Sorting a List of Dictionaries

The `key` function receives each dictionary and returns the score that should determine its position in the sorted list.

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

Setting `reverse=True` sorts by score in descending order, placing the highest score first.

## Sorting a Dictionary by Value

By default, iterating over a dictionary gives its keys. To sort by value, use `items()` to get its key-value pairs, then pass a `key` function to `sorted()`.

`items()` produces each pair as a tuple, with the key at position `0` and the value at position `1`. That is why `pair[1]` refers to the score.

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

The `key` function can return a tuple containing multiple values. Python compares them from left to right: it compares the first value, then uses the second to break ties, and so on.

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

By default, Python sorts strings by their character values, so uppercase and lowercase letters can affect the order. A `key` can remove that difference.

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

`sorted()` returns a new list containing all the items in their sorted order. Generators offer a different approach: they produce values one at a time as needed.

Next, you will learn about generators.
