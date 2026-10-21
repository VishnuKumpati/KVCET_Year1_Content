# The Standard Library

Python comes with a large set of ready-made modules for common tasks. Instead of writing everything from scratch, you can import these modules and use them straight away.

> **The Python standard library is the large collection of modules that comes with Python. These modules can be imported without installing anything.**

This is why Python is often described as having **"batteries included"**: many useful tools are ready to use from the start.

## Standard Library Modules You Have Already Used

Several modules in this course come from the standard library:

| Module | Used For | Topic |
| --- | --- | --- |
| `abc` | Creating abstract classes | **Abstraction** |
| `os` | Working with folders and paths | **File Paths and Errors** |
| `csv` | Reading and writing CSV files | **CSV Files** |
| `json` | Reading and writing JSON data | **JSON Files** |
| `math` | Mathematical functions | **Modules and Imports** |
| `re` | Searching for patterns in text | **Regular Expressions** |

The sections below introduce three more useful modules.

## The `random` Module

The **`random`** module provides functions for generating **pseudo-random** values and selecting random items. Pseudo-random values look random, but Python produces them using a calculation.

```python
import random

print(random.randint(1, 6))
print(random.choice(["Ravi", "Priya", "Arun"]))
```

**Possible output:**

```
6
Ravi
```

- `random.randint(1, 6)` returns a random whole number from 1 to 6, including both 1 and 6, like rolling a dice.
- `random.choice()` picks one random item from a list.

**The output changes each time the program runs**, because the values are random.

## The `statistics` Module

The **`statistics`** module calculates common statistics, such as the average of a set of numbers.

```python
import statistics

marks = [85, 92, 78, 90, 70]

print(statistics.mean(marks))
print(statistics.median(marks))
```

**Output:**

```
83
85
```

- `statistics.mean()` returns the **average**: the total of all the marks divided by the number of marks.
- `statistics.median()` returns the **middle value** when the numbers are arranged in order. Here, the ordered marks are `70, 78, 85, 90, 92`, so the median is `85`. If there is an even number of values, it returns the average of the two middle values.

## The `collections` Module

The **`collections`** module provides extra data structures. One of the most useful is **`Counter`**, which counts how many times each item appears.

```python
from collections import Counter

subjects = ["Maths", "Physics", "Maths", "Chemistry", "Maths", "Physics"]

count = Counter(subjects)
print(count)
print(count["Maths"])
```

**Output:**

```
Counter({'Maths': 3, 'Physics': 2, 'Chemistry': 1})
3
```

- `Counter(subjects)` counts each subject in the list.
- `count["Maths"]` gives the number of times `"Maths"` appears.

## Finding More Modules

The standard library contains many more modules. Before writing code for a common task, check whether the standard library already has a module for it. The official documentation lists every module and explains how to use it.

## Points to Remember

- **The standard library comes with Python**, so its modules do not need to be installed.
- **Standard library modules are imported** like any other module, using `import`.
- **Check the standard library first** before writing code for a common task.

## Further Reading

- 📎 **Official Python Standard Library reference** — https://docs.python.org/3/library/index.html
- 📎 **Official Python tutorial: a brief tour of the standard library** — https://docs.python.org/3/tutorial/stdlib.html

One of the most widely used standard library modules is `datetime`, which works with dates and times. In the next topic, **Dates and Times**, you'll learn how to use it.
