# Nested Data Structures

One student's name and marks fit in a dictionary. A class has many students, so the program needs many of those dictionaries. They are stored in a list.

A list can hold dictionaries, a dictionary can hold lists, and a dictionary can hold other dictionaries. This is called nesting.

## A List of Dictionaries

Each element of the list is one record, written as a dictionary.

**Example**

```python
students = [
    {"name": "Asha", "marks": 87},
    {"name": "Ravi", "marks": 72}
]

print(len(students))
print(students[0])
```

**Output:**

```
2
{'name': 'Asha', 'marks': 87}
```

`len()` gives `2`, the number of students. `students[0]` gives the first element, which is a whole dictionary.

Reaching a value inside the dictionary takes two steps.

**Example**

```python
students = [
    {"name": "Asha", "marks": 87},
    {"name": "Ravi", "marks": 72}
]

print(students[0]["name"])
```

**Output:**

```
Asha
```

`students[0]` selects the dictionary, and `["name"]` then selects a value inside it. The brackets are read from left to right.

## Looping Through a List of Dictionaries

A `for` loop gives one dictionary on each iteration.

**Example**

```python
students = [
    {"name": "Asha", "marks": 87},
    {"name": "Ravi", "marks": 72}
]

for student in students:
    print(student["name"], student["marks"])
```

**Output:**

```
Asha 87
Ravi 72
```

`student` holds a whole dictionary, so its values are reached by key inside the loop.

An `if` statement inside the loop selects the records that are wanted.

**Example**

```python
students = [
    {"name": "Asha", "marks": 87},
    {"name": "Ravi", "marks": 30}
]

passed = []
for student in students:
    if student["marks"] >= 35:
        passed.append(student["name"])

print(passed)
```

**Output:**

```
['Asha']
```

## A Dictionary with List Values

A key can hold a list, which is how one name is given several values.

**Example**

```python
marks = {"Asha": [87, 72], "Ravi": [95, 60]}

print(marks["Asha"])
print(marks["Asha"][0])
print(len(marks["Asha"]))
```

**Output:**

```
[87, 72]
87
2
```

`marks["Asha"]` gives the list. `marks["Asha"][0]` goes one step further and gives its first element.

The list behaves like any other list, so its methods work on it.

**Example**

```python
marks = {"Asha": [87, 72]}
marks["Asha"].append(95)
print(marks)
```

**Output:**

```
{'Asha': [87, 72, 95]}
```

`marks["Asha"]` is the list, and `append()` is called on it directly.

## Looping Through a Dictionary of Lists

The outer loop takes each pair, and the inner loop takes each value in that pair's list.

**Example**

```python
marks = {"Asha": [87, 72], "Ravi": [95, 60]}

for name, scores in marks.items():
    total = 0
    for score in scores:
        total = total + score
    print(name, total)
```

**Output:**

```
Asha 159
Ravi 155
```

`scores` holds a list on every iteration, which the inner loop then goes through.

## A Dictionary of Dictionaries

A key can hold another dictionary, which is how a name is given several named facts.

**Example**

```python
students = {
    "Asha": {"class": 7, "marks": 87},
    "Ravi": {"class": 8, "marks": 72}
}

print(students["Asha"])
print(students["Asha"]["marks"])
```

**Output:**

```
{'class': 7, 'marks': 87}
87
```

The first key selects the student. The second selects a fact about that student.

A value is changed the same way, with the assignment at the end.

**Example**

```python
students = {"Asha": {"class": 7, "marks": 87}}
students["Asha"]["marks"] = 90
print(students)
```

**Output:**

```
{'Asha': {'class': 7, 'marks': 90}}
```

## Errors in Nested Access

Each key in a nested access must exist.

**Example**

```python
students = {"Asha": {"class": 7}}
print(students["Asha"]["marks"])
```

**Output:**

```
KeyError: 'marks'
```

`"Asha"` exists, but `"marks"` does not exist inside that dictionary.

## Choosing a Structure

The shape of the data decides the structure.

| Data | Structure |
| --- | --- |
| Many records, each with the same fields | a list of dictionaries |
| One name with several values | a dictionary with list values |
| One name with several named facts | a dictionary of dictionaries |

## Further Reading

- 📎 **Nested dictionaries with worked examples** — https://www.programiz.com/python-programming/nested-dictionary
- 📎 **Official Python guide to data structures** — https://docs.python.org/3/tutorial/datastructures.html

That completes strings, dictionaries and sets. You can now store text, find values by name, keep only unique values, and build structures that hold other structures.

Next week you will learn shorter ways to build these collections, using a single line in place of a loop.
