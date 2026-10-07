# Regular Expressions

When working with text, a program often needs to find specific information inside a larger string. You already know how to do this when the exact text is known.

**Example**

```python
message = "Contact support on 9876543210 for help"
print(message.find("9876543210"))
```

**Output:**

```
19
```

Here, `find()` works because the exact number is known.

But suppose the program receives many messages, and each message contains a different ten-digit number:

```
Contact support on 9876543210 for help
Contact support on 9123456789 for help
Contact support on 8765432109 for help
```

The program still needs to find the number, but it cannot know the exact digits in advance. All it knows is that every number has ten digits in a row.

This is where **regular expressions (regex)** become useful. Instead of searching for one exact piece of text, a regular expression lets you describe a pattern that the text should follow.

For example:

```
\d{10}
```

means exactly ten digits in a row. It can therefore find `9876543210`, `9123456789`, or any other ten-digit sequence without knowing the actual digits beforehand.

Python provides regular expressions through its built-in `re` module.

```python
import re
```

With the module imported, you can use Python's regular expression functions to search, extract, replace and split text based on patterns.

## Writing a Pattern

The pattern itself describes what to find. In Python, that pattern is written inside a string, and you will often see an `r` before the opening quotation mark.

**Syntax**

```
r"pattern"
```

The `r` tells Python to treat backslashes as literal characters instead of interpreting them as Python escape sequences. This is useful because regular expressions use backslashes frequently.

## Searching for a Pattern

The `re.search()` function looks for the pattern anywhere in a string.

**Syntax**

```
re.search(pattern, string)
```

**Example**

```python
import re

text = "Call 9876543210 today"
match = re.search(r"\d{10}", text)
print(match.group())
print(match.start())
```

**Output:**

```
9876543210
5
```

`re.search()` returns a match object. `group()` gives the text that matched, and `start()` gives the index where it begins.

When nothing matches, the function returns `None`.

**Example**

```python
import re

text = "Call today"
match = re.search(r"\d{10}", text)
print(match)
```

**Output:**

```
None
```

Calling `group()` on `None` would raise an error, so the result is checked first.

**Example**

```python
import re

text = "Call today"
match = re.search(r"\d{10}", text)

if match:
    print("Found:", match.group())
else:
    print("No number found")
```

**Output:**

```
No number found
```

## Finding Every Match

`re.search()` stops at the first match. The `re.findall()` function returns every match as a list.

**Syntax**

```
re.findall(pattern, string)
```

**Example**

```python
import re

text = "Call 9876543210 or 9123456789"
print(re.findall(r"\d{10}", text))
```

**Output:**

```
['9876543210', '9123456789']
```

An empty list is returned when nothing matches, so there is no `None` to check for.

## Common Pattern Symbols

You have already used `\d` to represent a digit. Regular expressions provide other symbols for describing different kinds of characters.

| Pattern | Matches |
| --- | --- |
| `\d` | any digit |
| `\w` | a word character, such as a letter, digit or underscore |
| `\s` | a whitespace character, such as a space, tab or newline |
| `.` | almost any character |

**Example**

```python
import re

text = "Room 7B has 25 seats"
print(re.findall(r"\d", text))
```

**Output:**

```
['7', '2', '5']
```

Each `\d` matched one digit, which is why `25` appears as two separate results.

The dot matches almost any character.

**Example**

```python
import re

print(re.findall(r"c.t", "cat cot cut ct"))
```

**Output:**

```
['cat', 'cot', 'cut']
```

`ct` did not match, because the dot requires one character between the `c` and the `t`.

## Choosing Your Own Characters

Square brackets hold a set of characters, and any one of them matches. A dash inside the brackets gives a range.

**Example**

```python
import re

text = "cat bat rat mat"
print(re.findall(r"[cb]at", text))
print(re.findall(r"[a-z]at", text))
```

**Output:**

```
['cat', 'bat']
['cat', 'bat', 'rat', 'mat']
```

`[cb]` matched a `c` or a `b`. `[a-z]` matched any lowercase letter.

## Saying How Many

A **quantifier** controls how many times the part before it may repeat.

| Quantifier | Meaning |
| --- | --- |
| `{n}` | exactly n times |
| `{n,m}` | between n and m times |
| `+` | one or more times |
| `*` | zero or more times |
| `?` | zero or one time |

**Example**

```python
import re

text = "Marks: 7, 25, 100"
print(re.findall(r"\d+", text))
print(re.findall(r"\d{2}", text))
```

**Output:**

```
['7', '25', '100']
['25', '10']
```

`\d+` matched each run of digits as one result, whatever its length. `\d{2}` matches exactly two digits at a time. So `25` matches completely, while in `100`, the first two digits `10` form a match.

## Replacing by Pattern

The `re.sub()` function replaces every match with the given text and returns a new string.

**Syntax**

```
re.sub(pattern, replacement, string)
```

**Example**

```python
import re

text = "Call 9876543210 or 9123456789"
print(re.sub(r"\d{10}", "XXXXXXXXXX", text))
print(text)
```

**Output:**

```
Call XXXXXXXXXX or XXXXXXXXXX
Call 9876543210 or 9123456789
```

The original string is unchanged, exactly as with `replace()`. The difference is that `replace()` needs the exact digits, and `re.sub()` needs only the pattern.

## Splitting by Pattern

The `re.split()` function splits a string wherever the pattern matches.

**Syntax**

```
re.split(pattern, string)
```

**Example**

```python
import re

text = "87, 72;95  60"
print(re.split(r"[,;\s]+", text))
```

**Output:**

```
['87', '72', '95', '60']
```

The separators are inconsistent, with commas, a semicolon and spaces. `[,;\s]+` matches one or more commas, semicolons or whitespace characters in a row, so one call handles all of them. The `split()` method could not, because it takes only one fixed separator.

## Matching the Whole String

Two symbols fix a pattern to the ends of the string. `^` marks the beginning and `$` marks the end.

**Example**

```python
import re

for value in ["9876543210", "98765432101", "call 9876543210"]:
    match = re.search(r"^\d{10}$", value)
    if match:
        print(value, "is valid")
    else:
        print(value, "is not valid")
```

**Output:**

```
9876543210 is valid
98765432101 is not valid
call 9876543210 is not valid
```

The first value is exactly ten digits. The second has eleven. The third contains ten digits but has other text around them, so the pattern does not match the whole string.

This is how typed input is checked. Without `^` and `$`, `re.search()` could find ten digits anywhere inside the text, even when other characters appear before or after them.

## When to Use a String Method Instead

A regular expression is the right tool when the text follows a pattern rather than being fixed, such as a phone number, a date or a code. It is the wrong tool when the text is known.

Checking whether a sentence contains the word `simple` needs `in` or `find()`, not a pattern. A string method is easier to read and easier to get right.

## Symbol Reference

Every symbol used in this topic, with what it does and why it is needed.

| Symbol | Matches | Used for | Example |
| --- | --- | --- | --- |
| `\d` | any digit | numbers whose digits are not known | `\d{10}` finds a phone number |
| `\w` | a letter, digit or underscore | words and codes | `\w+` finds each word |
| `\s` | a space, tab or newline | the gaps between values | `\s+` matches any run of spaces |
| `.` | almost any character | one character whose value does not matter | `c.t` finds `cat` and `cot` |
| `[abc]` | any one character listed | a small set of allowed characters | `[cb]at` finds `cat` and `bat` |
| `[a-z]` | any one character in the range | letters or digits within limits | `[a-z]at` finds any lowercase letter before `at` |
| `{n}` | exactly n repeats | values of a fixed length | `\d{10}` requires ten digits |
| `{n,m}` | between n and m repeats | values of a varying length | `\d{1,3}` matches one to three digits |
| `+` | one or more repeats | a run of unknown length | `\d+` matches a whole number |
| `*` | zero or more repeats | a part that may be absent | `\d*` matches digits or nothing |
| `?` | zero or one repeat | an optional character | `-?` matches an optional dash |
| `^` | the start of the string | checking a value from its beginning | `^\d` requires a digit first |
| `$` | the end of the string | checking a value to its end | `\d$` requires a digit last |

## Further Reading

- 📎 **Regular expressions with worked examples** — https://www.programiz.com/python-programming/regex
- 📎 **Official Python guide to regular expressions** — https://docs.python.org/3/howto/regex.html

You can now find, replace and split text even when the exact text is not known in advance. But once you have found the information you need, you often need to show it in a readable message.

For example, a program may find a student's name and marks and then need to display:

```
Student: Asha, Marks: 87
```

Using `+` and `str()` works, but it becomes harder to read when a message contains several values.

Next, you will learn f-strings, which make it much easier to put values directly inside a string.
