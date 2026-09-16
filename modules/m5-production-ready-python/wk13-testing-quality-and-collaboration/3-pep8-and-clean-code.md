# PEP 8 and Clean Code

Code is read far more often than it is written — by reviewers, by teammates, and by you in six months with no memory of writing it. Two things make it readable: a consistent **style**, so nothing is surprising, and a clear **structure**, so the intent is visible.

**PEP 8** is Python's official style guide. It settles the questions that have no right answer — spaces or tabs, where to break a line, how to name things — so that all Python looks broadly alike.

Clean code is the harder half, and the part no tool can check.

## Naming

| Thing | Convention | Example |
| --- | --- | --- |
| Variable | `snake_case` | `student_name` |
| Function | `snake_case` | `calculate_total()` |
| Method | `snake_case` | `deposit()` |
| Class | `PascalCase` | `BankAccount` |
| Constant | `UPPER_SNAKE_CASE` | `MAX_RETRIES` |
| Module | short `lowercase` | `utils.py` |
| Internal | leading underscore | `_balance` |

The capital letter on a class is what tells a reader that `BankAccount` is a type and `bank_account` is one of them.

Four names to avoid: `l`, `I` and `O` alone, because they are indistinguishable from `1` and `0` in many fonts; and any name shadowing a built-in. Writing `list = [1, 2]` or `sum = 0` works and then breaks the real `list()` or `sum()` later in the same scope, with a confusing error.

## Layout

**Four spaces per indent level.** Never tabs, and never a mixture — Python raises `TabError` for a mixture.

**Line length up to 79 characters**, per PEP 8. Many projects allow 88 or 100; the rule that matters is that the project agrees on one.

**Two blank lines** between top-level functions and classes, **one blank line** between methods inside a class.

**Imports at the top**, one per line, in three groups separated by blank lines:

```python
import csv
import json

import pandas as pd
import requests

from myproject.models import Student
```

Standard library, then third-party, then your own. Anyone can then see a file's dependencies at a glance.

## Whitespace

```python
# Correct
total = price * quantity
result = calculate(items, tax=0.18)
numbers = [1, 2, 3]
if x == 5 and y > 3:
```

```python
# Wrong
total=price*quantity
result = calculate( items , tax = 0.18 )
numbers = [ 1,2,3 ]
if x==5 and y>3 :
```

The rules: spaces around operators, a space after each comma and none before, no space inside brackets, and no space before a colon.

The exception that catches people: **no spaces around `=` for keyword arguments and defaults** — `tax=0.18`, not `tax = 0.18`. The distinction separates a keyword argument from an assignment.

## Comments

A comment should say **why**, not what. The code already says what.

```python
# Bad: repeats the code
# add one to count
count = count + 1

# Good: explains a decision
# Retry three times; the upstream API drops roughly 1 in 50 connections
for attempt in range(3):
```

The worst comment is one that has become false. Code gets changed and comments do not, so a comment contradicting the code actively misleads. Prefer code that needs no comment: a well-named function beats a comment explaining a badly named one.

Docstrings are different — they document the interface, and `help()` reads them.

## Tools Do the Mechanical Part

Nobody applies PEP 8 by hand. Two kinds of tool do it.

A **formatter** rewrites the code. `black` is the standard:

```
pip install black
black --diff messy.py
```

**Output:**

```
-def CalcTotal( items,TAX=0.18 ):
-    t=0
+
+def CalcTotal(items, TAX=0.18):
+    t = 0
     for i in items:
-        t=t+i["price"]*i["qty"]
-    return t*(1+TAX)
+        t = t + i["price"] * i["qty"]
+    return t * (1 + TAX)
 
-x = CalcTotal([{"price":10,"qty":2}])
+
+x = CalcTotal([{"price": 10, "qty": 2}])
 print(x)
```

`--diff` shows the changes; `black messy.py` applies them. It has almost no options, which is the point — the formatting stops being a matter of opinion, and code review stops containing arguments about spacing.

A **linter** finds problems rather than formatting. `ruff` is fast and covers what older tools like `flake8` and `pylint` do:

```
pip install ruff
ruff check messy.py
```

**Output:**

```
messy.py:1:1: I001 [*] Import block is un-sorted or un-formatted
messy.py:1:8: F401 [*] `sys` imported but unused
messy.py:1:13: F401 [*] `os` imported but unused
Found 3 errors.
[*] 3 fixable with the `--fix` option.
```

Unused imports, unsorted import blocks, undefined names, unreachable code. `ruff check --fix` corrects what it safely can.

Note what the tools did **not** flag: `CalcTotal` is misnamed, `t` and `i` say nothing, and `TAX` is a parameter pretending to be a constant. Tools handle style. Everything below is yours.

## Names Carry the Meaning

```python
def calc(d, r):
    return d * (1 + r) ** 2
```

```python
def compound_amount(deposit, annual_rate):
    """Return the value of a deposit after two years of compounding."""
    return deposit * (1 + annual_rate) ** 2
```

Same code. The second needs no explanation.

A name should say what the thing **is** or what the function **does**. Length is not the measure — `i` is fine as a loop index over three lines, and `d` is not fine as a parameter.

Avoid names that say nothing: `data`, `info`, `temp`, `result`, `process()`, `handle()`, `do_stuff()`. If the honest name is `data`, the code probably does not know what it is working with.

## One Function, One Job

A function that reads a file, filters records, calculates totals and prints a report does four things. It cannot be tested a piece at a time, cannot be reused in part, and must be read entirely to be understood.

```python
def process(path):
    with open(path, encoding="utf-8") as handle:
        rows = list(csv.DictReader(handle))
    passed = [r for r in rows if int(r["mark"]) >= 50]
    total = sum(int(r["mark"]) for r in passed)
    print(f"{len(passed)} passed, average {total / len(passed):.1f}")
```

```python
def load_students(path):
    with open(path, encoding="utf-8") as handle:
        return list(csv.DictReader(handle))

def passing(students, pass_mark=50):
    return [s for s in students if int(s["mark"]) >= pass_mark]

def average_mark(students):
    if not students:
        return 0.0
    return sum(int(s["mark"]) for s in students) / len(students)

def report(path):
    passed = passing(load_students(path))
    print(f"{len(passed)} passed, average {average_mark(passed):.1f}")
```

Longer, and better. Each function is testable on its own, `passing` takes a pass mark instead of assuming one, and `average_mark` handles the empty case. `report` now reads as a summary of what happens.

A useful signal: if describing a function needs the word "and", it is doing two things.

## Magic Numbers

```python
if mark >= 50:
    status = "pass"
if attempts > 3:
    give_up()
```

What is `50`? What is `3`? A reader cannot tell, and neither can a search when the rule changes.

```python
PASS_MARK = 50
MAX_ATTEMPTS = 3

if mark >= PASS_MARK:
    status = "pass"
if attempts > MAX_ATTEMPTS:
    give_up()
```

The name explains the value, and changing it is a one-line edit in one place. `0` and `1` are usually fine bare; almost everything else earns a name.

## Do Not Repeat Yourself

The same logic in three places gets fixed in two of them. When a calculation, a rule or a constant appears more than twice, give it a name and call it.

The caution is not to over-apply it. Two pieces of code that look alike but express **different rules** should stay separate — merging them means the next change to one rule breaks the other. Repeated *knowledge* is the problem, not repeated characters.

## Return Early

Deeply nested conditions are hard to follow, because understanding the innermost line means holding every enclosing condition in your head:

```python
def withdraw(account, amount):
    if account.is_active:
        if amount > 0:
            if amount <= account.balance:
                account.balance -= amount
                return True
            else:
                return False
        else:
            return False
    else:
        return False
```

Handle the failures first and leave:

```python
def withdraw(account, amount):
    if not account.is_active:
        return False
    if amount <= 0:
        return False
    if amount > account.balance:
        return False

    account.balance -= amount
    return True
```

The conditions are now a list of reasons to refuse, and the real work sits unindented at the end. These are **guard clauses**, and they are the standard cure for nesting.

## The Standard to Aim For

**Make it work, then make it clean.** Get it correct first; tidying code that does the wrong thing is wasted effort.

**Run the formatter and the linter automatically.** In your editor on save, or as a pre-commit hook, so style never reaches review.

**Leave code better than you found it.** Rename one bad variable, extract one long function. Small improvements while passing through beat a cleanup project that never happens.

**Write for the next reader.** Usually you. The clever one-liner you are proud of today is the line you cannot decipher in March.

## Further Reading

- **PEP 8 — Style Guide for Python Code** — https://peps.python.org/pep-0008/
- **The ruff documentation** — https://docs.astral.sh/ruff/

PEP 8 settles style and tools enforce it; naming, small functions, named constants and guard clauses are the part that needs judgement. Next, recording what a program did while it runs.
