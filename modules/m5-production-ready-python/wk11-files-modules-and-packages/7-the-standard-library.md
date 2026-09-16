# The Standard Library

Python ships with a large collection of modules that need no installation. The phrase used for this is *batteries included*, and it is the reason a Python program can read a CSV, parse JSON, handle dates, and talk to the filesystem without depending on anything external.

`json`, `csv` and `pathlib` are already familiar. This chapter is a tour of the rest of what is worth knowing early.

The point is not to memorise any of it. It is to know what exists, so that the next time you are about to write something fiddly by hand you check first.

## math

Mathematics beyond the arithmetic operators:

```python
import math

print(math.sqrt(16))
print(math.floor(3.7), math.ceil(3.2))
print(math.pi)
print(round(math.pi, 4))
print(math.factorial(5), math.gcd(12, 18))
```

**Output:**

```
4.0
3 4
3.141592653589793
3.1416
120 6
```

Note that `math.sqrt(16)` returns `4.0`, a float. Most of `math` does.

The most useful function in the module solves a problem met much earlier — floating-point comparison:

```python
import math

print(0.1 + 0.2 == 0.3)
print(math.isclose(0.1 + 0.2, 0.3))
```

**Output:**

```
False
True
```

`isclose()` is the correct way to compare floats. `==` on computed floats is almost always a bug waiting for the right input.

## random

Random numbers and random choices:

```python
import random

random.seed(42)

print(random.random())
print(random.randint(1, 6))
print(random.choice(["Anita", "Ravi", "Meera"]))
print(random.sample(range(1, 50), 6))

deck = ["A", "K", "Q", "J"]
random.shuffle(deck)
print(deck)
```

**Output:**

```
0.6394267984578837
1
Meera
[18, 16, 15, 9, 7, 44]
['J', 'K', 'Q', 'A']
```

| Function | Gives |
| --- | --- |
| `random()` | a float from 0 up to 1 |
| `randint(a, b)` | a whole number from `a` to `b`, both included |
| `choice(seq)` | one item from a sequence |
| `sample(seq, k)` | `k` different items |
| `shuffle(seq)` | rearranges a list in place |

`randint()` is unusual in including its upper bound — unlike `range()`, which excludes it.

`seed()` makes the sequence reproducible. The same seed gives the same numbers every run, which is what makes the output above stable and what makes randomised code testable. Leave it out in real use.

`random` is **not** suitable for passwords, tokens or anything security-related. The `secrets` module is, and has a similar interface.

## datetime

Dates and times are harder than they look, and this module handles the hard parts.

```python
from datetime import date, datetime, timedelta

day = date(2026, 9, 16)
print(day)
print(day.year, day.month, day.day)
print(day.strftime("%A %d %B %Y"))
```

**Output:**

```
2026-09-16
2026 9 16
Wednesday 16 September 2026
```

`strftime()` formats a date as a string, using codes for each part:

| Code | Means |
| --- | --- |
| `%Y` | four-digit year |
| `%m` | month as a number |
| `%B` | month name |
| `%d` | day of the month |
| `%A` | weekday name |
| `%H`, `%M`, `%S` | hours, minutes, seconds |

```python
from datetime import datetime

moment = datetime(2026, 9, 16, 14, 30, 5)
print(moment)
print(moment.strftime("%d/%m/%Y"))
```

**Output:**

```
2026-09-16 14:30:05
16/09/2026
```

`strptime()` goes the other way, parsing a string using the same codes:

```python
from datetime import datetime

parsed = datetime.strptime("16-09-2026", "%d-%m-%Y")
print(parsed)
```

**Output:**

```
2026-09-16 00:00:00
```

The two names are easy to confuse. **`strftime` formats; `strptime` parses.**

`timedelta` is a span of time, and dates support arithmetic:

```python
from datetime import date, timedelta

day = date(2026, 9, 16)
print(day + timedelta(days=30))

gap = date(2026, 12, 25) - day
print(gap.days)
print(day < date(2026, 12, 25))
```

**Output:**

```
2026-10-16
100
True
```

Subtracting two dates gives a `timedelta`; adding one to a date gives a date. Month lengths and leap years are handled for you, which is the whole reason not to do this arithmetic by hand.

`date.today()` and `datetime.now()` give the current moment. They are not used in the examples here because their output would differ every run.

## collections

Four specialised containers, each solving a problem the built-in types make awkward.

**`Counter`** counts things:

```python
from collections import Counter

words = "the cat sat on the mat the end".split()
counts = Counter(words)

print(counts)
print(counts["the"])
print(counts["missing"])
print(counts.most_common(2))
```

**Output:**

```
Counter({'the': 3, 'cat': 1, 'sat': 1, 'on': 1, 'mat': 1, 'end': 1})
3
0
[('the', 3), ('cat', 1)]
```

The counting loop with `get(word, 0) + 1` becomes one call. A missing key gives `0` rather than raising, and `most_common()` answers the question that usually follows.

**`defaultdict`** supplies a value for a missing key automatically:

```python
from collections import defaultdict

grouped = defaultdict(list)
for name, course in [("Anita", "CS"), ("Ravi", "Maths"), ("Meera", "CS")]:
    grouped[course].append(name)

print(dict(grouped))
```

**Output:**

```
{'CS': ['Anita', 'Meera'], 'Maths': ['Ravi']}
```

`defaultdict(list)` creates an empty list the first time each key is used, so grouping needs no `setdefault()` and no check. `defaultdict(int)` starts counters at zero.

It is passed the **function** that makes the default — `list`, not `list()`.

**`namedtuple`** is a tuple whose positions have names:

```python
from collections import namedtuple

Point = namedtuple("Point", ["x", "y"])
location = Point(3, 5)

print(location)
print(location.x, location.y)
print(location[0])

x, y = location
print(x, y)
```

**Output:**

```
Point(x=3, y=5)
3 5
3
3 5
```

Everything a tuple does, plus readable access and a useful `repr`. It fits where a class would be all data and no behaviour.

**`deque`** is a list that is fast at both ends:

```python
from collections import deque

queue = deque([1, 2, 3])
queue.append(4)
queue.appendleft(0)
print(queue)
print(queue.popleft())
print(queue)
```

**Output:**

```
deque([0, 1, 2, 3, 4])
0
deque([1, 2, 3, 4])
```

`list.pop(0)` has to shift every remaining item along; `deque.popleft()` does not. Use a deque for a queue.

## statistics

Averages without writing them out:

```python
import statistics

marks = [78, 91, 64, 88, 91]
print(statistics.mean(marks))
print(statistics.median(marks))
print(statistics.mode(marks))
print(round(statistics.stdev(marks), 2))
```

**Output:**

```
82.4
88
91
11.59
```

## os and sys

`os` deals with the operating system, and `sys` with the running interpreter:

```python
import os
import sys

print(os.name)
print(sys.platform)
print(sys.version_info.major, sys.version_info.minor)
print(os.environ.get("NOT_SET_ANYWHERE", "default"))
```

**Output:**

```
nt
win32
3 13
default
```

`os.environ` is a dictionary of environment variables, and `get()` with a default is how a program reads optional configuration — an API key, a database URL — without crashing when it is absent.

`sys.argv` holds the command-line arguments, and `sys.exit()` ends the program with a status code.

For paths, prefer `pathlib` over the older `os.path`. For creating and deleting files and directories, `os` and `shutil` are still the tools.

## itertools

Tools for looping, all returning iterators:

```python
import itertools

print(list(itertools.combinations(["a", "b", "c"], 2)))
print(list(itertools.permutations(["a", "b"], 2)))
print(list(itertools.chain([1, 2], [3, 4])))
print(list(itertools.islice(itertools.count(10), 4)))
```

**Output:**

```
[('a', 'b'), ('a', 'c'), ('b', 'c')]
[('a', 'b'), ('b', 'a')]
[1, 2, 3, 4]
[10, 11, 12, 13]
```

`combinations` ignores order, `permutations` does not. `chain` treats several sequences as one. `count` is endless, which is why `islice` is there to take a finite piece of it.

## re

Regular expressions, for patterns too complicated for string methods:

```python
import re

print(re.findall(r"\d+", "a1 b22 c333"))
print(re.sub(r"\s+", " ", "too   many    spaces"))
print(bool(re.fullmatch(r"[a-z]+@[a-z]+\.com", "anita@example.com")))
```

**Output:**

```
['1', '22', '333']
too many spaces
True
```

Regular expressions are a language of their own and are worth learning later. The rule for now: if `split()`, `replace()`, `startswith()` and `in` can do the job, use them — they are far easier to read.

## Worth Knowing Exists

| Module | For |
| --- | --- |
| `argparse` | command-line arguments, properly |
| `sqlite3` | a database in a single file |
| `unittest` | testing, built in |
| `logging` | recording what a program did |
| `time` | timing and delays |
| `shutil` | copying, moving and deleting files |
| `subprocess` | running other programs |
| `urllib.request` | fetching a URL without installing anything |
| `secrets` | randomness safe for passwords and tokens |
| `dataclasses` | classes that are mostly data |
| `typing` | type hints |
| `zipfile`, `tarfile` | archives |

## Finding Things

Three habits find what you need.

**Search the module index** at https://docs.python.org/3/py-modindex.html before writing anything fiddly. Dates, paths, counting, combinations and file formats are all solved already.

**Use `help()` and `dir()`** at the interactive prompt. `dir(math)` lists what a module contains, and `help(math.isclose)` explains one thing.

**Check the standard library before installing anything.** A dependency is a cost — to install, to keep updated, and to trust. The standard library is already there, is maintained alongside the language, and will still work in ten years.

## Further Reading

- **The Python standard library** — https://docs.python.org/3/library/
- **The module index** — https://docs.python.org/3/py-modindex.html

`math`, `random`, `datetime`, `collections` and the rest cover an enormous amount of ordinary work with no installation. That completes what Python provides on its own — next, adding what it does not.
