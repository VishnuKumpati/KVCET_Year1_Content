# CSV Files

Programs often work with data arranged in rows and columns, such as the names and marks of many students. Saving this data as plain lines of text makes it hard to tell where one value ends and the next begins. **CSV files** store table-like data in a simple, standard format that both programs and spreadsheet applications, such as Microsoft Excel, can open.

> **A CSV file is a text file that stores data in rows, usually separating values with commas.**

**CSV** stands for **Comma-Separated Values**.

## What a CSV File Looks Like

A CSV file containing student marks looks like this:

```
Name,Marks
Ravi,85
Priya,92
```

- Each **line** is one row.
- **Commas** separate the values in a row.
- The first row usually holds the **column names**, called the **header**.

## The `csv` Module

Python's built-in **`csv` module** handles commas and other special characters inside values, so you do not have to split or join CSV data manually. The line `import csv` makes the module available. Importing is covered in **Modules and Imports**.

## Writing to a CSV File

`csv.writer()` creates a **writer** object. Its `writerow()` method writes one row, given as a list.

```python
import csv

with open("students.csv", "w", newline="", encoding="utf-8") as file:
    writer = csv.writer(file)
    writer.writerow(["Name", "Marks"])
    writer.writerow(["Ravi", 85])
    writer.writerow(["Priya", 92])

with open("students.csv", "r", encoding="utf-8") as file:
    print(file.read().strip())
```

**Output:**

```
Name,Marks
Ravi,85
Priya,92
```

- Each `writerow()` call writes one row. The writer adds the commas automatically.
- **`newline=""`** should be used when opening a file for the `csv` module. It lets the module handle line endings correctly and prevents extra blank lines when writing on some systems.
- The second block prints the file's content to show what was written.

### Writing Many Rows at Once

`writerows()` writes **several rows** from a list of lists.

```python
import csv

rows = [
    ["Name", "Marks"],
    ["Ravi", 85],
    ["Priya", 92],
    ["Arun", 78],
]

with open("students.csv", "w", newline="", encoding="utf-8") as file:
    writer = csv.writer(file)
    writer.writerows(rows)

with open("students.csv", "r", encoding="utf-8") as file:
    print(file.read().strip())
```

**Output:**

```
Name,Marks
Ravi,85
Priya,92
Arun,78
```

Each inner list in `rows` becomes one row in the file.

## Reading a CSV File

`csv.reader()` creates a **reader** object. Looping over it gives **one row at a time, as a list of strings**.

The reading examples in this topic use the `students.csv` file created in **Writing Many Rows at Once**. Run that example first.

```python
import csv

with open("students.csv", "r", newline="", encoding="utf-8") as file:
    reader = csv.reader(file)
    for row in reader:
        print(row)
```

**Output:**

```
['Name', 'Marks']
['Ravi', '85']
['Priya', '92']
['Arun', '78']
```

Notice that the marks appear as `'85'`, `'92'` and `'78'`. **The `csv` module reads every value as a string**, even if it looks like a number.

## Reading Rows by Column Name

`csv.DictReader()` reads each row as a **dictionary**. It uses the header row as the keys, so each value can be accessed by its column name.

```python
import csv

with open("students.csv", "r", newline="", encoding="utf-8") as file:
    reader = csv.DictReader(file)
    for row in reader:
        print(row["Name"], "scored", row["Marks"])
```

**Output:**

```
Ravi scored 85
Priya scored 92
Arun scored 78
```

- `DictReader` uses the header row, `Name,Marks`, as keys. The header itself is not returned as a data row.
- `row["Name"]` and `row["Marks"]` access values by column name, making the code easier to read.

## Working with Numbers from a CSV File

Because every value is read as a string, convert numbers before doing calculations.

```python
import csv

total = 0

with open("students.csv", "r", newline="", encoding="utf-8") as file:
    reader = csv.DictReader(file)
    for row in reader:
        total = total + int(row["Marks"])

print("Total marks:", total)
```

**Output:**

```
Total marks: 255
```

`int(row["Marks"])` converts each student's marks from a string to an integer, allowing Python to add them and calculate the total.

## Points to Remember

- **Use `newline=""`** when opening a file for the `csv` module.
- **`csv.writer()`** writes rows using `writerow()` and `writerows()`.
- **`csv.reader()`** gives each row as a list, while **`csv.DictReader()`** gives each row as a dictionary.
- **Values are read as strings.** Convert them with `int()` or `float()` before calculations.

## Further Reading

- 📎 **Official Python reference for the `csv` module** — https://docs.python.org/3/library/csv.html

CSV files work well for simple tables. Data with more structure, such as a student with a list of subjects, needs a different format. In the next topic, **JSON Files**, you'll learn how to store this kind of data.
