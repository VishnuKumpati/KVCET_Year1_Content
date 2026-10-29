# Introduction to pandas

Much of the data programs work with is arranged as a **table**: rows of records, with the same columns in each row. Student marks, sales records, survey results and the CSV file from **Building a Small API Program** are all tables.

Python's lists and dictionaries can hold table data, but answering simple questions takes many lines of code. Finding the average mark, keeping only the students from one city, or sorting by marks each needs its own loop. **pandas** is an external library built to make this work short and simple.

> **pandas is an external Python library for storing, exploring and analysing table data. It lets you filter, sort and summarise data with short, readable code instead of writing loops.**

pandas is one of the most widely used Python libraries for data analysis.

## Installing pandas

pandas is an external library, so it must be installed in your project's virtual environment. Activate the environment, as explained in **Virtual Environments and pip**, and run:

```
python -m pip install pandas
```

Downloading needs an internet connection. pandas also installs a few libraries it depends on, such as `numpy`.

## Importing pandas

pandas is almost always imported with the short name **`pd`**:

```python
import pandas as pd
```

`as pd` gives the library a shorter name, as covered in **Modules and Imports**. `pd` is the widely used convention in pandas examples and documentation, so using it makes code easier to recognise and follow.

## The Two Main Structures: Series and DataFrame

pandas provides two primary data structures: the **Series** and the **DataFrame**.

> **A Series is a one-dimensional collection of values, where each value has an associated label called an index.**

> **A DataFrame is a two-dimensional table of rows and columns. Each column in a DataFrame is a Series.**

**One-dimensional** means the values are arranged in a single sequence. **Two-dimensional** means the values are arranged in both rows and columns.

| Data Structure | Dimensions | Labels |
| --- | --- | --- |
| **Series** | One-dimensional | An **index**, which labels each value |
| **DataFrame** | Two-dimensional | A **row index**, which labels each row, and **column labels**, which name each column |

### Creating a Series

```python
import pandas as pd

marks = pd.Series([85, 92, 78])
print(marks)
```

**Output:**

```
0    85
1    92
2    78
dtype: int64
```

- The left column, `0`, `1` and `2`, is the **index**. pandas uses these labels to identify the values. By default, it starts with `0`, similar to the positions of items in a Python list.
- The right column holds the values.
- `dtype: int64` is the Series' **dtype**, short for **data type**. `int64` means a **64-bit integer**: whole numbers, each stored using 64 bits of memory.

### Creating a DataFrame

A DataFrame is commonly created from a **dictionary**. Each key becomes a **column label**, and each list becomes the values of that column.

```python
import pandas as pd

data = {
    "name": ["Ravi", "Priya", "Arun", "Meena", "Kavya"],
    "city": ["Chennai", "Madurai", "Chennai", "Coimbatore", "Madurai"],
    "marks": [85, 92, 78, 66, 88],
}

students = pd.DataFrame(data)
print(students)
```

**Output:**

```
    name        city  marks
0   Ravi     Chennai     85
1  Priya     Madurai     92
2   Arun     Chennai     78
3  Meena  Coimbatore     66
4  Kavya     Madurai     88
```

- Each **row** is one record, here one student, and each **column** holds one attribute of the records.
- The numbers on the left, `0` to `4`, are the **row index**, which labels each row.
- `name`, `city` and `marks` are the **column labels**.
- All the lists must have the **same length**, because each row needs a value in every column.

## Reading a CSV File

Most real data is stored in files rather than typed into code. **`pd.read_csv()`** reads a CSV file and returns a DataFrame.

The example below first saves the student data as `students.csv`, using **`to_csv()`**, and then reads it back. The remaining examples in this topic read this same file, so **run this example first**.

In real projects, you will often read an existing CSV file. Here, a small sample file is created first so you can run every example without needing a separate dataset.

```python
import pandas as pd

data = {
    "name": ["Ravi", "Priya", "Arun", "Meena", "Kavya"],
    "city": ["Chennai", "Madurai", "Chennai", "Coimbatore", "Madurai"],
    "marks": [85, 92, 78, 66, 88],
}

pd.DataFrame(data).to_csv("students.csv", index=False)

students = pd.read_csv("students.csv")
print(students.head(3))
print(students.shape)
print(list(students.columns))
```

**Output:**

```
    name     city  marks
0   Ravi  Chennai     85
1  Priya  Madurai     92
2   Arun  Chennai     78
(5, 3)
['name', 'city', 'marks']
```

- `to_csv("students.csv", index=False)` saves the DataFrame as a CSV file. The argument `index=False` prevents pandas from writing the DataFrame's index as an extra column. If you save to a filename that already exists, `to_csv()` replaces its contents.
- `pd.read_csv("students.csv")` reads the file back into a DataFrame. The first row of the file, called the **header row**, becomes the column labels.
- `head(n)` returns the **first `n` rows** of the DataFrame. `head(3)` returns the first three. Without an argument, it returns the first five. It is useful for inspecting large datasets quickly.
- `shape` is an **attribute** that returns a **tuple** in the form `(number of rows, number of columns)`. Here, `(5, 3)` means 5 rows and 3 columns.
- `columns` is an attribute that returns the **column labels** as a pandas **Index** object. `list()` converts it into a regular Python list so it prints clearly.

The CSV file, `students.csv`, contains:

```
name,city,marks
Ravi,Chennai,85
Priya,Madurai,92
Arun,Chennai,78
Meena,Coimbatore,66
Kavya,Madurai,88
```

The `completed_tasks.csv` file from **Building a Small API Program** can be read in exactly the same way: `pd.read_csv("completed_tasks.csv")`.

## Selecting Columns

Selecting columns by placing their labels in square brackets is called **indexing** a DataFrame. It works like looking up a key in a dictionary.

```python
import pandas as pd

students = pd.read_csv("students.csv")

print(students["marks"])
print(students[["name", "marks"]])
```

**Output:**

```
0    85
1    92
2    78
3    66
4    88
Name: marks, dtype: int64
    name  marks
0   Ravi     85
1  Priya     92
2   Arun     78
3  Meena     66
4  Kavya     88
```

- `students["marks"]`, with **one** column label, returns that column as a **Series**. The output shows the Series' `Name`, which is the column label, and its `dtype`.
- `students[["name", "marks"]]`, with a **list** of column names, returns those columns as a new **DataFrame**. The inner brackets create the list of names, and the outer brackets select those columns.

Selecting columns answers the question **which information do I want to see?** Filtering, covered next, answers **which records do I want to keep?**

## Filtering Rows

Filtering keeps only the rows that match a condition. In pandas, this technique is called **boolean indexing**: a condition is written inside square brackets, and pandas selects the rows for which the condition is `True`.

```python
import pandas as pd

students = pd.read_csv("students.csv")

high_scorers = students[students["marks"] >= 80]
print(high_scorers)

from_chennai = students[students["city"] == "Chennai"]
print(from_chennai)
```

**Output:**

```
    name     city  marks
0   Ravi  Chennai     85
1  Priya  Madurai     92
4  Kavya  Madurai     88
   name     city  marks
0  Ravi  Chennai     85
2  Arun  Chennai     78
```

- `students["marks"] >= 80` checks the condition for **every row at once** and returns a **boolean Series**, a Series of `True` and `False` values, one for each row. This boolean Series is often called a **boolean mask**.
- Placing the boolean mask inside `students[...]` keeps the rows whose value is `True` and excludes the rows whose value is `False`.
- The rows keep their original index numbers, such as `0`, `1` and `4`. This shows which original rows were kept.

The same filtering with a list would need a loop and an `if` statement. pandas does it in one line.

## Sorting Rows

**`sort_values()`** sorts the rows of a DataFrame by the values in one or more columns. Its first parameter, `by`, names the column to sort by.

```python
import pandas as pd

students = pd.read_csv("students.csv")

print(students.sort_values("marks", ascending=False))
```

**Output:**

```
    name        city  marks
1  Priya     Madurai     92
4  Kavya     Madurai     88
0   Ravi     Chennai     85
2   Arun     Chennai     78
3  Meena  Coimbatore     66
```

- `sort_values("marks")` passes `"marks"` as the `by` argument, so the rows are sorted by the `marks` column.
- `ascending=False` sorts in **descending order**, from highest to lowest. The default, `ascending=True`, sorts in **ascending order**, from lowest to highest.
- Sorting returns a **new** DataFrame. The original `students` DataFrame is not changed.

Some pandas operations return a new result, while others can modify an existing DataFrame. In this example, `sort_values()` returns a sorted result without changing `students`.

## Calculating Summaries

pandas provides methods to calculate **descriptive statistics**, values that summarise a column, such as its mean, maximum and minimum. These methods are called on a Series.

```python
import pandas as pd

students = pd.read_csv("students.csv")

print(students["marks"].mean())
print(students["marks"].max())
print(students["marks"].min())
print(students["city"].value_counts())
```

**Output:**

```
81.8
92
66
city
Chennai       2
Madurai       2
Coimbatore    1
Name: count, dtype: int64
```

| Method | What It Returns | Result Here |
| --- | --- | --- |
| `mean()` | The **mean**, the average of the values | `81.8` |
| `max()` | The highest value | `92` |
| `min()` | The lowest value | `66` |
| `value_counts()` | A Series containing the count of each **unique value**, in descending order of count | 2 from Chennai, 2 from Madurai, 1 from Coimbatore |

So far, you have selected, filtered, sorted and summarised existing data. You can also use a column's values to calculate a new result for every row, and store that result as another column.

## Adding a Column and Saving the Result

A new column is created by **assigning** values to a new column label, written in square brackets. Here, a `passed` column stores whether each student scored 70 or more.

```python
import pandas as pd

students = pd.read_csv("students.csv")

students["passed"] = students["marks"] >= 70
print(students)

students.to_csv("students_result.csv", index=False)
```

**Output:**

```
    name        city  marks  passed
0   Ravi     Chennai     85    True
1  Priya     Madurai     92    True
2   Arun     Chennai     78    True
3  Meena  Coimbatore     66   False
4  Kavya     Madurai     88    True
```

- `students["marks"] >= 70` returns a **boolean Series**, with `True` or `False` for every row.
- `students["passed"] = ...` assigns that boolean Series to a new column with the label `passed`.
- `to_csv()` saves the updated table to a new file, `students_result.csv`.

## Analysing API Data with pandas

The same pandas operations work with data from an API, not just CSV files. In this example, `requests`, from **Calling an API with requests**, fetches the tasks as JSON, and pandas converts the list of task dictionaries into a DataFrame. Activate the project's virtual environment, make sure `requests` and `pandas` are installed, and check that an internet connection is available.

```python
import pandas as pd
import requests

response = requests.get("https://jsonplaceholder.typicode.com/todos", params={"userId": 1}, timeout=10)
response.raise_for_status()

tasks = pd.DataFrame(response.json())

print(tasks.head(3))
print(tasks["completed"].value_counts())
```

**Output:**

```
   userId  id                               title  completed
0       1   1                  delectus aut autem      False
1       1   2  quis ut nam facilis et officia qui      False
2       1   3                 fugiat veniam minus      False
completed
True     11
False     9
Name: count, dtype: int64
```

- `response.json()` returns a **list of dictionaries**, one for each task.
- `pd.DataFrame()` turns that list into a table. Each dictionary becomes a **row**, and each key becomes a **column**.
- Each task dictionary contains keys such as `userId`, `id`, `title` and `completed`. pandas uses those keys as column names and places each task's values in the corresponding row.
- `value_counts()` counts completed and pending tasks in one line. The project in **Building a Small API Program** needed a loop and two lists to do the same.

The exact formatting and some displayed labels can vary slightly between pandas versions.

## Points to Remember

- **pandas** is an external library for storing, exploring and analysing table data. Install it with `python -m pip install pandas`, and import it as `pd`.
- **A Series** is a one-dimensional labelled collection of values, and **a DataFrame** is a two-dimensional table whose columns are Series.
- **`pd.read_csv()`** reads a CSV file into a DataFrame, and **`to_csv()`** saves a DataFrame to a CSV file.
- **Select columns** by indexing with column labels, **filter rows** with boolean indexing, **sort** with `sort_values()`, and calculate **descriptive statistics** with methods such as `mean()` and `value_counts()`.

## Further Reading

- 📎 **Official pandas guide: 10 minutes to pandas** — https://pandas.pydata.org/docs/user_guide/10min.html
- 📎 **Official pandas getting started tutorials** — https://pandas.pydata.org/docs/getting_started/intro_tutorials/index.html

This completes the topics of this week. You can now use external libraries, set up virtual environments, read documentation, call web APIs safely, and analyse the data you collect with pandas.
