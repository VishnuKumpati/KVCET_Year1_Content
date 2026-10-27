# Modules and Imports

As a program grows, keeping all of its code in one file makes it long and hard to manage. The same code may also be needed in many programs. **Modules** let you split code into separate files and reuse it wherever it is needed.

> **A Python module is a file containing Python code, such as functions, variables and classes, that can be imported into another Python file. A typical Python module has a `.py` extension.**

> **Importing means making the code from a module available in your program, using the `import` statement.**

Python comes with many **built-in modules**, which are ready to use.

## Importing a Module

The **`import`** statement makes a whole module available. Its contents are then used with **dot notation**: the module name, a dot, and the name of the function or variable.

```python
import math

print(math.sqrt(16))
print(math.pi)
```

**Output:**

```
4.0
3.141592653589793
```

- `import math` makes the built-in `math` module available.
- `math.sqrt(16)` calls the `sqrt()` function from the module, which returns the square root.
- `math.pi` reads the value of π stored in the module.

## Importing Specific Names

**`from ... import`** brings only the names you need. They can then be used **without** the module name.

```python
from math import sqrt, pi

print(sqrt(25))
print(pi)
```

**Output:**

```
5.0
3.141592653589793
```

Here, `sqrt` and `pi` are used directly. The module name `math` is not written before them.

## Giving a Module a Short Name

**`as`** gives a module a shorter name, called an **alias**.

```python
import math as m

print(m.sqrt(36))
```

**Output:**

```
6.0
```

After `import math as m`, the module is used as `m`. An alias is useful when a module name is long.

## Comparing the Ways to Import

| Statement | How the Function Is Used | When to Use It |
| --- | --- | --- |
| `import math` | `math.sqrt(16)` | When you need several things from a module and want it clear where each one comes from |
| `from math import sqrt` | `sqrt(16)` | When you need only a few specific names |
| `import math as m` | `m.sqrt(16)` | When the module name is long |

Python also allows `from math import *`, which imports many names from a module directly into your program. **Avoid this style**, because it can make it difficult to tell where a name came from and may cause naming conflicts.

## Imports You Have Already Used

The topics below used `import` lines as fixed syntax. Each line follows one of the import styles in this topic:

| Import Line | Topic | Import Style |
| --- | --- | --- |
| `from abc import ABC, abstractmethod` | **Abstraction** | Imports two specific names from the built-in `abc` module |
| `import os` | **File Paths and Errors** | Imports the whole built-in `os` module, used as `os.getcwd()` |
| `import csv` | **CSV Files** | Imports the whole built-in `csv` module, used as `csv.writer()` |
| `import json` | **JSON Files** | Imports the whole built-in `json` module, used as `json.dump()` |

**These lines simply made code from Python's built-in modules available**, exactly as shown in this topic.

## Creating Your Own Module

Any Python file can be used as a module. Create two files **in the same folder**:

**File 1: `greetings.py`**

```python
def say_hello(name):
    return "Hello, " + name

college = "City College"
```

**File 2: `main.py`**

```python
import greetings

print(greetings.say_hello("Ravi"))
print(greetings.college)
```

**Output when `main.py` is run:**

```
Hello, Ravi
City College
```

- `greetings.py` is the module. It contains one function and one variable.
- `import greetings` imports it. The `.py` is **not** written in the `import` statement.
- `main.py` uses the module's contents with dot notation, just like a built-in module.

The specific-name style works with your own modules too:

```python
from greetings import say_hello

print(say_hello("Priya"))
```

**Output:**

```
Hello, Priya
```

This example needs two files. Run it in a code editor on your computer, such as VS Code or IDLE. Online compilers that allow only one file cannot run it.

## Points to Remember

- **A Python module is typically a `.py` file** whose code can be imported into other Python files.
- **`import module`** uses dot notation, while **`from module import name`** uses the name directly.
- **`as`** gives a module a shorter name.
- **Avoid `from module import *`.**
- **For this lesson, keep your own module in the same folder** as the program that imports it, so Python can find it.

## Further Reading

- 📎 **Official Python tutorial on modules** — https://docs.python.org/3/tutorial/modules.html

As a program grows, it may need many modules of its own. In the next topic, **Packages**, you'll learn how to group related modules together in folders.
