# Reading and Writing Files

Everything a program has stored so far has lived in memory, and memory is emptied when the program ends. Run a marks program twice and the second run knows nothing about the first.

A **file** outlives the program. Writing to one keeps data between runs, and reading from one lets a program work on data it did not create.

## Opening a File

`open()` takes a filename and a **mode**, and returns a file object:

```python
handle = open("notes.txt", "w")
handle.write("first line\n")
handle.close()
```

Three things happened: the file was opened for writing, a line was written, and the file was closed.

`close()` matters. Writes are buffered — held in memory and written out in batches — so data may not reach the disk until the file is closed. A program that forgets can lose the end of what it wrote.

## The with Statement

Calling `close()` by hand is unreliable, because an error between the open and the close skips it. `with` closes the file automatically, whatever happens:

```python
with open("notes.txt", "w") as handle:
    handle.write("first line\n")
    handle.write("second line\n")

print(handle.closed)
```

**Output:**

```
True
```

The file was closed on leaving the block, without `close()` appearing. It would have closed just as reliably had the block raised an error.

**Always use `with`.** The bare `open()` form appears above only to show what `with` replaces.

## The Modes

The second argument says what the file is opened for:

| Mode | Meaning | If the file exists | If it does not |
| --- | --- | --- | --- |
| `"r"` | read | opened for reading | `FileNotFoundError` |
| `"w"` | write | **emptied** | created |
| `"a"` | append | written to at the end | created |
| `"x"` | create | `FileExistsError` | created |

`"r"` is the default, so `open("notes.txt")` opens for reading.

The dangerous one is `"w"`. It empties an existing file the moment it is opened, before anything is written:

```python
with open("notes.txt", "w") as handle:
    pass
```

That is now an empty file. If the intention was to add to it, the mode wanted was `"a"`.

Adding `"b"` to any mode — `"rb"`, `"wb"` — works on bytes rather than text, which is what images, PDFs and other non-text files need.

## Reading a Whole File

`read()` returns the entire contents as one string:

```python
with open("notes.txt", "w") as handle:
    handle.write("first line\n")
    handle.write("second line\n")

with open("notes.txt") as handle:
    content = handle.read()

print(repr(content))
print(content)
```

**Output:**

```
'first line\nsecond line\n'
first line
second line

```

`repr()` shows the string as Python sees it, with the newline characters visible. Printing it shows them as actual line breaks — and the trailing `\n` is why there is a blank line at the end.

`read()` is convenient and loads everything into memory at once. For a large file, that is a problem.

## Reading Line by Line

A file object is iterable, giving one line per iteration:

```python
with open("notes.txt") as handle:
    for number, line in enumerate(handle, start=1):
        print(number, line.strip())
```

**Output:**

```
1 first line
2 second line
```

This reads one line at a time rather than the whole file, so it works on a file of any size. It is the form to reach for by default.

`.strip()` is almost always wanted. Each line arrives with its newline still attached, so `print(line)` would double-space the output.

`readlines()` gives every line as a list, newlines included:

```python
with open("notes.txt") as handle:
    print(handle.readlines())
```

**Output:**

```
['first line\n', 'second line\n']
```

Useful when the lines are needed as a list — to count them, index them, or sort them. Like `read()`, it loads everything at once.

| Method | Returns | Memory |
| --- | --- | --- |
| `read()` | the whole file as one string | all of it |
| `readlines()` | a list of lines | all of it |
| `readline()` | the next line only | one line |
| iterating the file | one line per loop | one line |

## Writing

`write()` writes a string and returns how many characters it wrote:

```python
with open("names.txt", "w") as handle:
    handle.write("Anita")
    handle.write("Ravi")

with open("names.txt") as handle:
    print(handle.read())
```

**Output:**

```
AnitaRavi
```

`write()` adds nothing of its own. No newline, no separator — unlike `print()`, which adds a line break every time. Supply the `\n` yourself:

```python
with open("names.txt", "w") as handle:
    handle.write("Anita\n")
    handle.write("Ravi\n")

with open("names.txt") as handle:
    print(handle.read())
```

**Output:**

```
Anita
Ravi

```

`write()` only accepts strings. A number must be converted:

```python
with open("marks.txt", "w") as handle:
    handle.write(78)
```

**Output:**

```
TypeError: write() argument must be str, not int
```

Writing a list of lines is common enough to have its own method. `writelines()` takes any sequence of strings and — matching `write()` — adds no newlines:

```python
with open("names.txt", "w") as handle:
    handle.writelines(["Anita\n", "Ravi\n"])

with open("names.txt") as handle:
    print(handle.read())
```

**Output:**

```
Anita
Ravi

```

Writing a list of values is the loop you would expect:

```python
names = ["Anita", "Ravi", "Meera"]

with open("names.txt", "w") as handle:
    for name in names:
        handle.write(f"{name}\n")

with open("names.txt") as handle:
    print(handle.read().strip())
```

**Output:**

```
Anita
Ravi
Meera
```

## Appending

`"a"` adds to the end and leaves existing content alone:

```python
with open("log.txt", "w") as handle:
    handle.write("first entry\n")

with open("log.txt", "a") as handle:
    handle.write("second entry\n")

with open("log.txt") as handle:
    print(handle.read().strip())
```

**Output:**

```
first entry
second entry
```

Had the second block used `"w"`, the first entry would be gone. Append is the mode for logs, records, and anything that accumulates.

## Encoding

A file holds bytes; text is those bytes interpreted according to an **encoding**. Python picks a default that varies by operating system, which means a file written on one machine can be unreadable on another.

State it explicitly:

```python
with open("notes.txt", "w", encoding="utf-8") as handle:
    handle.write("Café — naïve\n")

with open("notes.txt", encoding="utf-8") as handle:
    print(handle.read().strip())
```

**Output:**

```
Café — naïve
```

`utf-8` handles every character in every language and is the standard for text files. Use it on every `open()` that deals with text. Omitting it works until the day a name has an accent in it.

Reading a file with the wrong encoding gives `UnicodeDecodeError`, or — worse — silently wrong characters.

## A Complete Example

Reading records, working on them, and writing a report:

```python
with open("marks.txt", "w", encoding="utf-8") as handle:
    handle.write("Anita 78\n")
    handle.write("Ravi 91\n")
    handle.write("Meera 64\n")

total = 0
count = 0

with open("marks.txt", encoding="utf-8") as handle:
    for line in handle:
        name, mark = line.split()
        total = total + int(mark)
        count = count + 1

with open("report.txt", "w", encoding="utf-8") as handle:
    handle.write(f"Students: {count}\n")
    handle.write(f"Average: {total / count:.1f}\n")

with open("report.txt", encoding="utf-8") as handle:
    print(handle.read().strip())
```

**Output:**

```
Students: 3
Average: 77.7
```

Three separate `with` blocks, each open only as long as it is needed. Note that `int(mark)` is required — everything read from a file is text, exactly as everything read from `input()` is.

## Further Reading

- **Official Python guide to reading and writing files** — https://docs.python.org/3/tutorial/inputoutput.html#reading-and-writing-files
- **Working with files in Python** — https://realpython.com/read-write-files-python/

`open()` with a mode, `with` to guarantee closing, and `utf-8` to be sure of the text. Next, naming the file to open and handling the times it is not there.
