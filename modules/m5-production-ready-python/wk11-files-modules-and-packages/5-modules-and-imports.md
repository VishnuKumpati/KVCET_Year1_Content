# Modules and Imports

Everything so far has lived in one file. That stops working somewhere around a few hundred lines: finding anything means scrolling, unrelated code sits side by side, and nothing can be reused by a second program.

A **module** is a `.py` file. Splitting a program across several of them, and using `import` to connect them, is how any real project is organised.

`import` is already familiar — `import json`, `import csv`, `from pathlib import Path`. Those are modules too. The only new thing here is writing your own.

## A Module Is a File

Create `greetings.py`:

```python
GREETING = "Hello"

def greet(name):
    return f"{GREETING}, {name}"

def shout(name):
    return greet(name).upper()
```

That is a complete module. The filename without `.py` is its name.

In another file in the same directory, `main.py`:

```python
import greetings

print(greetings.greet("Anita"))
print(greetings.GREETING)
```

**Output:**

```
Hello, Anita
Hello
```

`import greetings` makes the module available, and `greetings.greet` reaches inside it. The dot works the way it does on any object — a module is an object, and its contents are its attributes.

## The Forms of import

**`import module`** brings in the module, and its contents are reached through the name:

```python
import greetings
print(greetings.greet("Anita"))
```

**`from module import name`** brings in specific names directly:

```python
from greetings import greet, GREETING

print(greet("Ravi"))
print(GREETING)
```

**Output:**

```
Hello, Ravi
Hello
```

No prefix now. `greet` is a name in this file like any other.

**`as`** renames, in either form:

```python
import greetings as g
from greetings import greet as say_hello

print(g.shout("Meera"))
print(say_hello("Meera"))
```

**Output:**

```
HELLO, MEERA
Hello, Meera
```

This is what `import pandas as pd` is doing — a long name shortened by convention.

**`from module import *`** brings in everything:

```python
from greetings import *
```

Avoid it. A reader cannot tell which names came from where, and the module can silently overwrite names already defined — including ones from another `import *` above it. The explicit forms cost a few characters and say what they did.

| Form | Reach names as | Use when |
| --- | --- | --- |
| `import module` | `module.name` | the prefix aids the reader |
| `from module import name` | `name` | a few names, used often |
| `import module as alias` | `alias.name` | the module name is long |
| `from module import *` | `name` | never |

`import module` is the safer default. `greetings.greet(...)` says where `greet` came from; a bare `greet(...)` does not.

## Importing Runs the File

An import executes the module top to bottom. Add a print to `greetings.py`:

```python
print("-- greetings.py is running")

GREETING = "Hello"

def greet(name):
    return f"{GREETING}, {name}"
```

Then import it twice:

```python
import greetings

print(greetings.greet("Anita"))

import greetings

print("second import done")
```

**Output:**

```
-- greetings.py is running
Hello, Anita
second import done
```

The message appeared **once**, despite two imports. Python caches modules in `sys.modules`: the first import runs the file and stores the result, and every later import of the same name reuses it.

```python
import sys

print("greetings" in sys.modules)
import greetings
print("greetings" in sys.modules)
```

**Output:**

```
False
True
```

Two consequences follow.

**Top-level code runs on import.** A module that prints, reads a file or asks for input does so the moment anything imports it. A module should define things; it should not *do* things at the top level.

**Editing a module mid-session has no effect.** In a long-running interactive session, re-importing will not pick up changes, because the cached copy is returned. Restart instead.

## The Main Guard

A file can be both a module to import and a program to run. `__name__` distinguishes the two.

Python sets `__name__` to the module's name on import, and to `"__main__"` when the file is run directly:

```python
print("-- greetings.py is running")

GREETING = "Hello"

def greet(name):
    return f"{GREETING}, {name}"

print("-- __name__ is", __name__)

if __name__ == "__main__":
    print("-- running greetings.py directly")
    print(greet("Anita"))
```

Run it directly:

```
python greetings.py
```

**Output:**

```
-- greetings.py is running
-- __name__ is __main__
-- running greetings.py directly
Hello, Anita
```

Import it from `main.py` instead:

```
python main.py
```

**Output:**

```
-- greetings.py is running
-- __name__ is greetings
Hello, Anita
Hello
```

Same file, different `__name__`, and the guarded block ran only in the first case.

`if __name__ == "__main__":` is the standard way to separate a file's definitions from what it does when run. Put the definitions at the top level and the program underneath the guard:

```python
def main():
    print(greet("Anita"))

if __name__ == "__main__":
    main()
```

Without the guard, importing this module would run the program as a side effect — which is exactly what happens to people who omit it.

## Where Python Looks

An import searches, in order: built-in modules, then each directory in `sys.path`. `sys.path` starts with the directory of the script being run, followed by the installed-package directories.

A module that cannot be found raises:

```python
import nosuchmodule
```

**Output:**

```
ModuleNotFoundError: No module named 'nosuchmodule'
```

Three causes, in order of likelihood: the package is not installed, the file is not in a directory Python is searching, or the name is misspelled.

One trap deserves naming. A file of your own that shares a name with a library shadows it, because the script's own directory is searched first. A file called `json.py` or `random.py` in your project breaks every import of the real module, with errors that make no sense. Do not name files after standard library modules.

## Circular Imports

Two modules that import each other cause trouble. If `cart.py` has `import billing` and `billing.py` has `import cart`, one of them is asked for its contents while it is still partway through running, and an `ImportError` or `AttributeError` results.

The fix is not a trick. It is that the dependency is genuinely tangled, and the shared code belongs in a third module both can import.

## A Worked Split

A single-file program divided into three:

`products.py` — data and the rules about it:

```python
PRICES = {"pen": 10, "book": 250}

def price_of(item):
    return PRICES.get(item, 0)
```

`cart.py` — logic built on it:

```python
from products import price_of

def total(items):
    return sum(price_of(item) for item in items)
```

`app.py` — the program:

```python
from cart import total

def main():
    print(total(["pen", "book"]))

if __name__ == "__main__":
    main()
```

**Output:**

```
260
```

Each file has one job, each can be read on its own, and `cart.py` can be reused by any other program that needs a total.

The dependencies point one way — `app` uses `cart`, `cart` uses `products` — and nothing points back. Keeping that arrangement is what prevents circular imports.

## Further Reading

- **Official Python guide to modules** — https://docs.python.org/3/tutorial/modules.html
- **Python modules and imports** — https://realpython.com/python-modules-packages/

A module is a file, `import` runs it once and caches it, and the main guard separates what a file defines from what it does. Next, grouping modules into a directory.
