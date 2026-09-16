# Nested Data Structures

Lists, tuples, dictionaries and sets all hold values, and a value can be any of those four. Nothing more has to be learned to combine them; the combinations follow from what each type already does.

Real data is almost always a combination. A single record has named fields, which is a dictionary. Many records are a collection of those, which is a list. Put the two together and you have the shape that data arrives in from a file, a database or an API.

## A List of Dictionaries

Each dictionary is one record, and the list holds them in order:

```python
students = [
    {"name": "Anita", "age": 21, "marks": [78, 91, 64]},
    {"name": "Ravi", "age": 22, "marks": [88, 72, 95]},
    {"name": "Meera", "age": 20, "marks": [60, 85, 70]},
]
print(len(students))
```

**Output:**

```
3
```

Three items in the list, so three students. Each of those items is a dictionary with three keys, and one of those keys holds a list.

This is the single most useful structure in everyday programming. Every row of a spreadsheet, every record from a query, every item in a JSON response has this shape.

## Reading One Bracket at a Time

Nested access looks intimidating written out in full and is simple read one step at a time. Each bracket peels off one layer:

```python
print(students[0])
```

**Output:**

```
{'name': 'Anita', 'age': 21, 'marks': [78, 91, 64]}
```

`students[0]` is a dictionary. So a key can follow:

```python
print(students[0]["name"])
print(students[0]["marks"])
```

**Output:**

```
Anita
[78, 91, 64]
```

`students[0]["marks"]` is a list. So an index can follow:

```python
print(students[0]["marks"][1])
```

**Output:**

```
91
```

Read it left to right, saying what each step gives:

```
students                  →  a list of records
students[0]               →  one record, a dictionary
students[0]["marks"]      →  a list of numbers
students[0]["marks"][1]   →  one number
```

The bracket to use is decided by what the previous step produced: a number for a list, a key for a dictionary. Nothing else is involved.

Whole-structure functions work at any level, because each level is an ordinary value of its type:

```python
print(len(students[1]["marks"]))
print(max(students[1]["marks"]))
print(sum(students[1]["marks"]) / 3)
```

**Output:**

```
3
95
85.0
```

## Changing a Value Deep Inside

Assignment works the same way. Reach the thing you want and assign to it:

```python
students[1]["age"] = 23
print(students[1])
```

**Output:**

```
{'name': 'Ravi', 'age': 23, 'marks': [88, 72, 95]}
```

A list deep inside is a real list, so its methods apply:

```python
students[0]["marks"].append(88)
print(students[0]["marks"])
```

**Output:**

```
[78, 91, 64, 88]
```

No copying happened at any point. `students[0]["marks"]` is the actual inner list, not a view of it, and `append()` changed the structure in place.

## Looping over a List of Dictionaries

The loop variable holds one record per iteration, and the record's keys name its fields:

```python
students = [
    {"name": "Anita", "age": 21, "marks": [78, 91, 64]},
    {"name": "Ravi", "age": 22, "marks": [88, 72, 95]},
    {"name": "Meera", "age": 20, "marks": [60, 85, 70]},
]

for student in students:
    average = sum(student["marks"]) / len(student["marks"])
    print(f"{student['name']:<10}{average:>8.1f}")
```

**Output:**

```
Anita         77.7
Ravi          85.0
Meera         71.7
```

Note the single quotes around `'name'` inside the double-quoted f-string. The quotes inside the braces must differ from the quotes around the string.

Filtering is an `if` inside the loop:

```python
for student in students:
    if student["age"] >= 21:
        print(student["name"])
```

**Output:**

```
Anita
Ravi
```

And collecting is the usual empty list and `append()`:

```python
names = []
for student in students:
    if max(student["marks"]) >= 90:
        names.append(student["name"])
print(names)
```

**Output:**

```
['Anita', 'Ravi']
```

Filter, transform and collect are the three things done to a list of records, and all three are this loop with a different body.

## A Dictionary of Lists

Turn the nesting around and the dictionary is outermost. Each key names a group, and each value is the list of things in it:

```python
courses = {
    "Computer Science": ["Anita", "Meera"],
    "Mathematics": ["Ravi"],
}
print(courses["Computer Science"])
print(courses["Computer Science"][0])
print(len(courses["Mathematics"]))
```

**Output:**

```
['Anita', 'Meera']
Anita
1
```

Adding to an existing group appends to its list:

```python
courses["Mathematics"].append("Sunil")
print(courses["Mathematics"])
```

**Output:**

```
['Ravi', 'Sunil']
```

Adding to a group that does not exist yet needs the list to be created first, which is what `setdefault()` is for:

```python
courses.setdefault("Physics", []).append("Anita")
print(courses["Physics"])
```

**Output:**

```
['Anita']
```

Looping gives the group name and its list together:

```python
for course, names in courses.items():
    print(f"{course}: {', '.join(names)} ({len(names)})")
```

**Output:**

```
Computer Science: Anita, Meera (2)
Mathematics: Ravi, Sunil (2)
Physics: Anita (1)
```

This is the shape that grouping produces: a list of records goes in, and a dictionary of lists comes out, keyed by whatever the records were grouped by.

## A Dictionary of Dictionaries

When each record has a natural unique key, the outer structure can be a dictionary too:

```python
directory = {
    "anita": {"age": 21, "course": "Computer Science"},
    "ravi": {"age": 22, "course": "Mathematics"},
}
print(directory["anita"]["course"])
```

**Output:**

```
Computer Science
```

Compared with a list of dictionaries, this trades the ability to keep records in a chosen order for a direct lookup by key. Finding Anita in a list means looping until her record turns up; here it is one operation.

Looping needs `items()`, since the key is no longer stored inside the record:

```python
for username, details in directory.items():
    print(f"{username}: {details['age']}, {details['course']}")
```

**Output:**

```
anita: 21, Computer Science
ravi: 22, Mathematics
```

## Missing Keys at Depth

`KeyError` and `IndexError` apply at every level, and a long chain of brackets gives more places to fail:

```python
students = [{"name": "Anita", "age": 21}]
print(students[0]["grade"])
```

**Output:**

```
KeyError: 'grade'
```

The error names the key that was missing, which tells you which level went wrong. `get()` with a default handles a field that is legitimately optional:

```python
students = [{"name": "Anita", "age": 21}]
print(students[0].get("grade", "not graded"))
```

**Output:**

```
not graded
```

Be careful chaining `get()` calls, though. When the default is returned, the next step in the chain is applied to *that*:

```python
record = {"name": "Anita"}
print(record.get("marks", []))
print(len(record.get("marks", [])))
```

**Output:**

```
[]
0
```

Choosing an empty list as the default is what makes this work. The code that follows can loop over it, take its length or sum it, and a missing field behaves like an empty one rather than crashing.

## Building a Nested Structure

Nothing new is required. The empty structure is created, and the loop fills it.

Turning parallel lists into records:

```python
names = ["Anita", "Ravi", "Meera"]
ages = [21, 22, 20]

students = []
for name, age in zip(names, ages):
    students.append({"name": name, "age": age})

print(students)
```

**Output:**

```
[{'name': 'Anita', 'age': 21}, {'name': 'Ravi', 'age': 22}, {'name': 'Meera', 'age': 20}]
```

Grouping records into a dictionary of lists:

```python
students = [
    {"name": "Anita", "course": "Computer Science"},
    {"name": "Ravi", "course": "Mathematics"},
    {"name": "Meera", "course": "Computer Science"},
]

by_course = {}
for student in students:
    by_course.setdefault(student["course"], []).append(student["name"])

print(by_course)
```

**Output:**

```
{'Computer Science': ['Anita', 'Meera'], 'Mathematics': ['Ravi']}
```

Building a dictionary of dictionaries keyed by a field:

```python
students = [
    {"name": "Anita", "age": 21},
    {"name": "Ravi", "age": 22},
]

by_name = {}
for student in students:
    by_name[student["name"]] = student

print(by_name["Ravi"])
```

**Output:**

```
{'name': 'Ravi', 'age': 22}
```

Note that the records were not copied. `by_name["Ravi"]` and the second item of `students` are the same dictionary, so changing one shows up in the other. That is usually what you want from an index built over existing records, and it is worth knowing rather than discovering.

## Reading an Unfamiliar Structure

Data from a file or an API arrives in a shape you did not design. The way to understand it is to work from the outside in, one level at a time, asking what type each level is and what it contains.

```python
data = {
    "status": "ok",
    "results": [
        {"id": 1, "tags": ["python", "beginner"]},
        {"id": 2, "tags": ["python"]},
    ],
}

print(type(data))
print(list(data.keys()))
```

**Output:**

```
<class 'dict'>
['status', 'results']
```

A dictionary with two keys. `"results"` sounds like the data:

```python
print(type(data["results"]))
print(len(data["results"]))
```

**Output:**

```
<class 'list'>
2
```

A list of two things. Look at one of them:

```python
print(type(data["results"][0]))
print(data["results"][0])
```

**Output:**

```
<class 'dict'>
{'id': 1, 'tags': ['python', 'beginner']}
```

A record with an id and a list of tags. Now the path to any value is known:

```python
print(data["results"][0]["tags"][1])
```

**Output:**

```
beginner
```

Three tools do all of this: `type()` for what a level is, `len()` for how much it holds, and `keys()` for what a dictionary's fields are called. Take one step at a time and print what you get, rather than guessing a long chain of brackets and reading the error.

Once the shape is known, the loop follows from it:

```python
for result in data["results"]:
    print(result["id"], "—", ", ".join(result["tags"]))
```

**Output:**

```
1 — python, beginner
2 — python
```

## Further Reading

- **Official Python guide to data structures** — https://docs.python.org/3/tutorial/datastructures.html
- **Working with nested data** — https://realpython.com/python-data-structures/

Four types, combined freely, describe any data you are likely to meet, and loops over them filter, transform and group it. Those loops are written the same way every time, which is why Python provides a shorter way to write them.
