# Strings

A string has been used from the beginning of Python as a way to represent text inside quotation marks. What was not said then is that a string is also a sequence, built from individual characters in order, exactly as a list is built from elements.

Everything already learned about positions, indexing and slicing therefore applies to a string as well.

> **A string is an ordered sequence of characters.** Each character has a position, starting at `0`. A string is also immutable, which means its characters cannot be changed, added or removed after the string is created.

## String Immutability

Immutability is the one behaviour that sets a string apart from a list. A list allows its elements to be replaced, and a string does not.

**Example**

```python
name = "Python"
name[0] = "J"
```

**Output:**

```
TypeError: 'str' object does not support item assignment
```

A string cannot be changed. If you try to add something to a string, Python creates a new string and the old one stays the same.

**Example**

```python
name = "Python"
new_name = name + " 3"
print(new_name)
print(name)
```

**Output:**

```
Python 3
Python
```

`new_name` holds `Python 3`. `name` still holds `Python`. The `+` did not add anything to `name`. It made a new string out of it.

The same thing happens when the result is stored back in the same variable.

**Example**

```python
name = "Python"
name = name + " 3"
print(name)
```

**Output:**

```
Python 3
```

It looks as if `name` grew longer. It did not. Python made a new string, `Python 3`, and the assignment stored that new string in `name`. The original string `Python` was not changed.

Whenever an operation produces changed text, it produces a new string. The original string stays exactly as it was.

## Finding the Length of a String

The `len()` function returns the number of characters in a string.

**Example**

```python
name = "Python"
print(len(name))
```

**Output:**

```
6
```

Spaces count as characters, because a space is a character like any other.

**Example**

```python
spaced = "a b"
print(len(spaced))
```

**Output:**

```
3
```

## Accessing a Character

A character is read by writing its index in square brackets after the string name. Positions start at `0`, and a negative index counts from the end.

**Syntax**

```
string_name[index]
```

**Example**

```python
name = "Python"
print(name[0])
print(name[5])
print(name[-1])
```

**Output:**

```
P
n
n
```

```
name      =    P    y    t    h    o    n
index          0    1    2    3    4    5
negative      -6   -5   -4   -3   -2   -1
```

An index outside the string raises an error.

**Example**

```python
name = "Python"
print(name[6])
```

**Output:**

```
IndexError: string index out of range
```

The string has six characters, so its highest index is `5`.

## Slicing a String

A slice takes a range of characters and returns them as a new string. The stop index is not included.

**Syntax**

```
string_name[start:stop]
string_name[start:stop:step]
```

**Example**

```python
name = "Python"
print(name[0:3])
print(name[:3])
print(name[3:])
print(name[::2])
print(name[::-1])
```

**Output:**

```
Pyt
Pyt
hon
Pto
nohtyP
```

Every form behaves as it does on a list. Leaving out the start or stop uses the beginning or the end, a step controls how many positions the slice moves each time, and a negative step returns the characters in reverse order.

## Checking Whether Text Is in a String

The `in` operator tests whether one string appears inside another. It returns `True` or `False`.

**Syntax**

```
text in string_name
text not in string_name
```

**Example**

```python
sentence = "Python is simple"
print("is" in sentence)
print("Java" in sentence)
print("Java" not in sentence)
```

**Output:**

```
True
False
True
```

On a list, `in` looks for a whole element. On a string, it looks for a run of characters anywhere in the text.

## Looping Through a String

A `for` loop over a string gives one character on each iteration.

**Example**

```python
name = "cat"
for letter in name:
    print(letter)
```

**Output:**

```
c
a
t
```

## Joining and Repeating Strings

The `+` operator joins two strings end to end, which is called concatenation. The `*` operator repeats a string a given number of times.

**Example**

```python
first = "Py"
second = "thon"
print(first + second)
print("ab" * 3)
```

**Output:**

```
Python
ababab
```

Both operands of `+` must be strings. Joining a string to a number raises an error.

**Example**

```python
age = 20
print("Age: " + age)
```

**Output:**

```
TypeError: can only concatenate str (not "int") to str
```

The number has to be converted first, with `str(age)`.

## Escape Sequences

An **escape sequence** uses a backslash followed by a character to represent something special inside a string, such as a quotation mark, new line or tab.

| Escape sequence | Meaning |
| --- | --- |
| `\"` | a double quotation mark |
| `\'` | a single quotation mark |
| `\n` | a new line |
| `\t` | a tab |
| `\\` | a backslash |

**Example**

```python
print("She said \"hello\"")
print('It\'s fine')
print("Line one\nLine two")
print("Name\tMark")
print("C:\\pythonwork")
```

**Output:**

```
She said "hello"
It's fine
Line one
Line two
Name    Mark
C:\pythonwork
```

Each escape sequence is one character in the string, even though it is written with two.

## Multi-line Strings

A string written in triple quotation marks can span several lines. The line breaks are part of the string.

**Example**

```python
message = """Dear Asha,
Your result is ready.
Thank you."""
print(message)
```

**Output:**

```
Dear Asha,
Your result is ready.
Thank you.
```

Triple quotes are commonly used for docstrings, especially when the documentation spans several lines.

## Further Reading

- 📎 **Strings with worked examples** — https://www.programiz.com/python-programming/string
- 📎 **Official Python guide to strings** — https://docs.python.org/3/tutorial/introduction.html#text

Reading and slicing a string covers only part of what a program does with text. Changing case, removing spaces, searching and splitting are all common tasks.

Next, you will learn the methods a string provides for those.
