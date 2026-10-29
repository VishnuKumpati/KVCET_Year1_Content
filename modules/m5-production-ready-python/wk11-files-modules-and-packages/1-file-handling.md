# File Handling

Variables store data while a program runs. But what happens when the program ends and we need that data again?

Programs often need to preserve information such as student names, marks or reports and use it later. **Files** make this possible by storing data on a computer's storage, even after a program ends.

> **File handling means using a program to create, read, write and manage files.**

## What Is a File

> **A file is a named collection of data stored on a computer. Unlike data held only in variables, its contents can remain available after a program ends or the computer is switched off.**

Every file has a **name** and usually an **extension**, the part after the dot, such as `.txt` in `students.txt`. The extension usually indicates the file's type or format, although it does not guarantee what the file actually contains.

Files are of two main types:

| Type | What It Stores | Common Examples |
| --- | --- | --- |
| **Text file** | Characters that people can read, such as letters, numbers and symbols, arranged in lines | `.txt`, `.csv`, `.json`, `.py`, `.html` |
| **Binary file** | Data stored as bytes, which is not meant to be read as plain text | `.jpg`, `.png`, `.mp3`, `.pdf`, `.exe` |

This topic focuses on **text files**. To work with a binary file, add `b` to the mode, for example `"rb"` to read or `"wb"` to write binary data.

## Opening a File

Python opens a file using the built-in **`open()`** function.

**Example:**

```python
file = open("students.txt", "w")
file.close()
```

- The first argument, `"students.txt"`, is the **file name**.
- The second argument, `"w"`, is the **mode**. It tells Python what you want to do with the file.
- `open()` returns a **file object**, which is used to read from or write to the file.

Python opens `students.txt` for writing and stores the file object in `file`. The next line closes the file. Since only a file name is given, Python looks for or creates the file in the **current working directory**. You'll learn more about file locations in **File Paths and Errors**.

| Mode | Name | What It Does |
| --- | --- | --- |
| `"r"` | Read | Opens an existing file for reading only. This is the default mode. Reading starts at the beginning of the file. Raises an error if the file does not exist. |
| `"rb"` | Read binary | Opens an existing file for reading only, in binary format. Reading starts at the beginning of the file. Raises an error if the file does not exist. |
| `"r+"` | Read and write | Opens an existing file for both reading and writing. Starts at the beginning of the file. Raises an error if the file does not exist. |
| `"rb+"` | Read and write binary | Opens an existing file for both reading and writing, in binary format. Starts at the beginning of the file. Raises an error if the file does not exist. |
| `"w"` | Write | Opens the file for writing only. Creates the file if it does not exist. **Truncates the file**, removing its existing content. |
| `"wb"` | Write binary | Opens the file for writing only, in binary format. Creates the file if it does not exist. **Truncates the file**, removing its existing content. |
| `"w+"` | Write and read | Opens the file for both writing and reading. Creates the file if it does not exist. **Truncates the file**, removing its existing content. |
| `"wb+"` | Write and read binary | Opens the file for both writing and reading, in binary format. Creates the file if it does not exist. **Truncates the file**, removing its existing content. |
| `"a"` | Append | Opens the file for adding content at the end. Creates the file if it does not exist. Keeps the existing content. |
| `"ab"` | Append binary | Opens the file for adding content at the end, in binary format. Creates the file if it does not exist. Keeps the existing content. |
| `"a+"` | Append and read | Opens the file for both appending and reading. Creates the file if it does not exist. Keeps the existing content, and new content is always written at the end. |
| `"ab+"` | Append and read binary | Opens the file for both appending and reading, in binary format. Creates the file if it does not exist. Keeps the existing content, and new content is always written at the end. |

## Writing to a File and Closing It

The **`write()`** method writes text into a file. After the work is done, the **`close()`** method closes the file.

```python
file = open("students.txt", "w")
file.write("Ravi\n")
file.write("Priya")
file.close()

file = open("students.txt", "r")
print(file.read())
file.close()
```

**Output:**

```
Ravi
Priya
```

- `open("students.txt", "w")` opens `students.txt` for writing. If the file does not exist, Python creates it. If it already exists, Python removes its existing content.
- `write()` does not move to a new line on its own. **`\n` adds a new line**, so `Priya` is written on the second line.
- `read()` reads the whole file as one string.
- `close()` closes the file. **Close a file after using it** to release the file resource and make sure Python finishes writing its buffered data. The `with` statement, explained next, handles closing automatically.

This example uses `close()` to show how file handling works manually. In everyday Python code, prefer `with`, because it closes the file even if an error occurs inside the block.

## The `with` Statement

Remembering to call `close()` every time is easy to forget. The **`with`** statement opens a file and **closes it automatically** when the indented block ends.

**Syntax:**

```python
with open("students.txt", "w") as file:
    file.write("Ravi")
```

- `as file` stores the file object in the variable `file`.
- Use the file inside the indented block. When the block ends, Python closes the file automatically, so `close()` is not needed.

**Use the `with` statement when working with files.** It closes the file automatically when the block ends, even if an error occurs. This works because file objects support **context managers**, which handle setup and cleanup around a block of code. The remaining examples in this topic use `with`.

## Choosing a File Encoding

Computers store data as **bytes**, small units of data that a computer can store and process. When text is saved in a file, an encoding converts characters into bytes. When the file is read, the same encoding is used to convert those bytes back into characters.

> **An encoding is a set of rules that defines how characters are represented as bytes.**

For example, in UTF-8, the letter `A` is represented by the byte value `65`. A character can use one or more bytes: `A` uses one byte, while the Tamil letter `வ` uses three.

**UTF-8** is the most widely used encoding. It can store characters from almost every language in the world, including English, Tamil, Hindi and symbols.

### Why the Encoding Should Be Specified

If no encoding is given, Python uses the default encoding of the computer. **This default can be different on different computers.** A file written on one computer may then be read wrongly on another.

To avoid this, specify the encoding when opening a text file, using `encoding="utf-8"`.

```python
with open("greeting.txt", "w", encoding="utf-8") as file:
    file.write("Hello\nவணக்கம்\nनमस्ते")

with open("greeting.txt", "r", encoding="utf-8") as file:
    print(file.read())
```

**Output:**

```
Hello
வணக்கம்
नमस्ते
```

- The first block writes English, Tamil and Hindi words into the file using UTF-8.
- The second block reads the file using the **same** encoding, so every word appears correctly.

**Always use the same encoding to write and read a file.** If a file written in UTF-8 is read using a different encoding, the text may appear incorrectly, or Python may raise a **`UnicodeDecodeError`**. The result depends on the encoding used and the bytes in the file.

`encoding="utf-8"` is an **option** passed to `open()`. It is not a file mode, so it is written after the mode, separated by a comma. The remaining examples in this topic use it.

## Writing Numbers and Other Values

In text mode, `write()` expects a **string**. To write a number, convert it to a string first using `str()`.

```python
marks = 85

with open("marks.txt", "w", encoding="utf-8") as file:
    file.write(str(marks))

with open("marks.txt", "r", encoding="utf-8") as file:
    print(file.read())
```

**Output:**

```
85
```

Writing `file.write(marks)` without `str()` raises a **`TypeError`**, because `marks` is a number. For more complex data, such as lists of students or structured records, Python provides formats such as CSV and JSON, which are covered in **CSV Files** and **JSON Files**.

## Reading a File

Python provides three methods to read a file, and a file can also be read using a loop:

| Way of Reading | What It Gives |
| --- | --- |
| `read()` | The whole file as one string |
| `readline()` | One line at a time, as a string |
| `readlines()` | All the lines as a list of strings |
| A `for` loop | One line in each round of the loop |

Each example below first writes three names into `students.txt`, and then reads them in a different way.

### Reading the Whole File with `read()`

`read()` reads **everything** in the file and returns it as **one string**.

```python
with open("students.txt", "w", encoding="utf-8") as file:
    file.write("Ravi\nPriya\nArun")

with open("students.txt", "r", encoding="utf-8") as file:
    content = file.read()

print(content)
```

**Output:**

```
Ravi
Priya
Arun
```

`content` holds the whole file as one string: `"Ravi\nPriya\nArun"`. When it is printed, each `\n` starts a new line.

Use `read()` when the file is small and you need all of its content at once.

### Reading One Line with `readline()`

`readline()` reads **one line** each time it is called.

```python
with open("students.txt", "w", encoding="utf-8") as file:
    file.write("Ravi\nPriya\nArun")

with open("students.txt", "r", encoding="utf-8") as file:
    first = file.readline()
    second = file.readline()

print(first.strip())
print(second.strip())
```

**Output:**

```
Ravi
Priya
```

- The first `readline()` reads `"Ravi\n"`.
- The second `readline()` reads `"Priya\n"`, because Python keeps track of the **current reading position** in the file. Each call continues from where the previous one stopped.
- `strip()` removes whitespace, including the newline character (`\n`), from the beginning and end of a string. Be careful: `strip()` also removes spaces and tabs at the beginning and end, which may be meaningful in some data.

### When There Is Nothing Left to Read

A file may be empty, or a program may reach the end of a file while reading it.

- `read()` returns an empty string (`""`) when there is no more content to read.
- `readline()` also returns `""` when it reaches the end of the file.
- A **blank line** inside a file is different from the end of the file. A blank line is read as `"\n"`, a line containing only a newline character.

This difference becomes useful when processing files line by line.

### Reading All Lines into a List with `readlines()`

`readlines()` reads **all the lines** and returns them as a **list**. Each line becomes one item.

```python
with open("students.txt", "w", encoding="utf-8") as file:
    file.write("Ravi\nPriya\nArun")

with open("students.txt", "r", encoding="utf-8") as file:
    lines = file.readlines()

print(lines)
```

**Output:**

```
['Ravi\n', 'Priya\n', 'Arun']
```

Each item keeps its newline character (`\n`) when one is present. The last line in this example has no `\n` because the text written to the file does not end with a newline. Use `readlines()` when you want to work with the lines as a list.

### Reading Line by Line with a Loop

The simplest way to read a file line by line is to use a **`for` loop** on the file object.

```python
with open("students.txt", "w", encoding="utf-8") as file:
    file.write("Ravi\nPriya\nArun")

with open("students.txt", "r", encoding="utf-8") as file:
    for line in file:
        print(line.strip())
```

**Output:**

```
Ravi
Priya
Arun
```

Each round of the loop gives the next line of the file. `strip()` removes the newline and other surrounding whitespace before printing, preventing unwanted blank lines.

A `for` loop is **usually a good choice for large files**, because it processes one line at a time instead of loading the entire file into memory.

## Appending to a File

Mode `"w"` removes the existing content. To **add** content to the end of a file instead, use mode `"a"`.

```python
with open("students.txt", "w", encoding="utf-8") as file:
    file.write("Ravi\n")

with open("students.txt", "a", encoding="utf-8") as file:
    file.write("Priya")

with open("students.txt", "r", encoding="utf-8") as file:
    print(file.read())
```

**Output:**

```
Ravi
Priya
```

The first block opens the file in `"w"` mode and writes `Ravi`, replacing any previous content. The second block uses `"a"` mode, which adds `Priya` without removing `Ravi`.

## Points to Remember

- **Be careful with `"w"`.** Opening an existing file in this mode removes its previous content, even if you do not write anything new. Use `"a"` to keep the content and add to the end.
- **Use `\n` to start a new line** when writing. `write()` does not add one automatically.
- **Use the `with` statement** so the file is closed automatically.
- **Specify `encoding="utf-8"`** when opening text files.
- **Mode `"r"` raises a `FileNotFoundError`** if the file does not exist at the specified location.

## Further Reading

- 📎 **Official Python guide to reading and writing files** — https://docs.python.org/3/tutorial/inputoutput.html#reading-and-writing-files

The examples in this topic use only a file name. In the next topic, **File Paths and Errors**, you'll learn how to work with files in other folders and how to handle errors such as a missing file.
