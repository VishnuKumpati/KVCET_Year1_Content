# Context Managers

Imagine opening a file to save some data. Once you're done, you need to close it. If an error occurs before the line that closes the file, that line never runs, and the file stays open.

You could try to remember to close the file in every situation. Python offers a simpler solution: **context managers** help ensure cleanup happens when the block ends, even if an error occurs inside it.

> **A context manager manages setup and cleanup around a block of code, even if an error occurs inside it.**

Python uses the **`with`** statement to work with context managers.

## Working with a File without `with`

A file opened with `open()` must be closed with `close()`.

```python
file = open("notes.txt", "w")
file.write("Python is fun")
file.close()
```

This code writes `Python is fun` into a file named `notes.txt` and produces no output.

Here, `close()` must be remembered every time, and it is skipped if an error happens before it.

## The `with` Statement

The `with` statement closes the file **automatically** when the block ends.

**Syntax:**

```python
with open("notes.txt", "w") as file:
    file.write("Python is fun")
```

- `open("notes.txt", "w")` returns a file object, which is the context manager.
- `as file` stores the opened file in the variable `file`.
- The indented lines are the **block**. The file can be used inside it.
- When the block ends, Python closes the file automatically.

**Example:**

```python
with open("notes.txt", "w") as file:
    file.write("Python is fun")

with open("notes.txt", "r") as file:
    print(file.read())

print(file.closed)
```

**Output:**

```
Python is fun
True
```

`file.closed` is `True`, which confirms that the file was closed automatically. No `close()` call was written.

## How a Context Manager Works

A context manager uses two dunder methods:

| Method | When Python Calls It |
| --- | --- |
| `__enter__()` | At the start of the `with` block. Its return value is stored in the variable after `as`. |
| `__exit__()` | At the end of the `with` block, even if an error occurs inside the block. |

```mermaid
flowchart LR
    A["with statement starts"] --> B["__enter__() runs"]
    B --> C["Code inside the block runs"]
    C --> D["__exit__() runs"]
```

## Creating a Context Manager

You can create your own context manager by defining two methods: `__enter__()` and `__exit__()`.

```python
class ClassSession:
    def __enter__(self):
        print("Class starts")
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        print("Class ends")

with ClassSession() as session:
    print("Learning Python")
```

**Output:**

```
Class starts
Learning Python
Class ends
```

1. `with ClassSession() as session:` calls `__enter__()`, which prints `Class starts`. The object it returns is stored in `session`.
2. The block runs and prints `Learning Python`.
3. When the block ends, Python calls `__exit__()`, which prints `Class ends`.

`__exit__()` receives three parameters containing error details: `exc_type`, `exc_value` and `traceback`. If no error occurs, all three are `None`. **These parameters must be written in the method definition**, even if they are not used.

If an error occurs inside the block, Python still calls `__exit__()` before passing the error on. This allows the context manager to perform cleanup even when the block fails.

## Points to Remember

- **Use `with` when working with files.** It closes the file automatically, so `close()` is not needed.
- **`__enter__()` runs at the start of the block**, and its return value is stored in the variable after `as`.
- **`__exit__()` runs at the end of the block**, even if an error occurs.

## Further Reading

- 📎 **Official Python documentation on context managers** — https://docs.python.org/3/reference/datamodel.html#context-managers

Plain text files hold lines of text, but much real data is a table of rows and columns. In the next topic, **CSV Files**, you'll learn how to read and write tables stored in text files.
