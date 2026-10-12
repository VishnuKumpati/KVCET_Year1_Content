# Data Wrangling

Data arriving from a file, a form or another program is rarely in the shape a program needs. Names carry stray spaces, capitals are inconsistent, numbers arrive as text, records repeat, and some values are missing altogether.

> **Data wrangling is the work of cleaning raw data and reshaping it into a form a program can use.** It combines the string methods, lists, dictionaries and comprehensions already covered, applied in a particular order.

Here is a typical set of records, each one a line of text.

```python
raw = ["  asha , 87 ", "RAVI,30", "meera , 95 "]
```

Across these lines, the problems include extra spaces around some values, inconsistent capitals, and scores stored as text rather than numbers.

## Splitting a Line into Parts

The `split()` method breaks a line at a chosen separator and returns the pieces as a list.

```python
line = "  asha , 87 "
parts = line.split(",")
print(parts)
```

**Output:**

```
['  asha ', ' 87 ']
```

The line is now two pieces, and both still carry their spaces. `split(",")` separates at the comma and leaves everything else untouched.

## Cleaning Each Part

Each piece needs different treatment. The name needs its spaces removed and its capitals fixed. The score needs its surrounding whitespace removed and then must be converted to a number.

```python
line = "  asha , 87 "
parts = line.split(",")

name = parts[0].strip().title()
score = int(parts[1].strip())

print(name, score)
print(type(score))
```

**Output:**

```
Asha 87
<class 'int'>
```

**Explanation:**

- `parts[0]` is `"  asha "`. `strip()` removes the whitespace at both ends, giving `"asha"`.
- `title()` then capitalises it to give `"Asha"`.
- `parts[1]` is `" 87 "`. `strip()` removes the whitespace at both ends, giving `"87"`.
- `int()` converts that text into the number `87`.

`int()` converts the score from text into a number. Without it, `score` would remain text, and `score + 5` would raise a `TypeError` because Python cannot add a string and an integer.

## Building Records from Raw Lines

A loop applies the same cleaning to every line and collects the results as a list of dictionaries.

```python
raw = ["  asha , 87 ", "RAVI,30", "meera , 95 "]

students = []
for line in raw:
    parts = line.split(",")
    students.append({
        "name": parts[0].strip().title(),
        "score": int(parts[1].strip())
    })

for student in students:
    print(student)
```

**Output:**

```
{'name': 'Asha', 'score': 87}
{'name': 'Ravi', 'score': 30}
{'name': 'Meera', 'score': 95}
```

The three messy lines are now three clean records. `RAVI` became `Ravi`, the extra spaces around the values were removed, and each score is a number.

## Handling Values That Cannot Be Converted

Real data contains values that do not convert. A blank entry or a word such as `absent` makes `int()` raise a `ValueError`.

Wrapping the conversion in `try` and `except` lets the program skip those values and continue.

```python
raw = ["87", "absent", "95", "", "72"]

scores = []
for value in raw:
    try:
        scores.append(int(value))
    except ValueError:
        pass

print(scores)
```

**Output:**

```
[87, 95, 72]
```

`absent` and the empty string both raise a `ValueError`, so `except` catches them and the loop moves on. The three valid scores are collected.

`pass` means do nothing. It is used here because the invalid values simply need skipping, with no action taken.

This example silently skips invalid values. In real data-cleaning tasks, you may need to record or inspect invalid entries instead of discarding them, especially when the missing data matters.

## Removing Duplicate Values

The same value can appear more than once in raw data. A set keeps only one copy of each distinct value, such as a name or number.

```python
names = ["Asha", "Ravi", "Asha", "Meera"]

print(len(names))
print(len(set(names)))
print(sorted(set(names)))
```

**Output:**

```
4
3
['Asha', 'Meera', 'Ravi']
```

The list contains four names, but only three are unique. A set removes the duplicate, and `sorted()` returns the unique names in alphabetical order because a set does not guarantee their order.

This example removes duplicate names, not complete records. To remove duplicate records, you first need a rule for deciding when two records count as duplicates, such as having the same ID.

## Calculating Totals and Averages

Once the records are clean, the numbers can be used.

```python
students = [
    {"name": "Asha", "score": 87},
    {"name": "Ravi", "score": 30},
    {"name": "Meera", "score": 95}
]

total = sum(student["score"] for student in students)
print("Total:", total)
print(f"Average: {total / len(students):.2f}")
```

**Output:**

```
Total: 212
Average: 70.67
```

The `:.2f` part displays the average to two decimal places.

The generator expression inside `sum()` provides one score at a time, so no separate list of scores is created.

`max()` finds the highest record when given a `key` function.

```python
students = [
    {"name": "Asha", "score": 87},
    {"name": "Ravi", "score": 30},
    {"name": "Meera", "score": 95}
]

top = max(students, key=lambda student: student["score"])
print(top["name"])
```

**Output:**

```
Meera
```

`max()` compares the scores returned by the lambda, and gives back the whole dictionary that scored highest.

## Counting How Often Values Appear

A dictionary counts occurrences, with `get()` supplying a starting value of zero for a value seen for the first time.

```python
answers = ["tamil", "hindi", "tamil", "telugu", "tamil"]

counts = {}
for answer in answers:
    counts[answer] = counts.get(answer, 0) + 1

print(counts)
```

**Output:**

```
{'tamil': 3, 'hindi': 1, 'telugu': 1}
```

**Explanation:**

- `counts.get(answer, 0)` returns the current count, or `0` when the answer is not yet a key
- adding `1` gives the new count
- assigning it back stores the result under that key

If we tried to read `counts[answer]` before that key existed, Python would raise a `KeyError`. `get(answer, 0)` avoids this by returning `0` when the key is missing.

## Grouping Records Together

Records can be grouped by a value they share, such as class. Each group is stored as a list under its own dictionary key.

```python
students = [
    {"name": "Asha", "class": 7},
    {"name": "Ravi", "class": 8},
    {"name": "Meera", "class": 7}
]

groups = {}
for student in students:
    key = student["class"]
    if key not in groups:
        groups[key] = []
    groups[key].append(student["name"])

print(groups)
```

**Output:**

```
{7: ['Asha', 'Meera'], 8: ['Ravi']}
```

The `if` creates an empty list the first time each class is seen. Every name is then appended to the list for its class.

## The Order of the Steps

A common data-wrangling workflow follows these steps.

1. **Split** the raw text into its parts.
2. **Clean** each part with `strip()`, `lower()` or `title()`.
3. **Convert** text to numbers with `int()` or `float()`.
4. **Handle missing or invalid values** by correcting, recording or skipping them, depending on the task.
5. **Remove duplicates** when needed. Sets work for simple values such as strings and numbers; duplicate records may require a separate rule.
6. **Reshape** the parts into dictionaries or lists.
7. **Summarise** with totals, counts or groups.

Cleaning text before conversion can make values consistent. However, `int()` accepts surrounding whitespace, so `int("87\n")` returns `87`. Using `strip()` is still useful for removing unwanted whitespace before further processing.

## Further Reading

- 📎 **String methods with worked examples** — https://www.programiz.com/python-programming/methods/string
- 📎 **Official Python guide to data structures** — https://docs.python.org/3/tutorial/datastructures.html

That completes comprehensions, generators and data wrangling. You can now transform collections in a single line, produce values on demand, and clean raw data into a shape a program can use.

Next week you will learn how to build your own data types, using classes and objects.
