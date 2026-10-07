# Formatted Strings

A program rarely has only values to show. It has a message to present, with values sitting inside it, such as a student's name and marks in a line of a report.

Until now, building that message has required `+` and `str()`.

**Example**

```python
name = "Asha"
marks = 87
print("Student: " + name + ", Marks: " + str(marks))
```

**Output:**

```
Student: Asha, Marks: 87
```

The line is hard to read and easy to get wrong. The quotation marks, the `+` signs and the `str()` call all have to be in the right places, and a missing space inside the quotes is invisible until the program runs.

A formatted string writes the same message with the values placed directly inside the text.

## Writing an f-string

An **f-string** is a string with the letter `f` before the opening quotation mark. Anything inside curly braces is evaluated, and the result is inserted into the string.

**Syntax**

```
f"text {value} more text"
```

**Example**

```python
name = "Asha"
marks = 87
print(f"Student: {name}, Marks: {marks}")
```

**Output:**

```
Student: Asha, Marks: 87
```

The same output, in one readable line. No `+` signs and no `str()`. Python automatically places the values into the string.

The `f` is required. Without it, the braces and their contents are printed as ordinary characters.

**Example**

```python
name = "Asha"
print("Hello {name}")
```

**Output:**

```
Hello {name}
```

## Expressions Inside the Braces

The braces can hold any expression, not just a variable name. Python works it out first and puts the result into the string.

**Example**

```python
first = 87
second = 72
print(f"Total: {first + second}")
print(f"Average: {(first + second) / 2}")

name = "asha"
print(f"Name: {name.title()}")
```

**Output:**

```
Total: 159
Average: 79.5
Name: Asha
```

Arithmetic, method calls and function calls all work inside the braces.

## Controlling the Number of Decimal Places

A division often produces a long decimal. A colon after the value, followed by `.2f`, formats the number to two decimal places.

**Syntax**

```
f"{value:.2f}"
```

**Example**

```python
average = 84.666666
print(f"Average: {average}")
print(f"Average: {average:.2f}")
print(f"Average: {average:.0f}")
```

**Output:**

```
Average: 84.666666
Average: 84.67
Average: 85
```

The number before the `f` specifies how many decimal places to show. The value itself is unchanged; only the text produced is rounded.

## Adding Thousands Separators

A colon followed by a comma groups large numbers in threes.

**Syntax**

```
f"{value:,}"
```

**Example**

```python
population = 1234567
print(f"Population: {population:,}")
```

**Output:**

```
Population: 1,234,567
```

The comma and the decimal places can be used together, written as `,.2f`. This is the usual way to show an amount of money.

**Example**

```python
amount = 1234567.891
print(f"Amount: {amount:,.2f}")
```

**Output:**

```
Amount: 1,234,567.89
```

## Setting the Width of a Value

A colon followed by a number sets the minimum number of character positions for the value. This lines values into columns when several lines are printed.

| Format | Effect |
| --- | --- |
| `{value:<10}` | left aligned in 10 characters |
| `{value:>10}` | right aligned in 10 characters |
| `{value:^10}` | centred in 10 characters |

**Example**

```python
name = "Asha"
mark = 87
print(f"{name:<10}{mark:>5}")
```

**Output:**

```
Asha         87
```

`name` gets a minimum of 10 character positions and `mark` gets a minimum of 5. `<` puts the name on the left, while `>` puts the mark on the right.

## Printing a Brace

A brace that should appear in the output is written twice.

**Example**

```python
print(f"{{braces}}")
```

**Output:**

```
{braces}
```

## Formatted Strings Across Several Lines

An f-string can use triple quotation marks, so a long message keeps its line breaks.

**Example**

```python
name = "Asha"
marks = 87
message = f"""Dear {name},
Your marks: {marks}
Thank you."""
print(message)
```

**Output:**

```
Dear Asha,
Your marks: 87
Thank you.
```

## Formatting Reference

Every formatting option used in this topic, with what it does and why it is needed.

| Format | Effect | Used for |
| --- | --- | --- |
| `{value}` | inserts the value as text | putting any value into a message |
| `{value:.2f}` | shows two decimal places | prices, averages and percentages |
| `{value:,}` | groups digits in threes | large numbers that are hard to read |
| `{value:,.2f}` | groups digits and shows two decimal places | amounts of money |
| `{value:<10}` | left aligned in 10 characters | names and words in a column |
| `{value:>10}` | right aligned in 10 characters | numbers in a column |
| `{value:^10}` | centred in 10 characters | headings above a column |
| `{{` and `}}` | one brace in the output | text that contains braces |

## Further Reading

- 📎 **f-strings with worked examples** — https://www.programiz.com/python-programming/string-formatting
- 📎 **Official Python guide to formatted strings** — https://docs.python.org/3/tutorial/inputoutput.html

Strings, lists and tuples all find a value by its position. A program often needs to find a value by name instead, such as the marks belonging to a particular student.

Next, you will learn a collection that stores values under names.
