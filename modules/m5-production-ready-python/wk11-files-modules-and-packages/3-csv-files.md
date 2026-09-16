# CSV Files

**CSV** stands for comma-separated values. It is a plain text format for tables: one record per line, fields separated by commas, usually with a header row naming the columns.

```
name,course,mark
Anita,Computer Science,78
Ravi,Mathematics,91
```

Every spreadsheet and database can export it and read it, which is why data arrives this way so often. It is a text file, so everything from the previous chapters applies.

## Why Not Just split(",")

The obvious approach works until it does not:

```python
line = "Anita,Computer Science,78"
print(line.split(","))
```

**Output:**

```
['Anita', 'Computer Science', '78']
```

Now a name containing a comma:

```python
line = "Sharma, Anita,Computer Science,78"
print(line.split(","))
```

**Output:**

```
['Sharma', ' Anita', 'Computer Science', '78']
```

Four fields instead of three, and the record is wrong. CSV handles this by quoting fields that contain commas, and a quote inside a quoted field is doubled. Parsing that correctly by hand means tracking whether you are inside quotes, character by character.

The `csv` module already does it. Use it.

## Reading

`csv.reader` wraps a file object and yields each row as a list of strings:

```python
import csv

with open("marks.csv", newline="", encoding="utf-8") as handle:
    reader = csv.reader(handle)
    for row in reader:
        print(row)
```

**Output:**

```
['name', 'course', 'mark']
['Anita', 'Computer Science', '78']
['Ravi', 'Mathematics', '91']
```

Two details in the `open()` call matter.

**`newline=""` is required.** The csv module handles line endings itself, and letting Python translate them as well produces blank rows between records on Windows. Pass it on every `open()` used with `csv`, for reading and writing.

**Every field is a string.** `'78'`, not `78`. A CSV file carries no type information, so conversion is the caller's job.

## Skipping the Header

The header is just the first row, and `next()` takes one row from the reader:

```python
import csv

with open("marks.csv", newline="", encoding="utf-8") as handle:
    reader = csv.reader(handle)
    header = next(reader)
    print("Columns:", header)
    for row in reader:
        print(row[0], int(row[2]))
```

**Output:**

```
Columns: ['name', 'course', 'mark']
Anita 78
Ravi 91
```

The `for` loop continued from row two, because `next()` had already consumed row one. A reader is an iterator, so it produces each row once.

Totalling a column is the accumulator pattern with a conversion:

```python
import csv

total = 0
count = 0

with open("marks.csv", newline="", encoding="utf-8") as handle:
    reader = csv.reader(handle)
    next(reader)
    for row in reader:
        total = total + int(row[2])
        count = count + 1

print(f"{count} students, average {total / count:.1f}")
```

**Output:**

```
2 students, average 84.5
```

## Writing

`csv.writer` has `writerow()` for one row and `writerows()` for many:

```python
import csv

rows = [
    ["name", "course", "mark"],
    ["Anita", "Computer Science", 78],
    ["Ravi", "Mathematics", 91],
]

with open("marks.csv", "w", newline="", encoding="utf-8") as handle:
    writer = csv.writer(handle)
    writer.writerows(rows)

with open("marks.csv", encoding="utf-8") as handle:
    print(handle.read())
```

**Output:**

```
name,course,mark
Anita,Computer Science,78
Ravi,Mathematics,91

```

Note that the numbers were written without conversion. `csv` converts values to strings on the way out — unlike `write()`, which refuses anything but a string.

Quoting is handled automatically. A field containing a comma or a quote is quoted, and inner quotes are doubled:

```python
import csv

with open("tricky.csv", "w", newline="", encoding="utf-8") as handle:
    csv.writer(handle).writerow(["Sharma, Anita", 'He said "hi"'])

print(repr(open("tricky.csv", encoding="utf-8").read()))

with open("tricky.csv", newline="", encoding="utf-8") as handle:
    print(next(csv.reader(handle)))
```

**Output:**

```
'"Sharma, Anita","He said ""hi"""\n'
['Sharma, Anita', 'He said "hi"']
```

The file on disk is properly escaped, and reading it back gives the original values exactly. That round trip is the whole reason to use the module.

## DictReader

Indexing rows by number works and reads badly. `row[2]` says nothing; `row["mark"]` says everything.

`csv.DictReader` uses the header row as keys and yields a dictionary per record:

```python
import csv

with open("marks.csv", newline="", encoding="utf-8") as handle:
    reader = csv.DictReader(handle)
    for row in reader:
        print(row)
```

**Output:**

```
{'name': 'Anita', 'course': 'Computer Science', 'mark': '78'}
{'name': 'Ravi', 'course': 'Mathematics', 'mark': '91'}
```

The header is consumed automatically — no `next()` — and the records become the list of dictionaries that so much other code expects:

```python
import csv

with open("marks.csv", newline="", encoding="utf-8") as handle:
    students = list(csv.DictReader(handle))

for student in students:
    print(f"{student['name']:<10}{int(student['mark']):>5}")

print(f"{'Average':<10}{sum(int(s['mark']) for s in students) / len(students):>5.1f}")
```

**Output:**

```
Anita        78
Ravi         91
Average    84.5
```

Prefer `DictReader` unless the file has no header. Column order can change without breaking the code, and every access says which column it means.

## DictWriter

The counterpart takes the field names up front and writes dictionaries:

```python
import csv

students = [
    {"name": "Anita", "mark": 78},
    {"name": "Ravi", "mark": 91},
]

with open("out.csv", "w", newline="", encoding="utf-8") as handle:
    writer = csv.DictWriter(handle, fieldnames=["name", "mark"])
    writer.writeheader()
    writer.writerows(students)

with open("out.csv", encoding="utf-8") as handle:
    print(handle.read())
```

**Output:**

```
name,mark
Anita,78
Ravi,91

```

`fieldnames` fixes the column order, and `writeheader()` writes that row — it is easy to forget and produces a file with no header.

The keys must match `fieldnames`. An extra key raises `ValueError`; a missing one writes an empty field, or raises if `restval` says so.

## Other Delimiters

Despite the name, the separator is configurable. Tab-separated files are common:

```python
import csv

with open("marks.tsv", "w", newline="", encoding="utf-8") as handle:
    writer = csv.writer(handle, delimiter="\t")
    writer.writerow(["name", "mark"])
    writer.writerow(["Anita", 78])

with open("marks.tsv", newline="", encoding="utf-8") as handle:
    for row in csv.reader(handle, delimiter="\t"):
        print(row)
```

**Output:**

```
['name', 'mark']
['Anita', '78']
```

The `delimiter` must be given to both the reader and the writer. Semicolons are also widespread, in regions where the comma is the decimal separator.

## What CSV Cannot Do

CSV holds a flat table of text. It has no types, no nesting, and no agreed standard for much beyond the basics.

That means every number needs converting on the way in, a record with a list inside it has nowhere to go, and files from different sources disagree about quoting, encoding and line endings.

When the data is a flat table, CSV is the right choice — universally supported and readable in any editor. When it has structure, the next chapter's format is.

## Further Reading

- **Official reference for the `csv` module** — https://docs.python.org/3/library/csv.html
- **Reading and writing CSV in Python** — https://realpython.com/python-csv/

`csv.reader` and `csv.writer` handle quoting correctly, `newline=""` avoids blank rows, and `DictReader` turns a file into the list of dictionaries the rest of a program wants. Next, a format that keeps types and structure.
