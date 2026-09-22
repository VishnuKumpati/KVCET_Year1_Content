# f-strings

Putting values into text has been done two ways so far, and both have problems.

Commas in `print()` insert a space between every item, whether or not one is wanted:

```python
name = "Anita"
age = 21
print("Name:", name, "- Age:", age)
```

**Output:**

```
Name: Anita - Age: 21
```

Concatenation with `+` gives control of the spacing and demands `str()` around every number:

```python
name = "Anita"
age = 21
print("Name: " + name + " - Age: " + str(age))
```

**Output:**

```
Name: Anita - Age: 21
```

The second is fiddly to write and hard to read, and it only works inside `print()`. Neither builds a string you can store.

An **f-string** does both jobs cleanly.

## The f Prefix

Put `f` immediately before the opening quote, and any name in braces is replaced by its value:

```python
name = "Anita"
age = 21
print(f"Name: {name} - Age: {age}")
```

**Output:**

```
Name: Anita - Age: 21
```

The text reads as it will appear, spacing included. Values in braces are converted automatically, so `str()` is never needed.

An f-string is an ordinary string, so the result can be stored rather than printed:

```python
name = "Anita"
age = 21
line = f"{name} is {age}"
print(line)
print(len(line))
```

**Output:**

```
Anita is 21
11
```

Forgetting the `f` is the usual mistake. Without it, the braces are just characters:

```python
name = "Anita"
print("Hello {name}")
```

**Output:**

```
Hello {name}
```

An actual brace in the output is written by doubling it: `{{` produces `{`.

## Expressions in the Braces

The braces take any expression, not only a name:

```python
marks = [78, 91, 64]
name = "anita"
print(f"Highest: {max(marks)}")
print(f"Average: {sum(marks) / len(marks)}")
print(f"Name: {name.upper()}")
print(f"First mark: {marks[0]}")
```

**Output:**

```
Highest: 91
Average: 77.66666666666667
Name: ANITA
First mark: 78
```

Arithmetic, method calls, indexing and function calls all work. Keep them short. An f-string carrying a complicated calculation is harder to read than a line that computes the value first and then prints it.

Quotes inside the braces must differ from the quotes around the string:

```python
student = ["Anita", 78]
print(f"Name: {student[0]}")
```

If a quoted string is needed inside the braces, use single quotes inside a double-quoted f-string.

## Formatting Numbers

A colon inside the braces introduces a **format specification**, which controls how the value is displayed.

Decimal places are the most common need. `.2f` means "as a fixed-point number with two decimal places":

```python
average = 77.66666666666667
print(f"Average: {average:.2f}")
print(f"Average: {average:.1f}")
print(f"Average: {average:.0f}")
```

**Output:**

```
Average: 77.67
Average: 77.7
Average: 78
```

The value is rounded for display. The variable itself is unchanged, and the full precision is still there for any further calculation.

A comma inserts thousands separators:

```python
population = 1234567
print(f"Population: {population:,}")
```

**Output:**

```
Population: 1,234,567
```

The two combine, in that order:

```python
revenue = 1234567.891
print(f"Revenue: {revenue:,.2f}")
```

**Output:**

```
Revenue: 1,234,567.89
```

`%` multiplies by 100 and adds a percent sign, which is what a proportion normally needs:

```python
rate = 0.2567
print(f"Pass rate: {rate:.1%}")
```

**Output:**

```
Pass rate: 25.7%
```

Note that the value must be the proportion, not the percentage. `0.2567` displays as `25.7%`.

| Specification | Meaning | `1234.5678` becomes |
| --- | --- | --- |
| `.2f` | two decimal places | `1234.57` |
| `,` | thousands separators | `1,234.5678` |
| `,.2f` | both | `1,234.57` |
| `.1%` | percentage, one decimal | `123456.8%` |

## Width and Alignment

A number after the colon sets a minimum **width**, padding with spaces to fill it. Alignment is chosen with `<`, `>` or `^`:

```python
name = "Anita"
print(f"[{name:<10}]")
print(f"[{name:>10}]")
print(f"[{name:^10}]")
```

**Output:**

```
[Anita     ]
[     Anita]
[  Anita   ]
```

The brackets are there to make the padding visible.

| Symbol | Alignment |
| --- | --- |
| `<` | left |
| `>` | right |
| `^` | centre |

A value longer than the width is never cut off. The width is a minimum, not a limit.

This is what makes printed tables line up. Left-align text, right-align numbers, and give each column a fixed width:

```python
names = ["Anita", "Ravi", "Meera"]
marks = [78, 91, 64]

print(f"{'Name':<10}{'Mark':>5}")
for name, mark in zip(names, marks):
    print(f"{name:<10}{mark:>5}")
```

**Output:**

```
Name       Mark
Anita        78
Ravi         91
Meera        64
```

Numbers right-aligned put their last digits in one column, which is what makes a column of figures readable.

Width combines with the number formats, width first:

```python
prices = [5.5, 120.0, 19.99]
for price in prices:
    print(f"{price:>10.2f}")
```

**Output:**

```
      5.50
    120.00
     19.99
```

## Older Formatting Styles

Two earlier styles still appear in existing code and in older tutorials. Both do the same job less clearly.

`format()` is a string method, with empty braces as placeholders filled from its arguments:

```python
name = "Anita"
age = 21
print("{} is {}".format(name, age))
```

**Output:**

```
Anita is 21
```

Older still is the `%` operator, where `%s` stands for a string and `%d` for a whole number:

```python
name = "Anita"
age = 21
print("%s is %d" % (name, age))
```

**Output:**

```
Anita is 21
```

Both accept the same format specifications after a colon or a percent sign, so `"{:.2f}".format(value)` and `"%.2f" % value` mean what `f"{value:.2f}"` means.

Recognise them when reading other people's code. Write f-strings in your own: the value sits where it appears in the output, rather than in a list at the end that has to be matched up by counting.

## Further Reading

- **Official format specification reference** — https://docs.python.org/3/library/string.html#format-specification-mini-language
- **f-strings in depth** — https://realpython.com/python-f-strings/

f-strings put values into text and control how those values are displayed. Next, you will store values that are found by name rather than by position.
