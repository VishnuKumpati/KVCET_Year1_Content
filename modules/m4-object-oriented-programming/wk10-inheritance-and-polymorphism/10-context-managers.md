# Context Managers

Some things must be cleaned up. A file must be closed, a network connection released, a lock given back. Forgetting is not an error that shows up immediately — it shows up later, as a program that runs out of file handles or a lock nothing else can acquire.

The obvious approach fails at the first exception:

```python
handle = open("notes.txt", "w")
handle.write("first line\n")
handle.close()
```

If anything between the open and the close raises, `close()` never runs.

`try` / `finally` fixes that, since `finally` runs whatever happens:

```python
handle = open("notes.txt", "w")
try:
    handle.write("first line\n")
finally:
    handle.close()
```

Correct, and four lines of ceremony around one line of work — repeated at every place a file is opened.

A **context manager** packages that pattern. The `with` statement does the setup, runs your block, and guarantees the cleanup:

```python
with open("notes.txt", "w") as handle:
    handle.write("first line\n")
    handle.write("second line\n")

with open("notes.txt") as handle:
    for line in handle:
        print(line.strip())

print(handle.closed)
```

**Output:**

```
first line
second line
True
```

The file was closed without `close()` appearing anywhere. It would have been closed just as reliably if the block had raised.

## The Two Methods

`with` works on any object defining `__enter__` and `__exit__`:

- `__enter__` runs on entering the block. Whatever it returns is bound by `as`.
- `__exit__` runs on leaving the block, by any route — falling off the end, `return`, `break`, or an exception.

```python
class Section:
    def __init__(self, title):
        self.title = title

    def __enter__(self):
        print(f"-- start {self.title}")
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        print(f"-- end {self.title}")
        return False

with Section("report") as section:
    print("working on", section.title)
```

**Output:**

```
-- start report
working on report
-- end report
```

`return self` from `__enter__` is the usual choice, which is what makes `as section` give the object. It is not required — returning anything else binds that instead, and returning nothing binds `None`.

## Cleanup Happens Even on Failure

This is the point of the whole mechanism:

```python
class Section:
    def __init__(self, title):
        self.title = title

    def __enter__(self):
        print(f"-- start {self.title}")
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        print(f"-- end {self.title}")
        return False

try:
    with Section("risky"):
        print("about to fail")
        raise ValueError("boom")
except ValueError as error:
    print("caught:", error)
```

**Output:**

```
-- start risky
about to fail
-- end risky
caught: boom
```

`-- end risky` printed before the error was caught. `__exit__` ran first, then the exception carried on outwards.

## The __exit__ Parameters

`__exit__` receives three arguments describing the exception, all `None` when the block finished normally:

- `exc_type` — the exception's class
- `exc_value` — the exception object
- `traceback` — where it came from

So cleanup can vary by outcome:

```python
class Transaction:
    def __enter__(self):
        print("begin")
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        if exc_type is None:
            print("commit")
        else:
            print(f"rollback ({exc_type.__name__})")
        return False

with Transaction():
    print("doing work")

try:
    with Transaction():
        raise ValueError("bad data")
except ValueError:
    pass
```

**Output:**

```
begin
doing work
commit
begin
rollback (ValueError)
```

One class handles both paths, and the code inside the block says nothing about committing or rolling back.

## The Return Value of __exit__

`__exit__` returning `False` lets an exception continue outwards. Returning `True` **suppresses** it:

```python
class Quiet:
    def __enter__(self):
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        print("exit saw:", exc_type.__name__ if exc_type else None)
        return True

with Quiet():
    raise ValueError("suppressed")

print("carried on")
```

**Output:**

```
exit saw: ValueError
carried on
```

The `ValueError` vanished, and execution continued after the block.

Use this rarely. Swallowing exceptions hides failures, and a context manager that silently discards errors is very hard to debug. Returning `False` — or nothing at all, which means `None` and counts as false — is almost always right. Write `return False` explicitly so the intent is visible.

## contextlib.contextmanager

Writing a class for a manager with no state is heavy. `contextlib.contextmanager` builds one from a generator function:

```python
from contextlib import contextmanager

@contextmanager
def section(title):
    print(f"-- start {title}")
    try:
        yield title
    finally:
        print(f"-- end {title}")

with section("summary") as name:
    print("working on", name)
```

**Output:**

```
-- start summary
working on summary
-- end summary
```

The structure maps directly onto the two methods:

- everything before `yield` is `__enter__`
- the value yielded is what `as` binds
- everything after `yield` is `__exit__`

There must be exactly one `yield`. The function runs to it, pauses while the block executes, and resumes afterwards — the same pausing a generator does anywhere else.

The `try` / `finally` is essential. Without it, an exception in the block propagates out through the `yield` and the cleanup after it never runs, which defeats the entire purpose.

Use this form for a manager that is only a setup and a teardown. Use a class when the manager holds state, needs other methods, or has to inspect the exception in `__exit__`.

## Several at Once

One `with` can manage several objects, separated by commas, and they are cleaned up in reverse order:

```python
with open("input.txt") as source, open("output.txt", "w") as target:
    for line in source:
        target.write(line.upper())
```

Both files are closed whatever happens.

## Where They Are Used

Files are the introduction. The pattern extends to anything with a matched pair of operations:

| Context manager | Guarantees |
| --- | --- |
| `open()` | the file is closed |
| a database connection | the transaction commits or rolls back |
| a lock | the lock is released |
| a timer | the elapsed time is reported |
| a temporary directory | it is deleted |
| `unittest.mock.patch` | the patch is undone |

The signal that a context manager is wanted: **two operations that must always be paired**, where forgetting the second causes a problem that appears somewhere else. Writing one is two methods, and the block that uses it stops mentioning the cleanup at all.

## Further Reading

- **Official reference for `contextlib`** — https://docs.python.org/3/library/contextlib.html
- **Context managers and the `with` statement** — https://realpython.com/python-with-statement/

`with` calls `__enter__` before a block and `__exit__` after it, whatever the block does, so paired operations cannot be left unpaired. That completes object-oriented Python: a class binds data to behaviour, inheritance shares it, polymorphism uses it uniformly, and the dunder methods connect it all to the language's own syntax.
