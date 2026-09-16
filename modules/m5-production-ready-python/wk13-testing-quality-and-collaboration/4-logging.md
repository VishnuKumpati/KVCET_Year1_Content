# Logging

`print()` has been the way to see what a program is doing. It is fine while writing code and wrong in a program anyone else runs.

**Logging** is the alternative: a record of what happened, with a severity on each message, that can be switched on and off, redirected to a file, and filtered — without editing the code.

## Why print Is Not Enough

Debugging with `print()` produces lines that then have to be deleted, and re-added the next time something goes wrong. Worse, `print()` gives no way to say **how important** a message is. "Loaded 42 records" and "Could not write the report" look identical.

| | `print()` | `logging` |
| --- | --- | --- |
| Severity | none | five levels |
| Switching off | delete the lines | change one setting |
| Destination | the screen | screen, file, or both |
| Timestamps | write them yourself | built in |
| Where it came from | unknown | module name included |

The rule: **`print()` is for output the user asked for. `logging` is for information about what the program is doing.** A report going to the screen is `print()`. A note that a file was missing is logging.

## The Levels

Five, in increasing severity:

| Level | Use for |
| --- | --- |
| `DEBUG` | detail useful only while diagnosing a problem |
| `INFO` | confirmation that things are working as expected |
| `WARNING` | something unexpected, but the program continues |
| `ERROR` | something failed; the operation did not complete |
| `CRITICAL` | the program cannot continue |

```python
import logging

logging.debug("a debug message")
logging.info("an info message")
logging.warning("a warning message")
logging.error("an error message")
logging.critical("a critical message")
```

**Output:**

```
WARNING:root:a warning message
ERROR:root:an error message
CRITICAL:root:a critical message
```

Five calls, three lines. **The default level is `WARNING`**, so `DEBUG` and `INFO` were discarded.

This surprises everyone the first time, and it is the mechanism that makes logging useful: the messages exist in the code permanently, and the level decides which are shown.

Note also that logging writes to **standard error**, not standard output. That keeps it separate from a program's real output, so `python report.py > report.txt` captures the report and lets the log messages through to the screen.

## basicConfig

`basicConfig()` sets the level, the format and the destination. Call it once, at the start of the program:

```python
import logging

logging.basicConfig(level=logging.DEBUG)

logging.debug("a debug message")
logging.info("an info message")
logging.warning("a warning message")
```

**Output:**

```
DEBUG:root:a debug message
INFO:root:an info message
WARNING:root:a warning message
```

Everything now appears. Changing `DEBUG` to `WARNING` silences the first two, without touching a single logging call.

The default format is noisy. `format` controls it:

```python
import logging

logging.basicConfig(level=logging.INFO, format="%(levelname)s - %(message)s")

logging.info("Loaded 42 records")
logging.warning("3 records had no mark")
logging.error("Could not write the report")
```

**Output:**

```
INFO - Loaded 42 records
WARNING - 3 records had no mark
ERROR - Could not write the report
```

| Placeholder | Gives |
| --- | --- |
| `%(asctime)s` | the time |
| `%(levelname)s` | DEBUG, INFO, … |
| `%(name)s` | the logger's name |
| `%(message)s` | the message |
| `%(filename)s` | the source file |
| `%(lineno)d` | the line number |

A good general format:

```python
logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s %(levelname)-8s %(name)s: %(message)s",
)
```

`%(levelname)-8s` pads the level to eight characters, which keeps the messages aligned.

`basicConfig()` only takes effect on its first call. Later calls are ignored, which is why it belongs in one place at startup rather than in the modules doing the logging.

## A Logger per Module

Using `logging.info()` directly logs to the **root** logger, which is why the output above says `root`. In anything larger than a script, each module should have its own:

```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format="%(levelname)-8s %(name)s: %(message)s",
)

logger = logging.getLogger(__name__)
logger.info("starting")
logger.warning("something looks off")

other = logging.getLogger("shop.cart")
other.info("cart updated")
```

**Output:**

```
INFO     __main__: starting
WARNING  __main__: something looks off
INFO     shop.cart: cart updated
```

`logging.getLogger(__name__)` at the top of each module is the standard line. `__name__` is the module's name, so every message says which module produced it — and inside a package that comes out as `shop.cart`, matching the import path.

The names form a hierarchy, so an entire subsystem's level can be changed at once: setting `shop` to `DEBUG` affects `shop.cart` and `shop.billing` too.

## Lazy Formatting

Logging calls take formatting arguments rather than a pre-built string:

```python
logger.info("Loaded %s records from %s", count, name)
```

**Output:**

```
INFO: Loaded 42 records from marks.csv
```

`logger.info(f"Loaded {count} records from {name}")` produces the same output and does more work: the f-string is built **before** the call, so it is built even when the level means the message is discarded. With `%s` arguments, the formatting only happens if the message is actually emitted.

For a `DEBUG` line inside a loop running a million times, that is the difference between free and expensive. Use the `%s` form in logging calls, even though f-strings are right everywhere else.

## Logging an Exception

Inside an `except` block, `logger.exception()` records the message **and** the full traceback:

```python
import logging

logging.basicConfig(level=logging.INFO, format="%(levelname)s: %(message)s")
logger = logging.getLogger(__name__)

try:
    1 / 0
except ZeroDivisionError:
    logger.exception("Could not compute the average")
```

**Output:**

```
ERROR: Could not compute the average
Traceback (most recent call last):
  File "report.py", line 7, in <module>
    1 / 0
    ~~^~~
ZeroDivisionError: division by zero
```

Compare `logger.error("Could not compute the average")`, which records only:

```
ERROR: Could not compute the average
```

The message says something failed; the traceback says where and why. In production that difference is the difference between fixing a bug and guessing at it.

`logger.exception()` logs at `ERROR` level and works only inside an `except` block.

## Logging to a File

`filename` sends everything to a file instead of the screen:

```python
import logging

logging.basicConfig(
    level=logging.INFO,
    filename="app.log",
    filemode="w",
    format="%(levelname)s: %(message)s",
)

logging.info("started")
logging.error("something failed")
print("nothing appeared on screen")
```

**Output:**

```
nothing appeared on screen
```

`app.log` then contains:

```
INFO: started
ERROR: something failed
```

`filemode="w"` replaces the file each run. The default, `"a"`, appends — which is what a real log wants, so that history survives a restart.

For a long-running program, `logging.handlers.RotatingFileHandler` starts a new file once one grows too large, so logs cannot fill the disk.

## Screen and File Together

The usual arrangement is brief output on screen and full detail in a file. That needs two **handlers**, each with its own level:

```python
import logging
import sys

logger = logging.getLogger("report")
logger.setLevel(logging.DEBUG)

console = logging.StreamHandler(sys.stdout)
console.setLevel(logging.WARNING)
console.setFormatter(logging.Formatter("%(levelname)s: %(message)s"))

file_handler = logging.FileHandler("full.log", mode="w", encoding="utf-8")
file_handler.setLevel(logging.DEBUG)
file_handler.setFormatter(logging.Formatter("%(levelname)s %(name)s: %(message)s"))

logger.addHandler(console)
logger.addHandler(file_handler)

logger.debug("detail for the file only")
logger.warning("shown in both")
```

**Output:**

```
WARNING: shown in both
```

`full.log` contains both:

```
DEBUG report: detail for the file only
WARNING report: shown in both
```

The logger's own level is the first filter — a message below it reaches no handler at all — and each handler then applies its own. Set the logger to the most detailed level you ever want, and let the handlers decide what to show where.

## What to Log

**Log the boundaries.** Starting and finishing, files opened, requests sent, records processed. These are what let you reconstruct a run afterwards.

**Log at the right level.** Everything at `ERROR` makes the level useless; everything at `DEBUG` means real problems are missed.

**Include the values.** "Could not open the file" is much less useful than "Could not open data/marks.csv".

**Log the failure where it is handled**, not where it is raised. Logging and re-raising records the same problem several times.

**Never log secrets.** Passwords, API keys, tokens, card numbers and personal data must not reach a log file. Logs are copied, shared with support, and shipped to third-party services. This is a common and serious mistake, and once a secret is in a log it should be treated as compromised.

## Further Reading

- **Official Python logging HOWTO** — https://docs.python.org/3/howto/logging.html
- **Logging in Python** — https://realpython.com/python-logging/

Five levels, `basicConfig()` at startup, `getLogger(__name__)` per module, `%s` arguments rather than f-strings, and `logger.exception()` inside `except`. Next, keeping a history of the code itself.
