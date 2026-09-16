# File Paths and Errors

`open("notes.txt")` works only when `notes.txt` is where the program is looking. Two things decide that: what a path means, and what happens when it is wrong.

## Relative and Absolute Paths

An **absolute path** states the full location from the root of the filesystem:

```
C:\Users\anita\project\notes.txt
/home/anita/project/notes.txt
```

A **relative path** is interpreted from the **current working directory** — the directory the program is running in:

```
notes.txt
data/marks.csv
```

`os.getcwd()` reports the current working directory:

```python
import os
print(os.getcwd())
```

**Output:**

```
C:\Users\anita\project
```

This is the commonest source of "but the file is right there". The working directory is where the program was **launched from**, not where the `.py` file lives. Running `python scripts/report.py` from the project root leaves the working directory as the project root, so a relative path resolves from there.

Prefer relative paths in code — they keep a project portable — and be clear about what they are relative to.

## Separators

Windows separates path components with `\`, and Linux and macOS with `/`. That is awkward in Python, because `\` starts an escape sequence in a string:

```python
path = "data\new\table.txt"
print(path)
```

**Output:**

```
data
ew	able.txt
```

`\n` became a newline and `\t` a tab. The string never contained the path that was typed.

Sometimes it fails outright rather than silently:

```python
path = "C:\Users\anita\notes.txt"
```

**Output:**

```
SyntaxError: (unicode error) 'unicodeescape' codec can't decode bytes in position 2-3: truncated \UXXXXXXXX escape
```

`\U` begins a Unicode escape expecting eight hex digits, and `sers\ani` is not that. Nearly every Windows programmer meets this error while typing a path into a string.

Three ways round it: double the backslashes (`"data\\new"`), use a raw string (`r"data\new"`), or use forward slashes, which Windows accepts too (`"data/new"`).

Better than all three is not to write separators at all.

## pathlib

`pathlib.Path` represents a path as an object, and joins components with `/`:

```python
from pathlib import Path

path = Path("data") / "reports" / "term1.csv"
print(path)
```

**Output:**

```
data\reports\term1.csv
```

The `/` operator is doing the joining — nothing to do with division. Python inserts whatever separator the operating system uses, so the same code produces `data/reports/term1.csv` on Linux. That is the point: the separator stops being your problem.

A `Path` knows its own parts:

```python
from pathlib import Path

path = Path("data") / "reports" / "term1.csv"
print("name:", path.name)
print("stem:", path.stem)
print("suffix:", path.suffix)
print("parent:", path.parent)
print("parts:", path.parts)
```

**Output:**

```
name: term1.csv
stem: term1
suffix: .csv
parent: data\reports
parts: ('data', 'reports', 'term1.csv')
```

| Attribute | Gives |
| --- | --- |
| `name` | the final component, with extension |
| `stem` | the final component, without extension |
| `suffix` | the extension, including the dot |
| `parent` | the containing directory |
| `parts` | every component as a tuple |

Changing an extension is one call rather than string surgery:

```python
from pathlib import Path
print(Path("report.txt").with_suffix(".md"))
```

**Output:**

```
report.md
```

A `Path` can be given to `open()` anywhere a string can.

## Asking About a Path

```python
from pathlib import Path

print(Path("notes.txt").exists())
print(Path("missing.txt").exists())
print(Path("notes.txt").is_file())
print(Path(".").is_dir())
print(Path("notes.txt").stat().st_size)
```

**Output:**

```
True
False
True
True
18
```

`stat().st_size` is the size in bytes.

## Reading, Writing and Listing

For a whole small file, `Path` skips `open()` entirely:

```python
from pathlib import Path

Path("greeting.txt").write_text("hello\n", encoding="utf-8")
print(Path("greeting.txt").read_text(encoding="utf-8"))
```

**Output:**

```
hello

```

These open, do the work, and close, in one call. Use them for small files read or written in one go, and `with open(...)` when the file is large or processed line by line.

`mkdir()` creates a directory. `parents=True` creates missing parents, and `exist_ok=True` makes an existing directory acceptable rather than an error:

```python
from pathlib import Path
Path("data/reports").mkdir(parents=True, exist_ok=True)
```

`iterdir()` lists a directory, and `glob()` matches a pattern:

```python
from pathlib import Path

print(sorted(item.name for item in Path("data/reports").iterdir()))
print(sorted(item.name for item in Path("data/reports").glob("*.csv")))
print(sorted(item.name for item in Path("data").glob("**/*.csv")))
```

**Output:**

```
['term1.csv', 'term2.csv']
['term1.csv', 'term2.csv']
['term1.csv', 'term2.csv']
```

`*` matches within one directory level; `**` searches subdirectories too. Processing every CSV in a folder is one loop:

```python
from pathlib import Path

for path in sorted(Path("data/reports").glob("*.csv")):
    print(path.stem, len(path.read_text(encoding="utf-8").splitlines()))
```

**Output:**

```
term1 2
term2 2
```

## The File Errors

| Error | Cause |
| --- | --- |
| `FileNotFoundError` | reading a file that is not there, or writing into a directory that is not there |
| `PermissionError` | the operating system refused access |
| `FileExistsError` | creating something that already exists |
| `IsADirectoryError` | treating a directory as a file |
| `UnicodeDecodeError` | reading text with the wrong encoding |

The commonest by a distance:

```python
with open("missing.txt") as handle:
    print(handle.read())
```

**Output:**

```
FileNotFoundError: [Errno 2] No such file or directory: 'missing.txt'
```

`FileNotFoundError` also appears on *writing*, when the containing directory does not exist:

```python
with open("nowhere/deep/file.txt", "w") as handle:
    handle.write("x")
```

**Output:**

```
FileNotFoundError: [Errno 2] No such file or directory: 'nowhere/deep/file.txt'
```

`"w"` creates a missing **file**. It does not create missing **directories** — `mkdir(parents=True)` does that.

The exact wording of these messages varies between operating systems. The error type does not, and the type is what code should react to.

## Handling Them

Wrap the file operation in `try` / `except` and catch the specific error:

```python
def load_config(path):
    try:
        with open(path, encoding="utf-8") as handle:
            return handle.read()
    except FileNotFoundError:
        print(f"No file at {path}; using defaults")
        return ""

print(repr(load_config("missing.json")))
```

**Output:**

```
No file at missing.json; using defaults
''
```

The program continued with a sensible fallback instead of stopping.

Catch the specific error, not everything. A bare `except:` would swallow a `PermissionError` and a typo in the function alike, and report both as a missing file.

## Checking Against Trying

Two approaches look equivalent:

```python
from pathlib import Path

if Path("data.txt").exists():
    with open("data.txt", encoding="utf-8") as handle:
        content = handle.read()
else:
    content = ""
```

```python
try:
    with open("data.txt", encoding="utf-8") as handle:
        content = handle.read()
except FileNotFoundError:
    content = ""
```

The second is preferred in Python, for two reasons.

**The first has a gap.** The file can be deleted between the `exists()` check and the `open()`. The window is tiny and real, and the program crashes when it is hit.

**`exists()` does not mean readable.** A file can exist and still raise `PermissionError`. The check answers a different question from the one that matters.

The principle has a name — *easier to ask forgiveness than permission* — and it is why Python code tends to attempt an operation and handle the failure, rather than testing first. Use `exists()` when the answer is genuinely the point, such as deciding whether to create a file; use `try` when you are about to open it anyway.

## Further Reading

- **Official reference for `pathlib`** — https://docs.python.org/3/library/pathlib.html
- **Working with file paths in Python** — https://realpython.com/python-pathlib/

Paths are relative to where the program was launched, `pathlib` builds them without separator trouble, and the file errors are specific enough to catch individually. Next, a file format with structure.
