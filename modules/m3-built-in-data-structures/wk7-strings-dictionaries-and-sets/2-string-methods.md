# String Methods

A string is immutable, so no method changes one. Every method here reads the string it is called on and returns something new, leaving the original exactly as it was. Keep the result or lose it.

```python
name = "anita"
name.upper()
print(name)
```

**Output:**

```
anita
```

Nothing happened, because nothing was kept. The uppercase string was built and immediately discarded. Assign it:

```python
name = "anita"
name = name.upper()
print(name)
```

**Output:**

```
ANITA
```

That pattern applies to every method below.

## Changing Case

Four methods produce differently cased versions:

```python
text = "python is readable"
print(text.upper())
print(text.lower())
print(text.title())
print(text.capitalize())
```

**Output:**

```
PYTHON IS READABLE
python is readable
Python Is Readable
Python is readable
```

`title()` capitalises every word. `capitalize()` capitalises the first character of the string and lowercases everything else. The difference matters for names against sentences.

`lower()` is how case-insensitive comparison is done. Compare the converted forms rather than the originals:

```python
answer = "YES"
if answer.lower() == "yes":
    print("Confirmed")
```

**Output:**

```
Confirmed
```

This handles `"yes"`, `"Yes"`, `"YES"` and `"yEs"` with one test.

## Removing Whitespace

Text typed by a person often carries stray spaces at the ends. `strip()` removes whitespace from both ends:

```python
entry = "   anita   "
print("[" + entry + "]")
print("[" + entry.strip() + "]")
```

**Output:**

```
[   anita   ]
[anita]
```

The brackets are there only to make the spaces visible.

`lstrip()` and `rstrip()` remove from one end only:

```python
entry = "   anita   "
print("[" + entry.lstrip() + "]")
print("[" + entry.rstrip() + "]")
```

**Output:**

```
[anita   ]
[anita   ]
```

Whitespace inside the string is never touched. `strip()` works on the ends.

Given an argument, these methods remove those characters instead of whitespace:

```python
line = "###heading###"
print(line.strip("#"))
```

**Output:**

```
heading
```

Stripping input before using it removes a whole class of bug, where `"anita "` fails to match `"anita"` for no visible reason.

## Searching

`find()` reports the position where a piece of text starts:

```python
sentence = "Python is readable"
print(sentence.find("is"))
print(sentence.find("java"))
```

**Output:**

```
7
-1
```

A missing piece gives `-1` rather than an error, so `find()` can be used as a test as well as a locator.

`index()` does the same search and raises an error instead:

```python
sentence = "Python is readable"
print(sentence.index("java"))
```

**Output:**

```
ValueError: substring not found
```

Use `find()` when absence is expected and `index()` when absence means something has gone wrong.

`count()` reports how many times a piece appears:

```python
print("banana".count("a"))
print("banana".count("na"))
print("banana".count("z"))
```

**Output:**

```
3
2
0
```

`startswith()` and `endswith()` test the ends and return `True` or `False`:

```python
filename = "report.pdf"
print(filename.endswith(".pdf"))
print(filename.startswith("report"))
```

**Output:**

```
True
True
```

These are clearer than slicing for the same job. `filename[-4:] == ".pdf"` works, but it makes the reader count characters.

## Replacing

`replace()` returns a new string with every occurrence swapped:

```python
text = "one two one"
print(text.replace("one", "1"))
print(text)
```

**Output:**

```
1 two 1
one two one
```

Every match was replaced, and the original is untouched. A third argument limits how many:

```python
text = "one two one"
print(text.replace("one", "1", 1))
```

**Output:**

```
1 two one
```

Replacing with an empty string deletes:

```python
number = "98-765-4321"
print(number.replace("-", ""))
```

**Output:**

```
987654321
```

## Splitting and Joining

`split()` cuts a string into a list:

```python
line = "Anita,Ravi,Meera"
names = line.split(",")
print(names)
print(len(names))
```

**Output:**

```
['Anita', 'Ravi', 'Meera']
3
```

The separator is removed and does not appear in any of the pieces.

With no argument, `split()` splits on whitespace and treats any run of it as one separator:

```python
sentence = "Python   is    readable"
print(sentence.split())
```

**Output:**

```
['Python', 'is', 'readable']
```

No empty strings appear between the words. Splitting a sentence into words is the common use, and it makes every list operation available on text:

```python
sentence = "python is readable and python is popular"
words = sentence.split()
print("Words:", len(words))
print("python appears", words.count("python"), "times")
```

**Output:**

```
Words: 7
python appears 2 times
```

`join()` is the reverse, building one string out of a list. It is called on the separator, which reads backwards the first few times:

```python
names = ["Anita", "Ravi", "Meera"]
print(", ".join(names))
print(" and ".join(names))
print("".join(names))
```

**Output:**

```
Anita, Ravi, Meera
Anita and Ravi and Meera
AnitaRaviMeera
```

Read `", ".join(names)` as "put a comma and a space between the items of `names`". The separator has to be the one doing the joining, because that is the string whose contents go between the items.

Every item must already be a string:

```python
marks = [78, 91, 64]
print(", ".join(marks))
```

**Output:**

```
TypeError: sequence item 0: expected str instance, int found
```

Convert first:

```python
marks = [78, 91, 64]
pieces = []
for mark in marks:
    pieces.append(str(mark))
print(", ".join(pieces))
```

**Output:**

```
78, 91, 64
```

`split()` and `join()` together are how text becomes data and data becomes text again. Splitting a line into fields, working on the fields as a list, then joining them back is the shape of a great deal of real text processing.

## Testing the Contents

Three methods report what kind of characters a string holds. Each returns `True` or `False`:

```python
print("12345".isdigit())
print("12.5".isdigit())
print("abc".isalpha())
print("abc1".isalpha())
print("   ".isspace())
```

**Output:**

```
True
False
True
False
True
```

`isdigit()` is `False` for `"12.5"` because `.` is not a digit, and `isalpha()` is `False` for `"abc1"` for the matching reason. Each method requires *every* character to qualify. An empty string returns `False` from all of them.

`isdigit()` is the standard guard before converting input, because `int()` raises `ValueError` on anything else:

```python
answer = input("Enter your age: ")
if answer.isdigit():
    age = int(answer)
    print("Next year you will be", age + 1)
else:
    print("That is not a whole number.")
```

**Output:**

```
Enter your age: 21
Next year you will be 22
```

Note that `isdigit()` rejects negative numbers as well as decimals, since `-` and `.` are not digits.

## Summary of the Methods

| Method | Returns |
| --- | --- |
| `upper()`, `lower()` | the string in one case |
| `title()`, `capitalize()` | the string with capitals at word or sentence start |
| `strip()`, `lstrip()`, `rstrip()` | the string with ends trimmed |
| `find(piece)` | the first position, or `-1` |
| `index(piece)` | the first position, or `ValueError` |
| `count(piece)` | how many times it appears |
| `startswith(piece)`, `endswith(piece)` | `True` or `False` |
| `replace(old, new)` | the string with matches swapped |
| `split(separator)` | a list of pieces |
| `separator.join(items)` | one joined string |
| `isdigit()`, `isalpha()`, `isspace()` | `True` or `False` |

Only `split()` returns a list and only the tests return booleans. Everything else returns a string, and none of them alters the original.

## Further Reading

- **Official string method reference** — https://docs.python.org/3/library/stdtypes.html#string-methods
- **String methods with worked examples** — https://www.programiz.com/python-programming/methods/string

These methods clean, search and reshape text. Next, you will build text for display, replacing the comma-separated `print()` calls used so far.
