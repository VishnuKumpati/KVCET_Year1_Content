# JSON Files

Some data has more structure than a simple table. One record may contain a list of values, or even other records inside it. A CSV file cannot store this easily. **JSON** is a format that stores structured data as text, so it can be saved in a file and read back later. It is also one of the most common formats for sharing data between programs and websites.

> **JSON is a text format for storing and exchanging data. It supports objects containing name-value pairs, as well as arrays and simple values. JSON stands for JavaScript Object Notation.**

Although the name includes JavaScript, JSON is used by almost every programming language, including Python.

## What JSON Looks Like

JSON data looks very similar to a Python dictionary:

```
{
    "name": "Ravi",
    "marks": 85,
    "subjects": ["Maths", "Physics"],
    "passed": true
}
```

- Data is written as **name and value pairs**, such as `"name": "Ravi"`.
- Names and text values must be written inside **double quotes**. Single quotes are not valid in JSON.
- A value can be text, a number, a list, `true`, `false` or `null`.

The Python data types used in this lesson map to JSON types as follows:

| Python | JSON |
| --- | --- |
| `dict` | Object, written with `{ }` |
| `list` | Array, written with `[ ]` |
| `str` | String |
| `int`, `float` | Number |
| `True`, `False` | `true`, `false` |
| `None` | `null` |

## The `json` Module

Python's built-in **`json` module** converts Python data to JSON and back. The line `import json` makes the module available. Importing is covered in **Modules and Imports**.

## Writing JSON to a File

`json.dump()` writes Python data into a file in JSON format.

```python
import json

student = {
    "name": "Ravi",
    "marks": 85,
    "subjects": ["Maths", "Physics"],
    "passed": True
}

with open("student.json", "w", encoding="utf-8") as file:
    json.dump(student, file, indent=4)

with open("student.json", "r", encoding="utf-8") as file:
    print(file.read())
```

**Output:**

```
{
    "name": "Ravi",
    "marks": 85,
    "subjects": [
        "Maths",
        "Physics"
    ],
    "passed": true
}
```

- `json.dump(student, file, indent=4)` writes the dictionary `student` into `student.json`.
- **`indent=4`** adds spaces and new lines so the file is easy to read. Without it, all the data is written on one line.
- Python's `True` is written as JSON's `true`.

## Reading JSON from a File

`json.load()` reads JSON data from a file and converts it back into Python data. This example reads the `student.json` file created in **Writing JSON to a File**, so run that example first.

```python
import json

with open("student.json", "r", encoding="utf-8") as file:
    data = json.load(file)

print(data["name"])
print(data["subjects"])
print(type(data))
```

**Output:**

```
Ravi
['Maths', 'Physics']
<class 'dict'>
```

- `json.load(file)` converts the JSON data into a Python value. In this example, it returns a **dictionary**, because the file contains a JSON object.
- Values can then be used like any dictionary values, such as `data["name"]`.
- Unlike the `csv` module, **JSON keeps numbers as numbers**. `data["marks"]` is the number `85`, not the string `"85"`.

## Accessing Nested JSON Data

JSON data can contain objects and arrays **inside** other objects or arrays. This is called **nested** data, and it allows related information to be grouped together in a flexible structure.

```python
import json

student = {
    "name": "Ravi",
    "marks": {
        "Maths": 85,
        "Physics": 90
    },
    "subjects": ["Maths", "Physics"]
}

with open("student_marks.json", "w", encoding="utf-8") as file:
    json.dump(student, file, indent=4)

with open("student_marks.json", "r", encoding="utf-8") as file:
    data = json.load(file)

print(data["marks"]["Maths"])
print(data["subjects"][1])
```

**Output:**

```
85
Physics
```

- `data["marks"]` gives the inner dictionary. Adding `["Maths"]` gives the value inside it, `85`.
- `data["subjects"]` gives the list. Adding `[1]` gives its second item, `"Physics"`.

**Use one set of square brackets for each level** you move inside the data.

## Updating and Saving JSON Data

To change JSON data in a file, read it, change the Python data, and write it back. This example uses the `student.json` file created in **Writing JSON to a File**.

```python
import json

with open("student.json", "r", encoding="utf-8") as file:
    data = json.load(file)

data["marks"] = 90
if "Chemistry" not in data["subjects"]:
    data["subjects"].append("Chemistry")

with open("student.json", "w", encoding="utf-8") as file:
    json.dump(data, file, indent=4)

with open("student.json", "r", encoding="utf-8") as file:
    updated = json.load(file)

print(updated["marks"])
print(updated["subjects"])
```

**Output:**

```
90
['Maths', 'Physics', 'Chemistry']
```

1. `json.load()` reads the file into the dictionary `data`.
2. The marks are changed to `90`. The `if` condition checks whether `"Chemistry"` is already in the list, and adds it only if it is missing. This prevents duplicates if the example is run more than once.
3. `json.dump()` writes the updated dictionary back into the file. Mode `"w"` replaces the old content.
4. Reading the file again confirms that the changes were saved.

## Converting Between JSON and Strings

Sometimes JSON data is needed as a **string** instead of a file, for example when sending data to a website. Two functions work with strings:

| Function | What It Does |
| --- | --- |
| `json.dumps()` | Converts Python data into a JSON string |
| `json.loads()` | Converts a JSON string into Python data |

The `s` at the end of each name stands for **string**.

```python
import json

student = {"name": "Ravi", "marks": 85}

text = json.dumps(student)

print(text)
print(type(text))

data = json.loads(text)

print(data)
print(type(data))
print(data["marks"])
```

**Output:**

```
{"name": "Ravi", "marks": 85}
<class 'str'>
{'name': 'Ravi', 'marks': 85}
<class 'dict'>
85
```

Both values represent the same information, but their types are different:

- `text` is a **string** (`str`) containing JSON text.
- `data` is a Python **dictionary** (`dict`) that you can **access and modify directly**, such as `data["marks"]`.

`json.dumps()` converts Python data into JSON text. `json.loads()` converts JSON text back into Python data.

## Handling Invalid JSON

If JSON data does not follow the JSON rules, Python cannot read it and raises a **`json.JSONDecodeError`**. Common mistakes are a comma after the last value, or single quotes instead of double quotes.

```python
import json

text = '{"name": "Ravi", "marks": 85,}'

try:
    data = json.loads(text)
except json.JSONDecodeError:
    print("The JSON data is not valid.")
```

**Output:**

```
The JSON data is not valid.
```

The comma after `85` makes the JSON invalid. The `except json.JSONDecodeError` block catches the error and prints a clear message. The same error is raised by `json.load()` when a JSON **file** contains invalid data.

## Points to Remember

- **`json.dump()` and `json.load()`** work with files.
- **`json.dumps()` and `json.loads()`** work with strings.
- **Use `indent=4`** to make JSON files easy to read.
- **JSON keeps numbers as numbers**, so no conversion is needed after reading.
- **Use one set of square brackets for each level** of nested data.
- **Invalid JSON raises a `json.JSONDecodeError`**, which can be handled with `try` and `except`.

## Further Reading

- 📎 **Official Python reference for the `json` module** — https://docs.python.org/3/library/json.html

You have now used Python's `csv` and `json` modules to work with different file formats. In the next topic, **Modules and Imports**, you'll learn how modules help organise and reuse Python code.
