# Strings

A string has been used so far as a single value, something to print, compare or read from `input()`. A string is also a sequence of characters kept in order, so many of the operations used on lists work on strings as well.

> **A string is an ordered sequence of characters.** Each character has a position, so a string can be indexed, sliced and looped through like a list. Unlike a list, a string cannot be changed after it is created.

## Creating a String

A string is written inside single quotes or double quotes. Both produce the same type.

**Example**

```python
first = 'Asha'
second = "Asha"
print(first)
print(second)
print(type(first))
print(first == second)
```

**Output:**

```
Asha
Asha
<class 'str'>
True
```

The quotes mark where the string begins and ends. They are not part of the string, so they do not appear in the output.

## Finding the Length of a String

The `len()` function returns the number of characters in a string. Spaces and punctuation are characters too, so they are counted.

**Syntax**

```
len(string_name)
```

**Example**

```python
name = "Asha"
message = "Hello, Asha!"
print(len(name))
print(len(message))
print(len(""))
```

**Output:**

```
4
12
0
```

`""` is an empty string. It contains no characters, so its length is `0`.

## Accessing a Character

A character is read by writing its index inside square brackets after the string. Positions start at `0`, exactly as in a list.

```
word        P    y    t    h    o    n
index       0    1    2    3    4    5
negative   -6   -5   -4   -3   -2   -1
```

**Syntax**

```
string_name[index]
```

**Example**

```python
word = "Python"
print(word[0])
print(word[3])
print(type(word[0]))
```

**Output:**

```
P
h
<class 'str'>
```

`word[0]` gave the first character. Python has no separate character type, so a single character is a string of length `1`.

## Negative Indexing

A negative index counts from the end of the string, with `-1` for the last character.

**Example**

```python
word = "Python"
print(word[-1])
print(word[-3])
```

**Output:**

```
n
h
```

`word[-1]` gives the last character without needing to know the length of the string.

## Index Out of Range

An index that does not exist in the string causes an error.

**Example**

```python
word = "Python"
print(word[6])
```

**Output:**

```
IndexError: string index out of range
```

The string has six characters, so its highest index is `5`. Python raises an `IndexError`, the same error a list gives.

## Slicing a String

A slice takes a range of characters and returns them as a new string. The stop index is not included.

**Syntax**

```
string_name[start:stop]
```

**Example**

```python
word = "Python"
print(word[0:3])
print(word[2:])
print(word[:4])
```

**Output:**

```
Pyt
thon
Pyth
```

`word[0:3]` returned the characters at indexes `0`, `1` and `2`. Leaving out the start begins at the first character, and leaving out the stop runs to the end.

Slicing is how part of a string is taken out when its position is known.

**Example**

```python
date = "2026-10-05"
print("Year:", date[0:4])
print("Month:", date[5:7])
print("Day:", date[8:])
```

**Output:**

```
Year: 2026
Month: 10
Day: 05
```

## Slicing with a Step

A third value sets the step, which is how far the slice moves each time.

**Syntax**

```
string_name[start:stop:step]
```

**Example**

```python
word = "Python"
print(word[::2])
print(word[::-1])
```

**Output:**

```
Pto
nohtyP
```

A step of `2` selected every other character. A step of `-1` moved through the string backwards, which returns the string reversed.

## Strings Cannot Be Changed

A string is immutable, like a tuple. Assigning to an index raises an error.

**Example**

```python
word = "Python"
word[0] = "J"
```

**Output:**

```
TypeError: 'str' object does not support item assignment
```

A changed version of a string is made by building a new string from parts of the old one.

**Example**

```python
word = "Python"
new_word = "J" + word[1:]
print(new_word)
print(word)
```

**Output:**

```
Jython
Python
```

`new_word` is a separate string. `word` still holds `"Python"`.

Assigning the new string back to the same variable makes the variable refer to the new string.

**Example**

```python
word = "Python"
word = "J" + word[1:]
print(word)
```

**Output:**

```
Jython
```

The string `"Python"` was not changed. The variable `word` now holds a different string.

## Joining Strings

> **Concatenation is the joining of two strings end to end to make a new string.** It is written with the `+` operator.

**Example**

```python
first = "Asha"
last = "Kumar"
full = first + " " + last
print(full)
```

**Output:**

```
Asha Kumar
```

The space was written as its own string. `+` joins exactly what it is given and adds nothing between the parts.

Both sides of `+` must be strings. Joining a string and a number raises an error.

**Example**

```python
mark = 87
print("Marks: " + mark)
```

**Output:**

```
TypeError: can only concatenate str (not "int") to str
```

Converting the number with `str()` first makes the join work.

**Example**

```python
mark = 87
print("Marks: " + str(mark))
```

**Output:**

```
Marks: 87
```

## Repeating a String

The `*` operator repeats a string a given number of times.

**Syntax**

```
string_name * count
```

**Example**

```python
print("ab" * 3)
print("-" * 20)
```

**Output:**

```
ababab
--------------------
```

Repeating a single character is a quick way to print a separator line.

## Checking for a Substring

> **A substring is a sequence of characters that appears inside a larger string.** The `in` operator checks whether a substring is present, and `not in` checks whether it is absent. Both give `True` or `False`.

**Syntax**

```
substring in string_name
substring not in string_name
```

**Example**

```python
message = "Python is easy to learn"
print("easy" in message)
print("hard" in message)
print("hard" not in message)
```

**Output:**

```
True
False
True
```

The check is case-sensitive, so a capital letter and a lower-case letter are different characters.

**Example**

```python
message = "Python is easy to learn"
print("python" in message)
```

**Output:**

```
False
```

`"python"` was not found, because the string contains `"Python"` with a capital `P`.

## Looping Through a String

A `for` loop over a string gives one character on each iteration.

**Example**

```python
name = "Asha"
for letter in name:
    print(letter)
```

**Output:**

```
A
s
h
a
```

The loop ran four times, once for each character.

A condition inside the loop decides what happens for each character.

**Example**

```python
name = "Meera"
vowels = 0
for letter in name:
    if letter in "aeiou":
        vowels = vowels + 1
print("Vowels:", vowels)
```

**Output:**

```
Vowels: 3
```

`letter in "aeiou"` was true for `e`, `e` and `a`, so `vowels` was increased three times.

## Comparing Strings

The `==` operator checks whether two strings contain exactly the same characters in the same order.

**Example**

```python
print("Asha" == "Asha")
print("Asha" == "asha")
print("Asha" == "Asha ")
```

**Output:**

```
True
False
False
```

A different capital letter or an extra space is enough to make two strings unequal.

## Escape Sequences

Some characters cannot be typed directly inside a string. A double quote inside double quotes would end the string early, and pressing Enter would end the line.

> **An escape sequence is a backslash followed by a character, used to write a character that cannot be typed directly inside a string.** Python reads the two characters together as one special character.

**Example**

```python
print("She said \"hello\" quietly")
print("Line one\nLine two")
print("Name:\tAsha")
print("Path: C:\\Users")
```

**Output:**

```
She said "hello" quietly
Line one
Line two
Name:	Asha
Path: C:\Users
```

| Escape sequence | Produces |
| --- | --- |
| `\"` | a double quote |
| `\'` | a single quote |
| `\n` | a new line |
| `\t` | a tab |
| `\\` | a backslash |

An escape sequence is a single character, even though it takes two to write.

**Example**

```python
print(len("a\nb"))
```

**Output:**

```
3
```

A quote only needs escaping when it matches the quotes around the string. Using the other kind of quote on the outside avoids the backslash.

**Example**

```python
print('She said "hello" quietly')
```

**Output:**

```
She said "hello" quietly
```

## Multi-Line Strings

Three quotation marks start a string that can run across several lines. The line breaks are kept exactly as typed.

**Example**

```python
message = """Dear Asha,

Your result is ready.
Please log in to view it."""
print(message)
```

**Output:**

```
Dear Asha,

Your result is ready.
Please log in to view it.
```

No `\n` was written. Every line break inside the triple quotes is part of the string. Either `"""` or `'''` can be used.

## Summary

The key points about strings:

- A string is an ordered sequence of characters.
- A string is written inside single or double quotes, and both produce the type `str`.
- `len()` returns the number of characters, including spaces and punctuation.
- Indexing reads one character, and a single character is a string of length `1`.
- Negative indexes count from the end, with `-1` for the last character.
- An index that does not exist raises an `IndexError`.
- A slice returns a new string, and the stop index is not included.
- A step of `-1` returns the string reversed.
- A string is immutable, so assigning to an index raises a `TypeError`.
- A changed string is made by building a new string from parts of the old one.
- `+` joins two strings, and both sides must be strings.
- `*` repeats a string a given number of times.
- `in` and `not in` check whether a substring is present, and the check is case-sensitive.
- A `for` loop over a string gives one character on each iteration.
- `==` is true only when two strings have exactly the same characters in the same order.
- An escape sequence writes a special character, such as `\n` for a new line.
- Triple quotes create a string that runs across several lines.

## Further Reading

- 📎 **Strings with worked examples** — https://www.programiz.com/python-programming/string
- 📎 **Official Python guide to strings** — https://docs.python.org/3/tutorial/introduction.html#text

A string can now be created, indexed, sliced, joined and searched. But every changed string so far has been built by hand from slices and `+`.

Next, you will learn the methods that strings provide for working with text.
