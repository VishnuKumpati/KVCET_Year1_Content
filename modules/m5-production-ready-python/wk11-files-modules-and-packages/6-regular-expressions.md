# Regular Expressions

Programs often need to find text that follows a pattern, such as any phone number or PIN code, instead of one exact word. Searching for every possible value one by one is impossible. **Regular expressions** let you describe the pattern once and let Python find every match.

> **A regular expression is a pattern that describes the text you want to search for or check.**

Regular expressions are often called **regex** for short.

## The `re` Module

Python works with regular expressions through the built-in **`re` module**.

```python
import re
```

Patterns are usually written as **raw strings**, such as `r"\d{10}"`, so that backslashes are not treated as escape sequences. Raw strings are covered in **File Paths and Errors**.

## Pattern Symbols

A pattern is built using special symbols. These are the most common ones:

| Symbol | Meaning | Example Pattern | Matches |
| --- | --- | --- | --- |
| `\d` | Any digit, including `0` to `9` and many Unicode digits | `\d` | `7` |
| `\w` | Any word character, including letters, digits and underscores. In Python, it also matches letters from other languages. | `\w` | `a`, `5`, `_` |
| `\s` | Any space, tab or new line | `\s` | a space |
| `.` | Any character except a new line | `a.c` | `abc`, `a5c` |
| `+` | One or more of the previous item | `\d+` | `5`, `85`, `2024` |
| `{n}` | Exactly `n` of the previous item | `\d{6}` | `600001` |
| `[ ]` | Any one character inside the brackets | `[aeiou]` | `a`, `e` |

For example, `\d{10}` means **"exactly 10 digits"**.

## Searching for a Pattern

`re.search()` looks through the text and returns the **first** match. If nothing matches, it returns `None`.

```python
import re

text = "Call Ravi on 9876543210 for details."

result = re.search(r"\d{10}", text)

if result:
    print("Found:", result.group())
else:
    print("Not found")
```

**Output:**

```
Found: 9876543210
```

- `r"\d{10}"` describes any 10-digit number.
- `re.search()` finds the first part of the text that matches.
- `result.group()` returns the matched text.
- The `if` check is needed, because `re.search()` returns `None` when there is no match.

## Finding All Matches

`re.findall()` returns **every** match as a list of strings.

```python
import re

text = "Ravi scored 85, Priya scored 92 and Arun scored 78."

marks = re.findall(r"\d+", text)
print(marks)
```

**Output:**

```
['85', '92', '78']
```

`\d+` means "one or more digits", so each number is found as one match. The results are **strings**, not numbers.

The same function can find simple email addresses:

```python
import re

text = "Contact ravi@gmail.com or priya@yahoo.com"

emails = re.findall(r"\w+@\w+\.\w+", text)
print(emails)
```

**Output:**

```
['ravi@gmail.com', 'priya@yahoo.com']
```

`\.` matches a real dot. A backslash before `.` removes its special meaning. **This is a simplified pattern** for learning. It matches only a limited set of email formats, so it should not be used to check all real email addresses.

## Checking the Whole Text

`re.fullmatch()` checks whether the **entire** text matches the pattern. It is useful for checking input, such as a PIN code.

```python
import re

pin_codes = ["600001", "60001", "60A001"]

for pin in pin_codes:
    if re.fullmatch(r"\d{6}", pin):
        print(pin, "is valid")
    else:
        print(pin, "is not valid")
```

**Output:**

```
600001 is valid
60001 is not valid
60A001 is not valid
```

- `600001` has exactly 6 digits, so it matches.
- `60001` has only 5 digits.
- `60A001` contains a letter.

This checks only whether the input contains exactly six digits. It does not check whether the PIN code actually exists or belongs to a particular location.

## Replacing Matches

`re.sub()` replaces every match with new text.

```python
import re

text = "My number is 9876543210."

hidden = re.sub(r"\d", "*", text)
print(hidden)
```

**Output:**

```
My number is **********.
```

Each digit, `\d`, is replaced with `*`, which hides the phone number.

## Points to Remember

- **A regular expression describes a pattern**, not one exact piece of text.
- **Write patterns as raw strings**, such as `r"\d+"`.
- **`re.search()`** finds the first match, **`re.findall()`** finds all matches, **`re.fullmatch()`** checks the whole text, and **`re.sub()`** replaces matches.
- **`re.search()` returns `None`** when there is no match, so check the result before using it.

## Further Reading

- 📎 **Official Python regular expression how-to** — https://docs.python.org/3/howto/regex.html

As a program grows, it may need many modules of its own. In the next topic, **Packages**, you'll learn how to group related modules together in folders.
