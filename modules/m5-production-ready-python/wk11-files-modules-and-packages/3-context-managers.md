# Context Managers

Some tasks come in pairs: a setup step at the start and a cleanup step at the end. An exam hall is opened before an exam and must be closed afterwards, even if something goes wrong during the exam.

```python
class ExamHall:
    def open_hall(self):
        print("Hall opened")

    def close_hall(self):
        print("Hall closed")

hall = ExamHall()
hall.open_hall()
print("Exam in progress")
print(10 / 0)
hall.close_hall()
```

**Output:**

```
Hall opened
Exam in progress
ZeroDivisionError: division by zero
```

The error stopped the program before `hall.close_hall()` ran, so the hall was never closed. A **context manager** guarantees that the cleanup step always runs.

## Context Managers

> **A context manager is an object that runs setup code before a block of statements and cleanup code after it, even if an error occurs inside the block.** It is used with the **`with`** statement.

**Syntax:**

```python
with context_manager as variable:
    statements
```

- The setup code runs when the `with` block starts.
- The indented statements run next.
- The cleanup code runs when the block ends, whether it finishes normally or stops because of an error.
- `as variable` is optional. It stores the value returned by the setup code.

## The __enter__ and __exit__ Methods

Any object can be used with `with` if its class defines two dunder methods:

| Method | When Python Calls It | Purpose |
| --- | --- | --- |
| `__enter__(self)` | When the `with` block starts | Setup. Its return value is stored in the variable after `as`. |
| `__exit__(self, exc_type, exc_value, traceback)` | When the `with` block ends, for any reason | Cleanup |

The three extra parameters of `__exit__` describe an error, if one occurred inside the block. All three are `None` when the block finishes without an error. They must be included in the definition even when the method does not use them.

## Creating a Context Manager

```python
class ExamHall:
    def __init__(self, hall_name):
        self.hall_name = hall_name

    def __enter__(self):
        print(self.hall_name, "opened")
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        print(self.hall_name, "closed")

with ExamHall("Hall A") as hall:
    print("Exam in progress in", hall.hall_name)

print("After the with block")
```

**Output:**

```
Hall A opened
Exam in progress in Hall A
Hall A closed
After the with block
```

1. `ExamHall("Hall A")` creates the object.
2. `with` calls `__enter__`, which prints the opening message and returns the object itself. `as hall` stores that returned object in `hall`.
3. The block runs.
4. When the block ends, `with` calls `__exit__`, which prints the closing message.

Neither `__enter__` nor `__exit__` is called by name. The `with` statement calls both automatically.

## Cleanup Runs Even When an Error Occurs

```python
class ExamHall:
    def __init__(self, hall_name):
        self.hall_name = hall_name

    def __enter__(self):
        print(self.hall_name, "opened")
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        print(self.hall_name, "closed")

with ExamHall("Hall A") as hall:
    print("Exam in progress")
    print(10 / 0)
```

**Output:**

```
Hall A opened
Exam in progress
Hall A closed
ZeroDivisionError: division by zero
```

The error stopped the block, but `__exit__` still ran and the hall was closed before the error was reported. This is the main purpose of a context manager: **the cleanup step can never be skipped**.

## Where Context Managers Are Used

`with open(...)` works because `open()` returns a context manager. Its `__enter__` returns the file object, and its `__exit__` closes the file, so the file is closed even if an error occurs while reading or writing.

| Task | Setup | Cleanup Guaranteed by `with` |
| --- | --- | --- |
| Working with a file | Open the file | Close the file |
| Using a database | Connect to the database | Close the connection |
| Using a shared resource | Take control of it | Release it |

## Further Reading

- 📎 **Official Python reference for the `with` statement** — https://docs.python.org/3/reference/compound_stmts.html#the-with-statement
- 📎 **Official Python reference for context managers** — https://docs.python.org/3/reference/datamodel.html#context-managers

Plain text files hold lines of text, but much real data is a table of rows and columns. In the next topic, **CSV Files**, you'll learn how to read and write tables stored in text files.
