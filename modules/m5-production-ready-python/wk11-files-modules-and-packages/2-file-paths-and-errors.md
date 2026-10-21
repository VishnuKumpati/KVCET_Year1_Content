# File Paths and Errors

Files on a computer are kept in many different folders. A program must tell Python exactly where a file is, or Python cannot find it. Working with files can also fail, for example when a file is missing. A good program handles these problems instead of stopping suddenly.

> **A file path is the location of a file on a computer. It lists the folders that lead to the file, followed by the file name.**

A folder is also called a **directory**. Both words mean the same thing.

## The Current Working Directory

When a file is opened using only its name, such as `"students.txt"`, Python looks for it in the **current working directory**.

> **The current working directory is the folder Python uses by default when a file is given by its name only. It is usually the folder from which the program runs.**

The `getcwd()` function from Python's `os` module shows the current working directory. The line `import os` makes this function available. Importing is covered in **Modules and Imports**.

```python
import os

print(os.getcwd())
```

**Output:**

```
/home/user
```

This prints the folder Python uses when a file is opened using only its name.

This path will be different on your computer. The current working directory depends on how the program is launched and may differ from the folder containing the Python program. **If Python cannot find a file, use `os.getcwd()` to check where it is looking.**

## Absolute Paths

> **An absolute path gives the complete location of a file, starting from the root of the filesystem.**

The **root directory** is the top-level directory of a filesystem. On Windows, a drive commonly has a root such as `C:/`. On macOS and Linux, the filesystem root is `/`.

An absolute path gives the full location, so Python does not need to search relative to the current working directory. The file must still exist at that location, and the program must have permission to access it.

| Operating System | Example of an Absolute Path |
| --- | --- |
| Windows | `C:/Users/Ravi/Documents/students.txt` |
| macOS or Linux | `/home/ravi/documents/students.txt` |

For example, `open("C:/Users/Ravi/Documents/students.txt", "r", encoding="utf-8")` opens the file from the `Documents` folder, as long as the file exists at that location.

### Writing Windows Paths Safely

Windows normally shows paths with **backslashes** (`\`), such as `C:\Users\Ravi`. Inside a Python string, a backslash has a special meaning: it starts an **escape sequence**, such as `\n` for a new line or `\t` for a tab.

```python
print("C:\new_folder\table.txt")
print(r"C:\new_folder\table.txt")
```

**Output:**

```
C:
ew_folder    able.txt
C:\new_folder\table.txt
```

In the first print, `\n` creates a new line and `\t` creates a tab, breaking the path. The spaces shown above represent a tab character; they are not necessarily the exact spacing displayed by your terminal. The `r` in the second string makes it a **raw string**, preserving the backslashes.

> **A raw string is a string written with `r` before the opening quote. In a raw string, backslashes are treated as literal characters rather than starting escape sequences.**

On Windows, use either **forward slashes** (`C:/Users/Ravi`) or a **raw string** (`r"C:\Users\Ravi"`) for file paths.

## Relative Paths

> **A relative path is the location of a file starting from the current working directory.**

A relative path is shorter than an absolute path, because it does not include the full location.

| Relative Path | Where Python Looks |
| --- | --- |
| `"students.txt"` | In the current working directory |
| `"data/students.txt"` | Inside the `data` folder, in the current working directory |
| `"../students.txt"` | In the **parent folder**, one level above the current working directory |

The two dots, `..`, always mean **the parent folder**, which is the folder that contains the current one.

## Building Paths Safely

Sometimes a program builds a path from a folder name and a file name. Different operating systems use different separators between folders: Windows uses `\`, while macOS and Linux use `/`. The `os.path.join()` function combines path parts using the path conventions of the operating system. It builds a path string but does not create the folder or check whether the path exists.

The example below keeps the student names in a `data` folder and uses a relative path to reach them.

```python
import os

os.makedirs("data", exist_ok=True)

path = os.path.join("data", "students.txt")

with open(path, "w", encoding="utf-8") as file:
    file.write("Ravi\nPriya")

with open(path, "r", encoding="utf-8") as file:
    print(file.read())
```

**Output:**

```
Ravi
Priya
```

- `os.makedirs("data", exist_ok=True)` creates a **folder**, not a file.
    - If the `data` folder does not exist, Python creates it.
    - If the `data` folder already exists, `exist_ok=True` allows the program to continue without raising an error.
- `os.path.join("data", "students.txt")` builds the relative path. It becomes `data/students.txt` on macOS and Linux, and `data\students.txt` on Windows.
- The same `path` is then used to write the names and read them back.

## Common File Errors

When a file operation fails, Python raises an error. Two common errors are:

| Error | When It Occurs |
| --- | --- |
| `FileNotFoundError` | Python cannot find the file or folder used in the operation. |
| `PermissionError` | The program is not allowed to read or write the file. |

If an error is not handled, Python stops the normal execution of the program and reports the error.

## Handling File Errors

A **`try`** and **`except`** block lets a program respond to a specific error instead of stopping because of that error. What happens next depends on the code inside the `except` block.

```python
try:
    with open("missing.txt", "r", encoding="utf-8") as file:
        print(file.read())
except FileNotFoundError:
    print("The file was not found.")
```

**Output:**

```
The file was not found.
```

- The code inside `try` attempts to open `missing.txt`. This file is never created, so it is used to show what happens when a requested file cannot be found.
- Python raises a `FileNotFoundError`.
- The `except FileNotFoundError` block catches the error and prints a clear message, instead of stopping the program.

### Handling Permission Errors

A file may exist, but the program may not have permission to read it. Python then raises a `PermissionError`. One `try` block can have a separate `except` block for each error.

```python
try:
    with open("private.txt", "r", encoding="utf-8") as file:
        print(file.read())
except FileNotFoundError:
    print("The file was not found.")
except PermissionError:
    print("You do not have permission to read this file.")
```

**Possible output if `private.txt` does not exist:**

```
The file was not found.
```

If the file exists and is readable, its contents are printed instead. If it exists but cannot be read because of permissions, the `PermissionError` handler runs. **The output depends on the state of your computer**, so you may not see the permission message yourself.

## Checking Whether a File Exists

Another way to avoid a missing-file error is to check first. The `os.path.exists()` function returns `True` if the path exists and `False` if it does not. On some systems, it may also return `False` when the path exists but cannot be accessed. An invalid path may raise an error. **It does not guarantee that the file can be opened successfully.**

```python
import os

print(os.path.exists("students.txt"))
print(os.path.exists("missing.txt"))
```

**Possible output:**

```
True
False
```

Assuming `students.txt` exists in the current working directory and `missing.txt` does not, the output is `True` followed by `False`. **The result depends on which files actually exist in that folder.**

### Checking Is Not a Replacement for Handling Errors

A file may disappear or become inaccessible after it is checked. So **checking first does not replace error handling**. When a program must open a file, use `try` and `except` to handle the errors that may occur while opening it.

Checking with `os.path.exists()` is useful, for example, to show a message to the user. When reading the file is the real goal, handling `FileNotFoundError` directly is often simpler.

## Points to Remember

- **A file name alone** uses the current working directory.
- **An absolute path** gives the full location, while **a relative path** starts from the current working directory.
- **On Windows, use forward slashes or a raw string** for paths, so backslashes are not read as escape sequences.
- **Use `os.path.join()`** to build paths from separate parts.
- **Use `try` and `except`** to handle errors such as `FileNotFoundError` and `PermissionError`, so the program does not stop suddenly.

## Further Reading

- 📎 **Official Python reference for `os.getcwd()`** — https://docs.python.org/3/library/os.html#os.getcwd
- 📎 **Official Python reference for `os.path.exists()`** — https://docs.python.org/3/library/os.path.html#os.path.exists
- 📎 **Official Python guide to handling exceptions** — https://docs.python.org/3/tutorial/errors.html#handling-exceptions

Files often store data as rows and columns, such as the name and marks of many students. In the next topic, **CSV Files**, you'll learn how to read and write this kind of data.
