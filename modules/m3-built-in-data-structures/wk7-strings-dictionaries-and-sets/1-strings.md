# Strings

A string has been a single value up to now: something to print, or to compare, or to read from `input()`. It is also a **sequence of characters**, held in order, and that changes what can be done with it.

Everything positional carries over unchanged. A string indexes, slices, loops and reports its length exactly as a list does. The one difference is that a string cannot be changed.

## Indexing

Characters are numbered from `0`, and a character is read with square brackets:

```python
word = "Python"
print(word[0])
print(word[3])
```

**Output:**

```
P
t
```

```
 word:    "P"   "y"   "t"   "h"   "o"   "n"
 index:    0     1     2     3     4     5
 negative: -6    -5    -4    -3    -2    -1
```

Negative indexes count from the end:

```python
word = "Python"
print(word[-1])
print(word[-2])
```

**Output:**

```
n
o
```

`len()` counts characters, including spaces and punctuation:

```python
print(len("Python"))
print(len("Hello there"))
print(len(""))
```

**Output:**

```
6
11
0
```

An index past the end is an error, with the same message a list gives except for the type:

```python
word = "Python"
print(word[10])
```

**Output:**

```
IndexError: string index out of range
```

## Slicing

Slices take a range of characters and produce a new string. The `stop` value is excluded, as always:

```python
word = "Python"
print(word[0:3])
print(word[2:])
print(word[:4])
print(word[::-1])
```

**Output:**

```
Pyt
thon
Pyth
nohtyP
```

`[::-1]` reverses a string for exactly the reason it reverses a list: a step of `-1` walks the sequence backwards.

Slicing is how a piece of a string is extracted when its position is known:

```python
date = "2026-09-16"
print("Year:", date[0:4])
print("Month:", date[5:7])
print("Day:", date[8:10])
```

**Output:**

```
Year: 2026
Month: 09
Day: 16
```

## Immutability

A string cannot be changed in place:

```python
word = "Python"
word[0] = "J"
```

**Output:**

```
TypeError: 'str' object does not support item assignment
```

A string is immutable, like a tuple. There is no method that edits a string, and there never will be.

What looks like changing a string is always building a new one:

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

`word` is untouched. `new_word` is a separate string that was assembled from pieces of it.

The same holds when the result is assigned back to the original name:

```python
greeting = "hello"
greeting = greeting.upper()
print(greeting)
```

**Output:**

```
HELLO
```

`upper()` did not modify `"hello"`. It produced a new string, and the assignment pointed the name at it. This is why a string method's result must always be kept: a bare `greeting.upper()` on its own line computes a new string and throws it away.

This is also why passing a string into a function is safe in a way that passing a list is not. A function cannot alter a string its caller holds, because no operation exists that would alter it.

## Membership

`in` tests whether one string appears inside another:

```python
sentence = "Python is readable"
print("read" in sentence)
print("java" in sentence)
print("java" not in sentence)
```

**Output:**

```
True
False
True
```

Note the difference from a list. `in` on a list compares whole items, so a list of names contains `"Ravi"` but not `"Rav"`. `in` on a string matches any run of characters, so `"read"` is found inside `"readable"`.

The test is case sensitive:

```python
print("python" in "Python is readable")
```

**Output:**

```
False
```

## Looping over the Characters

A `for` loop over a string takes one character per iteration:

```python
for letter in "cat":
    print(letter)
```

**Output:**

```
c
a
t
```

That makes the ordinary loop patterns available on text. Counting vowels, for instance:

```python
sentence = "python programming"
vowel_count = 0
for letter in sentence:
    if letter in "aeiou":
        vowel_count = vowel_count + 1
print("Vowels:", vowel_count)
```

**Output:**

```
Vowels: 4
```

`enumerate()` works too, when the position of a character is needed:

```python
for position, letter in enumerate("cat"):
    print(position, letter)
```

**Output:**

```
0 c
1 a
2 t
```

## Concatenation and Repetition

`+` joins two strings into a new one:

```python
first = "Anita"
last = "Sharma"
full = first + " " + last
print(full)
```

**Output:**

```
Anita Sharma
```

The space had to be supplied. `+` joins exactly what it is given and adds nothing.

Both sides must be strings. A number has to be converted first:

```python
age = 21
print("Age: " + age)
```

**Output:**

```
TypeError: can only concatenate str (not "int") to str
```

`*` repeats a string a whole number of times:

```python
print("ab" * 3)
print("-" * 20)
```

**Output:**

```
ababab
--------------------
```

Repeating a character is the usual way to draw a separator line without typing it out.

## Escape Sequences

Some characters cannot be typed directly inside a string. A quote would end the string early, and a newline would end the line. A backslash marks the character that follows as special:

```python
print("She said \"hello\" quietly")
print("Line one\nLine two")
print("Name:\tAnita")
print("Path: C:\\Users")
```

**Output:**

```
She said "hello" quietly
Line one
Line two
Name:	Anita
Path: C:\Users
```

| Escape | Produces |
| --- | --- |
| `\"` | a double quote |
| `\'` | a single quote |
| `\n` | a newline |
| `\t` | a tab |
| `\\` | a backslash |

`\n` is a single character despite taking two to write:

```python
print(len("a\nb"))
```

**Output:**

```
3
```

Escaping a quote is only needed when it matches the quotes around the string. Swapping the outer quotes avoids the backslash entirely:

```python
print('She said "hello" quietly')
```

**Output:**

```
She said "hello" quietly
```

## Multi-Line Strings

Three quote marks open a string that may run across several lines, with the line breaks kept as typed:

```python
message = """Dear Anita,

Your result is ready.
Please log in to view it."""
print(message)
```

**Output:**

```
Dear Anita,

Your result is ready.
Please log in to view it.
```

No `\n` was written. Every line break inside the triple quotes is part of the string, which makes this the readable way to hold a block of text.

Either `"""` or `'''` works. Both are also what a docstring is written with.

## Further Reading

- **Official Python guide to strings** — https://docs.python.org/3/tutorial/introduction.html#text
- **Strings and their operations** — https://www.programiz.com/python-programming/string

A string is a sequence, so indexing, slicing, looping and membership all apply, and immutability means every operation on one produces a new string. Next, you will use the methods that produce those new strings.
