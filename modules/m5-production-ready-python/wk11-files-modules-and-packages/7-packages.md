# Packages

A large project can contain dozens of modules. Without a clear structure, related code gets scattered and hard to find. **Packages** solve this by organising related modules into folders.

> **A Python package groups related modules under a package name. It is commonly organised as a folder containing Python module files and may include an `__init__.py` file.**

## The Structure of a Package

A package is a folder with modules inside it. For example, a package named `college` could look like this:

```
project/
├── main.py
└── college/
    ├── __init__.py
    ├── students.py
    └── marks.py
```

- `college` is the **package**.
- `students.py` and `marks.py` are **modules** inside the package.
- `__init__.py` is commonly used to mark `college` as a regular Python package. **It can be empty**, or it can contain code that runs when the package is imported.
- `main.py` is the program that uses the package. It is outside the `college` folder.

Python can also treat a folder without `__init__.py` as a package. However, including `__init__.py` is the common and clear way to create one.

## Creating the Package

Create the folders and files shown above, with this code:

**File: `college/__init__.py`**

Leave this file empty.

**File: `college/students.py`**

```python
def welcome(name):
    return "Welcome, " + name
```

**File: `college/marks.py`**

```python
def grade(marks):
    if marks >= 40:
        return "Pass"
    return "Fail"
```

## Importing from a Package

A module inside a package is imported using **dot notation**: the package name, a dot, and the module name.

**File: `main.py`**

```python
import college.students
from college import marks
from college.marks import grade

print(college.students.welcome("Ravi"))
print(marks.grade(85))
print(grade(32))
```

**Output when `main.py` is run:**

```
Welcome, Ravi
Pass
Fail
```

| Import Line | How It Is Used |
| --- | --- |
| `import college.students` | `college.students.welcome("Ravi")` |
| `from college import marks` | `marks.grade(85)` |
| `from college.marks import grade` | `grade(32)` |

These are the same import styles you learned for modules. Here, the module is inside the `college` package, so its name includes the package.

This example needs several files and a folder. Run it in a code editor on your computer, such as VS Code or IDLE. Online compilers that allow only one file cannot run it.

## Installing Packages with `pip`

Many useful packages are written by other programmers and shared online. They are stored in the **Python Package Index (PyPI)**, and are installed using a tool called **`pip`**.

> **`pip` is a tool for installing Python packages, commonly from the Python Package Index (PyPI).**

`pip` runs in the **terminal** or **command prompt**, not inside a Python program. For example, this command installs a package named `requests`:

```
pip install requests
```

After it is installed, the package is imported like any other, using `import requests`. This line works only after the package has been installed.

If `pip` is not recognised on your computer, try `python -m pip install requests` instead.

## Points to Remember

- **A Python package groups related modules**, commonly in a folder. A regular package typically contains an `__init__.py` file.
- **Use dot notation** to import a module from a package, such as `import college.students`.
- **`pip install package_name`** installs a Python package, commonly from the Python Package Index (PyPI). It runs in the terminal, not in a Python program.

## Further Reading

- 📎 **Official Python tutorial on packages** — https://docs.python.org/3/tutorial/modules.html#packages
- 📎 **Official Python guide to installing packages** — https://docs.python.org/3/installing/index.html

Python already includes many useful modules and packages without installing anything. In the next topic, **The Standard Library**, you'll explore some of the most useful ones.
