# String Methods

A string cannot be changed, so a method never alters the string it is called on. Methods that transform text return a new string. Methods that answer a question return a number or a Boolean.

This rule explains why the methods in this topic never change the original string.

## Changing the Case of a String

Four methods change the capitalization of a string.

| Method | Returns |
| --- | --- |
| `upper()` | every letter in uppercase |
| `lower()` | every letter in lowercase |
| `title()` | the first letter of every word in uppercase |
| `capitalize()` | the first letter of the string in uppercase, the rest lowercase |

**Example**

```python
name = "python Programming"
print(name.upper())
print(name.lower())
print(name.title())
print(name.capitalize())
print(name)
```

**Output:**

```
PYTHON PROGRAMMING
python programming
Python Programming
Python programming
python Programming
```

The last line shows the original string, unchanged. Each method returned a new string and left `name` alone.

`lower()` is useful when comparing text that a person typed, because `"Yes"` and `"yes"` are different strings and converting both to lowercase makes them match.

## Removing Spaces from the Ends

Typed input often carries spaces at the start or the end. Three methods remove them.

| Method | Removes spaces from |
| --- | --- |
| `strip()` | both ends |
| `lstrip()` | the left end |
| `rstrip()` | the right end |

**Example**

```python
answer = "  yes  "
print("[" + answer.strip() + "]")
print("[" + answer.lstrip() + "]")
print("[" + answer.rstrip() + "]")
```

**Output:**

```
[yes]
[yes  ]
[  yes]
```

The square brackets are printed only to make the spaces visible. None of these methods touches spaces inside the text.

## Searching Inside a String

Four methods report on what a string contains. None of them changes it.

**Syntax**

```
string_name.find(text)
string_name.count(text)
string_name.startswith(text)
string_name.endswith(text)
```

`find()` returns the index where the text begins, or `-1` when the text is absent. `count()` returns how many times the text appears.

**Example**

```python
sentence = "Python is simple"
print(sentence.find("is"))
print(sentence.find("Java"))
print(sentence.count("i"))
```

**Output:**

```
7
-1
2
```

`is` begins at index `7`. `Java` is not in the string, so `find()` returned `-1` rather than raising an error.

The `index()` method does the same as `find()`, except that it raises an error when the text is absent.

**Example**

```python
sentence = "Python is simple"
print(sentence.index("Java"))
```

**Output:**

```
ValueError: substring not found
```

Use `find()` when the text may be missing, and `index()` when it should always be there.

`startswith()` and `endswith()` return a Boolean.

**Example**

```python
name = "Python"
print(name.startswith("Py"))
print(name.endswith("on"))
```

**Output:**

```
True
True
```

## Replacing Text

The `replace()` method returns a new string with one piece of text swapped for another.

**Syntax**

```
string_name.replace(old_text, new_text)
string_name.replace(old_text, new_text, count)
```

**Example**

```python
sentence = "Python is simple"
print(sentence.replace("simple", "powerful"))
print(sentence)
```

**Output:**

```
Python is powerful
Python is simple
```

Every occurrence is replaced unless a third argument limits how many.

**Example**

```python
print("a-b-a-b".replace("a", "x", 1))
```

**Output:**

```
x-b-a-b
```

Only the first `a` changed, because the count was `1`.

## Splitting a String into a List

The `split()` method breaks a string apart and returns the pieces as a list.

**Syntax**

```
string_name.split()
string_name.split(separator)
```

With no argument, it splits at the spaces.

**Example**

```python
sentence = "Python is simple"
print(sentence.split())
```

**Output:**

```
['Python', 'is', 'simple']
```

With an argument, it splits at that text instead.

**Example**

```python
print("87,72,95".split(","))
```

**Output:**

```
['87', '72', '95']
```

The elements are strings, even when they look like numbers. Each one needs `int()` before it can be used in a calculation.

## Joining a List into a String

The `join()` method does the opposite. It is called on the separator, and takes the list as its argument.

**Syntax**

```
separator.join(list_name)
```

**Example**

```python
words = ["Python", "is", "simple"]
print(" ".join(words))
print("-".join(words))
```

**Output:**

```
Python is simple
Python-is-simple
```

The method is called on the separator because the separator is what goes between the elements.

Every element of the list must be a string.

## Testing What a String Contains

Three methods return a Boolean describing the characters in a string.

| Method | Returns `True` when |
| --- | --- |
| `isdigit()` | every character is a digit |
| `isalpha()` | every character is a letter |
| `isspace()` | every character is a space |

**Example**

```python
print("87".isdigit())
print("8a".isdigit())
print("Asha".isalpha())
print("   ".isspace())
```

**Output:**

```
True
False
True
True
```

`isdigit()` can be used to check typed input before converting it with `int()`.

## Calling Methods One After Another

A method that returns a string can have another method called on its result straight away. This is called chaining.

**Example**

```python
answer = "  YES  "
print(answer.strip().lower())
```

**Output:**

```
yes
```

`strip()` ran first and returned `"YES"`. `lower()` was then called on that result and returned `"yes"`.

This pattern is how typed input is usually prepared before it is compared.

**Example**

```python
answer = input("Continue? ")
if answer.strip().lower() == "yes":
    print("Carrying on")
else:
    print("Stopping")
```

**Output:**

```
Continue?   YES  
Carrying on
```

The person typed capitals with spaces around them, and the comparison still matched.

## Further Reading

- 📎 **String methods with worked examples** — https://www.programiz.com/python-programming/methods/string
- 📎 **Official Python reference for string methods** — https://docs.python.org/3/library/stdtypes.html#string-methods

Building a message from several values still needs `+` and `str()`, which becomes awkward as soon as there is more than one value to insert.

Next, you will learn a way to put values directly inside a string.
