# Regular Expressions

String methods search for text you can write out in full. `find("cat")` needs the exact word. A great deal of real work is not like that: find every number in a line, check that an entry looks like an email address, split on any run of punctuation. The thing being searched for has a *shape*, not a spelling.

A **regular expression** describes that shape. It is a small pattern language, and the `re` module applies it.

```python
import re

print(re.findall(r"\d+", "Order 17 shipped on day 3"))
```

**Output:**

```
['17', '3']
```

`\d+` means "one or more digits". No string method can express that.

## Raw Strings

Patterns are written as raw strings — a normal string with `r` in front:

```python
print("a\tb")
print(r"a\tb")
```

**Output:**

```
a	b
a\tb
```

Without the `r`, Python interprets the backslash first and the pattern never sees it. Since patterns are full of backslashes, write every one as `r"..."` and the question never arises.

## Searching

`re.search()` looks for the pattern anywhere in the string. It returns a **match object** when it finds one, and `None` when it does not:

```python
import re

result = re.search(r"\d+", "Room 214 is free")
print(result.group())
print(result.start())

print(re.search(r"\d+", "No numbers here"))
```

**Output:**

```
214
5
None
```

`group()` gives the matched text and `start()` gives its position. Because a failed search returns `None`, the result is tested before it is used:

```python
import re

line = "Temperature: 31 degrees"
found = re.search(r"\d+", line)

if found:
    print("Number found:", found.group())
else:
    print("No number in that line.")
```

**Output:**

```
Number found: 31
```

Calling `group()` on `None` raises `AttributeError`, which is the usual first mistake with this module.

## What the Symbols Mean

A pattern is ordinary characters, which match themselves, mixed with special ones.

These match one character of a kind:

| Pattern | Matches |
| --- | --- |
| `\d` | one digit |
| `\w` | one letter, digit or underscore |
| `\s` | one space, tab or newline |
| `.` | any character except a newline |
| `[aeiou]` | any one character listed |
| `[a-z]` | any one character in the range |
| `[^0-9]` | any one character *not* listed |

```python
import re

print(re.findall(r"[aeiou]", "programming"))
print(re.findall(r"[A-Z]", "Anita and Ravi Met"))
print(re.findall(r"\w", "a-b c"))
```

**Output:**

```
['o', 'a', 'i']
['A', 'R', 'M']
['a', 'b', 'c']
```

These say how many times the thing before them repeats:

| Pattern | Means |
| --- | --- |
| `*` | zero or more |
| `+` | one or more |
| `?` | zero or one |
| `{3}` | exactly three |
| `{2,4}` | two to four |

```python
import re

print(re.findall(r"\d+", "a1 b22 c333"))
print(re.findall(r"\d{3}", "a1 b22 c333"))
print(re.findall(r"colou?r", "color and colour"))
```

**Output:**

```
['1', '22', '333']
['333']
['color', 'colour']
```

`\d+` took each run of digits whole rather than one digit at a time, because a quantifier takes as much as it can.

And these fix the position rather than matching a character:

| Pattern | Means |
| --- | --- |
| `^` | start of the string |
| `$` | end of the string |

```python
import re

print(bool(re.search(r"^Total", "Total: 91")))
print(bool(re.search(r"^Total", "Subtotal: 91")))
print(bool(re.search(r"\d$", "Total: 91")))
```

**Output:**

```
True
False
True
```

## Matching the Whole String

Validation asks whether the *entire* entry has the right shape, not whether some part of it does. `re.fullmatch()` asks that question:

```python
import re

print(bool(re.fullmatch(r"\d{6}", "560001")))
print(bool(re.fullmatch(r"\d{6}", "5600012")))
print(bool(re.search(r"\d{6}", "5600012")))
```

**Output:**

```
True
False
True
```

The last line is the trap: `search()` found six digits inside a seven-digit entry and reported success. Use `fullmatch()` to validate and `search()` to find.

```python
import re

def valid_email(text):
    return bool(re.fullmatch(r"[\w.]+@[\w]+\.[a-z]{2,}", text))

print(valid_email("anita@example.com"))
print(valid_email("anita@example"))
print(valid_email("not an address"))
```

**Output:**

```
True
False
False
```

That pattern is deliberately simple. Genuinely validating an email address is far harder than it looks, and the practical rule is to check for an obvious shape and let a confirmation message do the rest.

## Finding Every Match

`re.findall()` returns a list of every match, and an empty list when there are none:

```python
import re

log = "GET /home 200, GET /about 404, POST /login 500"
print(re.findall(r"\d{3}", log))
print(re.findall(r"/\w+", log))
```

**Output:**

```
['200', '404', '500']
['/home', '/about', '/login']
```

A list comes back, so every list operation applies to it.

## Capturing Parts of a Match

Parentheses mark a part of the pattern worth keeping on its own. Each pair is a **group**, numbered from one:

```python
import re

entry = "Anita scored 91"
found = re.search(r"(\w+) scored (\d+)", entry)

print(found.group())
print(found.group(1))
print(found.group(2))
```

**Output:**

```
Anita scored 91
Anita
91
```

`group()` with no argument is the whole match; `group(1)` is the first pair of parentheses. This pulls fields out of text with a known layout:

```python
import re

for line in ["16-09-2026", "01-01-2027"]:
    parts = re.search(r"(\d{2})-(\d{2})-(\d{4})", line)
    print(parts.group(3), parts.group(2), parts.group(1))
```

**Output:**

```
2026 09 16
2027 01 01
```

When the pattern has groups, `findall()` returns the groups rather than the whole match:

```python
import re

text = "Anita scored 91, Ravi scored 78"
print(re.findall(r"(\w+) scored (\d+)", text))
```

**Output:**

```
[('Anita', '91'), ('Ravi', '78')]
```

One tuple per match, one item per group. Note that `'91'` is a string — a pattern matches text and returns text, so converting with `int()` is still your job.

## Replacing and Splitting

`re.sub()` is `replace()` for patterns:

```python
import re

print(re.sub(r"\s+", " ", "too   many     spaces"))
print(re.sub(r"\d", "#", "Card 4213 8890"))
```

**Output:**

```
too many spaces
Card #### ####
```

Collapsing runs of whitespace is the common use, and `replace()` cannot do it because the runs differ in length.

`re.split()` is `split()` for patterns, which allows several separators at once:

```python
import re

print(re.split(r"[,;]\s*", "apples, pears;plums, figs"))
```

**Output:**

```
['apples', 'pears', 'plums', 'figs']
```

## Summary of the Functions

| Function | Returns |
| --- | --- |
| `re.search(pattern, text)` | a match object, or `None` |
| `re.fullmatch(pattern, text)` | a match object if the whole string fits, else `None` |
| `re.findall(pattern, text)` | a list of every match |
| `re.sub(pattern, new, text)` | the text with matches replaced |
| `re.split(pattern, text)` | a list of pieces |

Only `search()` and `fullmatch()` return something that needs testing for `None`. The other three always return a list or a string.

## When Not to Use One

A regular expression is compact and hard to read, including for the person who wrote it. The rule is to reach for one only when string methods cannot do the job:

```python
import re

filename = "report.pdf"

print(filename.endswith(".pdf"))
print(bool(re.search(r"\.pdf$", filename)))
```

**Output:**

```
True
True
```

Both work; the first is obviously correct at a glance. Use `in`, `startswith()`, `endswith()`, `split()` and `replace()` where they fit, and a pattern where the target has a shape rather than a spelling.

When you do write one, test it against the strings that should *fail* as well as the ones that should match. A pattern that is slightly too generous matches things you never intended, and that failure is silent.

## Further Reading

- **Official `re` module reference** — https://docs.python.org/3/library/re.html
- **Regular expressions explained step by step** — https://realpython.com/regex-python/
- **Build and test a pattern interactively** — https://regex101.com/

Patterns find and reshape text by its structure. Next, you will build text for display, replacing the comma-separated `print()` calls used so far.
