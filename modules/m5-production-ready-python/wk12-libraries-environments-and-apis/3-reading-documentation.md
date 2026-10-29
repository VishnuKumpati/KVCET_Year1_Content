# Reading Documentation

Every library contains far more functions, classes and options than any programmer can remember. Professional programmers do not memorise them. They look up how something works at the moment they need it, and they look it up in the **documentation**.

> **Documentation is the written reference that explains what a library, module, class or function does and how to use it correctly. It allows programmers to use code without reading its source code.**

A new phone or washing machine comes with a user manual that explains each button and setting. Documentation does the same job for code. Reading it is a core programming skill: it is how you learn a new library, find the correct way to call a function, and understand why something went wrong.

## Terms Used in Documentation

Documentation is written using precise programming terms. You have met most of these terms in earlier topics. This table collects them in one place, because documentation uses all of them together.

| Term | Meaning | Example |
| --- | --- | --- |
| **Function** | A named block of code that performs a task when it is called | `round()` |
| **Method** | A function that belongs to an object, called using a dot after the object | `marks.index(92)` |
| **Parameter** | A name listed in a function's definition that receives a value | In `round(number, ndigits=None)`, `number` and `ndigits` are parameters |
| **Argument** | The actual value passed to a parameter when the function is called | In `round(3.14159, 2)`, `3.14159` and `2` are arguments |
| **Positional argument** | An argument matched to a parameter by its **position** in the call | `round(3.14159, 2)` |
| **Keyword argument** | An argument matched to a parameter by its **name**, written as `name=value` | `round(3.14159, ndigits=2)` |
| **Default value** | The value a parameter uses when no argument is passed for it | `ndigits=None` |
| **Optional parameter** | A parameter with a default value, so passing an argument for it is not required | `ndigits` |
| **Return value** | The value a function sends back to the code that called it | `round(3.14159, 2)` returns `3.14` |
| **Exception** | An event, represented by an object, that interrupts the normal flow of a program unless it is handled | `ValueError` |
| **Signature** | The function's name and parameters, including any default values and argument restrictions | `round(number, ndigits=None)` |

The difference between a **parameter** and an **argument** matters in documentation: the documentation lists **parameters**, and you supply **arguments** when you call the function.

## Where to Find Documentation

There are three main places to find documentation.

| Source | What It Contains | When to Use It |
| --- | --- | --- |
| **The official documentation website** | Complete, detailed documentation of every module, function and class | When you need full details, examples and notes |
| **The library's PyPI page** | A description of an external library, its installation command and links to its full documentation | When you start using an external library |
| **The built-in `help()` function** | The documentation of a function, class or module, shown inside Python | When you need a quick reminder while writing code |

**Start with the official documentation.** It is written by the people who build the library, and it is kept up to date with each version.

### Finding the Official Python Documentation

1. Open **https://docs.python.org/3/** in your browser.
2. Check the **version selector**, usually near the top of the page. Website layouts can change, so its exact position may differ. Choose the version that matches your Python installation. You can find your version by running `python --version` in the terminal.
3. Open the **Library Reference**. It documents the **built-in functions** and every module in the **standard library**.
4. Use the **search box** to look up a name, such as `round` or `random`.
5. On a long page, press **Ctrl+F** (or **Cmd+F** on macOS) to find a word on that page.

### Finding an External Library's Documentation

External libraries publish their documentation on their own websites. Their **PyPI page** links to it. For example, the PyPI page of `requests`, the library used later this week, is:

```
https://pypi.org/project/requests/
```

| Part of the PyPI Page | What It Shows |
| --- | --- |
| **Installation command** | The `pip install` command for the library |
| **Project description** | What the library does, often with a short example |
| **Project links**, such as **Documentation** | Links to the library's full documentation website |
| **Release history** | The available versions and when each was released |

## How a Module's Documentation Page Is Organised

The official documentation groups related information into pages. Each standard library module usually has its own page. For example, the page for the `random` module, at https://docs.python.org/3/library/random.html, is organised like most module pages:

| Part of the Page | What It Contains |
| --- | --- |
| **Title and introduction** | What the module is for |
| **Grouped sections** | Related functions grouped together, such as functions for integers and functions for sequences |
| **Entries** | One entry for each function, class or constant, starting with its signature |
| **Notes and warnings** | Highlighted boxes with important details or limitations |
| **Examples** | Sample code showing the module in use, often near the end of the page |

To find a specific function on such a page, use **Ctrl+F** with the function's name.

## Reading a Function Entry, Line by Line

Every function entry follows the same pattern: a **signature**, followed by a **description**. This is the official entry for `round()`:

```
round(number, ndigits=None)
    Return number rounded to ndigits precision after the decimal point.
    If ndigits is omitted or is None, it returns the nearest integer to its input.
```

Read it one part at a time:

| Part | In This Entry | What It Tells You |
| --- | --- | --- |
| **Function name** | `round` | The name to call |
| **First parameter** | `number` | It has no default value, so an argument is **required** |
| **Second parameter** | `ndigits=None` | It has a default value of `None`, so it is **optional** |
| **Return value** | "Return number rounded to…" | The function returns the rounded number. When `ndigits` is omitted or `None`, it returns an integer |
| **Behaviour of the default** | "If ndigits is omitted or is None…" | Without `ndigits`, it returns the nearest **integer** |

The entry predicts exactly what the code will do:

```python
print(round(3.14159))
print(round(3.14159, 2))
print(round(3.14159, ndigits=2))
```

**Output:**

```
3
3.14
3.14
```

- `round(3.14159)` passes only `number`. `ndigits` uses its default value, `None`, so the result is the nearest integer, `3`. Python rounds exact halfway cases to the nearest **even** value. For example, `round(2.5)` returns `2`, while `round(3.5)` returns `4`.
- `round(3.14159, 2)` passes `2` as a **positional argument** for `ndigits`.
- `round(3.14159, ndigits=2)` passes the same value as a **keyword argument**. The result is the same.

## Reading Special Symbols in a Signature

Some signatures contain symbols that are not parameters. They describe **how** arguments must be passed. You will see them often in the documentation and in `help()`.

In these signatures, a standalone `/` or `*` marks a **boundary** between parameters with different argument-passing rules. Square brackets are a documentation convention, not Python syntax.

| Symbol | Meaning | Example |
| --- | --- | --- |
| `/` | Parameters **before** `/` are **positional-only**: they must be passed by position, not by name | `len(obj, /)` |
| `*` | Parameters **after** `*` are **keyword-only**: they must be passed as keyword arguments | `sorted(iterable, /, *, key=None, reverse=False)` |
| `[ ]` | In this documentation notation, square brackets enclose **optional arguments**; nested brackets mean those arguments are independently optional | `list.index(value[, start[, stop]])` |

The official signature of `sorted()` uses both `/` and `*`:

```
sorted(iterable, /, *, key=None, reverse=False)
```

- `iterable` comes before `/`, so it must be passed **by position**.
- `key` and `reverse` come after `*`, so they must be passed **by name**.
- Both have default values, so both are optional.

```python
print(sorted([3, 1, 2]))
print(sorted([3, 1, 2], reverse=True))
```

**Output:**

```
[1, 2, 3]
[3, 2, 1]
```

Writing `sorted([3, 1, 2], True)` would raise a **`TypeError`**, because `reverse` is **keyword-only** and cannot be passed by position. The signature tells you this before you run the code.

## Reading the Description Carefully

The description often contains small details that change how the function behaves. The official entry for `random.randint()` says:

```
random.randint(a, b)
    Return a random integer N such that a <= N <= b.
```

The condition `a <= N <= b` means the result `N` can be **equal to** `a` or `b`. So `random.randint(1, 6)` can return `1` or `6`, not only the numbers between them. A reader who skips this detail may expect the wrong range of results.

## Reading About Exceptions

Many entries state which **exception** a function **raises** when it cannot do its task. The official entry for the list method `index()` says:

```
list.index(value[, start[, stop]])
    Return zero-based index of the first occurrence of value in the list.
    Raises a ValueError if there is no such item.
```

- The notation `[, start[, stop]]` shows that you can optionally supply `start` and `stop` when calling `index()`.
- "**zero-based index**" means the first position is `0`.
- "**Raises a ValueError** if there is no such item" tells you what happens when the value is missing.

```python
marks = [85, 92, 78]
print(marks.index(92))
```

**Output:**

```
1
```

`92` is at position `1`, because counting starts at `0`. Calling `marks.index(50)` would raise a **`ValueError`**, exactly as the entry says, because `50` is not in the list. Knowing this in advance lets you handle the exception with `try` and `except`, as covered in **File Paths and Errors**.

## Getting Help Inside Python

The built-in **`help()`** function displays the documentation of a function, method, class or module without leaving Python.

```python
help(round)
```

**Possible output (shortened):**

```
Help on built-in function round in module builtins:

round(number, ndigits=None)
    Round a number to a given precision in decimal digits.
```

- Pass the function **itself**, without parentheses: `help(round)`, not `help(round())`. With parentheses, Python would call `round()` first, which raises a `TypeError` because the required argument `number` is missing.
- The output commonly includes the function's **signature** and **description**, depending on the documentation available for that object.
- The wording can differ slightly from the website and between Python versions.

`help()` also works with modules and methods. For example, after `import random`, `help(random)` shows the documentation of the whole module, and `help(list.index)` shows the documentation of the `index()` method.

## Writing Documentation for Your Own Functions

The text that `help()` displays comes from **docstrings**. You can write docstrings for your own functions in exactly the same way.

> **A docstring is a string literal written as the first statement inside a function, class or module. It documents what that code does, and Python makes it available through `help()`.**

Docstrings are commonly written with triple quotes, `"""`. Triple quotes also make it easy to write descriptions across multiple lines.

```python
def greet(name):
    """Return a greeting message for the given name."""
    return "Hello, " + name

help(greet)
```

**Output:**

```
Help on function greet in module __main__:

greet(name)
    Return a greeting message for the given name.
```

- `help(greet)` shows the function's **signature**, `greet(name)`, and its **docstring**.
- `__main__` is the name Python gives to the file you run directly.

A good docstring starts with a short sentence describing what the function does. This makes its purpose clear when someone reads the code or uses `help()`.

## A Routine for Using Any New Function

1. **Find the entry** in the official documentation, or use `help()`.
2. **Read the signature.** Identify the required parameters, the optional parameters and their default values, and any `/` or `*` symbols.
3. **Read the description.** Find out what the function returns and any special behaviour.
4. **Check for exceptions, notes and examples.**
5. **Try a small example** to confirm that the function behaves as the documentation says.

### Common Mistakes When Reading Documentation

| Mistake | Why It Causes Problems | What to Do Instead |
| --- | --- | --- |
| Reading documentation for a different Python version | A function may behave differently, or may not exist, in your version | Select your version on the documentation website |
| Skipping the description after reading the signature | Important details, such as whether a range includes its end values, are missed | Read the whole entry |
| Ignoring the default values | A function may behave differently from what you expect when an argument is left out | Check what each optional parameter defaults to |
| Writing `help(round())` | Python calls `round()` first, and the call fails | Pass the function itself: `help(round)` |
| Ignoring the exceptions listed | The program may stop unexpectedly | Plan how to handle each listed exception |

## Points to Remember

- **Documentation** explains what code does and how to use it. Start with the official documentation for your Python version.
- **A signature** shows the function's name and its parameters. Parameters with **default values** are optional.
- **`/`** marks positional-only parameters, and **`*`** marks keyword-only parameters.
- **`help()`** displays documentation inside Python, and **docstrings** let you document your own functions.

## Further Reading

- 📎 **Official Python documentation for built-in functions** — https://docs.python.org/3/library/functions.html
- 📎 **Official Python tutorial on documentation strings** — https://docs.python.org/3/tutorial/controlflow.html#documentation-strings
- 📎 **Official Python glossary**, the meaning of programming terms used in the documentation — https://docs.python.org/3/glossary.html

Many libraries connect a program to services on the internet, such as weather or payment services. In the next topic, **Web APIs and API Keys**, you'll learn how programs communicate with these services.
