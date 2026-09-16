# Reading Documentation

There are hundreds of thousands of packages on PyPI and nobody has memorised any but a few. The skill that matters is not knowing libraries — it is finding out how one works, quickly and from a source that is correct.

Documentation is that source. Tutorials go stale, forum answers describe versions from years ago, and an AI assistant will confidently describe functions that do not exist. The official documentation describes the version you actually installed.

## help()

`help()` prints a function's documentation, and works at the interactive prompt or in a script:

```python
help(round)
```

**Output:**

```
Help on built-in function round in module builtins:

round(number, ndigits=None)
    Round a number to a given precision in decimal digits.

    The return value is an integer if ndigits is omitted or None.  Otherwise
    the return value has the same type as the number.  ndigits may be negative.
```

Four facts in five lines: the parameters, that `ndigits` is optional, that omitting it returns an `int` while supplying it preserves the type, and that it may be negative. Reading that takes ten seconds and answers more than a search would.

It works on methods too:

```python
help(str.strip)
```

**Output:**

```
Help on method_descriptor:

strip(self, chars=None, /) unbound builtins.str method
    Return a copy of the string with leading and trailing whitespace removed.

    If chars is given and not None, remove characters in chars instead.
```

`help()` accepts a function, a method, a class, a module or an object. Note that the function is passed **without brackets** — `help(round)`, not `help(round())`.

## dir()

`dir()` lists what something contains. It answers "what can this do?" where `help()` answers "how does this one thing work?":

```python
import math
print([name for name in dir(math) if not name.startswith("_")][:12])
```

**Output:**

```
['acos', 'acosh', 'asin', 'asinh', 'atan', 'atan2', 'atanh', 'cbrt', 'ceil', 'comb', 'copysign', 'cos']
```

The filter removes the dunder names, which are rarely what you are looking for.

`dir()` on an object of your own lists its attributes and methods, which makes it a quick way to inspect something unfamiliar returned by a library.

The pair works well together: `dir()` to find the name, `help()` to learn the name.

## Reading a Signature

The first line of any documentation is the **signature**, and it encodes more than it first appears:

```python
import inspect
print(inspect.signature(sorted))
```

**Output:**

```
(iterable, /, *, key=None, reverse=False)
```

Four things are being said:

- `iterable` is required.
- `/` means everything before it is **positional-only** — it cannot be passed by name.
- `*` means everything after it is **keyword-only** — it must be passed by name.
- `key` and `reverse` have defaults, so both are optional.

So `sorted(marks, reverse=True)` is correct and `sorted(marks, True)` is not, and the signature said so before you tried it.

A longer one:

```python
import inspect
print(inspect.signature(open))
```

**Output:**

```
(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)
```

Only `file` is required. Everything else has a default, which is why `open("notes.txt")` works — and it shows that `mode` defaults to `'r'` and `encoding` defaults to `None`, meaning the platform default rather than UTF-8.

| Symbol | Means |
| --- | --- |
| `name` | required |
| `name=value` | optional, with that default |
| `/` | arguments before this are positional-only |
| `*` | arguments after this are keyword-only |
| `*args` | any number of positional arguments |
| `**kwargs` | any number of keyword arguments |

## The Official Python Documentation

`docs.python.org` has four parts worth knowing apart:

- **The Tutorial** — a guided introduction, read in order.
- **The Library Reference** — every standard library module. This is the part you return to.
- **The Language Reference** — how the language itself is defined. Rarely needed.
- **The HOWTOs** — focused guides on sorting, logging, regular expressions and more.

Two entry points do most of the work. The **module index** at https://docs.python.org/3/py-modindex.html lists every module alphabetically — scan it before writing something fiddly. The **search box** is reliable when you know the name of the thing you want.

Check the version selector in the corner. The documentation defaults to the newest release, which may describe features your Python does not have.

## Third-Party Documentation

Library documentation is less consistent. Most well-maintained projects have:

- a **quickstart** — the first thing to read, usually enough to begin
- an **API reference** — every function, generated from the code
- a **user guide** — longer explanations of concepts
- a **changelog** — what changed between versions, which is where to look when working code stops working

The quickstart and the changelog are the two people skip and should not.

Find the documentation from PyPI. Every package page links to its home page and documentation, which is safer than a search that may land on an old mirror.

## Writing Documentation Others Can Read

The same tools work on your own code, if you write docstrings:

```python
def apply_discount(price, percent=10, *, round_to=2):
    """Return price reduced by percent, rounded to round_to decimals.

    Args:
        price: the original price.
        percent: the discount percentage. Defaults to 10.
        round_to: decimal places in the result. Defaults to 2.

    Returns:
        The discounted price as a float.
    """
    return round(price * (1 - percent / 100), round_to)

help(apply_discount)
```

**Output:**

```
Help on function apply_discount in module __main__:

apply_discount(price, percent=10, *, round_to=2)
    Return price reduced by percent, rounded to round_to decimals.

    Args:
        price: the original price.
        percent: the discount percentage. Defaults to 10.
        round_to: decimal places in the result. Defaults to 2.

    Returns:
        The discounted price as a float.
```

Python generated the signature line itself. The docstring supplied the rest.

Note the `*` in that signature, which makes `round_to` keyword-only:

```python
print(apply_discount(200))
print(apply_discount(200, 25))
print(apply_discount(200, 25, round_to=0))
apply_discount(200, 25, 0)
```

**Output:**

```
180.0
150.0
150.0
TypeError: apply_discount() takes from 1 to 2 positional arguments but 3 were given
```

The `*` forced the third argument to be named. That is worth doing for any parameter whose meaning would be unclear as a bare value — `apply_discount(200, 25, 0)` gives the reader no idea what `0` is.

## Answering a Question from the Docs

A worked example. The question: *how do I sort a list of dictionaries by one field, case-insensitively?*

**Start with `help()`.** `help(sorted)` gives the signature and says `key` specifies a function of one argument used to extract a comparison key.

**Follow to the HOWTO.** The sorting HOWTO at https://docs.python.org/3/howto/sorting.html covers exactly this, with examples of key functions.

**Combine what both said.** A key function returning `record["name"].lower()` sorts by name, ignoring case.

```python
students = [{"name": "ravi"}, {"name": "Anita"}, {"name": "meera"}]
print(sorted(students, key=lambda student: student["name"].lower()))
```

**Output:**

```
[{'name': 'Anita'}, {'name': 'meera'}, {'name': 'ravi'}]
```

No search engine was involved, and the answer is correct for the installed version.

## Where Else to Look

**Official documentation** is authoritative and matches your version. Start here.

**Stack Overflow** is good for "why does this error happen", and every answer has a date. An accepted answer from 2013 may describe Python 2. Check the date, check the votes, check the comments — the correction is often in the comments rather than the answer.

**AI assistants** are fast and confidently wrong about specifics. They invent plausible function names and parameters that do not exist. Anything they tell you about a library must be checked against `help()` or the documentation before you rely on it.

**The source code** is the final word, and more readable than people expect. When documentation is ambiguous, the standard library's own source resolves it.

The order that works: `help()` first because it is instant and correct, documentation second for anything wider, search engines third for problems you cannot phrase as a lookup.

## Further Reading

- **The Python documentation** — https://docs.python.org/3/
- **The module index** — https://docs.python.org/3/py-modindex.html

`help()` and `dir()` answer most questions without leaving the interpreter, and a signature says which arguments are required, optional and keyword-only. Next, the kind of library that talks to another computer.
