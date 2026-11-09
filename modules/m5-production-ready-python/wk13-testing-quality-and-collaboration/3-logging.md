# Logging

Tests check whether code works correctly before it is used. Once a program is running in real use, however, a different question matters: what is the program actually doing? When something goes wrong, a programmer needs a record of what happened, when it happened and how serious it was. Without such a record, finding the cause of a problem means guessing.

Programs keep this record through **logging**.

> **Logging is the process of recording messages about events that happen while a program runs. It helps programmers understand a program's behaviour and find the cause of problems.**

> **A log message is a single recorded message that describes one event, such as "The program has started" or "Could not read the marks file".**

A collection of log messages, often saved in a file, is called a **log**.

## Why `print()` Is Not Enough

`print()` can show messages while a program runs, but it lacks features that logging provides:

- **No built-in log levels:** `print()` does not distinguish between normal updates, warnings and errors.
- **No built-in filtering:** You cannot easily show only important messages, or enable detailed messages when debugging.
- **No automatic timestamps:** You have to add date and time information yourself.
- **No built-in logging configuration:** You have to manage where messages go and how they are formatted yourself.

Logging provides all of these features. Messages below a chosen log level can be filtered out, timestamps can be added automatically, and messages can be saved to a file.

## The `logging` Module

Python provides logging through the built-in **`logging`** module, part of the standard library. It needs no installation.

```python
import logging

logging.debug("This is a debug message")
logging.info("This is an info message")
logging.warning("This is a warning message")
logging.error("This is an error message")
logging.critical("This is a critical message")
```

**Output:**

```
WARNING:root:This is a warning message
ERROR:root:This is an error message
CRITICAL:root:This is a critical message
```

Five messages were logged, but only three appeared. The reason is the **log level** of each message, explained next.

Each line of output has three parts, separated by colons:

- **`WARNING`** is the level of the message.
- **`root`** is the name of the **logger** that recorded it. A **logger** is the object that records log messages. The **root logger** is the default logger, used when you call functions such as `logging.warning()` directly.
- **`This is a warning message`** is the message itself.

## Log Levels

> **A log level is a label that shows how important or serious a log message is.**

The `logging` module provides five standard levels, from least to most serious:

| Level | Function | When to Use It | Example Message |
| --- | --- | --- | --- |
| **DEBUG** | `logging.debug()` | Detailed information, useful only while finding a problem | `"Checking the input values"` |
| **INFO** | `logging.info()` | Confirmation that things are working as expected | `"The program has started"` |
| **WARNING** | `logging.warning()` | Something unexpected happened, or may soon cause a problem, but the program still works | `"Marks are close to the limit"` |
| **ERROR** | `logging.error()` | A problem occurred that prevents an operation from completing successfully | `"Could not read the marks file"` |
| **CRITICAL** | `logging.critical()` | A very serious problem that may affect the entire application | `"The database cannot be reached"` |

**By default, Python shows only messages at the WARNING level or above.** This is why the DEBUG and INFO messages were not shown in the first example. Lower levels are hidden by default, so a program's output is not filled with routine details.

## Changing the Log Level with `basicConfig()`

The **`logging.basicConfig()`** function sets the basic logging configuration, including which messages are shown and how they are formatted. Its **`level`** argument sets the lowest level that will be recorded.

```python
import logging

logging.basicConfig(level=logging.INFO)

logging.debug("Checking the input values")
logging.info("The program has started")
logging.warning("Marks are close to the limit")
```

**Output:**

```
INFO:root:The program has started
WARNING:root:Marks are close to the limit
```

- `level=logging.INFO` means messages at **INFO or above** are recorded.
- The INFO and WARNING messages are shown.
- The DEBUG message is still hidden, because DEBUG is below INFO.

Changing `level=logging.INFO` to `level=logging.DEBUG` would show all three messages. This is how logging lets you switch detailed messages on or off **without changing any other line of code**.

Call `basicConfig()` near the start of the program, before logging messages. A **handler** is responsible for sending log messages to a destination, such as the screen or a file. If a logging message is recorded before `basicConfig()` is called, Python may automatically set up a default handler. In that case, a later call to `basicConfig()` will not change the configuration. Run each example as a separate Python script to see the configuration demonstrated in that example.

## Formatting Log Messages

The **`format`** argument of `basicConfig()` controls what each log message looks like. It uses **format attributes**: special names written as `%(name)s`, which logging replaces with real values.

```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s - %(levelname)s - %(message)s",
)

logging.info("The program has started")
logging.warning("Marks are close to the limit")
```

**Possible output:**

```
2026-11-09 10:16:57,084 - INFO - The program has started
2026-11-09 10:16:57,084 - WARNING - Marks are close to the limit
```

| Format Attribute | Replaced With | Example |
| --- | --- | --- |
| `%(asctime)s` | The date and time the message was logged | `2026-11-09 10:16:57,084` |
| `%(levelname)s` | The level of the message | `INFO` |
| `%(message)s` | The message itself | `The program has started` |
| `%(name)s` | The name of the logger | `root` |

The `-` characters and spaces in the format string appear exactly as written. The date and time will be different when you run the program, because they show the moment each message was logged.

## Logging to a File

Messages shown on the screen disappear when the program ends. The **`filename`** argument of `basicConfig()` saves them to a **log file** instead.

```python
import logging

logging.basicConfig(
    filename="app.log",
    level=logging.INFO,
    format="%(asctime)s - %(levelname)s - %(message)s",
)

logging.info("The program has started")
logging.error("Could not read the marks file")
```

Running this program shows **nothing on the screen**. Instead, a file named `app.log` is created in the current working directory.

**Possible contents of `app.log`:**

```
2026-11-09 10:16:57,114 - INFO - The program has started
2026-11-09 10:16:57,114 - ERROR - Could not read the marks file
```

- `filename="app.log"` saves messages that meet the configured log level to a file named `app.log`.
- By default, new messages are **added to the end** of the file each time the program runs. The earlier messages are kept, like opening a file in append mode, covered in **File Handling**.
- The log file can be opened later to see exactly what happened and when.

## Logging Errors in a Program

Logging is most useful in real code, where it records both normal events and problems. This example uses the `get_result()` function from **Edge Cases**:

```python
import logging

logging.basicConfig(level=logging.INFO, format="%(levelname)s - %(message)s")


def get_result(marks):
    if marks < 0 or marks > 100:
        raise ValueError("Marks must be between 0 and 100")
    if marks >= 40:
        return "Pass"
    return "Fail"


for marks in [75, 150]:
    try:
        result = get_result(marks)
        logging.info(f"Marks {marks}: {result}")
    except ValueError:
        logging.error(f"Invalid marks: {marks}")
```

**Output:**

```
INFO - Marks 75: Pass
ERROR - Invalid marks: 150
```

- For `75`, `get_result()` returns `"Pass"`, so an **INFO** message records the normal result.
- For `150`, `get_result()` raises a `ValueError`. The `except` block records an **ERROR** message, and the program **continues** instead of stopping.
- Each message includes the marks, so the log shows exactly which input caused the problem.

## Creating a Named Logger

The examples so far use the **root logger**. In larger programs, each module can use its own **named logger** with **`logging.getLogger()`**. This helps identify which part of the program recorded a message.

```python
import logging

logging.basicConfig(level=logging.INFO, format="%(name)s - %(levelname)s - %(message)s")

logger = logging.getLogger("results")

logger.info("Calculating results")
```

**Output:**

```
results - INFO - Calculating results
```

- `logging.getLogger("results")` gets a logger named `results`. If it does not already exist, Python creates it.
- `logger.info()` records a message using that logger. Named loggers have the same methods as the five logging functions: `debug()`, `info()`, `warning()`, `error()` and `critical()`.
- `%(name)s` in the format shows the logger's name, `results`.

In real projects, the logger is often created with `logging.getLogger(__name__)`. The special variable `__name__` holds the name of the current module, so each module's logger is automatically named after that module.

## Points to Remember

- **Logging records events while a program runs**, using the built-in `logging` module.
- **The five log levels** are DEBUG, INFO, WARNING, ERROR and CRITICAL. By default, only WARNING and above are shown.
- **`basicConfig()`** sets the lowest level to record, the message format and, with `filename`, a log file. Call it once, before logging any messages.
- **Log errors inside `except` blocks**, so problems are recorded while the program keeps running.

## Further Reading

- 📎 **Official Python logging tutorial** — https://docs.python.org/3/howto/logging.html
- 📎 **Official Python reference for the `logging` module** — https://docs.python.org/3/library/logging.html

Tested, logged code still changes over time, and every change needs to be tracked safely. In the next topic, **Git Fundamentals**, you'll learn how to record and manage the history of your code.
