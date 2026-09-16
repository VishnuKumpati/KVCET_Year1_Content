# Introduction to pandas

A list of dictionaries handles tabular data perfectly well, and the data wrangling already covered — filtering, sorting, grouping, totalling — is all doable with comprehensions and loops.

**pandas** does the same work in far less code, and stays fast when the table has a million rows rather than ten. It is the standard tool for data analysis in Python, and the foundation most data science work sits on.

```
pip install pandas
```

The conventional import is `pd`:

```python
import pandas as pd
```

## Series and DataFrame

Two types do everything.

A **Series** is one column: a sequence of values with an index.

```python
import pandas as pd

marks = pd.Series([78, 91, 64], name="mark")
print(marks)
```

**Output:**

```
0    78
1    91
2    64
Name: mark, dtype: int64
```

The left column is the **index**, added automatically. `dtype: int64` is the type of every value — a Series holds one type throughout, which is what makes it fast.

A **DataFrame** is a table: several Series sharing one index.

```python
import pandas as pd

students = pd.DataFrame({
    "name": ["Anita", "Ravi", "Meera", "Sunil"],
    "course": ["CS", "Maths", "CS", "Maths"],
    "mark": [78, 91, 64, 88],
})
print(students)
```

**Output:**

```
    name course  mark
0  Anita     CS    78
1   Ravi  Maths    91
2  Meera     CS    64
3  Sunil  Maths    88
```

Each key became a column, each value the column's contents. This is the shape almost all pandas work starts from.

## Reading a File

In practice the data comes from a file, and `read_csv()` is one line:

```python
import pandas as pd

students = pd.read_csv("students.csv")
print(students)
```

**Output:**

```
    name course  mark
0  Anita     CS    78
1   Ravi  Maths    91
2  Meera     CS    64
3  Sunil  Maths    88
```

Compare that with `open()`, `csv.DictReader`, a loop and an `int()` conversion — pandas read the header, split the rows and worked out the types.

`read_json()`, `read_excel()` and others follow the same pattern.

## Inspecting

Before analysing anything, find out what arrived:

```python
print(students.shape)
print(students.columns.tolist())
print(students.dtypes)
```

**Output:**

```
(4, 3)
['name', 'course', 'mark']
name        str
course      str
mark      int64
dtype: object
```

`shape` is `(rows, columns)`. `dtypes` shows the type pandas inferred per column — which is worth checking, because a column of numbers that arrived with a stray non-numeric value will be text, and arithmetic on it will fail or concatenate.

Older versions of pandas report text columns as `object` rather than `str`. Both mean the same thing.

`head()` shows the first rows, which is essential when the table is large:

```python
print(students.head(2))
```

**Output:**

```
    name course  mark
0  Anita     CS    78
1   Ravi  Maths    91
```

`describe()` summarises every numeric column at once:

```python
print(students.describe())
```

**Output:**

```
            mark
count   4.000000
mean   80.250000
std    12.175796
min    64.000000
25%    74.500000
50%    83.000000
75%    88.750000
max    91.000000
```

Count, mean, standard deviation, minimum, maximum and the quartiles, from one call. Only `mark` appears, because the other columns are text.

`info()` reports the column types and how many non-null values each holds — the quickest way to spot missing data.

## Selecting

One column, by name:

```python
print(students["name"])
```

**Output:**

```
0    Anita
1     Ravi
2    Meera
3    Sunil
Name: name, dtype: str
```

That is a Series. Several columns need a **list** of names, and give a DataFrame:

```python
print(students[["name", "mark"]])
```

**Output:**

```
    name  mark
0  Anita    78
1   Ravi    91
2  Meera    64
3  Sunil    88
```

The double brackets confuse everyone at first. The outer pair is the selection; the inner pair is the list of columns.

Rows are selected with `loc` (by label) and `iloc` (by position):

```python
print(students.loc[0])
print(students.loc[0, "name"])
print(students.iloc[0:2])
```

**Output:**

```
name      Anita
course       CS
mark         78
Name: 0, dtype: object
Anita
    name course  mark
0  Anita     CS    78
1   Ravi  Maths    91
```

`loc[0, "name"]` takes a row and a column, in that order. With the default index the two are the same, and they diverge once the index is something else — a date, or a student id.

## Filtering

A comparison on a column produces a Series of booleans, and a DataFrame indexed by that keeps the rows where it is `True`:

```python
print(students["mark"] >= 78)
print(students[students["mark"] >= 78])
```

**Output:**

```
0     True
1     True
2    False
3     True
Name: mark, dtype: bool
    name course  mark
0  Anita     CS    78
1   Ravi  Maths    91
3  Sunil  Maths    88
```

This is **boolean indexing**, and it replaces the filtering loop entirely.

Several conditions combine with `&` and `|`, and **each condition needs brackets**:

```python
print(students[(students["mark"] >= 78) & (students["course"] == "CS")])
```

**Output:**

```
    name course  mark
0  Anita     CS    78
```

Note `&` and `|`, not `and` and `or`. The keywords work on single true-or-false values; pandas needs the operators, which work element by element. Omitting the brackets produces a confusing error, because `&` binds more tightly than `>=`.

## Adding a Column

Assigning to a new name creates a column, computed across every row at once:

```python
students["passed"] = students["mark"] >= 70
print(students)
```

**Output:**

```
    name course  mark  passed
0  Anita     CS    78    True
1   Ravi  Maths    91    True
2  Meera     CS    64   False
3  Sunil  Maths    88    True
```

No loop. `students["mark"] >= 70` was evaluated for the whole column in one operation — **vectorisation**, and the reason pandas is fast. Writing a loop over rows is both slower and longer, and is the commonest way to write bad pandas.

## Sorting

```python
print(students.sort_values("mark", ascending=False))
```

**Output:**

```
    name course  mark  passed
1   Ravi  Maths    91    True
3  Sunil  Maths    88    True
0  Anita     CS    78    True
2  Meera     CS    64   False
```

The index came along — row `1` is still row `1`. Like `sorted()`, this returns a new DataFrame and leaves the original alone.

## Grouping

`groupby` is where pandas earns its place. The grouping loop with `setdefault()` becomes:

```python
print(students.groupby("course")["mark"].mean())
```

**Output:**

```
course
CS       71.0
Maths    89.5
Name: mark, dtype: float64
```

Read it as: group by course, take the mark column, average it.

`agg` computes several summaries at once:

```python
print(students.groupby("course")["mark"].agg(["count", "mean", "max"]))
```

**Output:**

```
        count  mean  max
course
CS          2  71.0   78
Maths       2  89.5   91
```

One line for what was a loop, a dictionary of lists, and two dictionary comprehensions.

## Missing Data

Real data has gaps. pandas represents them as `NaN` and handles them consistently:

```python
import pandas as pd

messy = pd.DataFrame({
    "name": ["Anita", "Ravi", "Meera"],
    "mark": [78, None, 64],
})
print(messy)
print(messy["mark"].isna().sum())
print(messy["mark"].mean())
```

**Output:**

```
    name  mark
0  Anita  78.0
1   Ravi   NaN
2  Meera  64.0
1
71.0
```

Two things to notice. The column became floats, because `NaN` is a float. And `mean()` **skipped** the missing value — `(78 + 64) / 2`, not `/ 3`. That is usually right and is worth knowing, because it is silent.

`dropna()` removes rows with gaps, and `fillna()` replaces them:

```python
print(messy.dropna())
print(messy.fillna(0))
```

**Output:**

```
    name  mark
0  Anita  78.0
2  Meera  64.0
    name  mark
0  Anita  78.0
1   Ravi   0.0
2  Meera  64.0
```

Choose deliberately. Dropping loses records; filling with `0` drags an average down. There is no default that is right for every column.

## Writing

```python
students.to_csv("output.csv", index=False)
```

`index=False` matters. Without it, the index becomes an unnamed first column in the file, which then reappears as a junk column the next time anything reads it.

## A Complete Analysis

```python
import pandas as pd

students = pd.read_csv("students.csv")

passed = students[students["mark"] >= 70]
summary = passed.groupby("course")["mark"].agg(["count", "mean"])

print(summary)
summary.to_csv("summary.csv")
```

**Output:**

```
        count  mean
course
CS          1  78.0
Maths       2  89.5
```

Read, filter, group, summarise, save — five lines.

## When to Use pandas

**Use it** for tabular data, for files with thousands of rows or more, for summarising and grouping, and for anything heading towards analysis or a chart.

**Do not use it** for a handful of records, for deeply nested JSON that is not really a table, or inside a program that otherwise has no need of it. `import pandas` costs a real dependency and a noticeable startup delay, and a list of ten dictionaries needs neither.

The honest test: if the work is a table and a question about it, pandas will be shorter and clearer. If it is a few records being passed around a program, plain Python already fits.

## Further Reading

- **The pandas documentation** — https://pandas.pydata.org/docs/
- **10 minutes to pandas** — https://pandas.pydata.org/docs/user_guide/10min.html

A DataFrame is a table, boolean indexing filters it, `groupby` summarises it, and operations apply to whole columns at once rather than row by row. That completes the outside world — next, making sure your own code is right.
